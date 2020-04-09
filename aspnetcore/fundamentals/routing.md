---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 瞭解ASP.NET核心路由如何負責匹配 HTTP 請求和調度到可執行終結點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/1/2020
uid: fundamentals/routing
ms.openlocfilehash: 5742ac6879ce46e01247ddd2f8bfe3e3b8a2a02a
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80751156"
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="02730-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="02730-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="02730-104">由[里安·諾瓦克](https://github.com/rynowak),[柯克·拉金](https://twitter.com/serpent5)和[里克·安德森](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="02730-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="02730-105">路由負責匹配傳入的 HTTP 請求並將這些請求發送到應用的可執行終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-105">Routing is responsible for matching incoming HTTP requests and dispatching those requests to the app's executable endpoints.</span></span> <span data-ttu-id="02730-106">[終結點](#endpoint)是應用可執行的請求處理代碼的單位。</span><span class="sxs-lookup"><span data-stu-id="02730-106">[Endpoints](#endpoint) are the app's units of executable request-handling code.</span></span> <span data-ttu-id="02730-107">終結點在應用中定義,並在應用啟動時配置。</span><span class="sxs-lookup"><span data-stu-id="02730-107">Endpoints are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="02730-108">終結點匹配過程可以從請求的 URL 中提取值,並提供這些值以進行請求處理。</span><span class="sxs-lookup"><span data-stu-id="02730-108">The endpoint matching process can extract values from the request's URL and provide those values for request processing.</span></span> <span data-ttu-id="02730-109">使用來自應用的終結點資訊,路由還能夠生成映射到終結點的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-109">Using endpoint information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="02730-110">套用可以使用以下功能設定路由:</span><span class="sxs-lookup"><span data-stu-id="02730-110">Apps can configure routing using:</span></span>

- <span data-ttu-id="02730-111">Controllers</span><span class="sxs-lookup"><span data-stu-id="02730-111">Controllers</span></span>
- <span data-ttu-id="02730-112">Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="02730-112">Razor Pages</span></span>
- <span data-ttu-id="02730-113">SignalR</span><span class="sxs-lookup"><span data-stu-id="02730-113">SignalR</span></span>
- <span data-ttu-id="02730-114">gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="02730-114">gRPC Services</span></span>
- <span data-ttu-id="02730-115">開啟終結點[的中間件](xref:fundamentals/middleware/index),如[執行狀況檢查](xref:host-and-deploy/health-checks)。</span><span class="sxs-lookup"><span data-stu-id="02730-115">Endpoint-enabled [middleware](xref:fundamentals/middleware/index) such as [Health Checks](xref:host-and-deploy/health-checks).</span></span>
- <span data-ttu-id="02730-116">在路由中註冊的委託和 lambdas。</span><span class="sxs-lookup"><span data-stu-id="02730-116">Delegates and lambdas registered with routing.</span></span>

<span data-ttu-id="02730-117">本文檔介紹ASP.NET核心路由的低級詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="02730-117">This document covers low-level details of ASP.NET Core routing.</span></span> <span data-ttu-id="02730-118">有關設定路由的資訊:</span><span class="sxs-lookup"><span data-stu-id="02730-118">For information on configuring routing:</span></span>

* <span data-ttu-id="02730-119">有關控制器,請參閱<xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="02730-119">For controllers, see <xref:mvc/controllers/routing>.</span></span>
* <span data-ttu-id="02730-120">有關剃刀頁約定,<xref:razor-pages/razor-pages-conventions>請參閱 。</span><span class="sxs-lookup"><span data-stu-id="02730-120">For Razor Pages conventions, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="02730-121">本文檔中描述的端點路由系統適用於 ASP.NET 酷 3.0 及更高版本。</span><span class="sxs-lookup"><span data-stu-id="02730-121">The endpoint routing system described in this document applies to ASP.NET Core 3.0 and later.</span></span> <span data-ttu-id="02730-122">有關基於<xref:Microsoft.AspNetCore.Routing.IRouter>的上一個路由系統的資訊,請使用以下方法之一選擇ASP.NET Core 2.1 版本:</span><span class="sxs-lookup"><span data-stu-id="02730-122">For information on the previous routing system based on <xref:Microsoft.AspNetCore.Routing.IRouter>, select the ASP.NET Core 2.1 version using one of the following approaches:</span></span>

* <span data-ttu-id="02730-123">早期版本的版本選擇器。</span><span class="sxs-lookup"><span data-stu-id="02730-123">The version selector for a previous version.</span></span>
* <span data-ttu-id="02730-124">選擇[ASP.NET 核心 2.1 路由](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="02730-124">Select [ASP.NET Core 2.1 routing](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="02730-125">[檢視或下載範例代碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02730-125">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="02730-126">此文件的下載示例由特定`Startup`類啟用。</span><span class="sxs-lookup"><span data-stu-id="02730-126">The download samples for this document are enabled by a specific `Startup` class.</span></span> <span data-ttu-id="02730-127">要運行特定範例,請修改*Program.cs*以調用`Startup`所需的 類。</span><span class="sxs-lookup"><span data-stu-id="02730-127">To run a specific sample, modify *Program.cs* to call the desired `Startup` class.</span></span>

## <a name="routing-basics"></a><span data-ttu-id="02730-128">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="02730-128">Routing basics</span></span>

<span data-ttu-id="02730-129">所有ASP.NET核心範本都包括在生成的代碼中路由。</span><span class="sxs-lookup"><span data-stu-id="02730-129">All ASP.NET Core templates include routing in the generated code.</span></span> <span data-ttu-id="02730-130">路由在中[的中間件](xref:fundamentals/middleware/index)管道中`Startup.Configure`註冊。</span><span class="sxs-lookup"><span data-stu-id="02730-130">Routing is registered in the [middleware](xref:fundamentals/middleware/index) pipeline in `Startup.Configure`.</span></span>

<span data-ttu-id="02730-131">以下代碼顯示了路由的基本範例:</span><span class="sxs-lookup"><span data-stu-id="02730-131">The following code shows a basic example of routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

<span data-ttu-id="02730-132">路由使用一對中間件,由和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*><xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>註冊。</span><span class="sxs-lookup"><span data-stu-id="02730-132">Routing uses a pair of middleware, registered by <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>:</span></span>

* <span data-ttu-id="02730-133">`UseRouting`將路由匹配添加到中間件管道。</span><span class="sxs-lookup"><span data-stu-id="02730-133">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="02730-134">此中間件檢視應用中定義的終結點集,並根據請求選擇[最佳符合項](#urlm)。</span><span class="sxs-lookup"><span data-stu-id="02730-134">This middleware looks at the set of endpoints defined in the app, and selects the [best match](#urlm) based on the request.</span></span>
* <span data-ttu-id="02730-135">`UseEndpoints`將終結點執行添加到中間件管道。</span><span class="sxs-lookup"><span data-stu-id="02730-135">`UseEndpoints` adds endpoint execution to the middleware pipeline.</span></span> <span data-ttu-id="02730-136">它運行與所選終結點關聯的委託。</span><span class="sxs-lookup"><span data-stu-id="02730-136">It runs the delegate associated with the selected endpoint.</span></span>

<span data-ttu-id="02730-137">前面的範例包括使用[MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*)方法*對代碼*終結點的單個路由:</span><span class="sxs-lookup"><span data-stu-id="02730-137">The preceding example includes a single *route to code* endpoint using the [MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*) method:</span></span>

* <span data-ttu-id="02730-138">當`GET`HTTP 要求送出到`/`根網址時 :</span><span class="sxs-lookup"><span data-stu-id="02730-138">When an HTTP `GET` request is sent to the root URL `/`:</span></span>
  * <span data-ttu-id="02730-139">顯示的請求委託執行。</span><span class="sxs-lookup"><span data-stu-id="02730-139">The request delegate shown executes.</span></span>
  * <span data-ttu-id="02730-140">`Hello World!`寫入 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="02730-140">`Hello World!` is written to the HTTP response.</span></span> <span data-ttu-id="02730-141">預設情況下,根網`/`址`https://localhost:5001/`為 。</span><span class="sxs-lookup"><span data-stu-id="02730-141">By default, the root URL `/` is `https://localhost:5001/`.</span></span>
* <span data-ttu-id="02730-142">如果請求方法不是`GET`或根`/`URL 不是 ,則沒有路由匹配,並且返回 HTTP 404。</span><span class="sxs-lookup"><span data-stu-id="02730-142">If the request method is not `GET` or the root URL is not `/`, no route matches and an HTTP 404 is returned.</span></span>

### <a name="endpoint"></a><span data-ttu-id="02730-143">端點</span><span class="sxs-lookup"><span data-stu-id="02730-143">Endpoint</span></span>

<a name="endpoint"></a>

<span data-ttu-id="02730-144">這個方法`MapGet`定義**的終結點**。</span><span class="sxs-lookup"><span data-stu-id="02730-144">The `MapGet` method is used to define an **endpoint**.</span></span> <span data-ttu-id="02730-145">終結點可以是:</span><span class="sxs-lookup"><span data-stu-id="02730-145">An endpoint is something that can be:</span></span>

* <span data-ttu-id="02730-146">通過匹配 URL 和 HTTP 方法進行選擇。</span><span class="sxs-lookup"><span data-stu-id="02730-146">Selected, by matching the URL and HTTP method.</span></span>
* <span data-ttu-id="02730-147">通過運行委託執行。</span><span class="sxs-lookup"><span data-stu-id="02730-147">Executed, by running the delegate.</span></span>

<span data-ttu-id="02730-148">可在中匹配和執行的應用的終結點在`UseEndpoints`中 配置。</span><span class="sxs-lookup"><span data-stu-id="02730-148">Endpoints that can be matched and executed by the app are configured in `UseEndpoints`.</span></span> <span data-ttu-id="02730-149">例如,<xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*><xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>和[類似的方法](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions)將請求委託連接到路由系統。</span><span class="sxs-lookup"><span data-stu-id="02730-149">For example, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>, and [similar methods](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions) connect request delegates to the routing system.</span></span>
<span data-ttu-id="02730-150">其他方法可用於將ASP.NET核心框架功能連接到路由系統:</span><span class="sxs-lookup"><span data-stu-id="02730-150">Additional methods can be used to connect ASP.NET Core framework features to the routing system:</span></span>
- [<span data-ttu-id="02730-151">剃刀頁面的地圖剃刀頁面</span><span class="sxs-lookup"><span data-stu-id="02730-151">MapRazorPages for Razor Pages</span></span>](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)
- [<span data-ttu-id="02730-152">控制器的地圖控制器</span><span class="sxs-lookup"><span data-stu-id="02730-152">MapControllers for controllers</span></span>](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- [<span data-ttu-id="02730-153">用於訊\<號 R 的 MapHub THub></span><span class="sxs-lookup"><span data-stu-id="02730-153">MapHub\<THub> for SignalR</span></span>](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*) 
- [<span data-ttu-id="02730-154">用於 gRPC 的 MapGrpc 服務\<服務></span><span class="sxs-lookup"><span data-stu-id="02730-154">MapGrpcService\<TService> for gRPC</span></span>](xref:grpc/aspnetcore)

<span data-ttu-id="02730-155">下面的範例顯示了具有更複雜的路由範本的路由:</span><span class="sxs-lookup"><span data-stu-id="02730-155">The following example shows routing with a more sophisticated route template:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

<span data-ttu-id="02730-156">這個字串`/hello/{name:alpha}`是**一個路由樣本**。</span><span class="sxs-lookup"><span data-stu-id="02730-156">The string `/hello/{name:alpha}` is a **route template**.</span></span> <span data-ttu-id="02730-157">它用於配置終結點的匹配方式。</span><span class="sxs-lookup"><span data-stu-id="02730-157">It is used to configure how the endpoint is matched.</span></span> <span data-ttu-id="02730-158">在這種情況下,範本符合:</span><span class="sxs-lookup"><span data-stu-id="02730-158">In this case, the template matches:</span></span>

* <span data-ttu-id="02730-159">網址`/hello/Ryan`</span><span class="sxs-lookup"><span data-stu-id="02730-159">A URL like `/hello/Ryan`</span></span>
* <span data-ttu-id="02730-160">以`/hello/`字母字元序列開頭的任何 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-160">Any URL path that begins with `/hello/` followed by a sequence of alphabetic characters.</span></span>  <span data-ttu-id="02730-161">`:alpha`應用僅匹配字母字元的路由約束。</span><span class="sxs-lookup"><span data-stu-id="02730-161">`:alpha` applies a route constraint that matches only alphabetic characters.</span></span> <span data-ttu-id="02730-162">[此文件稍後會介紹路由約束](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-162">[Route constraints](#route-constraint-reference) are explained later in this document.</span></span>

<span data-ttu-id="02730-163">網址 路徑的第二`{name:alpha}`段:</span><span class="sxs-lookup"><span data-stu-id="02730-163">The second segment of the URL path, `{name:alpha}`:</span></span>

* <span data-ttu-id="02730-164">繫結到參數`name`。</span><span class="sxs-lookup"><span data-stu-id="02730-164">Is bound to the `name` parameter.</span></span>
* <span data-ttu-id="02730-165">捕獲並存儲在[HTTPRequest.Routevalue](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)中。</span><span class="sxs-lookup"><span data-stu-id="02730-165">Is captured and stored in [HttpRequest.RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*).</span></span>

<span data-ttu-id="02730-166">本文件中描述的端點路由系統ASP.NET Core 3.0 起是新的。</span><span class="sxs-lookup"><span data-stu-id="02730-166">The endpoint routing system described in this document is new as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="02730-167">但是,ASP.NET Core 的所有版本都支援同一集路由範本功能和路由約束。</span><span class="sxs-lookup"><span data-stu-id="02730-167">However, all versions of ASP.NET Core support the same set of route template features and route constraints.</span></span>

<span data-ttu-id="02730-168">下面的範例顯示了具有[執行狀況檢查和](xref:host-and-deploy/health-checks)授權的路由:</span><span class="sxs-lookup"><span data-stu-id="02730-168">The following example shows routing with [health checks](xref:host-and-deploy/health-checks) and authorization:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="02730-169">前面的範例展示如何:</span><span class="sxs-lookup"><span data-stu-id="02730-169">The preceding example demonstrates how:</span></span>

* <span data-ttu-id="02730-170">授權中間件可與路由一起使用。</span><span class="sxs-lookup"><span data-stu-id="02730-170">The authorization middleware can be used with routing.</span></span>
* <span data-ttu-id="02730-171">終結點可用於配置授權行為。</span><span class="sxs-lookup"><span data-stu-id="02730-171">Endpoints can be used to configure authorization behavior.</span></span>

<span data-ttu-id="02730-172">調用<xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*>添加運行狀況檢查終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-172">The <xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*> call adds a health check endpoint.</span></span> <span data-ttu-id="02730-173"><xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>連結到此調用會將授權策略附加到終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-173">Chaining <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> on to this call attaches an authorization policy to the endpoint.</span></span>

<span data-ttu-id="02730-174">呼叫<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>並<xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*>添加身份驗證和授權中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-174">Calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> adds the authentication and authorization middleware.</span></span> <span data-ttu-id="02730-175">這些中間件放置在與<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>`UseEndpoints`之間,以便它們可以:</span><span class="sxs-lookup"><span data-stu-id="02730-175">These middleware are placed between <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and `UseEndpoints` so that they can:</span></span>

* <span data-ttu-id="02730-176">查看由`UseRouting`選擇哪個終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-176">See which endpoint was selected by `UseRouting`.</span></span>
* <span data-ttu-id="02730-177">在調度到終結點之前<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>應用授權策略。</span><span class="sxs-lookup"><span data-stu-id="02730-177">Apply an authorization policy before <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> dispatches to the endpoint.</span></span>

<a name="metadata"></a>

### <a name="endpoint-metadata"></a><span data-ttu-id="02730-178">端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="02730-178">Endpoint metadata</span></span>

<span data-ttu-id="02730-179">在前面的示例中,有兩個終結點,但只有運行狀況檢查終結點附加了授權策略。</span><span class="sxs-lookup"><span data-stu-id="02730-179">In the preceding example, there are two endpoints, but only the health check endpoint has an authorization policy attached.</span></span> <span data-ttu-id="02730-180">如果請求與運行狀況檢查終結點匹配,`/healthz`則執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="02730-180">If the request matches the health check endpoint, `/healthz`, an authorization check is performed.</span></span> <span data-ttu-id="02730-181">這說明終結點可以附加額外的數據。</span><span class="sxs-lookup"><span data-stu-id="02730-181">This demonstrates that endpoints can have extra data attached to them.</span></span> <span data-ttu-id="02730-182">此額外資料稱為終結點**中繼資料**:</span><span class="sxs-lookup"><span data-stu-id="02730-182">This extra data is called endpoint **metadata**:</span></span>

* <span data-ttu-id="02730-183">元數據可以通過路由感知中間件進行處理。</span><span class="sxs-lookup"><span data-stu-id="02730-183">The metadata can be processed by routing-aware middleware.</span></span>
* <span data-ttu-id="02730-184">元數據可以是任何 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="02730-184">The metadata can be of any .NET type.</span></span>

## <a name="routing-concepts"></a><span data-ttu-id="02730-185">路由概念</span><span class="sxs-lookup"><span data-stu-id="02730-185">Routing concepts</span></span>

<span data-ttu-id="02730-186">路由系統通過添加強大的**端點**概念構建在中間件管道之上。</span><span class="sxs-lookup"><span data-stu-id="02730-186">The routing system builds on top of the middleware pipeline by adding the powerful **endpoint** concept.</span></span> <span data-ttu-id="02730-187">端點表示應用功能在路由、授權和任意數量的ASP.NET Core 系統方面彼此不同的單位。</span><span class="sxs-lookup"><span data-stu-id="02730-187">Endpoints represent units of the app's functionality that are distinct from each other in terms of routing, authorization, and any number of ASP.NET Core's systems.</span></span>

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a><span data-ttu-id="02730-188">ASP.NET核心終結點定義</span><span class="sxs-lookup"><span data-stu-id="02730-188">ASP.NET Core endpoint definition</span></span>

<span data-ttu-id="02730-189">ASP.NET核心終結點是:</span><span class="sxs-lookup"><span data-stu-id="02730-189">An ASP.NET Core endpoint is:</span></span>

* <span data-ttu-id="02730-190">執行: 具有<xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="02730-190">Executable: Has a <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>.</span></span>
* <span data-ttu-id="02730-191">可擴展:具有[元數據](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*)集合。</span><span class="sxs-lookup"><span data-stu-id="02730-191">Extensible: Has a [Metadata](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*) collection.</span></span>
* <span data-ttu-id="02730-192">選擇:選擇,有[路由資訊](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)。</span><span class="sxs-lookup"><span data-stu-id="02730-192">Selectable: Optionally, has [routing information](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*).</span></span>
* <span data-ttu-id="02730-193">枚舉:可以通過<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>從[DI](xref:fundamentals/dependency-injection)檢索 列出終結點的集合。</span><span class="sxs-lookup"><span data-stu-id="02730-193">Enumerable: The collection of endpoints can be listed by retrieving the <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> from [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="02730-194">以下代碼展示如何檢索和檢查與目前要求符合的終結點:</span><span class="sxs-lookup"><span data-stu-id="02730-194">The following code shows how to retrieve and inspect the endpoint matching the current request:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

<span data-ttu-id="02730-195">可以從 中檢索終結點(如果選`HttpContext`中 )。</span><span class="sxs-lookup"><span data-stu-id="02730-195">The endpoint, if selected, can be retrieved from the `HttpContext`.</span></span> <span data-ttu-id="02730-196">可以檢查其屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-196">Its properties can be inspected.</span></span> <span data-ttu-id="02730-197">終結點物件不可變,在創建後無法修改。</span><span class="sxs-lookup"><span data-stu-id="02730-197">Endpoint objects are immutable and cannot be modified after creation.</span></span> <span data-ttu-id="02730-198">最常見的終結點型態<xref:Microsoft.AspNetCore.Routing.RouteEndpoint>是 。</span><span class="sxs-lookup"><span data-stu-id="02730-198">The most common type of endpoint is a <xref:Microsoft.AspNetCore.Routing.RouteEndpoint>.</span></span> <span data-ttu-id="02730-199">`RouteEndpoint`包括允許路由系統選擇的資訊。</span><span class="sxs-lookup"><span data-stu-id="02730-199">`RouteEndpoint` includes information that allows it to be to selected by the routing system.</span></span>

<span data-ttu-id="02730-200">在前面的代碼中,[應用。使用](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*)設定一個內聯[中間元件](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="02730-200">In the preceding code, [app.Use](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*) configures an in-line [middleware](xref:fundamentals/middleware/index).</span></span>

<a name="mt"></a>

<span data-ttu-id="02730-201">以下代碼顯示,根據導管中的呼叫`app.Use`位置 ,可能沒有終結點:</span><span class="sxs-lookup"><span data-stu-id="02730-201">The following code shows that, depending on where `app.Use` is called in the pipeline, there may not be an endpoint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

<span data-ttu-id="02730-202">前面的範例添加`Console.WriteLine`顯示是否選擇了終結點的語句。</span><span class="sxs-lookup"><span data-stu-id="02730-202">This preceding sample adds `Console.WriteLine` statements that display whether or not an endpoint has been selected.</span></span> <span data-ttu-id="02730-203">為清楚起見,該示例為提供的`/`終結點分配顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-203">For clarity, the sample assigns a display name to the provided `/` endpoint.</span></span>

<span data-ttu-id="02730-204">使用`/`網址執行此程式:</span><span class="sxs-lookup"><span data-stu-id="02730-204">Running this code with a URL of `/` displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

<span data-ttu-id="02730-205">使用任何其他網址 執行此代碼會顯示:</span><span class="sxs-lookup"><span data-stu-id="02730-205">Running this code with any other URL displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

<span data-ttu-id="02730-206">此輸出表明:</span><span class="sxs-lookup"><span data-stu-id="02730-206">This output demonstrates that:</span></span>

* <span data-ttu-id="02730-207">在調用終結點之前`UseRouting`,終結點始終為空。</span><span class="sxs-lookup"><span data-stu-id="02730-207">The endpoint is always null before `UseRouting` is called.</span></span>
* <span data-ttu-id="02730-208">如果找到匹配項,則終結點在和`UseRouting`<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之間為非空。</span><span class="sxs-lookup"><span data-stu-id="02730-208">If a match is found, the endpoint is non-null between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="02730-209">找到`UseEndpoints`符合項目時,中間件是**終端**機 。</span><span class="sxs-lookup"><span data-stu-id="02730-209">The `UseEndpoints` middleware is **terminal** when a match is found.</span></span> <span data-ttu-id="02730-210">[此文件稍後會定義終端中間件](#tm)。</span><span class="sxs-lookup"><span data-stu-id="02730-210">[Terminal middleware](#tm) is defined later in this document.</span></span>
* <span data-ttu-id="02730-211">僅在未找到匹配`UseEndpoints`項時執行中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-211">The middleware after `UseEndpoints` execute only when no match is found.</span></span>

<span data-ttu-id="02730-212">中間`UseRouting`件使用[SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*)方法將終結點附加到當前上下文。</span><span class="sxs-lookup"><span data-stu-id="02730-212">The `UseRouting` middleware uses the [SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*) method to attach the endpoint to the current context.</span></span> <span data-ttu-id="02730-213">有可能用自定義邏輯替換`UseRouting`中間件,並且仍然獲得使用端點的好處。</span><span class="sxs-lookup"><span data-stu-id="02730-213">It's possible to replace the `UseRouting` middleware with custom logic and still get the benefits of using endpoints.</span></span> <span data-ttu-id="02730-214">端點是低級基元(如中間件),不耦合到路由實現。</span><span class="sxs-lookup"><span data-stu-id="02730-214">Endpoints are a low-level primitive like middleware, and aren't coupled to the routing implementation.</span></span> <span data-ttu-id="02730-215">大多數應用不需要用自訂邏輯取代`UseRouting`。</span><span class="sxs-lookup"><span data-stu-id="02730-215">Most apps don't need to replace `UseRouting` with custom logic.</span></span>

<span data-ttu-id="02730-216">中間`UseEndpoints`件設計為`UseRouting`與中間件同時使用。</span><span class="sxs-lookup"><span data-stu-id="02730-216">The `UseEndpoints` middleware is designed to be used in tandem with the `UseRouting` middleware.</span></span> <span data-ttu-id="02730-217">執行終結點的核心邏輯並不複雜。</span><span class="sxs-lookup"><span data-stu-id="02730-217">The core logic to execute an endpoint isn't complicated.</span></span> <span data-ttu-id="02730-218">用於<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>檢索終結點,然後調用<xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>其 屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-218">Use <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> to retrieve the endpoint, and then invoke its <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> property.</span></span>

<span data-ttu-id="02730-219">以下代碼演示了中間件如何影響路由或回應:</span><span class="sxs-lookup"><span data-stu-id="02730-219">The following code demonstrates how middleware can influence or react to routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="02730-220">前面的範例演示了兩個重要概念:</span><span class="sxs-lookup"><span data-stu-id="02730-220">The preceding example demonstrates two important concepts:</span></span>

* <span data-ttu-id="02730-221">中間件可以運行之前`UseRouting`修改路由操作的數據。</span><span class="sxs-lookup"><span data-stu-id="02730-221">Middleware can run before `UseRouting` to modify the data that routing operates upon.</span></span>
    * <span data-ttu-id="02730-222">通常,在路由之前出現的中間件會修改請求的某些屬性,例如<xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>,<xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*><xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>或 。</span><span class="sxs-lookup"><span data-stu-id="02730-222">Usually middleware that appears before routing modifies some property of the request, such as <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>, <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>, or <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>.</span></span>
* <span data-ttu-id="02730-223">中間件可以在執行終結點`UseRouting`<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之前 運行和處理路由結果。</span><span class="sxs-lookup"><span data-stu-id="02730-223">Middleware can run between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> to process the results of routing before the endpoint is executed.</span></span>
    * <span data-ttu-id="02730-224">置`UseRouting`層與`UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="02730-224">Middleware that runs between `UseRouting` and `UseEndpoints`:</span></span>
      * <span data-ttu-id="02730-225">通常檢查元數據以瞭解端點。</span><span class="sxs-lookup"><span data-stu-id="02730-225">Usually inspects metadata to understand the endpoints.</span></span>
      * <span data-ttu-id="02730-226">通常做出安全決策,如和`UseAuthorization``UseCors`執行的那樣。</span><span class="sxs-lookup"><span data-stu-id="02730-226">Often makes security decisions, as done by `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="02730-227">中間件和中繼資料的組合允許配置每個終結點的策略。</span><span class="sxs-lookup"><span data-stu-id="02730-227">The combination of middleware and metadata allows configuring policies per-endpoint.</span></span>

<span data-ttu-id="02730-228">前面的代碼顯示了支援每個終結點策略的自定義中間件的範例。</span><span class="sxs-lookup"><span data-stu-id="02730-228">The preceding code shows an example of a custom middleware that supports per-endpoint policies.</span></span> <span data-ttu-id="02730-229">中介件向主控台寫入存取敏感資料的*稽核紀錄*。</span><span class="sxs-lookup"><span data-stu-id="02730-229">The middleware writes an *audit log* of access to sensitive data to the console.</span></span> <span data-ttu-id="02730-230">中間件可以配置為*審核*具有元數據`AuditPolicyAttribute`的 終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-230">The middleware can be configured to *audit* an endpoint with the `AuditPolicyAttribute` metadata.</span></span> <span data-ttu-id="02730-231">此示例演示了*一種加入加入*模式,其中僅審核標記為敏感終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-231">This sample demonstrates an *opt-in* pattern where only endpoints that are marked as sensitive are audited.</span></span> <span data-ttu-id="02730-232">可以反向定義此邏輯,例如審核所有未標記為安全的事物。</span><span class="sxs-lookup"><span data-stu-id="02730-232">It's possible to define this logic in reverse, auditing everything that isn't marked as safe, for example.</span></span> <span data-ttu-id="02730-233">端點元數據系統是靈活的。</span><span class="sxs-lookup"><span data-stu-id="02730-233">The endpoint metadata system is flexible.</span></span> <span data-ttu-id="02730-234">此邏輯可以以適合用例的任何方式進行設計。</span><span class="sxs-lookup"><span data-stu-id="02730-234">This logic could be designed in whatever way suits the use case.</span></span>

<span data-ttu-id="02730-235">前面的範例代碼旨在演示終結點的基本概念。</span><span class="sxs-lookup"><span data-stu-id="02730-235">The preceding sample code is intended to demonstrate the basic concepts of endpoints.</span></span> <span data-ttu-id="02730-236">**該示例不適合生產。**</span><span class="sxs-lookup"><span data-stu-id="02730-236">**The sample is not intended for production use**.</span></span> <span data-ttu-id="02730-237">*稽核紀錄*中間件的更完整版本將:</span><span class="sxs-lookup"><span data-stu-id="02730-237">A more complete version of an *audit log* middleware would:</span></span>

* <span data-ttu-id="02730-238">登錄到文件或資料庫。</span><span class="sxs-lookup"><span data-stu-id="02730-238">Log to a file or database.</span></span>
* <span data-ttu-id="02730-239">包括使用者、IP 位址、敏感終結點的名稱等詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="02730-239">Include details such as the user, IP address, name of the sensitive endpoint, and more.</span></span>

<span data-ttu-id="02730-240">審核策略中繼`AuditPolicyAttribute`資料`Attribute`定義為 更易於使用基於類的框架(如控制器和 SignalR)。</span><span class="sxs-lookup"><span data-stu-id="02730-240">The audit policy metadata `AuditPolicyAttribute` is defined as an `Attribute` for easier use with class-based frameworks such as controllers and SignalR.</span></span> <span data-ttu-id="02730-241">使用*路由代碼*時 :</span><span class="sxs-lookup"><span data-stu-id="02730-241">When using *route to code*:</span></span>

* <span data-ttu-id="02730-242">元數據與生成器 API 一起連接。</span><span class="sxs-lookup"><span data-stu-id="02730-242">Metadata is attached with a builder API.</span></span>
* <span data-ttu-id="02730-243">創建終結點時,基於類的框架包括相應方法和類上的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-243">Class-based frameworks include all attributes on the corresponding method and class when creating endpoints.</span></span>

<span data-ttu-id="02730-244">元數據類型的最佳做法是將它們定義為介面或屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-244">The best practices for metadata types are to define them either as interfaces or attributes.</span></span> <span data-ttu-id="02730-245">介面和屬性允許代碼重用。</span><span class="sxs-lookup"><span data-stu-id="02730-245">Interfaces and attributes allow code reuse.</span></span> <span data-ttu-id="02730-246">元數據系統是靈活的,不會施加任何限制。</span><span class="sxs-lookup"><span data-stu-id="02730-246">The metadata system is flexible and doesn't impose any limitations.</span></span>

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a><span data-ttu-id="02730-247">比較終端中間件與路由</span><span class="sxs-lookup"><span data-stu-id="02730-247">Comparing a terminal middleware and routing</span></span>

<span data-ttu-id="02730-248">以下代碼範例使用中間件與使用路由進行對比:</span><span class="sxs-lookup"><span data-stu-id="02730-248">The following code sample contrasts using middleware with using routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="02730-249">中間元件的樣式`Approach 1:`是**終端中間件**。</span><span class="sxs-lookup"><span data-stu-id="02730-249">The style of middleware shown with `Approach 1:` is **terminal middleware**.</span></span> <span data-ttu-id="02730-250">它被稱為終端中間件,因為它執行匹配操作:</span><span class="sxs-lookup"><span data-stu-id="02730-250">It's called terminal middleware because it does a matching operation:</span></span>

* <span data-ttu-id="02730-251">前面的範例中的匹配操作用於`Path == "/"`中間件和`Path == "/Movie"`路由。</span><span class="sxs-lookup"><span data-stu-id="02730-251">The matching operation in the preceding sample is `Path == "/"` for the middleware and `Path == "/Movie"` for routing.</span></span>
* <span data-ttu-id="02730-252">當匹配成功時,它將執行一些功能並返回,而不是調用`next`中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-252">When a match is successful, it executes some functionality and returns, rather than invoking the `next` middleware.</span></span>

<span data-ttu-id="02730-253">它被稱為終端中間件,因為它終止搜索,執行一些功能,然後返回。</span><span class="sxs-lookup"><span data-stu-id="02730-253">It's called terminal middleware because it terminates the search, executes some functionality, and then returns.</span></span>

<span data-ttu-id="02730-254">比較終端中間件和路由:</span><span class="sxs-lookup"><span data-stu-id="02730-254">Comparing a terminal middleware and routing:</span></span>
* <span data-ttu-id="02730-255">這兩種方法都允許終止處理管道:</span><span class="sxs-lookup"><span data-stu-id="02730-255">Both approaches allow terminating the processing pipeline:</span></span>
    * <span data-ttu-id="02730-256">中間件通過返回而不是調用`next`終止管道。</span><span class="sxs-lookup"><span data-stu-id="02730-256">Middleware terminates the pipeline by returning rather than invoking `next`.</span></span>
    * <span data-ttu-id="02730-257">端點始終是終端。</span><span class="sxs-lookup"><span data-stu-id="02730-257">Endpoints are always terminal.</span></span>
* <span data-ttu-id="02730-258">終端中間件允許將中間件放置在導管中的任意位置:</span><span class="sxs-lookup"><span data-stu-id="02730-258">Terminal middleware allows positioning the middleware at an arbitrary place in the pipeline:</span></span>
    * <span data-ttu-id="02730-259">在位置執行<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>。</span><span class="sxs-lookup"><span data-stu-id="02730-259">Endpoints execute at the position of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="02730-260">終端中間件允許任意代碼確定中間件何時符合:</span><span class="sxs-lookup"><span data-stu-id="02730-260">Terminal middleware allows arbitrary code to determine when the middleware matches:</span></span>
    * <span data-ttu-id="02730-261">自定義路由匹配代碼可能非常詳細且難以正確編寫。</span><span class="sxs-lookup"><span data-stu-id="02730-261">Custom route matching code can be verbose and difficult to write correctly.</span></span>
    * <span data-ttu-id="02730-262">路由為典型應用提供了直接的解決方案。</span><span class="sxs-lookup"><span data-stu-id="02730-262">Routing provides straightforward solutions for typical apps.</span></span> <span data-ttu-id="02730-263">大多數應用不需要自定義路由匹配代碼。</span><span class="sxs-lookup"><span data-stu-id="02730-263">Most apps don't require custom route matching code.</span></span>
* <span data-ttu-id="02730-264">端點介面與中間件(如`UseAuthorization``UseCors`與 )</span><span class="sxs-lookup"><span data-stu-id="02730-264">Endpoints interface with middleware such as `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="02730-265">使用具有`UseAuthorization``UseCors`或需要手動與授權系統介面的終端中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-265">Using a terminal middleware with `UseAuthorization` or `UseCors` requires manual interfacing with the authorization system.</span></span>

<span data-ttu-id="02730-266">[終結點](#endpoint)定義兩者:</span><span class="sxs-lookup"><span data-stu-id="02730-266">An [endpoint](#endpoint) defines both:</span></span>

* <span data-ttu-id="02730-267">處理請求的委託。</span><span class="sxs-lookup"><span data-stu-id="02730-267">A delegate to process requests.</span></span>
* <span data-ttu-id="02730-268">任意元數據的集合。</span><span class="sxs-lookup"><span data-stu-id="02730-268">A collection of arbitrary metadata.</span></span> <span data-ttu-id="02730-269">元數據用於根據附加到每個終結點的策略和配置實現跨領域問題。</span><span class="sxs-lookup"><span data-stu-id="02730-269">The metadata is used to implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="02730-270">終端中間件可以是一個有效的工具,但可能需要:</span><span class="sxs-lookup"><span data-stu-id="02730-270">Terminal middleware can be an effective tool, but can require:</span></span>

* <span data-ttu-id="02730-271">大量的編碼和測試。</span><span class="sxs-lookup"><span data-stu-id="02730-271">A significant amount of coding and testing.</span></span>
* <span data-ttu-id="02730-272">與其他系統手動集成,實現所需的靈活性。</span><span class="sxs-lookup"><span data-stu-id="02730-272">Manual integration with other systems to achieve the desired level of flexibility.</span></span>

<span data-ttu-id="02730-273">在編寫終端中間件之前,請考慮與路由集成。</span><span class="sxs-lookup"><span data-stu-id="02730-273">Consider integrating with routing before writing a terminal middleware.</span></span>

<span data-ttu-id="02730-274">與[Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>整合或通常可轉換為路由感知終結點的現有終端中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-274">Existing terminal middleware that integrates with [Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) or <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> can usually be turned into a routing aware endpoint.</span></span> <span data-ttu-id="02730-275">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16)展示了路由器軟體的模式:</span><span class="sxs-lookup"><span data-stu-id="02730-275">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16) demonstrates the pattern for router-ware:</span></span>
* <span data-ttu-id="02730-276">在<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>上 寫入擴展方法。</span><span class="sxs-lookup"><span data-stu-id="02730-276">Write an extension method on <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span>
* <span data-ttu-id="02730-277">使用<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>創建嵌套中間件管道。</span><span class="sxs-lookup"><span data-stu-id="02730-277">Create a nested middleware pipeline using <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>.</span></span>
* <span data-ttu-id="02730-278">將中間件附加到新管道。</span><span class="sxs-lookup"><span data-stu-id="02730-278">Attach the middleware to the new pipeline.</span></span> <span data-ttu-id="02730-279">在此案例中為 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>。</span><span class="sxs-lookup"><span data-stu-id="02730-279">In this case, <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>.</span></span>
* <span data-ttu-id="02730-280"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>中介元件導管進入<xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="02730-280"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*> the middleware pipeline into a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="02730-281">調用`Map`並提供新的中間件管道。</span><span class="sxs-lookup"><span data-stu-id="02730-281">Call `Map` and provide the new middleware pipeline.</span></span>
* <span data-ttu-id="02730-282">返回擴展方法提供的`Map`生成器物件。</span><span class="sxs-lookup"><span data-stu-id="02730-282">Return the builder object provided by `Map` from the extension method.</span></span>

<span data-ttu-id="02730-283">以下代碼顯示了[MapHealth 檢查](xref:host-and-deploy/health-checks)的使用 :</span><span class="sxs-lookup"><span data-stu-id="02730-283">The following code shows use of [MapHealthChecks](xref:host-and-deploy/health-checks):</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

<span data-ttu-id="02730-284">前面的示例顯示了返回生成器物件的重要性。</span><span class="sxs-lookup"><span data-stu-id="02730-284">The preceding sample shows why returning the builder object is important.</span></span> <span data-ttu-id="02730-285">返回生成器物件允許應用開發人員配置策略,如終結點的授權。</span><span class="sxs-lookup"><span data-stu-id="02730-285">Returning the builder object allows the app developer to configure policies such as authorization for the endpoint.</span></span> <span data-ttu-id="02730-286">在此示例中,運行狀況檢查中間件與授權系統沒有直接集成。</span><span class="sxs-lookup"><span data-stu-id="02730-286">In this example, the health checks middleware has no direct integration with the authorization system.</span></span>

<span data-ttu-id="02730-287">創建元數據系統是為了回應擴展作者使用終端中間件遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="02730-287">The metadata system was created in response to the problems encountered by extensibility authors using terminal middleware.</span></span> <span data-ttu-id="02730-288">每個中間件實現自己與授權系統的集成都是有問題的。</span><span class="sxs-lookup"><span data-stu-id="02730-288">It's problematic for each middleware to implement its own integration with the authorization system.</span></span>

<a name="urlm"></a>

### <a name="url-matching"></a><span data-ttu-id="02730-289">URL 比對</span><span class="sxs-lookup"><span data-stu-id="02730-289">URL matching</span></span>

* <span data-ttu-id="02730-290">路由將傳入請求與[終結點](#endpoint)匹配的過程。</span><span class="sxs-lookup"><span data-stu-id="02730-290">Is the process by which routing matches an incoming request to an [endpoint](#endpoint).</span></span>
* <span data-ttu-id="02730-291">基於 URL 路徑和標頭中的數據。</span><span class="sxs-lookup"><span data-stu-id="02730-291">Is based on data in the URL path and headers.</span></span>
* <span data-ttu-id="02730-292">可以擴展以考慮請求中的任何數據。</span><span class="sxs-lookup"><span data-stu-id="02730-292">Can be extended to consider any data in the request.</span></span>

<span data-ttu-id="02730-293">當路由中間元件執行時,它會設定`Endpoint`與 路由值<xref:Microsoft.AspNetCore.Http.HttpContext>到 目前[要求上的請求功能](xref:fundamentals/request-features):</span><span class="sxs-lookup"><span data-stu-id="02730-293">When a routing middleware executes, it sets an `Endpoint` and route values to a [request feature](xref:fundamentals/request-features) on the <xref:Microsoft.AspNetCore.Http.HttpContext> from the current request:</span></span>

* <span data-ttu-id="02730-294">呼叫[HTTPContext.取得終結點](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>)獲取終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-294">Calling [HttpContext.GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>) gets the endpoint.</span></span>
* <span data-ttu-id="02730-295">`HttpRequest.RouteValues` 會取得路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="02730-295">`HttpRequest.RouteValues` gets the collection of route values.</span></span>

<span data-ttu-id="02730-296">路由中間件之後運行[的中間件](xref:fundamentals/middleware/index)可以檢查終結點並採取措施。</span><span class="sxs-lookup"><span data-stu-id="02730-296">[Middleware](xref:fundamentals/middleware/index) running after the routing middleware can inspect the endpoint and take action.</span></span> <span data-ttu-id="02730-297">例如,授權中間件可以詢問終結點的元數據集合以用於授權策略。</span><span class="sxs-lookup"><span data-stu-id="02730-297">For example, an authorization middleware can interrogate the endpoint's metadata collection for an authorization policy.</span></span> <span data-ttu-id="02730-298">執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。</span><span class="sxs-lookup"><span data-stu-id="02730-298">After all of the middleware in the request processing pipeline is executed, the selected endpoint's delegate is invoked.</span></span>

<span data-ttu-id="02730-299">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="02730-299">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="02730-300">由於中間件應用基於所選終結點的策略,因此請務必:</span><span class="sxs-lookup"><span data-stu-id="02730-300">Because the middleware applies policies based on the selected endpoint, it's important that:</span></span>

* <span data-ttu-id="02730-301">任何可能影響調度或應用安全策略的決定都會在路由系統內做出。</span><span class="sxs-lookup"><span data-stu-id="02730-301">Any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

> [!WARNING]
> <span data-ttu-id="02730-302">對於向後相容性,當執行控制器或 Razor Pages 終結點委託時[,RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData)的屬性將設置為基於迄今執行的請求處理的適當值。</span><span class="sxs-lookup"><span data-stu-id="02730-302">For backwards-compatibility, when a Controller or Razor Pages endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>
>
> <span data-ttu-id="02730-303">該`RouteContext`類型將在將來的版本中標記為過時:</span><span class="sxs-lookup"><span data-stu-id="02730-303">The `RouteContext` type will be marked obsolete in a future release:</span></span>
>
> * <span data-ttu-id="02730-304">移到`RouteData.Values``HttpRequest.RouteValues`。</span><span class="sxs-lookup"><span data-stu-id="02730-304">Migrate `RouteData.Values` to `HttpRequest.RouteValues`.</span></span>
> * <span data-ttu-id="02730-305">從`RouteData.DataTokens`終結點中繼資料移至檢索[IDataTokens 資料](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata)。</span><span class="sxs-lookup"><span data-stu-id="02730-305">Migrate `RouteData.DataTokens` to retrieve [IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) from the endpoint metadata.</span></span>

<span data-ttu-id="02730-306">URL 匹配在一組可配置的階段中運行。</span><span class="sxs-lookup"><span data-stu-id="02730-306">URL matching operates in a configurable set of phases.</span></span> <span data-ttu-id="02730-307">在每個階段,輸出是一組匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-307">In each phase, the output is a set of matches.</span></span> <span data-ttu-id="02730-308">下一階段可以進一步縮小比賽範圍。</span><span class="sxs-lookup"><span data-stu-id="02730-308">The set of matches can be narrowed down further by the next phase.</span></span> <span data-ttu-id="02730-309">路由實現不保證匹配終結點的處理順序。</span><span class="sxs-lookup"><span data-stu-id="02730-309">The routing implementation does not guarantee a processing order for matching endpoints.</span></span> <span data-ttu-id="02730-310">**所有**可能的匹配項都立即處理。</span><span class="sxs-lookup"><span data-stu-id="02730-310">**All** possible matches are processed at once.</span></span> <span data-ttu-id="02730-311">URL 匹配階段按以下順序發生。</span><span class="sxs-lookup"><span data-stu-id="02730-311">The URL matching phases occur in the following order.</span></span> <span data-ttu-id="02730-312">ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="02730-312">ASP.NET Core:</span></span>

1. <span data-ttu-id="02730-313">根據終結點集及其路由範本處理 URL 路徑,收集**所有**匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-313">Processes the URL path against the set of endpoints and their route templates, collecting **all** of the matches.</span></span>
1. <span data-ttu-id="02730-314">獲取上述清單並刪除應用路由約束失敗匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-314">Takes the preceding list and removes matches that fail with route constraints applied.</span></span>
1. <span data-ttu-id="02730-315">獲取上述清單並刪除使[MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy)實例集失敗的匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-315">Takes the preceding list and removes matches that fail the set of [MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy) instances.</span></span>
1. <span data-ttu-id="02730-316">使用[終結點選擇器](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector)從上述清單中做出最終決定。</span><span class="sxs-lookup"><span data-stu-id="02730-316">Uses the [EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) to make a final decision from the preceding list.</span></span>

<span data-ttu-id="02730-317">終結點列表的優先權根據:</span><span class="sxs-lookup"><span data-stu-id="02730-317">The list of endpoints is prioritized according to:</span></span>

* <span data-ttu-id="02730-318">[路由終結點。順序](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span><span class="sxs-lookup"><span data-stu-id="02730-318">The [RouteEndpoint.Order](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span></span>
* <span data-ttu-id="02730-319">[路由樣本優先順序](#rtp)</span><span class="sxs-lookup"><span data-stu-id="02730-319">The [route template precedence](#rtp)</span></span>

<span data-ttu-id="02730-320">在每個階段處理所有符合的終結點,直到達到<xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector>。</span><span class="sxs-lookup"><span data-stu-id="02730-320">All matching endpoints are processed in each phase until the <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> is reached.</span></span> <span data-ttu-id="02730-321">這是`EndpointSelector`最後階段。</span><span class="sxs-lookup"><span data-stu-id="02730-321">The `EndpointSelector` is the final phase.</span></span> <span data-ttu-id="02730-322">它將從匹配中選擇最高優先順序終結點作為最佳匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-322">It chooses the highest priority endpoint from the matches as the best match.</span></span> <span data-ttu-id="02730-323">如果存在與最佳匹配項具有相同優先順序的其他匹配項,則引發不明確的匹配異常。</span><span class="sxs-lookup"><span data-stu-id="02730-323">If there are other matches with the same priority as the best match, an ambiguous match exception is thrown.</span></span>

<span data-ttu-id="02730-324">路由優先順序是根據給定更高優先順序**的更具體的**路由範本計算的。</span><span class="sxs-lookup"><span data-stu-id="02730-324">The route precedence is computed based on a **more specific** route template being given a higher priority.</span></span> <span data-ttu-id="02730-325">例如,請考慮樣本`/hello`與`/{message}`:</span><span class="sxs-lookup"><span data-stu-id="02730-325">For example, consider the templates `/hello` and `/{message}`:</span></span>

* <span data-ttu-id="02730-326">兩者都與`/hello`URL 路徑匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-326">Both match the URL path `/hello`.</span></span>
* <span data-ttu-id="02730-327">`/hello`更具體,因此優先順序更高。</span><span class="sxs-lookup"><span data-stu-id="02730-327">`/hello`  is more specific and therefore higher priority.</span></span>

<span data-ttu-id="02730-328">通常,路由優先順序非常適合實際使用的 URL 方案類型。</span><span class="sxs-lookup"><span data-stu-id="02730-328">In general, route precedence does a good job of choosing the best match for the kinds of URL schemes used in practice.</span></span> <span data-ttu-id="02730-329">僅在<xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order>必要時使用,以避免模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="02730-329">Use <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order> only when necessary to avoid an ambiguity.</span></span>

<span data-ttu-id="02730-330">由於路由提供的擴充性類型,路由系統無法提前計算不明確的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-330">Due to the kinds of extensibility provided by routing, it isn't possible for the routing system to compute ahead of time the ambiguous routes.</span></span> <span data-ttu-id="02730-331">考慮路由範本`/{message:alpha}``/{message:int}`與的範例:</span><span class="sxs-lookup"><span data-stu-id="02730-331">Consider an example such as the route templates `/{message:alpha}` and `/{message:int}`:</span></span>

* <span data-ttu-id="02730-332">約束`alpha`僅匹配字母字元。</span><span class="sxs-lookup"><span data-stu-id="02730-332">The `alpha` constraint matches only alphabetic characters.</span></span>
* <span data-ttu-id="02730-333">約束`int`僅與數位匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-333">The `int` constraint matches only numbers.</span></span>
* <span data-ttu-id="02730-334">這些範本具有相同的路由優先順序,但沒有它們匹配的單個 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-334">These templates have the same route precedence, but there's no single URL they both match.</span></span>
* <span data-ttu-id="02730-335">如果路由系統在啟動時報告存在歧義錯誤,它將阻止此有效用例。</span><span class="sxs-lookup"><span data-stu-id="02730-335">If the routing system reported an ambiguity error at startup, it would block this valid use case.</span></span>

> [!WARNING]
>
> <span data-ttu-id="02730-336">內部<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>操作的順序不會影響路由的行為,但有一個例外。</span><span class="sxs-lookup"><span data-stu-id="02730-336">The order of operations inside <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> doesn't influence the behavior of routing, with one exception.</span></span> <span data-ttu-id="02730-337"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>並根據<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>所調用的順序自動為其終結點分配訂單值。</span><span class="sxs-lookup"><span data-stu-id="02730-337"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="02730-338">這將模擬控制器的長期行為,而路由系統沒有提供與舊路由實現相同的保證。</span><span class="sxs-lookup"><span data-stu-id="02730-338">This simulates long-time behavior of controllers without the routing system providing the same guarantees as older routing implementations.</span></span>
>
> <span data-ttu-id="02730-339">在路由的舊實現中,可以實現依賴於路由處理順序的路由擴展性。</span><span class="sxs-lookup"><span data-stu-id="02730-339">In the legacy implementation of routing, it's possible to implement routing extensibility that has a dependency on the order in which routes are processed.</span></span> <span data-ttu-id="02730-340">ASP.NET Core 3.0 及更高版本中的終結點路由:</span><span class="sxs-lookup"><span data-stu-id="02730-340">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>
> 
> * <span data-ttu-id="02730-341">沒有路線的概念。</span><span class="sxs-lookup"><span data-stu-id="02730-341">Doesn't have a concept of routes.</span></span>
> * <span data-ttu-id="02730-342">不提供訂購保證。</span><span class="sxs-lookup"><span data-stu-id="02730-342">Doesn't provide ordering guarantees.</span></span> <span data-ttu-id="02730-343">一次處理所有終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-343">All endpoints are processed at once.</span></span>
>
> <span data-ttu-id="02730-344">如果這意味著您被卡住使用舊路由系統,[則開啟 GitHub 問題以獲得説明](https://github.com/dotnet/aspnetcore/issues)。</span><span class="sxs-lookup"><span data-stu-id="02730-344">If this means you're stuck using the legacy routing system, [open a GitHub issue for assistance](https://github.com/dotnet/aspnetcore/issues).</span></span>

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a><span data-ttu-id="02730-345">路由樣本優先權和終結點選擇順序</span><span class="sxs-lookup"><span data-stu-id="02730-345">Route template precedence and endpoint selection order</span></span>

<span data-ttu-id="02730-346">[路由範本優先順序](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16)是一個系統,它根據每個工藝路線範本的特定於值分配了值。</span><span class="sxs-lookup"><span data-stu-id="02730-346">[Route template precedence](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16) is a system that assigns each route template a value based on how specific it is.</span></span> <span data-ttu-id="02730-347">路由樣本優先順序:</span><span class="sxs-lookup"><span data-stu-id="02730-347">Route template precedence:</span></span>

* <span data-ttu-id="02730-348">避免在常見情況下調整終結點的順序。</span><span class="sxs-lookup"><span data-stu-id="02730-348">Avoids the need to adjust the order of endpoints in common cases.</span></span>
* <span data-ttu-id="02730-349">嘗試匹配路由行為的常識性期望。</span><span class="sxs-lookup"><span data-stu-id="02730-349">Attempts to match the common-sense expectations of routing behavior.</span></span>

<span data-ttu-id="02730-350">例如,請考慮範本`/Products/List`和`/Products/{id}`。</span><span class="sxs-lookup"><span data-stu-id="02730-350">For example, consider templates `/Products/List` and `/Products/{id}`.</span></span> <span data-ttu-id="02730-351">假設`/Products/List`這比`/Products/{id}``/Products/List`URL 路徑 更適合。</span><span class="sxs-lookup"><span data-stu-id="02730-351">It would be reasonable to assume that `/Products/List` is a better match than `/Products/{id}` for the URL path `/Products/List`.</span></span> <span data-ttu-id="02730-352">之所以有效,是因為文本段`/List`被認為比參數`/{id}`段 具有更好的優先順序。</span><span class="sxs-lookup"><span data-stu-id="02730-352">The works because the literal segment `/List` is considered to have better precedence than the parameter segment `/{id}`.</span></span>

<span data-ttu-id="02730-353">優先順序的工作原理與路由範本的定義方式相結合的詳細資訊:</span><span class="sxs-lookup"><span data-stu-id="02730-353">The details of how precedence works are coupled to how route templates are defined:</span></span>

* <span data-ttu-id="02730-354">具有更多段的範本被視為更具體。</span><span class="sxs-lookup"><span data-stu-id="02730-354">Templates with more segments are considered more specific.</span></span>
* <span data-ttu-id="02730-355">具有文本文本的段被認為比參數段更具體。</span><span class="sxs-lookup"><span data-stu-id="02730-355">A segment with literal text is considered more specific than a parameter segment.</span></span>
* <span data-ttu-id="02730-356">具有約束的參數段被認為比沒有約束的參數段更具體。</span><span class="sxs-lookup"><span data-stu-id="02730-356">A parameter segment with a constraint is considered more specific than one without.</span></span>
* <span data-ttu-id="02730-357">複雜段被視為具有約束的參數段的特定段。</span><span class="sxs-lookup"><span data-stu-id="02730-357">A complex segment is considered as specific as a parameter segment with a constraint.</span></span>
* <span data-ttu-id="02730-358">捕獲所有參數都是最不具體的。</span><span class="sxs-lookup"><span data-stu-id="02730-358">Catch all parameters are the least specific.</span></span>

<span data-ttu-id="02730-359">有關精確值的引用,請參閱[GitHub 上的原始碼](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189)。</span><span class="sxs-lookup"><span data-stu-id="02730-359">See the [source code on GitHub](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189) for a reference of exact values.</span></span>

<a name="lg"></a>

### <a name="url-generation-concepts"></a><span data-ttu-id="02730-360">網址產生概念</span><span class="sxs-lookup"><span data-stu-id="02730-360">URL generation concepts</span></span>

<span data-ttu-id="02730-361">網址產生:</span><span class="sxs-lookup"><span data-stu-id="02730-361">URL generation:</span></span>

* <span data-ttu-id="02730-362">路由可以基於一組路由值創建 URL 路徑的過程。</span><span class="sxs-lookup"><span data-stu-id="02730-362">Is the process by which routing can create a URL path based on a set of route values.</span></span>
* <span data-ttu-id="02730-363">允許在終結點和訪問它們的 URL 之間進行邏輯分離。</span><span class="sxs-lookup"><span data-stu-id="02730-363">Allows for a logical separation between endpoints and the URLs that access them.</span></span>

<span data-ttu-id="02730-364">端點路由包括<xref:Microsoft.AspNetCore.Routing.LinkGenerator>API。</span><span class="sxs-lookup"><span data-stu-id="02730-364">Endpoint routing includes the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API.</span></span> <span data-ttu-id="02730-365">`LinkGenerator`是從[DI](xref:fundamentals/dependency-injection)提供的單例服務。</span><span class="sxs-lookup"><span data-stu-id="02730-365">`LinkGenerator` is a singleton service available from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="02730-366">API`LinkGenerator`可以在執行請求的上下文之外使用。</span><span class="sxs-lookup"><span data-stu-id="02730-366">The `LinkGenerator` API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="02730-367">[Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper)和依賴<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>於的方案(如[標記説明程式](xref:mvc/views/tag-helpers/intro)、HTML 幫助器和[操作結果](xref:mvc/controllers/actions)`LinkGenerator`)在內部使用 API 來提供連結生成功能。</span><span class="sxs-lookup"><span data-stu-id="02730-367">[Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper) and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the `LinkGenerator` API internally to provide link generating capabilities.</span></span>

<span data-ttu-id="02730-368">連結產生器背後支援的概念為「位址」\*\*\*\* 和「位址配置」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-368">The link generator is backed by the concept of an **address** and **address schemes**.</span></span> <span data-ttu-id="02730-369">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="02730-369">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="02730-370">例如,許多使用者從控制器中熟悉的路由名稱和路由值方案,Razor Pages 作為位址方案實現。</span><span class="sxs-lookup"><span data-stu-id="02730-370">For example, the route name and route values scenarios many users are familiar with from controllers and Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="02730-371">鏈路產生器可以透過以下擴充方法連結到控制器和 Razor 頁面:</span><span class="sxs-lookup"><span data-stu-id="02730-371">The link generator can link to controllers and Razor Pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="02730-372">這些方法的重載接受包含的`HttpContext`參數。</span><span class="sxs-lookup"><span data-stu-id="02730-372">Overloads of these methods accept arguments that include the `HttpContext`.</span></span> <span data-ttu-id="02730-373">這些方法在功能上等同於[Url.Action](xref:System.Web.Mvc.UrlHelper.Action*)和[Url.Page,](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)但提供了額外的靈活性和選項。</span><span class="sxs-lookup"><span data-stu-id="02730-373">These methods are functionally equivalent to [Url.Action](xref:System.Web.Mvc.UrlHelper.Action*) and [Url.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*), but offer additional flexibility and options.</span></span>

<span data-ttu-id="02730-374">這些方法`GetPath*`與`Url.Action`和`Url.Page`最相似 ,因為它們生成包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-374">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page`, in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="02730-375">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-375">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="02730-376">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-376">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="02730-377">除非重寫,否則將使用執行請求[的環境](#ambient)路由值、URL 基本路徑、方案和主機。</span><span class="sxs-lookup"><span data-stu-id="02730-377">The [ambient](#ambient) route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="02730-378">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="02730-378"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="02730-379">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="02730-379">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="02730-380">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="02730-380">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="02730-381">評估每個端點的 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="02730-381">Each endpoint's <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern> is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="02730-382">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="02730-382">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="02730-383"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="02730-383">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="02730-384">使用鏈路產生器的最便捷方法是透過擴充方法對特定位址類型執行操作:</span><span class="sxs-lookup"><span data-stu-id="02730-384">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type:</span></span>

| <span data-ttu-id="02730-385">擴充方法</span><span class="sxs-lookup"><span data-stu-id="02730-385">Extension Method</span></span> | <span data-ttu-id="02730-386">描述</span><span class="sxs-lookup"><span data-stu-id="02730-386">Description</span></span> |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="02730-387">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-387">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="02730-388">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-388">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="02730-389">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="02730-389">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="02730-390">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="02730-390">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="02730-391">如果未驗證`Host`傳入請求的標頭,則可以將不受信任的請求輸入發送回檢視或頁面中的 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="02730-391">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view or page.</span></span> <span data-ttu-id="02730-392">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-392">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="02730-393">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="02730-393">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="02730-394">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="02730-394">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="02730-395">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-395">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="02730-396">指定一個空的基本路徑來撤銷對`Map*`連結生成的影響。</span><span class="sxs-lookup"><span data-stu-id="02730-396">Specify an empty base path to undo the `Map*` affect on link generation.</span></span>

### <a name="middleware-example"></a><span data-ttu-id="02730-397">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="02730-397">Middleware example</span></span>

<span data-ttu-id="02730-398">在下面的示例中,中間件使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator>API 創建指向列出存儲產品的操作方法的連結。</span><span class="sxs-lookup"><span data-stu-id="02730-398">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create a link to an action method that lists store products.</span></span> <span data-ttu-id="02730-399">使用連結產生器將其注入類,並且呼叫`GenerateLink`可用於應用中的任何類:</span><span class="sxs-lookup"><span data-stu-id="02730-399">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a><span data-ttu-id="02730-400">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="02730-400">Route template reference</span></span>

<span data-ttu-id="02730-401">其中`{}`的標記定義路由參數,這些參數在路由匹配時被綁定。</span><span class="sxs-lookup"><span data-stu-id="02730-401">Tokens within `{}` define route parameters that are bound if the route is matched.</span></span> <span data-ttu-id="02730-402">可以在工藝路線段中定義多個路由參數,但路由參數必須用文本值分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-402">More than one route parameter can be defined in a route segment, but route parameters  must be separated by a literal value.</span></span> <span data-ttu-id="02730-403">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="02730-403">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span>  <span data-ttu-id="02730-404">路由參數必須具有名稱,並且可能具有指定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-404">Route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="02730-405">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="02730-405">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="02730-406">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="02730-406">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="02730-407">要匹配文字路由參數分隔符`{``}`或 ,請通過重複該字元來轉義分隔符。</span><span class="sxs-lookup"><span data-stu-id="02730-407">To match a literal route parameter delimiter `{` or `}`, escape the delimiter by repeating the character.</span></span> <span data-ttu-id="02730-408">例如`{{`或`}}`。</span><span class="sxs-lookup"><span data-stu-id="02730-408">For example `{{` or `}}`.</span></span>

<span data-ttu-id="02730-409">星號`*`或雙星`**`號 :</span><span class="sxs-lookup"><span data-stu-id="02730-409">Asterisk `*` or double asterisk `**`:</span></span>

* <span data-ttu-id="02730-410">可用作路由參數的首碼,以綁定到URI的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="02730-410">Can be used as a prefix to a route parameter to bind to the rest of the URI.</span></span>
* <span data-ttu-id="02730-411">稱為 **「全部捕獲」** 參數。</span><span class="sxs-lookup"><span data-stu-id="02730-411">Are called a **catch-all** parameters.</span></span> <span data-ttu-id="02730-412">例如，`blog/{**slug}`：</span><span class="sxs-lookup"><span data-stu-id="02730-412">For example, `blog/{**slug}`:</span></span>
  * <span data-ttu-id="02730-413">匹配以`/blog`URI 開頭並具有任何值的任何 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-413">Matches any URI that starts with `/blog` and has any value following it.</span></span>
  * <span data-ttu-id="02730-414">以下值`/blog`分配給[slug](https://developer.mozilla.org/docs/Glossary/Slug)路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-414">The value following `/blog` is assigned to the [slug](https://developer.mozilla.org/docs/Glossary/Slug) route value.</span></span>

<span data-ttu-id="02730-415">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="02730-415">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="02730-416">當路由用於生成 URL(包括路徑分隔`/`符 )時,捕獲所有參數將轉義相應的字元。</span><span class="sxs-lookup"><span data-stu-id="02730-416">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator `/` characters.</span></span> <span data-ttu-id="02730-417">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="02730-417">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="02730-418">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="02730-418">Note the escaped forward slash.</span></span> <span data-ttu-id="02730-419">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="02730-419">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="02730-420">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="02730-420">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="02730-421">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="02730-421">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="02730-422">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="02730-422">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="02730-423">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="02730-423">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="02730-424">如果 URL`filename`中 僅存在一個值,則路由`.`將匹配, 因為尾隨是可選的。</span><span class="sxs-lookup"><span data-stu-id="02730-424">If only a value for `filename` exists in the URL, the route matches because the trailing `.` is  optional.</span></span> <span data-ttu-id="02730-425">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="02730-425">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="02730-426">路由參數可能有「預設值」\*\*\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-426">Route parameters may have **default values** designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="02730-427">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-427">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="02730-428">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-428">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="02730-429">通過將問號 (`?`) 添加到參數名稱的末尾,使路由參數成為可選的。</span><span class="sxs-lookup"><span data-stu-id="02730-429">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name.</span></span> <span data-ttu-id="02730-430">例如： `id?` 。</span><span class="sxs-lookup"><span data-stu-id="02730-430">For example, `id?`.</span></span> <span data-ttu-id="02730-431">可選值和預設路由參數之間的區別是:</span><span class="sxs-lookup"><span data-stu-id="02730-431">The difference between optional values and default route parameters is:</span></span>

* <span data-ttu-id="02730-432">具有預設值的路由參數始終生成值。</span><span class="sxs-lookup"><span data-stu-id="02730-432">A route parameter with a default value always produces a value.</span></span>
* <span data-ttu-id="02730-433">僅當請求 URL 提供值時,可選參數才具有值。</span><span class="sxs-lookup"><span data-stu-id="02730-433">An optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="02730-434">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-434">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="02730-435">路由`:`參數名稱之後的添加和約束名稱指定路由參數上的內聯約束。</span><span class="sxs-lookup"><span data-stu-id="02730-435">Adding `:` and constraint name after the route parameter name specifies an inline constraint on a route parameter.</span></span> <span data-ttu-id="02730-436">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 `(...)` 括住。</span><span class="sxs-lookup"><span data-stu-id="02730-436">If the constraint requires arguments, they're enclosed in parentheses `(...)` after the constraint name.</span></span> <span data-ttu-id="02730-437">可以透過額外另一`:`個 規範名稱來指定多個*內聯約束*。</span><span class="sxs-lookup"><span data-stu-id="02730-437">Multiple *inline constraints* can be specified by appending another `:` and constraint name.</span></span>

<span data-ttu-id="02730-438">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="02730-438">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="02730-439">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-439">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="02730-440">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-440">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="02730-441">路由參數可能也有參數變壓器。</span><span class="sxs-lookup"><span data-stu-id="02730-441">Route parameters may also have parameter transformers.</span></span> <span data-ttu-id="02730-442">參數轉換器在生成連結並將操作和頁面與 URL 匹配時轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="02730-442">Parameter transformers transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="02730-443">與約束一樣,參數變壓器可以通過在路由參數名稱之後添加`:`和轉換器名稱來內聯地添加到路由參數中。</span><span class="sxs-lookup"><span data-stu-id="02730-443">Like constraints, parameter transformers can be added inline to a route parameter by adding a `:` and transformer name after the route parameter name.</span></span> <span data-ttu-id="02730-444">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="02730-444">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="02730-445">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-445">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="02730-446">下表演示了範例路由範本及其行為:</span><span class="sxs-lookup"><span data-stu-id="02730-446">The following table demonstrates example route templates and their behavior:</span></span>

| <span data-ttu-id="02730-447">路由範本</span><span class="sxs-lookup"><span data-stu-id="02730-447">Route Template</span></span>                           | <span data-ttu-id="02730-448">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="02730-448">Example Matching URI</span></span>    | <span data-ttu-id="02730-449">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="02730-449">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="02730-450">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="02730-450">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="02730-451">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="02730-451">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="02730-452">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="02730-452">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="02730-453">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="02730-453">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="02730-454">映射到`Products`控制器,`Details`並將`id`操作設置為 123。</span><span class="sxs-lookup"><span data-stu-id="02730-454">Maps to the `Products` controller and  `Details` action with`id` set to 123.</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="02730-455">映射到`Home`控制器與`Index`方法 。</span><span class="sxs-lookup"><span data-stu-id="02730-455">Maps to the `Home` controller and `Index` method.</span></span> <span data-ttu-id="02730-456">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="02730-456">`id` is ignored.</span></span>        |
| `{controller=Home}/{action=Index}/{id?}` | `/Products`         | <span data-ttu-id="02730-457">映射到`Products`控制器與`Index`方法 。</span><span class="sxs-lookup"><span data-stu-id="02730-457">Maps to the `Products` controller and `Index` method.</span></span> <span data-ttu-id="02730-458">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="02730-458">`id` is ignored.</span></span>        |

<span data-ttu-id="02730-459">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="02730-459">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="02730-460">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="02730-460">Constraints and defaults can also be specified outside the route template.</span></span>

### <a name="complex-segments"></a><span data-ttu-id="02730-461">複雜區段</span><span class="sxs-lookup"><span data-stu-id="02730-461">Complex segments</span></span>

<span data-ttu-id="02730-462">通過以[非貪婪](#greedy)的方式從右向左匹配文本分隔符來處理複雜的段。</span><span class="sxs-lookup"><span data-stu-id="02730-462">Complex segments are processed by matching up literal delimiters from right to left in a [non-greedy](#greedy) way.</span></span> <span data-ttu-id="02730-463">例如,`[Route("/a{b}c{d}")]`是一個複雜的段。</span><span class="sxs-lookup"><span data-stu-id="02730-463">For example, `[Route("/a{b}c{d}")]` is a complex segment.</span></span>
<span data-ttu-id="02730-464">複雜段以特定的方式工作,必須理解這些方式才能成功使用它們。</span><span class="sxs-lookup"><span data-stu-id="02730-464">Complex segments work in a particular way that must be understood to use them successfully.</span></span> <span data-ttu-id="02730-465">本節中的示例演示了為什麼複雜段只有在分隔符文本未顯示在參數值內時才真正正常工作。</span><span class="sxs-lookup"><span data-stu-id="02730-465">The example in this section demonstrates why complex segments only really work well when the delimiter text doesn't appear inside the parameter values.</span></span> <span data-ttu-id="02730-466">對於更複雜的情況,需要使用[正則表達式](/dotnet/standard/base-types/regular-expressions)並手動提取值。</span><span class="sxs-lookup"><span data-stu-id="02730-466">Using a [regex](/dotnet/standard/base-types/regular-expressions) and then manually extracting the values is needed for more complex cases.</span></span>

<span data-ttu-id="02730-467">這是路由使用範本`/a{b}c{d}`和 URL`/abcd`路徑執行的步驟的摘要。</span><span class="sxs-lookup"><span data-stu-id="02730-467">This is a summary of the steps that routing performs with the template `/a{b}c{d}` and the URL path `/abcd`.</span></span> <span data-ttu-id="02730-468">協助`|`視覺化演演算法的工作原理:</span><span class="sxs-lookup"><span data-stu-id="02730-468">The `|` is used to help visualize how the algorithm works:</span></span>

* <span data-ttu-id="02730-469">第一個字面(從右至`c`左)是 。</span><span class="sxs-lookup"><span data-stu-id="02730-469">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="02730-470">因此`/abcd`,從右搜索並尋找`/ab|c|d`。</span><span class="sxs-lookup"><span data-stu-id="02730-470">So `/abcd` is searched from right and finds `/ab|c|d`.</span></span>
* <span data-ttu-id="02730-471">右側的所有內容現在`d`都與路由`{d}`參數 匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-471">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="02730-472">下一個字面(從右至`a`左)是 。</span><span class="sxs-lookup"><span data-stu-id="02730-472">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="02730-473">因此`/ab|c|d`,從我們離開的地方開始搜索,然後`a`被`/|a|b|c|d`發現 。</span><span class="sxs-lookup"><span data-stu-id="02730-473">So `/ab|c|d` is searched starting where we left off, then `a` is found `/|a|b|c|d`.</span></span>
* <span data-ttu-id="02730-474">右側的值`b`( ) 現在與`{b}`路由參數 匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-474">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="02730-475">沒有剩餘的文本和剩餘的路由範本,因此這是匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-475">There is no remaining text and no remaining route template, so this is a match.</span></span>

<span data-ttu-id="02730-476">下面是使用同一範本`/a{b}c{d}`和`/aabcd`URL 路徑的否定案例的示例。</span><span class="sxs-lookup"><span data-stu-id="02730-476">Here's an example of a negative case using the same template `/a{b}c{d}` and the URL path `/aabcd`.</span></span> <span data-ttu-id="02730-477">`|`用於幫助可視化演演演算法的工作原理。</span><span class="sxs-lookup"><span data-stu-id="02730-477">The `|` is used to help visualize how the algorithm works.</span></span> <span data-ttu-id="02730-478">此情況不是匹配,由同一演算法解釋:</span><span class="sxs-lookup"><span data-stu-id="02730-478">This case isn't a match, which is explained by the same algorithm:</span></span>
* <span data-ttu-id="02730-479">第一個字面(從右至`c`左)是 。</span><span class="sxs-lookup"><span data-stu-id="02730-479">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="02730-480">因此`/aabcd`,從右搜索並尋找`/aab|c|d`。</span><span class="sxs-lookup"><span data-stu-id="02730-480">So `/aabcd` is searched from right and finds `/aab|c|d`.</span></span>
* <span data-ttu-id="02730-481">右側的所有內容現在`d`都與路由`{d}`參數 匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-481">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="02730-482">下一個字面(從右至`a`左)是 。</span><span class="sxs-lookup"><span data-stu-id="02730-482">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="02730-483">因此`/aab|c|d`,從我們離開的地方開始搜索,然後`a`被`/a|a|b|c|d`發現 。</span><span class="sxs-lookup"><span data-stu-id="02730-483">So `/aab|c|d` is searched starting where we left off, then `a` is found `/a|a|b|c|d`.</span></span>
* <span data-ttu-id="02730-484">右側的值`b`( ) 現在與`{b}`路由參數 匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-484">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="02730-485">此時還有剩餘的文本`a`,但演演演算法已用完路由範本進行解析,因此這不是匹配項。</span><span class="sxs-lookup"><span data-stu-id="02730-485">At this point there is remaining text `a`, but the algorithm has run out of route template to parse, so this is not a match.</span></span>

<span data-ttu-id="02730-486">由於符合演演算法[是非貪婪的](#greedy):</span><span class="sxs-lookup"><span data-stu-id="02730-486">Since the matching algorithm is [non-greedy](#greedy):</span></span>

* <span data-ttu-id="02730-487">它匹配每個步驟中可能最少的文本量。</span><span class="sxs-lookup"><span data-stu-id="02730-487">It matches the smallest amount of text possible in each step.</span></span>
* <span data-ttu-id="02730-488">任何參數值內出現分隔符值的情況都會導致不匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-488">Any case where the delimiter value appears inside the parameter values results in not matching.</span></span>

<span data-ttu-id="02730-489">正則表達式可以更多地控制它們的匹配行為。</span><span class="sxs-lookup"><span data-stu-id="02730-489">Regular expressions provide much more control over their matching behavior.</span></span>

<a name="greedy"></a>

<span data-ttu-id="02730-490">貪婪匹配,也知道為[懶惰匹配](https://wikipedia.org/wiki/Regular_expression#Lazy_matching),匹配最大的可能的字串。</span><span class="sxs-lookup"><span data-stu-id="02730-490">Greedy matching, also know as [lazy matching](https://wikipedia.org/wiki/Regular_expression#Lazy_matching), matches the largest possible string.</span></span> <span data-ttu-id="02730-491">非貪婪匹配盡可能小的字串。</span><span class="sxs-lookup"><span data-stu-id="02730-491">Non-greedy matches the smallest possible string.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="02730-492">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="02730-492">Route constraint reference</span></span>

<span data-ttu-id="02730-493">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="02730-493">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="02730-494">工藝路線約束通常檢查通過工藝路線範本關聯的路由值,並做出正確或錯誤的決定是否該值是否可以接受。</span><span class="sxs-lookup"><span data-stu-id="02730-494">Route constraints generally inspect the route value associated via the route template and make a true or false decision about whether the value is acceptable.</span></span> <span data-ttu-id="02730-495">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="02730-495">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="02730-496">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="02730-496">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="02730-497">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="02730-497">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="02730-498">請勿針對輸入驗證使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-498">Don't use constraints for input validation.</span></span> <span data-ttu-id="02730-499">如果約束用於輸入驗證,則無效輸入會導致`404`"未找到"回應。</span><span class="sxs-lookup"><span data-stu-id="02730-499">If constraints are used for input validation, invalid input results in a `404` Not Found response.</span></span> <span data-ttu-id="02730-500">無效輸入應生成帶有`400`相應錯誤消息的"錯誤請求」。</span><span class="sxs-lookup"><span data-stu-id="02730-500">Invalid input should produce a `400` Bad Request with an appropriate error message.</span></span> <span data-ttu-id="02730-501">路由條件約束會用來釐清類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="02730-501">Route constraints are used to disambiguate similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="02730-502">下表演示了示例路由約束及其預期行為:</span><span class="sxs-lookup"><span data-stu-id="02730-502">The following table demonstrates example route constraints and their expected behavior:</span></span>

| <span data-ttu-id="02730-503">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="02730-503">constraint</span></span> | <span data-ttu-id="02730-504">範例</span><span class="sxs-lookup"><span data-stu-id="02730-504">Example</span></span> | <span data-ttu-id="02730-505">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-505">Example Matches</span></span> | <span data-ttu-id="02730-506">注意</span><span class="sxs-lookup"><span data-stu-id="02730-506">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="02730-507">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-507">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-508">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="02730-508">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="02730-509">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="02730-509">`true`, `FALSE`</span></span> | <span data-ttu-id="02730-510">符合`true`項目`false`或 。</span><span class="sxs-lookup"><span data-stu-id="02730-510">Matches `true` or `false`.</span></span> <span data-ttu-id="02730-511">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="02730-511">Case-insensitive</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="02730-512">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="02730-512">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="02730-513">匹配不變區域性`DateTime`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-513">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="02730-514">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-514">See preceding warning.</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="02730-515">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="02730-515">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="02730-516">匹配不變區域性`decimal`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-516">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="02730-517">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-517">See preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="02730-518">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-518">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-519">匹配不變區域性`double`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-519">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="02730-520">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-520">See preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="02730-521">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-521">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-522">匹配不變區域性`float`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-522">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="02730-523">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-523">See preceding warning.</span></span>|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | <span data-ttu-id="02730-524">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="02730-524">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="02730-525">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-525">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-526">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="02730-526">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="02730-527">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-527">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="02730-528">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-528">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="02730-529">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-529">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="02730-530">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-530">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="02730-531">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="02730-531">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="02730-532">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="02730-532">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="02730-533">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="02730-533">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="02730-534">字串必須包含一個或多個字母字元,`a`-`z`並且不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="02730-534">String must consist of one or more alphabetical characters, `a`-`z` and case-insensitive.</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="02730-535">字串必須與正則表達式匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-535">String must match the regular expression.</span></span> <span data-ttu-id="02730-536">請參閱有關定義正則表達式的提示。</span><span class="sxs-lookup"><span data-stu-id="02730-536">See tips about defining a regular expression.</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="02730-537">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="02730-537">Used to enforce that a non-parameter value is present during URL generation</span></span> |

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="02730-538">多個冒號分隔約束可以應用於單個參數。</span><span class="sxs-lookup"><span data-stu-id="02730-538">Multiple, colon delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="02730-539">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="02730-539">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="02730-540">驗證 URL 並轉換為 CLR 類型的路由約束始終使用不變區域性。</span><span class="sxs-lookup"><span data-stu-id="02730-540">Route constraints that verify the URL and are converted to a CLR type always use the invariant culture.</span></span> <span data-ttu-id="02730-541">例如,轉換為 CLR`int``DateTime`類型或 。</span><span class="sxs-lookup"><span data-stu-id="02730-541">For example, conversion to the CLR type `int` or `DateTime`.</span></span> <span data-ttu-id="02730-542">這些約束假定 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="02730-542">These constraints assume that the URL is not localizable.</span></span> <span data-ttu-id="02730-543">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="02730-543">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="02730-544">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="02730-544">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="02730-545">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="02730-545">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

### <a name="regular-expressions-in-constraints"></a><span data-ttu-id="02730-546">限制中的正規表示式</span><span class="sxs-lookup"><span data-stu-id="02730-546">Regular expressions in constraints</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="02730-547">可以使用`regex(...)`路由約束將正則表達式指定為內聯約束。</span><span class="sxs-lookup"><span data-stu-id="02730-547">Regular expressions can be specified as inline constraints using the `regex(...)` route constraint.</span></span> <span data-ttu-id="02730-548"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>族中的方法也接受約束的物件文本。</span><span class="sxs-lookup"><span data-stu-id="02730-548">Methods in the <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> family also accept an object literal of constraints.</span></span> <span data-ttu-id="02730-549">如果使用該窗體,字串值將解釋為正則表達式。</span><span class="sxs-lookup"><span data-stu-id="02730-549">If that form is used, string values are interpreted as regular expressions.</span></span>

<span data-ttu-id="02730-550">以下代碼使用內聯正則表示式約束:</span><span class="sxs-lookup"><span data-stu-id="02730-550">The following code uses an inline regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

<span data-ttu-id="02730-551">以下代碼使用物件文字指定正規表示式約束:</span><span class="sxs-lookup"><span data-stu-id="02730-551">The following code uses an object literal to specify a regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

<span data-ttu-id="02730-552">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="02730-552">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="02730-553">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="02730-553">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="02730-554">正規表示式使用與路由和 C# 語言類似的分隔符和權杖。</span><span class="sxs-lookup"><span data-stu-id="02730-554">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="02730-555">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="02730-555">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="02730-556">要在內聯約束`^\d{3}-\d{2}-\d{4}$`中使用正則運算式,請使用以下項之一:</span><span class="sxs-lookup"><span data-stu-id="02730-556">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in an inline constraint, use one of the following:</span></span>

* <span data-ttu-id="02730-557">將`\`字串中提供的字元替換為`\\`C# 原始檔檔中的字元,`\`以便轉義 字串轉義字元。</span><span class="sxs-lookup"><span data-stu-id="02730-557">Replace `\` characters provided in the string as `\\` characters in the C# source file in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="02730-558">[逐字串文字](/dotnet/csharp/language-reference/keywords/string)。</span><span class="sxs-lookup"><span data-stu-id="02730-558">[Verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="02730-559">要轉義路由參數分隔符`{`, `}` `[` `]`、 , , 將表示式的字`{{``}}`元`[[``]]`加倍, 例如, 、 、 、 、 、 、 , , , 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、</span><span class="sxs-lookup"><span data-stu-id="02730-559">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression, for example, `{{`, `}}`, `[[`, `]]`.</span></span> <span data-ttu-id="02730-560">下表顯示了正規表示式及其轉義版本:</span><span class="sxs-lookup"><span data-stu-id="02730-560">The following table shows a regular expression and its escaped version:</span></span>

| <span data-ttu-id="02730-561">規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-561">Regular expression</span></span>    | <span data-ttu-id="02730-562">轉義正則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-562">Escaped regular expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="02730-563">路由中使用的正則表達式通常以`^`字元開頭,並匹配字串的起始位置。</span><span class="sxs-lookup"><span data-stu-id="02730-563">Regular expressions used in routing often start with the `^` character and match the starting position of the string.</span></span> <span data-ttu-id="02730-564">運算式通常以`$`字元結尾,並匹配字串的末尾。</span><span class="sxs-lookup"><span data-stu-id="02730-564">The expressions often end with the `$` character and match the end of the string.</span></span> <span data-ttu-id="02730-565">`^`和`$`字元確保正則表達式與整個路由參數值匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-565">The `^` and `$` characters ensure that the regular expression matches the entire route parameter value.</span></span> <span data-ttu-id="02730-566">如果沒有`^`和`$`字元,正則表達式匹配字串中的任何子字串,這通常是不可取的。</span><span class="sxs-lookup"><span data-stu-id="02730-566">Without the `^` and `$` characters, the regular expression matches any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="02730-567">下表提供了示例,並解釋了為什麼它們匹配或不匹配:</span><span class="sxs-lookup"><span data-stu-id="02730-567">The following table provides examples and explains why they match or fail to match:</span></span>

| <span data-ttu-id="02730-568">運算是</span><span class="sxs-lookup"><span data-stu-id="02730-568">Expression</span></span>   | <span data-ttu-id="02730-569">String</span><span class="sxs-lookup"><span data-stu-id="02730-569">String</span></span>    | <span data-ttu-id="02730-570">相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-570">Match</span></span> | <span data-ttu-id="02730-571">註解</span><span class="sxs-lookup"><span data-stu-id="02730-571">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="02730-572">hello</span><span class="sxs-lookup"><span data-stu-id="02730-572">hello</span></span>     | <span data-ttu-id="02730-573">是</span><span class="sxs-lookup"><span data-stu-id="02730-573">Yes</span></span>   | <span data-ttu-id="02730-574">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-574">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-575">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-575">123abc456</span></span> | <span data-ttu-id="02730-576">是</span><span class="sxs-lookup"><span data-stu-id="02730-576">Yes</span></span>   | <span data-ttu-id="02730-577">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-577">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-578">mz</span><span class="sxs-lookup"><span data-stu-id="02730-578">mz</span></span>        | <span data-ttu-id="02730-579">是</span><span class="sxs-lookup"><span data-stu-id="02730-579">Yes</span></span>   | <span data-ttu-id="02730-580">符合運算式</span><span class="sxs-lookup"><span data-stu-id="02730-580">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="02730-581">MZ</span><span class="sxs-lookup"><span data-stu-id="02730-581">MZ</span></span>        | <span data-ttu-id="02730-582">是</span><span class="sxs-lookup"><span data-stu-id="02730-582">Yes</span></span>   | <span data-ttu-id="02730-583">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="02730-583">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="02730-584">hello</span><span class="sxs-lookup"><span data-stu-id="02730-584">hello</span></span>     | <span data-ttu-id="02730-585">否</span><span class="sxs-lookup"><span data-stu-id="02730-585">No</span></span>    | <span data-ttu-id="02730-586">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-586">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="02730-587">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-587">123abc456</span></span> | <span data-ttu-id="02730-588">否</span><span class="sxs-lookup"><span data-stu-id="02730-588">No</span></span>    | <span data-ttu-id="02730-589">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-589">See `^` and `$` above</span></span> |

<span data-ttu-id="02730-590">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-590">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="02730-591">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-591">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="02730-592">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="02730-592">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="02730-593">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="02730-593">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="02730-594">在約束字典中傳遞的與已知約束之一不匹配的約束也被視為正則表達式。</span><span class="sxs-lookup"><span data-stu-id="02730-594">Constraints that are passed in the constraints dictionary that don't match one of the known constraints are also treated as regular expressions.</span></span> <span data-ttu-id="02730-595">在範本中傳遞的與已知約束之一不匹配的約束不被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-595">Constraints that are passed  within a template that don't match one of the known constraints are not treated as regular expressions.</span></span>

### <a name="custom-route-constraints"></a><span data-ttu-id="02730-596">自訂路由約束</span><span class="sxs-lookup"><span data-stu-id="02730-596">Custom route constraints</span></span>

<span data-ttu-id="02730-597">可以通過<xref:Microsoft.AspNetCore.Routing.IRouteConstraint>實現 介面來創建自定義路由約束。</span><span class="sxs-lookup"><span data-stu-id="02730-597">Custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="02730-598">介面`IRouteConstraint`<xref:System.Web.Routing.IRouteConstraint.Match*>包含 ,如果`true`滿足`false`約束 , 則傳回該介面。</span><span class="sxs-lookup"><span data-stu-id="02730-598">The `IRouteConstraint` interface contains <xref:System.Web.Routing.IRouteConstraint.Match*>, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="02730-599">很少需要自定義路由約束。</span><span class="sxs-lookup"><span data-stu-id="02730-599">Custom route constraints are rarely needed.</span></span> <span data-ttu-id="02730-600">在實現自定義路由約束之前,請考慮其他替代方法,如模型綁定。</span><span class="sxs-lookup"><span data-stu-id="02730-600">Before implementing a custom route constraint, consider alternatives, such as model binding.</span></span>

<span data-ttu-id="02730-601">要使用自定義`IRouteConstraint`,路由約束類型必須註冊到服務<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>容器 中的應用。</span><span class="sxs-lookup"><span data-stu-id="02730-601">To use a custom `IRouteConstraint`, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the service container.</span></span> <span data-ttu-id="02730-602">`ConstraintMap` 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 `IRouteConstraint` 實作。</span><span class="sxs-lookup"><span data-stu-id="02730-602">A `ConstraintMap` is a dictionary that maps route constraint keys to `IRouteConstraint` implementations that validate those constraints.</span></span> <span data-ttu-id="02730-603">更新應用程式的 `ConstraintMap` 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="02730-603">An app's `ConstraintMap` can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="02730-604">例如：</span><span class="sxs-lookup"><span data-stu-id="02730-604">For example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

<span data-ttu-id="02730-605">上述限制在以下代碼中應用:</span><span class="sxs-lookup"><span data-stu-id="02730-605">The preceding constraint is applied in the following code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

<span data-ttu-id="02730-606">的`MyCustomConstraint`實`0`用防止 應用程式於路由參數:</span><span class="sxs-lookup"><span data-stu-id="02730-606">The implementation of `MyCustomConstraint` prevents `0` being applied to a route parameter:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="02730-607">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="02730-607">The preceding code:</span></span>

* <span data-ttu-id="02730-608">防止`0``{id}`在 路徑段中。</span><span class="sxs-lookup"><span data-stu-id="02730-608">Prevents `0` in the `{id}` segment of the route.</span></span>
* <span data-ttu-id="02730-609">顯示為提供實現自定義約束的基本範例。</span><span class="sxs-lookup"><span data-stu-id="02730-609">Is shown to provide a basic example of implementing a custom constraint.</span></span> <span data-ttu-id="02730-610">它不應在生產應用中使用。</span><span class="sxs-lookup"><span data-stu-id="02730-610">It should not be used in a production app.</span></span>

<span data-ttu-id="02730-611">以下代碼是防止處理包含的更好的`id``0`方法:</span><span class="sxs-lookup"><span data-stu-id="02730-611">The following code is a better approach to preventing an `id` containing a `0` from being processed:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="02730-612">與`MyCustomConstraint`該方法,上述代碼具有以下優點:</span><span class="sxs-lookup"><span data-stu-id="02730-612">The preceding code has the following advantages over the `MyCustomConstraint` approach:</span></span>

* <span data-ttu-id="02730-613">它不需要自定義約束。</span><span class="sxs-lookup"><span data-stu-id="02730-613">It doesn't require a custom constraint.</span></span>
* <span data-ttu-id="02730-614">當路由參數包括`0`時,它返回更具描述性的錯誤。</span><span class="sxs-lookup"><span data-stu-id="02730-614">It returns a more descriptive error when the route parameter includes `0`.</span></span>

## <a name="parameter-transformer-reference"></a><span data-ttu-id="02730-615">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="02730-615">Parameter transformer reference</span></span>

<span data-ttu-id="02730-616">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="02730-616">Parameter transformers:</span></span>

* <span data-ttu-id="02730-617">使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator>生成連結時執行。</span><span class="sxs-lookup"><span data-stu-id="02730-617">Execute when generating a link using <xref:Microsoft.AspNetCore.Routing.LinkGenerator>.</span></span>
* <span data-ttu-id="02730-618">實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="02730-618">Implement <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>.</span></span>
* <span data-ttu-id="02730-619">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="02730-619">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="02730-620">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="02730-620">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="02730-621">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="02730-621">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="02730-622">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="02730-622">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="02730-623">請考慮以下`IOutboundParameterTransformer`實現:</span><span class="sxs-lookup"><span data-stu-id="02730-623">Consider the following `IOutboundParameterTransformer` implementation:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

<span data-ttu-id="02730-624">要在路由模式中使用參數轉換器,請使用<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>`Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="02730-624">To use a parameter transformer in a route pattern, configure it using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

<span data-ttu-id="02730-625">ASP.NET核心框架使用參數轉換器來轉換端點解析的URI。</span><span class="sxs-lookup"><span data-stu-id="02730-625">The ASP.NET Core framework uses parameter transformers to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="02730-626">此參數`area`轉換器轉換用於符合 的`controller`路由值`action``page`。</span><span class="sxs-lookup"><span data-stu-id="02730-626">For example, parameter transformers transform the route values used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="02730-627">使用前面的路由範本,操作`SubscriptionManagementController.GetAll`與URI`/subscription-management/get-all`匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-627">With the preceding route template, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="02730-628">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-628">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="02730-629">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="02730-629">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="02730-630">ASP.NET核心提供 API 約定,用於使用具有生成路由的參數變壓器:</span><span class="sxs-lookup"><span data-stu-id="02730-630">ASP.NET Core provides API conventions for using parameter transformers with generated routes:</span></span>

* <span data-ttu-id="02730-631"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC 約定將指定的參數轉換器應用於應用中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="02730-631">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="02730-632">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="02730-632">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="02730-633">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="02730-633">For more information, see [Use a parameter transformer to customize token replacement](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="02730-634">剃刀頁面使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention>API 約定。</span><span class="sxs-lookup"><span data-stu-id="02730-634">Razor Pages uses the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API convention.</span></span> <span data-ttu-id="02730-635">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="02730-635">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="02730-636">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="02730-636">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="02730-637">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="02730-637">For more information, see [Use a parameter transformer to customize page routes](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

<a name="ugr"></a>

## <a name="url-generation-reference"></a><span data-ttu-id="02730-638">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="02730-638">URL generation reference</span></span>

<span data-ttu-id="02730-639">本節包含 URL 生成實現的演演演算法的引用。</span><span class="sxs-lookup"><span data-stu-id="02730-639">This section contains a reference for the algorithm implemented by URL generation.</span></span> <span data-ttu-id="02730-640">實際上,URL 生成的最複雜示例使用控制器或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="02730-640">In practice, most complex examples of URL generation use controllers or Razor Pages.</span></span> <span data-ttu-id="02730-641">有關詳細資訊[,請參閱控制器中的路由](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="02730-641">See  [routing in controllers](xref:mvc/controllers/routing) for additional information.</span></span>

<span data-ttu-id="02730-642">URL 生成過程從調用[LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*)或類似方法開始。</span><span class="sxs-lookup"><span data-stu-id="02730-642">The URL generation process begins with a call to [LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*) or a similar method.</span></span> <span data-ttu-id="02730-643">該方法提供位址、一組路由值以及有關 當前請求`HttpContext`的可選資訊。</span><span class="sxs-lookup"><span data-stu-id="02730-643">The method is provided with an address, a set of route values, and optionally information about the current request from `HttpContext`.</span></span>

<span data-ttu-id="02730-644">第一步是使用 與位址類型匹配的[`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1)解析 一組候選終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-644">The first step is to use the address to resolve a set of candidate endpoints using an [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) that matches the address's type.</span></span>

<span data-ttu-id="02730-645">位址方案找到一組候選項后,將反覆排序和處理終結點,直到 URL 生成操作成功。</span><span class="sxs-lookup"><span data-stu-id="02730-645">Once of set of candidates is found by the address scheme, the endpoints are ordered and processed iteratively until a URL generation operation succeeds.</span></span> <span data-ttu-id="02730-646">URL 生成**不**檢查歧義,返回的第一個結果是最終結果。</span><span class="sxs-lookup"><span data-stu-id="02730-646">URL generation does **not** check for ambiguities, the first result returned is the final result.</span></span>

### <a name="troubleshooting-url-generation-with-logging"></a><span data-ttu-id="02730-647">使用紀錄記錄對網址產生進行故障排除</span><span class="sxs-lookup"><span data-stu-id="02730-647">Troubleshooting URL generation with logging</span></span>

<span data-ttu-id="02730-648">此網址產生的變更, 並將紀錄紀錄`Microsoft.AspNetCore.Routing`等級設定為`TRACE`。</span><span class="sxs-lookup"><span data-stu-id="02730-648">The first step in troubleshooting URL generation is setting the logging level of `Microsoft.AspNetCore.Routing` to `TRACE`.</span></span> <span data-ttu-id="02730-649">`LinkGenerator`記錄有關其處理的許多詳細資訊,這些詳細資訊可用於解決問題。</span><span class="sxs-lookup"><span data-stu-id="02730-649">`LinkGenerator` logs many details about its processing which can be useful to troubleshoot problems.</span></span>

<span data-ttu-id="02730-650">有關網址產生的詳細資訊,請參考[網址](#ugr)。</span><span class="sxs-lookup"><span data-stu-id="02730-650">See [URL generation reference](#ugr) for details on URL generation.</span></span>

### <a name="addresses"></a><span data-ttu-id="02730-651">位址</span><span class="sxs-lookup"><span data-stu-id="02730-651">Addresses</span></span>

<span data-ttu-id="02730-652">位址是 URL 生成中用於將調用綁定到一組候選終結點的概念。</span><span class="sxs-lookup"><span data-stu-id="02730-652">Addresses are the concept in URL generation used to bind a call into the link generator to a set of candidate endpoints.</span></span>

<span data-ttu-id="02730-653">預設情況下,位址是一個可擴展的概念,附帶兩個實現:</span><span class="sxs-lookup"><span data-stu-id="02730-653">Addresses are an extensible concept that come with two implementations by default:</span></span>

* <span data-ttu-id="02730-654">使用*終結點名稱*`string`( ) 作為位址:</span><span class="sxs-lookup"><span data-stu-id="02730-654">Using *endpoint name* (`string`) as the address:</span></span>
    * <span data-ttu-id="02730-655">提供與 MVC 路由名稱類似的功能。</span><span class="sxs-lookup"><span data-stu-id="02730-655">Provides similar functionality to MVC's route name.</span></span>
    * <span data-ttu-id="02730-656">使用<xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata>元數據類型。</span><span class="sxs-lookup"><span data-stu-id="02730-656">Uses the <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> metadata type.</span></span>
    * <span data-ttu-id="02730-657">根據所有已註冊終結點的元數據解析提供的字串。</span><span class="sxs-lookup"><span data-stu-id="02730-657">Resolves the provided string against the metadata of all registered endpoints.</span></span>
    * <span data-ttu-id="02730-658">如果多個終結點使用相同的名稱,則在啟動時引發異常。</span><span class="sxs-lookup"><span data-stu-id="02730-658">Throws an exception on startup if multiple endpoints use the same name.</span></span>
    * <span data-ttu-id="02730-659">建議用於控制器和剃刀頁以外的通用用途。</span><span class="sxs-lookup"><span data-stu-id="02730-659">Recommended for general-purpose use outside of controllers and Razor Pages.</span></span>
* <span data-ttu-id="02730-660">使用*路由值*<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>( ) 作為位址:</span><span class="sxs-lookup"><span data-stu-id="02730-660">Using *route values* (<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>) as the address:</span></span>
    * <span data-ttu-id="02730-661">提供與控制器和 Razor 頁面舊 URL 生成類似的功能。</span><span class="sxs-lookup"><span data-stu-id="02730-661">Provides similar functionality to controllers and Razor Pages legacy URL generation.</span></span>
    * <span data-ttu-id="02730-662">擴展和調試非常複雜。</span><span class="sxs-lookup"><span data-stu-id="02730-662">Very complex to extend and debug.</span></span>
    * <span data-ttu-id="02730-663">提供`IUrlHelper`、標記幫助器、HTML 幫助器、操作結果等使用的實現。</span><span class="sxs-lookup"><span data-stu-id="02730-663">Provides the implementation used by `IUrlHelper`, Tag Helpers, HTML Helpers, Action Results, etc.</span></span>

<span data-ttu-id="02730-664">位址機制的使用要透過任何條件, 並使用您要顯示的檔案與符合的動作的關聯:</span><span class="sxs-lookup"><span data-stu-id="02730-664">The role of the address scheme is to make the association between the address and matching endpoints by arbitrary criteria:</span></span>

* <span data-ttu-id="02730-665">終結點名稱方案執行基本字典查找。</span><span class="sxs-lookup"><span data-stu-id="02730-665">The endpoint name scheme performs a basic dictionary lookup.</span></span>
* <span data-ttu-id="02730-666">路由值方案具有複雜的最佳集演演演算法子集。</span><span class="sxs-lookup"><span data-stu-id="02730-666">The route values scheme has a complex best subset of set algorithm.</span></span>

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a><span data-ttu-id="02730-667">環境值與顯式值</span><span class="sxs-lookup"><span data-stu-id="02730-667">Ambient values and explicit values</span></span>

<span data-ttu-id="02730-668">從目前請求中,路由訪問當前請求`HttpContext.Request.RouteValues`的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-668">From the current request, routing accesses the route values of the current request `HttpContext.Request.RouteValues`.</span></span> <span data-ttu-id="02730-669">與目前要求關聯的值稱為**環境值**。</span><span class="sxs-lookup"><span data-stu-id="02730-669">The values associated with the current request are referred to as the **ambient values**.</span></span> <span data-ttu-id="02730-670">為清楚起見,文件將傳遞給方法的路由值稱為**顯式值**。</span><span class="sxs-lookup"><span data-stu-id="02730-670">For the purpose of clarity, the documentation refers to the route values passed in to methods as **explicit values**.</span></span>

<span data-ttu-id="02730-671">下面的範例顯示環境值和顯式值。</span><span class="sxs-lookup"><span data-stu-id="02730-671">The following example shows ambient values and explicit values.</span></span> <span data-ttu-id="02730-672">它提供目前要求的環境值與顯式值: `{ id = 17, }`</span><span class="sxs-lookup"><span data-stu-id="02730-672">It provides ambient values from the current request and explicit values: `{ id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

<span data-ttu-id="02730-673">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="02730-673">The preceding code:</span></span>

* <span data-ttu-id="02730-674">傳回 `/Widget/Index/17`。</span><span class="sxs-lookup"><span data-stu-id="02730-674">Returns `/Widget/Index/17`</span></span>
* <span data-ttu-id="02730-675">透過<xref:Microsoft.AspNetCore.Routing.LinkGenerator> [DI](xref:fundamentals/dependency-injection)取得 。</span><span class="sxs-lookup"><span data-stu-id="02730-675">Gets <xref:Microsoft.AspNetCore.Routing.LinkGenerator> via [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="02730-676">以下代碼不提供環境值與顯式值: `{ controller = "Home", action = "Subscribe", id = 17, }`:</span><span class="sxs-lookup"><span data-stu-id="02730-676">The following code provides no ambient values and explicit values: `{ controller = "Home", action = "Subscribe", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

<span data-ttu-id="02730-677">前面的方法傳回`/Home/Subscribe/17`</span><span class="sxs-lookup"><span data-stu-id="02730-677">The preceding  method returns `/Home/Subscribe/17`</span></span>

<span data-ttu-id="02730-678">傳`WidgetController``/Widget/Subscribe/17`回以下代碼:</span><span class="sxs-lookup"><span data-stu-id="02730-678">The following code in the `WidgetController` returns `/Widget/Subscribe/17`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

<span data-ttu-id="02730-679">以下代碼提供目前要求中的環境值與顯式值中的控制器: `{ action = "Edit", id = 17, }`:</span><span class="sxs-lookup"><span data-stu-id="02730-679">The following code provides the controller from ambient values in the current request and explicit values: `{ action = "Edit", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

<span data-ttu-id="02730-680">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="02730-680">In the preceding code:</span></span>

* <span data-ttu-id="02730-681">`/Gadget/Edit/17`返回。</span><span class="sxs-lookup"><span data-stu-id="02730-681">`/Gadget/Edit/17` is returned.</span></span>
* <span data-ttu-id="02730-682"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url>取得<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>。</span><span class="sxs-lookup"><span data-stu-id="02730-682"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url> gets the <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>.</span></span>
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
<span data-ttu-id="02730-683">生成具有操作方法絕對路徑的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-683">generates a URL with an absolute path for an action method.</span></span> <span data-ttu-id="02730-684">網址包含`action`指定的名稱`route`和值。</span><span class="sxs-lookup"><span data-stu-id="02730-684">The URL contains the specified `action` name and `route` values.</span></span>

<span data-ttu-id="02730-685">以下代碼提供目前要求的環境值與顯式值: `{ page = "./Edit, id = 17, }`:</span><span class="sxs-lookup"><span data-stu-id="02730-685">The following code provides ambient values from the current request and explicit values: `{ page = "./Edit, id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="02730-686">上述代碼集`url`於`/Edit/17`編輯 剃刀頁面包含以下頁面指令時:</span><span class="sxs-lookup"><span data-stu-id="02730-686">The preceding code sets `url` to  `/Edit/17` when the Edit Razor Page contains the following page directive:</span></span>

 `@page "{id:int}"`

<span data-ttu-id="02730-687">若編輯「 頁不包含`"{id:int}"`」 路由樣本`url`,`/Edit?id=17`則為 。</span><span class="sxs-lookup"><span data-stu-id="02730-687">If the Edit page doesn't contain the `"{id:int}"` route template, `url` is `/Edit?id=17`.</span></span>

<span data-ttu-id="02730-688">除了此處描述的規則之外,MVC<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>的行為 還增加了一層複雜性:</span><span class="sxs-lookup"><span data-stu-id="02730-688">The behavior of MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> adds a layer of complexity in addition to the rules described here:</span></span>

* <span data-ttu-id="02730-689">`IUrlHelper`始終提供當前請求中的路由值作為環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-689">`IUrlHelper` always provides the route values from the current request as ambient values.</span></span>
* <span data-ttu-id="02730-690">[IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*)始終將`action`當前`controller`值 和路由值複製為顯式值,除非開發人員重寫。</span><span class="sxs-lookup"><span data-stu-id="02730-690">[IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*) always copies the current `action` and `controller` route values as explicit values unless overridden by the developer.</span></span>
* <span data-ttu-id="02730-691">[IUrlHelper.page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)始終將`page`當前 路由值複製為顯式值,除非被覆蓋。</span><span class="sxs-lookup"><span data-stu-id="02730-691">[IUrlHelper.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*) always copies the current `page` route value as an explicit value unless overridden.</span></span> <!--by the user-->
* <span data-ttu-id="02730-692">`IUrlHelper.Page`始終將當前`handler`路由`null`值 作為顯式值覆蓋,除非被覆蓋。</span><span class="sxs-lookup"><span data-stu-id="02730-692">`IUrlHelper.Page` always overrides the current `handler` route value with `null` as an explicit values unless overridden.</span></span>

<span data-ttu-id="02730-693">使用者通常對環境值的行為細節感到驚訝,因為 MVC 似乎並不遵循其自己的規則。</span><span class="sxs-lookup"><span data-stu-id="02730-693">Users are often surprised by the behavioral details of ambient values, because MVC doesn't seem to follow its own rules.</span></span> <span data-ttu-id="02730-694">出於歷史和相容性的原因,某些路由值(如`action`、、`page``handler``controller`和)具有自己的特殊情況行為。</span><span class="sxs-lookup"><span data-stu-id="02730-694">For historical and compatibility reasons, certain route values such as `action`, `controller`, `page`, and `handler` have their own special-case behavior.</span></span>

<span data-ttu-id="02730-695">提供的等效功能`LinkGenerator.GetPathByAction`和`LinkGenerator.GetPathByPage`複製這些`IUrlHelper`異常,以便進行相容性。</span><span class="sxs-lookup"><span data-stu-id="02730-695">The equivalent functionality provided by `LinkGenerator.GetPathByAction` and `LinkGenerator.GetPathByPage` duplicates these anomalies of `IUrlHelper` for compatibility.</span></span>

### <a name="url-generation-process"></a><span data-ttu-id="02730-696">網址產生程序</span><span class="sxs-lookup"><span data-stu-id="02730-696">URL generation process</span></span>

<span data-ttu-id="02730-697">找到候選終結點集後,URL 產生演演算法:</span><span class="sxs-lookup"><span data-stu-id="02730-697">Once the set of candidate endpoints are found, the URL generation algorithm:</span></span>

* <span data-ttu-id="02730-698">反覆運算地處理端點。</span><span class="sxs-lookup"><span data-stu-id="02730-698">Processes the endpoints iteratively.</span></span>
* <span data-ttu-id="02730-699">返回第一個成功的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-699">Returns the first successful result.</span></span>

<span data-ttu-id="02730-700">此過程的第一步稱為**路由值失效**。</span><span class="sxs-lookup"><span data-stu-id="02730-700">The first step in this process is called **route value invalidation**.</span></span>  <span data-ttu-id="02730-701">路由值失效是路由決定應使用環境值中的路由值以及應忽略的路由值的過程。</span><span class="sxs-lookup"><span data-stu-id="02730-701">Route value invalidation is the process by which routing decides which route values from the ambient values should be used and which should be ignored.</span></span> <span data-ttu-id="02730-702">考慮每個環境值,並與顯式值組合,或忽略。</span><span class="sxs-lookup"><span data-stu-id="02730-702">Each ambient value is considered and either combined with the explicit values, or ignored.</span></span>

<span data-ttu-id="02730-703">考慮環境值角色的最佳方法是嘗試保存應用程式開發人員鍵入,在某些常見情況下。</span><span class="sxs-lookup"><span data-stu-id="02730-703">The best way to think about the role of ambient values is that they attempt to save application developers typing, in some common cases.</span></span> <span data-ttu-id="02730-704">傳統上,環境值有用的方案與 MVC 相關:</span><span class="sxs-lookup"><span data-stu-id="02730-704">Traditionally, the scenarios where ambient values are helpful are related to MVC:</span></span>

* <span data-ttu-id="02730-705">連結到同一控制器中的另一個操作時,不需要指定控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-705">When linking to another action in the same controller, the controller name doesn't need to be specified.</span></span>
* <span data-ttu-id="02730-706">連結到同一區域中的另一個控制器時,不需要指定區域名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-706">When linking to another controller in the same area, the area name doesn't need to be specified.</span></span>
* <span data-ttu-id="02730-707">連結到同一操作方法時,不需要指定路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-707">When linking to the same action method, route values don't need to be specified.</span></span>
* <span data-ttu-id="02730-708">連結到應用的另一部分時,您不希望傳遞在應用的該部分中沒有意義的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-708">When linking to another part of the app, you don't want to carry over route values that have no meaning in that part of the app.</span></span>

<span data-ttu-id="02730-709">調用`LinkGenerator``IUrlHelper`或`null`返回 通常是由於不理解路由值失效引起的。</span><span class="sxs-lookup"><span data-stu-id="02730-709">Calls to `LinkGenerator` or `IUrlHelper` that return `null` are usually caused by not understanding route value invalidation.</span></span> <span data-ttu-id="02730-710">通過顯式指定更多路由值來排除路由值失效的問題,以查看這是否解決了問題。</span><span class="sxs-lookup"><span data-stu-id="02730-710">Troubleshoot route value invalidation by explicitly specifying more of the route values to see if that solves the problem.</span></span>

<span data-ttu-id="02730-711">路由值失效的工作原理是假定應用的 URL 方案是分層的,層次結構由從左到右形成。</span><span class="sxs-lookup"><span data-stu-id="02730-711">Route value invalidation works on the assumption that the app's URL scheme is hierarchical, with a hierarchy formed from left-to-right.</span></span> <span data-ttu-id="02730-712">考慮基本的控制器路由範本`{controller}/{action}/{id?}`,以便直觀地瞭解它在實踐中是如何工作的。</span><span class="sxs-lookup"><span data-stu-id="02730-712">Consider the basic controller route template `{controller}/{action}/{id?}` to get an intuitive sense of how this works in practice.</span></span> <span data-ttu-id="02730-713">變更值的**變更**會使右方顯示的所有路由值 **。**</span><span class="sxs-lookup"><span data-stu-id="02730-713">A **change** to a value **invalidates** all of the route values that appear to the right.</span></span> <span data-ttu-id="02730-714">這反映了關於層次結構的假設。</span><span class="sxs-lookup"><span data-stu-id="02730-714">This reflects the assumption about hierarchy.</span></span> <span data-ttu-id="02730-715">如果應用具有的環境`id`值,並且操作為`controller`指定 不同的值:</span><span class="sxs-lookup"><span data-stu-id="02730-715">If the app has an ambient value for `id`, and the operation specifies a different value for the `controller`:</span></span>

* <span data-ttu-id="02730-716">`id`不會重複使用,因為`{controller}`位於的`{id?}`左側。</span><span class="sxs-lookup"><span data-stu-id="02730-716">`id` won't be reused because `{controller}` is to the left of `{id?}`.</span></span>

<span data-ttu-id="02730-717">展示此原則的一些範例:</span><span class="sxs-lookup"><span data-stu-id="02730-717">Some examples demonstrating this principle:</span></span>

* <span data-ttu-id="02730-718">如果顯式值包含的值`id`,則忽略`id`的環境 值。</span><span class="sxs-lookup"><span data-stu-id="02730-718">If the explicit values contain a value for `id`, the ambient value for `id` is ignored.</span></span> <span data-ttu-id="02730-719">的環境值`controller`,`action`可以使用。</span><span class="sxs-lookup"><span data-stu-id="02730-719">The ambient values for `controller` and `action` can be used.</span></span>
* <span data-ttu-id="02730-720">如果顯式值包含的值`action`,則忽略`action`其的任何環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-720">If the explicit values contain a value for `action`, any ambient value for `action` is ignored.</span></span> <span data-ttu-id="02730-721">可以使用的環境`controller`值。</span><span class="sxs-lookup"><span data-stu-id="02730-721">The ambient values for `controller` can be used.</span></span> <span data-ttu-id="02730-722">如果`action`的 顯式值與`action`的環境 值不同,`id`則不會使用 該值。</span><span class="sxs-lookup"><span data-stu-id="02730-722">If the explicit value for `action` is different from the ambient value for `action`, the `id` value won't be used.</span></span>  <span data-ttu-id="02730-723">如果`action`的 顯式值與`action`的環境 值相同,則`id`可以使用 該值。</span><span class="sxs-lookup"><span data-stu-id="02730-723">If the explicit value for `action` is the same as the ambient value for `action`, the `id` value can be used.</span></span>
* <span data-ttu-id="02730-724">如果顯式值包含的值`controller`,則忽略`controller`其的任何環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-724">If the explicit values contain a value for `controller`, any ambient value for `controller` is ignored.</span></span> <span data-ttu-id="02730-725">如果`controller`的顯式值與`controller`的環境值不同,則不會`action`使用`id`和值。</span><span class="sxs-lookup"><span data-stu-id="02730-725">If the explicit value for `controller` is different from the ambient value for `controller`, the `action` and `id` values won't be used.</span></span> <span data-ttu-id="02730-726">如果`controller`的顯式值與 和的值的`controller`環境 值相同,`action`則`id`可以使用和值。</span><span class="sxs-lookup"><span data-stu-id="02730-726">If the explicit value for `controller` is the same as the ambient value for `controller`, the `action` and `id` values can be used.</span></span>

<span data-ttu-id="02730-727">由於存在屬性路由和專用常規路由,此過程更加複雜。</span><span class="sxs-lookup"><span data-stu-id="02730-727">This process is further complicated by the existence of attribute routes and dedicated conventional routes.</span></span> <span data-ttu-id="02730-728">控制器常規路由,例如`{controller}/{action}/{id?}`使用路由參數指定層次結構。</span><span class="sxs-lookup"><span data-stu-id="02730-728">Controller conventional routes such as `{controller}/{action}/{id?}` specify a hierarchy using route parameters.</span></span> <span data-ttu-id="02730-729">對控制器與 Razor 網頁[的傳統路由](xref:mvc/controllers/routing#dcr)與[屬性路由](xref:mvc/controllers/routing#ar):</span><span class="sxs-lookup"><span data-stu-id="02730-729">For [dedicated conventional routes](xref:mvc/controllers/routing#dcr) and [attribute routes](xref:mvc/controllers/routing#ar) to controllers and Razor Pages:</span></span>

* <span data-ttu-id="02730-730">存在路由值的層次結構。</span><span class="sxs-lookup"><span data-stu-id="02730-730">There is a hierarchy of route values.</span></span>
* <span data-ttu-id="02730-731">它們不顯示在範本中。</span><span class="sxs-lookup"><span data-stu-id="02730-731">They don't appear in the template.</span></span>

<span data-ttu-id="02730-732">在這些情況下,URL生成定義了**所需的值**概念。</span><span class="sxs-lookup"><span data-stu-id="02730-732">For these cases, URL generation defines the **required values** concept.</span></span> <span data-ttu-id="02730-733">由控制器和 Razor Pages 創建的終結點具有指定的值,這些值允許路由值失效。</span><span class="sxs-lookup"><span data-stu-id="02730-733">Endpoints created by controllers and Razor Pages have required values specified that allow route value invalidation to work.</span></span>

<span data-ttu-id="02730-734">路由值失效演算法的詳細資訊:</span><span class="sxs-lookup"><span data-stu-id="02730-734">The route value invalidation algorithm in detail:</span></span>

* <span data-ttu-id="02730-735">所需的值名稱與路由參數結合使用,然後從左至右處理。</span><span class="sxs-lookup"><span data-stu-id="02730-735">The required value names are combined with the route parameters, then processed from left-to-right.</span></span>
* <span data-ttu-id="02730-736">對於每個參數,將比較環境值和顯式值:</span><span class="sxs-lookup"><span data-stu-id="02730-736">For each parameter, the ambient value and explicit value are compared:</span></span>
    * <span data-ttu-id="02730-737">如果環境值和顯式值相同,則該過程將繼續。</span><span class="sxs-lookup"><span data-stu-id="02730-737">If the ambient value and explicit value are the same, the process continues.</span></span>
    * <span data-ttu-id="02730-738">如果環境值存在,並且顯式值不存在,則生成 URL 時將使用環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-738">If the ambient value is present and the explicit value isn't, the ambient value is used when generating the URL.</span></span>
    * <span data-ttu-id="02730-739">如果環境值不存在且顯式值存在,則拒絕環境值和所有後續環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-739">If the ambient value isn't present and the explicit value is, reject the ambient value and all subsequent ambient values.</span></span>
    * <span data-ttu-id="02730-740">如果存在環境值和顯式值,並且兩個值不同,則拒絕環境值和所有後續環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-740">If the ambient value and the explicit value are present, and the two values are different, reject the ambient value and all subsequent ambient values.</span></span>

<span data-ttu-id="02730-741">此時,URL 生成操作已準備好評估路由約束。</span><span class="sxs-lookup"><span data-stu-id="02730-741">At this point, the URL generation operation is ready to evaluate route constraints.</span></span> <span data-ttu-id="02730-742">接受值集與參數預設值相結合,這些預設值提供給約束。</span><span class="sxs-lookup"><span data-stu-id="02730-742">The set of accepted values is combined with the parameter default values, which is provided to constraints.</span></span> <span data-ttu-id="02730-743">如果約束全部通過,則操作將繼續。</span><span class="sxs-lookup"><span data-stu-id="02730-743">If the constraints all pass, the operation continues.</span></span>

<span data-ttu-id="02730-744">接下來,**接受的值**可用於展開路由範本。</span><span class="sxs-lookup"><span data-stu-id="02730-744">Next, the **accepted values** can be used to expand the route template.</span></span> <span data-ttu-id="02730-745">處理式製程的樣本:</span><span class="sxs-lookup"><span data-stu-id="02730-745">The route template is processed:</span></span>

* <span data-ttu-id="02730-746">從左到右。</span><span class="sxs-lookup"><span data-stu-id="02730-746">From left-to-right.</span></span>
* <span data-ttu-id="02730-747">每個參數都代有其接受值。</span><span class="sxs-lookup"><span data-stu-id="02730-747">Each parameter has its accepted value substituted.</span></span>
* <span data-ttu-id="02730-748">以下特殊情況:</span><span class="sxs-lookup"><span data-stu-id="02730-748">With the following special cases:</span></span>
  * <span data-ttu-id="02730-749">如果接受的值缺少值,並且參數具有預設值,則使用預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-749">If the accepted values is missing a value and the parameter has a default value, the default value is used.</span></span>
  * <span data-ttu-id="02730-750">如果接受的值缺少值,並且參數是可選的,則繼續處理。</span><span class="sxs-lookup"><span data-stu-id="02730-750">If the accepted values is missing a value and the parameter is optional, processing continues.</span></span>
  * <span data-ttu-id="02730-751">如果缺少的可選參數右側的任何路由參數具有值,則操作將失敗。</span><span class="sxs-lookup"><span data-stu-id="02730-751">If any route parameter to the right of a missing optional parameter has a value, the operation fails.</span></span>
  * <!-- review default-valued parameters optional parameters --> <span data-ttu-id="02730-752">儘可能摺疊連續的預設值參數和可選參數。</span><span class="sxs-lookup"><span data-stu-id="02730-752">Contiguous default-valued parameters and optional parameters are collapsed where possible.</span></span>

<span data-ttu-id="02730-753">顯式提供與路由段不匹配的值將添加到查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="02730-753">Values explicitly provided that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="02730-754">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-754">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="02730-755">環境值</span><span class="sxs-lookup"><span data-stu-id="02730-755">Ambient Values</span></span>                     | <span data-ttu-id="02730-756">明確值</span><span class="sxs-lookup"><span data-stu-id="02730-756">Explicit Values</span></span>                        | <span data-ttu-id="02730-757">結果</span><span class="sxs-lookup"><span data-stu-id="02730-757">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="02730-758">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-758">controller = "Home"</span></span>                | <span data-ttu-id="02730-759">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-759">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-760">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-760">controller = "Home"</span></span>                | <span data-ttu-id="02730-761">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-761">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="02730-762">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-762">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="02730-763">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-763">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-764">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-764">controller = "Home"</span></span>                | <span data-ttu-id="02730-765">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-765">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

### <a name="problems-with-route-value-invalidation"></a><span data-ttu-id="02730-766">路由值失效問題</span><span class="sxs-lookup"><span data-stu-id="02730-766">Problems with route value invalidation</span></span>

<span data-ttu-id="02730-767">從 ASP.NET Core 3.0 起,早期 ASP.NET Core 版本中使用的某些 URL 生成方案不能很好地與 URL 生成配合使用。</span><span class="sxs-lookup"><span data-stu-id="02730-767">As of ASP.NET Core 3.0, some URL generation schemes used in earlier ASP.NET Core versions don't work well with URL generation.</span></span> <span data-ttu-id="02730-768">ASP.NET核心團隊計劃在未來版本中添加功能來滿足這些需求。</span><span class="sxs-lookup"><span data-stu-id="02730-768">The ASP.NET Core team plans to add features to address these needs in a future release.</span></span> <span data-ttu-id="02730-769">目前,最好的解決方案是使用舊路由。</span><span class="sxs-lookup"><span data-stu-id="02730-769">For now the best solution is to use legacy routing.</span></span>

<span data-ttu-id="02730-770">以下代碼顯示了路由不支援的 URL 生成方案的範例。</span><span class="sxs-lookup"><span data-stu-id="02730-770">The following code shows an example of a URL generation scheme that's not supported by routing.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

<span data-ttu-id="02730-771">在前面的代碼中,`culture`路由參數用於當地語系化。</span><span class="sxs-lookup"><span data-stu-id="02730-771">In the preceding code, the `culture` route parameter is used for localization.</span></span> <span data-ttu-id="02730-772">願望是始終將`culture`參數作為環境值接受。</span><span class="sxs-lookup"><span data-stu-id="02730-772">The desire is to have the `culture` parameter always accepted as an ambient value.</span></span> <span data-ttu-id="02730-773">但是,`culture`由於所需值的工作方式,該參數不被接受為環境值:</span><span class="sxs-lookup"><span data-stu-id="02730-773">However, the `culture` parameter is not accepted as an ambient value because of the way required values work:</span></span>

* <span data-ttu-id="02730-774">在`"default"`工藝路線範本中`culture`, 路由參數`controller`位於的左側,因此`controller`更改為 不會`culture`使 無效。</span><span class="sxs-lookup"><span data-stu-id="02730-774">In the `"default"` route template, the `culture` route parameter is to the left of `controller`, so changes to `controller` won't invalidate `culture`.</span></span>
* <span data-ttu-id="02730-775">在`"blog"`工藝路線範本中`culture`, 路由參數被視為`controller`位於的右側,該參數顯示在所需的值中。</span><span class="sxs-lookup"><span data-stu-id="02730-775">In the `"blog"` route template, the `culture` route parameter is considered to be to the right of `controller`, which appears in the required values.</span></span>

## <a name="configuring-endpoint-metadata"></a><span data-ttu-id="02730-776">設定終結點中繼資料</span><span class="sxs-lookup"><span data-stu-id="02730-776">Configuring endpoint metadata</span></span>

<span data-ttu-id="02730-777">以下連結提供有關設定終結點中繼資料的資訊:</span><span class="sxs-lookup"><span data-stu-id="02730-777">The following links provide information on configuring endpoint metadata:</span></span>

* [<span data-ttu-id="02730-778">使用端點路由啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="02730-778">Enable Cors with endpoint routing</span></span>](xref:security/cors#enable-cors-with-endpoint-routing)
* <span data-ttu-id="02730-779">使用自訂`[MinimumAgeAuthorize]`屬性的[I 授權政策提供者範例](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)</span><span class="sxs-lookup"><span data-stu-id="02730-779">[IAuthorizationPolicyProvider sample](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) using a custom `[MinimumAgeAuthorize]` attribute</span></span>
* <span data-ttu-id="02730-780">[使用授權設定測試認證](xref:security/authentication/identity#test-identity)</span><span class="sxs-lookup"><span data-stu-id="02730-780">[Test authentication with the [Authorize] attribute](xref:security/authentication/identity#test-identity)</span></span>
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* <span data-ttu-id="02730-781">[使用授權設定方案](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span><span class="sxs-lookup"><span data-stu-id="02730-781">[Selecting the scheme with the [Authorize] attribute](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span></span>
* <span data-ttu-id="02730-782">[使用授權設定設定規則](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span><span class="sxs-lookup"><span data-stu-id="02730-782">[Applying policies using the [Authorize] attribute](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span></span>
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a><span data-ttu-id="02730-783">與「需要主機」在路由中匹配主機</span><span class="sxs-lookup"><span data-stu-id="02730-783">Host matching in routes with RequireHost</span></span>

<span data-ttu-id="02730-784"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>將約束應用於需要指定主機的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-784"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*> applies a constraint to the route which requires the specified host.</span></span> <span data-ttu-id="02730-785">`RequireHost`或[[主機]](xref:Microsoft.AspNetCore.Routing.HostAttribute)參數可以是:</span><span class="sxs-lookup"><span data-stu-id="02730-785">The `RequireHost` or [[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute) parameter can be:</span></span>

* <span data-ttu-id="02730-786">主機: `www.domain.com``www.domain.com`, 與任何埠匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-786">Host: `www.domain.com`, matches `www.domain.com` with any port.</span></span>
* <span data-ttu-id="02730-787">具有通配符的`*.domain.com``www.domain.com`主機`subdomain.domain.com`:、`www.subdomain.domain.com`匹配 、或在任何埠上。</span><span class="sxs-lookup"><span data-stu-id="02730-787">Host with wildcard: `*.domain.com`, matches `www.domain.com`, `subdomain.domain.com`, or `www.subdomain.domain.com` on any port.</span></span>
* <span data-ttu-id="02730-788">連接埠: `*:5000`, 符合連接埠 5000 與任何主機。</span><span class="sxs-lookup"><span data-stu-id="02730-788">Port: `*:5000`, matches port 5000 with any host.</span></span>
* <span data-ttu-id="02730-789">主機和埠:`www.domain.com:5000`或`*.domain.com:5000`匹配主機和埠。</span><span class="sxs-lookup"><span data-stu-id="02730-789">Host and port: `www.domain.com:5000` or `*.domain.com:5000`, matches host and port.</span></span>

<span data-ttu-id="02730-790">可以使用`RequireHost``[Host]`或指定多個參數。</span><span class="sxs-lookup"><span data-stu-id="02730-790">Multiple parameters can be specified using `RequireHost` or `[Host]`.</span></span> <span data-ttu-id="02730-791">約束匹配對任何參數有效的主機。</span><span class="sxs-lookup"><span data-stu-id="02730-791">The constraint  matches hosts valid for any of the parameters.</span></span> <span data-ttu-id="02730-792">例如,`[Host("domain.com", "*.domain.com")]``domain.com`符合`www.domain.com`與`subdomain.domain.com`。</span><span class="sxs-lookup"><span data-stu-id="02730-792">For example, `[Host("domain.com", "*.domain.com")]` matches `domain.com`, `www.domain.com`, and `subdomain.domain.com`.</span></span>

<span data-ttu-id="02730-793">以下代碼用於`RequireHost`要求在路由上指定主機:</span><span class="sxs-lookup"><span data-stu-id="02730-793">The following code uses `RequireHost` to require the specified host on the route:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

<span data-ttu-id="02730-794">以下代碼使用控制器上`[Host]`的屬性要求任何指定的主機:</span><span class="sxs-lookup"><span data-stu-id="02730-794">The following code uses the `[Host]` attribute on the controller to require any of the specified hosts:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

<span data-ttu-id="02730-795">將`[Host]`屬性同時套用於控制器和操作方法時:</span><span class="sxs-lookup"><span data-stu-id="02730-795">When the `[Host]` attribute is applied to both the controller and action method:</span></span>

* <span data-ttu-id="02730-796">使用操作的屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-796">The attribute on the action is used.</span></span>
* <span data-ttu-id="02730-797">將忽略控制器屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-797">The controller attribute is ignored.</span></span>

## <a name="performance-guidance-for-routing"></a><span data-ttu-id="02730-798">路由效能指南</span><span class="sxs-lookup"><span data-stu-id="02730-798">Performance guidance for routing</span></span>

<span data-ttu-id="02730-799">大多數路由在 ASP.NET 酷 3.0 中更新,以提高性能。</span><span class="sxs-lookup"><span data-stu-id="02730-799">Most of routing was updated in ASP.NET Core 3.0 to increase performance.</span></span>

<span data-ttu-id="02730-800">當應用存在性能問題時,通常懷疑路由是問題所在。</span><span class="sxs-lookup"><span data-stu-id="02730-800">When an app has performance problems, routing is often suspected as the problem.</span></span> <span data-ttu-id="02730-801">懷疑路由的原因是,控制器和 Razor Pages 等框架在其日誌記錄消息中報告框架內花費的時間。</span><span class="sxs-lookup"><span data-stu-id="02730-801">The reason routing is suspected is that frameworks like controllers and Razor Pages report the amount of time spent inside the framework in their logging messages.</span></span> <span data-ttu-id="02730-802">當控制器報告的時間與請求的總時間之間存在顯著差異時:</span><span class="sxs-lookup"><span data-stu-id="02730-802">When there's a significant difference between the time reported by controllers and the total time of the request:</span></span>

* <span data-ttu-id="02730-803">開發人員會消除他們的應用代碼,作為問題的根源。</span><span class="sxs-lookup"><span data-stu-id="02730-803">Developers eliminate their app code as the source of the problem.</span></span>
* <span data-ttu-id="02730-804">通常假設路由是原因。</span><span class="sxs-lookup"><span data-stu-id="02730-804">It's common to assume routing is the cause.</span></span>

<span data-ttu-id="02730-805">路由使用數千個終結點進行性能測試。</span><span class="sxs-lookup"><span data-stu-id="02730-805">Routing is performance tested using thousands of endpoints.</span></span> <span data-ttu-id="02730-806">典型的應用不太可能僅僅因為太大而遇到性能問題。</span><span class="sxs-lookup"><span data-stu-id="02730-806">It's unlikely that a typical app will encounter a performance problem just by being too large.</span></span> <span data-ttu-id="02730-807">路由性能緩慢的最常見的根本原因通常是行為不端的自定義中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-807">The most common root cause of slow routing performance is usually a badly-behaving custom middleware.</span></span>

<span data-ttu-id="02730-808">以下代碼範例演示了縮小延遲源的基本技術:</span><span class="sxs-lookup"><span data-stu-id="02730-808">This following code sample demonstrates a basic technique for narrowing down the source of delay:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

<span data-ttu-id="02730-809">到時間路由:</span><span class="sxs-lookup"><span data-stu-id="02730-809">To time routing:</span></span>

* <span data-ttu-id="02730-810">將每個中間件與上述代碼中顯示的計時中間件的副本進行交錯。</span><span class="sxs-lookup"><span data-stu-id="02730-810">Interleave each middleware with a copy of the timing middleware shown in the preceding code.</span></span>
* <span data-ttu-id="02730-811">添加唯一標識碼以將計時數據與代碼相關聯。</span><span class="sxs-lookup"><span data-stu-id="02730-811">Add a unique identifier to correlate the timing data with the code.</span></span>

<span data-ttu-id="02730-812">這是縮小延遲的基本方法,當它很重要時,例如,超過`10ms`。</span><span class="sxs-lookup"><span data-stu-id="02730-812">This is a basic way to narrow down the delay when it's significant, for example, more than `10ms`.</span></span>  <span data-ttu-id="02730-813">從`Time 2``Time 1`報表中減`UseRouting`去 中間件內花費的時間。</span><span class="sxs-lookup"><span data-stu-id="02730-813">Subtracting `Time 2` from `Time 1` reports the time spent inside the `UseRouting` middleware.</span></span>

<span data-ttu-id="02730-814">以下代碼對前面的計時代碼使用更緊湊的方法:</span><span class="sxs-lookup"><span data-stu-id="02730-814">The following code uses a more compact approach to the preceding timing code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a><span data-ttu-id="02730-815">潛在的昂貴路由功能</span><span class="sxs-lookup"><span data-stu-id="02730-815">Potentially expensive routing features</span></span>

<span data-ttu-id="02730-816">以下清單提供了一些與基本路由範本相比相對昂貴的路由功能的見解:</span><span class="sxs-lookup"><span data-stu-id="02730-816">The following list provides some insight into routing features that are relatively expensive compared with basic route templates:</span></span>

* <span data-ttu-id="02730-817">正則運算式:可以編寫複雜或運行時間長且輸入量少的正則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-817">Regular expressions: It's possible to write regular expressions that are complex, or have long running time with a small amount of input.</span></span>

* <span data-ttu-id="02730-818">複雜欄位`{x}-{y}-{z}`( ):</span><span class="sxs-lookup"><span data-stu-id="02730-818">Complex segments (`{x}-{y}-{z}`):</span></span> 
  * <span data-ttu-id="02730-819">比分析常規 URL 路徑段要昂貴得多。</span><span class="sxs-lookup"><span data-stu-id="02730-819">Are significantly more expensive than parsing a regular URL path segment.</span></span>
  * <span data-ttu-id="02730-820">導致分配更多子字串。</span><span class="sxs-lookup"><span data-stu-id="02730-820">Result in many more substrings being allocated.</span></span>
  * <span data-ttu-id="02730-821">在核心 3.0 路由性能更新ASP.NET未更新複雜的段邏輯。</span><span class="sxs-lookup"><span data-stu-id="02730-821">The complex segment logic was not updated in ASP.NET Core 3.0 routing performance update.</span></span>

* <span data-ttu-id="02730-822">同步數據訪問:許多複雜應用具有資料庫訪問許可權,作為路由的一部分。</span><span class="sxs-lookup"><span data-stu-id="02730-822">Synchronous data access: Many complex apps have database access as part of their routing.</span></span> <span data-ttu-id="02730-823">ASP.NET Core 2.2 和早期路由可能無法提供正確的擴展點來支援資料庫存取路由。</span><span class="sxs-lookup"><span data-stu-id="02730-823">ASP.NET Core 2.2 and earlier routing might not provide the right extensibility points to support database access routing.</span></span> <span data-ttu-id="02730-824">例如,<xref:Microsoft.AspNetCore.Routing.IRouteConstraint><xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>和是同步的。</span><span class="sxs-lookup"><span data-stu-id="02730-824">For example, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, and <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are synchronous.</span></span> <span data-ttu-id="02730-825">擴展點,如<xref:Microsoft.AspNetCore.Routing.MatcherPolicy>和<xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext>是異步的。</span><span class="sxs-lookup"><span data-stu-id="02730-825">Extensibility points such as <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> and <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> are asynchronous.</span></span>

## <a name="guidance-for-library-authors"></a><span data-ttu-id="02730-826">圖書館作者指南</span><span class="sxs-lookup"><span data-stu-id="02730-826">Guidance for library authors</span></span>

<span data-ttu-id="02730-827">本節包含在路由之上構建的庫作者指南。</span><span class="sxs-lookup"><span data-stu-id="02730-827">This section contains guidance for library authors building on top of routing.</span></span> <span data-ttu-id="02730-828">這些詳細資訊旨在確保應用開發人員使用擴展路由的庫和框架獲得良好的體驗。</span><span class="sxs-lookup"><span data-stu-id="02730-828">These details are intended to ensure that app developers have a good experience using libraries and frameworks that extend routing.</span></span>

### <a name="define-endpoints"></a><span data-ttu-id="02730-829">定義點</span><span class="sxs-lookup"><span data-stu-id="02730-829">Define endpoints</span></span>

<span data-ttu-id="02730-830">要建立使用路由進行 URL 匹配的框架,請首先定義<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>基於的用戶體驗。</span><span class="sxs-lookup"><span data-stu-id="02730-830">To create a framework that uses routing for URL matching, start by defining a user experience that builds on top of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>

<span data-ttu-id="02730-831">**在**上建<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>構 。</span><span class="sxs-lookup"><span data-stu-id="02730-831">**DO** build on top of <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span> <span data-ttu-id="02730-832">這允許使用者使用其他ASP.NET核心功能組成您的框架,而不會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="02730-832">This allows users to compose your framework with other ASP.NET Core features without confusion.</span></span> <span data-ttu-id="02730-833">每個ASP.NET核心範本都包括路由。</span><span class="sxs-lookup"><span data-stu-id="02730-833">Every ASP.NET Core template includes routing.</span></span> <span data-ttu-id="02730-834">假設路由是存在的,並且使用者很熟悉。</span><span class="sxs-lookup"><span data-stu-id="02730-834">Assume routing is present and familiar for users.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="02730-835">**一些從**調用`MapMyFramework(...)`該實現<xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>器 返回密封的混凝土類型。</span><span class="sxs-lookup"><span data-stu-id="02730-835">**DO** return a sealed concrete type from a call to `MapMyFramework(...)` that implements <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>.</span></span> <span data-ttu-id="02730-836">大多數框架`Map...`方法都遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="02730-836">Most framework `Map...` methods follow this pattern.</span></span> <span data-ttu-id="02730-837">介面`IEndpointConventionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="02730-837">The `IEndpointConventionBuilder` interface:</span></span>

* <span data-ttu-id="02730-838">允許元數據的可組合性。</span><span class="sxs-lookup"><span data-stu-id="02730-838">Allows composability of metadata.</span></span>
* <span data-ttu-id="02730-839">以各種擴充方法為目標。</span><span class="sxs-lookup"><span data-stu-id="02730-839">Is targeted by a variety of extension methods.</span></span>

<span data-ttu-id="02730-840">以聲明自己的類型,可以向生成器添加您自己的特定於框架的功能。</span><span class="sxs-lookup"><span data-stu-id="02730-840">Declaring your own type allows you to add your own framework-specific functionality to the builder.</span></span> <span data-ttu-id="02730-841">可以包裝一個框架聲明的生成器,並將調用轉發給它。</span><span class="sxs-lookup"><span data-stu-id="02730-841">It's ok to wrap a framework-declared builder and forward calls to it.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="02730-842">**考慮**寫你<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>自己的 。</span><span class="sxs-lookup"><span data-stu-id="02730-842">**CONSIDER** writing your own <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>.</span></span> <span data-ttu-id="02730-843">`EndpointDataSource`是用於聲明和更新終結點集合的低級基元。</span><span class="sxs-lookup"><span data-stu-id="02730-843">`EndpointDataSource` is the low-level primitive for declaring and updating a collection of endpoints.</span></span> <span data-ttu-id="02730-844">`EndpointDataSource`是控制器和剃刀頁使用的強大 API。</span><span class="sxs-lookup"><span data-stu-id="02730-844">`EndpointDataSource` is a powerful API used by controllers and Razor Pages.</span></span>

<span data-ttu-id="02730-845">路由測試具有不更新資料來源[的基本範例](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17)。</span><span class="sxs-lookup"><span data-stu-id="02730-845">The routing tests have a [basic example](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17) of a non-updating data source.</span></span>

<span data-ttu-id="02730-846">預設情況下**不要**試著`EndpointDataSource`註冊 。</span><span class="sxs-lookup"><span data-stu-id="02730-846">**DO NOT** attempt to register an `EndpointDataSource` by default.</span></span> <span data-ttu-id="02730-847">要求使用者在中<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>註冊您的框架。</span><span class="sxs-lookup"><span data-stu-id="02730-847">Require users to register your framework in <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span> <span data-ttu-id="02730-848">路由的原理是,默認情況下不包含任何內容,這就是`UseEndpoints`註冊終結點的位置。</span><span class="sxs-lookup"><span data-stu-id="02730-848">The philosophy of routing is that nothing is included by default, and that `UseEndpoints` is the place to register endpoints.</span></span>

### <a name="creating-routing-integrated-middleware"></a><span data-ttu-id="02730-849">建立路由整合式中間件</span><span class="sxs-lookup"><span data-stu-id="02730-849">Creating routing-integrated middleware</span></span>

<span data-ttu-id="02730-850">**考慮**將中繼資料類型定義為介面。</span><span class="sxs-lookup"><span data-stu-id="02730-850">**CONSIDER** defining metadata types as an interface.</span></span>

<span data-ttu-id="02730-851">**DO**使將元數據類型用作類和方法的屬性成為可能。</span><span class="sxs-lookup"><span data-stu-id="02730-851">**DO** make it possible to use metadata types as an attribute on classes and methods.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

<span data-ttu-id="02730-852">控制器和 Razor Pages 等框架支援將中繼資料屬性應用於類型和方法。</span><span class="sxs-lookup"><span data-stu-id="02730-852">Frameworks like controllers and Razor Pages support applying metadata attributes to types and methods.</span></span> <span data-ttu-id="02730-853">若宣告中繼資料型態:</span><span class="sxs-lookup"><span data-stu-id="02730-853">If you declare metadata types:</span></span>

* <span data-ttu-id="02730-854">使它們作為[屬性](/dotnet/csharp/programming-guide/concepts/attributes/)可訪問。</span><span class="sxs-lookup"><span data-stu-id="02730-854">Make them accessible as [attributes](/dotnet/csharp/programming-guide/concepts/attributes/).</span></span>
* <span data-ttu-id="02730-855">大多數使用者都熟悉應用屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-855">Most users are familiar with applying attributes.</span></span>

<span data-ttu-id="02730-856">將中繼資料型態聲明為介面會增加另一層靈活性:</span><span class="sxs-lookup"><span data-stu-id="02730-856">Declaring a metadata type as an interface adds another layer of flexibility:</span></span>

* <span data-ttu-id="02730-857">介面是可組合的。</span><span class="sxs-lookup"><span data-stu-id="02730-857">Interfaces are composable.</span></span>
* <span data-ttu-id="02730-858">開發人員可以聲明自己的類型,這些類型結合了多個策略。</span><span class="sxs-lookup"><span data-stu-id="02730-858">Developers can declare their own types that combine multiple policies.</span></span>

<span data-ttu-id="02730-859">**這樣做**可以覆蓋元數據,如以下範例所示:</span><span class="sxs-lookup"><span data-stu-id="02730-859">**DO** make it possible to override metadata, as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

<span data-ttu-id="02730-860">遵循這些準則的最佳方法是避免定義**標籤中繼資料**:</span><span class="sxs-lookup"><span data-stu-id="02730-860">The best way to follow these guidelines is to avoid defining **marker metadata**:</span></span>

* <span data-ttu-id="02730-861">不要只查找元數據類型的存在。</span><span class="sxs-lookup"><span data-stu-id="02730-861">Don't just look for the presence of a metadata type.</span></span>
* <span data-ttu-id="02730-862">在元數據上定義屬性並檢查該屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-862">Define a property on the metadata and check the property.</span></span>

<span data-ttu-id="02730-863">元數據收集按優先順序排序和支援重寫。</span><span class="sxs-lookup"><span data-stu-id="02730-863">The metadata collection is ordered and supports overriding by priority.</span></span> <span data-ttu-id="02730-864">對於控制器,操作方法的元數據最為具體。</span><span class="sxs-lookup"><span data-stu-id="02730-864">In the case of controllers, metadata on the action method is most specific.</span></span>

<span data-ttu-id="02730-865">**一些有**中間件有用,沒有路由。</span><span class="sxs-lookup"><span data-stu-id="02730-865">**DO** make middleware useful with and without routing.</span></span>

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

<span data-ttu-id="02730-866">作為本指南的範例,`UseAuthorization`請考慮中間件。</span><span class="sxs-lookup"><span data-stu-id="02730-866">As an example of this guideline, consider the `UseAuthorization` middleware.</span></span> <span data-ttu-id="02730-867">授權中間件允許您傳遞回退策略。</span><span class="sxs-lookup"><span data-stu-id="02730-867">The authorization middleware allows you to pass in a fallback policy.</span></span> <!-- shown where?  (shown here) --> <span data-ttu-id="02730-868">遞迴退原則 (如果指定)適用於這兩種策略:</span><span class="sxs-lookup"><span data-stu-id="02730-868">The fallback policy, if specified, applies to both:</span></span>

* <span data-ttu-id="02730-869">沒有指定策略的終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-869">Endpoints without a specified policy.</span></span>
* <span data-ttu-id="02730-870">與終結點不匹配的請求。</span><span class="sxs-lookup"><span data-stu-id="02730-870">Requests that don't match an endpoint.</span></span>

<span data-ttu-id="02730-871">這使得授權中間件在路由上下文之外很有用。</span><span class="sxs-lookup"><span data-stu-id="02730-871">This makes the authorization middleware useful outside of the context of routing.</span></span> <span data-ttu-id="02730-872">授權中間件可用於傳統的中間件編程。</span><span class="sxs-lookup"><span data-stu-id="02730-872">The authorization middleware can be used for traditional middleware programming.</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="02730-873">路由負責將請求 URI 映射到終結點,並將傳入請求發送到這些終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-873">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="02730-874">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="02730-874">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="02730-875">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="02730-875">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="02730-876">使用來自應用的路由資訊,路由還能夠生成映射到終結點的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-876">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="02730-877">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="02730-877">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="02730-878"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="02730-878">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="02730-879">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="02730-879">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="02730-880">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="02730-880">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="02730-881">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="02730-881">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02730-882">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="02730-882">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="02730-883">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="02730-883">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="02730-884">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="02730-884">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="02730-885">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02730-885">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="02730-886">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="02730-886">Routing basics</span></span>

<span data-ttu-id="02730-887">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="02730-887">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="02730-888">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="02730-888">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="02730-889">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="02730-889">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="02730-890">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="02730-890">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="02730-891">開發人員通常使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專用常規路由在特殊情況下向應用的高流量區域添加其他簡潔的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-891">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span> <span data-ttu-id="02730-892">特殊情況示例包括博客和電子商務終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-892">Specialized situations examples include, blog and ecommerce endpoints.</span></span>

<span data-ttu-id="02730-893">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="02730-893">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="02730-894">這意味著同一邏輯資源上的許多操作(例如 GET 和 POST)使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-894">This means that many operations, for example, GET, and POST, on the same logical resource use the same URL.</span></span> <span data-ttu-id="02730-895">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="02730-895">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="02730-896">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="02730-896">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="02730-897">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="02730-897">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="02730-898">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="02730-898">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="02730-899">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="02730-899">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="02730-900">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="02730-900">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="02730-901">路由使用*終結點*(`Endpoint`) 表示應用中的邏輯終結點。</span><span class="sxs-lookup"><span data-stu-id="02730-901">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="02730-902">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="02730-902">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="02730-903">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="02730-903">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="02730-904">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="02730-904">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="02730-905">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="02730-905">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="02730-906">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="02730-906">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="02730-907"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="02730-907"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="02730-908">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="02730-908">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="02730-909">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="02730-909">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="02730-910">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="02730-910">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="02730-911">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="02730-911">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="02730-912">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="02730-912">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="02730-913">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="02730-913">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="02730-914">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-914">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="02730-915">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-915">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="02730-916">在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="02730-916">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="02730-917">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="02730-917">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="02730-918">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="02730-918">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="02730-919">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-919">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="02730-920">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-920">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="02730-921">URL 比對</span><span class="sxs-lookup"><span data-stu-id="02730-921">URL matching</span></span>

<span data-ttu-id="02730-922">URL 比對是路由用來將傳入要求分派給「端點」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-922">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="02730-923">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="02730-923">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="02730-924">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="02730-924">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="02730-925">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="02730-925">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="02730-926">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="02730-926">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="02730-927">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="02730-927">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="02730-928">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="02730-928">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="02730-929">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="02730-929">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="02730-930">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="02730-930">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="02730-931">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="02730-931"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="02730-932">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="02730-932">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="02730-933">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="02730-933">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="02730-934">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="02730-934">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="02730-935">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="02730-935">Routes can be nested inside of one another.</span></span> <span data-ttu-id="02730-936"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-936">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="02730-937">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-937">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="02730-938"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="02730-938">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="02730-939">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="02730-939">URL generation with LinkGenerator</span></span>

<span data-ttu-id="02730-940">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-940">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="02730-941">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-941">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="02730-942">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="02730-942">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="02730-943"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>是從[DI](xref:fundamentals/dependency-injection)檢索的單例服務。</span><span class="sxs-lookup"><span data-stu-id="02730-943"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="02730-944">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="02730-944">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="02730-945">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="02730-945">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="02730-946">連結產生器背後支援的概念為「位址」\*\* 和「位址配置」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-946">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="02730-947">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="02730-947">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="02730-948">例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="02730-948">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="02730-949">連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="02730-949">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="02730-950">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="02730-950">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="02730-951">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="02730-951">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="02730-952">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-952">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="02730-953">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-953">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="02730-954">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-954">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="02730-955">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="02730-955">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="02730-956">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="02730-956"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="02730-957">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="02730-957">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="02730-958">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="02730-958">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="02730-959">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="02730-959">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="02730-960">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="02730-960">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="02730-961"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="02730-961">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="02730-962">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="02730-962">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="02730-963">擴充方法</span><span class="sxs-lookup"><span data-stu-id="02730-963">Extension Method</span></span>   | <span data-ttu-id="02730-964">描述</span><span class="sxs-lookup"><span data-stu-id="02730-964">Description</span></span>                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="02730-965">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-965">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="02730-966">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-966">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="02730-967">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="02730-967">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="02730-968">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="02730-968">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="02730-969">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="02730-969">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="02730-970">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-970">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="02730-971">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="02730-971">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="02730-972">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="02730-972">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="02730-973">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-973">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="02730-974">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="02730-974">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="02730-975">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="02730-975">Differences from earlier versions of routing</span></span>

<span data-ttu-id="02730-976">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="02730-976">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="02730-977">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="02730-977">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="02730-978">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="02730-978">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="02730-979">使用 2.1[相容性版本](xref:mvc/compatibility-version)(`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 繼續使用相容性分片。</span><span class="sxs-lookup"><span data-stu-id="02730-979">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="02730-980">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="02730-980">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="02730-981">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="02730-981">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="02730-982">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="02730-982">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="02730-983">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="02730-983">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="02730-984">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="02730-984">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="02730-985">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="02730-985">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="02730-986">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-986">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="02730-987">當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。</span><span class="sxs-lookup"><span data-stu-id="02730-987">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="02730-988">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="02730-988">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="02730-989">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="02730-989">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="02730-990">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="02730-990">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="02730-991">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="02730-991">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="02730-992">不過，如果動作不存在，則端點路由會產生空字串。\*\*</span><span class="sxs-lookup"><span data-stu-id="02730-992">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="02730-993">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="02730-993">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="02730-994">連結產生「環境值失效演算法」\*\* 在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="02730-994">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="02730-995">「環境值失效」\*\* 是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-995">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="02730-996">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="02730-996">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="02730-997">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="02730-997">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="02730-998">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="02730-998">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="02730-999">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="02730-999">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="02730-1000">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="02730-1000">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="02730-1001">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="02730-1001">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="02730-1002">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="02730-1002">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="02730-1003">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="02730-1003">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="02730-1004">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="02730-1004">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="02730-1005">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="02730-1005">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="02730-1006">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="02730-1006">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="02730-1007">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="02730-1007">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="02730-1008">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1008">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="02730-1009">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1009">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="02730-1010">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1010">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="02730-1011">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="02730-1011">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="02730-1012">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1012">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="02730-1013">路由</span><span class="sxs-lookup"><span data-stu-id="02730-1013">Route</span></span>              | <span data-ttu-id="02730-1014">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="02730-1014">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="02730-1015">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="02730-1015">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="02730-1016">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="02730-1016">Middleware example</span></span>

<span data-ttu-id="02730-1017">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="02730-1017">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="02730-1018">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="02730-1018">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

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

### <a name="create-routes"></a><span data-ttu-id="02730-1019">建立路由</span><span class="sxs-lookup"><span data-stu-id="02730-1019">Create routes</span></span>

<span data-ttu-id="02730-1020">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1020">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="02730-1021">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1021">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="02730-1022"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1022"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="02730-1023"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1023"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="02730-1024">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="02730-1024">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="02730-1025">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="02730-1025">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="02730-1026">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1026">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="02730-1027">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1027">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="02730-1028">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="02730-1028">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="02730-1029">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="02730-1029">Route parameters are named.</span></span> <span data-ttu-id="02730-1030">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="02730-1030">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="02730-1031">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1031">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="02730-1032">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1032">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="02730-1033">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1033">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="02730-1034">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1034">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="02730-1035">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1035">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="02730-1036">如果沒有相應的 URL 路徑段,可選參數不會生成路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1036">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="02730-1037">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1037">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="02730-1038">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="02730-1038">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="02730-1039">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="02730-1039">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="02730-1040">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="02730-1040">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="02730-1041">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="02730-1041">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="02730-1042">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-1042">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="02730-1043"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1043">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="02730-1044">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-1044">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="02730-1045">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1045">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="02730-1046">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="02730-1046">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="02730-1047">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="02730-1047">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="02730-1048">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="02730-1048">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="02730-1049">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1049">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="02730-1050">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1050">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="02730-1051">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="02730-1051">Default values can be specified in the route template.</span></span> <span data-ttu-id="02730-1052">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="02730-1052">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="02730-1053">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1053">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="02730-1054">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="02730-1054">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="02730-1055">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1055">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="02730-1057">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="02730-1057">Route class URL generation</span></span>

<span data-ttu-id="02730-1058"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1058">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="02730-1059">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-1059">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="02730-1060">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1060">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="02730-1061">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="02730-1061">What values would be produced?</span></span> <span data-ttu-id="02730-1062">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="02730-1062">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="02730-1063">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1063">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="02730-1064">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="02730-1064">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="02730-1065">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-1065">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="02730-1066">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1066">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="02730-1067">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="02730-1067">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="02730-1068">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="02730-1068">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="02730-1069">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1069">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="02730-1070">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1070">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="02730-1071">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1071">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="02730-1072">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="02730-1072">Use Routing Middleware</span></span>

<span data-ttu-id="02730-1073">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="02730-1073">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="02730-1074">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="02730-1074">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="02730-1075">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="02730-1075">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="02730-1076">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="02730-1076">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="02730-1077"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅匹配 HTTP GET 請求。</span><span class="sxs-lookup"><span data-stu-id="02730-1077"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="02730-1078">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="02730-1078">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="02730-1079">URI</span><span class="sxs-lookup"><span data-stu-id="02730-1079">URI</span></span>                    | <span data-ttu-id="02730-1080">回應</span><span class="sxs-lookup"><span data-stu-id="02730-1080">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="02730-1081">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1081">Hello!</span></span> <span data-ttu-id="02730-1082">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="02730-1082">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="02730-1083">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1083">Hello!</span></span> <span data-ttu-id="02730-1084">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="02730-1084">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="02730-1085">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1085">Hello!</span></span> <span data-ttu-id="02730-1086">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="02730-1086">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="02730-1087">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1087">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="02730-1088">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="02730-1088">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="02730-1089">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="02730-1089">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="02730-1090">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1090">The request falls through, no match.</span></span>              |

<span data-ttu-id="02730-1091">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="02730-1091">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="02730-1092">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="02730-1092">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="02730-1093">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="02730-1093">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="02730-1094">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="02730-1094">Route template reference</span></span>

<span data-ttu-id="02730-1095">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1095">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="02730-1096">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-1096">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="02730-1097">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="02730-1097">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="02730-1098">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-1098">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="02730-1099">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="02730-1099">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="02730-1100">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="02730-1100">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="02730-1101">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="02730-1101">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="02730-1102">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="02730-1102">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="02730-1103">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="02730-1103">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="02730-1104">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="02730-1104">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="02730-1105">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="02730-1105">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="02730-1106">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1106">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="02730-1107">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="02730-1107">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="02730-1108">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1108">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="02730-1109">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-1109">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="02730-1110">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1110">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="02730-1111">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1111">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="02730-1112">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="02730-1112">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="02730-1113">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="02730-1113">Note the escaped forward slash.</span></span> <span data-ttu-id="02730-1114">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="02730-1114">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="02730-1115">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="02730-1115">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="02730-1116">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-1116">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="02730-1117">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1117">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="02730-1118">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1118">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="02730-1119">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="02730-1119">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="02730-1120">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="02730-1120">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="02730-1121">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1121">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="02730-1122">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1122">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="02730-1123">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="02730-1123">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="02730-1124">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-1124">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="02730-1125">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1125">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="02730-1126">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-1126">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="02730-1127">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1127">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="02730-1128">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1128">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="02730-1129">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1129">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="02730-1130">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="02730-1130">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="02730-1131">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1131">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="02730-1132">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1132">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="02730-1133">路由範本</span><span class="sxs-lookup"><span data-stu-id="02730-1133">Route Template</span></span>                           | <span data-ttu-id="02730-1134">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="02730-1134">Example Matching URI</span></span>    | <span data-ttu-id="02730-1135">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="02730-1135">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="02730-1136">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="02730-1136">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="02730-1137">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="02730-1137">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="02730-1138">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="02730-1138">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="02730-1139">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="02730-1139">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="02730-1140">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="02730-1140">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="02730-1141">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="02730-1141">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="02730-1142">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="02730-1142">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="02730-1143">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="02730-1143">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="02730-1144">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1144">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="02730-1145">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="02730-1145">Reserved routing names</span></span>

<span data-ttu-id="02730-1146">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="02730-1146">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="02730-1147">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="02730-1147">Route constraint reference</span></span>

<span data-ttu-id="02730-1148">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="02730-1148">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="02730-1149">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="02730-1149">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="02730-1150">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1150">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="02730-1151">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1151">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="02730-1152">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="02730-1152">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="02730-1153">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-1153">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="02730-1154">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="02730-1154">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="02730-1155">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="02730-1155">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="02730-1156">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1156">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="02730-1157">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="02730-1157">constraint</span></span> | <span data-ttu-id="02730-1158">範例</span><span class="sxs-lookup"><span data-stu-id="02730-1158">Example</span></span> | <span data-ttu-id="02730-1159">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1159">Example Matches</span></span> | <span data-ttu-id="02730-1160">注意</span><span class="sxs-lookup"><span data-stu-id="02730-1160">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="02730-1161">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-1161">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-1162">匹配任何整數。</span><span class="sxs-lookup"><span data-stu-id="02730-1162">Matches any integer.</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="02730-1163">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="02730-1163">`true`, `FALSE`</span></span> | <span data-ttu-id="02730-1164">匹配`true`或「假」。。</span><span class="sxs-lookup"><span data-stu-id="02730-1164">Matches `true` or \`false.</span></span> <span data-ttu-id="02730-1165">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="02730-1165">Case-insensitive.</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="02730-1166">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="02730-1166">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="02730-1167">匹配不變區域性`DateTime`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1167">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="02730-1168">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1168">See  preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="02730-1169">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="02730-1169">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="02730-1170">匹配不變區域性`decimal`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1170">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="02730-1171">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1171">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="02730-1172">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-1172">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-1173">匹配不變區域性`double`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1173">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="02730-1174">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1174">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="02730-1175">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-1175">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-1176">匹配不變區域性`float`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1176">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="02730-1177">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1177">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="02730-1178">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="02730-1178">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="02730-1179">匹配有效`Guid`值。</span><span class="sxs-lookup"><span data-stu-id="02730-1179">Matches a valid `Guid` value.</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="02730-1180">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-1180">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-1181">匹配有效`long`值。</span><span class="sxs-lookup"><span data-stu-id="02730-1181">Matches a valid `long` value.</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="02730-1182">字串必須至少為 4 個字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1182">String must be at least 4 characters.</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="02730-1183">字串最多有 8 個字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1183">String has maximum of 8 characters.</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="02730-1184">字串必須正好為 12 個字元長。</span><span class="sxs-lookup"><span data-stu-id="02730-1184">String must be exactly 12 characters long.</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="02730-1185">字串必須至少為 8,並且最多有 16 個字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1185">String must be at least 8 and has maximum of 16 characters.</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="02730-1186">整數值必須至少為 18。</span><span class="sxs-lookup"><span data-stu-id="02730-1186">Integer value must be at least 18.</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="02730-1187">整數值最大值為 120。</span><span class="sxs-lookup"><span data-stu-id="02730-1187">Integer value maximum of 120.</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="02730-1188">整數值必須至少為 18,最大值必須為 120。</span><span class="sxs-lookup"><span data-stu-id="02730-1188">Integer value must be at least 18 and maximum of 120.</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="02730-1189">字串必須由一個或多個字母字元`a`-`z`組成。</span><span class="sxs-lookup"><span data-stu-id="02730-1189">String must consist of one or more alphabetical characters `a`-`z`.</span></span>  <span data-ttu-id="02730-1190">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="02730-1190">Case-insensitive.</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="02730-1191">字串必須與正則表達式匹配。</span><span class="sxs-lookup"><span data-stu-id="02730-1191">String must match the regular expression.</span></span> <span data-ttu-id="02730-1192">請參閱有關定義正則表達式的提示。</span><span class="sxs-lookup"><span data-stu-id="02730-1192">See tips about defining a regular expression.</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="02730-1193">用於強制在 URL 生成期間存在非參數值。</span><span class="sxs-lookup"><span data-stu-id="02730-1193">Used to enforce that a non-parameter value is present during URL generation.</span></span> |

<span data-ttu-id="02730-1194">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1194">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="02730-1195">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="02730-1195">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="02730-1196">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="02730-1196">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="02730-1197">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="02730-1197">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="02730-1198">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1198">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="02730-1199">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1199">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="02730-1200">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="02730-1200">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="02730-1201">規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1201">Regular expressions</span></span>

<span data-ttu-id="02730-1202">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="02730-1202">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="02730-1203">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="02730-1203">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="02730-1204">正規表示式使用與路由和 C# 語言類似的分隔符和權杖。</span><span class="sxs-lookup"><span data-stu-id="02730-1204">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="02730-1205">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="02730-1205">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="02730-1206">要在路由中使用正規`^\d{3}-\d{2}-\d{4}$`表示式:</span><span class="sxs-lookup"><span data-stu-id="02730-1206">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing:</span></span>

* <span data-ttu-id="02730-1207">表達式必須在字串中作為原始碼中的雙`\`反斜杠字元提供單個反斜杠`\\`字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1207">The expression must have the single backslash `\` characters provided in the string as double backslash `\\` characters in the source code.</span></span>
* <span data-ttu-id="02730-1208">要轉義字串轉`\\`義字元,`\`必須使用正則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-1208">The regular expression must us `\\` in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="02730-1209">正規表示式在使用`\\`[逐字字串文本](/dotnet/csharp/language-reference/keywords/string)時不需要。</span><span class="sxs-lookup"><span data-stu-id="02730-1209">The regular expression doesn't require `\\` when using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="02730-1210">要轉義路由參數分隔符`{` `}` `[`, `]`、 、`{{``}``[[`, 將表示式`]]`的字元加倍, 、 、</span><span class="sxs-lookup"><span data-stu-id="02730-1210">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression `{{`, `}`, `[[`, `]]`.</span></span> <span data-ttu-id="02730-1211">下表顯示了正規表示式和轉義版本:</span><span class="sxs-lookup"><span data-stu-id="02730-1211">The following table shows a regular expression and the escaped version:</span></span>

| <span data-ttu-id="02730-1212">規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1212">Regular Expression</span></span>    | <span data-ttu-id="02730-1213">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1213">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="02730-1214">路由中使用的正則表達式通常以串字開頭`^`,並匹配字串的起始位置。</span><span class="sxs-lookup"><span data-stu-id="02730-1214">Regular expressions used in routing often start with the caret `^` character and match starting position of the string.</span></span> <span data-ttu-id="02730-1215">表達式通常以美元符號`$`字元和字串的匹配端結束。</span><span class="sxs-lookup"><span data-stu-id="02730-1215">The expressions often end with the dollar sign `$` character and match end of the string.</span></span> <span data-ttu-id="02730-1216">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="02730-1216">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="02730-1217">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-1217">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="02730-1218">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="02730-1218">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="02730-1219">運算是</span><span class="sxs-lookup"><span data-stu-id="02730-1219">Expression</span></span>   | <span data-ttu-id="02730-1220">String</span><span class="sxs-lookup"><span data-stu-id="02730-1220">String</span></span>    | <span data-ttu-id="02730-1221">相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1221">Match</span></span> | <span data-ttu-id="02730-1222">註解</span><span class="sxs-lookup"><span data-stu-id="02730-1222">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="02730-1223">hello</span><span class="sxs-lookup"><span data-stu-id="02730-1223">hello</span></span>     | <span data-ttu-id="02730-1224">是</span><span class="sxs-lookup"><span data-stu-id="02730-1224">Yes</span></span>   | <span data-ttu-id="02730-1225">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1225">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-1226">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-1226">123abc456</span></span> | <span data-ttu-id="02730-1227">是</span><span class="sxs-lookup"><span data-stu-id="02730-1227">Yes</span></span>   | <span data-ttu-id="02730-1228">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1228">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-1229">mz</span><span class="sxs-lookup"><span data-stu-id="02730-1229">mz</span></span>        | <span data-ttu-id="02730-1230">是</span><span class="sxs-lookup"><span data-stu-id="02730-1230">Yes</span></span>   | <span data-ttu-id="02730-1231">符合運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1231">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="02730-1232">MZ</span><span class="sxs-lookup"><span data-stu-id="02730-1232">MZ</span></span>        | <span data-ttu-id="02730-1233">是</span><span class="sxs-lookup"><span data-stu-id="02730-1233">Yes</span></span>   | <span data-ttu-id="02730-1234">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="02730-1234">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="02730-1235">hello</span><span class="sxs-lookup"><span data-stu-id="02730-1235">hello</span></span>     | <span data-ttu-id="02730-1236">否</span><span class="sxs-lookup"><span data-stu-id="02730-1236">No</span></span>    | <span data-ttu-id="02730-1237">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-1237">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="02730-1238">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-1238">123abc456</span></span> | <span data-ttu-id="02730-1239">否</span><span class="sxs-lookup"><span data-stu-id="02730-1239">No</span></span>    | <span data-ttu-id="02730-1240">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-1240">See `^` and `$` above</span></span> |

<span data-ttu-id="02730-1241">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-1241">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="02730-1242">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-1242">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="02730-1243">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="02730-1243">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="02730-1244">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1244">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="02730-1245">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-1245">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="02730-1246">自訂路由約束</span><span class="sxs-lookup"><span data-stu-id="02730-1246">Custom route constraints</span></span>

<span data-ttu-id="02730-1247">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="02730-1247">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="02730-1248"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="02730-1248">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="02730-1249">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="02730-1249">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="02730-1250"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="02730-1250">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="02730-1251">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="02730-1251">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="02730-1252">例如：</span><span class="sxs-lookup"><span data-stu-id="02730-1252">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="02730-1253">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="02730-1253">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="02730-1254">例如：</span><span class="sxs-lookup"><span data-stu-id="02730-1254">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="02730-1255">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="02730-1255">Parameter transformer reference</span></span>

<span data-ttu-id="02730-1256">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="02730-1256">Parameter transformers:</span></span>

* <span data-ttu-id="02730-1257">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="02730-1257">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="02730-1258">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="02730-1258">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="02730-1259">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="02730-1259">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="02730-1260">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="02730-1260">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="02730-1261">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1261">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="02730-1262">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="02730-1262">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="02730-1263">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="02730-1263">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="02730-1264">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-1264">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="02730-1265">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1265">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="02730-1266">使用上述的路由，動作 `SubscriptionManagementController.GetAll` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="02730-1266">With the preceding route, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="02730-1267">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1267">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="02730-1268">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="02730-1268">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="02730-1269">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="02730-1269">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="02730-1270">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="02730-1270">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="02730-1271">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1271">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="02730-1272">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="02730-1272">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="02730-1273">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="02730-1273">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="02730-1274">Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="02730-1274">Razor Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="02730-1275">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="02730-1275">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="02730-1276">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="02730-1276">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="02730-1277">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="02730-1277">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="02730-1278">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="02730-1278">URL generation reference</span></span>

<span data-ttu-id="02730-1279">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="02730-1279">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="02730-1280">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="02730-1280">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="02730-1281">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1281">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="02730-1282">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1282">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="02730-1283"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1283">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="02730-1284">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="02730-1284">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="02730-1285">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1285">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="02730-1286">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1286">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="02730-1287">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="02730-1287">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="02730-1288">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1288">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="02730-1289">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="02730-1289">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="02730-1290">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1290">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="02730-1291">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-1291">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="02730-1292">環境值</span><span class="sxs-lookup"><span data-stu-id="02730-1292">Ambient Values</span></span>                     | <span data-ttu-id="02730-1293">明確值</span><span class="sxs-lookup"><span data-stu-id="02730-1293">Explicit Values</span></span>                        | <span data-ttu-id="02730-1294">結果</span><span class="sxs-lookup"><span data-stu-id="02730-1294">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="02730-1295">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1295">controller = "Home"</span></span>                | <span data-ttu-id="02730-1296">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1296">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-1297">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1297">controller = "Home"</span></span>                | <span data-ttu-id="02730-1298">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1298">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="02730-1299">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-1299">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="02730-1300">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1300">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-1301">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1301">controller = "Home"</span></span>                | <span data-ttu-id="02730-1302">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-1302">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="02730-1303">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="02730-1303">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="02730-1304">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="02730-1304">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="02730-1305">複雜區段</span><span class="sxs-lookup"><span data-stu-id="02730-1305">Complex segments</span></span>

<span data-ttu-id="02730-1306">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1306">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="02730-1307">請參閱[此程式碼](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="02730-1307">See [this code](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="02730-1308">[程式法範例](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="02730-1308">The [code sample](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="02730-1309">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1309">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="02730-1310">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="02730-1310">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="02730-1311">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1311">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="02730-1312">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1312">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

<span data-ttu-id="02730-1313">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="02730-1313">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> <span data-ttu-id="02730-1314">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1314">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="02730-1315">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="02730-1315">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="02730-1316">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="02730-1316">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="02730-1317">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02730-1317">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="02730-1318">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="02730-1318">Routing basics</span></span>

<span data-ttu-id="02730-1319">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="02730-1319">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="02730-1320">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="02730-1320">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="02730-1321">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="02730-1321">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="02730-1322">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="02730-1322">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="02730-1323">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="02730-1323">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="02730-1324">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1324">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="02730-1325">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1325">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="02730-1326">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="02730-1326">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="02730-1327">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="02730-1327">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="02730-1328">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1328">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="02730-1329">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="02730-1329">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="02730-1330">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="02730-1330">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="02730-1331">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1331">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="02730-1332">路由使用路由實現<xref:Microsoft.AspNetCore.Routing.IRouter>:</span><span class="sxs-lookup"><span data-stu-id="02730-1332">Routing uses routes implementations of <xref:Microsoft.AspNetCore.Routing.IRouter> to:</span></span>

* <span data-ttu-id="02730-1333">將傳入要求對應至「路由處理常式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1333">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="02730-1334">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1334">Generate the URLs used in responses.</span></span>

<span data-ttu-id="02730-1335">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1335">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="02730-1336">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1336">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="02730-1337">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1337">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="02730-1338">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="02730-1338">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="02730-1339">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="02730-1339">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="02730-1340">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1340">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="02730-1341">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="02730-1341">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="02730-1342"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1342"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="02730-1343">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="02730-1343">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="02730-1344">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="02730-1344">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="02730-1345">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="02730-1345">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="02730-1346"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1346"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
<span data-ttu-id="02730-1347">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="02730-1347">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="02730-1348">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1348">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="02730-1349">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1349">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="02730-1350">URL 比對</span><span class="sxs-lookup"><span data-stu-id="02730-1350">URL matching</span></span>

<span data-ttu-id="02730-1351">URL 比對是路由用來將傳入要求分派給「處理常式」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-1351">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="02730-1352">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="02730-1352">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="02730-1353">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="02730-1353">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="02730-1354">傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="02730-1354">Incoming requests enter the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="02730-1355"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」\*\* 要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1355">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="02730-1356">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1356">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="02730-1357">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="02730-1357">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="02730-1358"><xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="02730-1358">The primary input to <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="02730-1359">[RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="02730-1359">The [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="02730-1360">呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1360">A match that calls <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> also sets the properties of the [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="02730-1361">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1361">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="02730-1362">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="02730-1362">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="02730-1363">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="02730-1363">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="02730-1364">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="02730-1364"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="02730-1365">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1365">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="02730-1366">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="02730-1366">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="02730-1367">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="02730-1367">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="02730-1368">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="02730-1368">Routes can be nested inside of one another.</span></span> <span data-ttu-id="02730-1369"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-1369">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="02730-1370">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1370">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="02730-1371"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="02730-1371">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation"></a><span data-ttu-id="02730-1372">URL 產生</span><span class="sxs-lookup"><span data-stu-id="02730-1372">URL generation</span></span>

<span data-ttu-id="02730-1373">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-1373">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="02730-1374">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-1374">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="02730-1375">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1375">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="02730-1376">每個「路由」\*\* 會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="02730-1376">Each *route* has its <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="02730-1377"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="02730-1377">The primary inputs to <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> are:</span></span>

* [<span data-ttu-id="02730-1378">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="02730-1378">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [<span data-ttu-id="02730-1379">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="02730-1379">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [<span data-ttu-id="02730-1380">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="02730-1380">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

<span data-ttu-id="02730-1381">路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="02730-1381">Routes primarily use the route values provided by <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> and <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="02730-1382"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1382">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="02730-1383">相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1383">In contrast, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="02730-1384">提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="02730-1384">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="02730-1385">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1385">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="02730-1386">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1386">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="02730-1387"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。</span><span class="sxs-lookup"><span data-stu-id="02730-1387">The output of <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is a <xref:Microsoft.AspNetCore.Routing.VirtualPathData>.</span></span> <span data-ttu-id="02730-1388"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1388"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> is a parallel of <xref:Microsoft.AspNetCore.Routing.RouteData>.</span></span> <span data-ttu-id="02730-1389"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-1389"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> contains the <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="02730-1390">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1390">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="02730-1391">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-1391">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="02730-1392">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-1392">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="02730-1393">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="02730-1393">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="02730-1394">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="02730-1394">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="02730-1395">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1395">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

### <a name="create-routes"></a><span data-ttu-id="02730-1396">建立路由</span><span class="sxs-lookup"><span data-stu-id="02730-1396">Create routes</span></span>

<span data-ttu-id="02730-1397">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="02730-1397">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="02730-1398">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」\*\* 語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="02730-1398"><xref:Microsoft.AspNetCore.Routing.Route> uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="02730-1399">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1399"><xref:Microsoft.AspNetCore.Routing.Route> uses the same route template to generate a URL when <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is called.</span></span>

<span data-ttu-id="02730-1400">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1400">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="02730-1401">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1401">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="02730-1402"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1402"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="02730-1403"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1403"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="02730-1404">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1404">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="02730-1405">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1405">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="02730-1406">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="02730-1406">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="02730-1407">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="02730-1407">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="02730-1408">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1408">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="02730-1409">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1409">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="02730-1410">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="02730-1410">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="02730-1411">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="02730-1411">Route parameters are named.</span></span> <span data-ttu-id="02730-1412">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="02730-1412">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="02730-1413">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1413">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="02730-1414">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1414">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="02730-1415">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1415">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="02730-1416">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1416">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="02730-1417">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1417">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="02730-1418">如果沒有相應的 URL 路徑段,可選參數不會生成路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1418">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="02730-1419">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1419">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="02730-1420">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="02730-1420">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="02730-1421">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="02730-1421">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="02730-1422">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="02730-1422">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="02730-1423">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="02730-1423">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="02730-1424">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-1424">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="02730-1425"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1425">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="02730-1426">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-1426">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="02730-1427">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1427">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="02730-1428">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="02730-1428">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="02730-1429">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="02730-1429">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="02730-1430">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="02730-1430">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="02730-1431">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1431">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="02730-1432">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1432">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="02730-1433">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="02730-1433">Default values can be specified in the route template.</span></span> <span data-ttu-id="02730-1434">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="02730-1434">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="02730-1435">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1435">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="02730-1436">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="02730-1436">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="02730-1437">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="02730-1437">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="02730-1439">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="02730-1439">Route class URL generation</span></span>

<span data-ttu-id="02730-1440"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1440">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="02730-1441">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="02730-1441">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="02730-1442">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1442">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="02730-1443">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="02730-1443">What values would be produced?</span></span> <span data-ttu-id="02730-1444">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="02730-1444">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="02730-1445">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1445">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="02730-1446">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="02730-1446">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="02730-1447">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="02730-1447">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="02730-1448">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="02730-1448">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="02730-1449">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="02730-1449">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="02730-1450">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="02730-1450">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="02730-1451">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1451">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="02730-1452">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="02730-1452">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="02730-1453">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1453">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="02730-1454">使用路由中間件</span><span class="sxs-lookup"><span data-stu-id="02730-1454">Use routing middleware</span></span>

<span data-ttu-id="02730-1455">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="02730-1455">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="02730-1456">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="02730-1456">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="02730-1457">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="02730-1457">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="02730-1458">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="02730-1458">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="02730-1459"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅匹配 HTTP GET 請求。</span><span class="sxs-lookup"><span data-stu-id="02730-1459"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="02730-1460">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="02730-1460">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="02730-1461">URI</span><span class="sxs-lookup"><span data-stu-id="02730-1461">URI</span></span>                    | <span data-ttu-id="02730-1462">回應</span><span class="sxs-lookup"><span data-stu-id="02730-1462">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="02730-1463">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1463">Hello!</span></span> <span data-ttu-id="02730-1464">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="02730-1464">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="02730-1465">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1465">Hello!</span></span> <span data-ttu-id="02730-1466">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="02730-1466">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="02730-1467">Hello!</span><span class="sxs-lookup"><span data-stu-id="02730-1467">Hello!</span></span> <span data-ttu-id="02730-1468">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="02730-1468">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="02730-1469">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1469">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="02730-1470">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="02730-1470">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="02730-1471">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="02730-1471">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="02730-1472">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1472">The request falls through, no match.</span></span>              |

<span data-ttu-id="02730-1473">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="02730-1473">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="02730-1474">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="02730-1474">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

<span data-ttu-id="02730-1475">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="02730-1475">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="02730-1476">其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="02730-1476">Some of listed methods, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, require a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="02730-1477">路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」\*\* 使用。</span><span class="sxs-lookup"><span data-stu-id="02730-1477">The <xref:Microsoft.AspNetCore.Http.RequestDelegate> is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="02730-1478">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="02730-1478">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="02730-1479">如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="02730-1479">If the `Map*` method doesn't accept a handler, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

<span data-ttu-id="02730-1480">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="02730-1480">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="02730-1481">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="02730-1481">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="02730-1482">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="02730-1482">Route template reference</span></span>

<span data-ttu-id="02730-1483">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1483">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="02730-1484">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-1484">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="02730-1485">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="02730-1485">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="02730-1486">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="02730-1486">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="02730-1487">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="02730-1487">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="02730-1488">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="02730-1488">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="02730-1489">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="02730-1489">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="02730-1490">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="02730-1490">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="02730-1491">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="02730-1491">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="02730-1492">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="02730-1492">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="02730-1493">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="02730-1493">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="02730-1494">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="02730-1494">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="02730-1495">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="02730-1495">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="02730-1496">這稱為「全部擷取」\*\* 參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1496">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="02730-1497">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="02730-1497">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="02730-1498">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1498">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="02730-1499">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="02730-1499">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="02730-1500">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="02730-1500">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="02730-1501">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="02730-1501">Note the escaped forward slash.</span></span>

<span data-ttu-id="02730-1502">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="02730-1502">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="02730-1503">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1503">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="02730-1504">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="02730-1504">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="02730-1505">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="02730-1505">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="02730-1506">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="02730-1506">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="02730-1507">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1507">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="02730-1508">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="02730-1508">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="02730-1509">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="02730-1509">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="02730-1510">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="02730-1510">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="02730-1511">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1511">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="02730-1512">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-1512">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="02730-1513">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="02730-1513">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="02730-1514">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1514">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="02730-1515">路由範本</span><span class="sxs-lookup"><span data-stu-id="02730-1515">Route Template</span></span>                           | <span data-ttu-id="02730-1516">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="02730-1516">Example Matching URI</span></span>    | <span data-ttu-id="02730-1517">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="02730-1517">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="02730-1518">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="02730-1518">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="02730-1519">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="02730-1519">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="02730-1520">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="02730-1520">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="02730-1521">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="02730-1521">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="02730-1522">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="02730-1522">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="02730-1523">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="02730-1523">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="02730-1524">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="02730-1524">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="02730-1525">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="02730-1525">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="02730-1526">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1526">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="02730-1527">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="02730-1527">Route constraint reference</span></span>

<span data-ttu-id="02730-1528">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="02730-1528">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="02730-1529">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="02730-1529">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="02730-1530">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1530">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="02730-1531">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="02730-1531">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="02730-1532">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="02730-1532">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="02730-1533">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="02730-1533">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="02730-1534">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="02730-1534">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="02730-1535">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="02730-1535">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="02730-1536">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="02730-1536">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="02730-1537">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="02730-1537">constraint</span></span> | <span data-ttu-id="02730-1538">範例</span><span class="sxs-lookup"><span data-stu-id="02730-1538">Example</span></span> | <span data-ttu-id="02730-1539">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1539">Example Matches</span></span> | <span data-ttu-id="02730-1540">注意</span><span class="sxs-lookup"><span data-stu-id="02730-1540">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="02730-1541">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-1541">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-1542">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="02730-1542">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="02730-1543">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="02730-1543">`true`, `FALSE`</span></span> | <span data-ttu-id="02730-1544">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="02730-1544">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="02730-1545">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="02730-1545">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="02730-1546">匹配不變區域性`DateTime`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1546">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="02730-1547">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1547">See  preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="02730-1548">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="02730-1548">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="02730-1549">匹配不變區域性`decimal`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1549">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="02730-1550">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1550">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="02730-1551">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-1551">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-1552">匹配不變區域性`double`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1552">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="02730-1553">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1553">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="02730-1554">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="02730-1554">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="02730-1555">匹配不變區域性`float`中的有效值。</span><span class="sxs-lookup"><span data-stu-id="02730-1555">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="02730-1556">請參閱前面的警告。</span><span class="sxs-lookup"><span data-stu-id="02730-1556">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="02730-1557">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="02730-1557">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="02730-1558">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="02730-1558">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="02730-1559">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="02730-1559">`123456789`, `-123456789`</span></span> | <span data-ttu-id="02730-1560">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="02730-1560">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="02730-1561">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-1561">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="02730-1562">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-1562">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="02730-1563">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-1563">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="02730-1564">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="02730-1564">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="02730-1565">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="02730-1565">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="02730-1566">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="02730-1566">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="02730-1567">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="02730-1567">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="02730-1568">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="02730-1568">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="02730-1569">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="02730-1569">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="02730-1570">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="02730-1570">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="02730-1571">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="02730-1571">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="02730-1572">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="02730-1572">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="02730-1573">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="02730-1573">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="02730-1574">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="02730-1574">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="02730-1575">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="02730-1575">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="02730-1576">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1576">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="02730-1577">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="02730-1577">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="02730-1578">規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1578">Regular expressions</span></span>

<span data-ttu-id="02730-1579">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="02730-1579">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="02730-1580">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="02730-1580">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="02730-1581">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="02730-1581">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="02730-1582">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="02730-1582">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="02730-1583">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="02730-1583">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="02730-1584">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="02730-1584">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="02730-1585">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="02730-1585">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="02730-1586">規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1586">Regular Expression</span></span>    | <span data-ttu-id="02730-1587">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1587">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="02730-1588">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="02730-1588">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="02730-1589">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="02730-1589">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="02730-1590">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="02730-1590">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="02730-1591">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-1591">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="02730-1592">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="02730-1592">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="02730-1593">運算是</span><span class="sxs-lookup"><span data-stu-id="02730-1593">Expression</span></span>   | <span data-ttu-id="02730-1594">String</span><span class="sxs-lookup"><span data-stu-id="02730-1594">String</span></span>    | <span data-ttu-id="02730-1595">相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1595">Match</span></span> | <span data-ttu-id="02730-1596">註解</span><span class="sxs-lookup"><span data-stu-id="02730-1596">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="02730-1597">hello</span><span class="sxs-lookup"><span data-stu-id="02730-1597">hello</span></span>     | <span data-ttu-id="02730-1598">是</span><span class="sxs-lookup"><span data-stu-id="02730-1598">Yes</span></span>   | <span data-ttu-id="02730-1599">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1599">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-1600">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-1600">123abc456</span></span> | <span data-ttu-id="02730-1601">是</span><span class="sxs-lookup"><span data-stu-id="02730-1601">Yes</span></span>   | <span data-ttu-id="02730-1602">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="02730-1602">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="02730-1603">mz</span><span class="sxs-lookup"><span data-stu-id="02730-1603">mz</span></span>        | <span data-ttu-id="02730-1604">是</span><span class="sxs-lookup"><span data-stu-id="02730-1604">Yes</span></span>   | <span data-ttu-id="02730-1605">符合運算式</span><span class="sxs-lookup"><span data-stu-id="02730-1605">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="02730-1606">MZ</span><span class="sxs-lookup"><span data-stu-id="02730-1606">MZ</span></span>        | <span data-ttu-id="02730-1607">是</span><span class="sxs-lookup"><span data-stu-id="02730-1607">Yes</span></span>   | <span data-ttu-id="02730-1608">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="02730-1608">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="02730-1609">hello</span><span class="sxs-lookup"><span data-stu-id="02730-1609">hello</span></span>     | <span data-ttu-id="02730-1610">否</span><span class="sxs-lookup"><span data-stu-id="02730-1610">No</span></span>    | <span data-ttu-id="02730-1611">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-1611">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="02730-1612">123abc456</span><span class="sxs-lookup"><span data-stu-id="02730-1612">123abc456</span></span> | <span data-ttu-id="02730-1613">否</span><span class="sxs-lookup"><span data-stu-id="02730-1613">No</span></span>    | <span data-ttu-id="02730-1614">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="02730-1614">See `^` and `$` above</span></span> |

<span data-ttu-id="02730-1615">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="02730-1615">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="02730-1616">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-1616">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="02730-1617">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="02730-1617">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="02730-1618">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="02730-1618">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="02730-1619">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="02730-1619">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="02730-1620">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="02730-1620">Custom Route Constraints</span></span>

<span data-ttu-id="02730-1621">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="02730-1621">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="02730-1622"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="02730-1622">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="02730-1623">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="02730-1623">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="02730-1624"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="02730-1624">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="02730-1625">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="02730-1625">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="02730-1626">例如：</span><span class="sxs-lookup"><span data-stu-id="02730-1626">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="02730-1627">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="02730-1627">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="02730-1628">例如：</span><span class="sxs-lookup"><span data-stu-id="02730-1628">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a><span data-ttu-id="02730-1629">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="02730-1629">URL generation reference</span></span>

<span data-ttu-id="02730-1630">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="02730-1630">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="02730-1631">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="02730-1631">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="02730-1632">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="02730-1632">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="02730-1633">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="02730-1633">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="02730-1634"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="02730-1634">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="02730-1635">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="02730-1635">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="02730-1636">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1636">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="02730-1637">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1637">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="02730-1638">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="02730-1638">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="02730-1639">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="02730-1639">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="02730-1640">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="02730-1640">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="02730-1641">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="02730-1641">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="02730-1642">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="02730-1642">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="02730-1643">環境值</span><span class="sxs-lookup"><span data-stu-id="02730-1643">Ambient Values</span></span>                     | <span data-ttu-id="02730-1644">明確值</span><span class="sxs-lookup"><span data-stu-id="02730-1644">Explicit Values</span></span>                        | <span data-ttu-id="02730-1645">結果</span><span class="sxs-lookup"><span data-stu-id="02730-1645">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="02730-1646">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1646">controller = "Home"</span></span>                | <span data-ttu-id="02730-1647">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1647">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-1648">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1648">controller = "Home"</span></span>                | <span data-ttu-id="02730-1649">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1649">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="02730-1650">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-1650">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="02730-1651">action = "About"</span><span class="sxs-lookup"><span data-stu-id="02730-1651">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="02730-1652">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="02730-1652">controller = "Home"</span></span>                | <span data-ttu-id="02730-1653">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="02730-1653">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="02730-1654">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="02730-1654">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="02730-1655">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="02730-1655">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="02730-1656">複雜區段</span><span class="sxs-lookup"><span data-stu-id="02730-1656">Complex segments</span></span>

<span data-ttu-id="02730-1657">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="02730-1657">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="02730-1658">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="02730-1658">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="02730-1659">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="02730-1659">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>

::: moniker-end
