---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 探索 ASP.NET Core 路由如何負責比對 HTTP 要求和分派至可執行檔端點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/1/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/routing
ms.openlocfilehash: e2b1672066a5b3c0bb6bc44e316bda93ae0f21b7
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774898"
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="ee246-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="ee246-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="ee246-104">By [Ryan Nowak](https://github.com/rynowak)、 [Kirk Larkin](https://twitter.com/serpent5)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee246-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ee246-105">路由會負責比對傳入 HTTP 要求，並將這些要求分派至應用程式的可執行端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-105">Routing is responsible for matching incoming HTTP requests and dispatching those requests to the app's executable endpoints.</span></span> <span data-ttu-id="ee246-106">[端點](#endpoint)是應用程式的可執行要求處理常式代碼單位。</span><span class="sxs-lookup"><span data-stu-id="ee246-106">[Endpoints](#endpoint) are the app's units of executable request-handling code.</span></span> <span data-ttu-id="ee246-107">端點會在應用程式中定義，並在應用程式啟動時設定。</span><span class="sxs-lookup"><span data-stu-id="ee246-107">Endpoints are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ee246-108">端點比對程式可以從要求的 URL 中解壓縮值，並提供這些值來進行要求處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-108">The endpoint matching process can extract values from the request's URL and provide those values for request processing.</span></span> <span data-ttu-id="ee246-109">使用來自應用程式的端點資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="ee246-109">Using endpoint information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="ee246-110">應用程式可以使用下列內容來設定路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-110">Apps can configure routing using:</span></span>

- <span data-ttu-id="ee246-111">Controllers</span><span class="sxs-lookup"><span data-stu-id="ee246-111">Controllers</span></span>
- <span data-ttu-id="ee246-112">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ee246-112">Razor Pages</span></span>
- <span data-ttu-id="ee246-113">SignalR</span><span class="sxs-lookup"><span data-stu-id="ee246-113">SignalR</span></span>
- <span data-ttu-id="ee246-114">gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="ee246-114">gRPC Services</span></span>
- <span data-ttu-id="ee246-115">端點啟用的[中介軟體](xref:fundamentals/middleware/index)，例如[健康情況檢查](xref:host-and-deploy/health-checks)。</span><span class="sxs-lookup"><span data-stu-id="ee246-115">Endpoint-enabled [middleware](xref:fundamentals/middleware/index) such as [Health Checks](xref:host-and-deploy/health-checks).</span></span>
- <span data-ttu-id="ee246-116">已向路由註冊的委派和 lambda。</span><span class="sxs-lookup"><span data-stu-id="ee246-116">Delegates and lambdas registered with routing.</span></span>

<span data-ttu-id="ee246-117">本檔涵蓋 ASP.NET Core 路由的低層級詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-117">This document covers low-level details of ASP.NET Core routing.</span></span> <span data-ttu-id="ee246-118">如需設定路由的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="ee246-118">For information on configuring routing:</span></span>

* <span data-ttu-id="ee246-119">針對控制器，請<xref:mvc/controllers/routing>參閱。</span><span class="sxs-lookup"><span data-stu-id="ee246-119">For controllers, see <xref:mvc/controllers/routing>.</span></span>
* <span data-ttu-id="ee246-120">如 Razor Pages 慣例，請<xref:razor-pages/razor-pages-conventions>參閱。</span><span class="sxs-lookup"><span data-stu-id="ee246-120">For Razor Pages conventions, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ee246-121">本檔中所述的端點路由系統適用于 ASP.NET Core 3.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="ee246-121">The endpoint routing system described in this document applies to ASP.NET Core 3.0 and later.</span></span> <span data-ttu-id="ee246-122">如需以為基礎的先前路由系統<xref:Microsoft.AspNetCore.Routing.IRouter>的詳細資訊，請使用下列其中一種方法來選取 ASP.NET Core 2.1 版本：</span><span class="sxs-lookup"><span data-stu-id="ee246-122">For information on the previous routing system based on <xref:Microsoft.AspNetCore.Routing.IRouter>, select the ASP.NET Core 2.1 version using one of the following approaches:</span></span>

* <span data-ttu-id="ee246-123">先前版本的版本選取器。</span><span class="sxs-lookup"><span data-stu-id="ee246-123">The version selector for a previous version.</span></span>
* <span data-ttu-id="ee246-124">選取 [ [ASP.NET Core 2.1 路由](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)]。</span><span class="sxs-lookup"><span data-stu-id="ee246-124">Select [ASP.NET Core 2.1 routing](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="ee246-125">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ee246-125">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ee246-126">此檔的下載範例會由特定`Startup`類別啟用。</span><span class="sxs-lookup"><span data-stu-id="ee246-126">The download samples for this document are enabled by a specific `Startup` class.</span></span> <span data-ttu-id="ee246-127">若要執行特定的範例，請修改*Program.cs*以呼叫`Startup`所需的類別。</span><span class="sxs-lookup"><span data-stu-id="ee246-127">To run a specific sample, modify *Program.cs* to call the desired `Startup` class.</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ee246-128">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ee246-128">Routing basics</span></span>

<span data-ttu-id="ee246-129">所有 ASP.NET Core 範本都會在產生的程式碼中包含路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-129">All ASP.NET Core templates include routing in the generated code.</span></span> <span data-ttu-id="ee246-130">路由是在的[中介軟體](xref:fundamentals/middleware/index)管線中`Startup.Configure`註冊。</span><span class="sxs-lookup"><span data-stu-id="ee246-130">Routing is registered in the [middleware](xref:fundamentals/middleware/index) pipeline in `Startup.Configure`.</span></span>

<span data-ttu-id="ee246-131">下列程式碼顯示路由的基本範例：</span><span class="sxs-lookup"><span data-stu-id="ee246-131">The following code shows a basic example of routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

<span data-ttu-id="ee246-132">路由會使用一對由<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>註冊的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ee246-132">Routing uses a pair of middleware, registered by <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>:</span></span>

* <span data-ttu-id="ee246-133">`UseRouting`將路由對應新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-133">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="ee246-134">此中介軟體會查看應用程式中定義的端點集合，並根據要求選取[最符合的項](#urlm)。</span><span class="sxs-lookup"><span data-stu-id="ee246-134">This middleware looks at the set of endpoints defined in the app, and selects the [best match](#urlm) based on the request.</span></span>
* <span data-ttu-id="ee246-135">`UseEndpoints`將端點執行新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-135">`UseEndpoints` adds endpoint execution to the middleware pipeline.</span></span> <span data-ttu-id="ee246-136">它會執行與所選端點相關聯的委派。</span><span class="sxs-lookup"><span data-stu-id="ee246-136">It runs the delegate associated with the selected endpoint.</span></span>

<span data-ttu-id="ee246-137">上述範例包含使用[MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*)方法*對程式碼*端點的單一路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-137">The preceding example includes a single *route to code* endpoint using the [MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*) method:</span></span>

* <span data-ttu-id="ee246-138">當 HTTP `GET`要求傳送至根 URL `/`時：</span><span class="sxs-lookup"><span data-stu-id="ee246-138">When an HTTP `GET` request is sent to the root URL `/`:</span></span>
  * <span data-ttu-id="ee246-139">所顯示的要求委派會執行。</span><span class="sxs-lookup"><span data-stu-id="ee246-139">The request delegate shown executes.</span></span>
  * <span data-ttu-id="ee246-140">`Hello World!`會寫入 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="ee246-140">`Hello World!` is written to the HTTP response.</span></span> <span data-ttu-id="ee246-141">根據預設，根 URL `/`是。 `https://localhost:5001/`</span><span class="sxs-lookup"><span data-stu-id="ee246-141">By default, the root URL `/` is `https://localhost:5001/`.</span></span>
* <span data-ttu-id="ee246-142">如果要求方法不`GET`是`/`，或根 URL 不是，則不會符合任何路由，且會傳回 HTTP 404。</span><span class="sxs-lookup"><span data-stu-id="ee246-142">If the request method is not `GET` or the root URL is not `/`, no route matches and an HTTP 404 is returned.</span></span>

### <a name="endpoint"></a><span data-ttu-id="ee246-143">端點</span><span class="sxs-lookup"><span data-stu-id="ee246-143">Endpoint</span></span>

<a name="endpoint"></a>

<span data-ttu-id="ee246-144">`MapGet`方法是用來定義**端點**。</span><span class="sxs-lookup"><span data-stu-id="ee246-144">The `MapGet` method is used to define an **endpoint**.</span></span> <span data-ttu-id="ee246-145">端點可以是：</span><span class="sxs-lookup"><span data-stu-id="ee246-145">An endpoint is something that can be:</span></span>

* <span data-ttu-id="ee246-146">已選取，藉由比對 URL 和 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-146">Selected, by matching the URL and HTTP method.</span></span>
* <span data-ttu-id="ee246-147">執行委派。</span><span class="sxs-lookup"><span data-stu-id="ee246-147">Executed, by running the delegate.</span></span>

<span data-ttu-id="ee246-148">應用程式可比對並執行的端點會在中`UseEndpoints`設定。</span><span class="sxs-lookup"><span data-stu-id="ee246-148">Endpoints that can be matched and executed by the app are configured in `UseEndpoints`.</span></span> <span data-ttu-id="ee246-149">例如， <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*> <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>、和類似的[方法](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions)會將要求委派連接至路由系統。</span><span class="sxs-lookup"><span data-stu-id="ee246-149">For example, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>, and [similar methods](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions) connect request delegates to the routing system.</span></span>
<span data-ttu-id="ee246-150">您可以使用其他方法，將 ASP.NET Core framework 功能連接到路由系統：</span><span class="sxs-lookup"><span data-stu-id="ee246-150">Additional methods can be used to connect ASP.NET Core framework features to the routing system:</span></span>
- [<span data-ttu-id="ee246-151">Razor Pages 的 MapRazorPages</span><span class="sxs-lookup"><span data-stu-id="ee246-151">MapRazorPages for Razor Pages</span></span>](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)
- [<span data-ttu-id="ee246-152">適用于控制器的 MapControllers</span><span class="sxs-lookup"><span data-stu-id="ee246-152">MapControllers for controllers</span></span>](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- [<span data-ttu-id="ee246-153">SignalR\<的 MapHub THub></span><span class="sxs-lookup"><span data-stu-id="ee246-153">MapHub\<THub> for SignalR</span></span>](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*) 
- [<span data-ttu-id="ee246-154">GRPC\<的 MapGrpcService TService></span><span class="sxs-lookup"><span data-stu-id="ee246-154">MapGrpcService\<TService> for gRPC</span></span>](xref:grpc/aspnetcore)

<span data-ttu-id="ee246-155">下列範例顯示具有更複雜路由範本的路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-155">The following example shows routing with a more sophisticated route template:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

<span data-ttu-id="ee246-156">此字串`/hello/{name:alpha}`為**路由範本**。</span><span class="sxs-lookup"><span data-stu-id="ee246-156">The string `/hello/{name:alpha}` is a **route template**.</span></span> <span data-ttu-id="ee246-157">它是用來設定端點的相符方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-157">It is used to configure how the endpoint is matched.</span></span> <span data-ttu-id="ee246-158">在此情況下，範本會符合：</span><span class="sxs-lookup"><span data-stu-id="ee246-158">In this case, the template matches:</span></span>

* <span data-ttu-id="ee246-159">URL，例如`/hello/Ryan`</span><span class="sxs-lookup"><span data-stu-id="ee246-159">A URL like `/hello/Ryan`</span></span>
* <span data-ttu-id="ee246-160">開頭為`/hello/`的任何 URL 路徑，後面接著一連串的字母字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-160">Any URL path that begins with `/hello/` followed by a sequence of alphabetic characters.</span></span>  <span data-ttu-id="ee246-161">`:alpha`套用僅符合字母字元的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-161">`:alpha` applies a route constraint that matches only alphabetic characters.</span></span> <span data-ttu-id="ee246-162">本檔稍後會說明[路由條件約束](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-162">[Route constraints](#route-constraint-reference) are explained later in this document.</span></span>

<span data-ttu-id="ee246-163">URL 路徑的第二個區段`{name:alpha}`：</span><span class="sxs-lookup"><span data-stu-id="ee246-163">The second segment of the URL path, `{name:alpha}`:</span></span>

* <span data-ttu-id="ee246-164">已系結至`name`參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-164">Is bound to the `name` parameter.</span></span>
* <span data-ttu-id="ee246-165">會被捕捉並儲存在[HttpRequest 中。 RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)。</span><span class="sxs-lookup"><span data-stu-id="ee246-165">Is captured and stored in [HttpRequest.RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*).</span></span>

<span data-ttu-id="ee246-166">本檔中所述的端點路由系統是 ASP.NET Core 3.0 的新功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-166">The endpoint routing system described in this document is new as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="ee246-167">不過，ASP.NET Core 的所有版本都支援一組相同的路由範本功能和路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-167">However, all versions of ASP.NET Core support the same set of route template features and route constraints.</span></span>

<span data-ttu-id="ee246-168">下列範例顯示具有健康情況[檢查](xref:host-and-deploy/health-checks)和授權的路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-168">The following example shows routing with [health checks](xref:host-and-deploy/health-checks) and authorization:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="ee246-169">上述範例示範如何：</span><span class="sxs-lookup"><span data-stu-id="ee246-169">The preceding example demonstrates how:</span></span>

* <span data-ttu-id="ee246-170">授權中介軟體可以與路由搭配使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-170">The authorization middleware can be used with routing.</span></span>
* <span data-ttu-id="ee246-171">端點可以用來設定授權行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-171">Endpoints can be used to configure authorization behavior.</span></span>

<span data-ttu-id="ee246-172"><xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*>呼叫會新增健康情況檢查端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-172">The <xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*> call adds a health check endpoint.</span></span> <span data-ttu-id="ee246-173">此<xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>呼叫的連結會將授權原則附加至端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-173">Chaining <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> on to this call attaches an authorization policy to the endpoint.</span></span>

<span data-ttu-id="ee246-174">呼叫<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>並<xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*>新增驗證和授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-174">Calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> adds the authentication and authorization middleware.</span></span> <span data-ttu-id="ee246-175">這些中介軟體會放<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>在`UseEndpoints`和之間，使其可以：</span><span class="sxs-lookup"><span data-stu-id="ee246-175">These middleware are placed between <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and `UseEndpoints` so that they can:</span></span>

* <span data-ttu-id="ee246-176">查看所選取的端點`UseRouting`。</span><span class="sxs-lookup"><span data-stu-id="ee246-176">See which endpoint was selected by `UseRouting`.</span></span>
* <span data-ttu-id="ee246-177">將分派給端點之前<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ，請先套用授權原則。</span><span class="sxs-lookup"><span data-stu-id="ee246-177">Apply an authorization policy before <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> dispatches to the endpoint.</span></span>

<a name="metadata"></a>

### <a name="endpoint-metadata"></a><span data-ttu-id="ee246-178">端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="ee246-178">Endpoint metadata</span></span>

<span data-ttu-id="ee246-179">在上述範例中，有兩個端點，但只有健全狀況檢查端點已附加授權原則。</span><span class="sxs-lookup"><span data-stu-id="ee246-179">In the preceding example, there are two endpoints, but only the health check endpoint has an authorization policy attached.</span></span> <span data-ttu-id="ee246-180">如果要求符合健康狀態檢查端點， `/healthz`則會執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="ee246-180">If the request matches the health check endpoint, `/healthz`, an authorization check is performed.</span></span> <span data-ttu-id="ee246-181">這會示範端點可以附加額外的資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-181">This demonstrates that endpoints can have extra data attached to them.</span></span> <span data-ttu-id="ee246-182">這種額外的資料稱為端點**中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="ee246-182">This extra data is called endpoint **metadata**:</span></span>

* <span data-ttu-id="ee246-183">可由路由感知中介軟體處理中繼資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-183">The metadata can be processed by routing-aware middleware.</span></span>
* <span data-ttu-id="ee246-184">中繼資料可以是任何 .NET 型別。</span><span class="sxs-lookup"><span data-stu-id="ee246-184">The metadata can be of any .NET type.</span></span>

## <a name="routing-concepts"></a><span data-ttu-id="ee246-185">路由概念</span><span class="sxs-lookup"><span data-stu-id="ee246-185">Routing concepts</span></span>

<span data-ttu-id="ee246-186">路由系統會藉由新增強大的**端點**概念，在中介軟體管線之上建立。</span><span class="sxs-lookup"><span data-stu-id="ee246-186">The routing system builds on top of the middleware pipeline by adding the powerful **endpoint** concept.</span></span> <span data-ttu-id="ee246-187">端點代表應用程式功能的單位，在路由、授權及任何數目的 ASP.NET Core 系統之間彼此不同。</span><span class="sxs-lookup"><span data-stu-id="ee246-187">Endpoints represent units of the app's functionality that are distinct from each other in terms of routing, authorization, and any number of ASP.NET Core's systems.</span></span>

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a><span data-ttu-id="ee246-188">ASP.NET Core 端點定義</span><span class="sxs-lookup"><span data-stu-id="ee246-188">ASP.NET Core endpoint definition</span></span>

<span data-ttu-id="ee246-189">ASP.NET Core 的端點為：</span><span class="sxs-lookup"><span data-stu-id="ee246-189">An ASP.NET Core endpoint is:</span></span>

* <span data-ttu-id="ee246-190">可執行檔： <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>具有。</span><span class="sxs-lookup"><span data-stu-id="ee246-190">Executable: Has a <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>.</span></span>
* <span data-ttu-id="ee246-191">可擴充：具有[中繼資料](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*)集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-191">Extensible: Has a [Metadata](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*) collection.</span></span>
* <span data-ttu-id="ee246-192">可選取：選擇性地擁有[路由資訊](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)。</span><span class="sxs-lookup"><span data-stu-id="ee246-192">Selectable: Optionally, has [routing information](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*).</span></span>
* <span data-ttu-id="ee246-193">可列舉：藉由<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>從[DI](xref:fundamentals/dependency-injection)抓取來列出端點的集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-193">Enumerable: The collection of endpoints can be listed by retrieving the <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> from [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ee246-194">下列程式碼示範如何抓取和檢查符合目前要求的端點：</span><span class="sxs-lookup"><span data-stu-id="ee246-194">The following code shows how to retrieve and inspect the endpoint matching the current request:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

<span data-ttu-id="ee246-195">若已選取，則可以從抓取端點`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="ee246-195">The endpoint, if selected, can be retrieved from the `HttpContext`.</span></span> <span data-ttu-id="ee246-196">可以檢查其屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-196">Its properties can be inspected.</span></span> <span data-ttu-id="ee246-197">端點物件是不可變的，而且無法在建立之後修改。</span><span class="sxs-lookup"><span data-stu-id="ee246-197">Endpoint objects are immutable and cannot be modified after creation.</span></span> <span data-ttu-id="ee246-198">最常見的端點類型是<xref:Microsoft.AspNetCore.Routing.RouteEndpoint>。</span><span class="sxs-lookup"><span data-stu-id="ee246-198">The most common type of endpoint is a <xref:Microsoft.AspNetCore.Routing.RouteEndpoint>.</span></span> <span data-ttu-id="ee246-199">`RouteEndpoint`包含可讓路由系統選取的資訊。</span><span class="sxs-lookup"><span data-stu-id="ee246-199">`RouteEndpoint` includes information that allows it to be to selected by the routing system.</span></span>

<span data-ttu-id="ee246-200">在上述程式碼中， [app。使用](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*)會設定內嵌[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="ee246-200">In the preceding code, [app.Use](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*) configures an in-line [middleware](xref:fundamentals/middleware/index).</span></span>

<a name="mt"></a>

<span data-ttu-id="ee246-201">下列程式碼顯示，視管線中呼叫`app.Use`的位置而定，可能不會有端點：</span><span class="sxs-lookup"><span data-stu-id="ee246-201">The following code shows that, depending on where `app.Use` is called in the pipeline, there may not be an endpoint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

<span data-ttu-id="ee246-202">先前的範例會`Console.WriteLine`加入語句，以顯示是否已選取端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-202">This preceding sample adds `Console.WriteLine` statements that display whether or not an endpoint has been selected.</span></span> <span data-ttu-id="ee246-203">為了清楚起見，此範例會將顯示名稱指派給`/`提供的端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-203">For clarity, the sample assigns a display name to the provided `/` endpoint.</span></span>

<span data-ttu-id="ee246-204">以`/`顯示的 URL 執行此程式碼：</span><span class="sxs-lookup"><span data-stu-id="ee246-204">Running this code with a URL of `/` displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

<span data-ttu-id="ee246-205">執行此程式碼時，會顯示任何其他 URL：</span><span class="sxs-lookup"><span data-stu-id="ee246-205">Running this code with any other URL displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

<span data-ttu-id="ee246-206">此輸出示範：</span><span class="sxs-lookup"><span data-stu-id="ee246-206">This output demonstrates that:</span></span>

* <span data-ttu-id="ee246-207">在呼叫之前`UseRouting` ，端點一律為 null。</span><span class="sxs-lookup"><span data-stu-id="ee246-207">The endpoint is always null before `UseRouting` is called.</span></span>
* <span data-ttu-id="ee246-208">如果找到相符的，則與`UseRouting` <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之間的端點為非 null。</span><span class="sxs-lookup"><span data-stu-id="ee246-208">If a match is found, the endpoint is non-null between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="ee246-209">當`UseEndpoints`找到相符的時，中介軟體就是**終端**機。</span><span class="sxs-lookup"><span data-stu-id="ee246-209">The `UseEndpoints` middleware is **terminal** when a match is found.</span></span> <span data-ttu-id="ee246-210">[終端機中介軟體](#tm)會在本檔稍後定義。</span><span class="sxs-lookup"><span data-stu-id="ee246-210">[Terminal middleware](#tm) is defined later in this document.</span></span>
* <span data-ttu-id="ee246-211">只有當找`UseEndpoints`不到相符的時，才會在執行後中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-211">The middleware after `UseEndpoints` execute only when no match is found.</span></span>

<span data-ttu-id="ee246-212">`UseRouting`中介軟體會使用[task.setendpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*)方法，將端點附加至目前的內容。</span><span class="sxs-lookup"><span data-stu-id="ee246-212">The `UseRouting` middleware uses the [SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*) method to attach the endpoint to the current context.</span></span> <span data-ttu-id="ee246-213">您可以使用自訂邏輯`UseRouting`來取代中介軟體，但仍可獲得使用端點的優點。</span><span class="sxs-lookup"><span data-stu-id="ee246-213">It's possible to replace the `UseRouting` middleware with custom logic and still get the benefits of using endpoints.</span></span> <span data-ttu-id="ee246-214">端點是低層級的基本型別，例如中介軟體，而且不會與路由執行結合。</span><span class="sxs-lookup"><span data-stu-id="ee246-214">Endpoints are a low-level primitive like middleware, and aren't coupled to the routing implementation.</span></span> <span data-ttu-id="ee246-215">大部分的應用程式不需要`UseRouting`以自訂邏輯取代。</span><span class="sxs-lookup"><span data-stu-id="ee246-215">Most apps don't need to replace `UseRouting` with custom logic.</span></span>

<span data-ttu-id="ee246-216">`UseEndpoints`中介軟體是設計用來與`UseRouting`中介軟體一起使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-216">The `UseEndpoints` middleware is designed to be used in tandem with the `UseRouting` middleware.</span></span> <span data-ttu-id="ee246-217">執行端點的核心邏輯並不複雜。</span><span class="sxs-lookup"><span data-stu-id="ee246-217">The core logic to execute an endpoint isn't complicated.</span></span> <span data-ttu-id="ee246-218">使用<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>來取出端點，然後叫用其<xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-218">Use <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> to retrieve the endpoint, and then invoke its <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> property.</span></span>

<span data-ttu-id="ee246-219">下列程式碼示範中介軟體會如何影響或回應路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-219">The following code demonstrates how middleware can influence or react to routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="ee246-220">上述範例示範兩個重要概念：</span><span class="sxs-lookup"><span data-stu-id="ee246-220">The preceding example demonstrates two important concepts:</span></span>

* <span data-ttu-id="ee246-221">中介軟體可以在`UseRouting`前執行，以修改路由運作的資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-221">Middleware can run before `UseRouting` to modify the data that routing operates upon.</span></span>
    * <span data-ttu-id="ee246-222">通常在路由之前出現的中介軟體會修改要求的某些屬性， <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>例如<xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>、或<xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-222">Usually middleware that appears before routing modifies some property of the request, such as <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>, <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>, or <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>.</span></span>
* <span data-ttu-id="ee246-223">中介軟體可在`UseRouting`和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>之間執行，以便在執行端點之前處理路由的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-223">Middleware can run between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> to process the results of routing before the endpoint is executed.</span></span>
    * <span data-ttu-id="ee246-224">在和`UseRouting` `UseEndpoints`之間執行的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ee246-224">Middleware that runs between `UseRouting` and `UseEndpoints`:</span></span>
      * <span data-ttu-id="ee246-225">通常會檢查中繼資料以瞭解端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-225">Usually inspects metadata to understand the endpoints.</span></span>
      * <span data-ttu-id="ee246-226">通常會依照`UseAuthorization`和`UseCors`完成安全性決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-226">Often makes security decisions, as done by `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="ee246-227">中介軟體和中繼資料的組合，可讓您設定每個端點的原則。</span><span class="sxs-lookup"><span data-stu-id="ee246-227">The combination of middleware and metadata allows configuring policies per-endpoint.</span></span>

<span data-ttu-id="ee246-228">上述程式碼顯示支援每個端點原則的自訂中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="ee246-228">The preceding code shows an example of a custom middleware that supports per-endpoint policies.</span></span> <span data-ttu-id="ee246-229">中介軟體會將敏感性資料之存取權的*審核記錄*寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="ee246-229">The middleware writes an *audit log* of access to sensitive data to the console.</span></span> <span data-ttu-id="ee246-230">中介軟體可以設定為使用*audit* `AuditPolicyAttribute`中繼資料來審核端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-230">The middleware can be configured to *audit* an endpoint with the `AuditPolicyAttribute` metadata.</span></span> <span data-ttu-id="ee246-231">這個範例會示範*加入宣告*模式，其中只會審核標示為機密的端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-231">This sample demonstrates an *opt-in* pattern where only endpoints that are marked as sensitive are audited.</span></span> <span data-ttu-id="ee246-232">例如，您可以反向定義此邏輯，以審核未標示為安全的所有專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-232">It's possible to define this logic in reverse, auditing everything that isn't marked as safe, for example.</span></span> <span data-ttu-id="ee246-233">端點中繼資料系統很有彈性。</span><span class="sxs-lookup"><span data-stu-id="ee246-233">The endpoint metadata system is flexible.</span></span> <span data-ttu-id="ee246-234">這個邏輯可以設計成符合使用案例的任何方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-234">This logic could be designed in whatever way suits the use case.</span></span>

<span data-ttu-id="ee246-235">先前的範例程式碼主要是用來示範端點的基本概念。</span><span class="sxs-lookup"><span data-stu-id="ee246-235">The preceding sample code is intended to demonstrate the basic concepts of endpoints.</span></span> <span data-ttu-id="ee246-236">**此範例並非供生產環境使用**。</span><span class="sxs-lookup"><span data-stu-id="ee246-236">**The sample is not intended for production use**.</span></span> <span data-ttu-id="ee246-237">更完整的*audit 記錄*檔中介軟體版本如下：</span><span class="sxs-lookup"><span data-stu-id="ee246-237">A more complete version of an *audit log* middleware would:</span></span>

* <span data-ttu-id="ee246-238">記錄到檔案或資料庫。</span><span class="sxs-lookup"><span data-stu-id="ee246-238">Log to a file or database.</span></span>
* <span data-ttu-id="ee246-239">包含詳細資料，例如使用者、IP 位址、機密端點的名稱等等。</span><span class="sxs-lookup"><span data-stu-id="ee246-239">Include details such as the user, IP address, name of the sensitive endpoint, and more.</span></span>

<span data-ttu-id="ee246-240">稽核原則中繼資料`AuditPolicyAttribute`會定義為`Attribute` ，以便更輕鬆地搭配以類別為基礎的架構（例如控制器和 SignalR）使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-240">The audit policy metadata `AuditPolicyAttribute` is defined as an `Attribute` for easier use with class-based frameworks such as controllers and SignalR.</span></span> <span data-ttu-id="ee246-241">使用*路由至程式碼*時：</span><span class="sxs-lookup"><span data-stu-id="ee246-241">When using *route to code*:</span></span>

* <span data-ttu-id="ee246-242">中繼資料是使用產生器 API 所附加。</span><span class="sxs-lookup"><span data-stu-id="ee246-242">Metadata is attached with a builder API.</span></span>
* <span data-ttu-id="ee246-243">以類別為基礎的架構在建立端點時，會在對應的方法和類別上包含所有屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-243">Class-based frameworks include all attributes on the corresponding method and class when creating endpoints.</span></span>

<span data-ttu-id="ee246-244">元資料類型的最佳作法是將它們定義為介面或屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-244">The best practices for metadata types are to define them either as interfaces or attributes.</span></span> <span data-ttu-id="ee246-245">介面和屬性允許程式碼重複使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-245">Interfaces and attributes allow code reuse.</span></span> <span data-ttu-id="ee246-246">中繼資料系統具有彈性，而且不會強加任何限制。</span><span class="sxs-lookup"><span data-stu-id="ee246-246">The metadata system is flexible and doesn't impose any limitations.</span></span>

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a><span data-ttu-id="ee246-247">比較終端中介軟體和路由</span><span class="sxs-lookup"><span data-stu-id="ee246-247">Comparing a terminal middleware and routing</span></span>

<span data-ttu-id="ee246-248">下列程式碼範例會將中介軟體與使用路由進行對比：</span><span class="sxs-lookup"><span data-stu-id="ee246-248">The following code sample contrasts using middleware with using routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="ee246-249">以`Approach 1:`顯示的中介軟體樣式是**終端中介軟體**。</span><span class="sxs-lookup"><span data-stu-id="ee246-249">The style of middleware shown with `Approach 1:` is **terminal middleware**.</span></span> <span data-ttu-id="ee246-250">這稱為「終端中介軟體」，因為它會執行比對作業：</span><span class="sxs-lookup"><span data-stu-id="ee246-250">It's called terminal middleware because it does a matching operation:</span></span>

* <span data-ttu-id="ee246-251">前述範例中的比`Path == "/"`對作業適用于中介軟體和`Path == "/Movie"`路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-251">The matching operation in the preceding sample is `Path == "/"` for the middleware and `Path == "/Movie"` for routing.</span></span>
* <span data-ttu-id="ee246-252">當相符項成功時，它會執行一些功能並傳回，而不是`next`叫用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-252">When a match is successful, it executes some functionality and returns, rather than invoking the `next` middleware.</span></span>

<span data-ttu-id="ee246-253">這稱為「終端中介軟體」，因為它會終止搜尋、執行一些功能，然後傳回。</span><span class="sxs-lookup"><span data-stu-id="ee246-253">It's called terminal middleware because it terminates the search, executes some functionality, and then returns.</span></span>

<span data-ttu-id="ee246-254">比較終端機中介軟體和路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-254">Comparing a terminal middleware and routing:</span></span>
* <span data-ttu-id="ee246-255">這兩種方法都允許終止處理管線：</span><span class="sxs-lookup"><span data-stu-id="ee246-255">Both approaches allow terminating the processing pipeline:</span></span>
    * <span data-ttu-id="ee246-256">中介軟體會藉由傳回而不是`next`叫用來終止管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-256">Middleware terminates the pipeline by returning rather than invoking `next`.</span></span>
    * <span data-ttu-id="ee246-257">端點一律是 [終端機]。</span><span class="sxs-lookup"><span data-stu-id="ee246-257">Endpoints are always terminal.</span></span>
* <span data-ttu-id="ee246-258">終端中介軟體允許將中介軟體放在管線中的任意位置：</span><span class="sxs-lookup"><span data-stu-id="ee246-258">Terminal middleware allows positioning the middleware at an arbitrary place in the pipeline:</span></span>
    * <span data-ttu-id="ee246-259">端點會在的位置執行<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-259">Endpoints execute at the position of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="ee246-260">終端中介軟體可讓任意程式碼判斷中介軟體何時符合：</span><span class="sxs-lookup"><span data-stu-id="ee246-260">Terminal middleware allows arbitrary code to determine when the middleware matches:</span></span>
    * <span data-ttu-id="ee246-261">自訂路由比對程式碼可能很詳細，而且很容易正確寫入。</span><span class="sxs-lookup"><span data-stu-id="ee246-261">Custom route matching code can be verbose and difficult to write correctly.</span></span>
    * <span data-ttu-id="ee246-262">路由為一般應用程式提供直接的解決方案。</span><span class="sxs-lookup"><span data-stu-id="ee246-262">Routing provides straightforward solutions for typical apps.</span></span> <span data-ttu-id="ee246-263">大部分的應用程式都不需要自訂路由比對程式碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-263">Most apps don't require custom route matching code.</span></span>
* <span data-ttu-id="ee246-264">具有中介軟體的端點介面`UseAuthorization` ， `UseCors`例如和。</span><span class="sxs-lookup"><span data-stu-id="ee246-264">Endpoints interface with middleware such as `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="ee246-265">搭配或`UseCors`使用終端中間`UseAuthorization`件時，必須手動與授權系統互動。</span><span class="sxs-lookup"><span data-stu-id="ee246-265">Using a terminal middleware with `UseAuthorization` or `UseCors` requires manual interfacing with the authorization system.</span></span>

<span data-ttu-id="ee246-266">[端點](#endpoint)會定義兩者：</span><span class="sxs-lookup"><span data-stu-id="ee246-266">An [endpoint](#endpoint) defines both:</span></span>

* <span data-ttu-id="ee246-267">用來處理要求的委派。</span><span class="sxs-lookup"><span data-stu-id="ee246-267">A delegate to process requests.</span></span>
* <span data-ttu-id="ee246-268">任意中繼資料的集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-268">A collection of arbitrary metadata.</span></span> <span data-ttu-id="ee246-269">中繼資料是用來根據附加至每個端點的原則和設定來執行跨領域考慮。</span><span class="sxs-lookup"><span data-stu-id="ee246-269">The metadata is used to implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="ee246-270">終端中介軟體可以是有效的工具，但可能需要：</span><span class="sxs-lookup"><span data-stu-id="ee246-270">Terminal middleware can be an effective tool, but can require:</span></span>

* <span data-ttu-id="ee246-271">大量的編碼和測試。</span><span class="sxs-lookup"><span data-stu-id="ee246-271">A significant amount of coding and testing.</span></span>
* <span data-ttu-id="ee246-272">與其他系統進行手動整合，以達到所需的彈性層級。</span><span class="sxs-lookup"><span data-stu-id="ee246-272">Manual integration with other systems to achieve the desired level of flexibility.</span></span>

<span data-ttu-id="ee246-273">撰寫終端中介軟體之前，請考慮與路由整合。</span><span class="sxs-lookup"><span data-stu-id="ee246-273">Consider integrating with routing before writing a terminal middleware.</span></span>

<span data-ttu-id="ee246-274">與[Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)或<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>整合的現有終端機中介軟體，通常可以轉換成路由感知端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-274">Existing terminal middleware that integrates with [Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) or <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> can usually be turned into a routing aware endpoint.</span></span> <span data-ttu-id="ee246-275">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16)示範路由器的模式：</span><span class="sxs-lookup"><span data-stu-id="ee246-275">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16) demonstrates the pattern for router-ware:</span></span>
* <span data-ttu-id="ee246-276">在上<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>撰寫擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-276">Write an extension method on <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span>
* <span data-ttu-id="ee246-277">使用<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>建立嵌套中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-277">Create a nested middleware pipeline using <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>.</span></span>
* <span data-ttu-id="ee246-278">將中介軟體附加至新管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-278">Attach the middleware to the new pipeline.</span></span> <span data-ttu-id="ee246-279">在此案例中為 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-279">In this case, <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>.</span></span>
* <span data-ttu-id="ee246-280"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>中介軟體管線至<xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="ee246-280"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*> the middleware pipeline into a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="ee246-281">呼叫`Map`並提供新的中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-281">Call `Map` and provide the new middleware pipeline.</span></span>
* <span data-ttu-id="ee246-282">`Map`從擴充方法傳回提供的產生器物件。</span><span class="sxs-lookup"><span data-stu-id="ee246-282">Return the builder object provided by `Map` from the extension method.</span></span>

<span data-ttu-id="ee246-283">下列程式碼示範如何使用[MapHealthChecks](xref:host-and-deploy/health-checks)：</span><span class="sxs-lookup"><span data-stu-id="ee246-283">The following code shows use of [MapHealthChecks](xref:host-and-deploy/health-checks):</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

<span data-ttu-id="ee246-284">上述範例顯示為何傳回產生器物件是很重要的。</span><span class="sxs-lookup"><span data-stu-id="ee246-284">The preceding sample shows why returning the builder object is important.</span></span> <span data-ttu-id="ee246-285">傳回 builder 物件可讓應用程式開發人員設定原則，例如端點的授權。</span><span class="sxs-lookup"><span data-stu-id="ee246-285">Returning the builder object allows the app developer to configure policies such as authorization for the endpoint.</span></span> <span data-ttu-id="ee246-286">在此範例中，健康狀態檢查中介軟體沒有與授權系統的直接整合。</span><span class="sxs-lookup"><span data-stu-id="ee246-286">In this example, the health checks middleware has no direct integration with the authorization system.</span></span>

<span data-ttu-id="ee246-287">已建立中繼資料系統，以回應擴充性作者使用終端機中介軟體所遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="ee246-287">The metadata system was created in response to the problems encountered by extensibility authors using terminal middleware.</span></span> <span data-ttu-id="ee246-288">每個中介軟體都有問題，使其與授權系統整合在一起。</span><span class="sxs-lookup"><span data-stu-id="ee246-288">It's problematic for each middleware to implement its own integration with the authorization system.</span></span>

<a name="urlm"></a>

### <a name="url-matching"></a><span data-ttu-id="ee246-289">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ee246-289">URL matching</span></span>

* <span data-ttu-id="ee246-290">這是路由符合連入要求至[端點](#endpoint)的進程。</span><span class="sxs-lookup"><span data-stu-id="ee246-290">Is the process by which routing matches an incoming request to an [endpoint](#endpoint).</span></span>
* <span data-ttu-id="ee246-291">是以 URL 路徑和標頭中的資料為基礎。</span><span class="sxs-lookup"><span data-stu-id="ee246-291">Is based on data in the URL path and headers.</span></span>
* <span data-ttu-id="ee246-292">可以擴充以考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-292">Can be extended to consider any data in the request.</span></span>

<span data-ttu-id="ee246-293">當路由中介軟體執行時，它會`Endpoint`從目前的要求將和路由值設定<xref:Microsoft.AspNetCore.Http.HttpContext>為上的[要求功能](xref:fundamentals/request-features)：</span><span class="sxs-lookup"><span data-stu-id="ee246-293">When a routing middleware executes, it sets an `Endpoint` and route values to a [request feature](xref:fundamentals/request-features) on the <xref:Microsoft.AspNetCore.Http.HttpContext> from the current request:</span></span>

* <span data-ttu-id="ee246-294">呼叫[HttpCoNtext GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>)會取得端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-294">Calling [HttpContext.GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>) gets the endpoint.</span></span>
* <span data-ttu-id="ee246-295">`HttpRequest.RouteValues` 會取得路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-295">`HttpRequest.RouteValues` gets the collection of route values.</span></span>

<span data-ttu-id="ee246-296">在路由中介軟體之後執行的[中介軟體](xref:fundamentals/middleware/index)可以檢查端點並採取動作。</span><span class="sxs-lookup"><span data-stu-id="ee246-296">[Middleware](xref:fundamentals/middleware/index) running after the routing middleware can inspect the endpoint and take action.</span></span> <span data-ttu-id="ee246-297">例如，授權中介軟體可以針對授權原則詢問端點的元資料集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-297">For example, an authorization middleware can interrogate the endpoint's metadata collection for an authorization policy.</span></span> <span data-ttu-id="ee246-298">執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。</span><span class="sxs-lookup"><span data-stu-id="ee246-298">After all of the middleware in the request processing pipeline is executed, the selected endpoint's delegate is invoked.</span></span>

<span data-ttu-id="ee246-299">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-299">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="ee246-300">因為中介軟體會根據選取的端點套用原則，所以請務必：</span><span class="sxs-lookup"><span data-stu-id="ee246-300">Because the middleware applies policies based on the selected endpoint, it's important that:</span></span>

* <span data-ttu-id="ee246-301">任何可能影響分派或應用安全性原則的決策都是在路由系統內進行。</span><span class="sxs-lookup"><span data-stu-id="ee246-301">Any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

> [!WARNING]
> <span data-ttu-id="ee246-302">針對回溯相容性，當執行控制器或 Razor Pages 端點委派時，會根據到目前為止執行的要求處理，將[routecoNtext.routedata](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData)的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-302">For backwards-compatibility, when a Controller or Razor Pages endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>
>
> <span data-ttu-id="ee246-303">在`RouteContext`未來的版本中，此類型將會標示為已淘汰：</span><span class="sxs-lookup"><span data-stu-id="ee246-303">The `RouteContext` type will be marked obsolete in a future release:</span></span>
>
> * <span data-ttu-id="ee246-304">遷移`RouteData.Values`至`HttpRequest.RouteValues`。</span><span class="sxs-lookup"><span data-stu-id="ee246-304">Migrate `RouteData.Values` to `HttpRequest.RouteValues`.</span></span>
> * <span data-ttu-id="ee246-305">遷移`RouteData.DataTokens`以從端點中繼資料取得[IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) 。</span><span class="sxs-lookup"><span data-stu-id="ee246-305">Migrate `RouteData.DataTokens` to retrieve [IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) from the endpoint metadata.</span></span>

<span data-ttu-id="ee246-306">URL 比對作業會以一組可設定的階段來運作。</span><span class="sxs-lookup"><span data-stu-id="ee246-306">URL matching operates in a configurable set of phases.</span></span> <span data-ttu-id="ee246-307">在每個階段中，輸出是一組相符專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-307">In each phase, the output is a set of matches.</span></span> <span data-ttu-id="ee246-308">下一個階段可進一步縮小一組相符專案的範圍。</span><span class="sxs-lookup"><span data-stu-id="ee246-308">The set of matches can be narrowed down further by the next phase.</span></span> <span data-ttu-id="ee246-309">路由執行並不保證符合端點的處理順序。</span><span class="sxs-lookup"><span data-stu-id="ee246-309">The routing implementation does not guarantee a processing order for matching endpoints.</span></span> <span data-ttu-id="ee246-310">**所有**可能的相符專案都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-310">**All** possible matches are processed at once.</span></span> <span data-ttu-id="ee246-311">URL 符合的階段會依照下列順序發生。</span><span class="sxs-lookup"><span data-stu-id="ee246-311">The URL matching phases occur in the following order.</span></span> <span data-ttu-id="ee246-312">ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="ee246-312">ASP.NET Core:</span></span>

1. <span data-ttu-id="ee246-313">會處理一組端點及其路由範本的 URL 路徑，並收集**所有**相符專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-313">Processes the URL path against the set of endpoints and their route templates, collecting **all** of the matches.</span></span>
1. <span data-ttu-id="ee246-314">接受上述清單，並移除已套用路由條件約束而失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-314">Takes the preceding list and removes matches that fail with route constraints applied.</span></span>
1. <span data-ttu-id="ee246-315">接受上述清單並移除[MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy)實例集失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-315">Takes the preceding list and removes matches that fail the set of [MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy) instances.</span></span>
1. <span data-ttu-id="ee246-316">會使用[EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) ，從上述清單中進行最後的決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-316">Uses the [EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) to make a final decision from the preceding list.</span></span>

<span data-ttu-id="ee246-317">端點清單的優先順序會根據：</span><span class="sxs-lookup"><span data-stu-id="ee246-317">The list of endpoints is prioritized according to:</span></span>

* <span data-ttu-id="ee246-318">[RouteEndpoint。](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span><span class="sxs-lookup"><span data-stu-id="ee246-318">The [RouteEndpoint.Order](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span></span>
* <span data-ttu-id="ee246-319">[路由範本優先順序](#rtp)</span><span class="sxs-lookup"><span data-stu-id="ee246-319">The [route template precedence](#rtp)</span></span>

<span data-ttu-id="ee246-320">所有相符的<xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector>端點都會在每個階段中處理，直到到達為止。</span><span class="sxs-lookup"><span data-stu-id="ee246-320">All matching endpoints are processed in each phase until the <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> is reached.</span></span> <span data-ttu-id="ee246-321">`EndpointSelector`是最後一個階段。</span><span class="sxs-lookup"><span data-stu-id="ee246-321">The `EndpointSelector` is the final phase.</span></span> <span data-ttu-id="ee246-322">它會從相符專案中選擇最高優先順序的端點，以做為最符合專案。</span><span class="sxs-lookup"><span data-stu-id="ee246-322">It chooses the highest priority endpoint from the matches as the best match.</span></span> <span data-ttu-id="ee246-323">如果有其他符合的優先權與最符合的專案相同，則會擲回不明確的相符例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ee246-323">If there are other matches with the same priority as the best match, an ambiguous match exception is thrown.</span></span>

<span data-ttu-id="ee246-324">路由優先順序是根據指定較高優先順序的較**特定**路由範本來計算。</span><span class="sxs-lookup"><span data-stu-id="ee246-324">The route precedence is computed based on a **more specific** route template being given a higher priority.</span></span> <span data-ttu-id="ee246-325">例如，請考慮範本`/hello`和： `/{message}`</span><span class="sxs-lookup"><span data-stu-id="ee246-325">For example, consider the templates `/hello` and `/{message}`:</span></span>

* <span data-ttu-id="ee246-326">兩者都符合 URL 路徑`/hello`。</span><span class="sxs-lookup"><span data-stu-id="ee246-326">Both match the URL path `/hello`.</span></span>
* <span data-ttu-id="ee246-327">`/hello`更明確，因此優先順序較高。</span><span class="sxs-lookup"><span data-stu-id="ee246-327">`/hello`  is more specific and therefore higher priority.</span></span>

<span data-ttu-id="ee246-328">一般來說，路由優先順序會針對實務中使用的 URL 配置類型選擇最符合的工作。</span><span class="sxs-lookup"><span data-stu-id="ee246-328">In general, route precedence does a good job of choosing the best match for the kinds of URL schemes used in practice.</span></span> <span data-ttu-id="ee246-329">只有<xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order>在必要時才使用，以避免發生不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="ee246-329">Use <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order> only when necessary to avoid an ambiguity.</span></span>

<span data-ttu-id="ee246-330">由於路由所提供的擴充性種類，路由系統無法在不明確的路由時間之前計算。</span><span class="sxs-lookup"><span data-stu-id="ee246-330">Due to the kinds of extensibility provided by routing, it isn't possible for the routing system to compute ahead of time the ambiguous routes.</span></span> <span data-ttu-id="ee246-331">假設有一個範例，例如路由範本`/{message:alpha}`和`/{message:int}`：</span><span class="sxs-lookup"><span data-stu-id="ee246-331">Consider an example such as the route templates `/{message:alpha}` and `/{message:int}`:</span></span>

* <span data-ttu-id="ee246-332">`alpha`條件約束只符合字母字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-332">The `alpha` constraint matches only alphabetic characters.</span></span>
* <span data-ttu-id="ee246-333">`int`條件約束只符合數位。</span><span class="sxs-lookup"><span data-stu-id="ee246-333">The `int` constraint matches only numbers.</span></span>
* <span data-ttu-id="ee246-334">這些範本具有相同的路由優先順序，但沒有相符的單一 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-334">These templates have the same route precedence, but there's no single URL they both match.</span></span>
* <span data-ttu-id="ee246-335">如果路由系統在啟動時報告了不明確的錯誤，則會封鎖此有效的使用案例。</span><span class="sxs-lookup"><span data-stu-id="ee246-335">If the routing system reported an ambiguity error at startup, it would block this valid use case.</span></span>

> [!WARNING]
>
> <span data-ttu-id="ee246-336">內部<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>作業的順序不會影響路由的行為，但有一個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ee246-336">The order of operations inside <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> doesn't influence the behavior of routing, with one exception.</span></span> <span data-ttu-id="ee246-337"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>會根據叫用的順序，自動指派訂單值給其端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-337"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="ee246-338">這會模擬控制器的長時間行為，而不會有路由系統提供與舊版路由執行相同的保證。</span><span class="sxs-lookup"><span data-stu-id="ee246-338">This simulates long-time behavior of controllers without the routing system providing the same guarantees as older routing implementations.</span></span>
>
> <span data-ttu-id="ee246-339">在路由的舊版執行中，可以根據路由的處理順序來執行路由擴充性。</span><span class="sxs-lookup"><span data-stu-id="ee246-339">In the legacy implementation of routing, it's possible to implement routing extensibility that has a dependency on the order in which routes are processed.</span></span> <span data-ttu-id="ee246-340">ASP.NET Core 3.0 和更新版本中的端點路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-340">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>
> 
> * <span data-ttu-id="ee246-341">沒有路由的概念。</span><span class="sxs-lookup"><span data-stu-id="ee246-341">Doesn't have a concept of routes.</span></span>
> * <span data-ttu-id="ee246-342">不提供排序保證。</span><span class="sxs-lookup"><span data-stu-id="ee246-342">Doesn't provide ordering guarantees.</span></span> <span data-ttu-id="ee246-343">所有端點都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-343">All endpoints are processed at once.</span></span>
>
> <span data-ttu-id="ee246-344">如果這表示您在使用舊版路由系統時停滯，請[開啟 GitHub 問題以取得協助](https://github.com/dotnet/aspnetcore/issues)。</span><span class="sxs-lookup"><span data-stu-id="ee246-344">If this means you're stuck using the legacy routing system, [open a GitHub issue for assistance](https://github.com/dotnet/aspnetcore/issues).</span></span>

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a><span data-ttu-id="ee246-345">路由範本優先順序和端點選取順序</span><span class="sxs-lookup"><span data-stu-id="ee246-345">Route template precedence and endpoint selection order</span></span>

<span data-ttu-id="ee246-346">[路由範本優先順序](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16)是一種系統，它會根據特定的方式，為每個路由範本指派一個值。</span><span class="sxs-lookup"><span data-stu-id="ee246-346">[Route template precedence](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16) is a system that assigns each route template a value based on how specific it is.</span></span> <span data-ttu-id="ee246-347">路由範本優先順序：</span><span class="sxs-lookup"><span data-stu-id="ee246-347">Route template precedence:</span></span>

* <span data-ttu-id="ee246-348">避免在一般情況下調整端點順序的需求。</span><span class="sxs-lookup"><span data-stu-id="ee246-348">Avoids the need to adjust the order of endpoints in common cases.</span></span>
* <span data-ttu-id="ee246-349">嘗試符合常見的路由行為預期。</span><span class="sxs-lookup"><span data-stu-id="ee246-349">Attempts to match the common-sense expectations of routing behavior.</span></span>

<span data-ttu-id="ee246-350">例如，請考慮範本`/Products/List`和`/Products/{id}`。</span><span class="sxs-lookup"><span data-stu-id="ee246-350">For example, consider templates `/Products/List` and `/Products/{id}`.</span></span> <span data-ttu-id="ee246-351">假設`/Products/List`比起 URL 路徑`/Products/{id}` `/Products/List`的比對更相符，這是合理的做法。</span><span class="sxs-lookup"><span data-stu-id="ee246-351">It would be reasonable to assume that `/Products/List` is a better match than `/Products/{id}` for the URL path `/Products/List`.</span></span> <span data-ttu-id="ee246-352">的作用是因為常值`/List`區段被視為具有比參數區段`/{id}`更好的優先順序。</span><span class="sxs-lookup"><span data-stu-id="ee246-352">The works because the literal segment `/List` is considered to have better precedence than the parameter segment `/{id}`.</span></span>

<span data-ttu-id="ee246-353">優先順序運作方式的詳細資料會結合如何定義路由範本：</span><span class="sxs-lookup"><span data-stu-id="ee246-353">The details of how precedence works are coupled to how route templates are defined:</span></span>

* <span data-ttu-id="ee246-354">具有更多區段的範本會被視為更明確。</span><span class="sxs-lookup"><span data-stu-id="ee246-354">Templates with more segments are considered more specific.</span></span>
* <span data-ttu-id="ee246-355">具有常值文字的區段會被視為比參數區段更明確。</span><span class="sxs-lookup"><span data-stu-id="ee246-355">A segment with literal text is considered more specific than a parameter segment.</span></span>
* <span data-ttu-id="ee246-356">具有條件約束的參數區段在沒有的情況下會被視為較明確。</span><span class="sxs-lookup"><span data-stu-id="ee246-356">A parameter segment with a constraint is considered more specific than one without.</span></span>
* <span data-ttu-id="ee246-357">複雜的區段會被視為具有條件約束的參數區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-357">A complex segment is considered as specific as a parameter segment with a constraint.</span></span>
* <span data-ttu-id="ee246-358">Catch-all 參數是最不明確的。</span><span class="sxs-lookup"><span data-stu-id="ee246-358">Catch-all parameters are the least specific.</span></span> <span data-ttu-id="ee246-359">如需有關 catch all 路由的重要資訊，請參閱[路由範本參考](#rtr)中的**全部攔截**。</span><span class="sxs-lookup"><span data-stu-id="ee246-359">See **catch-all** in the [Route template reference](#rtr) for important information on catch-all routes.</span></span>

<span data-ttu-id="ee246-360">如需確切值的參考，請參閱[GitHub 上的原始程式碼](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189)。</span><span class="sxs-lookup"><span data-stu-id="ee246-360">See the [source code on GitHub](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189) for a reference of exact values.</span></span>

<a name="lg"></a>

### <a name="url-generation-concepts"></a><span data-ttu-id="ee246-361">URL 產生概念</span><span class="sxs-lookup"><span data-stu-id="ee246-361">URL generation concepts</span></span>

<span data-ttu-id="ee246-362">URL 產生：</span><span class="sxs-lookup"><span data-stu-id="ee246-362">URL generation:</span></span>

* <span data-ttu-id="ee246-363">這是路由可以根據一組路由值建立 URL 路徑的進程。</span><span class="sxs-lookup"><span data-stu-id="ee246-363">Is the process by which routing can create a URL path based on a set of route values.</span></span>
* <span data-ttu-id="ee246-364">允許端點與存取它們的 Url 之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-364">Allows for a logical separation between endpoints and the URLs that access them.</span></span>

<span data-ttu-id="ee246-365">端點路由包含<xref:Microsoft.AspNetCore.Routing.LinkGenerator> API。</span><span class="sxs-lookup"><span data-stu-id="ee246-365">Endpoint routing includes the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API.</span></span> <span data-ttu-id="ee246-366">`LinkGenerator`是[DI](xref:fundamentals/dependency-injection)提供的單一服務。</span><span class="sxs-lookup"><span data-stu-id="ee246-366">`LinkGenerator` is a singleton service available from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ee246-367">`LinkGenerator` API 可以在執行中要求的內容之外使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-367">The `LinkGenerator` API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="ee246-368">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper)和依賴的案例<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>，例如標籤協助程式[、HTML](xref:mvc/views/tag-helpers/intro)協助程式和[動作結果](xref:mvc/controllers/actions)，會在內部使用`LinkGenerator` API 來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-368">[Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper) and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the `LinkGenerator` API internally to provide link generating capabilities.</span></span>

<span data-ttu-id="ee246-369">連結產生器背後支援的概念為「位址」\*\*\*\* 和「位址配置」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-369">The link generator is backed by the concept of an **address** and **address schemes**.</span></span> <span data-ttu-id="ee246-370">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-370">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="ee246-371">例如，許多使用者熟悉的路由名稱和路由值案例會將控制器和 Razor Pages 實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="ee246-371">For example, the route name and route values scenarios many users are familiar with from controllers and Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="ee246-372">連結產生器可以透過下列擴充方法連結至控制器和 Razor Pages：</span><span class="sxs-lookup"><span data-stu-id="ee246-372">The link generator can link to controllers and Razor Pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="ee246-373">這些方法的多載會接受包含的`HttpContext`引數。</span><span class="sxs-lookup"><span data-stu-id="ee246-373">Overloads of these methods accept arguments that include the `HttpContext`.</span></span> <span data-ttu-id="ee246-374">這些方法在功能上相當於[url. 動作](xref:System.Web.Mvc.UrlHelper.Action*)和[url](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)，但提供額外的彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="ee246-374">These methods are functionally equivalent to [Url.Action](xref:System.Web.Mvc.UrlHelper.Action*) and [Url.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*), but offer additional flexibility and options.</span></span>

<span data-ttu-id="ee246-375">這些`GetPath*`方法與`Url.Action`和`Url.Page`最相似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-375">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page`, in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="ee246-376">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-376">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="ee246-377">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-377">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="ee246-378">除非覆寫，否則會使用執行中要求的[環境](#ambient)路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="ee246-378">The [ambient](#ambient) route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="ee246-379">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="ee246-379"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="ee246-380">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="ee246-380">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="ee246-381">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="ee246-381">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="ee246-382">評估每個端點的 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="ee246-382">Each endpoint's <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern> is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="ee246-383">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="ee246-383">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="ee246-384"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-384">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="ee246-385">使用連結產生器最方便的方式，就是透過擴充方法來執行特定網址類別型的作業：</span><span class="sxs-lookup"><span data-stu-id="ee246-385">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type:</span></span>

| <span data-ttu-id="ee246-386">擴充方法</span><span class="sxs-lookup"><span data-stu-id="ee246-386">Extension Method</span></span> | <span data-ttu-id="ee246-387">說明</span><span class="sxs-lookup"><span data-stu-id="ee246-387">Description</span></span> |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="ee246-388">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-388">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="ee246-389">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-389">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="ee246-390">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="ee246-390">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="ee246-391">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ee246-391">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="ee246-392">如果未`Host`驗證傳入要求的標頭，則不受信任的要求輸入可以在視圖或頁面的 uri 中傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee246-392">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view or page.</span></span> <span data-ttu-id="ee246-393">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-393">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="ee246-394">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-394">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="ee246-395">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-395">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="ee246-396">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-396">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="ee246-397">指定空的基底路徑，以`Map*`復原連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="ee246-397">Specify an empty base path to undo the `Map*` affect on link generation.</span></span>

### <a name="middleware-example"></a><span data-ttu-id="ee246-398">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="ee246-398">Middleware example</span></span>

<span data-ttu-id="ee246-399">在下列範例中，中介軟體會使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 來建立列出商店產品之動作方法的連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-399">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create a link to an action method that lists store products.</span></span> <span data-ttu-id="ee246-400">藉由將連結產生器插入類別並呼叫`GenerateLink` ，可供應用程式中的任何類別使用：</span><span class="sxs-lookup"><span data-stu-id="ee246-400">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a><span data-ttu-id="ee246-401">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ee246-401">Route template reference</span></span>

<span data-ttu-id="ee246-402">內`{}`的 token 會定義路由符合時所系結的路由參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-402">Tokens within `{}` define route parameters that are bound if the route is matched.</span></span> <span data-ttu-id="ee246-403">在路由區段中可以定義一個以上的路由參數，但路由參數必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-403">More than one route parameter can be defined in a route segment, but route parameters  must be separated by a literal value.</span></span> <span data-ttu-id="ee246-404">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ee246-404">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span>  <span data-ttu-id="ee246-405">路由參數必須有名稱，而且可能會指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-405">Route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ee246-406">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ee246-406">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ee246-407">文字比對不區分大小寫，而且會根據 URL 路徑的解碼標記法。</span><span class="sxs-lookup"><span data-stu-id="ee246-407">Text matching is case-insensitive and based on the decoded representation of the URL's path.</span></span> <span data-ttu-id="ee246-408">若要比對常值路由`{`參數`}`分隔符號或，請重複字元來將分隔符號轉義。</span><span class="sxs-lookup"><span data-stu-id="ee246-408">To match a literal route parameter delimiter `{` or `}`, escape the delimiter by repeating the character.</span></span> <span data-ttu-id="ee246-409">例如， `{{`或`}}`。</span><span class="sxs-lookup"><span data-stu-id="ee246-409">For example `{{` or `}}`.</span></span>

<span data-ttu-id="ee246-410">星號`*`或雙星號`**`：</span><span class="sxs-lookup"><span data-stu-id="ee246-410">Asterisk `*` or double asterisk `**`:</span></span>

* <span data-ttu-id="ee246-411">可以做為路由參數的前置詞，以系結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ee246-411">Can be used as a prefix to a route parameter to bind to the rest of the URI.</span></span>
* <span data-ttu-id="ee246-412">稱為「**全部攔截**」參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-412">Are called a **catch-all** parameters.</span></span> <span data-ttu-id="ee246-413">例如，`blog/{**slug}`：</span><span class="sxs-lookup"><span data-stu-id="ee246-413">For example, `blog/{**slug}`:</span></span>
  * <span data-ttu-id="ee246-414">會比對開頭為的`/blog`任何 URI，並在其後面加上任何值。</span><span class="sxs-lookup"><span data-stu-id="ee246-414">Matches any URI that starts with `/blog` and has any value following it.</span></span>
  * <span data-ttu-id="ee246-415">下列`/blog`值會指派給 [[資訊](https://developer.mozilla.org/docs/Glossary/Slug)區路線] 路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-415">The value following `/blog` is assigned to the [slug](https://developer.mozilla.org/docs/Glossary/Slug) route value.</span></span>

[!INCLUDE[](~/includes/catchall.md)]

<span data-ttu-id="ee246-416">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-416">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ee246-417">當使用路由來產生 URL （包括路徑分隔符號`/` ）時，catch-all 參數會將適當的字元進行轉義。</span><span class="sxs-lookup"><span data-stu-id="ee246-417">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator `/` characters.</span></span> <span data-ttu-id="ee246-418">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ee246-418">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ee246-419">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ee246-419">Note the escaped forward slash.</span></span> <span data-ttu-id="ee246-420">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="ee246-420">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="ee246-421">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="ee246-421">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="ee246-422">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ee246-422">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ee246-423">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ee246-423">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ee246-424">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ee246-424">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ee246-425">如果 URL 中只有`filename`存在的值，則路由會符合，因為尾端`.`是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="ee246-425">If only a value for `filename` exists in the URL, the route matches because the trailing `.` is  optional.</span></span> <span data-ttu-id="ee246-426">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-426">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ee246-427">路由參數可能有「預設值」\*\*\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-427">Route parameters may have **default values** designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ee246-428">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-428">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ee246-429">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-429">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ee246-430">將問號（`?`）附加至參數名稱的結尾，可將路由參數設為選擇性。</span><span class="sxs-lookup"><span data-stu-id="ee246-430">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name.</span></span> <span data-ttu-id="ee246-431">例如： `id?` 。</span><span class="sxs-lookup"><span data-stu-id="ee246-431">For example, `id?`.</span></span> <span data-ttu-id="ee246-432">選擇性值與預設路由參數之間的差異如下：</span><span class="sxs-lookup"><span data-stu-id="ee246-432">The difference between optional values and default route parameters is:</span></span>

* <span data-ttu-id="ee246-433">具有預設值的路由參數一律會產生值。</span><span class="sxs-lookup"><span data-stu-id="ee246-433">A route parameter with a default value always produces a value.</span></span>
* <span data-ttu-id="ee246-434">選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ee246-434">An optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ee246-435">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-435">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ee246-436">在`:`路由參數名稱之後加入和條件約束名稱，會在路由參數上指定內嵌條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-436">Adding `:` and constraint name after the route parameter name specifies an inline constraint on a route parameter.</span></span> <span data-ttu-id="ee246-437">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 `(...)` 括住。</span><span class="sxs-lookup"><span data-stu-id="ee246-437">If the constraint requires arguments, they're enclosed in parentheses `(...)` after the constraint name.</span></span> <span data-ttu-id="ee246-438">藉由附加另一個`:`和條件約束名稱，可以指定多個*內嵌條件約束*。</span><span class="sxs-lookup"><span data-stu-id="ee246-438">Multiple *inline constraints* can be specified by appending another `:` and constraint name.</span></span>

<span data-ttu-id="ee246-439">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-439">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ee246-440">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-440">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ee246-441">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-441">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ee246-442">路由參數也可以具有參數轉換器。</span><span class="sxs-lookup"><span data-stu-id="ee246-442">Route parameters may also have parameter transformers.</span></span> <span data-ttu-id="ee246-443">參數轉換器會在產生連結並比對動作和頁面到 Url 時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-443">Parameter transformers transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="ee246-444">如同條件約束，參數轉換器可以藉由在路由參數名稱之後新增`:`和轉換器名稱，以內嵌方式加入至路由參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-444">Like constraints, parameter transformers can be added inline to a route parameter by adding a `:` and transformer name after the route parameter name.</span></span> <span data-ttu-id="ee246-445">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="ee246-445">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="ee246-446">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-446">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="ee246-447">下表示范範例路由範本及其行為：</span><span class="sxs-lookup"><span data-stu-id="ee246-447">The following table demonstrates example route templates and their behavior:</span></span>

| <span data-ttu-id="ee246-448">路由範本</span><span class="sxs-lookup"><span data-stu-id="ee246-448">Route Template</span></span>                           | <span data-ttu-id="ee246-449">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ee246-449">Example Matching URI</span></span>    | <span data-ttu-id="ee246-450">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ee246-450">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ee246-451">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ee246-451">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ee246-452">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ee246-452">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ee246-453">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ee246-453">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ee246-454">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ee246-454">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ee246-455">對應至`Products`控制器和`Details`動作，`id`並將設定為123。</span><span class="sxs-lookup"><span data-stu-id="ee246-455">Maps to the `Products` controller and  `Details` action with`id` set to 123.</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ee246-456">對應至`Home`控制器和`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-456">Maps to the `Home` controller and `Index` method.</span></span> <span data-ttu-id="ee246-457">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="ee246-457">`id` is ignored.</span></span>        |
| `{controller=Home}/{action=Index}/{id?}` | `/Products`         | <span data-ttu-id="ee246-458">對應至`Products`控制器和`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-458">Maps to the `Products` controller and `Index` method.</span></span> <span data-ttu-id="ee246-459">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="ee246-459">`id` is ignored.</span></span>        |

<span data-ttu-id="ee246-460">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-460">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ee246-461">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ee246-461">Constraints and defaults can also be specified outside the route template.</span></span>

### <a name="complex-segments"></a><span data-ttu-id="ee246-462">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ee246-462">Complex segments</span></span>

<span data-ttu-id="ee246-463">複雜區段的處理方式是以[非貪婪](#greedy)的方式，將常值分隔符號從右至左比對。</span><span class="sxs-lookup"><span data-stu-id="ee246-463">Complex segments are processed by matching up literal delimiters from right to left in a [non-greedy](#greedy) way.</span></span> <span data-ttu-id="ee246-464">例如， `[Route("/a{b}c{d}")]`是一個複雜的區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-464">For example, `[Route("/a{b}c{d}")]` is a complex segment.</span></span>
<span data-ttu-id="ee246-465">複雜的區段會以特定的方式工作，必須瞭解才能成功使用它們。</span><span class="sxs-lookup"><span data-stu-id="ee246-465">Complex segments work in a particular way that must be understood to use them successfully.</span></span> <span data-ttu-id="ee246-466">本章節中的範例將示範當分隔符號文字不會出現在參數值內時，複雜區段的效果為何。</span><span class="sxs-lookup"><span data-stu-id="ee246-466">The example in this section demonstrates why complex segments only really work well when the delimiter text doesn't appear inside the parameter values.</span></span> <span data-ttu-id="ee246-467">在較複雜的情況下，需要使用[RegEx](/dotnet/standard/base-types/regular-expressions) ，然後手動將值解壓縮。</span><span class="sxs-lookup"><span data-stu-id="ee246-467">Using a [regex](/dotnet/standard/base-types/regular-expressions) and then manually extracting the values is needed for more complex cases.</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ee246-468">這是路由執行的步驟和範本`/a{b}c{d}`和 URL 路徑`/abcd`的摘要。</span><span class="sxs-lookup"><span data-stu-id="ee246-468">This is a summary of the steps that routing performs with the template `/a{b}c{d}` and the URL path `/abcd`.</span></span> <span data-ttu-id="ee246-469">`|`是用來協助視覺化演算法的運作方式：</span><span class="sxs-lookup"><span data-stu-id="ee246-469">The `|` is used to help visualize how the algorithm works:</span></span>

* <span data-ttu-id="ee246-470">第一個常值（由右至左`c`）為。</span><span class="sxs-lookup"><span data-stu-id="ee246-470">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="ee246-471">因此`/abcd` ，會向右搜尋並`/ab|c|d`尋找。</span><span class="sxs-lookup"><span data-stu-id="ee246-471">So `/abcd` is searched from right and finds `/ab|c|d`.</span></span>
* <span data-ttu-id="ee246-472">右邊的所有專案（`d`）現在都符合路由參數`{d}`。</span><span class="sxs-lookup"><span data-stu-id="ee246-472">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="ee246-473">下一個常值（由右至左`a`）為。</span><span class="sxs-lookup"><span data-stu-id="ee246-473">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="ee246-474">因此`/ab|c|d` ，在從離開的地方開始搜尋， `a`然後找到`/|a|b|c|d`。</span><span class="sxs-lookup"><span data-stu-id="ee246-474">So `/ab|c|d` is searched starting where we left off, then `a` is found `/|a|b|c|d`.</span></span>
* <span data-ttu-id="ee246-475">右邊的值（`b`）現在符合路由參數。 `{b}`</span><span class="sxs-lookup"><span data-stu-id="ee246-475">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="ee246-476">沒有任何剩餘的文字，也沒有剩餘的路由範本，因此這是相符的。</span><span class="sxs-lookup"><span data-stu-id="ee246-476">There is no remaining text and no remaining route template, so this is a match.</span></span>

<span data-ttu-id="ee246-477">以下是使用相同範本`/a{b}c{d}`和 URL 路徑`/aabcd`的負面案例範例。</span><span class="sxs-lookup"><span data-stu-id="ee246-477">Here's an example of a negative case using the same template `/a{b}c{d}` and the URL path `/aabcd`.</span></span> <span data-ttu-id="ee246-478">`|`是用來協助視覺化演算法的運作方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-478">The `|` is used to help visualize how the algorithm works.</span></span> <span data-ttu-id="ee246-479">這種情況並不相符，這會透過相同的演算法來說明：</span><span class="sxs-lookup"><span data-stu-id="ee246-479">This case isn't a match, which is explained by the same algorithm:</span></span>
* <span data-ttu-id="ee246-480">第一個常值（由右至左`c`）為。</span><span class="sxs-lookup"><span data-stu-id="ee246-480">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="ee246-481">因此`/aabcd` ，會向右搜尋並`/aab|c|d`尋找。</span><span class="sxs-lookup"><span data-stu-id="ee246-481">So `/aabcd` is searched from right and finds `/aab|c|d`.</span></span>
* <span data-ttu-id="ee246-482">右邊的所有專案（`d`）現在都符合路由參數`{d}`。</span><span class="sxs-lookup"><span data-stu-id="ee246-482">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="ee246-483">下一個常值（由右至左`a`）為。</span><span class="sxs-lookup"><span data-stu-id="ee246-483">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="ee246-484">因此`/aab|c|d` ，在從離開的地方開始搜尋， `a`然後找到`/a|a|b|c|d`。</span><span class="sxs-lookup"><span data-stu-id="ee246-484">So `/aab|c|d` is searched starting where we left off, then `a` is found `/a|a|b|c|d`.</span></span>
* <span data-ttu-id="ee246-485">右邊的值（`b`）現在符合路由參數。 `{b}`</span><span class="sxs-lookup"><span data-stu-id="ee246-485">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="ee246-486">此時會有剩餘的文字`a`，但演算法已用完路由範本進行剖析，因此這不是相符的。</span><span class="sxs-lookup"><span data-stu-id="ee246-486">At this point there is remaining text `a`, but the algorithm has run out of route template to parse, so this is not a match.</span></span>

<span data-ttu-id="ee246-487">因為比對演算法[是非貪婪](#greedy)的：</span><span class="sxs-lookup"><span data-stu-id="ee246-487">Since the matching algorithm is [non-greedy](#greedy):</span></span>

* <span data-ttu-id="ee246-488">它會比對每個步驟中可能的最小文字數量。</span><span class="sxs-lookup"><span data-stu-id="ee246-488">It matches the smallest amount of text possible in each step.</span></span>
* <span data-ttu-id="ee246-489">在參數值內出現分隔符號值的任何情況下，都會導致不相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-489">Any case where the delimiter value appears inside the parameter values results in not matching.</span></span>

<span data-ttu-id="ee246-490">正則運算式可讓您更充分掌控其比對行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-490">Regular expressions provide much more control over their matching behavior.</span></span>

<a name="greedy"></a>

<span data-ttu-id="ee246-491">貪婪比對（也稱為[延遲](https://wikipedia.org/wiki/Regular_expression#Lazy_matching)比對）符合最大的可能字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-491">Greedy matching, also know as [lazy matching](https://wikipedia.org/wiki/Regular_expression#Lazy_matching), matches the largest possible string.</span></span> <span data-ttu-id="ee246-492">非貪婪的會符合最小的可能字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-492">Non-greedy matches the smallest possible string.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="ee246-493">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ee246-493">Route constraint reference</span></span>

<span data-ttu-id="ee246-494">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ee246-494">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ee246-495">路由條件約束通常會檢查透過路由範本相關聯的路由值，並對值是否可接受進行 true 或 false 的決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-495">Route constraints generally inspect the route value associated via the route template and make a true or false decision about whether the value is acceptable.</span></span> <span data-ttu-id="ee246-496">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-496">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ee246-497">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-497">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ee246-498">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ee246-498">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ee246-499">請勿針對輸入驗證使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-499">Don't use constraints for input validation.</span></span> <span data-ttu-id="ee246-500">如果條件約束用於輸入驗證，則不正確輸入會導致`404`找不到的回應。</span><span class="sxs-lookup"><span data-stu-id="ee246-500">If constraints are used for input validation, invalid input results in a `404` Not Found response.</span></span> <span data-ttu-id="ee246-501">不正確輸入應該會`400`產生不正確的要求，並出現適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ee246-501">Invalid input should produce a `400` Bad Request with an appropriate error message.</span></span> <span data-ttu-id="ee246-502">路由條件約束會用來釐清類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ee246-502">Route constraints are used to disambiguate similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ee246-503">下表示范範例路由條件約束及其預期行為：</span><span class="sxs-lookup"><span data-stu-id="ee246-503">The following table demonstrates example route constraints and their expected behavior:</span></span>

| <span data-ttu-id="ee246-504">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="ee246-504">constraint</span></span> | <span data-ttu-id="ee246-505">範例</span><span class="sxs-lookup"><span data-stu-id="ee246-505">Example</span></span> | <span data-ttu-id="ee246-506">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-506">Example Matches</span></span> | <span data-ttu-id="ee246-507">備忘錄</span><span class="sxs-lookup"><span data-stu-id="ee246-507">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ee246-508">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-508">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-509">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="ee246-509">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ee246-510">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ee246-510">`true`, `FALSE`</span></span> | <span data-ttu-id="ee246-511">符合`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="ee246-511">Matches `true` or `false`.</span></span> <span data-ttu-id="ee246-512">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ee246-512">Case-insensitive</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ee246-513">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ee246-513">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ee246-514">符合不因`DateTime`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-514">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ee246-515">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-515">See preceding warning.</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="ee246-516">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ee246-516">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ee246-517">符合不因`decimal`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-517">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ee246-518">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-518">See preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ee246-519">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-519">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-520">符合不因`double`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-520">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ee246-521">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-521">See preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ee246-522">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-522">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-523">符合不因`float`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-523">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ee246-524">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-524">See preceding warning.</span></span>|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | <span data-ttu-id="ee246-525">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="ee246-525">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ee246-526">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-526">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-527">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="ee246-527">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ee246-528">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-528">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="ee246-529">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-529">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ee246-530">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-530">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ee246-531">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-531">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ee246-532">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="ee246-532">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ee246-533">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ee246-533">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ee246-534">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ee246-534">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ee246-535">字串必須包含一或多個字母字元， `a` - `z`並不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-535">String must consist of one or more alphabetical characters, `a`-`z` and case-insensitive.</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ee246-536">字串必須符合正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-536">String must match the regular expression.</span></span> <span data-ttu-id="ee246-537">請參閱定義正則運算式的秘訣。</span><span class="sxs-lookup"><span data-stu-id="ee246-537">See tips about defining a regular expression.</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ee246-538">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="ee246-538">Used to enforce that a non-parameter value is present during URL generation</span></span> |

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ee246-539">多個以冒號分隔的條件約束可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-539">Multiple, colon delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ee246-540">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ee246-540">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ee246-541">驗證 URL 並轉換成 CLR 類型的路由條件約束，一律會使用不因文化特性而異。</span><span class="sxs-lookup"><span data-stu-id="ee246-541">Route constraints that verify the URL and are converted to a CLR type always use the invariant culture.</span></span> <span data-ttu-id="ee246-542">例如，轉換成 CLR 型`int`別或。 `DateTime`</span><span class="sxs-lookup"><span data-stu-id="ee246-542">For example, conversion to the CLR type `int` or `DateTime`.</span></span> <span data-ttu-id="ee246-543">這些條件約束假設 URL 無法當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ee246-543">These constraints assume that the URL is not localizable.</span></span> <span data-ttu-id="ee246-544">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-544">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ee246-545">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-545">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ee246-546">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ee246-546">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

### <a name="regular-expressions-in-constraints"></a><span data-ttu-id="ee246-547">條件約束中的正則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-547">Regular expressions in constraints</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ee246-548">正則運算式可以指定為使用路由條件約束`regex(...)`的內嵌條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-548">Regular expressions can be specified as inline constraints using the `regex(...)` route constraint.</span></span> <span data-ttu-id="ee246-549"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>系列中的方法也會接受條件約束的物件常值。</span><span class="sxs-lookup"><span data-stu-id="ee246-549">Methods in the <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> family also accept an object literal of constraints.</span></span> <span data-ttu-id="ee246-550">如果使用該表單，字串值就會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-550">If that form is used, string values are interpreted as regular expressions.</span></span>

<span data-ttu-id="ee246-551">下列程式碼會使用內嵌 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="ee246-551">The following code uses an inline regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

<span data-ttu-id="ee246-552">下列程式碼會使用物件常值來指定 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="ee246-552">The following code uses an object literal to specify a regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

<span data-ttu-id="ee246-553">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ee246-553">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ee246-554">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-554">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ee246-555">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="ee246-555">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="ee246-556">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-556">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ee246-557">若要在內嵌條件`^\d{3}-\d{2}-\d{4}$`約束中使用正則運算式，請使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="ee246-557">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in an inline constraint, use one of the following:</span></span>

* <span data-ttu-id="ee246-558">將`\`字串中提供的字元取代`\\`為 c # 原始檔中的字元，以便將`\`字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="ee246-558">Replace `\` characters provided in the string as `\\` characters in the C# source file in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="ee246-559">[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值。</span><span class="sxs-lookup"><span data-stu-id="ee246-559">[Verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="ee246-560">若要轉義路由參數分隔符號`{`、 `}`、 `[`、 `]`，請將運算式中的`{{`字元加倍， `}}`例如、、 `[[`、。 `]]`</span><span class="sxs-lookup"><span data-stu-id="ee246-560">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression, for example, `{{`, `}}`, `[[`, `]]`.</span></span> <span data-ttu-id="ee246-561">下表顯示正則運算式和其轉義版本：</span><span class="sxs-lookup"><span data-stu-id="ee246-561">The following table shows a regular expression and its escaped version:</span></span>

| <span data-ttu-id="ee246-562">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-562">Regular expression</span></span>    | <span data-ttu-id="ee246-563">已轉義的正則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-563">Escaped regular expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ee246-564">路由中使用的正則運算式通常以`^`字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ee246-564">Regular expressions used in routing often start with the `^` character and match the starting position of the string.</span></span> <span data-ttu-id="ee246-565">運算式通常以`$`字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ee246-565">The expressions often end with the `$` character and match the end of the string.</span></span> <span data-ttu-id="ee246-566">`^`和`$`字元可確保正則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ee246-566">The `^` and `$` characters ensure that the regular expression matches the entire route parameter value.</span></span> <span data-ttu-id="ee246-567">如果沒有`^`和`$`字元，正則運算式會比對字串內的任何子字串，這通常是不需要的。</span><span class="sxs-lookup"><span data-stu-id="ee246-567">Without the `^` and `$` characters, the regular expression matches any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ee246-568">下表提供範例，並說明它們符合或無法符合的原因：</span><span class="sxs-lookup"><span data-stu-id="ee246-568">The following table provides examples and explains why they match or fail to match:</span></span>

| <span data-ttu-id="ee246-569">運算是</span><span class="sxs-lookup"><span data-stu-id="ee246-569">Expression</span></span>   | <span data-ttu-id="ee246-570">字串</span><span class="sxs-lookup"><span data-stu-id="ee246-570">String</span></span>    | <span data-ttu-id="ee246-571">比對</span><span class="sxs-lookup"><span data-stu-id="ee246-571">Match</span></span> | <span data-ttu-id="ee246-572">註解</span><span class="sxs-lookup"><span data-stu-id="ee246-572">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ee246-573">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-573">hello</span></span>     | <span data-ttu-id="ee246-574">是</span><span class="sxs-lookup"><span data-stu-id="ee246-574">Yes</span></span>   | <span data-ttu-id="ee246-575">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-575">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-576">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-576">123abc456</span></span> | <span data-ttu-id="ee246-577">是</span><span class="sxs-lookup"><span data-stu-id="ee246-577">Yes</span></span>   | <span data-ttu-id="ee246-578">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-578">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-579">mz</span><span class="sxs-lookup"><span data-stu-id="ee246-579">mz</span></span>        | <span data-ttu-id="ee246-580">是</span><span class="sxs-lookup"><span data-stu-id="ee246-580">Yes</span></span>   | <span data-ttu-id="ee246-581">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-581">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ee246-582">MZ</span><span class="sxs-lookup"><span data-stu-id="ee246-582">MZ</span></span>        | <span data-ttu-id="ee246-583">是</span><span class="sxs-lookup"><span data-stu-id="ee246-583">Yes</span></span>   | <span data-ttu-id="ee246-584">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ee246-584">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-585">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-585">hello</span></span>     | <span data-ttu-id="ee246-586">否</span><span class="sxs-lookup"><span data-stu-id="ee246-586">No</span></span>    | <span data-ttu-id="ee246-587">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-587">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-588">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-588">123abc456</span></span> | <span data-ttu-id="ee246-589">否</span><span class="sxs-lookup"><span data-stu-id="ee246-589">No</span></span>    | <span data-ttu-id="ee246-590">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-590">See `^` and `$` above</span></span> |

<span data-ttu-id="ee246-591">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-591">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ee246-592">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-592">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ee246-593">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-593">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ee246-594">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-594">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ee246-595">在條件約束字典中傳遞並不符合其中一個已知條件約束的條件約束，也會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-595">Constraints that are passed in the constraints dictionary that don't match one of the known constraints are also treated as regular expressions.</span></span> <span data-ttu-id="ee246-596">在不符合其中一個已知條件約束的範本內傳遞的條件約束，不會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-596">Constraints that are passed  within a template that don't match one of the known constraints are not treated as regular expressions.</span></span>

### <a name="custom-route-constraints"></a><span data-ttu-id="ee246-597">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="ee246-597">Custom route constraints</span></span>

<span data-ttu-id="ee246-598">您可以藉由執行<xref:Microsoft.AspNetCore.Routing.IRouteConstraint>介面來建立自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-598">Custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ee246-599">`IRouteConstraint`介面<xref:System.Web.Routing.IRouteConstraint.Match*>包含， `true`如果條件約束已滿足，則會傳回`false` ，否則會傳回。</span><span class="sxs-lookup"><span data-stu-id="ee246-599">The `IRouteConstraint` interface contains <xref:System.Web.Routing.IRouteConstraint.Match*>, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ee246-600">很少需要自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-600">Custom route constraints are rarely needed.</span></span> <span data-ttu-id="ee246-601">在執行自訂路由條件約束之前，請考慮使用替代專案，例如模型系結。</span><span class="sxs-lookup"><span data-stu-id="ee246-601">Before implementing a custom route constraint, consider alternatives, such as model binding.</span></span>

<span data-ttu-id="ee246-602">若要使用自`IRouteConstraint`定義，則必須向服務容器<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>中的應用程式註冊路由條件約束類型。</span><span class="sxs-lookup"><span data-stu-id="ee246-602">To use a custom `IRouteConstraint`, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the service container.</span></span> <span data-ttu-id="ee246-603">`ConstraintMap` 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 `IRouteConstraint` 實作。</span><span class="sxs-lookup"><span data-stu-id="ee246-603">A `ConstraintMap` is a dictionary that maps route constraint keys to `IRouteConstraint` implementations that validate those constraints.</span></span> <span data-ttu-id="ee246-604">更新應用程式的 `ConstraintMap` 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ee246-604">An app's `ConstraintMap` can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ee246-605">例如：</span><span class="sxs-lookup"><span data-stu-id="ee246-605">For example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

<span data-ttu-id="ee246-606">上述條件約束會套用至下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ee246-606">The preceding constraint is applied in the following code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

<span data-ttu-id="ee246-607">的執行`MyCustomConstraint`會防止`0`套用至路由參數：</span><span class="sxs-lookup"><span data-stu-id="ee246-607">The implementation of `MyCustomConstraint` prevents `0` being applied to a route parameter:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ee246-608">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="ee246-608">The preceding code:</span></span>

* <span data-ttu-id="ee246-609">防止`0`在路由`{id}`的區段中。</span><span class="sxs-lookup"><span data-stu-id="ee246-609">Prevents `0` in the `{id}` segment of the route.</span></span>
* <span data-ttu-id="ee246-610">會顯示以提供執行自訂條件約束的基本範例。</span><span class="sxs-lookup"><span data-stu-id="ee246-610">Is shown to provide a basic example of implementing a custom constraint.</span></span> <span data-ttu-id="ee246-611">不應在生產應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-611">It should not be used in a production app.</span></span>

<span data-ttu-id="ee246-612">下列程式碼是避免處理`id`包含`0`之的較佳方法：</span><span class="sxs-lookup"><span data-stu-id="ee246-612">The following code is a better approach to preventing an `id` containing a `0` from being processed:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="ee246-613">上述程式碼在此`MyCustomConstraint`方法上具有下列優點：</span><span class="sxs-lookup"><span data-stu-id="ee246-613">The preceding code has the following advantages over the `MyCustomConstraint` approach:</span></span>

* <span data-ttu-id="ee246-614">不需要自訂條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-614">It doesn't require a custom constraint.</span></span>
* <span data-ttu-id="ee246-615">當路由參數包含`0`時，它會傳回更具描述性的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ee246-615">It returns a more descriptive error when the route parameter includes `0`.</span></span>

## <a name="parameter-transformer-reference"></a><span data-ttu-id="ee246-616">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="ee246-616">Parameter transformer reference</span></span>

<span data-ttu-id="ee246-617">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="ee246-617">Parameter transformers:</span></span>

* <span data-ttu-id="ee246-618">使用<xref:Microsoft.AspNetCore.Routing.LinkGenerator>產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="ee246-618">Execute when generating a link using <xref:Microsoft.AspNetCore.Routing.LinkGenerator>.</span></span>
* <span data-ttu-id="ee246-619">實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="ee246-619">Implement <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>.</span></span>
* <span data-ttu-id="ee246-620">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="ee246-620">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="ee246-621">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="ee246-621">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="ee246-622">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-622">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="ee246-623">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="ee246-623">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="ee246-624">請考慮下列`IOutboundParameterTransformer`執行：</span><span class="sxs-lookup"><span data-stu-id="ee246-624">Consider the following `IOutboundParameterTransformer` implementation:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

<span data-ttu-id="ee246-625">若要在路由模式中使用參數轉換器，請使用<xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>中的`Startup.ConfigureServices`來設定它：</span><span class="sxs-lookup"><span data-stu-id="ee246-625">To use a parameter transformer in a route pattern, configure it using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

<span data-ttu-id="ee246-626">ASP.NET Core framework 會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-626">The ASP.NET Core framework uses parameter transformers to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="ee246-627">例如，參數轉換器會轉換用來比`area`對、 `controller`、 `action`和`page`的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-627">For example, parameter transformers transform the route values used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="ee246-628">使用上述路由範本，動作`SubscriptionManagementController.GetAll`會與 URI `/subscription-management/get-all`相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-628">With the preceding route template, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="ee246-629">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-629">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="ee246-630">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="ee246-630">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="ee246-631">ASP.NET Core 提供使用參數轉換器搭配產生的路由的 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="ee246-631">ASP.NET Core provides API conventions for using parameter transformers with generated routes:</span></span>

* <span data-ttu-id="ee246-632"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC 慣例會將指定的參數轉換器套用至應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-632">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="ee246-633">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ee246-633">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="ee246-634">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="ee246-634">For more information, see [Use a parameter transformer to customize token replacement](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="ee246-635">Razor Pages 使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ee246-635">Razor Pages uses the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API convention.</span></span> <span data-ttu-id="ee246-636">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="ee246-636">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="ee246-637">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-637">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="ee246-638">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="ee246-638">For more information, see [Use a parameter transformer to customize page routes](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

<a name="ugr"></a>

## <a name="url-generation-reference"></a><span data-ttu-id="ee246-639">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ee246-639">URL generation reference</span></span>

<span data-ttu-id="ee246-640">此章節包含 URL 產生所實作為演算法的參考。</span><span class="sxs-lookup"><span data-stu-id="ee246-640">This section contains a reference for the algorithm implemented by URL generation.</span></span> <span data-ttu-id="ee246-641">實際上，最複雜的 URL 產生範例會使用控制器或 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="ee246-641">In practice, most complex examples of URL generation use controllers or Razor Pages.</span></span> <span data-ttu-id="ee246-642">如需其他資訊，請參閱[控制器中的路由](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="ee246-642">See  [routing in controllers](xref:mvc/controllers/routing) for additional information.</span></span>

<span data-ttu-id="ee246-643">URL 產生進程會從呼叫[LinkGenerator. GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*)或類似的方法開始。</span><span class="sxs-lookup"><span data-stu-id="ee246-643">The URL generation process begins with a call to [LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*) or a similar method.</span></span> <span data-ttu-id="ee246-644">提供的方法包含位址、一組路由值，以及來自`HttpContext`的目前要求的選擇性資訊。</span><span class="sxs-lookup"><span data-stu-id="ee246-644">The method is provided with an address, a set of route values, and optionally information about the current request from `HttpContext`.</span></span>

<span data-ttu-id="ee246-645">第一個步驟是使用位址來解析一組候選端點[`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) ，並使用符合網址類別型的。</span><span class="sxs-lookup"><span data-stu-id="ee246-645">The first step is to use the address to resolve a set of candidate endpoints using an [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) that matches the address's type.</span></span>

<span data-ttu-id="ee246-646">位址配置找到一組候選項目之後，就會反復排序並處理端點，直到 URL 產生作業成功為止。</span><span class="sxs-lookup"><span data-stu-id="ee246-646">Once of set of candidates is found by the address scheme, the endpoints are ordered and processed iteratively until a URL generation operation succeeds.</span></span> <span data-ttu-id="ee246-647">URL**產生不會檢查是否**有多義性，傳回的第一個結果是最終的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-647">URL generation does **not** check for ambiguities, the first result returned is the final result.</span></span>

### <a name="troubleshooting-url-generation-with-logging"></a><span data-ttu-id="ee246-648">使用記錄進行 URL 產生的疑難排解</span><span class="sxs-lookup"><span data-stu-id="ee246-648">Troubleshooting URL generation with logging</span></span>

<span data-ttu-id="ee246-649">針對 URL 產生進行疑難排解的第一個步驟是將的記錄`Microsoft.AspNetCore.Routing`層`TRACE`級設定為。</span><span class="sxs-lookup"><span data-stu-id="ee246-649">The first step in troubleshooting URL generation is setting the logging level of `Microsoft.AspNetCore.Routing` to `TRACE`.</span></span> <span data-ttu-id="ee246-650">`LinkGenerator`記錄其處理的許多詳細資料，這有助於疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="ee246-650">`LinkGenerator` logs many details about its processing which can be useful to troubleshoot problems.</span></span>

<span data-ttu-id="ee246-651">如需 URL 產生的詳細資訊，請參閱[url 產生參考](#ugr)。</span><span class="sxs-lookup"><span data-stu-id="ee246-651">See [URL generation reference](#ugr) for details on URL generation.</span></span>

### <a name="addresses"></a><span data-ttu-id="ee246-652">位址</span><span class="sxs-lookup"><span data-stu-id="ee246-652">Addresses</span></span>

<span data-ttu-id="ee246-653">位址是 URL 產生的概念，用來將連結產生器的呼叫系結至一組候選端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-653">Addresses are the concept in URL generation used to bind a call into the link generator to a set of candidate endpoints.</span></span>

<span data-ttu-id="ee246-654">位址是一種可延伸的概念，預設會隨附兩個執行：</span><span class="sxs-lookup"><span data-stu-id="ee246-654">Addresses are an extensible concept that come with two implementations by default:</span></span>

* <span data-ttu-id="ee246-655">使用*端點名稱*（`string`）作為位址：</span><span class="sxs-lookup"><span data-stu-id="ee246-655">Using *endpoint name* (`string`) as the address:</span></span>
    * <span data-ttu-id="ee246-656">為 MVC 的路由名稱提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-656">Provides similar functionality to MVC's route name.</span></span>
    * <span data-ttu-id="ee246-657">使用<xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata>元資料類型。</span><span class="sxs-lookup"><span data-stu-id="ee246-657">Uses the <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> metadata type.</span></span>
    * <span data-ttu-id="ee246-658">針對所有已註冊端點的中繼資料，解析提供的字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-658">Resolves the provided string against the metadata of all registered endpoints.</span></span>
    * <span data-ttu-id="ee246-659">如果多個端點使用相同的名稱，則會在啟動時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ee246-659">Throws an exception on startup if multiple endpoints use the same name.</span></span>
    * <span data-ttu-id="ee246-660">建議在控制器和 Razor Pages 外部使用一般用途。</span><span class="sxs-lookup"><span data-stu-id="ee246-660">Recommended for general-purpose use outside of controllers and Razor Pages.</span></span>
* <span data-ttu-id="ee246-661">使用*路由值*（<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>）做為位址：</span><span class="sxs-lookup"><span data-stu-id="ee246-661">Using *route values* (<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>) as the address:</span></span>
    * <span data-ttu-id="ee246-662">提供與控制器類似的功能，並 Razor Pages 舊版的 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="ee246-662">Provides similar functionality to controllers and Razor Pages legacy URL generation.</span></span>
    * <span data-ttu-id="ee246-663">擴充和調試非常複雜。</span><span class="sxs-lookup"><span data-stu-id="ee246-663">Very complex to extend and debug.</span></span>
    * <span data-ttu-id="ee246-664">提供所使用的實`IUrlHelper`作為、標記協助程式、HTML Helper、動作結果等。</span><span class="sxs-lookup"><span data-stu-id="ee246-664">Provides the implementation used by `IUrlHelper`, Tag Helpers, HTML Helpers, Action Results, etc.</span></span>

<span data-ttu-id="ee246-665">位址配置的角色是讓位址和相符端點之間的關聯依照任意準則：</span><span class="sxs-lookup"><span data-stu-id="ee246-665">The role of the address scheme is to make the association between the address and matching endpoints by arbitrary criteria:</span></span>

* <span data-ttu-id="ee246-666">端點名稱配置會執行基本的字典查閱。</span><span class="sxs-lookup"><span data-stu-id="ee246-666">The endpoint name scheme performs a basic dictionary lookup.</span></span>
* <span data-ttu-id="ee246-667">路由值配置具有集合演算法的複雜最佳子集。</span><span class="sxs-lookup"><span data-stu-id="ee246-667">The route values scheme has a complex best subset of set algorithm.</span></span>

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a><span data-ttu-id="ee246-668">環境值和明確的值</span><span class="sxs-lookup"><span data-stu-id="ee246-668">Ambient values and explicit values</span></span>

<span data-ttu-id="ee246-669">根據目前的要求，路由會存取目前要求`HttpContext.Request.RouteValues`的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-669">From the current request, routing accesses the route values of the current request `HttpContext.Request.RouteValues`.</span></span> <span data-ttu-id="ee246-670">與目前要求相關聯的值稱為「**環境值**」。</span><span class="sxs-lookup"><span data-stu-id="ee246-670">The values associated with the current request are referred to as the **ambient values**.</span></span> <span data-ttu-id="ee246-671">為了清楚起見，檔是指傳遞至方法做為**明確值**的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-671">For the purpose of clarity, the documentation refers to the route values passed in to methods as **explicit values**.</span></span>

<span data-ttu-id="ee246-672">下列範例會顯示環境值和明確的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-672">The following example shows ambient values and explicit values.</span></span> <span data-ttu-id="ee246-673">它會從目前的要求和明確的值中提供`{ id = 17, }`環境值：</span><span class="sxs-lookup"><span data-stu-id="ee246-673">It provides ambient values from the current request and explicit values: `{ id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

<span data-ttu-id="ee246-674">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="ee246-674">The preceding code:</span></span>

* <span data-ttu-id="ee246-675">傳回 `/Widget/Index/17`。</span><span class="sxs-lookup"><span data-stu-id="ee246-675">Returns `/Widget/Index/17`</span></span>
* <span data-ttu-id="ee246-676">透過<xref:Microsoft.AspNetCore.Routing.LinkGenerator> [DI](xref:fundamentals/dependency-injection)取得。</span><span class="sxs-lookup"><span data-stu-id="ee246-676">Gets <xref:Microsoft.AspNetCore.Routing.LinkGenerator> via [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ee246-677">下列程式碼不會提供環境值和明確的`{ controller = "Home", action = "Subscribe", id = 17, }`值：</span><span class="sxs-lookup"><span data-stu-id="ee246-677">The following code provides no ambient values and explicit values: `{ controller = "Home", action = "Subscribe", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

<span data-ttu-id="ee246-678">上述方法會傳回`/Home/Subscribe/17`</span><span class="sxs-lookup"><span data-stu-id="ee246-678">The preceding  method returns `/Home/Subscribe/17`</span></span>

<span data-ttu-id="ee246-679">中`WidgetController`的下列程式碼會`/Widget/Subscribe/17`傳回：</span><span class="sxs-lookup"><span data-stu-id="ee246-679">The following code in the `WidgetController` returns `/Widget/Subscribe/17`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

<span data-ttu-id="ee246-680">下列程式碼會從目前要求中的環境值和明確值提供控制器： `{ action = "Edit", id = 17, }`</span><span class="sxs-lookup"><span data-stu-id="ee246-680">The following code provides the controller from ambient values in the current request and explicit values: `{ action = "Edit", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

<span data-ttu-id="ee246-681">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="ee246-681">In the preceding code:</span></span>

* <span data-ttu-id="ee246-682">`/Gadget/Edit/17`傳回。</span><span class="sxs-lookup"><span data-stu-id="ee246-682">`/Gadget/Edit/17` is returned.</span></span>
* <span data-ttu-id="ee246-683"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url>取得<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>。</span><span class="sxs-lookup"><span data-stu-id="ee246-683"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url> gets the <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>.</span></span>
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
<span data-ttu-id="ee246-684">產生具有動作方法之絕對路徑的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-684">generates a URL with an absolute path for an action method.</span></span> <span data-ttu-id="ee246-685">URL 包含指定`action`的名稱和`route`值。</span><span class="sxs-lookup"><span data-stu-id="ee246-685">The URL contains the specified `action` name and `route` values.</span></span>

<span data-ttu-id="ee246-686">下列程式碼提供來自目前要求和明確值的環境值： `{ page = "./Edit, id = 17, }`</span><span class="sxs-lookup"><span data-stu-id="ee246-686">The following code provides ambient values from the current request and explicit values: `{ page = "./Edit, id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="ee246-687">當編輯 Razor 頁面`url`包含`/Edit/17`下列 Page 指示詞時，上述程式碼會將設為：</span><span class="sxs-lookup"><span data-stu-id="ee246-687">The preceding code sets `url` to  `/Edit/17` when the Edit Razor Page contains the following page directive:</span></span>

 `@page "{id:int}"`

<span data-ttu-id="ee246-688">如果 [編輯] 頁面不包含`"{id:int}"`路由範本， `url`則`/Edit?id=17`為。</span><span class="sxs-lookup"><span data-stu-id="ee246-688">If the Edit page doesn't contain the `"{id:int}"` route template, `url` is `/Edit?id=17`.</span></span>

<span data-ttu-id="ee246-689">除了此處所述的<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>規則之外，MVC 的行為也會增加一層複雜度：</span><span class="sxs-lookup"><span data-stu-id="ee246-689">The behavior of MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> adds a layer of complexity in addition to the rules described here:</span></span>

* <span data-ttu-id="ee246-690">`IUrlHelper`一律會提供來自目前要求的路由值做為環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-690">`IUrlHelper` always provides the route values from the current request as ambient values.</span></span>
* <span data-ttu-id="ee246-691">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*)一律會將目前`action`的和`controller`路由值複製成明確的值，除非由開發人員覆寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-691">[IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*) always copies the current `action` and `controller` route values as explicit values unless overridden by the developer.</span></span>
* <span data-ttu-id="ee246-692">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)一律會將目前`page`的路由值複製成明確的值，除非予以覆寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-692">[IUrlHelper.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*) always copies the current `page` route value as an explicit value unless overridden.</span></span> <!--by the user-->
* <span data-ttu-id="ee246-693">`IUrlHelper.Page`除非覆寫， `handler`否則一律會`null`以明確的值覆寫目前的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-693">`IUrlHelper.Page` always overrides the current `handler` route value with `null` as an explicit values unless overridden.</span></span>

<span data-ttu-id="ee246-694">使用者通常會因為環境值的行為詳細資料而感到驚訝，因為 MVC 似乎不會遵循其本身的規則。</span><span class="sxs-lookup"><span data-stu-id="ee246-694">Users are often surprised by the behavioral details of ambient values, because MVC doesn't seem to follow its own rules.</span></span> <span data-ttu-id="ee246-695">基於歷史和相容性的原因，某些路由值`action`（ `controller`例如`page`、、 `handler`和）有自己的特殊案例行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-695">For historical and compatibility reasons, certain route values such as `action`, `controller`, `page`, and `handler` have their own special-case behavior.</span></span>

<span data-ttu-id="ee246-696">所提供的對等`LinkGenerator.GetPathByAction`功能`LinkGenerator.GetPathByPage` ，會複製這些`IUrlHelper`異常的以實現相容性。</span><span class="sxs-lookup"><span data-stu-id="ee246-696">The equivalent functionality provided by `LinkGenerator.GetPathByAction` and `LinkGenerator.GetPathByPage` duplicates these anomalies of `IUrlHelper` for compatibility.</span></span>

### <a name="url-generation-process"></a><span data-ttu-id="ee246-697">URL 產生進程</span><span class="sxs-lookup"><span data-stu-id="ee246-697">URL generation process</span></span>

<span data-ttu-id="ee246-698">一旦找到候選端點集合之後，URL 產生演算法就會：</span><span class="sxs-lookup"><span data-stu-id="ee246-698">Once the set of candidate endpoints are found, the URL generation algorithm:</span></span>

* <span data-ttu-id="ee246-699">反復處理端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-699">Processes the endpoints iteratively.</span></span>
* <span data-ttu-id="ee246-700">傳回第一個成功的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-700">Returns the first successful result.</span></span>

<span data-ttu-id="ee246-701">此程式的第一個步驟是所謂的**路由值失效**。</span><span class="sxs-lookup"><span data-stu-id="ee246-701">The first step in this process is called **route value invalidation**.</span></span>  <span data-ttu-id="ee246-702">路由值失效是路由決定應該使用哪些來自環境值的路由值以及應該忽略的進程。</span><span class="sxs-lookup"><span data-stu-id="ee246-702">Route value invalidation is the process by which routing decides which route values from the ambient values should be used and which should be ignored.</span></span> <span data-ttu-id="ee246-703">會考慮每個環境值，並將其與明確值結合，或予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ee246-703">Each ambient value is considered and either combined with the explicit values, or ignored.</span></span>

<span data-ttu-id="ee246-704">考慮環境值角色的最佳方式，就是他們會嘗試儲存應用程式開發人員在某些常見的情況下輸入。</span><span class="sxs-lookup"><span data-stu-id="ee246-704">The best way to think about the role of ambient values is that they attempt to save application developers typing, in some common cases.</span></span> <span data-ttu-id="ee246-705">傳統上，環境值很有用的案例與 MVC 相關：</span><span class="sxs-lookup"><span data-stu-id="ee246-705">Traditionally, the scenarios where ambient values are helpful are related to MVC:</span></span>

* <span data-ttu-id="ee246-706">連結至相同控制器中的另一個動作時，不需要指定控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-706">When linking to another action in the same controller, the controller name doesn't need to be specified.</span></span>
* <span data-ttu-id="ee246-707">連結至相同區域中的另一個控制器時，不需要指定區功能變數名稱稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-707">When linking to another controller in the same area, the area name doesn't need to be specified.</span></span>
* <span data-ttu-id="ee246-708">連結至相同的動作方法時，不需要指定路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-708">When linking to the same action method, route values don't need to be specified.</span></span>
* <span data-ttu-id="ee246-709">當連結到應用程式的另一個部分時，您不會想要在應用程式的該部分中執行不具意義的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-709">When linking to another part of the app, you don't want to carry over route values that have no meaning in that part of the app.</span></span>

<span data-ttu-id="ee246-710">呼叫`LinkGenerator`或`IUrlHelper`傳回的通常`null`是因為不了解路由值失效所造成。</span><span class="sxs-lookup"><span data-stu-id="ee246-710">Calls to `LinkGenerator` or `IUrlHelper` that return `null` are usually caused by not understanding route value invalidation.</span></span> <span data-ttu-id="ee246-711">明確指定更多的路由值，以查看是否能解決問題，以針對路由值失效進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="ee246-711">Troubleshoot route value invalidation by explicitly specifying more of the route values to see if that solves the problem.</span></span>

<span data-ttu-id="ee246-712">路由值失效的假設是應用程式的 URL 配置是階層式的，並以從左至右形成的階層。</span><span class="sxs-lookup"><span data-stu-id="ee246-712">Route value invalidation works on the assumption that the app's URL scheme is hierarchical, with a hierarchy formed from left-to-right.</span></span> <span data-ttu-id="ee246-713">請考慮使用「基本控制器`{controller}/{action}/{id?}`路由」範本，以直覺的方式來瞭解這在實務上如何運作。</span><span class="sxs-lookup"><span data-stu-id="ee246-713">Consider the basic controller route template `{controller}/{action}/{id?}` to get an intuitive sense of how this works in practice.</span></span> <span data-ttu-id="ee246-714">值的**變更**會使顯示在右側的所有路由值**失效**。</span><span class="sxs-lookup"><span data-stu-id="ee246-714">A **change** to a value **invalidates** all of the route values that appear to the right.</span></span> <span data-ttu-id="ee246-715">這反映了關於階層的假設。</span><span class="sxs-lookup"><span data-stu-id="ee246-715">This reflects the assumption about hierarchy.</span></span> <span data-ttu-id="ee246-716">如果應用程式具有的`id`環境值，且作業為指定了不同的`controller`值：</span><span class="sxs-lookup"><span data-stu-id="ee246-716">If the app has an ambient value for `id`, and the operation specifies a different value for the `controller`:</span></span>

* <span data-ttu-id="ee246-717">`id`不會重複使用`{controller}` ，因為位於的左邊`{id?}`。</span><span class="sxs-lookup"><span data-stu-id="ee246-717">`id` won't be reused because `{controller}` is to the left of `{id?}`.</span></span>

<span data-ttu-id="ee246-718">示範此原則的一些範例如下：</span><span class="sxs-lookup"><span data-stu-id="ee246-718">Some examples demonstrating this principle:</span></span>

* <span data-ttu-id="ee246-719">如果明確的值包含的值`id`，則`id`會忽略的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-719">If the explicit values contain a value for `id`, the ambient value for `id` is ignored.</span></span> <span data-ttu-id="ee246-720">您可以使用`controller`和`action`的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-720">The ambient values for `controller` and `action` can be used.</span></span>
* <span data-ttu-id="ee246-721">如果明確的值包含的值`action`，則會忽略的`action`任何環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-721">If the explicit values contain a value for `action`, any ambient value for `action` is ignored.</span></span> <span data-ttu-id="ee246-722">`controller`可以使用的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-722">The ambient values for `controller` can be used.</span></span> <span data-ttu-id="ee246-723">如果的明確值`action`與的環境值不同`action`，則不會使用`id`此值。</span><span class="sxs-lookup"><span data-stu-id="ee246-723">If the explicit value for `action` is different from the ambient value for `action`, the `id` value won't be used.</span></span>  <span data-ttu-id="ee246-724">如果的明確值`action`與的環境值相同`action`，則`id`可以使用值。</span><span class="sxs-lookup"><span data-stu-id="ee246-724">If the explicit value for `action` is the same as the ambient value for `action`, the `id` value can be used.</span></span>
* <span data-ttu-id="ee246-725">如果明確的值包含的值`controller`，則會忽略的`controller`任何環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-725">If the explicit values contain a value for `controller`, any ambient value for `controller` is ignored.</span></span> <span data-ttu-id="ee246-726">如果的`controller`明確值與的環境值不同`controller`，則不會使用`action`和`id`值。</span><span class="sxs-lookup"><span data-stu-id="ee246-726">If the explicit value for `controller` is different from the ambient value for `controller`, the `action` and `id` values won't be used.</span></span> <span data-ttu-id="ee246-727">如果的`controller`明確值與的環境值相同`controller`，則`action`可以使用和`id`值。</span><span class="sxs-lookup"><span data-stu-id="ee246-727">If the explicit value for `controller` is the same as the ambient value for `controller`, the `action` and `id` values can be used.</span></span>

<span data-ttu-id="ee246-728">這個程式會因為屬性路由的存在和專用的慣例路由而變得更複雜。</span><span class="sxs-lookup"><span data-stu-id="ee246-728">This process is further complicated by the existence of attribute routes and dedicated conventional routes.</span></span> <span data-ttu-id="ee246-729">控制器慣例路由， `{controller}/{action}/{id?}`例如使用路由參數來指定階層。</span><span class="sxs-lookup"><span data-stu-id="ee246-729">Controller conventional routes such as `{controller}/{action}/{id?}` specify a hierarchy using route parameters.</span></span> <span data-ttu-id="ee246-730">針對[專用的傳統路由](xref:mvc/controllers/routing#dcr)和[屬性路由](xref:mvc/controllers/routing#ar)至控制器和 Razor Pages：</span><span class="sxs-lookup"><span data-stu-id="ee246-730">For [dedicated conventional routes](xref:mvc/controllers/routing#dcr) and [attribute routes](xref:mvc/controllers/routing#ar) to controllers and Razor Pages:</span></span>

* <span data-ttu-id="ee246-731">有路由值的階層。</span><span class="sxs-lookup"><span data-stu-id="ee246-731">There is a hierarchy of route values.</span></span>
* <span data-ttu-id="ee246-732">它們不會出現在範本中。</span><span class="sxs-lookup"><span data-stu-id="ee246-732">They don't appear in the template.</span></span>

<span data-ttu-id="ee246-733">在這些情況下，URL 產生會定義**必要的值**概念。</span><span class="sxs-lookup"><span data-stu-id="ee246-733">For these cases, URL generation defines the **required values** concept.</span></span> <span data-ttu-id="ee246-734">控制器和 Razor Pages 所建立的端點具有指定的必要值，讓路由值失效。</span><span class="sxs-lookup"><span data-stu-id="ee246-734">Endpoints created by controllers and Razor Pages have required values specified that allow route value invalidation to work.</span></span>

<span data-ttu-id="ee246-735">路由值失效演算法詳細資料：</span><span class="sxs-lookup"><span data-stu-id="ee246-735">The route value invalidation algorithm in detail:</span></span>

* <span data-ttu-id="ee246-736">必要的值名稱會與路由參數結合，然後由左至右處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-736">The required value names are combined with the route parameters, then processed from left-to-right.</span></span>
* <span data-ttu-id="ee246-737">針對每個參數，會比較環境值和明確的值：</span><span class="sxs-lookup"><span data-stu-id="ee246-737">For each parameter, the ambient value and explicit value are compared:</span></span>
    * <span data-ttu-id="ee246-738">如果環境值和明確的值相同，則進程會繼續進行。</span><span class="sxs-lookup"><span data-stu-id="ee246-738">If the ambient value and explicit value are the same, the process continues.</span></span>
    * <span data-ttu-id="ee246-739">如果環境值存在且明確的值不是，則會在產生 URL 時使用環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-739">If the ambient value is present and the explicit value isn't, the ambient value is used when generating the URL.</span></span>
    * <span data-ttu-id="ee246-740">如果環境值不存在，且明確的值為，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-740">If the ambient value isn't present and the explicit value is, reject the ambient value and all subsequent ambient values.</span></span>
    * <span data-ttu-id="ee246-741">如果環境值和明確值存在，而且這兩個值不同，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-741">If the ambient value and the explicit value are present, and the two values are different, reject the ambient value and all subsequent ambient values.</span></span>

<span data-ttu-id="ee246-742">此時，URL 產生作業已準備好評估路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-742">At this point, the URL generation operation is ready to evaluate route constraints.</span></span> <span data-ttu-id="ee246-743">一組接受的值會與提供給條件約束的參數預設值結合。</span><span class="sxs-lookup"><span data-stu-id="ee246-743">The set of accepted values is combined with the parameter default values, which is provided to constraints.</span></span> <span data-ttu-id="ee246-744">如果條件約束都通過，作業就會繼續。</span><span class="sxs-lookup"><span data-stu-id="ee246-744">If the constraints all pass, the operation continues.</span></span>

<span data-ttu-id="ee246-745">接下來，可以使用**接受的值**來展開路由範本。</span><span class="sxs-lookup"><span data-stu-id="ee246-745">Next, the **accepted values** can be used to expand the route template.</span></span> <span data-ttu-id="ee246-746">會處理路由範本：</span><span class="sxs-lookup"><span data-stu-id="ee246-746">The route template is processed:</span></span>

* <span data-ttu-id="ee246-747">從左至右。</span><span class="sxs-lookup"><span data-stu-id="ee246-747">From left-to-right.</span></span>
* <span data-ttu-id="ee246-748">每個參數都有其接受的值取代。</span><span class="sxs-lookup"><span data-stu-id="ee246-748">Each parameter has its accepted value substituted.</span></span>
* <span data-ttu-id="ee246-749">有下列特殊案例：</span><span class="sxs-lookup"><span data-stu-id="ee246-749">With the following special cases:</span></span>
  * <span data-ttu-id="ee246-750">如果接受的值遺漏值，且參數具有預設值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-750">If the accepted values is missing a value and the parameter has a default value, the default value is used.</span></span>
  * <span data-ttu-id="ee246-751">如果接受的值遺漏值，而且參數是選擇性的，則會繼續處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-751">If the accepted values is missing a value and the parameter is optional, processing continues.</span></span>
  * <span data-ttu-id="ee246-752">如果遺漏選擇性參數右邊的任何路由參數具有值，則作業會失敗。</span><span class="sxs-lookup"><span data-stu-id="ee246-752">If any route parameter to the right of a missing optional parameter has a value, the operation fails.</span></span>
  * <!-- review default-valued parameters optional parameters --> <span data-ttu-id="ee246-753">連續的預設值參數和選擇性參數會盡可能折迭。</span><span class="sxs-lookup"><span data-stu-id="ee246-753">Contiguous default-valued parameters and optional parameters are collapsed where possible.</span></span>

<span data-ttu-id="ee246-754">明確提供並不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-754">Values explicitly provided that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ee246-755">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-755">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ee246-756">環境值</span><span class="sxs-lookup"><span data-stu-id="ee246-756">Ambient Values</span></span>                     | <span data-ttu-id="ee246-757">明確值</span><span class="sxs-lookup"><span data-stu-id="ee246-757">Explicit Values</span></span>                        | <span data-ttu-id="ee246-758">結果</span><span class="sxs-lookup"><span data-stu-id="ee246-758">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ee246-759">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-759">controller = "Home"</span></span>                | <span data-ttu-id="ee246-760">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-760">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-761">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-761">controller = "Home"</span></span>                | <span data-ttu-id="ee246-762">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-762">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ee246-763">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-763">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ee246-764">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-764">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-765">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-765">controller = "Home"</span></span>                | <span data-ttu-id="ee246-766">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-766">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

### <a name="problems-with-route-value-invalidation"></a><span data-ttu-id="ee246-767">路由值失效的問題</span><span class="sxs-lookup"><span data-stu-id="ee246-767">Problems with route value invalidation</span></span>

<span data-ttu-id="ee246-768">從 ASP.NET Core 3.0，先前 ASP.NET Core 版本中使用的某些 URL 產生配置，不適用於 URL 產生的效果。</span><span class="sxs-lookup"><span data-stu-id="ee246-768">As of ASP.NET Core 3.0, some URL generation schemes used in earlier ASP.NET Core versions don't work well with URL generation.</span></span> <span data-ttu-id="ee246-769">ASP.NET Core 小組計畫在未來的版本中新增功能，以解決這些需求。</span><span class="sxs-lookup"><span data-stu-id="ee246-769">The ASP.NET Core team plans to add features to address these needs in a future release.</span></span> <span data-ttu-id="ee246-770">現在最好的解決方案是使用舊版路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-770">For now the best solution is to use legacy routing.</span></span>

<span data-ttu-id="ee246-771">下列程式碼顯示路由不支援的 URL 產生配置範例。</span><span class="sxs-lookup"><span data-stu-id="ee246-771">The following code shows an example of a URL generation scheme that's not supported by routing.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

<span data-ttu-id="ee246-772">在上述程式碼中， `culture` route 參數是用於當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ee246-772">In the preceding code, the `culture` route parameter is used for localization.</span></span> <span data-ttu-id="ee246-773">想要讓`culture`參數一律接受為環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-773">The desire is to have the `culture` parameter always accepted as an ambient value.</span></span> <span data-ttu-id="ee246-774">不過，因為`culture`必要值的使用方式，所以不接受參數作為環境值：</span><span class="sxs-lookup"><span data-stu-id="ee246-774">However, the `culture` parameter is not accepted as an ambient value because of the way required values work:</span></span>

* <span data-ttu-id="ee246-775">在`"default"` `culture`路由範本中，路由參數是在的左邊`controller`，因此變更`controller`不會使失效。 `culture`</span><span class="sxs-lookup"><span data-stu-id="ee246-775">In the `"default"` route template, the `culture` route parameter is to the left of `controller`, so changes to `controller` won't invalidate `culture`.</span></span>
* <span data-ttu-id="ee246-776">在`"blog"`路由範本中， `culture`路由參數會被視為位於的右邊`controller`，這會出現在必要的值中。</span><span class="sxs-lookup"><span data-stu-id="ee246-776">In the `"blog"` route template, the `culture` route parameter is considered to be to the right of `controller`, which appears in the required values.</span></span>

## <a name="configuring-endpoint-metadata"></a><span data-ttu-id="ee246-777">設定端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="ee246-777">Configuring endpoint metadata</span></span>

<span data-ttu-id="ee246-778">下列連結提供設定端點中繼資料的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="ee246-778">The following links provide information on configuring endpoint metadata:</span></span>

* [<span data-ttu-id="ee246-779">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="ee246-779">Enable Cors with endpoint routing</span></span>](xref:security/cors#enable-cors-with-endpoint-routing)
* <span data-ttu-id="ee246-780">使用自訂`[MinimumAgeAuthorize]`屬性的[IAuthorizationPolicyProvider 範例](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)</span><span class="sxs-lookup"><span data-stu-id="ee246-780">[IAuthorizationPolicyProvider sample](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) using a custom `[MinimumAgeAuthorize]` attribute</span></span>
* <span data-ttu-id="ee246-781">[使用 [授權] 屬性測試驗證](xref:security/authentication/identity#test-identity)</span><span class="sxs-lookup"><span data-stu-id="ee246-781">[Test authentication with the [Authorize] attribute](xref:security/authentication/identity#test-identity)</span></span>
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* <span data-ttu-id="ee246-782">[選取具有 [授權] 屬性的配置](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span><span class="sxs-lookup"><span data-stu-id="ee246-782">[Selecting the scheme with the [Authorize] attribute](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span></span>
* <span data-ttu-id="ee246-783">[使用 [授權] 屬性套用原則](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span><span class="sxs-lookup"><span data-stu-id="ee246-783">[Applying policies using the [Authorize] attribute](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span></span>
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a><span data-ttu-id="ee246-784">搭配 RequireHost 的路由中的主機比對</span><span class="sxs-lookup"><span data-stu-id="ee246-784">Host matching in routes with RequireHost</span></span>

<span data-ttu-id="ee246-785"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>將條件約束套用至需要指定之主控制項的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-785"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*> applies a constraint to the route which requires the specified host.</span></span> <span data-ttu-id="ee246-786">`RequireHost`或[[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute)參數可以是：</span><span class="sxs-lookup"><span data-stu-id="ee246-786">The `RequireHost` or [[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute) parameter can be:</span></span>

* <span data-ttu-id="ee246-787">主機： `www.domain.com`，符合`www.domain.com`任何埠。</span><span class="sxs-lookup"><span data-stu-id="ee246-787">Host: `www.domain.com`, matches `www.domain.com` with any port.</span></span>
* <span data-ttu-id="ee246-788">具有萬用字元的主機`*.domain.com`：、 `www.domain.com`符合`subdomain.domain.com`、或`www.subdomain.domain.com`任何埠上的。</span><span class="sxs-lookup"><span data-stu-id="ee246-788">Host with wildcard: `*.domain.com`, matches `www.domain.com`, `subdomain.domain.com`, or `www.subdomain.domain.com` on any port.</span></span>
* <span data-ttu-id="ee246-789">埠： `*:5000`，符合任何主機的通訊埠5000。</span><span class="sxs-lookup"><span data-stu-id="ee246-789">Port: `*:5000`, matches port 5000 with any host.</span></span>
* <span data-ttu-id="ee246-790">主機和埠： `www.domain.com:5000`或`*.domain.com:5000`符合主機和埠。</span><span class="sxs-lookup"><span data-stu-id="ee246-790">Host and port: `www.domain.com:5000` or `*.domain.com:5000`, matches host and port.</span></span>

<span data-ttu-id="ee246-791">您可以使用`RequireHost`或`[Host]`來指定多個參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-791">Multiple parameters can be specified using `RequireHost` or `[Host]`.</span></span> <span data-ttu-id="ee246-792">條件約束符合任何參數的有效主機。</span><span class="sxs-lookup"><span data-stu-id="ee246-792">The constraint  matches hosts valid for any of the parameters.</span></span> <span data-ttu-id="ee246-793">例如， `[Host("domain.com", "*.domain.com")]`會比`domain.com`對`www.domain.com`、和`subdomain.domain.com`。</span><span class="sxs-lookup"><span data-stu-id="ee246-793">For example, `[Host("domain.com", "*.domain.com")]` matches `domain.com`, `www.domain.com`, and `subdomain.domain.com`.</span></span>

<span data-ttu-id="ee246-794">下列程式碼會`RequireHost`使用來要求路由上指定的主機：</span><span class="sxs-lookup"><span data-stu-id="ee246-794">The following code uses `RequireHost` to require the specified host on the route:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

<span data-ttu-id="ee246-795">下列程式碼會在`[Host]`控制器上使用屬性來要求任何指定的主機：</span><span class="sxs-lookup"><span data-stu-id="ee246-795">The following code uses the `[Host]` attribute on the controller to require any of the specified hosts:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

<span data-ttu-id="ee246-796">當`[Host]`屬性同時套用至控制器和動作方法時：</span><span class="sxs-lookup"><span data-stu-id="ee246-796">When the `[Host]` attribute is applied to both the controller and action method:</span></span>

* <span data-ttu-id="ee246-797">會使用動作上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-797">The attribute on the action is used.</span></span>
* <span data-ttu-id="ee246-798">已忽略控制器屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-798">The controller attribute is ignored.</span></span>

## <a name="performance-guidance-for-routing"></a><span data-ttu-id="ee246-799">路由的效能指引</span><span class="sxs-lookup"><span data-stu-id="ee246-799">Performance guidance for routing</span></span>

<span data-ttu-id="ee246-800">大部分的路由都已在 ASP.NET Core 3.0 中更新，以提高效能。</span><span class="sxs-lookup"><span data-stu-id="ee246-800">Most of routing was updated in ASP.NET Core 3.0 to increase performance.</span></span>

<span data-ttu-id="ee246-801">當應用程式發生效能問題時，通常會懷疑路由是問題。</span><span class="sxs-lookup"><span data-stu-id="ee246-801">When an app has performance problems, routing is often suspected as the problem.</span></span> <span data-ttu-id="ee246-802">路由的原因是，控制器和 Razor Pages 之類的架構，會報告其記錄訊息內所花費的時間量。</span><span class="sxs-lookup"><span data-stu-id="ee246-802">The reason routing is suspected is that frameworks like controllers and Razor Pages report the amount of time spent inside the framework in their logging messages.</span></span> <span data-ttu-id="ee246-803">當控制器回報的時間與要求的總時間之間有顯著的差異時：</span><span class="sxs-lookup"><span data-stu-id="ee246-803">When there's a significant difference between the time reported by controllers and the total time of the request:</span></span>

* <span data-ttu-id="ee246-804">開發人員會將其應用程式程式碼排除為問題的來源。</span><span class="sxs-lookup"><span data-stu-id="ee246-804">Developers eliminate their app code as the source of the problem.</span></span>
* <span data-ttu-id="ee246-805">通常會假設路由是原因。</span><span class="sxs-lookup"><span data-stu-id="ee246-805">It's common to assume routing is the cause.</span></span>

<span data-ttu-id="ee246-806">路由會使用數千個端點來測試效能。</span><span class="sxs-lookup"><span data-stu-id="ee246-806">Routing is performance tested using thousands of endpoints.</span></span> <span data-ttu-id="ee246-807">一般的應用程式不太可能會遇到太大的效能問題。</span><span class="sxs-lookup"><span data-stu-id="ee246-807">It's unlikely that a typical app will encounter a performance problem just by being too large.</span></span> <span data-ttu-id="ee246-808">緩慢路由效能最常見的根本原因通常是行為不佳的自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-808">The most common root cause of slow routing performance is usually a badly-behaving custom middleware.</span></span>

<span data-ttu-id="ee246-809">下列程式碼範例示範縮小延遲來源的基本技巧：</span><span class="sxs-lookup"><span data-stu-id="ee246-809">This following code sample demonstrates a basic technique for narrowing down the source of delay:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

<span data-ttu-id="ee246-810">若要進行時間路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-810">To time routing:</span></span>

* <span data-ttu-id="ee246-811">使用上述程式碼中所示的計時中介軟體複本來交錯每個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-811">Interleave each middleware with a copy of the timing middleware shown in the preceding code.</span></span>
* <span data-ttu-id="ee246-812">新增唯一識別碼，將計時資料與程式碼相互關聯。</span><span class="sxs-lookup"><span data-stu-id="ee246-812">Add a unique identifier to correlate the timing data with the code.</span></span>

<span data-ttu-id="ee246-813">這是將延遲縮小到最大的基本方式，例如，超過`10ms`。</span><span class="sxs-lookup"><span data-stu-id="ee246-813">This is a basic way to narrow down the delay when it's significant, for example, more than `10ms`.</span></span>  <span data-ttu-id="ee246-814">從`Time 2` `Time 1`報表減去`UseRouting`中介軟體內所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="ee246-814">Subtracting `Time 2` from `Time 1` reports the time spent inside the `UseRouting` middleware.</span></span>

<span data-ttu-id="ee246-815">下列程式碼會針對上述計時程式碼使用更精簡的方法：</span><span class="sxs-lookup"><span data-stu-id="ee246-815">The following code uses a more compact approach to the preceding timing code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a><span data-ttu-id="ee246-816">可能昂貴的路由功能</span><span class="sxs-lookup"><span data-stu-id="ee246-816">Potentially expensive routing features</span></span>

<span data-ttu-id="ee246-817">下列清單提供一些路由功能的深入解析，相較于基本的路由範本，這會相當耗費資源：</span><span class="sxs-lookup"><span data-stu-id="ee246-817">The following list provides some insight into routing features that are relatively expensive compared with basic route templates:</span></span>

* <span data-ttu-id="ee246-818">正則運算式：可以撰寫複雜的正則運算式，或具有少量輸入的長期執行時間。</span><span class="sxs-lookup"><span data-stu-id="ee246-818">Regular expressions: It's possible to write regular expressions that are complex, or have long running time with a small amount of input.</span></span>

* <span data-ttu-id="ee246-819">複雜區段（`{x}-{y}-{z}`）：</span><span class="sxs-lookup"><span data-stu-id="ee246-819">Complex segments (`{x}-{y}-{z}`):</span></span> 
  * <span data-ttu-id="ee246-820">比剖析一般 URL 路徑區段高得多。</span><span class="sxs-lookup"><span data-stu-id="ee246-820">Are significantly more expensive than parsing a regular URL path segment.</span></span>
  * <span data-ttu-id="ee246-821">導致已配置更多子字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-821">Result in many more substrings being allocated.</span></span>
  * <span data-ttu-id="ee246-822">在 ASP.NET Core 3.0 路由效能更新中，未更新複雜的區段邏輯。</span><span class="sxs-lookup"><span data-stu-id="ee246-822">The complex segment logic was not updated in ASP.NET Core 3.0 routing performance update.</span></span>

* <span data-ttu-id="ee246-823">同步資料存取：許多複雜的應用程式在其路由中都具有資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="ee246-823">Synchronous data access: Many complex apps have database access as part of their routing.</span></span> <span data-ttu-id="ee246-824">ASP.NET Core 2.2 和較舊的路由可能不會提供支援資料庫存取路由的正確擴充點。</span><span class="sxs-lookup"><span data-stu-id="ee246-824">ASP.NET Core 2.2 and earlier routing might not provide the right extensibility points to support database access routing.</span></span> <span data-ttu-id="ee246-825">例如， <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>、和<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>都是同步的。</span><span class="sxs-lookup"><span data-stu-id="ee246-825">For example, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, and <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are synchronous.</span></span> <span data-ttu-id="ee246-826"><xref:Microsoft.AspNetCore.Routing.MatcherPolicy>和<xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext>等擴充點都是非同步。</span><span class="sxs-lookup"><span data-stu-id="ee246-826">Extensibility points such as <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> and <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> are asynchronous.</span></span>

## <a name="guidance-for-library-authors"></a><span data-ttu-id="ee246-827">程式庫作者指引</span><span class="sxs-lookup"><span data-stu-id="ee246-827">Guidance for library authors</span></span>

<span data-ttu-id="ee246-828">本章節包含程式庫作者在路由之上建立的指導方針。</span><span class="sxs-lookup"><span data-stu-id="ee246-828">This section contains guidance for library authors building on top of routing.</span></span> <span data-ttu-id="ee246-829">這些詳細資料的目的是要確保應用程式開發人員能夠使用延伸路由的程式庫和架構。</span><span class="sxs-lookup"><span data-stu-id="ee246-829">These details are intended to ensure that app developers have a good experience using libraries and frameworks that extend routing.</span></span>

### <a name="define-endpoints"></a><span data-ttu-id="ee246-830">定義端點</span><span class="sxs-lookup"><span data-stu-id="ee246-830">Define endpoints</span></span>

<span data-ttu-id="ee246-831">若要建立使用路由進行 URL 比對的架構，請先定義以為基礎的使用者體驗<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-831">To create a framework that uses routing for URL matching, start by defining a user experience that builds on top of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>

<span data-ttu-id="ee246-832">**請**在之上建立組建<xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="ee246-832">**DO** build on top of <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span> <span data-ttu-id="ee246-833">這可讓使用者使用其他 ASP.NET Core 功能來撰寫您的架構，而不會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="ee246-833">This allows users to compose your framework with other ASP.NET Core features without confusion.</span></span> <span data-ttu-id="ee246-834">每個 ASP.NET Core 範本都包含路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-834">Every ASP.NET Core template includes routing.</span></span> <span data-ttu-id="ee246-835">假設路由存在且熟悉使用者。</span><span class="sxs-lookup"><span data-stu-id="ee246-835">Assume routing is present and familiar for users.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="ee246-836">**確實**從對`MapMyFramework(...)`所執行的呼叫傳回密封的實體型<xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>別。</span><span class="sxs-lookup"><span data-stu-id="ee246-836">**DO** return a sealed concrete type from a call to `MapMyFramework(...)` that implements <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>.</span></span> <span data-ttu-id="ee246-837">大部分的`Map...`架構方法會遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="ee246-837">Most framework `Map...` methods follow this pattern.</span></span> <span data-ttu-id="ee246-838">`IEndpointConventionBuilder`介面：</span><span class="sxs-lookup"><span data-stu-id="ee246-838">The `IEndpointConventionBuilder` interface:</span></span>

* <span data-ttu-id="ee246-839">允許複合性中繼資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-839">Allows composability of metadata.</span></span>
* <span data-ttu-id="ee246-840">的目標是各種擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-840">Is targeted by a variety of extension methods.</span></span>

<span data-ttu-id="ee246-841">宣告您自己的型別可讓您將自己的架構特定功能加入至產生器。</span><span class="sxs-lookup"><span data-stu-id="ee246-841">Declaring your own type allows you to add your own framework-specific functionality to the builder.</span></span> <span data-ttu-id="ee246-842">將架構宣告的產生器和轉送呼叫包裝在一起是正常的。</span><span class="sxs-lookup"><span data-stu-id="ee246-842">It's ok to wrap a framework-declared builder and forward calls to it.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="ee246-843">**請考慮**撰寫自己<xref:Microsoft.AspNetCore.Routing.EndpointDataSource>的。</span><span class="sxs-lookup"><span data-stu-id="ee246-843">**CONSIDER** writing your own <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>.</span></span> <span data-ttu-id="ee246-844">`EndpointDataSource`是用來宣告和更新端點集合的低層級基本類型。</span><span class="sxs-lookup"><span data-stu-id="ee246-844">`EndpointDataSource` is the low-level primitive for declaring and updating a collection of endpoints.</span></span> <span data-ttu-id="ee246-845">`EndpointDataSource`是控制器和 Razor Pages 所使用的強大 API。</span><span class="sxs-lookup"><span data-stu-id="ee246-845">`EndpointDataSource` is a powerful API used by controllers and Razor Pages.</span></span>

<span data-ttu-id="ee246-846">路由測試有一個不會更新資料來源的[基本範例](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17)。</span><span class="sxs-lookup"><span data-stu-id="ee246-846">The routing tests have a [basic example](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17) of a non-updating data source.</span></span>

<span data-ttu-id="ee246-847">預設**不會**嘗試註冊`EndpointDataSource` 。</span><span class="sxs-lookup"><span data-stu-id="ee246-847">**DO NOT** attempt to register an `EndpointDataSource` by default.</span></span> <span data-ttu-id="ee246-848">要求使用者在中註冊您的<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>架構。</span><span class="sxs-lookup"><span data-stu-id="ee246-848">Require users to register your framework in <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span> <span data-ttu-id="ee246-849">路由的原理是預設不會包含任何內容，也`UseEndpoints`就是註冊端點的位置。</span><span class="sxs-lookup"><span data-stu-id="ee246-849">The philosophy of routing is that nothing is included by default, and that `UseEndpoints` is the place to register endpoints.</span></span>

### <a name="creating-routing-integrated-middleware"></a><span data-ttu-id="ee246-850">建立路由整合中介軟體</span><span class="sxs-lookup"><span data-stu-id="ee246-850">Creating routing-integrated middleware</span></span>

<span data-ttu-id="ee246-851">**請考慮**將元資料類型定義為介面。</span><span class="sxs-lookup"><span data-stu-id="ee246-851">**CONSIDER** defining metadata types as an interface.</span></span>

<span data-ttu-id="ee246-852">您可以使用中繼資料**類型做為**類別和方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-852">**DO** make it possible to use metadata types as an attribute on classes and methods.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

<span data-ttu-id="ee246-853">控制器和 Razor Pages 之類的架構支援將中繼資料屬性套用至類型和方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-853">Frameworks like controllers and Razor Pages support applying metadata attributes to types and methods.</span></span> <span data-ttu-id="ee246-854">如果您宣告元資料類型：</span><span class="sxs-lookup"><span data-stu-id="ee246-854">If you declare metadata types:</span></span>

* <span data-ttu-id="ee246-855">使其可做為[屬性](/dotnet/csharp/programming-guide/concepts/attributes/)存取。</span><span class="sxs-lookup"><span data-stu-id="ee246-855">Make them accessible as [attributes](/dotnet/csharp/programming-guide/concepts/attributes/).</span></span>
* <span data-ttu-id="ee246-856">大部分的使用者都很熟悉套用屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-856">Most users are familiar with applying attributes.</span></span>

<span data-ttu-id="ee246-857">將元資料類型宣告為介面，會增加另一層彈性：</span><span class="sxs-lookup"><span data-stu-id="ee246-857">Declaring a metadata type as an interface adds another layer of flexibility:</span></span>

* <span data-ttu-id="ee246-858">介面是可組合的。</span><span class="sxs-lookup"><span data-stu-id="ee246-858">Interfaces are composable.</span></span>
* <span data-ttu-id="ee246-859">開發人員可以宣告自己的類型，結合多個原則。</span><span class="sxs-lookup"><span data-stu-id="ee246-859">Developers can declare their own types that combine multiple policies.</span></span>

<span data-ttu-id="ee246-860">**確實**能夠覆寫中繼資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="ee246-860">**DO** make it possible to override metadata, as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

<span data-ttu-id="ee246-861">遵循這些指導方針的最佳方式是避免定義**標記中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="ee246-861">The best way to follow these guidelines is to avoid defining **marker metadata**:</span></span>

* <span data-ttu-id="ee246-862">請不要只尋找元資料類型是否存在。</span><span class="sxs-lookup"><span data-stu-id="ee246-862">Don't just look for the presence of a metadata type.</span></span>
* <span data-ttu-id="ee246-863">在中繼資料上定義屬性，並檢查屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-863">Define a property on the metadata and check the property.</span></span>

<span data-ttu-id="ee246-864">元資料集合會進行排序，並依優先順序支援覆寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-864">The metadata collection is ordered and supports overriding by priority.</span></span> <span data-ttu-id="ee246-865">在控制器的案例中，動作方法上的中繼資料是最明確的。</span><span class="sxs-lookup"><span data-stu-id="ee246-865">In the case of controllers, metadata on the action method is most specific.</span></span>

<span data-ttu-id="ee246-866">**讓中間**件適用于且不使用路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-866">**DO** make middleware useful with and without routing.</span></span>

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

<span data-ttu-id="ee246-867">作為此指導方針的範例，請考慮`UseAuthorization`中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-867">As an example of this guideline, consider the `UseAuthorization` middleware.</span></span> <span data-ttu-id="ee246-868">授權中介軟體可讓您傳入回退原則。</span><span class="sxs-lookup"><span data-stu-id="ee246-868">The authorization middleware allows you to pass in a fallback policy.</span></span> <!-- shown where?  (shown here) --> <span data-ttu-id="ee246-869">如果指定了回溯原則，則會套用至兩者：</span><span class="sxs-lookup"><span data-stu-id="ee246-869">The fallback policy, if specified, applies to both:</span></span>

* <span data-ttu-id="ee246-870">沒有指定之原則的端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-870">Endpoints without a specified policy.</span></span>
* <span data-ttu-id="ee246-871">不符合端點的要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-871">Requests that don't match an endpoint.</span></span>

<span data-ttu-id="ee246-872">這可讓授權中介軟體適用于路由內容以外的地方。</span><span class="sxs-lookup"><span data-stu-id="ee246-872">This makes the authorization middleware useful outside of the context of routing.</span></span> <span data-ttu-id="ee246-873">授權中介軟體可用於傳統中介軟體程式設計。</span><span class="sxs-lookup"><span data-stu-id="ee246-873">The authorization middleware can be used for traditional middleware programming.</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ee246-874">路由會負責將要求 Uri 對應至端點，並將傳入要求分派至這些端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-874">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="ee246-875">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ee246-875">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ee246-876">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-876">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ee246-877">使用來自應用程式的路由資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="ee246-877">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="ee246-878">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ee246-878">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ee246-879"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="ee246-879">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ee246-880">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ee246-880">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="ee246-881">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="ee246-881">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ee246-882">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="ee246-882">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee246-883">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-883">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="ee246-884">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ee246-884">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="ee246-885">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-885">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ee246-886">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ee246-886">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ee246-887">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ee246-887">Routing basics</span></span>

<span data-ttu-id="ee246-888">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="ee246-888">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="ee246-889">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="ee246-889">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="ee246-890">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="ee246-890">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="ee246-891">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="ee246-891">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="ee246-892">開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專用的慣例路由，在特殊情況下，將額外的簡易路由新增到應用程式的高流量區域。</span><span class="sxs-lookup"><span data-stu-id="ee246-892">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span> <span data-ttu-id="ee246-893">特殊情況的範例包括： blog 和電子商務端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-893">Specialized situations examples include, blog and ecommerce endpoints.</span></span>

<span data-ttu-id="ee246-894">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-894">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="ee246-895">這表示相同邏輯資源上的許多作業（例如 GET 和 POST）都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-895">This means that many operations, for example, GET, and POST, on the same logical resource use the same URL.</span></span> <span data-ttu-id="ee246-896">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="ee246-896">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="ee246-897">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="ee246-897">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="ee246-898">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-898">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="ee246-899">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-899">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ee246-900">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="ee246-900">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="ee246-901">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-901">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="ee246-902">路由會*endpoints*使用端點`Endpoint`（）來代表應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-902">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="ee246-903">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-903">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="ee246-904">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="ee246-904">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="ee246-905">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ee246-905">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ee246-906">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-906">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ee246-907">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="ee246-907">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ee246-908"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-908"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ee246-909">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="ee246-909">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ee246-910">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-910">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="ee246-911">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-911">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="ee246-912">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-912">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="ee246-913">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ee246-913">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ee246-914">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="ee246-914">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="ee246-915">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-915">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="ee246-916">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-916">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="ee246-917">在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="ee246-917">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="ee246-918">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-918">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="ee246-919">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-919">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="ee246-920">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-920">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="ee246-921">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-921">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="ee246-922">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ee246-922">URL matching</span></span>

<span data-ttu-id="ee246-923">URL 比對是路由用來將傳入要求分派給「端點」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-923">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="ee246-924">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-924">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ee246-925">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ee246-925">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ee246-926">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-926">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="ee246-927">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-927">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="ee246-928">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-928">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="ee246-929">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-929">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="ee246-930">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-930">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="ee246-931">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="ee246-931">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="ee246-932">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-932"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="ee246-933">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-933">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="ee246-934">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="ee246-934">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="ee246-935">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="ee246-935">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="ee246-936">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="ee246-936">Routes can be nested inside of one another.</span></span> <span data-ttu-id="ee246-937"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-937">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="ee246-938">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-938">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="ee246-939"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="ee246-939">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="ee246-940">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="ee246-940">URL generation with LinkGenerator</span></span>

<span data-ttu-id="ee246-941">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-941">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ee246-942">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-942">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="ee246-943">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="ee246-943">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="ee246-944"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>是可從[DI](xref:fundamentals/dependency-injection)抓取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="ee246-944"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ee246-945">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="ee246-945">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="ee246-946">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-946">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="ee246-947">連結產生器背後支援的概念為「位址」\*\* 和「位址配置」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-947">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="ee246-948">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="ee246-948">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="ee246-949">例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="ee246-949">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="ee246-950">連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="ee246-950">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="ee246-951">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="ee246-951">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="ee246-952">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="ee246-952">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="ee246-953">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-953">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="ee246-954">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-954">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="ee246-955">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-955">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="ee246-956">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="ee246-956">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="ee246-957">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="ee246-957"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="ee246-958">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="ee246-958">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="ee246-959">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="ee246-959">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="ee246-960">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="ee246-960">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="ee246-961">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="ee246-961">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="ee246-962"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ee246-962">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="ee246-963">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-963">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="ee246-964">擴充方法</span><span class="sxs-lookup"><span data-stu-id="ee246-964">Extension Method</span></span>   | <span data-ttu-id="ee246-965">說明</span><span class="sxs-lookup"><span data-stu-id="ee246-965">Description</span></span>                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="ee246-966">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-966">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="ee246-967">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-967">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="ee246-968">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="ee246-968">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="ee246-969">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ee246-969">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="ee246-970">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee246-970">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="ee246-971">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-971">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="ee246-972">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-972">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="ee246-973">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-973">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="ee246-974">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-974">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="ee246-975">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="ee246-975">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="ee246-976">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="ee246-976">Differences from earlier versions of routing</span></span>

<span data-ttu-id="ee246-977">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="ee246-977">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="ee246-978">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="ee246-978">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="ee246-979">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="ee246-979">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="ee246-980">請使用 2.1[相容](xref:mvc/compatibility-version)性版本`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`（）繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-980">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="ee246-981">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="ee246-981">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="ee246-982">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ee246-982">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ee246-983">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ee246-983">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="ee246-984">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-984">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="ee246-985">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="ee246-985">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="ee246-986">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="ee246-986">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="ee246-987">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-987">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="ee246-988">當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。</span><span class="sxs-lookup"><span data-stu-id="ee246-988">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="ee246-989">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ee246-989">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ee246-990">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ee246-990">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="ee246-991">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="ee246-991">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="ee246-992">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="ee246-992">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="ee246-993">不過，如果動作不存在，則端點路由會產生空字串。\*\*</span><span class="sxs-lookup"><span data-stu-id="ee246-993">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="ee246-994">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="ee246-994">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="ee246-995">連結產生「環境值失效演算法」\*\* 在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-995">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="ee246-996">「環境值失效」\*\* 是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-996">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="ee246-997">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="ee246-997">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="ee246-998">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-998">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="ee246-999">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ee246-999">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="ee246-1000">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="ee246-1000">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="ee246-1001">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="ee246-1001">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ee246-1002">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-1002">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="ee246-1003">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ee246-1003">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="ee246-1004">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ee246-1004">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="ee246-1005">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1005">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="ee246-1006">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="ee246-1006">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="ee246-1007">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1007">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="ee246-1008">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1008">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="ee246-1009">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1009">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="ee246-1010">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1010">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="ee246-1011">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1011">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="ee246-1012">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="ee246-1012">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="ee246-1013">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1013">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="ee246-1014">路由</span><span class="sxs-lookup"><span data-stu-id="ee246-1014">Route</span></span>              | <span data-ttu-id="ee246-1015">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="ee246-1015">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="ee246-1016">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="ee246-1016">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="ee246-1017">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="ee246-1017">Middleware example</span></span>

<span data-ttu-id="ee246-1018">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-1018">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="ee246-1019">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1019">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

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

### <a name="create-routes"></a><span data-ttu-id="ee246-1020">建立路由</span><span class="sxs-lookup"><span data-stu-id="ee246-1020">Create routes</span></span>

<span data-ttu-id="ee246-1021">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1021">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="ee246-1022">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1022">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="ee246-1023"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1023"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ee246-1024"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1024"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ee246-1025">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1025">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="ee246-1026">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="ee246-1026">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ee246-1027">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1027">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="ee246-1028">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1028">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="ee246-1029">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1029">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="ee246-1030">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="ee246-1030">Route parameters are named.</span></span> <span data-ttu-id="ee246-1031">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="ee246-1031">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="ee246-1032">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1032">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="ee246-1033">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1033">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="ee246-1034">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1034">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="ee246-1035">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1035">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="ee246-1036">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1036">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="ee246-1037">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1037">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="ee246-1038">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1038">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="ee246-1039">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="ee246-1039">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="ee246-1040">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1040">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="ee246-1041">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ee246-1041">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="ee246-1042">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1042">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="ee246-1043">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1043">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="ee246-1044"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1044">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="ee246-1045">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-1045">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="ee246-1046">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1046">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="ee246-1047">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="ee246-1047">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="ee246-1048">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1048">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="ee246-1049">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="ee246-1049">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ee246-1050">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1050">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ee246-1051">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1051">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ee246-1052">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1052">Default values can be specified in the route template.</span></span> <span data-ttu-id="ee246-1053">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1053">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="ee246-1054">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1054">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="ee246-1055">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="ee246-1055">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="ee246-1056">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1056">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="ee246-1058">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="ee246-1058">Route class URL generation</span></span>

<span data-ttu-id="ee246-1059"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1059">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="ee246-1060">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-1060">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="ee246-1061">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1061">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="ee246-1062">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="ee246-1062">What values would be produced?</span></span> <span data-ttu-id="ee246-1063">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="ee246-1063">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="ee246-1064">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1064">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ee246-1065">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1065">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="ee246-1066">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-1066">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="ee246-1067">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1067">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="ee246-1068">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1068">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="ee246-1069">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-1069">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="ee246-1070">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1070">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ee246-1071">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1071">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="ee246-1072">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1072">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="ee246-1073">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="ee246-1073">Use Routing Middleware</span></span>

<span data-ttu-id="ee246-1074">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1074">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="ee246-1075">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ee246-1075">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="ee246-1076">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="ee246-1076">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="ee246-1077">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="ee246-1077">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="ee246-1078"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1078"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="ee246-1079">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="ee246-1079">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="ee246-1080">URI</span><span class="sxs-lookup"><span data-stu-id="ee246-1080">URI</span></span>                    | <span data-ttu-id="ee246-1081">回應</span><span class="sxs-lookup"><span data-stu-id="ee246-1081">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="ee246-1082">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1082">Hello!</span></span> <span data-ttu-id="ee246-1083">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1083">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="ee246-1084">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1084">Hello!</span></span> <span data-ttu-id="ee246-1085">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1085">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="ee246-1086">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1086">Hello!</span></span> <span data-ttu-id="ee246-1087">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1087">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="ee246-1088">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1088">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="ee246-1089">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="ee246-1089">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="ee246-1090">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="ee246-1090">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="ee246-1091">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1091">The request falls through, no match.</span></span>              |

<span data-ttu-id="ee246-1092">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="ee246-1092">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="ee246-1093">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="ee246-1093">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="ee246-1094">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1094">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="ee246-1095">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1095">Route template reference</span></span>

<span data-ttu-id="ee246-1096">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1096">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="ee246-1097">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-1097">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="ee246-1098">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1098">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="ee246-1099">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1099">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ee246-1100">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ee246-1100">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ee246-1101">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="ee246-1101">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="ee246-1102">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-1102">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="ee246-1103">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ee246-1103">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ee246-1104">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ee246-1104">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ee246-1105">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1105">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ee246-1106">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-1106">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="ee246-1107">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1107">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ee246-1108">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ee246-1108">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ee246-1109">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1109">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="ee246-1110">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-1110">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ee246-1111">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1111">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ee246-1112">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1112">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ee246-1113">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1113">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ee246-1114">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ee246-1114">Note the escaped forward slash.</span></span> <span data-ttu-id="ee246-1115">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="ee246-1115">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="ee246-1116">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1116">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="ee246-1117">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-1117">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ee246-1118">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1118">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ee246-1119">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1119">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ee246-1120">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="ee246-1120">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="ee246-1121">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1121">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ee246-1122">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1122">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ee246-1123">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1123">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="ee246-1124">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="ee246-1124">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="ee246-1125">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-1125">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="ee246-1126">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1126">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ee246-1127">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-1127">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ee246-1128">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1128">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ee246-1129">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1129">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="ee246-1130">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1130">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="ee246-1131">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="ee246-1131">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="ee246-1132">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1132">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="ee246-1133">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1133">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="ee246-1134">路由範本</span><span class="sxs-lookup"><span data-stu-id="ee246-1134">Route Template</span></span>                           | <span data-ttu-id="ee246-1135">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ee246-1135">Example Matching URI</span></span>    | <span data-ttu-id="ee246-1136">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ee246-1136">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ee246-1137">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1137">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ee246-1138">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1138">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ee246-1139">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1139">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ee246-1140">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ee246-1140">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ee246-1141">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1141">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ee246-1142">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1142">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="ee246-1143">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1143">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ee246-1144">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1144">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="ee246-1145">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1145">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="ee246-1146">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="ee246-1146">Reserved routing names</span></span>

<span data-ttu-id="ee246-1147">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="ee246-1147">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="ee246-1148">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1148">Route constraint reference</span></span>

<span data-ttu-id="ee246-1149">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ee246-1149">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ee246-1150">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-1150">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="ee246-1151">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1151">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ee246-1152">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1152">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ee246-1153">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ee246-1153">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ee246-1154">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-1154">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="ee246-1155">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ee246-1155">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="ee246-1156">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ee246-1156">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ee246-1157">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1157">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="ee246-1158">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="ee246-1158">constraint</span></span> | <span data-ttu-id="ee246-1159">範例</span><span class="sxs-lookup"><span data-stu-id="ee246-1159">Example</span></span> | <span data-ttu-id="ee246-1160">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1160">Example Matches</span></span> | <span data-ttu-id="ee246-1161">備忘錄</span><span class="sxs-lookup"><span data-stu-id="ee246-1161">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ee246-1162">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-1162">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-1163">符合任何整數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1163">Matches any integer.</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ee246-1164">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ee246-1164">`true`, `FALSE`</span></span> | <span data-ttu-id="ee246-1165">符合`true`或 ' false。</span><span class="sxs-lookup"><span data-stu-id="ee246-1165">Matches `true` or \`false.</span></span> <span data-ttu-id="ee246-1166">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-1166">Case-insensitive.</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ee246-1167">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ee246-1167">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ee246-1168">符合不因`DateTime`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1168">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1169">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1169">See  preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="ee246-1170">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ee246-1170">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ee246-1171">符合不因`decimal`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1171">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1172">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1172">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ee246-1173">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-1173">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-1174">符合不因`double`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1174">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1175">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1175">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ee246-1176">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-1176">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-1177">符合不因`float`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1177">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1178">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1178">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="ee246-1179">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ee246-1179">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ee246-1180">符合有效`Guid`的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1180">Matches a valid `Guid` value.</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ee246-1181">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-1181">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-1182">符合有效`long`的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1182">Matches a valid `long` value.</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ee246-1183">字串必須至少有4個字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1183">String must be at least 4 characters.</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="ee246-1184">字串最多可以有8個字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1184">String has maximum of 8 characters.</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ee246-1185">字串長度必須剛好12個字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1185">String must be exactly 12 characters long.</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ee246-1186">字串必須至少為8個字元，且最多可以有16個字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1186">String must be at least 8 and has maximum of 16 characters.</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ee246-1187">整數值必須至少為18。</span><span class="sxs-lookup"><span data-stu-id="ee246-1187">Integer value must be at least 18.</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ee246-1188">整數值上限為120。</span><span class="sxs-lookup"><span data-stu-id="ee246-1188">Integer value maximum of 120.</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ee246-1189">整數值必須至少為18，而最大值為120。</span><span class="sxs-lookup"><span data-stu-id="ee246-1189">Integer value must be at least 18 and maximum of 120.</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ee246-1190">字串必須包含一或多個字母字元`a` - `z`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1190">String must consist of one or more alphabetical characters `a`-`z`.</span></span>  <span data-ttu-id="ee246-1191">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee246-1191">Case-insensitive.</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ee246-1192">字串必須符合正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1192">String must match the regular expression.</span></span> <span data-ttu-id="ee246-1193">請參閱定義正則運算式的秘訣。</span><span class="sxs-lookup"><span data-stu-id="ee246-1193">See tips about defining a regular expression.</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ee246-1194">用來強制在 URL 產生期間出現非參數值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1194">Used to enforce that a non-parameter value is present during URL generation.</span></span> |

<span data-ttu-id="ee246-1195">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1195">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ee246-1196">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ee246-1196">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ee246-1197">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1197">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ee246-1198">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ee246-1198">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="ee246-1199">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1199">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ee246-1200">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1200">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ee246-1201">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1201">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="ee246-1202">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1202">Regular expressions</span></span>

<span data-ttu-id="ee246-1203">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1203">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ee246-1204">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1204">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ee246-1205">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="ee246-1205">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="ee246-1206">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-1206">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ee246-1207">若要在路由中`^\d{3}-\d{2}-\d{4}$`使用正則運算式：</span><span class="sxs-lookup"><span data-stu-id="ee246-1207">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing:</span></span>

* <span data-ttu-id="ee246-1208">運算式必須在字串中提供單一`\`反斜線字元，做為原始程式碼中`\\`的雙反斜線字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1208">The expression must have the single backslash `\` characters provided in the string as double backslash `\\` characters in the source code.</span></span>
* <span data-ttu-id="ee246-1209">正則運算式必須`\\`是，才能將`\`字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="ee246-1209">The regular expression must us `\\` in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="ee246-1210">使用[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值`\\`時，不需要正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1210">The regular expression doesn't require `\\` when using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="ee246-1211">若要 escape 路由參數分隔符號`{`、 `}`、 `[`、 `]`，請將`{{`運算式中的字元、 `}` `[[`、、 `]]`加倍。</span><span class="sxs-lookup"><span data-stu-id="ee246-1211">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression `{{`, `}`, `[[`, `]]`.</span></span> <span data-ttu-id="ee246-1212">下表顯示正則運算式和已轉義的版本：</span><span class="sxs-lookup"><span data-stu-id="ee246-1212">The following table shows a regular expression and the escaped version:</span></span>

| <span data-ttu-id="ee246-1213">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1213">Regular Expression</span></span>    | <span data-ttu-id="ee246-1214">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1214">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ee246-1215">路由中使用的正則運算式通常會以`^`插入號字元開頭，並比對字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ee246-1215">Regular expressions used in routing often start with the caret `^` character and match starting position of the string.</span></span> <span data-ttu-id="ee246-1216">這些運算式通常會以貨幣符號`$`字元結尾，並比對字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ee246-1216">The expressions often end with the dollar sign `$` character and match end of the string.</span></span> <span data-ttu-id="ee246-1217">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1217">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="ee246-1218">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-1218">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ee246-1219">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="ee246-1219">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="ee246-1220">運算是</span><span class="sxs-lookup"><span data-stu-id="ee246-1220">Expression</span></span>   | <span data-ttu-id="ee246-1221">字串</span><span class="sxs-lookup"><span data-stu-id="ee246-1221">String</span></span>    | <span data-ttu-id="ee246-1222">比對</span><span class="sxs-lookup"><span data-stu-id="ee246-1222">Match</span></span> | <span data-ttu-id="ee246-1223">註解</span><span class="sxs-lookup"><span data-stu-id="ee246-1223">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1224">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-1224">hello</span></span>     | <span data-ttu-id="ee246-1225">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1225">Yes</span></span>   | <span data-ttu-id="ee246-1226">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1226">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1227">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-1227">123abc456</span></span> | <span data-ttu-id="ee246-1228">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1228">Yes</span></span>   | <span data-ttu-id="ee246-1229">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1229">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1230">mz</span><span class="sxs-lookup"><span data-stu-id="ee246-1230">mz</span></span>        | <span data-ttu-id="ee246-1231">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1231">Yes</span></span>   | <span data-ttu-id="ee246-1232">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1232">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1233">MZ</span><span class="sxs-lookup"><span data-stu-id="ee246-1233">MZ</span></span>        | <span data-ttu-id="ee246-1234">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1234">Yes</span></span>   | <span data-ttu-id="ee246-1235">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ee246-1235">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-1236">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-1236">hello</span></span>     | <span data-ttu-id="ee246-1237">否</span><span class="sxs-lookup"><span data-stu-id="ee246-1237">No</span></span>    | <span data-ttu-id="ee246-1238">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-1238">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-1239">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-1239">123abc456</span></span> | <span data-ttu-id="ee246-1240">否</span><span class="sxs-lookup"><span data-stu-id="ee246-1240">No</span></span>    | <span data-ttu-id="ee246-1241">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-1241">See `^` and `$` above</span></span> |

<span data-ttu-id="ee246-1242">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1242">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ee246-1243">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1243">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ee246-1244">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-1244">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ee246-1245">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1245">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ee246-1246">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1246">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="ee246-1247">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="ee246-1247">Custom route constraints</span></span>

<span data-ttu-id="ee246-1248">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="ee246-1248">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ee246-1249"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1249">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ee246-1250">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="ee246-1250">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="ee246-1251"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="ee246-1251">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="ee246-1252">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ee246-1252">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ee246-1253">例如：</span><span class="sxs-lookup"><span data-stu-id="ee246-1253">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="ee246-1254">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1254">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="ee246-1255">例如：</span><span class="sxs-lookup"><span data-stu-id="ee246-1255">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="ee246-1256">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1256">Parameter transformer reference</span></span>

<span data-ttu-id="ee246-1257">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="ee246-1257">Parameter transformers:</span></span>

* <span data-ttu-id="ee246-1258">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="ee246-1258">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="ee246-1259">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1259">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="ee246-1260">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="ee246-1260">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="ee246-1261">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1261">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="ee246-1262">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1262">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="ee246-1263">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1263">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="ee246-1264">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="ee246-1264">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="ee246-1265">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-1265">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="ee246-1266">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1266">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="ee246-1267">使用上述的路由，動作 `SubscriptionManagementController.GetAll` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="ee246-1267">With the preceding route, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="ee246-1268">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1268">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="ee246-1269">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1269">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="ee246-1270">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="ee246-1270">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="ee246-1271">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ee246-1271">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="ee246-1272">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1272">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="ee246-1273">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1273">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="ee246-1274">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1274">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="ee246-1275">Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ee246-1275">Razor Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="ee246-1276">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="ee246-1276">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="ee246-1277">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-1277">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="ee246-1278">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1278">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="ee246-1279">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1279">URL generation reference</span></span>

<span data-ttu-id="ee246-1280">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-1280">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="ee246-1281">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1281">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="ee246-1282">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1282">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="ee246-1283">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1283">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="ee246-1284"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1284">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="ee246-1285">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1285">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="ee246-1286">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1286">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ee246-1287">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1287">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="ee246-1288">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ee246-1288">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="ee246-1289">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1289">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="ee246-1290">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="ee246-1290">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="ee246-1291">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1291">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ee246-1292">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-1292">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ee246-1293">環境值</span><span class="sxs-lookup"><span data-stu-id="ee246-1293">Ambient Values</span></span>                     | <span data-ttu-id="ee246-1294">明確值</span><span class="sxs-lookup"><span data-stu-id="ee246-1294">Explicit Values</span></span>                        | <span data-ttu-id="ee246-1295">結果</span><span class="sxs-lookup"><span data-stu-id="ee246-1295">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ee246-1296">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1296">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1297">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1297">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-1298">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1298">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1299">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1299">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ee246-1300">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-1300">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ee246-1301">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1301">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-1302">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1302">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1303">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-1303">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="ee246-1304">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="ee246-1304">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="ee246-1305">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-1305">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="ee246-1306">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ee246-1306">Complex segments</span></span>

<span data-ttu-id="ee246-1307">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1307">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="ee246-1308">請參閱[此程式碼](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="ee246-1308">See [this code](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="ee246-1309">[程式法範例](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="ee246-1309">The [code sample](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ee246-1310">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1310">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="ee246-1311">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1311">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ee246-1312">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1312">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ee246-1313">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1313">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

<span data-ttu-id="ee246-1314">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ee246-1314">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> <span data-ttu-id="ee246-1315">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1315">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="ee246-1316">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1316">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="ee246-1317">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1317">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ee246-1318">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ee246-1318">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ee246-1319">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ee246-1319">Routing basics</span></span>

<span data-ttu-id="ee246-1320">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="ee246-1320">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="ee246-1321">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="ee246-1321">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="ee246-1322">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="ee246-1322">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="ee246-1323">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="ee246-1323">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="ee246-1324">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1324">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="ee246-1325">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1325">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="ee246-1326">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1326">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="ee246-1327">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="ee246-1327">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="ee246-1328">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="ee246-1328">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="ee246-1329">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1329">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="ee246-1330">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1330">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ee246-1331">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1331">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="ee246-1332">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1332">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="ee246-1333">路由會使用的<xref:Microsoft.AspNetCore.Routing.IRouter>路由執行來：</span><span class="sxs-lookup"><span data-stu-id="ee246-1333">Routing uses routes implementations of <xref:Microsoft.AspNetCore.Routing.IRouter> to:</span></span>

* <span data-ttu-id="ee246-1334">將傳入要求對應至「路由處理常式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1334">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="ee246-1335">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1335">Generate the URLs used in responses.</span></span>

<span data-ttu-id="ee246-1336">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1336">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="ee246-1337">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1337">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="ee246-1338">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1338">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="ee246-1339">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1339">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="ee246-1340">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ee246-1340">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ee246-1341">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1341">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ee246-1342">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="ee246-1342">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ee246-1343"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1343"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ee246-1344">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="ee246-1344">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ee246-1345">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1345">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ee246-1346">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="ee246-1346">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="ee246-1347"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1347"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
<span data-ttu-id="ee246-1348">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="ee246-1348">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="ee246-1349">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1349">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="ee246-1350">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1350">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="ee246-1351">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ee246-1351">URL matching</span></span>

<span data-ttu-id="ee246-1352">URL 比對是路由用來將傳入要求分派給「處理常式」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-1352">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="ee246-1353">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-1353">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ee246-1354">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ee246-1354">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ee246-1355">傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="ee246-1355">Incoming requests enter the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="ee246-1356"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」\*\* 要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1356">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="ee246-1357">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1357">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="ee246-1358">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee246-1358">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="ee246-1359"><xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1359">The primary input to <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="ee246-1360">[RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-1360">The [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="ee246-1361">呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1361">A match that calls <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> also sets the properties of the [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="ee246-1362">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1362">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="ee246-1363">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-1363">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="ee246-1364">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="ee246-1364">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="ee246-1365">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-1365"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="ee246-1366">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1366">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="ee246-1367">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="ee246-1367">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="ee246-1368">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="ee246-1368">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="ee246-1369">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="ee246-1369">Routes can be nested inside of one another.</span></span> <span data-ttu-id="ee246-1370"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-1370">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="ee246-1371">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1371">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="ee246-1372"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1372">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation"></a><span data-ttu-id="ee246-1373">URL 產生</span><span class="sxs-lookup"><span data-stu-id="ee246-1373">URL generation</span></span>

<span data-ttu-id="ee246-1374">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-1374">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ee246-1375">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-1375">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="ee246-1376">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1376">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="ee246-1377">每個「路由」\*\* 會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="ee246-1377">Each *route* has its <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="ee246-1378"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="ee246-1378">The primary inputs to <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> are:</span></span>

* [<span data-ttu-id="ee246-1379">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="ee246-1379">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [<span data-ttu-id="ee246-1380">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="ee246-1380">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [<span data-ttu-id="ee246-1381">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="ee246-1381">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

<span data-ttu-id="ee246-1382">路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1382">Routes primarily use the route values provided by <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> and <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="ee246-1383"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1383">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="ee246-1384">相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1384">In contrast, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="ee246-1385">提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="ee246-1385">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="ee246-1386">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1386">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="ee246-1387">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1387">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="ee246-1388"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1388">The output of <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is a <xref:Microsoft.AspNetCore.Routing.VirtualPathData>.</span></span> <span data-ttu-id="ee246-1389"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1389"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> is a parallel of <xref:Microsoft.AspNetCore.Routing.RouteData>.</span></span> <span data-ttu-id="ee246-1390"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1390"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> contains the <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="ee246-1391">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1391">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="ee246-1392">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-1392">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="ee246-1393">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-1393">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="ee246-1394">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="ee246-1394">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="ee246-1395">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="ee246-1395">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="ee246-1396">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1396">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

### <a name="create-routes"></a><span data-ttu-id="ee246-1397">建立路由</span><span class="sxs-lookup"><span data-stu-id="ee246-1397">Create routes</span></span>

<span data-ttu-id="ee246-1398">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="ee246-1398">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="ee246-1399">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」\*\* 語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1399"><xref:Microsoft.AspNetCore.Routing.Route> uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="ee246-1400">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1400"><xref:Microsoft.AspNetCore.Routing.Route> uses the same route template to generate a URL when <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is called.</span></span>

<span data-ttu-id="ee246-1401">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1401">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="ee246-1402">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1402">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="ee246-1403"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1403"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ee246-1404"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1404"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ee246-1405">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1405">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="ee246-1406">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1406">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="ee246-1407">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1407">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="ee246-1408">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="ee246-1408">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ee246-1409">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1409">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="ee246-1410">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1410">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="ee246-1411">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1411">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="ee246-1412">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="ee246-1412">Route parameters are named.</span></span> <span data-ttu-id="ee246-1413">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="ee246-1413">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="ee246-1414">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1414">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="ee246-1415">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1415">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="ee246-1416">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1416">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="ee246-1417">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1417">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="ee246-1418">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1418">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="ee246-1419">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1419">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="ee246-1420">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1420">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="ee246-1421">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="ee246-1421">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="ee246-1422">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1422">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="ee246-1423">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ee246-1423">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="ee246-1424">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1424">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="ee246-1425">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1425">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="ee246-1426"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1426">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="ee246-1427">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-1427">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="ee246-1428">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1428">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="ee246-1429">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="ee246-1429">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="ee246-1430">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1430">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="ee246-1431">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="ee246-1431">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ee246-1432">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1432">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ee246-1433">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1433">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ee246-1434">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1434">Default values can be specified in the route template.</span></span> <span data-ttu-id="ee246-1435">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1435">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="ee246-1436">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1436">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="ee246-1437">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="ee246-1437">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="ee246-1438">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1438">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="ee246-1440">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="ee246-1440">Route class URL generation</span></span>

<span data-ttu-id="ee246-1441"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1441">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="ee246-1442">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="ee246-1442">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="ee246-1443">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1443">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="ee246-1444">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="ee246-1444">What values would be produced?</span></span> <span data-ttu-id="ee246-1445">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="ee246-1445">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="ee246-1446">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1446">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ee246-1447">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1447">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="ee246-1448">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="ee246-1448">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="ee246-1449">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ee246-1449">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="ee246-1450">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1450">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="ee246-1451">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="ee246-1451">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="ee246-1452">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1452">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ee246-1453">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="ee246-1453">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="ee246-1454">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1454">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="ee246-1455">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="ee246-1455">Use routing middleware</span></span>

<span data-ttu-id="ee246-1456">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1456">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="ee246-1457">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ee246-1457">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="ee246-1458">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="ee246-1458">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="ee246-1459">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="ee246-1459">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="ee246-1460"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>&ndash;僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1460"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="ee246-1461">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="ee246-1461">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="ee246-1462">URI</span><span class="sxs-lookup"><span data-stu-id="ee246-1462">URI</span></span>                    | <span data-ttu-id="ee246-1463">回應</span><span class="sxs-lookup"><span data-stu-id="ee246-1463">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="ee246-1464">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1464">Hello!</span></span> <span data-ttu-id="ee246-1465">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1465">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="ee246-1466">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1466">Hello!</span></span> <span data-ttu-id="ee246-1467">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1467">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="ee246-1468">Hello!</span><span class="sxs-lookup"><span data-stu-id="ee246-1468">Hello!</span></span> <span data-ttu-id="ee246-1469">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ee246-1469">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="ee246-1470">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1470">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="ee246-1471">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="ee246-1471">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="ee246-1472">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="ee246-1472">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="ee246-1473">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1473">The request falls through, no match.</span></span>              |

<span data-ttu-id="ee246-1474">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1474">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="ee246-1475">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1475">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

<span data-ttu-id="ee246-1476">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="ee246-1476">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="ee246-1477">其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1477">Some of listed methods, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, require a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="ee246-1478">路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」\*\* 使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-1478">The <xref:Microsoft.AspNetCore.Http.RequestDelegate> is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="ee246-1479">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="ee246-1479">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="ee246-1480">如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1480">If the `Map*` method doesn't accept a handler, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

<span data-ttu-id="ee246-1481">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="ee246-1481">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="ee246-1482">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1482">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="ee246-1483">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1483">Route template reference</span></span>

<span data-ttu-id="ee246-1484">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1484">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="ee246-1485">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-1485">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="ee246-1486">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1486">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="ee246-1487">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1487">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ee246-1488">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ee246-1488">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ee246-1489">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="ee246-1489">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="ee246-1490">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-1490">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="ee246-1491">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ee246-1491">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ee246-1492">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ee246-1492">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ee246-1493">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1493">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ee246-1494">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-1494">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="ee246-1495">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ee246-1495">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ee246-1496">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ee246-1496">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ee246-1497">這稱為「全部擷取」\*\* 參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1497">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="ee246-1498">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ee246-1498">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ee246-1499">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1499">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ee246-1500">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1500">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ee246-1501">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1501">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ee246-1502">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ee246-1502">Note the escaped forward slash.</span></span>

<span data-ttu-id="ee246-1503">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ee246-1503">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ee246-1504">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1504">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ee246-1505">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1505">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ee246-1506">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="ee246-1506">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="ee246-1507">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1507">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ee246-1508">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1508">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ee246-1509">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee246-1509">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="ee246-1510">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="ee246-1510">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="ee246-1511">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="ee246-1511">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="ee246-1512">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1512">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ee246-1513">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-1513">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ee246-1514">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ee246-1514">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ee246-1515">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1515">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="ee246-1516">路由範本</span><span class="sxs-lookup"><span data-stu-id="ee246-1516">Route Template</span></span>                           | <span data-ttu-id="ee246-1517">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ee246-1517">Example Matching URI</span></span>    | <span data-ttu-id="ee246-1518">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ee246-1518">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ee246-1519">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1519">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ee246-1520">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1520">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ee246-1521">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1521">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ee246-1522">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ee246-1522">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ee246-1523">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1523">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ee246-1524">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1524">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="ee246-1525">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1525">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ee246-1526">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ee246-1526">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="ee246-1527">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1527">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="ee246-1528">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1528">Route constraint reference</span></span>

<span data-ttu-id="ee246-1529">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ee246-1529">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ee246-1530">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="ee246-1530">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="ee246-1531">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1531">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ee246-1532">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ee246-1532">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ee246-1533">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ee246-1533">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ee246-1534">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ee246-1534">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="ee246-1535">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ee246-1535">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="ee246-1536">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ee246-1536">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ee246-1537">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="ee246-1537">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="ee246-1538">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="ee246-1538">constraint</span></span> | <span data-ttu-id="ee246-1539">範例</span><span class="sxs-lookup"><span data-stu-id="ee246-1539">Example</span></span> | <span data-ttu-id="ee246-1540">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1540">Example Matches</span></span> | <span data-ttu-id="ee246-1541">備忘錄</span><span class="sxs-lookup"><span data-stu-id="ee246-1541">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ee246-1542">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-1542">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-1543">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="ee246-1543">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ee246-1544">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ee246-1544">`true`, `FALSE`</span></span> | <span data-ttu-id="ee246-1545">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ee246-1545">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ee246-1546">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ee246-1546">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ee246-1547">符合不因`DateTime`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1547">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1548">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1548">See  preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="ee246-1549">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ee246-1549">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ee246-1550">符合不因`decimal`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1550">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1551">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1551">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ee246-1552">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-1552">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-1553">符合不因`double`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1553">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1554">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1554">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ee246-1555">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ee246-1555">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ee246-1556">符合不因`float`文化特性而異的有效值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1556">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ee246-1557">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ee246-1557">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="ee246-1558">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ee246-1558">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ee246-1559">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="ee246-1559">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ee246-1560">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ee246-1560">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ee246-1561">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="ee246-1561">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ee246-1562">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-1562">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="ee246-1563">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-1563">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ee246-1564">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-1564">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ee246-1565">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="ee246-1565">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ee246-1566">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="ee246-1566">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ee246-1567">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ee246-1567">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ee246-1568">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ee246-1568">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ee246-1569">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ee246-1569">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ee246-1570">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="ee246-1570">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ee246-1571">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="ee246-1571">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="ee246-1572">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1572">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ee246-1573">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ee246-1573">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ee246-1574">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="ee246-1574">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ee246-1575">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ee246-1575">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="ee246-1576">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1576">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ee246-1577">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1577">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ee246-1578">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ee246-1578">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="ee246-1579">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1579">Regular expressions</span></span>

<span data-ttu-id="ee246-1580">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1580">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ee246-1581">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ee246-1581">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ee246-1582">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ee246-1582">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="ee246-1583">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ee246-1583">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ee246-1584">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="ee246-1584">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="ee246-1585">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="ee246-1585">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="ee246-1586">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="ee246-1586">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="ee246-1587">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1587">Regular Expression</span></span>    | <span data-ttu-id="ee246-1588">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1588">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ee246-1589">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ee246-1589">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="ee246-1590">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ee246-1590">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="ee246-1591">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1591">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="ee246-1592">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-1592">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ee246-1593">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="ee246-1593">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="ee246-1594">運算是</span><span class="sxs-lookup"><span data-stu-id="ee246-1594">Expression</span></span>   | <span data-ttu-id="ee246-1595">字串</span><span class="sxs-lookup"><span data-stu-id="ee246-1595">String</span></span>    | <span data-ttu-id="ee246-1596">比對</span><span class="sxs-lookup"><span data-stu-id="ee246-1596">Match</span></span> | <span data-ttu-id="ee246-1597">註解</span><span class="sxs-lookup"><span data-stu-id="ee246-1597">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1598">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-1598">hello</span></span>     | <span data-ttu-id="ee246-1599">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1599">Yes</span></span>   | <span data-ttu-id="ee246-1600">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1600">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1601">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-1601">123abc456</span></span> | <span data-ttu-id="ee246-1602">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1602">Yes</span></span>   | <span data-ttu-id="ee246-1603">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ee246-1603">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1604">mz</span><span class="sxs-lookup"><span data-stu-id="ee246-1604">mz</span></span>        | <span data-ttu-id="ee246-1605">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1605">Yes</span></span>   | <span data-ttu-id="ee246-1606">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ee246-1606">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ee246-1607">MZ</span><span class="sxs-lookup"><span data-stu-id="ee246-1607">MZ</span></span>        | <span data-ttu-id="ee246-1608">是</span><span class="sxs-lookup"><span data-stu-id="ee246-1608">Yes</span></span>   | <span data-ttu-id="ee246-1609">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ee246-1609">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-1610">hello</span><span class="sxs-lookup"><span data-stu-id="ee246-1610">hello</span></span>     | <span data-ttu-id="ee246-1611">否</span><span class="sxs-lookup"><span data-stu-id="ee246-1611">No</span></span>    | <span data-ttu-id="ee246-1612">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-1612">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ee246-1613">123abc456</span><span class="sxs-lookup"><span data-stu-id="ee246-1613">123abc456</span></span> | <span data-ttu-id="ee246-1614">否</span><span class="sxs-lookup"><span data-stu-id="ee246-1614">No</span></span>    | <span data-ttu-id="ee246-1615">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ee246-1615">See `^` and `$` above</span></span> |

<span data-ttu-id="ee246-1616">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1616">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ee246-1617">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1617">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ee246-1618">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ee246-1618">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ee246-1619">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1619">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ee246-1620">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ee246-1620">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="ee246-1621">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="ee246-1621">Custom Route Constraints</span></span>

<span data-ttu-id="ee246-1622">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="ee246-1622">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ee246-1623"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1623">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ee246-1624">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="ee246-1624">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="ee246-1625"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="ee246-1625">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="ee246-1626">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ee246-1626">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ee246-1627">例如：</span><span class="sxs-lookup"><span data-stu-id="ee246-1627">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="ee246-1628">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="ee246-1628">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="ee246-1629">例如：</span><span class="sxs-lookup"><span data-stu-id="ee246-1629">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a><span data-ttu-id="ee246-1630">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ee246-1630">URL generation reference</span></span>

<span data-ttu-id="ee246-1631">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-1631">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="ee246-1632">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="ee246-1632">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="ee246-1633">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1633">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="ee246-1634">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="ee246-1634">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="ee246-1635"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="ee246-1635">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="ee246-1636">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="ee246-1636">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="ee246-1637">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1637">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ee246-1638">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1638">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="ee246-1639">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ee246-1639">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="ee246-1640">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="ee246-1640">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="ee246-1641">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="ee246-1641">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="ee246-1642">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ee246-1642">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ee246-1643">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ee246-1643">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ee246-1644">環境值</span><span class="sxs-lookup"><span data-stu-id="ee246-1644">Ambient Values</span></span>                     | <span data-ttu-id="ee246-1645">明確值</span><span class="sxs-lookup"><span data-stu-id="ee246-1645">Explicit Values</span></span>                        | <span data-ttu-id="ee246-1646">結果</span><span class="sxs-lookup"><span data-stu-id="ee246-1646">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ee246-1647">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1647">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1648">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1648">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-1649">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1649">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1650">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1650">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ee246-1651">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-1651">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ee246-1652">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ee246-1652">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ee246-1653">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ee246-1653">controller = "Home"</span></span>                | <span data-ttu-id="ee246-1654">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ee246-1654">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="ee246-1655">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="ee246-1655">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="ee246-1656">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ee246-1656">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="ee246-1657">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ee246-1657">Complex segments</span></span>

<span data-ttu-id="ee246-1658">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="ee246-1658">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="ee246-1659">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="ee246-1659">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="ee246-1660">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="ee246-1660">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>

::: moniker-end
