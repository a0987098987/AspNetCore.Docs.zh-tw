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
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/routing
ms.openlocfilehash: 25464817314f79c5bfd11d982cc9b09a3c72df15
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060341"
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="ba7fe-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="ba7fe-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="ba7fe-104">By [Ryan Nowak](https://github.com/rynowak)、 [Kirk Larkin](https://twitter.com/serpent5)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ba7fe-105">路由會負責比對傳入 HTTP 要求，並將這些要求分派至應用程式的可執行端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-105">Routing is responsible for matching incoming HTTP requests and dispatching those requests to the app's executable endpoints.</span></span> <span data-ttu-id="ba7fe-106">[端點](#endpoint)是應用程式的可執行要求處理常式代碼單位。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-106">[Endpoints](#endpoint) are the app's units of executable request-handling code.</span></span> <span data-ttu-id="ba7fe-107">端點會在應用程式中定義，並在應用程式啟動時設定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-107">Endpoints are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ba7fe-108">端點比對程式可以從要求的 URL 中解壓縮值，並提供這些值來進行要求處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-108">The endpoint matching process can extract values from the request's URL and provide those values for request processing.</span></span> <span data-ttu-id="ba7fe-109">使用來自應用程式的端點資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-109">Using endpoint information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="ba7fe-110">應用程式可以使用下列內容來設定路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-110">Apps can configure routing using:</span></span>

- <span data-ttu-id="ba7fe-111">控制器</span><span class="sxs-lookup"><span data-stu-id="ba7fe-111">Controllers</span></span>
- Razor<span data-ttu-id="ba7fe-112">頁面</span><span class="sxs-lookup"><span data-stu-id="ba7fe-112"> Pages</span></span>
- SignalR
- <span data-ttu-id="ba7fe-113">gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="ba7fe-113">gRPC Services</span></span>
- <span data-ttu-id="ba7fe-114">端點啟用的[中介軟體](xref:fundamentals/middleware/index)，例如[健康情況檢查](xref:host-and-deploy/health-checks)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-114">Endpoint-enabled [middleware](xref:fundamentals/middleware/index) such as [Health Checks](xref:host-and-deploy/health-checks).</span></span>
- <span data-ttu-id="ba7fe-115">已向路由註冊的委派和 lambda。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-115">Delegates and lambdas registered with routing.</span></span>

<span data-ttu-id="ba7fe-116">本檔涵蓋 ASP.NET Core 路由的低層級詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-116">This document covers low-level details of ASP.NET Core routing.</span></span> <span data-ttu-id="ba7fe-117">如需設定路由的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-117">For information on configuring routing:</span></span>

* <span data-ttu-id="ba7fe-118">針對控制器，請參閱 <xref:mvc/controllers/routing> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-118">For controllers, see <xref:mvc/controllers/routing>.</span></span>
* <span data-ttu-id="ba7fe-119">如需 Razor 頁面慣例，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-119">For Razor Pages conventions, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ba7fe-120">本檔中所述的端點路由系統適用于 ASP.NET Core 3.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-120">The endpoint routing system described in this document applies to ASP.NET Core 3.0 and later.</span></span> <span data-ttu-id="ba7fe-121">如需以為基礎的先前路由系統的詳細資訊 <xref:Microsoft.AspNetCore.Routing.IRouter> ，請使用下列其中一種方法來選取 ASP.NET Core 2.1 版本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-121">For information on the previous routing system based on <xref:Microsoft.AspNetCore.Routing.IRouter>, select the ASP.NET Core 2.1 version using one of the following approaches:</span></span>

* <span data-ttu-id="ba7fe-122">先前版本的版本選取器。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-122">The version selector for a previous version.</span></span>
* <span data-ttu-id="ba7fe-123">選取 [ [ASP.NET Core 2.1 路由](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)]。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-123">Select [ASP.NET Core 2.1 routing](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="ba7fe-124">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ba7fe-124">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ba7fe-125">此檔的下載範例會由特定 `Startup` 類別啟用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-125">The download samples for this document are enabled by a specific `Startup` class.</span></span> <span data-ttu-id="ba7fe-126">若要執行特定的範例，請修改*Program.cs*以呼叫所需的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-126">To run a specific sample, modify *Program.cs* to call the desired `Startup` class.</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ba7fe-127">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ba7fe-127">Routing basics</span></span>

<span data-ttu-id="ba7fe-128">所有 ASP.NET Core 範本都會在產生的程式碼中包含路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-128">All ASP.NET Core templates include routing in the generated code.</span></span> <span data-ttu-id="ba7fe-129">路由是在的[中介軟體](xref:fundamentals/middleware/index)管線中註冊 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-129">Routing is registered in the [middleware](xref:fundamentals/middleware/index) pipeline in `Startup.Configure`.</span></span>

<span data-ttu-id="ba7fe-130">下列程式碼顯示路由的基本範例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-130">The following code shows a basic example of routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

<span data-ttu-id="ba7fe-131">路由會使用一對由和註冊的中介軟體 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-131">Routing uses a pair of middleware, registered by <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>:</span></span>

* <span data-ttu-id="ba7fe-132">`UseRouting`將路由對應新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-132">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="ba7fe-133">此中介軟體會查看應用程式中定義的端點集合，並根據要求選取[最符合的項](#urlm)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-133">This middleware looks at the set of endpoints defined in the app, and selects the [best match](#urlm) based on the request.</span></span>
* <span data-ttu-id="ba7fe-134">`UseEndpoints`將端點執行新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-134">`UseEndpoints` adds endpoint execution to the middleware pipeline.</span></span> <span data-ttu-id="ba7fe-135">它會執行與所選端點相關聯的委派。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-135">It runs the delegate associated with the selected endpoint.</span></span>

<span data-ttu-id="ba7fe-136">上述範例包含使用[MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*)方法*對程式碼*端點的單一路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-136">The preceding example includes a single *route to code* endpoint using the [MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*) method:</span></span>

* <span data-ttu-id="ba7fe-137">當 HTTP `GET` 要求傳送至根 URL 時 `/` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-137">When an HTTP `GET` request is sent to the root URL `/`:</span></span>
  * <span data-ttu-id="ba7fe-138">所顯示的要求委派會執行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-138">The request delegate shown executes.</span></span>
  * <span data-ttu-id="ba7fe-139">`Hello World!`會寫入 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-139">`Hello World!` is written to the HTTP response.</span></span> <span data-ttu-id="ba7fe-140">根據預設，根 URL `/` 是 `https://localhost:5001/` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-140">By default, the root URL `/` is `https://localhost:5001/`.</span></span>
* <span data-ttu-id="ba7fe-141">如果要求方法不是 `GET` ，或根 URL 不是 `/` ，則不會符合任何路由，且會傳回 HTTP 404。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-141">If the request method is not `GET` or the root URL is not `/`, no route matches and an HTTP 404 is returned.</span></span>

### <a name="endpoint"></a><span data-ttu-id="ba7fe-142">端點</span><span class="sxs-lookup"><span data-stu-id="ba7fe-142">Endpoint</span></span>

<a name="endpoint"></a>

<span data-ttu-id="ba7fe-143">`MapGet`方法是用來定義**端點**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-143">The `MapGet` method is used to define an **endpoint**.</span></span> <span data-ttu-id="ba7fe-144">端點可以是：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-144">An endpoint is something that can be:</span></span>

* <span data-ttu-id="ba7fe-145">已選取，藉由比對 URL 和 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-145">Selected, by matching the URL and HTTP method.</span></span>
* <span data-ttu-id="ba7fe-146">執行委派。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-146">Executed, by running the delegate.</span></span>

<span data-ttu-id="ba7fe-147">應用程式可比對並執行的端點會在中設定 `UseEndpoints` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-147">Endpoints that can be matched and executed by the app are configured in `UseEndpoints`.</span></span> <span data-ttu-id="ba7fe-148">例如，、 <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*> <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*> 和類似的[方法](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions)會將要求委派連接至路由系統。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-148">For example, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>, and [similar methods](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions) connect request delegates to the routing system.</span></span>
<span data-ttu-id="ba7fe-149">您可以使用其他方法，將 ASP.NET Core framework 功能連接到路由系統：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-149">Additional methods can be used to connect ASP.NET Core framework features to the routing system:</span></span>
- <span data-ttu-id="ba7fe-150">[頁面的 MapRazorPages Razor](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-150">[MapRazorPages for Razor Pages](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)</span></span>
- [<span data-ttu-id="ba7fe-151">適用于控制器的 MapControllers</span><span class="sxs-lookup"><span data-stu-id="ba7fe-151">MapControllers for controllers</span></span>](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- <span data-ttu-id="ba7fe-152">[\<THub>適用于的 MapHubSignalR](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-152">[MapHub\<THub> for SignalR](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*)</span></span> 
- [<span data-ttu-id="ba7fe-153">\<TService>適用于 gRPC 的 MapGrpcService</span><span class="sxs-lookup"><span data-stu-id="ba7fe-153">MapGrpcService\<TService> for gRPC</span></span>](xref:grpc/aspnetcore)

<span data-ttu-id="ba7fe-154">下列範例顯示具有更複雜路由範本的路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-154">The following example shows routing with a more sophisticated route template:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-155">此字串 `/hello/{name:alpha}` 為**路由範本**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-155">The string `/hello/{name:alpha}` is a **route template**.</span></span> <span data-ttu-id="ba7fe-156">它是用來設定端點的相符方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-156">It is used to configure how the endpoint is matched.</span></span> <span data-ttu-id="ba7fe-157">在此情況下，範本會符合：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-157">In this case, the template matches:</span></span>

* <span data-ttu-id="ba7fe-158">URL，例如`/hello/Ryan`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-158">A URL like `/hello/Ryan`</span></span>
* <span data-ttu-id="ba7fe-159">開頭為的任何 URL 路徑， `/hello/` 後面接著一連串的字母字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-159">Any URL path that begins with `/hello/` followed by a sequence of alphabetic characters.</span></span>  <span data-ttu-id="ba7fe-160">`:alpha`套用僅符合字母字元的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-160">`:alpha` applies a route constraint that matches only alphabetic characters.</span></span> <span data-ttu-id="ba7fe-161">本檔稍後會說明[路由條件約束](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-161">[Route constraints](#route-constraint-reference) are explained later in this document.</span></span>

<span data-ttu-id="ba7fe-162">URL 路徑的第二個區段 `{name:alpha}` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-162">The second segment of the URL path, `{name:alpha}`:</span></span>

* <span data-ttu-id="ba7fe-163">已系結至 `name` 參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-163">Is bound to the `name` parameter.</span></span>
* <span data-ttu-id="ba7fe-164">會被捕捉並儲存在[HttpRequest 中。 RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-164">Is captured and stored in [HttpRequest.RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*).</span></span>

<span data-ttu-id="ba7fe-165">本檔中所述的端點路由系統是 ASP.NET Core 3.0 的新功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-165">The endpoint routing system described in this document is new as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="ba7fe-166">不過，ASP.NET Core 的所有版本都支援一組相同的路由範本功能和路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-166">However, all versions of ASP.NET Core support the same set of route template features and route constraints.</span></span>

<span data-ttu-id="ba7fe-167">下列範例顯示具有健康情況[檢查](xref:host-and-deploy/health-checks)和授權的路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-167">The following example shows routing with [health checks](xref:host-and-deploy/health-checks) and authorization:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="ba7fe-168">上述範例示範如何：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-168">The preceding example demonstrates how:</span></span>

* <span data-ttu-id="ba7fe-169">授權中介軟體可以與路由搭配使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-169">The authorization middleware can be used with routing.</span></span>
* <span data-ttu-id="ba7fe-170">端點可以用來設定授權行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-170">Endpoints can be used to configure authorization behavior.</span></span>

<span data-ttu-id="ba7fe-171"><xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*>呼叫會新增健康情況檢查端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-171">The <xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*> call adds a health check endpoint.</span></span> <span data-ttu-id="ba7fe-172">此呼叫的連結會將 <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> 授權原則附加至端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-172">Chaining <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> on to this call attaches an authorization policy to the endpoint.</span></span>

<span data-ttu-id="ba7fe-173">呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> 新增驗證和授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-173">Calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> adds the authentication and authorization middleware.</span></span> <span data-ttu-id="ba7fe-174">這些中介軟體會放在 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> 和之間 `UseEndpoints` ，使其可以：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-174">These middleware are placed between <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and `UseEndpoints` so that they can:</span></span>

* <span data-ttu-id="ba7fe-175">查看所選取的端點 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-175">See which endpoint was selected by `UseRouting`.</span></span>
* <span data-ttu-id="ba7fe-176">將分派給端點之前，請先套用授權原則 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-176">Apply an authorization policy before <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> dispatches to the endpoint.</span></span>

<a name="metadata"></a>

### <a name="endpoint-metadata"></a><span data-ttu-id="ba7fe-177">端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="ba7fe-177">Endpoint metadata</span></span>

<span data-ttu-id="ba7fe-178">在上述範例中，有兩個端點，但只有健全狀況檢查端點已附加授權原則。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-178">In the preceding example, there are two endpoints, but only the health check endpoint has an authorization policy attached.</span></span> <span data-ttu-id="ba7fe-179">如果要求符合健康狀態檢查端點， `/healthz` 則會執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-179">If the request matches the health check endpoint, `/healthz`, an authorization check is performed.</span></span> <span data-ttu-id="ba7fe-180">這會示範端點可以附加額外的資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-180">This demonstrates that endpoints can have extra data attached to them.</span></span> <span data-ttu-id="ba7fe-181">這種額外的資料稱為端點**中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-181">This extra data is called endpoint **metadata**:</span></span>

* <span data-ttu-id="ba7fe-182">可由路由感知中介軟體處理中繼資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-182">The metadata can be processed by routing-aware middleware.</span></span>
* <span data-ttu-id="ba7fe-183">中繼資料可以是任何 .NET 型別。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-183">The metadata can be of any .NET type.</span></span>

## <a name="routing-concepts"></a><span data-ttu-id="ba7fe-184">路由概念</span><span class="sxs-lookup"><span data-stu-id="ba7fe-184">Routing concepts</span></span>

<span data-ttu-id="ba7fe-185">路由系統會藉由新增強大的**端點**概念，在中介軟體管線之上建立。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-185">The routing system builds on top of the middleware pipeline by adding the powerful **endpoint** concept.</span></span> <span data-ttu-id="ba7fe-186">端點代表應用程式功能的單位，在路由、授權及任何數目的 ASP.NET Core 系統之間彼此不同。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-186">Endpoints represent units of the app's functionality that are distinct from each other in terms of routing, authorization, and any number of ASP.NET Core's systems.</span></span>

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a><span data-ttu-id="ba7fe-187">ASP.NET Core 端點定義</span><span class="sxs-lookup"><span data-stu-id="ba7fe-187">ASP.NET Core endpoint definition</span></span>

<span data-ttu-id="ba7fe-188">ASP.NET Core 的端點為：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-188">An ASP.NET Core endpoint is:</span></span>

* <span data-ttu-id="ba7fe-189">可執行檔：具有 <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-189">Executable: Has a <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>.</span></span>
* <span data-ttu-id="ba7fe-190">可擴充：具有[中繼資料](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*)集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-190">Extensible: Has a [Metadata](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*) collection.</span></span>
* <span data-ttu-id="ba7fe-191">可選取：選擇性地擁有[路由資訊](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-191">Selectable: Optionally, has [routing information](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*).</span></span>
* <span data-ttu-id="ba7fe-192">可列舉：藉由從 DI 抓取來列出端點的 <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> 集合[DI](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-192">Enumerable: The collection of endpoints can be listed by retrieving the <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> from [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ba7fe-193">下列程式碼示範如何抓取和檢查符合目前要求的端點：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-193">The following code shows how to retrieve and inspect the endpoint matching the current request:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-194">若已選取，則可以從抓取端點 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-194">The endpoint, if selected, can be retrieved from the `HttpContext`.</span></span> <span data-ttu-id="ba7fe-195">可以檢查其屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-195">Its properties can be inspected.</span></span> <span data-ttu-id="ba7fe-196">端點物件是不可變的，而且無法在建立之後修改。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-196">Endpoint objects are immutable and cannot be modified after creation.</span></span> <span data-ttu-id="ba7fe-197">最常見的端點類型是 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-197">The most common type of endpoint is a <xref:Microsoft.AspNetCore.Routing.RouteEndpoint>.</span></span> <span data-ttu-id="ba7fe-198">`RouteEndpoint`包含可讓路由系統選取的資訊。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-198">`RouteEndpoint` includes information that allows it to be to selected by the routing system.</span></span>

<span data-ttu-id="ba7fe-199">在上述程式碼中， [app。使用](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*)會設定內嵌[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-199">In the preceding code, [app.Use](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*) configures an in-line [middleware](xref:fundamentals/middleware/index).</span></span>

<a name="mt"></a>

<span data-ttu-id="ba7fe-200">下列程式碼顯示，視管線中呼叫的位置而定， `app.Use` 可能不會有端點：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-200">The following code shows that, depending on where `app.Use` is called in the pipeline, there may not be an endpoint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-201">先前 `Console.WriteLine` 的範例會加入語句，以顯示是否已選取端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-201">This preceding sample adds `Console.WriteLine` statements that display whether or not an endpoint has been selected.</span></span> <span data-ttu-id="ba7fe-202">為了清楚起見，此範例會將顯示名稱指派給提供的 `/` 端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-202">For clarity, the sample assigns a display name to the provided `/` endpoint.</span></span>

<span data-ttu-id="ba7fe-203">以顯示的 URL 執行此程式碼 `/` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-203">Running this code with a URL of `/` displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

<span data-ttu-id="ba7fe-204">執行此程式碼時，會顯示任何其他 URL：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-204">Running this code with any other URL displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

<span data-ttu-id="ba7fe-205">此輸出示範：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-205">This output demonstrates that:</span></span>

* <span data-ttu-id="ba7fe-206">在呼叫之前，端點一律為 null `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-206">The endpoint is always null before `UseRouting` is called.</span></span>
* <span data-ttu-id="ba7fe-207">如果找到相符的，則與之間的端點為非 null `UseRouting` <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-207">If a match is found, the endpoint is non-null between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="ba7fe-208">`UseEndpoints`當找到相符的時，中介軟體就是**終端**機。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-208">The `UseEndpoints` middleware is **terminal** when a match is found.</span></span> <span data-ttu-id="ba7fe-209">[終端機中介軟體](#tm)會在本檔稍後定義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-209">[Terminal middleware](#tm) is defined later in this document.</span></span>
* <span data-ttu-id="ba7fe-210">`UseEndpoints`只有當找不到相符的時，才會在執行後中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-210">The middleware after `UseEndpoints` execute only when no match is found.</span></span>

<span data-ttu-id="ba7fe-211">`UseRouting`中介軟體會使用[task.setendpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*)方法，將端點附加至目前的內容。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-211">The `UseRouting` middleware uses the [SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*) method to attach the endpoint to the current context.</span></span> <span data-ttu-id="ba7fe-212">您可以 `UseRouting` 使用自訂邏輯來取代中介軟體，但仍可獲得使用端點的優點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-212">It's possible to replace the `UseRouting` middleware with custom logic and still get the benefits of using endpoints.</span></span> <span data-ttu-id="ba7fe-213">端點是低層級的基本型別，例如中介軟體，而且不會與路由執行結合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-213">Endpoints are a low-level primitive like middleware, and aren't coupled to the routing implementation.</span></span> <span data-ttu-id="ba7fe-214">大部分的應用程式不需要以 `UseRouting` 自訂邏輯取代。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-214">Most apps don't need to replace `UseRouting` with custom logic.</span></span>

<span data-ttu-id="ba7fe-215">`UseEndpoints`中介軟體是設計用來與中介軟體一起使用 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-215">The `UseEndpoints` middleware is designed to be used in tandem with the `UseRouting` middleware.</span></span> <span data-ttu-id="ba7fe-216">執行端點的核心邏輯並不複雜。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-216">The core logic to execute an endpoint isn't complicated.</span></span> <span data-ttu-id="ba7fe-217">使用 <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> 來取出端點，然後叫用其 <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> 屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-217">Use <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> to retrieve the endpoint, and then invoke its <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> property.</span></span>

<span data-ttu-id="ba7fe-218">下列程式碼示範中介軟體會如何影響或回應路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-218">The following code demonstrates how middleware can influence or react to routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-219">上述範例示範兩個重要概念：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-219">The preceding example demonstrates two important concepts:</span></span>

* <span data-ttu-id="ba7fe-220">中介軟體可以在前執行， `UseRouting` 以修改路由運作的資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-220">Middleware can run before `UseRouting` to modify the data that routing operates upon.</span></span>
    * <span data-ttu-id="ba7fe-221">通常在路由之前出現的中介軟體會修改要求的某些屬性，例如 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 、 <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*> 或 <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-221">Usually middleware that appears before routing modifies some property of the request, such as <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>, <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>, or <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>.</span></span>
* <span data-ttu-id="ba7fe-222">中介軟體可在 `UseRouting` 和之間執行， <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 以便在執行端點之前處理路由的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-222">Middleware can run between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> to process the results of routing before the endpoint is executed.</span></span>
    * <span data-ttu-id="ba7fe-223">在和之間執行的中介軟體 `UseRouting` `UseEndpoints` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-223">Middleware that runs between `UseRouting` and `UseEndpoints`:</span></span>
      * <span data-ttu-id="ba7fe-224">通常會檢查中繼資料以瞭解端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-224">Usually inspects metadata to understand the endpoints.</span></span>
      * <span data-ttu-id="ba7fe-225">通常會依照和完成安全性決策 `UseAuthorization` `UseCors` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-225">Often makes security decisions, as done by `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="ba7fe-226">中介軟體和中繼資料的組合，可讓您設定每個端點的原則。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-226">The combination of middleware and metadata allows configuring policies per-endpoint.</span></span>

<span data-ttu-id="ba7fe-227">上述程式碼顯示支援每個端點原則的自訂中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-227">The preceding code shows an example of a custom middleware that supports per-endpoint policies.</span></span> <span data-ttu-id="ba7fe-228">中介軟體會將敏感性資料之存取權的*審核記錄*寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-228">The middleware writes an *audit log* of access to sensitive data to the console.</span></span> <span data-ttu-id="ba7fe-229">中介軟體可以設定為使用中繼資料來*審核*端點 `AuditPolicyAttribute` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-229">The middleware can be configured to *audit* an endpoint with the `AuditPolicyAttribute` metadata.</span></span> <span data-ttu-id="ba7fe-230">這個範例會示範*加入宣告*模式，其中只會審核標示為機密的端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-230">This sample demonstrates an *opt-in* pattern where only endpoints that are marked as sensitive are audited.</span></span> <span data-ttu-id="ba7fe-231">例如，您可以反向定義此邏輯，以審核未標示為安全的所有專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-231">It's possible to define this logic in reverse, auditing everything that isn't marked as safe, for example.</span></span> <span data-ttu-id="ba7fe-232">端點中繼資料系統很有彈性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-232">The endpoint metadata system is flexible.</span></span> <span data-ttu-id="ba7fe-233">這個邏輯可以設計成符合使用案例的任何方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-233">This logic could be designed in whatever way suits the use case.</span></span>

<span data-ttu-id="ba7fe-234">先前的範例程式碼主要是用來示範端點的基本概念。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-234">The preceding sample code is intended to demonstrate the basic concepts of endpoints.</span></span> <span data-ttu-id="ba7fe-235">**此範例並非供生產環境使用**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-235">**The sample is not intended for production use**.</span></span> <span data-ttu-id="ba7fe-236">更完整的*audit 記錄*檔中介軟體版本如下：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-236">A more complete version of an *audit log* middleware would:</span></span>

* <span data-ttu-id="ba7fe-237">記錄到檔案或資料庫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-237">Log to a file or database.</span></span>
* <span data-ttu-id="ba7fe-238">包含詳細資料，例如使用者、IP 位址、機密端點的名稱等等。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-238">Include details such as the user, IP address, name of the sensitive endpoint, and more.</span></span>

<span data-ttu-id="ba7fe-239">稽核原則中繼資料 `AuditPolicyAttribute` 會定義為 `Attribute` ，以便更容易與以類別為基礎的架構（例如控制器和）搭配使用 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-239">The audit policy metadata `AuditPolicyAttribute` is defined as an `Attribute` for easier use with class-based frameworks such as controllers and SignalR.</span></span> <span data-ttu-id="ba7fe-240">使用*路由至程式碼*時：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-240">When using *route to code*:</span></span>

* <span data-ttu-id="ba7fe-241">中繼資料是使用產生器 API 所附加。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-241">Metadata is attached with a builder API.</span></span>
* <span data-ttu-id="ba7fe-242">以類別為基礎的架構在建立端點時，會在對應的方法和類別上包含所有屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-242">Class-based frameworks include all attributes on the corresponding method and class when creating endpoints.</span></span>

<span data-ttu-id="ba7fe-243">元資料類型的最佳作法是將它們定義為介面或屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-243">The best practices for metadata types are to define them either as interfaces or attributes.</span></span> <span data-ttu-id="ba7fe-244">介面和屬性允許程式碼重複使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-244">Interfaces and attributes allow code reuse.</span></span> <span data-ttu-id="ba7fe-245">中繼資料系統具有彈性，而且不會強加任何限制。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-245">The metadata system is flexible and doesn't impose any limitations.</span></span>

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a><span data-ttu-id="ba7fe-246">比較終端中介軟體和路由</span><span class="sxs-lookup"><span data-stu-id="ba7fe-246">Comparing a terminal middleware and routing</span></span>

<span data-ttu-id="ba7fe-247">下列程式碼範例會將中介軟體與使用路由進行對比：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-247">The following code sample contrasts using middleware with using routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-248">以顯示的中介軟體樣式 `Approach 1:` 是**終端中介軟體**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-248">The style of middleware shown with `Approach 1:` is **terminal middleware**.</span></span> <span data-ttu-id="ba7fe-249">這稱為「終端中介軟體」，因為它會執行比對作業：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-249">It's called terminal middleware because it does a matching operation:</span></span>

* <span data-ttu-id="ba7fe-250">前述範例中的比對作業 `Path == "/"` 適用于中介軟體和 `Path == "/Movie"` 路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-250">The matching operation in the preceding sample is `Path == "/"` for the middleware and `Path == "/Movie"` for routing.</span></span>
* <span data-ttu-id="ba7fe-251">當相符項成功時，它會執行一些功能並傳回，而不是叫用 `next` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-251">When a match is successful, it executes some functionality and returns, rather than invoking the `next` middleware.</span></span>

<span data-ttu-id="ba7fe-252">這稱為「終端中介軟體」，因為它會終止搜尋、執行一些功能，然後傳回。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-252">It's called terminal middleware because it terminates the search, executes some functionality, and then returns.</span></span>

<span data-ttu-id="ba7fe-253">比較終端機中介軟體和路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-253">Comparing a terminal middleware and routing:</span></span>
* <span data-ttu-id="ba7fe-254">這兩種方法都允許終止處理管線：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-254">Both approaches allow terminating the processing pipeline:</span></span>
    * <span data-ttu-id="ba7fe-255">中介軟體會藉由傳回而不是叫用來終止管線 `next` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-255">Middleware terminates the pipeline by returning rather than invoking `next`.</span></span>
    * <span data-ttu-id="ba7fe-256">端點一律是 [終端機]。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-256">Endpoints are always terminal.</span></span>
* <span data-ttu-id="ba7fe-257">終端中介軟體允許將中介軟體放在管線中的任意位置：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-257">Terminal middleware allows positioning the middleware at an arbitrary place in the pipeline:</span></span>
    * <span data-ttu-id="ba7fe-258">端點會在的位置執行 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-258">Endpoints execute at the position of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="ba7fe-259">終端中介軟體可讓任意程式碼判斷中介軟體何時符合：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-259">Terminal middleware allows arbitrary code to determine when the middleware matches:</span></span>
    * <span data-ttu-id="ba7fe-260">自訂路由比對程式碼可能很詳細，而且很容易正確寫入。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-260">Custom route matching code can be verbose and difficult to write correctly.</span></span>
    * <span data-ttu-id="ba7fe-261">路由為一般應用程式提供直接的解決方案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-261">Routing provides straightforward solutions for typical apps.</span></span> <span data-ttu-id="ba7fe-262">大部分的應用程式都不需要自訂路由比對程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-262">Most apps don't require custom route matching code.</span></span>
* <span data-ttu-id="ba7fe-263">具有中介軟體的端點介面，例如 `UseAuthorization` 和 `UseCors` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-263">Endpoints interface with middleware such as `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="ba7fe-264">搭配或使用終端中介軟體時， `UseAuthorization` `UseCors` 必須手動與授權系統互動。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-264">Using a terminal middleware with `UseAuthorization` or `UseCors` requires manual interfacing with the authorization system.</span></span>

<span data-ttu-id="ba7fe-265">[端點](#endpoint)會定義兩者：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-265">An [endpoint](#endpoint) defines both:</span></span>

* <span data-ttu-id="ba7fe-266">用來處理要求的委派。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-266">A delegate to process requests.</span></span>
* <span data-ttu-id="ba7fe-267">任意中繼資料的集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-267">A collection of arbitrary metadata.</span></span> <span data-ttu-id="ba7fe-268">中繼資料是用來根據附加至每個端點的原則和設定來執行跨領域考慮。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-268">The metadata is used to implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="ba7fe-269">終端中介軟體可以是有效的工具，但可能需要：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-269">Terminal middleware can be an effective tool, but can require:</span></span>

* <span data-ttu-id="ba7fe-270">大量的編碼和測試。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-270">A significant amount of coding and testing.</span></span>
* <span data-ttu-id="ba7fe-271">與其他系統進行手動整合，以達到所需的彈性層級。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-271">Manual integration with other systems to achieve the desired level of flexibility.</span></span>

<span data-ttu-id="ba7fe-272">撰寫終端中介軟體之前，請考慮與路由整合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-272">Consider integrating with routing before writing a terminal middleware.</span></span>

<span data-ttu-id="ba7fe-273">與[Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)或整合的現有終端機中介軟體， <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 通常可以轉換成路由感知端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-273">Existing terminal middleware that integrates with [Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) or <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> can usually be turned into a routing aware endpoint.</span></span> <span data-ttu-id="ba7fe-274">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16)示範路由器的模式：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-274">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16) demonstrates the pattern for router-ware:</span></span>
* <span data-ttu-id="ba7fe-275">在上撰寫擴充方法 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-275">Write an extension method on <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span>
* <span data-ttu-id="ba7fe-276">使用建立嵌套中介軟體管線 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-276">Create a nested middleware pipeline using <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>.</span></span>
* <span data-ttu-id="ba7fe-277">將中介軟體附加至新管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-277">Attach the middleware to the new pipeline.</span></span> <span data-ttu-id="ba7fe-278">在此案例中為 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-278">In this case, <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>.</span></span>
* <span data-ttu-id="ba7fe-279"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>中介軟體管線至 <xref:Microsoft.AspNetCore.Http.RequestDelegate> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-279"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*> the middleware pipeline into a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="ba7fe-280">呼叫 `Map` 並提供新的中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-280">Call `Map` and provide the new middleware pipeline.</span></span>
* <span data-ttu-id="ba7fe-281">從擴充方法傳回提供的產生器物件 `Map` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-281">Return the builder object provided by `Map` from the extension method.</span></span>

<span data-ttu-id="ba7fe-282">下列程式碼示範如何使用[MapHealthChecks](xref:host-and-deploy/health-checks)：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-282">The following code shows use of [MapHealthChecks](xref:host-and-deploy/health-checks):</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

<span data-ttu-id="ba7fe-283">上述範例顯示為何傳回產生器物件是很重要的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-283">The preceding sample shows why returning the builder object is important.</span></span> <span data-ttu-id="ba7fe-284">傳回 builder 物件可讓應用程式開發人員設定原則，例如端點的授權。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-284">Returning the builder object allows the app developer to configure policies such as authorization for the endpoint.</span></span> <span data-ttu-id="ba7fe-285">在此範例中，健康狀態檢查中介軟體沒有與授權系統的直接整合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-285">In this example, the health checks middleware has no direct integration with the authorization system.</span></span>

<span data-ttu-id="ba7fe-286">已建立中繼資料系統，以回應擴充性作者使用終端機中介軟體所遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-286">The metadata system was created in response to the problems encountered by extensibility authors using terminal middleware.</span></span> <span data-ttu-id="ba7fe-287">每個中介軟體都有問題，使其與授權系統整合在一起。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-287">It's problematic for each middleware to implement its own integration with the authorization system.</span></span>

<a name="urlm"></a>

### <a name="url-matching"></a><span data-ttu-id="ba7fe-288">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-288">URL matching</span></span>

* <span data-ttu-id="ba7fe-289">這是路由符合連入要求至[端點](#endpoint)的進程。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-289">Is the process by which routing matches an incoming request to an [endpoint](#endpoint).</span></span>
* <span data-ttu-id="ba7fe-290">是以 URL 路徑和標頭中的資料為基礎。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-290">Is based on data in the URL path and headers.</span></span>
* <span data-ttu-id="ba7fe-291">可以擴充以考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-291">Can be extended to consider any data in the request.</span></span>

<span data-ttu-id="ba7fe-292">當路由中介軟體執行時，它會 `Endpoint` 從目前的要求將和路由值設定為上的[要求功能](xref:fundamentals/request-features) <xref:Microsoft.AspNetCore.Http.HttpContext> ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-292">When a routing middleware executes, it sets an `Endpoint` and route values to a [request feature](xref:fundamentals/request-features) on the <xref:Microsoft.AspNetCore.Http.HttpContext> from the current request:</span></span>

* <span data-ttu-id="ba7fe-293">呼叫[HttpCoNtext GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>)會取得端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-293">Calling [HttpContext.GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>) gets the endpoint.</span></span>
* <span data-ttu-id="ba7fe-294">`HttpRequest.RouteValues` 會取得路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-294">`HttpRequest.RouteValues` gets the collection of route values.</span></span>

<span data-ttu-id="ba7fe-295">在路由中介軟體之後執行的[中介軟體](xref:fundamentals/middleware/index)可以檢查端點並採取動作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-295">[Middleware](xref:fundamentals/middleware/index) running after the routing middleware can inspect the endpoint and take action.</span></span> <span data-ttu-id="ba7fe-296">例如，授權中介軟體可以針對授權原則詢問端點的元資料集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-296">For example, an authorization middleware can interrogate the endpoint's metadata collection for an authorization policy.</span></span> <span data-ttu-id="ba7fe-297">執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-297">After all of the middleware in the request processing pipeline is executed, the selected endpoint's delegate is invoked.</span></span>

<span data-ttu-id="ba7fe-298">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-298">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="ba7fe-299">因為中介軟體會根據選取的端點套用原則，所以請務必：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-299">Because the middleware applies policies based on the selected endpoint, it's important that:</span></span>

* <span data-ttu-id="ba7fe-300">任何可能影響分派或應用安全性原則的決策都是在路由系統內進行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-300">Any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

> [!WARNING]
> <span data-ttu-id="ba7fe-301">針對回溯相容性，當執行控制器或 Razor 頁面端點委派時，會根據到目前為止執行的要求處理，將[routecoNtext.routedata](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData)的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-301">For backwards-compatibility, when a Controller or Razor Pages endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>
>
> <span data-ttu-id="ba7fe-302">在 `RouteContext` 未來的版本中，此類型將會標示為已淘汰：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-302">The `RouteContext` type will be marked obsolete in a future release:</span></span>
>
> * <span data-ttu-id="ba7fe-303">遷移 `RouteData.Values` 至 `HttpRequest.RouteValues` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-303">Migrate `RouteData.Values` to `HttpRequest.RouteValues`.</span></span>
> * <span data-ttu-id="ba7fe-304">遷移 `RouteData.DataTokens` 以從端點中繼資料取得[IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-304">Migrate `RouteData.DataTokens` to retrieve [IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) from the endpoint metadata.</span></span>

<span data-ttu-id="ba7fe-305">URL 比對作業會以一組可設定的階段來運作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-305">URL matching operates in a configurable set of phases.</span></span> <span data-ttu-id="ba7fe-306">在每個階段中，輸出是一組相符專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-306">In each phase, the output is a set of matches.</span></span> <span data-ttu-id="ba7fe-307">下一個階段可進一步縮小一組相符專案的範圍。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-307">The set of matches can be narrowed down further by the next phase.</span></span> <span data-ttu-id="ba7fe-308">路由執行並不保證符合端點的處理順序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-308">The routing implementation does not guarantee a processing order for matching endpoints.</span></span> <span data-ttu-id="ba7fe-309">**所有**可能的相符專案都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-309">**All** possible matches are processed at once.</span></span> <span data-ttu-id="ba7fe-310">URL 符合的階段會依照下列順序發生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-310">The URL matching phases occur in the following order.</span></span> <span data-ttu-id="ba7fe-311">ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-311">ASP.NET Core:</span></span>

1. <span data-ttu-id="ba7fe-312">會處理一組端點及其路由範本的 URL 路徑，並收集**所有**相符專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-312">Processes the URL path against the set of endpoints and their route templates, collecting **all** of the matches.</span></span>
1. <span data-ttu-id="ba7fe-313">接受上述清單，並移除已套用路由條件約束而失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-313">Takes the preceding list and removes matches that fail with route constraints applied.</span></span>
1. <span data-ttu-id="ba7fe-314">接受上述清單並移除[MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy)實例集失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-314">Takes the preceding list and removes matches that fail the set of [MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy) instances.</span></span>
1. <span data-ttu-id="ba7fe-315">會使用[EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) ，從上述清單中進行最後的決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-315">Uses the [EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) to make a final decision from the preceding list.</span></span>

<span data-ttu-id="ba7fe-316">端點清單的優先順序會根據：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-316">The list of endpoints is prioritized according to:</span></span>

* <span data-ttu-id="ba7fe-317">[RouteEndpoint。](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-317">The [RouteEndpoint.Order](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span></span>
* <span data-ttu-id="ba7fe-318">[路由範本優先順序](#rtp)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-318">The [route template precedence](#rtp)</span></span>

<span data-ttu-id="ba7fe-319">所有相符的端點都會在每個階段中處理，直到 <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> 到達為止。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-319">All matching endpoints are processed in each phase until the <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> is reached.</span></span> <span data-ttu-id="ba7fe-320">`EndpointSelector`是最後一個階段。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-320">The `EndpointSelector` is the final phase.</span></span> <span data-ttu-id="ba7fe-321">它會從相符專案中選擇最高優先順序的端點，以做為最符合專案。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-321">It chooses the highest priority endpoint from the matches as the best match.</span></span> <span data-ttu-id="ba7fe-322">如果有其他符合的優先權與最符合的專案相同，則會擲回不明確的相符例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-322">If there are other matches with the same priority as the best match, an ambiguous match exception is thrown.</span></span>

<span data-ttu-id="ba7fe-323">路由優先順序是根據指定較高優先順序的較**特定**路由範本來計算。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-323">The route precedence is computed based on a **more specific** route template being given a higher priority.</span></span> <span data-ttu-id="ba7fe-324">例如，請考慮範本 `/hello` 和 `/{message}` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-324">For example, consider the templates `/hello` and `/{message}`:</span></span>

* <span data-ttu-id="ba7fe-325">兩者都符合 URL 路徑 `/hello` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-325">Both match the URL path `/hello`.</span></span>
* <span data-ttu-id="ba7fe-326">`/hello`更明確，因此優先順序較高。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-326">`/hello`  is more specific and therefore higher priority.</span></span>

<span data-ttu-id="ba7fe-327">一般來說，路由優先順序會針對實務中使用的 URL 配置類型選擇最符合的工作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-327">In general, route precedence does a good job of choosing the best match for the kinds of URL schemes used in practice.</span></span> <span data-ttu-id="ba7fe-328"><xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order>只有在必要時才使用，以避免發生不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-328">Use <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order> only when necessary to avoid an ambiguity.</span></span>

<span data-ttu-id="ba7fe-329">由於路由所提供的擴充性種類，路由系統無法在不明確的路由時間之前計算。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-329">Due to the kinds of extensibility provided by routing, it isn't possible for the routing system to compute ahead of time the ambiguous routes.</span></span> <span data-ttu-id="ba7fe-330">假設有一個範例，例如路由範本 `/{message:alpha}` 和 `/{message:int}` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-330">Consider an example such as the route templates `/{message:alpha}` and `/{message:int}`:</span></span>

* <span data-ttu-id="ba7fe-331">`alpha`條件約束只符合字母字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-331">The `alpha` constraint matches only alphabetic characters.</span></span>
* <span data-ttu-id="ba7fe-332">`int`條件約束只符合數位。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-332">The `int` constraint matches only numbers.</span></span>
* <span data-ttu-id="ba7fe-333">這些範本具有相同的路由優先順序，但沒有相符的單一 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-333">These templates have the same route precedence, but there's no single URL they both match.</span></span>
* <span data-ttu-id="ba7fe-334">如果路由系統在啟動時報告了不明確的錯誤，則會封鎖此有效的使用案例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-334">If the routing system reported an ambiguity error at startup, it would block this valid use case.</span></span>

> [!WARNING]
>
> <span data-ttu-id="ba7fe-335">內部作業的順序 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 不會影響路由的行為，但有一個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-335">The order of operations inside <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> doesn't influence the behavior of routing, with one exception.</span></span> <span data-ttu-id="ba7fe-336"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和會根據叫用 <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> 的順序，自動指派訂單值給其端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-336"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="ba7fe-337">這會模擬控制器的長時間行為，而不會有路由系統提供與舊版路由執行相同的保證。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-337">This simulates long-time behavior of controllers without the routing system providing the same guarantees as older routing implementations.</span></span>
>
> <span data-ttu-id="ba7fe-338">在路由的舊版執行中，可以根據路由的處理順序來執行路由擴充性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-338">In the legacy implementation of routing, it's possible to implement routing extensibility that has a dependency on the order in which routes are processed.</span></span> <span data-ttu-id="ba7fe-339">ASP.NET Core 3.0 和更新版本中的端點路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-339">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>
> 
> * <span data-ttu-id="ba7fe-340">沒有路由的概念。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-340">Doesn't have a concept of routes.</span></span>
> * <span data-ttu-id="ba7fe-341">不提供排序保證。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-341">Doesn't provide ordering guarantees.</span></span> <span data-ttu-id="ba7fe-342">所有端點都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-342">All endpoints are processed at once.</span></span>
>
> <span data-ttu-id="ba7fe-343">如果這表示您在使用舊版路由系統時停滯，請[開啟 GitHub 問題以取得協助](https://github.com/dotnet/aspnetcore/issues)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-343">If this means you're stuck using the legacy routing system, [open a GitHub issue for assistance](https://github.com/dotnet/aspnetcore/issues).</span></span>

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a><span data-ttu-id="ba7fe-344">路由範本優先順序和端點選取順序</span><span class="sxs-lookup"><span data-stu-id="ba7fe-344">Route template precedence and endpoint selection order</span></span>

<span data-ttu-id="ba7fe-345">[路由範本優先順序](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16)是一種系統，它會根據特定的方式，為每個路由範本指派一個值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-345">[Route template precedence](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16) is a system that assigns each route template a value based on how specific it is.</span></span> <span data-ttu-id="ba7fe-346">路由範本優先順序：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-346">Route template precedence:</span></span>

* <span data-ttu-id="ba7fe-347">避免在一般情況下調整端點順序的需求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-347">Avoids the need to adjust the order of endpoints in common cases.</span></span>
* <span data-ttu-id="ba7fe-348">嘗試符合常見的路由行為預期。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-348">Attempts to match the common-sense expectations of routing behavior.</span></span>

<span data-ttu-id="ba7fe-349">例如，請考慮範本 `/Products/List` 和 `/Products/{id}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-349">For example, consider templates `/Products/List` and `/Products/{id}`.</span></span> <span data-ttu-id="ba7fe-350">假設 `/Products/List` 比起 URL 路徑的比對更相符，這是合理的做法 `/Products/{id}` `/Products/List` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-350">It would be reasonable to assume that `/Products/List` is a better match than `/Products/{id}` for the URL path `/Products/List`.</span></span> <span data-ttu-id="ba7fe-351">的作用是因為常 `/List` 值區段被視為具有比參數區段更好的優先順序 `/{id}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-351">The works because the literal segment `/List` is considered to have better precedence than the parameter segment `/{id}`.</span></span>

<span data-ttu-id="ba7fe-352">優先順序運作方式的詳細資料會結合如何定義路由範本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-352">The details of how precedence works are coupled to how route templates are defined:</span></span>

* <span data-ttu-id="ba7fe-353">具有更多區段的範本會被視為更明確。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-353">Templates with more segments are considered more specific.</span></span>
* <span data-ttu-id="ba7fe-354">具有常值文字的區段會被視為比參數區段更明確。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-354">A segment with literal text is considered more specific than a parameter segment.</span></span>
* <span data-ttu-id="ba7fe-355">具有條件約束的參數區段在沒有的情況下會被視為較明確。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-355">A parameter segment with a constraint is considered more specific than one without.</span></span>
* <span data-ttu-id="ba7fe-356">複雜的區段會被視為具有條件約束的參數區段。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-356">A complex segment is considered as specific as a parameter segment with a constraint.</span></span>
* <span data-ttu-id="ba7fe-357">Catch-all 參數是最不明確的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-357">Catch-all parameters are the least specific.</span></span> <span data-ttu-id="ba7fe-358">如需有關 catch all 路由的重要資訊，請參閱[路由範本參考](#rtr)中的**全部攔截**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-358">See **catch-all** in the [Route template reference](#rtr) for important information on catch-all routes.</span></span>

<span data-ttu-id="ba7fe-359">如需確切值的參考，請參閱[GitHub 上的原始程式碼](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-359">See the [source code on GitHub](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189) for a reference of exact values.</span></span>

<a name="lg"></a>

### <a name="url-generation-concepts"></a><span data-ttu-id="ba7fe-360">URL 產生概念</span><span class="sxs-lookup"><span data-stu-id="ba7fe-360">URL generation concepts</span></span>

<span data-ttu-id="ba7fe-361">URL 產生：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-361">URL generation:</span></span>

* <span data-ttu-id="ba7fe-362">這是路由可以根據一組路由值建立 URL 路徑的進程。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-362">Is the process by which routing can create a URL path based on a set of route values.</span></span>
* <span data-ttu-id="ba7fe-363">允許端點與存取它們的 Url 之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-363">Allows for a logical separation between endpoints and the URLs that access them.</span></span>

<span data-ttu-id="ba7fe-364">端點路由包含 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-364">Endpoint routing includes the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API.</span></span> <span data-ttu-id="ba7fe-365">`LinkGenerator`是[DI](xref:fundamentals/dependency-injection)提供的單一服務。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-365">`LinkGenerator` is a singleton service available from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ba7fe-366">`LinkGenerator`API 可以在執行中要求的內容之外使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-366">The `LinkGenerator` API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="ba7fe-367">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper)和依賴的案例，例如標籤協助程式 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)，會在[Tag Helpers](xref:mvc/views/tag-helpers/intro)內部使用 `LinkGenerator` API 來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-367">[Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper) and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the `LinkGenerator` API internally to provide link generating capabilities.</span></span>

<span data-ttu-id="ba7fe-368">連結產生器背後支援的概念為「位址」\*\*\*\* 和「位址配置」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-368">The link generator is backed by the concept of an **address** and **address schemes**.</span></span> <span data-ttu-id="ba7fe-369">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-369">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="ba7fe-370">例如，許多使用者熟悉的路由名稱和路由值案例會將控制器和頁面實 Razor 作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-370">For example, the route name and route values scenarios many users are familiar with from controllers and Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="ba7fe-371">連結產生器可以透過下列擴充方法連結至控制器和 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-371">The link generator can link to controllers and Razor Pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="ba7fe-372">這些方法的多載會接受包含的引數 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-372">Overloads of these methods accept arguments that include the `HttpContext`.</span></span> <span data-ttu-id="ba7fe-373">這些方法在功能上相當於[url. 動作](xref:System.Web.Mvc.UrlHelper.Action*)和[url](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)，但提供額外的彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-373">These methods are functionally equivalent to [Url.Action](xref:System.Web.Mvc.UrlHelper.Action*) and [Url.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*), but offer additional flexibility and options.</span></span>

<span data-ttu-id="ba7fe-374">這些 `GetPath*` 方法與和最相似 `Url.Action` `Url.Page` ，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-374">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page`, in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="ba7fe-375">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-375">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="ba7fe-376">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-376">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="ba7fe-377">除非覆寫，否則會使用執行中要求的[環境](#ambient)路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-377">The [ambient](#ambient) route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="ba7fe-378">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-378"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="ba7fe-379">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-379">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="ba7fe-380">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-380">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="ba7fe-381">評估每個端點的 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-381">Each endpoint's <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern> is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="ba7fe-382">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-382">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="ba7fe-383"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-383">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="ba7fe-384">使用連結產生器最方便的方式，就是透過擴充方法來執行特定網址類別型的作業：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-384">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type:</span></span>

| <span data-ttu-id="ba7fe-385">擴充方法</span><span class="sxs-lookup"><span data-stu-id="ba7fe-385">Extension Method</span></span> | <span data-ttu-id="ba7fe-386">描述</span><span class="sxs-lookup"><span data-stu-id="ba7fe-386">Description</span></span> |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="ba7fe-387">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-387">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="ba7fe-388">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-388">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="ba7fe-389">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-389">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="ba7fe-390">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-390">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="ba7fe-391">如果 `Host` 未驗證傳入要求的標頭，則不受信任的要求輸入可以在視圖或頁面的 uri 中傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-391">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view or page.</span></span> <span data-ttu-id="ba7fe-392">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-392">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="ba7fe-393">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-393">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="ba7fe-394">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-394">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="ba7fe-395">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-395">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="ba7fe-396">指定空的基底路徑，以復原 `Map*` 連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-396">Specify an empty base path to undo the `Map*` affect on link generation.</span></span>

### <a name="middleware-example"></a><span data-ttu-id="ba7fe-397">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="ba7fe-397">Middleware example</span></span>

<span data-ttu-id="ba7fe-398">在下列範例中，中介軟體會使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 來建立列出商店產品之動作方法的連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-398">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create a link to an action method that lists store products.</span></span> <span data-ttu-id="ba7fe-399">藉由將連結產生器插入類別並呼叫， `GenerateLink` 可供應用程式中的任何類別使用：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-399">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a><span data-ttu-id="ba7fe-400">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-400">Route template reference</span></span>

<span data-ttu-id="ba7fe-401">內 `{}` 的 token 會定義路由符合時所系結的路由參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-401">Tokens within `{}` define route parameters that are bound if the route is matched.</span></span> <span data-ttu-id="ba7fe-402">在路由區段中可以定義一個以上的路由參數，但路由參數必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-402">More than one route parameter can be defined in a route segment, but route parameters  must be separated by a literal value.</span></span> <span data-ttu-id="ba7fe-403">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-403">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span>  <span data-ttu-id="ba7fe-404">路由參數必須有名稱，而且可能會指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-404">Route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ba7fe-405">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-405">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ba7fe-406">文字比對不區分大小寫，而且會根據 URL 路徑的解碼標記法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-406">Text matching is case-insensitive and based on the decoded representation of the URL's path.</span></span> <span data-ttu-id="ba7fe-407">若要比對常值路由參數分隔符號 `{` 或 `}` ，請重複字元來將分隔符號轉義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-407">To match a literal route parameter delimiter `{` or `}`, escape the delimiter by repeating the character.</span></span> <span data-ttu-id="ba7fe-408">例如， `{{` 或 `}}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-408">For example `{{` or `}}`.</span></span>

<span data-ttu-id="ba7fe-409">星號 `*` 或雙星號 `**` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-409">Asterisk `*` or double asterisk `**`:</span></span>

* <span data-ttu-id="ba7fe-410">可以做為路由參數的前置詞，以系結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-410">Can be used as a prefix to a route parameter to bind to the rest of the URI.</span></span>
* <span data-ttu-id="ba7fe-411">稱為「**全部攔截**」參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-411">Are called a **catch-all** parameters.</span></span> <span data-ttu-id="ba7fe-412">例如，`blog/{**slug}`：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-412">For example, `blog/{**slug}`:</span></span>
  * <span data-ttu-id="ba7fe-413">會比對開頭為的任何 URI `/blog` ，並在其後面加上任何值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-413">Matches any URI that starts with `/blog` and has any value following it.</span></span>
  * <span data-ttu-id="ba7fe-414">下列值 `/blog` 會指派給 [[資訊](https://developer.mozilla.org/docs/Glossary/Slug)區路線] 路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-414">The value following `/blog` is assigned to the [slug](https://developer.mozilla.org/docs/Glossary/Slug) route value.</span></span>

[!INCLUDE[](~/includes/catchall.md)]

<span data-ttu-id="ba7fe-415">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-415">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ba7fe-416">當使用路由來產生 URL （包括路徑分隔符號）時，catch-all 參數會將適當的字元進行轉義 `/` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-416">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator `/` characters.</span></span> <span data-ttu-id="ba7fe-417">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-417">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ba7fe-418">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-418">Note the escaped forward slash.</span></span> <span data-ttu-id="ba7fe-419">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-419">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="ba7fe-420">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-420">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="ba7fe-421">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-421">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ba7fe-422">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-422">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ba7fe-423">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-423">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ba7fe-424">如果 `filename` URL 中只有存在的值，則路由會符合，因為尾端 `.` 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-424">If only a value for `filename` exists in the URL, the route matches because the trailing `.` is  optional.</span></span> <span data-ttu-id="ba7fe-425">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-425">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ba7fe-426">路由參數可能有「預設值」\*\*\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-426">Route parameters may have **default values** designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ba7fe-427">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-427">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ba7fe-428">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-428">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ba7fe-429">將問號（ `?` ）附加至參數名稱的結尾，可將路由參數設為選擇性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-429">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name.</span></span> <span data-ttu-id="ba7fe-430">例如： `id?` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-430">For example, `id?`.</span></span> <span data-ttu-id="ba7fe-431">選擇性值與預設路由參數之間的差異如下：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-431">The difference between optional values and default route parameters is:</span></span>

* <span data-ttu-id="ba7fe-432">具有預設值的路由參數一律會產生值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-432">A route parameter with a default value always produces a value.</span></span>
* <span data-ttu-id="ba7fe-433">選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-433">An optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ba7fe-434">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-434">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ba7fe-435">在 `:` 路由參數名稱之後加入和條件約束名稱，會在路由參數上指定內嵌條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-435">Adding `:` and constraint name after the route parameter name specifies an inline constraint on a route parameter.</span></span> <span data-ttu-id="ba7fe-436">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 `(...)` 括住。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-436">If the constraint requires arguments, they're enclosed in parentheses `(...)` after the constraint name.</span></span> <span data-ttu-id="ba7fe-437">藉由附加另一個和條件約束名稱，可以指定多個*內嵌條件約束* `:` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-437">Multiple *inline constraints* can be specified by appending another `:` and constraint name.</span></span>

<span data-ttu-id="ba7fe-438">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-438">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ba7fe-439">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-439">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ba7fe-440">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-440">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ba7fe-441">路由參數也可以具有參數轉換器。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-441">Route parameters may also have parameter transformers.</span></span> <span data-ttu-id="ba7fe-442">參數轉換器會在產生連結並比對動作和頁面到 Url 時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-442">Parameter transformers transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="ba7fe-443">如同條件約束，參數轉換器可以藉由 `:` 在路由參數名稱之後新增和轉換器名稱，以內嵌方式加入至路由參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-443">Like constraints, parameter transformers can be added inline to a route parameter by adding a `:` and transformer name after the route parameter name.</span></span> <span data-ttu-id="ba7fe-444">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-444">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="ba7fe-445">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-445">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="ba7fe-446">下表示范範例路由範本及其行為：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-446">The following table demonstrates example route templates and their behavior:</span></span>

| <span data-ttu-id="ba7fe-447">路由範本</span><span class="sxs-lookup"><span data-stu-id="ba7fe-447">Route Template</span></span>                           | <span data-ttu-id="ba7fe-448">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ba7fe-448">Example Matching URI</span></span>    | <span data-ttu-id="ba7fe-449">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ba7fe-449">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ba7fe-450">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-450">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ba7fe-451">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-451">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ba7fe-452">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-452">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ba7fe-453">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-453">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ba7fe-454">對應至 `Products` 控制器和 `Details` 動作，並 `id` 將設定為123。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-454">Maps to the `Products` controller and  `Details` action with`id` set to 123.</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ba7fe-455">對應至 `Home` 控制器和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-455">Maps to the `Home` controller and `Index` method.</span></span> <span data-ttu-id="ba7fe-456">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-456">`id` is ignored.</span></span>        |
| `{controller=Home}/{action=Index}/{id?}` | `/Products`         | <span data-ttu-id="ba7fe-457">對應至 `Products` 控制器和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-457">Maps to the `Products` controller and `Index` method.</span></span> <span data-ttu-id="ba7fe-458">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-458">`id` is ignored.</span></span>        |

<span data-ttu-id="ba7fe-459">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-459">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ba7fe-460">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-460">Constraints and defaults can also be specified outside the route template.</span></span>

### <a name="complex-segments"></a><span data-ttu-id="ba7fe-461">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ba7fe-461">Complex segments</span></span>

<span data-ttu-id="ba7fe-462">複雜區段的處理方式是以[非貪婪](#greedy)的方式，將常值分隔符號從右至左比對。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-462">Complex segments are processed by matching up literal delimiters from right to left in a [non-greedy](#greedy) way.</span></span> <span data-ttu-id="ba7fe-463">例如， `[Route("/a{b}c{d}")]` 是一個複雜的區段。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-463">For example, `[Route("/a{b}c{d}")]` is a complex segment.</span></span>
<span data-ttu-id="ba7fe-464">複雜的區段會以特定的方式工作，必須瞭解才能成功使用它們。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-464">Complex segments work in a particular way that must be understood to use them successfully.</span></span> <span data-ttu-id="ba7fe-465">本章節中的範例將示範當分隔符號文字不會出現在參數值內時，複雜區段的效果為何。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-465">The example in this section demonstrates why complex segments only really work well when the delimiter text doesn't appear inside the parameter values.</span></span> <span data-ttu-id="ba7fe-466">在較複雜的情況下，需要使用[RegEx](/dotnet/standard/base-types/regular-expressions) ，然後手動將值解壓縮。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-466">Using a [regex](/dotnet/standard/base-types/regular-expressions) and then manually extracting the values is needed for more complex cases.</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ba7fe-467">這是路由執行的步驟 `/a{b}c{d}` 和範本和 URL 路徑的摘要 `/abcd` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-467">This is a summary of the steps that routing performs with the template `/a{b}c{d}` and the URL path `/abcd`.</span></span> <span data-ttu-id="ba7fe-468">`|`是用來協助視覺化演算法的運作方式：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-468">The `|` is used to help visualize how the algorithm works:</span></span>

* <span data-ttu-id="ba7fe-469">第一個常值（由右至左）為 `c` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-469">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="ba7fe-470">因此， `/abcd` 會向右搜尋並尋找 `/ab|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-470">So `/abcd` is searched from right and finds `/ab|c|d`.</span></span>
* <span data-ttu-id="ba7fe-471">右邊的所有專案（ `d` ）現在都符合路由參數 `{d}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-471">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="ba7fe-472">下一個常值（由右至左）為 `a` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-472">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="ba7fe-473">因此， `/ab|c|d` 在從離開的地方開始搜尋，然後 `a` 找到 `/|a|b|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-473">So `/ab|c|d` is searched starting where we left off, then `a` is found `/|a|b|c|d`.</span></span>
* <span data-ttu-id="ba7fe-474">右邊的值（ `b` ）現在符合路由參數 `{b}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-474">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="ba7fe-475">沒有任何剩餘的文字，也沒有剩餘的路由範本，因此這是相符的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-475">There is no remaining text and no remaining route template, so this is a match.</span></span>

<span data-ttu-id="ba7fe-476">以下是使用相同範本和 URL 路徑的負面案例範例 `/a{b}c{d}` `/aabcd` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-476">Here's an example of a negative case using the same template `/a{b}c{d}` and the URL path `/aabcd`.</span></span> <span data-ttu-id="ba7fe-477">`|`是用來協助視覺化演算法的運作方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-477">The `|` is used to help visualize how the algorithm works.</span></span> <span data-ttu-id="ba7fe-478">這種情況並不相符，這會透過相同的演算法來說明：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-478">This case isn't a match, which is explained by the same algorithm:</span></span>
* <span data-ttu-id="ba7fe-479">第一個常值（由右至左）為 `c` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-479">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="ba7fe-480">因此， `/aabcd` 會向右搜尋並尋找 `/aab|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-480">So `/aabcd` is searched from right and finds `/aab|c|d`.</span></span>
* <span data-ttu-id="ba7fe-481">右邊的所有專案（ `d` ）現在都符合路由參數 `{d}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-481">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="ba7fe-482">下一個常值（由右至左）為 `a` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-482">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="ba7fe-483">因此， `/aab|c|d` 在從離開的地方開始搜尋，然後 `a` 找到 `/a|a|b|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-483">So `/aab|c|d` is searched starting where we left off, then `a` is found `/a|a|b|c|d`.</span></span>
* <span data-ttu-id="ba7fe-484">右邊的值（ `b` ）現在符合路由參數 `{b}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-484">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="ba7fe-485">此時會有剩餘的文字 `a` ，但演算法已用完路由範本進行剖析，因此這不是相符的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-485">At this point there is remaining text `a`, but the algorithm has run out of route template to parse, so this is not a match.</span></span>

<span data-ttu-id="ba7fe-486">因為比對演算法[是非貪婪](#greedy)的：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-486">Since the matching algorithm is [non-greedy](#greedy):</span></span>

* <span data-ttu-id="ba7fe-487">它會比對每個步驟中可能的最小文字數量。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-487">It matches the smallest amount of text possible in each step.</span></span>
* <span data-ttu-id="ba7fe-488">在參數值內出現分隔符號值的任何情況下，都會導致不相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-488">Any case where the delimiter value appears inside the parameter values results in not matching.</span></span>

<span data-ttu-id="ba7fe-489">正則運算式可讓您更充分掌控其比對行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-489">Regular expressions provide much more control over their matching behavior.</span></span>

<a name="greedy"></a>

<span data-ttu-id="ba7fe-490">貪婪比對（也稱為[延遲](https://wikipedia.org/wiki/Regular_expression#Lazy_matching)比對）符合最大的可能字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-490">Greedy matching, also know as [lazy matching](https://wikipedia.org/wiki/Regular_expression#Lazy_matching), matches the largest possible string.</span></span> <span data-ttu-id="ba7fe-491">非貪婪的會符合最小的可能字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-491">Non-greedy matches the smallest possible string.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="ba7fe-492">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-492">Route constraint reference</span></span>

<span data-ttu-id="ba7fe-493">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-493">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ba7fe-494">路由條件約束通常會檢查透過路由範本相關聯的路由值，並對值是否可接受進行 true 或 false 的決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-494">Route constraints generally inspect the route value associated via the route template and make a true or false decision about whether the value is acceptable.</span></span> <span data-ttu-id="ba7fe-495">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-495">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ba7fe-496">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-496">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ba7fe-497">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-497">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ba7fe-498">請勿針對輸入驗證使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-498">Don't use constraints for input validation.</span></span> <span data-ttu-id="ba7fe-499">如果條件約束用於輸入驗證，則不正確輸入會導致找不到的 `404` 回應。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-499">If constraints are used for input validation, invalid input results in a `404` Not Found response.</span></span> <span data-ttu-id="ba7fe-500">不正確輸入應該會產生不 `400` 正確的要求，並出現適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-500">Invalid input should produce a `400` Bad Request with an appropriate error message.</span></span> <span data-ttu-id="ba7fe-501">路由條件約束會用來釐清類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-501">Route constraints are used to disambiguate similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ba7fe-502">下表示范範例路由條件約束及其預期行為：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-502">The following table demonstrates example route constraints and their expected behavior:</span></span>

| <span data-ttu-id="ba7fe-503">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-503">constraint</span></span> | <span data-ttu-id="ba7fe-504">範例</span><span class="sxs-lookup"><span data-stu-id="ba7fe-504">Example</span></span> | <span data-ttu-id="ba7fe-505">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-505">Example Matches</span></span> | <span data-ttu-id="ba7fe-506">備註</span><span class="sxs-lookup"><span data-stu-id="ba7fe-506">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ba7fe-507">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-507">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-508">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="ba7fe-508">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ba7fe-509">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-509">`true`, `FALSE`</span></span> | <span data-ttu-id="ba7fe-510">符合 `true` 或 `false` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-510">Matches `true` or `false`.</span></span> <span data-ttu-id="ba7fe-511">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ba7fe-511">Case-insensitive</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ba7fe-512">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-512">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ba7fe-513">符合不因文化特性而異的有效 `DateTime` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-513">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-514">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-514">See preceding warning.</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="ba7fe-515">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-515">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ba7fe-516">符合不因文化特性而異的有效 `decimal` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-516">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-517">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-517">See preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ba7fe-518">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-518">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-519">符合不因文化特性而異的有效 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-519">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-520">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-520">See preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ba7fe-521">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-521">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-522">符合不因文化特性而異的有效 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-522">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-523">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-523">See preceding warning.</span></span>|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | <span data-ttu-id="ba7fe-524">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-524">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ba7fe-525">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-525">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-526">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-526">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ba7fe-527">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-527">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="ba7fe-528">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-528">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ba7fe-529">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-529">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ba7fe-530">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-530">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ba7fe-531">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="ba7fe-531">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ba7fe-532">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ba7fe-532">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ba7fe-533">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ba7fe-533">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ba7fe-534">字串必須包含一或多個字母字元， `a` - `z` 並不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-534">String must consist of one or more alphabetical characters, `a`-`z` and case-insensitive.</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ba7fe-535">字串必須符合正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-535">String must match the regular expression.</span></span> <span data-ttu-id="ba7fe-536">請參閱定義正則運算式的秘訣。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-536">See tips about defining a regular expression.</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ba7fe-537">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-537">Used to enforce that a non-parameter value is present during URL generation</span></span> |

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ba7fe-538">多個以冒號分隔的條件約束可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-538">Multiple, colon delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ba7fe-539">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-539">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ba7fe-540">驗證 URL 並轉換成 CLR 類型的路由條件約束，一律會使用不因文化特性而異。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-540">Route constraints that verify the URL and are converted to a CLR type always use the invariant culture.</span></span> <span data-ttu-id="ba7fe-541">例如，轉換成 CLR 型別 `int` 或 `DateTime` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-541">For example, conversion to the CLR type `int` or `DateTime`.</span></span> <span data-ttu-id="ba7fe-542">這些條件約束假設 URL 無法當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-542">These constraints assume that the URL is not localizable.</span></span> <span data-ttu-id="ba7fe-543">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-543">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ba7fe-544">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-544">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ba7fe-545">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-545">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

### <a name="regular-expressions-in-constraints"></a><span data-ttu-id="ba7fe-546">條件約束中的正則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-546">Regular expressions in constraints</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ba7fe-547">正則運算式可以指定為使用路由條件約束的內嵌條件約束 `regex(...)` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-547">Regular expressions can be specified as inline constraints using the `regex(...)` route constraint.</span></span> <span data-ttu-id="ba7fe-548">系列中的方法 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> 也會接受條件約束的物件常值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-548">Methods in the <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> family also accept an object literal of constraints.</span></span> <span data-ttu-id="ba7fe-549">如果使用該表單，字串值就會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-549">If that form is used, string values are interpreted as regular expressions.</span></span>

<span data-ttu-id="ba7fe-550">下列程式碼會使用內嵌 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-550">The following code uses an inline regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

<span data-ttu-id="ba7fe-551">下列程式碼會使用物件常值來指定 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-551">The following code uses an object literal to specify a regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

<span data-ttu-id="ba7fe-552">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-552">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ba7fe-553">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-553">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ba7fe-554">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-554">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="ba7fe-555">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-555">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ba7fe-556">若要 `^\d{3}-\d{2}-\d{4}$` 在內嵌條件約束中使用正則運算式，請使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-556">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in an inline constraint, use one of the following:</span></span>

* <span data-ttu-id="ba7fe-557">將 `\` 字串中提供的字元取代為 c # 原始檔中的字元，以便 `\\` 將 `\` 字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-557">Replace `\` characters provided in the string as `\\` characters in the C# source file in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="ba7fe-558">[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-558">[Verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="ba7fe-559">若要轉義路由參數分隔符號 `{` 、 `}` 、 `[` 、，請將 `]` 運算式中的字元加倍，例如、 `{{` 、 `}}` `[[` 、 `]]` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-559">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression, for example, `{{`, `}}`, `[[`, `]]`.</span></span> <span data-ttu-id="ba7fe-560">下表顯示正則運算式和其轉義版本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-560">The following table shows a regular expression and its escaped version:</span></span>

| <span data-ttu-id="ba7fe-561">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-561">Regular expression</span></span>    | <span data-ttu-id="ba7fe-562">已轉義的正則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-562">Escaped regular expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ba7fe-563">路由中使用的正則運算式通常以 `^` 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-563">Regular expressions used in routing often start with the `^` character and match the starting position of the string.</span></span> <span data-ttu-id="ba7fe-564">運算式通常以 `$` 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-564">The expressions often end with the `$` character and match the end of the string.</span></span> <span data-ttu-id="ba7fe-565">`^`和 `$` 字元可確保正則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-565">The `^` and `$` characters ensure that the regular expression matches the entire route parameter value.</span></span> <span data-ttu-id="ba7fe-566">如果沒有 `^` 和 `$` 字元，正則運算式會比對字串內的任何子字串，這通常是不需要的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-566">Without the `^` and `$` characters, the regular expression matches any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ba7fe-567">下表提供範例，並說明它們符合或無法符合的原因：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-567">The following table provides examples and explains why they match or fail to match:</span></span>

| <span data-ttu-id="ba7fe-568">運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-568">Expression</span></span>   | <span data-ttu-id="ba7fe-569">String</span><span class="sxs-lookup"><span data-stu-id="ba7fe-569">String</span></span>    | <span data-ttu-id="ba7fe-570">比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-570">Match</span></span> | <span data-ttu-id="ba7fe-571">註解</span><span class="sxs-lookup"><span data-stu-id="ba7fe-571">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-572">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-572">hello</span></span>     | <span data-ttu-id="ba7fe-573">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-573">Yes</span></span>   | <span data-ttu-id="ba7fe-574">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-574">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-575">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-575">123abc456</span></span> | <span data-ttu-id="ba7fe-576">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-576">Yes</span></span>   | <span data-ttu-id="ba7fe-577">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-577">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-578">mz</span><span class="sxs-lookup"><span data-stu-id="ba7fe-578">mz</span></span>        | <span data-ttu-id="ba7fe-579">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-579">Yes</span></span>   | <span data-ttu-id="ba7fe-580">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-580">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-581">MZ</span><span class="sxs-lookup"><span data-stu-id="ba7fe-581">MZ</span></span>        | <span data-ttu-id="ba7fe-582">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-582">Yes</span></span>   | <span data-ttu-id="ba7fe-583">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ba7fe-583">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-584">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-584">hello</span></span>     | <span data-ttu-id="ba7fe-585">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-585">No</span></span>    | <span data-ttu-id="ba7fe-586">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-586">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-587">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-587">123abc456</span></span> | <span data-ttu-id="ba7fe-588">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-588">No</span></span>    | <span data-ttu-id="ba7fe-589">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-589">See `^` and `$` above</span></span> |

<span data-ttu-id="ba7fe-590">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-590">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ba7fe-591">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-591">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ba7fe-592">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-592">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ba7fe-593">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-593">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ba7fe-594">在條件約束字典中傳遞並不符合其中一個已知條件約束的條件約束，也會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-594">Constraints that are passed in the constraints dictionary that don't match one of the known constraints are also treated as regular expressions.</span></span> <span data-ttu-id="ba7fe-595">在不符合其中一個已知條件約束的範本內傳遞的條件約束，不會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-595">Constraints that are passed  within a template that don't match one of the known constraints are not treated as regular expressions.</span></span>

### <a name="custom-route-constraints"></a><span data-ttu-id="ba7fe-596">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="ba7fe-596">Custom route constraints</span></span>

<span data-ttu-id="ba7fe-597">您可以藉由執行介面來建立自訂路由條件約束 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-597">Custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ba7fe-598">`IRouteConstraint`介面包含 <xref:System.Web.Routing.IRouteConstraint.Match*> ， `true` 如果條件約束已滿足，則會傳回，否則會傳回 `false` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-598">The `IRouteConstraint` interface contains <xref:System.Web.Routing.IRouteConstraint.Match*>, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ba7fe-599">很少需要自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-599">Custom route constraints are rarely needed.</span></span> <span data-ttu-id="ba7fe-600">在執行自訂路由條件約束之前，請考慮使用替代專案，例如模型系結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-600">Before implementing a custom route constraint, consider alternatives, such as model binding.</span></span>

<span data-ttu-id="ba7fe-601">[ASP.NET Core[條件約束](https://github.com/dotnet/aspnetcore/tree/master/src/Http/Routing/src/Constraints)] 資料夾會提供建立條件約束的良好範例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-601">The ASP.NET Core [Constraints](https://github.com/dotnet/aspnetcore/tree/master/src/Http/Routing/src/Constraints) folder provides good examples of creating a constraints.</span></span> <span data-ttu-id="ba7fe-602">例如， [GuidRouteConstraint](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Constraints/GuidRouteConstraint.cs#L18)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-602">For example, [GuidRouteConstraint](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Constraints/GuidRouteConstraint.cs#L18).</span></span>

<span data-ttu-id="ba7fe-603">若要使用自訂 `IRouteConstraint` ，則必須向 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 服務容器中的應用程式註冊路由條件約束類型。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-603">To use a custom `IRouteConstraint`, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the service container.</span></span> <span data-ttu-id="ba7fe-604">`ConstraintMap` 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 `IRouteConstraint` 實作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-604">A `ConstraintMap` is a dictionary that maps route constraint keys to `IRouteConstraint` implementations that validate those constraints.</span></span> <span data-ttu-id="ba7fe-605">更新應用程式的 `ConstraintMap` 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-605">An app's `ConstraintMap` can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ba7fe-606">例如：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-606">For example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

<span data-ttu-id="ba7fe-607">上述條件約束會套用至下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-607">The preceding constraint is applied in the following code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

<span data-ttu-id="ba7fe-608">的執行 `MyCustomConstraint` 會防止套用 `0` 至路由參數：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-608">The implementation of `MyCustomConstraint` prevents `0` being applied to a route parameter:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="ba7fe-609">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-609">The preceding code:</span></span>

* <span data-ttu-id="ba7fe-610">防止 `0` 在 `{id}` 路由的區段中。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-610">Prevents `0` in the `{id}` segment of the route.</span></span>
* <span data-ttu-id="ba7fe-611">會顯示以提供執行自訂條件約束的基本範例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-611">Is shown to provide a basic example of implementing a custom constraint.</span></span> <span data-ttu-id="ba7fe-612">不應在生產應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-612">It should not be used in a production app.</span></span>

<span data-ttu-id="ba7fe-613">下列程式碼是避免 `id` 處理包含之的較佳方法 `0` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-613">The following code is a better approach to preventing an `id` containing a `0` from being processed:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="ba7fe-614">上述程式碼在此方法上具有下列優點 `MyCustomConstraint` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-614">The preceding code has the following advantages over the `MyCustomConstraint` approach:</span></span>

* <span data-ttu-id="ba7fe-615">不需要自訂條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-615">It doesn't require a custom constraint.</span></span>
* <span data-ttu-id="ba7fe-616">當路由參數包含時，它會傳回更具描述性的錯誤 `0` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-616">It returns a more descriptive error when the route parameter includes `0`.</span></span>

## <a name="parameter-transformer-reference"></a><span data-ttu-id="ba7fe-617">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-617">Parameter transformer reference</span></span>

<span data-ttu-id="ba7fe-618">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-618">Parameter transformers:</span></span>

* <span data-ttu-id="ba7fe-619">使用產生連結時執行 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-619">Execute when generating a link using <xref:Microsoft.AspNetCore.Routing.LinkGenerator>.</span></span>
* <span data-ttu-id="ba7fe-620">實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-620">Implement <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>.</span></span>
* <span data-ttu-id="ba7fe-621">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-621">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="ba7fe-622">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-622">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="ba7fe-623">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-623">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="ba7fe-624">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-624">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="ba7fe-625">請考慮下列執行 `IOutboundParameterTransformer` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-625">Consider the following `IOutboundParameterTransformer` implementation:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

<span data-ttu-id="ba7fe-626">若要在路由模式中使用參數轉換器，請使用中的來設定它 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-626">To use a parameter transformer in a route pattern, configure it using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

<span data-ttu-id="ba7fe-627">ASP.NET Core framework 會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-627">The ASP.NET Core framework uses parameter transformers to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="ba7fe-628">例如，參數轉換器會轉換用來比對 `area` 、、和的路由值 `controller` `action` `page` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-628">For example, parameter transformers transform the route values used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="ba7fe-629">使用上述路由範本，動作 `SubscriptionManagementController.GetAll` 會與 URI 相符 `/subscription-management/get-all` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-629">With the preceding route template, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="ba7fe-630">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-630">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="ba7fe-631">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-631">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="ba7fe-632">ASP.NET Core 提供使用參數轉換器搭配產生的路由的 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-632">ASP.NET Core provides API conventions for using parameter transformers with generated routes:</span></span>

* <span data-ttu-id="ba7fe-633"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName>MVC 慣例會將指定的參數轉換器套用至應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-633">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="ba7fe-634">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-634">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="ba7fe-635">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-635">For more information, see [Use a parameter transformer to customize token replacement](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* Razor<span data-ttu-id="ba7fe-636">頁面會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-636"> Pages uses the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API convention.</span></span> <span data-ttu-id="ba7fe-637">此慣例會將指定的參數轉換器套用至所有自動探索的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-637">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="ba7fe-638">參數轉換器會轉換頁面路由的資料夾和檔案名區段 Razor 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-638">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="ba7fe-639">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-639">For more information, see [Use a parameter transformer to customize page routes](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

<a name="ugr"></a>

## <a name="url-generation-reference"></a><span data-ttu-id="ba7fe-640">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-640">URL generation reference</span></span>

<span data-ttu-id="ba7fe-641">此章節包含 URL 產生所實作為演算法的參考。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-641">This section contains a reference for the algorithm implemented by URL generation.</span></span> <span data-ttu-id="ba7fe-642">實際上，最複雜的 URL 產生範例會使用控制器或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-642">In practice, most complex examples of URL generation use controllers or Razor Pages.</span></span> <span data-ttu-id="ba7fe-643">如需其他資訊，請參閱[控制器中的路由](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-643">See  [routing in controllers](xref:mvc/controllers/routing) for additional information.</span></span>

<span data-ttu-id="ba7fe-644">URL 產生進程會從呼叫[LinkGenerator. GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*)或類似的方法開始。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-644">The URL generation process begins with a call to [LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*) or a similar method.</span></span> <span data-ttu-id="ba7fe-645">提供的方法包含位址、一組路由值，以及來自的目前要求的選擇性資訊 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-645">The method is provided with an address, a set of route values, and optionally information about the current request from `HttpContext`.</span></span>

<span data-ttu-id="ba7fe-646">第一個步驟是使用位址來解析一組候選端點，並使用 [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) 符合網址類別型的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-646">The first step is to use the address to resolve a set of candidate endpoints using an [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) that matches the address's type.</span></span>

<span data-ttu-id="ba7fe-647">位址配置找到一組候選項目之後，就會反復排序並處理端點，直到 URL 產生作業成功為止。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-647">Once of set of candidates is found by the address scheme, the endpoints are ordered and processed iteratively until a URL generation operation succeeds.</span></span> <span data-ttu-id="ba7fe-648">URL**產生不會檢查是否**有多義性，傳回的第一個結果是最終的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-648">URL generation does **not** check for ambiguities, the first result returned is the final result.</span></span>

### <a name="troubleshooting-url-generation-with-logging"></a><span data-ttu-id="ba7fe-649">使用記錄進行 URL 產生的疑難排解</span><span class="sxs-lookup"><span data-stu-id="ba7fe-649">Troubleshooting URL generation with logging</span></span>

<span data-ttu-id="ba7fe-650">針對 URL 產生進行疑難排解的第一個步驟是將的記錄層級設定 `Microsoft.AspNetCore.Routing` 為 `TRACE` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-650">The first step in troubleshooting URL generation is setting the logging level of `Microsoft.AspNetCore.Routing` to `TRACE`.</span></span> <span data-ttu-id="ba7fe-651">`LinkGenerator`記錄其處理的許多詳細資料，這有助於疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-651">`LinkGenerator` logs many details about its processing which can be useful to troubleshoot problems.</span></span>

<span data-ttu-id="ba7fe-652">如需 URL 產生的詳細資訊，請參閱[url 產生參考](#ugr)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-652">See [URL generation reference](#ugr) for details on URL generation.</span></span>

### <a name="addresses"></a><span data-ttu-id="ba7fe-653">位址</span><span class="sxs-lookup"><span data-stu-id="ba7fe-653">Addresses</span></span>

<span data-ttu-id="ba7fe-654">位址是 URL 產生的概念，用來將連結產生器的呼叫系結至一組候選端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-654">Addresses are the concept in URL generation used to bind a call into the link generator to a set of candidate endpoints.</span></span>

<span data-ttu-id="ba7fe-655">位址是一種可延伸的概念，預設會隨附兩個執行：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-655">Addresses are an extensible concept that come with two implementations by default:</span></span>

* <span data-ttu-id="ba7fe-656">使用*端點名稱*（ `string` ）作為位址：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-656">Using *endpoint name* (`string`) as the address:</span></span>
    * <span data-ttu-id="ba7fe-657">為 MVC 的路由名稱提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-657">Provides similar functionality to MVC's route name.</span></span>
    * <span data-ttu-id="ba7fe-658">使用 <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> 元資料類型。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-658">Uses the <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> metadata type.</span></span>
    * <span data-ttu-id="ba7fe-659">針對所有已註冊端點的中繼資料，解析提供的字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-659">Resolves the provided string against the metadata of all registered endpoints.</span></span>
    * <span data-ttu-id="ba7fe-660">如果多個端點使用相同的名稱，則會在啟動時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-660">Throws an exception on startup if multiple endpoints use the same name.</span></span>
    * <span data-ttu-id="ba7fe-661">建議用於在控制器和頁面以外的一般用途 Razor 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-661">Recommended for general-purpose use outside of controllers and Razor Pages.</span></span>
* <span data-ttu-id="ba7fe-662">使用*路由值*（ <xref:Microsoft.AspNetCore.Routing.RouteValuesAddress> ）做為位址：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-662">Using *route values* (<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>) as the address:</span></span>
    * <span data-ttu-id="ba7fe-663">提供類似的功能來進行控制器和 Razor 分頁的舊版 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-663">Provides similar functionality to controllers and Razor Pages legacy URL generation.</span></span>
    * <span data-ttu-id="ba7fe-664">擴充和調試非常複雜。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-664">Very complex to extend and debug.</span></span>
    * <span data-ttu-id="ba7fe-665">提供所使用的實作為 `IUrlHelper` 、標記協助程式、HTML helper、動作結果等。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-665">Provides the implementation used by `IUrlHelper`, Tag Helpers, HTML Helpers, Action Results, etc.</span></span>

<span data-ttu-id="ba7fe-666">位址配置的角色是讓位址和相符端點之間的關聯依照任意準則：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-666">The role of the address scheme is to make the association between the address and matching endpoints by arbitrary criteria:</span></span>

* <span data-ttu-id="ba7fe-667">端點名稱配置會執行基本的字典查閱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-667">The endpoint name scheme performs a basic dictionary lookup.</span></span>
* <span data-ttu-id="ba7fe-668">路由值配置具有集合演算法的複雜最佳子集。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-668">The route values scheme has a complex best subset of set algorithm.</span></span>

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a><span data-ttu-id="ba7fe-669">環境值和明確的值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-669">Ambient values and explicit values</span></span>

<span data-ttu-id="ba7fe-670">根據目前的要求，路由會存取目前要求的路由值 `HttpContext.Request.RouteValues` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-670">From the current request, routing accesses the route values of the current request `HttpContext.Request.RouteValues`.</span></span> <span data-ttu-id="ba7fe-671">與目前要求相關聯的值稱為「**環境值**」。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-671">The values associated with the current request are referred to as the **ambient values**.</span></span> <span data-ttu-id="ba7fe-672">為了清楚起見，檔是指傳遞至方法做為**明確值**的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-672">For the purpose of clarity, the documentation refers to the route values passed in to methods as **explicit values**.</span></span>

<span data-ttu-id="ba7fe-673">下列範例會顯示環境值和明確的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-673">The following example shows ambient values and explicit values.</span></span> <span data-ttu-id="ba7fe-674">它會從目前的要求和明確的值中提供環境 `{ id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-674">It provides ambient values from the current request and explicit values: `{ id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

<span data-ttu-id="ba7fe-675">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-675">The preceding code:</span></span>

* <span data-ttu-id="ba7fe-676">傳回 `/Widget/Index/17`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-676">Returns `/Widget/Index/17`</span></span>
* <span data-ttu-id="ba7fe-677">透過 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> [DI](xref:fundamentals/dependency-injection)取得。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-677">Gets <xref:Microsoft.AspNetCore.Routing.LinkGenerator> via [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ba7fe-678">下列程式碼不會提供環境值和明確的 `{ controller = "Home", action = "Subscribe", id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-678">The following code provides no ambient values and explicit values: `{ controller = "Home", action = "Subscribe", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

<span data-ttu-id="ba7fe-679">上述方法會傳回`/Home/Subscribe/17`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-679">The preceding  method returns `/Home/Subscribe/17`</span></span>

<span data-ttu-id="ba7fe-680">中的下列程式碼會傳回 `WidgetController` `/Widget/Subscribe/17` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-680">The following code in the `WidgetController` returns `/Widget/Subscribe/17`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

<span data-ttu-id="ba7fe-681">下列程式碼會從目前要求中的環境值和明確值提供 `{ action = "Edit", id = 17, }` 控制器：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-681">The following code provides the controller from ambient values in the current request and explicit values: `{ action = "Edit", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

<span data-ttu-id="ba7fe-682">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-682">In the preceding code:</span></span>

* <span data-ttu-id="ba7fe-683">`/Gadget/Edit/17`傳回。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-683">`/Gadget/Edit/17` is returned.</span></span>
* <span data-ttu-id="ba7fe-684"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url>取得 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-684"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url> gets the <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>.</span></span>
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
<span data-ttu-id="ba7fe-685">產生具有動作方法之絕對路徑的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-685">generates a URL with an absolute path for an action method.</span></span> <span data-ttu-id="ba7fe-686">URL 包含指定的 `action` 名稱和 `route` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-686">The URL contains the specified `action` name and `route` values.</span></span>

<span data-ttu-id="ba7fe-687">下列程式碼提供來自目前要求和明確值的環境 `{ page = "./Edit, id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-687">The following code provides ambient values from the current request and explicit values: `{ page = "./Edit, id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="ba7fe-688">上述程式碼會 `url` 在 `/Edit/17` [編輯] Razor 頁面包含下列頁面指示詞時，將設為：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-688">The preceding code sets `url` to  `/Edit/17` when the Edit Razor Page contains the following page directive:</span></span>

 `@page "{id:int}"`

<span data-ttu-id="ba7fe-689">如果 [編輯] 頁面不包含 `"{id:int}"` 路由範本， `url` 則為 `/Edit?id=17` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-689">If the Edit page doesn't contain the `"{id:int}"` route template, `url` is `/Edit?id=17`.</span></span>

<span data-ttu-id="ba7fe-690"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper>除了此處所述的規則之外，MVC 的行為也會增加一層複雜度：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-690">The behavior of MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> adds a layer of complexity in addition to the rules described here:</span></span>

* <span data-ttu-id="ba7fe-691">`IUrlHelper`一律會提供來自目前要求的路由值做為環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-691">`IUrlHelper` always provides the route values from the current request as ambient values.</span></span>
* <span data-ttu-id="ba7fe-692">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*)一律會將目前的 `action` 和 `controller` 路由值複製成明確的值，除非由開發人員覆寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-692">[IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*) always copies the current `action` and `controller` route values as explicit values unless overridden by the developer.</span></span>
* <span data-ttu-id="ba7fe-693">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)一律會將目前的 `page` 路由值複製成明確的值，除非予以覆寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-693">[IUrlHelper.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*) always copies the current `page` route value as an explicit value unless overridden.</span></span> <!--by the user-->
* <span data-ttu-id="ba7fe-694">`IUrlHelper.Page`除非覆寫，否則一律會以明確的值覆寫目前的 `handler` 路由值 `null` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-694">`IUrlHelper.Page` always overrides the current `handler` route value with `null` as an explicit values unless overridden.</span></span>

<span data-ttu-id="ba7fe-695">使用者通常會因為環境值的行為詳細資料而感到驚訝，因為 MVC 似乎不會遵循其本身的規則。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-695">Users are often surprised by the behavioral details of ambient values, because MVC doesn't seem to follow its own rules.</span></span> <span data-ttu-id="ba7fe-696">基於歷史和相容性的原因，某些路由值（例如 `action` 、 `controller` 、 `page` 和） `handler` 有自己的特殊案例行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-696">For historical and compatibility reasons, certain route values such as `action`, `controller`, `page`, and `handler` have their own special-case behavior.</span></span>

<span data-ttu-id="ba7fe-697">所提供的對等功能 `LinkGenerator.GetPathByAction` ，會 `LinkGenerator.GetPathByPage` 複製這些異常的 `IUrlHelper` 以實現相容性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-697">The equivalent functionality provided by `LinkGenerator.GetPathByAction` and `LinkGenerator.GetPathByPage` duplicates these anomalies of `IUrlHelper` for compatibility.</span></span>

### <a name="url-generation-process"></a><span data-ttu-id="ba7fe-698">URL 產生進程</span><span class="sxs-lookup"><span data-stu-id="ba7fe-698">URL generation process</span></span>

<span data-ttu-id="ba7fe-699">一旦找到候選端點集合之後，URL 產生演算法就會：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-699">Once the set of candidate endpoints are found, the URL generation algorithm:</span></span>

* <span data-ttu-id="ba7fe-700">反復處理端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-700">Processes the endpoints iteratively.</span></span>
* <span data-ttu-id="ba7fe-701">傳回第一個成功的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-701">Returns the first successful result.</span></span>

<span data-ttu-id="ba7fe-702">此程式的第一個步驟是所謂的**路由值失效**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-702">The first step in this process is called **route value invalidation**.</span></span>  <span data-ttu-id="ba7fe-703">路由值失效是路由決定應該使用哪些來自環境值的路由值以及應該忽略的進程。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-703">Route value invalidation is the process by which routing decides which route values from the ambient values should be used and which should be ignored.</span></span> <span data-ttu-id="ba7fe-704">會考慮每個環境值，並將其與明確值結合，或予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-704">Each ambient value is considered and either combined with the explicit values, or ignored.</span></span>

<span data-ttu-id="ba7fe-705">考慮環境值角色的最佳方式，就是他們會嘗試儲存應用程式開發人員在某些常見的情況下輸入。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-705">The best way to think about the role of ambient values is that they attempt to save application developers typing, in some common cases.</span></span> <span data-ttu-id="ba7fe-706">傳統上，環境值很有用的案例與 MVC 相關：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-706">Traditionally, the scenarios where ambient values are helpful are related to MVC:</span></span>

* <span data-ttu-id="ba7fe-707">連結至相同控制器中的另一個動作時，不需要指定控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-707">When linking to another action in the same controller, the controller name doesn't need to be specified.</span></span>
* <span data-ttu-id="ba7fe-708">連結至相同區域中的另一個控制器時，不需要指定區功能變數名稱稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-708">When linking to another controller in the same area, the area name doesn't need to be specified.</span></span>
* <span data-ttu-id="ba7fe-709">連結至相同的動作方法時，不需要指定路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-709">When linking to the same action method, route values don't need to be specified.</span></span>
* <span data-ttu-id="ba7fe-710">當連結到應用程式的另一個部分時，您不會想要在應用程式的該部分中執行不具意義的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-710">When linking to another part of the app, you don't want to carry over route values that have no meaning in that part of the app.</span></span>

<span data-ttu-id="ba7fe-711">呼叫 `LinkGenerator` 或傳回 `IUrlHelper` 的 `null` 通常是因為不了解路由值失效所造成。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-711">Calls to `LinkGenerator` or `IUrlHelper` that return `null` are usually caused by not understanding route value invalidation.</span></span> <span data-ttu-id="ba7fe-712">明確指定更多的路由值，以查看是否能解決問題，以針對路由值失效進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-712">Troubleshoot route value invalidation by explicitly specifying more of the route values to see if that solves the problem.</span></span>

<span data-ttu-id="ba7fe-713">路由值失效的假設是應用程式的 URL 配置是階層式的，並以從左至右形成的階層。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-713">Route value invalidation works on the assumption that the app's URL scheme is hierarchical, with a hierarchy formed from left-to-right.</span></span> <span data-ttu-id="ba7fe-714">請考慮使用「基本控制器路由」範本 `{controller}/{action}/{id?}` ，以直覺的方式來瞭解這在實務上如何運作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-714">Consider the basic controller route template `{controller}/{action}/{id?}` to get an intuitive sense of how this works in practice.</span></span> <span data-ttu-id="ba7fe-715">值的**變更**會使顯示在右側的所有路由值**失效**。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-715">A **change** to a value **invalidates** all of the route values that appear to the right.</span></span> <span data-ttu-id="ba7fe-716">這反映了關於階層的假設。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-716">This reflects the assumption about hierarchy.</span></span> <span data-ttu-id="ba7fe-717">如果應用程式具有的環境值 `id` ，且作業為指定了不同的值 `controller` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-717">If the app has an ambient value for `id`, and the operation specifies a different value for the `controller`:</span></span>

* <span data-ttu-id="ba7fe-718">`id`不會重複使用 `{controller}` ，因為位於的左邊 `{id?}` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-718">`id` won't be reused because `{controller}` is to the left of `{id?}`.</span></span>

<span data-ttu-id="ba7fe-719">示範此原則的一些範例如下：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-719">Some examples demonstrating this principle:</span></span>

* <span data-ttu-id="ba7fe-720">如果明確的值包含的值 `id` ，則會忽略的環境值 `id` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-720">If the explicit values contain a value for `id`, the ambient value for `id` is ignored.</span></span> <span data-ttu-id="ba7fe-721">您 `controller` 可以使用和的環境值 `action` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-721">The ambient values for `controller` and `action` can be used.</span></span>
* <span data-ttu-id="ba7fe-722">如果明確的值包含的值 `action` ，則會忽略的任何環境值 `action` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-722">If the explicit values contain a value for `action`, any ambient value for `action` is ignored.</span></span> <span data-ttu-id="ba7fe-723">可以使用的環境值 `controller` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-723">The ambient values for `controller` can be used.</span></span> <span data-ttu-id="ba7fe-724">如果的明確值與 `action` 的環境值不同 `action` ，則 `id` 不會使用此值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-724">If the explicit value for `action` is different from the ambient value for `action`, the `id` value won't be used.</span></span>  <span data-ttu-id="ba7fe-725">如果的明確值與 `action` 的環境值相同 `action` ，則 `id` 可以使用值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-725">If the explicit value for `action` is the same as the ambient value for `action`, the `id` value can be used.</span></span>
* <span data-ttu-id="ba7fe-726">如果明確的值包含的值 `controller` ，則會忽略的任何環境值 `controller` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-726">If the explicit values contain a value for `controller`, any ambient value for `controller` is ignored.</span></span> <span data-ttu-id="ba7fe-727">如果的明確值與 `controller` 的環境值不同 `controller` ，則 `action` `id` 不會使用和值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-727">If the explicit value for `controller` is different from the ambient value for `controller`, the `action` and `id` values won't be used.</span></span> <span data-ttu-id="ba7fe-728">如果的明確值與 `controller` 的環境值相同 `controller` ，則 `action` `id` 可以使用和值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-728">If the explicit value for `controller` is the same as the ambient value for `controller`, the `action` and `id` values can be used.</span></span>

<span data-ttu-id="ba7fe-729">這個程式會因為屬性路由的存在和專用的慣例路由而變得更複雜。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-729">This process is further complicated by the existence of attribute routes and dedicated conventional routes.</span></span> <span data-ttu-id="ba7fe-730">控制器慣例路由，例如 `{controller}/{action}/{id?}` 使用路由參數來指定階層。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-730">Controller conventional routes such as `{controller}/{action}/{id?}` specify a hierarchy using route parameters.</span></span> <span data-ttu-id="ba7fe-731">針對[專用的傳統路由](xref:mvc/controllers/routing#dcr)，以及控制器和頁面的[屬性路由](xref:mvc/controllers/routing#ar) Razor ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-731">For [dedicated conventional routes](xref:mvc/controllers/routing#dcr) and [attribute routes](xref:mvc/controllers/routing#ar) to controllers and Razor Pages:</span></span>

* <span data-ttu-id="ba7fe-732">有路由值的階層。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-732">There is a hierarchy of route values.</span></span>
* <span data-ttu-id="ba7fe-733">它們不會出現在範本中。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-733">They don't appear in the template.</span></span>

<span data-ttu-id="ba7fe-734">在這些情況下，URL 產生會定義**必要的值**概念。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-734">For these cases, URL generation defines the **required values** concept.</span></span> <span data-ttu-id="ba7fe-735">控制器和頁面所建立的端點， Razor 所指定的必要值可讓路由值失效。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-735">Endpoints created by controllers and Razor Pages have required values specified that allow route value invalidation to work.</span></span>

<span data-ttu-id="ba7fe-736">路由值失效演算法詳細資料：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-736">The route value invalidation algorithm in detail:</span></span>

* <span data-ttu-id="ba7fe-737">必要的值名稱會與路由參數結合，然後由左至右處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-737">The required value names are combined with the route parameters, then processed from left-to-right.</span></span>
* <span data-ttu-id="ba7fe-738">針對每個參數，會比較環境值和明確的值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-738">For each parameter, the ambient value and explicit value are compared:</span></span>
    * <span data-ttu-id="ba7fe-739">如果環境值和明確的值相同，則進程會繼續進行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-739">If the ambient value and explicit value are the same, the process continues.</span></span>
    * <span data-ttu-id="ba7fe-740">如果環境值存在且明確的值不是，則會在產生 URL 時使用環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-740">If the ambient value is present and the explicit value isn't, the ambient value is used when generating the URL.</span></span>
    * <span data-ttu-id="ba7fe-741">如果環境值不存在，且明確的值為，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-741">If the ambient value isn't present and the explicit value is, reject the ambient value and all subsequent ambient values.</span></span>
    * <span data-ttu-id="ba7fe-742">如果環境值和明確值存在，而且這兩個值不同，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-742">If the ambient value and the explicit value are present, and the two values are different, reject the ambient value and all subsequent ambient values.</span></span>

<span data-ttu-id="ba7fe-743">此時，URL 產生作業已準備好評估路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-743">At this point, the URL generation operation is ready to evaluate route constraints.</span></span> <span data-ttu-id="ba7fe-744">一組接受的值會與提供給條件約束的參數預設值結合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-744">The set of accepted values is combined with the parameter default values, which is provided to constraints.</span></span> <span data-ttu-id="ba7fe-745">如果條件約束都通過，作業就會繼續。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-745">If the constraints all pass, the operation continues.</span></span>

<span data-ttu-id="ba7fe-746">接下來，可以使用**接受的值**來展開路由範本。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-746">Next, the **accepted values** can be used to expand the route template.</span></span> <span data-ttu-id="ba7fe-747">會處理路由範本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-747">The route template is processed:</span></span>

* <span data-ttu-id="ba7fe-748">從左至右。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-748">From left-to-right.</span></span>
* <span data-ttu-id="ba7fe-749">每個參數都有其接受的值取代。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-749">Each parameter has its accepted value substituted.</span></span>
* <span data-ttu-id="ba7fe-750">有下列特殊案例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-750">With the following special cases:</span></span>
  * <span data-ttu-id="ba7fe-751">如果接受的值遺漏值，且參數具有預設值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-751">If the accepted values is missing a value and the parameter has a default value, the default value is used.</span></span>
  * <span data-ttu-id="ba7fe-752">如果接受的值遺漏值，而且參數是選擇性的，則會繼續處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-752">If the accepted values is missing a value and the parameter is optional, processing continues.</span></span>
  * <span data-ttu-id="ba7fe-753">如果遺漏選擇性參數右邊的任何路由參數具有值，則作業會失敗。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-753">If any route parameter to the right of a missing optional parameter has a value, the operation fails.</span></span>
  * <!-- review default-valued parameters optional parameters --> <span data-ttu-id="ba7fe-754">連續的預設值參數和選擇性參數會盡可能折迭。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-754">Contiguous default-valued parameters and optional parameters are collapsed where possible.</span></span>

<span data-ttu-id="ba7fe-755">明確提供並不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-755">Values explicitly provided that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ba7fe-756">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-756">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ba7fe-757">環境值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-757">Ambient Values</span></span>                     | <span data-ttu-id="ba7fe-758">明確值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-758">Explicit Values</span></span>                        | <span data-ttu-id="ba7fe-759">結果</span><span class="sxs-lookup"><span data-stu-id="ba7fe-759">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ba7fe-760">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-760">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-761">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-761">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-762">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-762">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-763">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-763">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ba7fe-764">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-764">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ba7fe-765">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-765">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-766">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-766">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-767">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-767">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

### <a name="problems-with-route-value-invalidation"></a><span data-ttu-id="ba7fe-768">路由值失效的問題</span><span class="sxs-lookup"><span data-stu-id="ba7fe-768">Problems with route value invalidation</span></span>

<span data-ttu-id="ba7fe-769">從 ASP.NET Core 3.0，先前 ASP.NET Core 版本中使用的某些 URL 產生配置，不適用於 URL 產生的效果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-769">As of ASP.NET Core 3.0, some URL generation schemes used in earlier ASP.NET Core versions don't work well with URL generation.</span></span> <span data-ttu-id="ba7fe-770">ASP.NET Core 小組計畫在未來的版本中新增功能，以解決這些需求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-770">The ASP.NET Core team plans to add features to address these needs in a future release.</span></span> <span data-ttu-id="ba7fe-771">現在最好的解決方案是使用舊版路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-771">For now the best solution is to use legacy routing.</span></span>

<span data-ttu-id="ba7fe-772">下列程式碼顯示路由不支援的 URL 產生配置範例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-772">The following code shows an example of a URL generation scheme that's not supported by routing.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

<span data-ttu-id="ba7fe-773">在上述程式碼中， `culture` route 參數是用於當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-773">In the preceding code, the `culture` route parameter is used for localization.</span></span> <span data-ttu-id="ba7fe-774">想要讓 `culture` 參數一律接受為環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-774">The desire is to have the `culture` parameter always accepted as an ambient value.</span></span> <span data-ttu-id="ba7fe-775">不過， `culture` 因為必要值的使用方式，所以不接受參數作為環境值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-775">However, the `culture` parameter is not accepted as an ambient value because of the way required values work:</span></span>

* <span data-ttu-id="ba7fe-776">在 `"default"` 路由範本中， `culture` 路由參數是在的左邊 `controller` ，因此變更 `controller` 不會使失效 `culture` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-776">In the `"default"` route template, the `culture` route parameter is to the left of `controller`, so changes to `controller` won't invalidate `culture`.</span></span>
* <span data-ttu-id="ba7fe-777">在 `"blog"` 路由範本中， `culture` 路由參數會被視為位於的右邊 `controller` ，這會出現在必要的值中。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-777">In the `"blog"` route template, the `culture` route parameter is considered to be to the right of `controller`, which appears in the required values.</span></span>

## <a name="configuring-endpoint-metadata"></a><span data-ttu-id="ba7fe-778">設定端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="ba7fe-778">Configuring endpoint metadata</span></span>

<span data-ttu-id="ba7fe-779">下列連結提供設定端點中繼資料的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-779">The following links provide information on configuring endpoint metadata:</span></span>

* [<span data-ttu-id="ba7fe-780">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="ba7fe-780">Enable Cors with endpoint routing</span></span>](xref:security/cors#enable-cors-with-endpoint-routing)
* <span data-ttu-id="ba7fe-781">使用自訂屬性的[IAuthorizationPolicyProvider 範例](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) `[MinimumAgeAuthorize]`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-781">[IAuthorizationPolicyProvider sample](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) using a custom `[MinimumAgeAuthorize]` attribute</span></span>
* <span data-ttu-id="ba7fe-782">[使用 [授權] 屬性測試驗證](xref:security/authentication/identity#test-identity)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-782">[Test authentication with the [Authorize] attribute](xref:security/authentication/identity#test-identity)</span></span>
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* <span data-ttu-id="ba7fe-783">[選取具有 [授權] 屬性的配置](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-783">[Selecting the scheme with the [Authorize] attribute](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span></span>
* <span data-ttu-id="ba7fe-784">[使用 [授權] 屬性套用原則](xref:security/authorization/policies#apply-policies-to-mvc-controllers)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-784">[Apply policies using the [Authorize] attribute](xref:security/authorization/policies#apply-policies-to-mvc-controllers)</span></span>
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a><span data-ttu-id="ba7fe-785">搭配 RequireHost 的路由中的主機比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-785">Host matching in routes with RequireHost</span></span>

<span data-ttu-id="ba7fe-786"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>將條件約束套用至需要指定之主控制項的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-786"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*> applies a constraint to the route which requires the specified host.</span></span> <span data-ttu-id="ba7fe-787">`RequireHost`或[[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute)參數可以是：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-787">The `RequireHost` or [[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute) parameter can be:</span></span>

* <span data-ttu-id="ba7fe-788">主機： `www.domain.com` ，符合 `www.domain.com` 任何埠。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-788">Host: `www.domain.com`, matches `www.domain.com` with any port.</span></span>
* <span data-ttu-id="ba7fe-789">具有萬用字元的主機： `*.domain.com` 、符合 `www.domain.com` 、 `subdomain.domain.com` 或 `www.subdomain.domain.com` 任何埠上的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-789">Host with wildcard: `*.domain.com`, matches `www.domain.com`, `subdomain.domain.com`, or `www.subdomain.domain.com` on any port.</span></span>
* <span data-ttu-id="ba7fe-790">埠： `*:5000` ，符合任何主機的通訊埠5000。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-790">Port: `*:5000`, matches port 5000 with any host.</span></span>
* <span data-ttu-id="ba7fe-791">主機和埠： `www.domain.com:5000` 或 `*.domain.com:5000` 符合主機和埠。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-791">Host and port: `www.domain.com:5000` or `*.domain.com:5000`, matches host and port.</span></span>

<span data-ttu-id="ba7fe-792">您可以使用或來指定多個參數 `RequireHost` `[Host]` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-792">Multiple parameters can be specified using `RequireHost` or `[Host]`.</span></span> <span data-ttu-id="ba7fe-793">條件約束符合任何參數的有效主機。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-793">The constraint  matches hosts valid for any of the parameters.</span></span> <span data-ttu-id="ba7fe-794">例如，會比對 `[Host("domain.com", "*.domain.com")]` `domain.com` 、 `www.domain.com` 和 `subdomain.domain.com` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-794">For example, `[Host("domain.com", "*.domain.com")]` matches `domain.com`, `www.domain.com`, and `subdomain.domain.com`.</span></span>

<span data-ttu-id="ba7fe-795">下列程式碼會使用 `RequireHost` 來要求路由上指定的主機：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-795">The following code uses `RequireHost` to require the specified host on the route:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

<span data-ttu-id="ba7fe-796">下列程式碼會在 `[Host]` 控制器上使用屬性來要求任何指定的主機：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-796">The following code uses the `[Host]` attribute on the controller to require any of the specified hosts:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

<span data-ttu-id="ba7fe-797">當 `[Host]` 屬性同時套用至控制器和動作方法時：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-797">When the `[Host]` attribute is applied to both the controller and action method:</span></span>

* <span data-ttu-id="ba7fe-798">會使用動作上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-798">The attribute on the action is used.</span></span>
* <span data-ttu-id="ba7fe-799">已忽略控制器屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-799">The controller attribute is ignored.</span></span>

## <a name="performance-guidance-for-routing"></a><span data-ttu-id="ba7fe-800">路由的效能指引</span><span class="sxs-lookup"><span data-stu-id="ba7fe-800">Performance guidance for routing</span></span>

<span data-ttu-id="ba7fe-801">大部分的路由都已在 ASP.NET Core 3.0 中更新，以提高效能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-801">Most of routing was updated in ASP.NET Core 3.0 to increase performance.</span></span>

<span data-ttu-id="ba7fe-802">當應用程式發生效能問題時，通常會懷疑路由是問題。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-802">When an app has performance problems, routing is often suspected as the problem.</span></span> <span data-ttu-id="ba7fe-803">路由的原因是，控制器和頁面之類的架構，會 Razor 回報在其記錄訊息內所花費的時間量。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-803">The reason routing is suspected is that frameworks like controllers and Razor Pages report the amount of time spent inside the framework in their logging messages.</span></span> <span data-ttu-id="ba7fe-804">當控制器回報的時間與要求的總時間之間有顯著的差異時：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-804">When there's a significant difference between the time reported by controllers and the total time of the request:</span></span>

* <span data-ttu-id="ba7fe-805">開發人員會將其應用程式程式碼排除為問題的來源。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-805">Developers eliminate their app code as the source of the problem.</span></span>
* <span data-ttu-id="ba7fe-806">通常會假設路由是原因。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-806">It's common to assume routing is the cause.</span></span>

<span data-ttu-id="ba7fe-807">路由會使用數千個端點來測試效能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-807">Routing is performance tested using thousands of endpoints.</span></span> <span data-ttu-id="ba7fe-808">一般的應用程式不太可能會遇到太大的效能問題。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-808">It's unlikely that a typical app will encounter a performance problem just by being too large.</span></span> <span data-ttu-id="ba7fe-809">緩慢路由效能最常見的根本原因通常是行為不佳的自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-809">The most common root cause of slow routing performance is usually a badly-behaving custom middleware.</span></span>

<span data-ttu-id="ba7fe-810">下列程式碼範例示範縮小延遲來源的基本技巧：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-810">This following code sample demonstrates a basic technique for narrowing down the source of delay:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

<span data-ttu-id="ba7fe-811">若要進行時間路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-811">To time routing:</span></span>

* <span data-ttu-id="ba7fe-812">使用上述程式碼中所示的計時中介軟體複本來交錯每個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-812">Interleave each middleware with a copy of the timing middleware shown in the preceding code.</span></span>
* <span data-ttu-id="ba7fe-813">新增唯一識別碼，將計時資料與程式碼相互關聯。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-813">Add a unique identifier to correlate the timing data with the code.</span></span>

<span data-ttu-id="ba7fe-814">這是將延遲縮小到最大的基本方式，例如，超過 `10ms` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-814">This is a basic way to narrow down the delay when it's significant, for example, more than `10ms`.</span></span>  <span data-ttu-id="ba7fe-815">`Time 2`從 `Time 1` 報表減去中介軟體內所花費的時間 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-815">Subtracting `Time 2` from `Time 1` reports the time spent inside the `UseRouting` middleware.</span></span>

<span data-ttu-id="ba7fe-816">下列程式碼會針對上述計時程式碼使用更精簡的方法：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-816">The following code uses a more compact approach to the preceding timing code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a><span data-ttu-id="ba7fe-817">可能昂貴的路由功能</span><span class="sxs-lookup"><span data-stu-id="ba7fe-817">Potentially expensive routing features</span></span>

<span data-ttu-id="ba7fe-818">下列清單提供一些路由功能的深入解析，相較于基本的路由範本，這會相當耗費資源：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-818">The following list provides some insight into routing features that are relatively expensive compared with basic route templates:</span></span>

* <span data-ttu-id="ba7fe-819">正則運算式：可以撰寫複雜的正則運算式，或具有少量輸入的長期執行時間。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-819">Regular expressions: It's possible to write regular expressions that are complex, or have long running time with a small amount of input.</span></span>

* <span data-ttu-id="ba7fe-820">複雜區段（ `{x}-{y}-{z}` ）：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-820">Complex segments (`{x}-{y}-{z}`):</span></span> 
  * <span data-ttu-id="ba7fe-821">比剖析一般 URL 路徑區段高得多。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-821">Are significantly more expensive than parsing a regular URL path segment.</span></span>
  * <span data-ttu-id="ba7fe-822">導致已配置更多子字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-822">Result in many more substrings being allocated.</span></span>
  * <span data-ttu-id="ba7fe-823">在 ASP.NET Core 3.0 路由效能更新中，未更新複雜的區段邏輯。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-823">The complex segment logic was not updated in ASP.NET Core 3.0 routing performance update.</span></span>

* <span data-ttu-id="ba7fe-824">同步資料存取：許多複雜的應用程式在其路由中都具有資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-824">Synchronous data access: Many complex apps have database access as part of their routing.</span></span> <span data-ttu-id="ba7fe-825">ASP.NET Core 2.2 和較舊的路由可能不會提供支援資料庫存取路由的正確擴充點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-825">ASP.NET Core 2.2 and earlier routing might not provide the right extensibility points to support database access routing.</span></span> <span data-ttu-id="ba7fe-826">例如，、 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 和 <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> 都是同步的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-826">For example, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, and <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are synchronous.</span></span> <span data-ttu-id="ba7fe-827">和等擴充點 <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> 都是非同步。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-827">Extensibility points such as <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> and <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> are asynchronous.</span></span>

## <a name="guidance-for-library-authors"></a><span data-ttu-id="ba7fe-828">程式庫作者指引</span><span class="sxs-lookup"><span data-stu-id="ba7fe-828">Guidance for library authors</span></span>

<span data-ttu-id="ba7fe-829">本章節包含程式庫作者在路由之上建立的指導方針。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-829">This section contains guidance for library authors building on top of routing.</span></span> <span data-ttu-id="ba7fe-830">這些詳細資料的目的是要確保應用程式開發人員能夠使用延伸路由的程式庫和架構。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-830">These details are intended to ensure that app developers have a good experience using libraries and frameworks that extend routing.</span></span>

### <a name="define-endpoints"></a><span data-ttu-id="ba7fe-831">定義端點</span><span class="sxs-lookup"><span data-stu-id="ba7fe-831">Define endpoints</span></span>

<span data-ttu-id="ba7fe-832">若要建立使用路由進行 URL 比對的架構，請先定義以為基礎的使用者體驗 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-832">To create a framework that uses routing for URL matching, start by defining a user experience that builds on top of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>

<span data-ttu-id="ba7fe-833">**請**在之上建立組建 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-833">**DO** build on top of <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span> <span data-ttu-id="ba7fe-834">這可讓使用者使用其他 ASP.NET Core 功能來撰寫您的架構，而不會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-834">This allows users to compose your framework with other ASP.NET Core features without confusion.</span></span> <span data-ttu-id="ba7fe-835">每個 ASP.NET Core 範本都包含路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-835">Every ASP.NET Core template includes routing.</span></span> <span data-ttu-id="ba7fe-836">假設路由存在且熟悉使用者。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-836">Assume routing is present and familiar for users.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="ba7fe-837">**確實**從對所執行的呼叫傳回密封的實體型別 `MapMyFramework(...)` <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-837">**DO** return a sealed concrete type from a call to `MapMyFramework(...)` that implements <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>.</span></span> <span data-ttu-id="ba7fe-838">大部分 `Map...` 的架構方法會遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-838">Most framework `Map...` methods follow this pattern.</span></span> <span data-ttu-id="ba7fe-839">`IEndpointConventionBuilder`介面：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-839">The `IEndpointConventionBuilder` interface:</span></span>

* <span data-ttu-id="ba7fe-840">允許複合性中繼資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-840">Allows composability of metadata.</span></span>
* <span data-ttu-id="ba7fe-841">的目標是各種擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-841">Is targeted by a variety of extension methods.</span></span>

<span data-ttu-id="ba7fe-842">宣告您自己的型別可讓您將自己的架構特定功能加入至產生器。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-842">Declaring your own type allows you to add your own framework-specific functionality to the builder.</span></span> <span data-ttu-id="ba7fe-843">將架構宣告的產生器和轉送呼叫包裝在一起是正常的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-843">It's ok to wrap a framework-declared builder and forward calls to it.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="ba7fe-844">**請考慮**撰寫自己 <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> 的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-844">**CONSIDER** writing your own <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>.</span></span> <span data-ttu-id="ba7fe-845">`EndpointDataSource`是用來宣告和更新端點集合的低層級基本類型。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-845">`EndpointDataSource` is the low-level primitive for declaring and updating a collection of endpoints.</span></span> <span data-ttu-id="ba7fe-846">`EndpointDataSource`是控制器和頁面所使用的強大 API Razor 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-846">`EndpointDataSource` is a powerful API used by controllers and Razor Pages.</span></span>

<span data-ttu-id="ba7fe-847">路由測試有一個不會更新資料來源的[基本範例](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-847">The routing tests have a [basic example](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17) of a non-updating data source.</span></span>

<span data-ttu-id="ba7fe-848">預設**不會**嘗試註冊 `EndpointDataSource` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-848">**DO NOT** attempt to register an `EndpointDataSource` by default.</span></span> <span data-ttu-id="ba7fe-849">要求使用者在中註冊您的架構 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-849">Require users to register your framework in <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span> <span data-ttu-id="ba7fe-850">路由的原理是預設不會包含任何內容，也 `UseEndpoints` 就是註冊端點的位置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-850">The philosophy of routing is that nothing is included by default, and that `UseEndpoints` is the place to register endpoints.</span></span>

### <a name="creating-routing-integrated-middleware"></a><span data-ttu-id="ba7fe-851">建立路由整合中介軟體</span><span class="sxs-lookup"><span data-stu-id="ba7fe-851">Creating routing-integrated middleware</span></span>

<span data-ttu-id="ba7fe-852">**請考慮**將元資料類型定義為介面。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-852">**CONSIDER** defining metadata types as an interface.</span></span>

<span data-ttu-id="ba7fe-853">您可以使用中繼資料**類型做為**類別和方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-853">**DO** make it possible to use metadata types as an attribute on classes and methods.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

<span data-ttu-id="ba7fe-854">控制器和頁面之類 Razor 的架構支援將中繼資料屬性套用至類型和方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-854">Frameworks like controllers and Razor Pages support applying metadata attributes to types and methods.</span></span> <span data-ttu-id="ba7fe-855">如果您宣告元資料類型：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-855">If you declare metadata types:</span></span>

* <span data-ttu-id="ba7fe-856">使其可做為[屬性](/dotnet/csharp/programming-guide/concepts/attributes/)存取。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-856">Make them accessible as [attributes](/dotnet/csharp/programming-guide/concepts/attributes/).</span></span>
* <span data-ttu-id="ba7fe-857">大部分的使用者都很熟悉套用屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-857">Most users are familiar with applying attributes.</span></span>

<span data-ttu-id="ba7fe-858">將元資料類型宣告為介面，會增加另一層彈性：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-858">Declaring a metadata type as an interface adds another layer of flexibility:</span></span>

* <span data-ttu-id="ba7fe-859">介面是可組合的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-859">Interfaces are composable.</span></span>
* <span data-ttu-id="ba7fe-860">開發人員可以宣告自己的類型，結合多個原則。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-860">Developers can declare their own types that combine multiple policies.</span></span>

<span data-ttu-id="ba7fe-861">**確實**能夠覆寫中繼資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-861">**DO** make it possible to override metadata, as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

<span data-ttu-id="ba7fe-862">遵循這些指導方針的最佳方式是避免定義**標記中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-862">The best way to follow these guidelines is to avoid defining **marker metadata**:</span></span>

* <span data-ttu-id="ba7fe-863">請不要只尋找元資料類型是否存在。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-863">Don't just look for the presence of a metadata type.</span></span>
* <span data-ttu-id="ba7fe-864">在中繼資料上定義屬性，並檢查屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-864">Define a property on the metadata and check the property.</span></span>

<span data-ttu-id="ba7fe-865">元資料集合會進行排序，並依優先順序支援覆寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-865">The metadata collection is ordered and supports overriding by priority.</span></span> <span data-ttu-id="ba7fe-866">在控制器的案例中，動作方法上的中繼資料是最明確的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-866">In the case of controllers, metadata on the action method is most specific.</span></span>

<span data-ttu-id="ba7fe-867">**讓中間**件適用于且不使用路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-867">**DO** make middleware useful with and without routing.</span></span>

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

<span data-ttu-id="ba7fe-868">作為此指導方針的範例，請考慮 `UseAuthorization` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-868">As an example of this guideline, consider the `UseAuthorization` middleware.</span></span> <span data-ttu-id="ba7fe-869">授權中介軟體可讓您傳入回退原則。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-869">The authorization middleware allows you to pass in a fallback policy.</span></span> <!-- shown where?  (shown here) --> <span data-ttu-id="ba7fe-870">如果指定了回溯原則，則會套用至兩者：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-870">The fallback policy, if specified, applies to both:</span></span>

* <span data-ttu-id="ba7fe-871">沒有指定之原則的端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-871">Endpoints without a specified policy.</span></span>
* <span data-ttu-id="ba7fe-872">不符合端點的要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-872">Requests that don't match an endpoint.</span></span>

<span data-ttu-id="ba7fe-873">這可讓授權中介軟體適用于路由內容以外的地方。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-873">This makes the authorization middleware useful outside of the context of routing.</span></span> <span data-ttu-id="ba7fe-874">授權中介軟體可用於傳統中介軟體程式設計。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-874">The authorization middleware can be used for traditional middleware programming.</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ba7fe-875">路由會負責將要求 Uri 對應至端點，並將傳入要求分派至這些端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-875">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="ba7fe-876">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-876">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ba7fe-877">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-877">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ba7fe-878">使用來自應用程式的路由資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-878">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="ba7fe-879">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-879">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ba7fe-880"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-880">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ba7fe-881">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-881">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="ba7fe-882">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-882">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ba7fe-883">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-883">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba7fe-884">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-884">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="ba7fe-885">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-885">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="ba7fe-886">如需頁面中路由慣例的詳細資訊 Razor ，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-886">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ba7fe-887">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ba7fe-887">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ba7fe-888">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ba7fe-888">Routing basics</span></span>

<span data-ttu-id="ba7fe-889">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-889">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="ba7fe-890">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-890">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="ba7fe-891">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-891">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="ba7fe-892">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-892">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="ba7fe-893">開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專用的慣例路由，在特殊情況下，將額外的簡易路由新增到應用程式的高流量區域。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-893">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span> <span data-ttu-id="ba7fe-894">特殊情況的範例包括： blog 和電子商務端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-894">Specialized situations examples include, blog and ecommerce endpoints.</span></span>

<span data-ttu-id="ba7fe-895">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-895">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="ba7fe-896">這表示相同邏輯資源上的許多作業（例如 GET 和 POST）都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-896">This means that many operations, for example, GET, and POST, on the same logical resource use the same URL.</span></span> <span data-ttu-id="ba7fe-897">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-897">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

Razor<span data-ttu-id="ba7fe-898">頁面應用程式會使用預設的傳統路由，在應用程式的*Pages*資料夾中提供已命名的資源。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-898"> Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="ba7fe-899">還有其他慣例可讓您自訂 Razor 頁面路由行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-899">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="ba7fe-900">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-900">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ba7fe-901">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-901">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="ba7fe-902">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-902">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="ba7fe-903">路由會使用*端點*（ `Endpoint` ）來代表應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-903">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="ba7fe-904">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-904">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="ba7fe-905">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-905">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="ba7fe-906">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-906">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ba7fe-907">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-907">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ba7fe-908">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-908">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ba7fe-909"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-909"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ba7fe-910">應用程式模型（例如 MVC/ Razor Pages）會註冊其所有端點，其具有可預測的路由案例執行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-910">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ba7fe-911">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-911">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="ba7fe-912">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-912">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="ba7fe-913">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-913">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="ba7fe-914">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-914">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ba7fe-915">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-915">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="ba7fe-916">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-916">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="ba7fe-917">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-917">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="ba7fe-918">隨著 ASP.NET Core 2.2 中的端點路由發行，端點連結僅限於 MVC/ Razor pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-918">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="ba7fe-919">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-919">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="ba7fe-920">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-920">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="ba7fe-921">[ASP.NET CORE mvc](xref:mvc/overview)會將路由新增至中介軟體管線，作為其設定的一部分，並處理 MVC 和 Razor 頁面應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-921">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="ba7fe-922">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-922">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="ba7fe-923">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-923">URL matching</span></span>

<span data-ttu-id="ba7fe-924">URL 比對是路由用來將傳入要求分派給「端點」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-924">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="ba7fe-925">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-925">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ba7fe-926">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-926">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ba7fe-927">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-927">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="ba7fe-928">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-928">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="ba7fe-929">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-929">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="ba7fe-930">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-930">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="ba7fe-931">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-931">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="ba7fe-932">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-932">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="ba7fe-933">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-933"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="ba7fe-934">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-934">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="ba7fe-935">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-935">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="ba7fe-936">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-936">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="ba7fe-937">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-937">Routes can be nested inside of one another.</span></span> <span data-ttu-id="ba7fe-938"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-938">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="ba7fe-939">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-939">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="ba7fe-940"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-940">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="ba7fe-941">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="ba7fe-941">URL generation with LinkGenerator</span></span>

<span data-ttu-id="ba7fe-942">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-942">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ba7fe-943">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-943">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="ba7fe-944">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-944">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="ba7fe-945"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>是可從[DI](xref:fundamentals/dependency-injection)抓取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-945"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ba7fe-946">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-946">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="ba7fe-947">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-947">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="ba7fe-948">連結產生器背後支援的概念為「位址」\*\* 和「位址配置」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-948">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="ba7fe-949">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-949">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="ba7fe-950">例如，許多使用者熟悉的路由名稱和路由值案例， Razor 都是以位址配置的形式來執行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-950">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="ba7fe-951">連結產生器可以透過下列擴充方法連結至 MVC/ Razor pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-951">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="ba7fe-952">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-952">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="ba7fe-953">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-953">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="ba7fe-954">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-954">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="ba7fe-955">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-955">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="ba7fe-956">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-956">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="ba7fe-957">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-957">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="ba7fe-958">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-958"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="ba7fe-959">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-959">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="ba7fe-960">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-960">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="ba7fe-961">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-961">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="ba7fe-962">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-962">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="ba7fe-963"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-963">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="ba7fe-964">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-964">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="ba7fe-965">擴充方法</span><span class="sxs-lookup"><span data-stu-id="ba7fe-965">Extension Method</span></span>   | <span data-ttu-id="ba7fe-966">描述</span><span class="sxs-lookup"><span data-stu-id="ba7fe-966">Description</span></span>                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="ba7fe-967">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-967">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="ba7fe-968">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-968">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="ba7fe-969">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-969">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="ba7fe-970">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-970">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="ba7fe-971">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-971">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="ba7fe-972">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-972">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="ba7fe-973">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-973">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="ba7fe-974">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-974">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="ba7fe-975">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-975">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="ba7fe-976">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-976">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="ba7fe-977">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="ba7fe-977">Differences from earlier versions of routing</span></span>

<span data-ttu-id="ba7fe-978">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-978">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="ba7fe-979">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-979">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="ba7fe-980">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-980">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="ba7fe-981">請使用 2.1[相容性版本](xref:mvc/compatibility-version)（ `.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)` ）繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-981">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="ba7fe-982">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-982">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="ba7fe-983">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-983">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ba7fe-984">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-984">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="ba7fe-985">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-985">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="ba7fe-986">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-986">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="ba7fe-987">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-987">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="ba7fe-988">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-988">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="ba7fe-989">Razor當嘗試連結至不存在的控制器/動作或頁面時，MVC/Pages 搭配傳統路由使用的連結產生會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-989">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="ba7fe-990">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-990">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ba7fe-991">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-991">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="ba7fe-992">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-992">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="ba7fe-993">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-993">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="ba7fe-994">不過，如果動作不存在，則端點路由會產生空字串。\*\*</span><span class="sxs-lookup"><span data-stu-id="ba7fe-994">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="ba7fe-995">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-995">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="ba7fe-996">連結產生「環境值失效演算法」\*\* 在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-996">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="ba7fe-997">「環境值失效」\*\* 是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-997">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="ba7fe-998">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-998">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="ba7fe-999">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-999">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="ba7fe-1000">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1000">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="ba7fe-1001">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1001">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="ba7fe-1002">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1002">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ba7fe-1003">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1003">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="ba7fe-1004">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1004">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="ba7fe-1005">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1005">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="ba7fe-1006">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1006">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="ba7fe-1007">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1007">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="ba7fe-1008">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1008">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="ba7fe-1009">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1009">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="ba7fe-1010">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1010">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="ba7fe-1011">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1011">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="ba7fe-1012">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1012">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="ba7fe-1013">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1013">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="ba7fe-1014">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1014">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="ba7fe-1015">路由</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1015">Route</span></span>              | <span data-ttu-id="ba7fe-1016">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1016">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="ba7fe-1017">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1017">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="ba7fe-1018">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1018">Middleware example</span></span>

<span data-ttu-id="ba7fe-1019">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1019">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="ba7fe-1020">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1020">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

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

### <a name="create-routes"></a><span data-ttu-id="ba7fe-1021">建立路由</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1021">Create routes</span></span>

<span data-ttu-id="ba7fe-1022">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1022">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="ba7fe-1023">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1023">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="ba7fe-1024"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1024"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ba7fe-1025"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1025"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ba7fe-1026">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1026">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="ba7fe-1027">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1027">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ba7fe-1028">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1028">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="ba7fe-1029">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1029">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="ba7fe-1030">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1030">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="ba7fe-1031">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1031">Route parameters are named.</span></span> <span data-ttu-id="ba7fe-1032">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1032">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="ba7fe-1033">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1033">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="ba7fe-1034">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1034">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="ba7fe-1035">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1035">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="ba7fe-1036">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1036">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="ba7fe-1037">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1037">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="ba7fe-1038">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1038">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="ba7fe-1039">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1039">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="ba7fe-1040">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1040">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="ba7fe-1041">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1041">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="ba7fe-1042">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1042">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="ba7fe-1043">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1043">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="ba7fe-1044">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1044">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="ba7fe-1045"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1045">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="ba7fe-1046">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1046">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="ba7fe-1047">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1047">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="ba7fe-1048">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1048">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="ba7fe-1049">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1049">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="ba7fe-1050">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1050">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ba7fe-1051">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1051">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ba7fe-1052">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1052">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ba7fe-1053">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1053">Default values can be specified in the route template.</span></span> <span data-ttu-id="ba7fe-1054">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1054">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="ba7fe-1055">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1055">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="ba7fe-1056">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1056">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="ba7fe-1057">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1057">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="ba7fe-1059">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1059">Route class URL generation</span></span>

<span data-ttu-id="ba7fe-1060"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1060">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="ba7fe-1061">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1061">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="ba7fe-1062">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1062">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="ba7fe-1063">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1063">What values would be produced?</span></span> <span data-ttu-id="ba7fe-1064">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1064">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="ba7fe-1065">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1065">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ba7fe-1066">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1066">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="ba7fe-1067">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1067">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="ba7fe-1068">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1068">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="ba7fe-1069">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1069">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="ba7fe-1070">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1070">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="ba7fe-1071">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1071">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ba7fe-1072">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1072">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="ba7fe-1073">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1073">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="ba7fe-1074">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1074">Use Routing Middleware</span></span>

<span data-ttu-id="ba7fe-1075">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1075">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="ba7fe-1076">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1076">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="ba7fe-1077">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1077">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="ba7fe-1078">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1078">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="ba7fe-1079"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>：僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1079"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>: Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="ba7fe-1080">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1080">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="ba7fe-1081">URI</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1081">URI</span></span>                    | <span data-ttu-id="ba7fe-1082">回應</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1082">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="ba7fe-1083">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1083">Hello!</span></span> <span data-ttu-id="ba7fe-1084">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1084">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="ba7fe-1085">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1085">Hello!</span></span> <span data-ttu-id="ba7fe-1086">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1086">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="ba7fe-1087">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1087">Hello!</span></span> <span data-ttu-id="ba7fe-1088">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1088">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="ba7fe-1089">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1089">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="ba7fe-1090">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1090">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="ba7fe-1091">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1091">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="ba7fe-1092">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1092">The request falls through, no match.</span></span>              |

<span data-ttu-id="ba7fe-1093">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1093">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="ba7fe-1094">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1094">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="ba7fe-1095">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1095">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="ba7fe-1096">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1096">Route template reference</span></span>

<span data-ttu-id="ba7fe-1097">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1097">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="ba7fe-1098">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1098">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="ba7fe-1099">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1099">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="ba7fe-1100">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1100">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ba7fe-1101">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1101">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ba7fe-1102">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1102">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="ba7fe-1103">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1103">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="ba7fe-1104">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1104">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ba7fe-1105">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1105">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ba7fe-1106">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1106">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ba7fe-1107">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1107">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="ba7fe-1108">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1108">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ba7fe-1109">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1109">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ba7fe-1110">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1110">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="ba7fe-1111">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1111">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ba7fe-1112">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1112">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ba7fe-1113">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1113">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ba7fe-1114">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1114">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ba7fe-1115">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1115">Note the escaped forward slash.</span></span> <span data-ttu-id="ba7fe-1116">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1116">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="ba7fe-1117">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1117">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="ba7fe-1118">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1118">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ba7fe-1119">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1119">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ba7fe-1120">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1120">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ba7fe-1121">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1121">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="ba7fe-1122">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1122">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ba7fe-1123">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1123">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ba7fe-1124">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1124">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="ba7fe-1125">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1125">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="ba7fe-1126">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1126">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="ba7fe-1127">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1127">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ba7fe-1128">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1128">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ba7fe-1129">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1129">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ba7fe-1130">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1130">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="ba7fe-1131">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1131">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="ba7fe-1132">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1132">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="ba7fe-1133">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1133">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="ba7fe-1134">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1134">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="ba7fe-1135">路由範本</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1135">Route Template</span></span>                           | <span data-ttu-id="ba7fe-1136">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1136">Example Matching URI</span></span>    | <span data-ttu-id="ba7fe-1137">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1137">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ba7fe-1138">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1138">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ba7fe-1139">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1139">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ba7fe-1140">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1140">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ba7fe-1141">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1141">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ba7fe-1142">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1142">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ba7fe-1143">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1143">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="ba7fe-1144">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1144">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ba7fe-1145">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1145">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="ba7fe-1146">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1146">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="ba7fe-1147">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1147">Reserved routing names</span></span>

<span data-ttu-id="ba7fe-1148">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1148">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="ba7fe-1149">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1149">Route constraint reference</span></span>

<span data-ttu-id="ba7fe-1150">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1150">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ba7fe-1151">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1151">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="ba7fe-1152">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1152">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ba7fe-1153">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1153">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ba7fe-1154">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1154">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ba7fe-1155">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1155">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="ba7fe-1156">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1156">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="ba7fe-1157">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1157">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ba7fe-1158">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1158">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="ba7fe-1159">條件約束</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1159">Constraint</span></span> | <span data-ttu-id="ba7fe-1160">範例</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1160">Example</span></span> | <span data-ttu-id="ba7fe-1161">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1161">Example matches</span></span> | <span data-ttu-id="ba7fe-1162">備註</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1162">Notes</span></span> |
|------------|---------|-----------------|-------|
| `int` | `{id:int}` | <span data-ttu-id="ba7fe-1163">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1163">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-1164">符合任何整數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1164">Matches any integer.</span></span>|
| `bool` | `{active:bool}` | <span data-ttu-id="ba7fe-1165">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1165">`true`, `FALSE`</span></span> | <span data-ttu-id="ba7fe-1166">符合 `true` 或 `false` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1166">Matches `true` or `false`.</span></span> <span data-ttu-id="ba7fe-1167">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1167">Case-insensitive.</span></span>|
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ba7fe-1168">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1168">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ba7fe-1169">符合不因文化特性而異的有效 `DateTime` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1169">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1170">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1170">See preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="ba7fe-1171">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1171">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ba7fe-1172">符合不因文化特性而異的有效 `decimal` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1172">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1173">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1173">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ba7fe-1174">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1174">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-1175">符合不因文化特性而異的有效 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1175">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1176">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1176">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ba7fe-1177">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1177">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-1178">符合不因文化特性而異的有效 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1178">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1179">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1179">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="ba7fe-1180">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1180">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ba7fe-1181">符合有效的 `Guid` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1181">Matches a valid `Guid` value.</span></span>|
| `long` | `{ticks:long}` | <span data-ttu-id="ba7fe-1182">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1182">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-1183">符合有效的 `long` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1183">Matches a valid `long` value.</span></span>|
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ba7fe-1184">字串必須至少有4個字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1184">String must be at least 4 characters.</span></span>|
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | <span data-ttu-id="ba7fe-1185">字串最多可以有8個字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1185">String has maximum of 8 characters.</span></span>|
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ba7fe-1186">字串長度必須剛好12個字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1186">String must be exactly 12 characters long.</span></span>|
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ba7fe-1187">字串必須至少為8個字元，且最多可以有16個字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1187">String must be at least 8 and has maximum of 16 characters.</span></span>|
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ba7fe-1188">整數值必須至少為18。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1188">Integer value must be at least 18.</span></span>|
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ba7fe-1189">整數值上限為120。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1189">Integer value maximum of 120.</span></span>|
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ba7fe-1190">整數值必須至少為18，而最大值為120。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1190">Integer value must be at least 18 and maximum of 120.</span></span>|
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ba7fe-1191">字串必須包含一或多個字母字元 `a` - `z` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1191">String must consist of one or more alphabetical characters `a`-`z`.</span></span> <span data-ttu-id="ba7fe-1192">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1192">Case-insensitive.</span></span>|
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ba7fe-1193">字串必須符合正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1193">String must match the regular expression.</span></span> <span data-ttu-id="ba7fe-1194">請參閱定義正則運算式的秘訣。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1194">See tips about defining a regular expression.</span></span>|
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ba7fe-1195">用來強制在 URL 產生期間出現非參數值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1195">Used to enforce that a non-parameter value is present during URL generation.</span></span>|

<span data-ttu-id="ba7fe-1196">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1196">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ba7fe-1197">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1197">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ba7fe-1198">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1198">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ba7fe-1199">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1199">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="ba7fe-1200">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1200">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ba7fe-1201">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1201">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ba7fe-1202">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1202">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="ba7fe-1203">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1203">Regular expressions</span></span>

<span data-ttu-id="ba7fe-1204">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1204">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ba7fe-1205">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1205">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ba7fe-1206">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1206">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="ba7fe-1207">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1207">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ba7fe-1208">若要在路由中使用正則運算式 `^\d{3}-\d{2}-\d{4}$` ：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1208">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing:</span></span>

* <span data-ttu-id="ba7fe-1209">運算式必須在字串中提供單一反斜線 `\` 字元，做為原始程式碼中的雙反斜線 `\\` 字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1209">The expression must have the single backslash `\` characters provided in the string as double backslash `\\` characters in the source code.</span></span>
* <span data-ttu-id="ba7fe-1210">正則運算式必須是， `\\` 才能將 `\` 字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1210">The regular expression must us `\\` in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="ba7fe-1211">`\\`使用[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值時，不需要正則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1211">The regular expression doesn't require `\\` when using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="ba7fe-1212">若要 escape 路由參數分隔符號 `{` 、 `}` 、 `[` 、，請將 `]` 運算式中的字元、、、加倍 `{{` `}` `[[` `]]` 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1212">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression `{{`, `}`, `[[`, `]]`.</span></span> <span data-ttu-id="ba7fe-1213">下表顯示正則運算式和已轉義的版本：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1213">The following table shows a regular expression and the escaped version:</span></span>

| <span data-ttu-id="ba7fe-1214">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1214">Regular Expression</span></span>    | <span data-ttu-id="ba7fe-1215">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1215">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ba7fe-1216">路由中使用的正則運算式通常會以插入 `^` 號字元開頭，並比對字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1216">Regular expressions used in routing often start with the caret `^` character and match starting position of the string.</span></span> <span data-ttu-id="ba7fe-1217">這些運算式通常會以貨幣符號 `$` 字元結尾，並比對字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1217">The expressions often end with the dollar sign `$` character and match end of the string.</span></span> <span data-ttu-id="ba7fe-1218">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1218">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="ba7fe-1219">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1219">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ba7fe-1220">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1220">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="ba7fe-1221">運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1221">Expression</span></span>   | <span data-ttu-id="ba7fe-1222">String</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1222">String</span></span>    | <span data-ttu-id="ba7fe-1223">比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1223">Match</span></span> | <span data-ttu-id="ba7fe-1224">註解</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1224">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1225">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1225">hello</span></span>     | <span data-ttu-id="ba7fe-1226">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1226">Yes</span></span>   | <span data-ttu-id="ba7fe-1227">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1227">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1228">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1228">123abc456</span></span> | <span data-ttu-id="ba7fe-1229">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1229">Yes</span></span>   | <span data-ttu-id="ba7fe-1230">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1230">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1231">mz</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1231">mz</span></span>        | <span data-ttu-id="ba7fe-1232">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1232">Yes</span></span>   | <span data-ttu-id="ba7fe-1233">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1233">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1234">MZ</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1234">MZ</span></span>        | <span data-ttu-id="ba7fe-1235">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1235">Yes</span></span>   | <span data-ttu-id="ba7fe-1236">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1236">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-1237">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1237">hello</span></span>     | <span data-ttu-id="ba7fe-1238">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1238">No</span></span>    | <span data-ttu-id="ba7fe-1239">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1239">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-1240">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1240">123abc456</span></span> | <span data-ttu-id="ba7fe-1241">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1241">No</span></span>    | <span data-ttu-id="ba7fe-1242">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1242">See `^` and `$` above</span></span> |

<span data-ttu-id="ba7fe-1243">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1243">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ba7fe-1244">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1244">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ba7fe-1245">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1245">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ba7fe-1246">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1246">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ba7fe-1247">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1247">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="ba7fe-1248">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1248">Custom route constraints</span></span>

<span data-ttu-id="ba7fe-1249">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1249">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ba7fe-1250"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1250">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ba7fe-1251">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1251">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="ba7fe-1252"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1252">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="ba7fe-1253">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1253">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ba7fe-1254">例如：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1254">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="ba7fe-1255">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1255">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="ba7fe-1256">例如：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1256">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="ba7fe-1257">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1257">Parameter transformer reference</span></span>

<span data-ttu-id="ba7fe-1258">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1258">Parameter transformers:</span></span>

* <span data-ttu-id="ba7fe-1259">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1259">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="ba7fe-1260">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1260">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="ba7fe-1261">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1261">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="ba7fe-1262">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1262">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="ba7fe-1263">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1263">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="ba7fe-1264">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1264">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="ba7fe-1265">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1265">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="ba7fe-1266">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1266">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="ba7fe-1267">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1267">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="ba7fe-1268">使用上述的路由，動作 `SubscriptionManagementController.GetAll` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1268">With the preceding route, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="ba7fe-1269">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1269">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="ba7fe-1270">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1270">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="ba7fe-1271">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1271">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="ba7fe-1272">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1272">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="ba7fe-1273">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1273">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="ba7fe-1274">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1274">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="ba7fe-1275">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1275">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* Razor<span data-ttu-id="ba7fe-1276">頁面具有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1276"> Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="ba7fe-1277">此慣例會將指定的參數轉換器套用至所有自動探索的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1277">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="ba7fe-1278">參數轉換器會轉換頁面路由的資料夾和檔案名區段 Razor 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1278">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="ba7fe-1279">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1279">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="ba7fe-1280">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1280">URL generation reference</span></span>

<span data-ttu-id="ba7fe-1281">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1281">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="ba7fe-1282">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1282">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="ba7fe-1283">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1283">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="ba7fe-1284">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1284">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="ba7fe-1285"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1285">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="ba7fe-1286">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1286">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="ba7fe-1287">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1287">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ba7fe-1288">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1288">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="ba7fe-1289">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1289">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="ba7fe-1290">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1290">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="ba7fe-1291">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1291">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="ba7fe-1292">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1292">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ba7fe-1293">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1293">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ba7fe-1294">環境值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1294">Ambient Values</span></span>                     | <span data-ttu-id="ba7fe-1295">明確值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1295">Explicit Values</span></span>                        | <span data-ttu-id="ba7fe-1296">結果</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1296">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ba7fe-1297">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1297">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1298">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1298">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-1299">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1299">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1300">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1300">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ba7fe-1301">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1301">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ba7fe-1302">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1302">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-1303">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1303">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1304">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1304">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="ba7fe-1305">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1305">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="ba7fe-1306">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1306">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="ba7fe-1307">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1307">Complex segments</span></span>

<span data-ttu-id="ba7fe-1308">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1308">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="ba7fe-1309">請參閱[此程式碼](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1309">See [this code](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="ba7fe-1310">[程式法範例](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1310">The [code sample](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ba7fe-1311">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1311">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="ba7fe-1312">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1312">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ba7fe-1313">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1313">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ba7fe-1314">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1314">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

<span data-ttu-id="ba7fe-1315">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1315">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> <span data-ttu-id="ba7fe-1316">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1316">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="ba7fe-1317">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1317">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="ba7fe-1318">如需頁面中路由慣例的詳細資訊 Razor ，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1318">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ba7fe-1319">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1319">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ba7fe-1320">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1320">Routing basics</span></span>

<span data-ttu-id="ba7fe-1321">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1321">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="ba7fe-1322">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1322">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="ba7fe-1323">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1323">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="ba7fe-1324">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1324">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="ba7fe-1325">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1325">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="ba7fe-1326">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1326">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="ba7fe-1327">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1327">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="ba7fe-1328">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1328">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

Razor<span data-ttu-id="ba7fe-1329">頁面應用程式會使用預設的傳統路由，在應用程式的*Pages*資料夾中提供已命名的資源。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1329"> Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="ba7fe-1330">還有其他慣例可讓您自訂 Razor 頁面路由行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1330">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="ba7fe-1331">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1331">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ba7fe-1332">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1332">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="ba7fe-1333">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1333">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="ba7fe-1334">路由會使用的路由 <xref:Microsoft.AspNetCore.Routing.IRouter> 執行來：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1334">Routing uses routes implementations of <xref:Microsoft.AspNetCore.Routing.IRouter> to:</span></span>

* <span data-ttu-id="ba7fe-1335">將傳入要求對應至「路由處理常式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1335">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="ba7fe-1336">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1336">Generate the URLs used in responses.</span></span>

<span data-ttu-id="ba7fe-1337">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1337">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="ba7fe-1338">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1338">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="ba7fe-1339">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1339">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="ba7fe-1340">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1340">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="ba7fe-1341">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1341">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ba7fe-1342">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1342">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ba7fe-1343">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1343">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ba7fe-1344"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1344"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ba7fe-1345">應用程式模型（例如 MVC/ Razor Pages）會註冊其所有的路由，其具有可預測的路由案例執行。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1345">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ba7fe-1346">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1346">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ba7fe-1347">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1347">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="ba7fe-1348"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1348"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
<span data-ttu-id="ba7fe-1349">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1349">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="ba7fe-1350">[ASP.NET CORE mvc](xref:mvc/overview)會將路由新增至中介軟體管線，作為其設定的一部分，並處理 MVC 和 Razor 頁面應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1350">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="ba7fe-1351">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1351">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="ba7fe-1352">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1352">URL matching</span></span>

<span data-ttu-id="ba7fe-1353">URL 比對是路由用來將傳入要求分派給「處理常式」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1353">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="ba7fe-1354">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1354">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ba7fe-1355">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1355">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ba7fe-1356">傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1356">Incoming requests enter the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="ba7fe-1357"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」\*\* 要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1357">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="ba7fe-1358">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1358">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="ba7fe-1359">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1359">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="ba7fe-1360"><xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1360">The primary input to <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="ba7fe-1361">[RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1361">The [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="ba7fe-1362">呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1362">A match that calls <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> also sets the properties of the [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="ba7fe-1363">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1363">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="ba7fe-1364">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1364">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="ba7fe-1365">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1365">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="ba7fe-1366">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1366"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="ba7fe-1367">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1367">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="ba7fe-1368">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1368">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="ba7fe-1369">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1369">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="ba7fe-1370">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1370">Routes can be nested inside of one another.</span></span> <span data-ttu-id="ba7fe-1371"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1371">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="ba7fe-1372">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1372">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="ba7fe-1373"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1373">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation"></a><span data-ttu-id="ba7fe-1374">URL 產生</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1374">URL generation</span></span>

<span data-ttu-id="ba7fe-1375">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1375">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ba7fe-1376">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1376">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="ba7fe-1377">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1377">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="ba7fe-1378">每個「路由」\*\* 會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1378">Each *route* has its <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="ba7fe-1379"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1379">The primary inputs to <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> are:</span></span>

* [<span data-ttu-id="ba7fe-1380">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1380">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [<span data-ttu-id="ba7fe-1381">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1381">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [<span data-ttu-id="ba7fe-1382">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1382">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

<span data-ttu-id="ba7fe-1383">路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1383">Routes primarily use the route values provided by <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> and <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="ba7fe-1384"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1384">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="ba7fe-1385">相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1385">In contrast, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="ba7fe-1386">提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1386">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="ba7fe-1387">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1387">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="ba7fe-1388">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1388">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="ba7fe-1389"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1389">The output of <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is a <xref:Microsoft.AspNetCore.Routing.VirtualPathData>.</span></span> <span data-ttu-id="ba7fe-1390"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1390"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> is a parallel of <xref:Microsoft.AspNetCore.Routing.RouteData>.</span></span> <span data-ttu-id="ba7fe-1391"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1391"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> contains the <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="ba7fe-1392">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1392">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="ba7fe-1393">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1393">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="ba7fe-1394">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1394">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="ba7fe-1395">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1395">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="ba7fe-1396">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1396">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="ba7fe-1397">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1397">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

### <a name="create-routes"></a><span data-ttu-id="ba7fe-1398">建立路由</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1398">Create routes</span></span>

<span data-ttu-id="ba7fe-1399">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1399">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="ba7fe-1400">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」\*\* 語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1400"><xref:Microsoft.AspNetCore.Routing.Route> uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="ba7fe-1401">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1401"><xref:Microsoft.AspNetCore.Routing.Route> uses the same route template to generate a URL when <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is called.</span></span>

<span data-ttu-id="ba7fe-1402">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1402">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="ba7fe-1403">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1403">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="ba7fe-1404"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1404"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ba7fe-1405"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1405"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ba7fe-1406">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1406">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="ba7fe-1407">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1407">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="ba7fe-1408">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1408">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="ba7fe-1409">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1409">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ba7fe-1410">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1410">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="ba7fe-1411">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1411">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="ba7fe-1412">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1412">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="ba7fe-1413">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1413">Route parameters are named.</span></span> <span data-ttu-id="ba7fe-1414">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1414">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="ba7fe-1415">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1415">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="ba7fe-1416">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1416">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="ba7fe-1417">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1417">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="ba7fe-1418">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1418">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="ba7fe-1419">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1419">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="ba7fe-1420">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1420">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="ba7fe-1421">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1421">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="ba7fe-1422">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1422">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="ba7fe-1423">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1423">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="ba7fe-1424">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1424">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="ba7fe-1425">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1425">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="ba7fe-1426">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1426">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="ba7fe-1427"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1427">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="ba7fe-1428">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1428">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="ba7fe-1429">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1429">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="ba7fe-1430">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1430">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="ba7fe-1431">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1431">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="ba7fe-1432">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1432">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ba7fe-1433">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1433">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ba7fe-1434">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1434">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ba7fe-1435">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1435">Default values can be specified in the route template.</span></span> <span data-ttu-id="ba7fe-1436">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1436">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="ba7fe-1437">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1437">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="ba7fe-1438">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1438">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="ba7fe-1439">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1439">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="ba7fe-1441">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1441">Route class URL generation</span></span>

<span data-ttu-id="ba7fe-1442"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1442">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="ba7fe-1443">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1443">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="ba7fe-1444">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1444">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="ba7fe-1445">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1445">What values would be produced?</span></span> <span data-ttu-id="ba7fe-1446">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1446">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="ba7fe-1447">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1447">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ba7fe-1448">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1448">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="ba7fe-1449">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1449">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="ba7fe-1450">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1450">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="ba7fe-1451">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1451">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="ba7fe-1452">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1452">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="ba7fe-1453">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1453">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ba7fe-1454">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1454">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="ba7fe-1455">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1455">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="ba7fe-1456">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1456">Use routing middleware</span></span>

<span data-ttu-id="ba7fe-1457">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1457">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="ba7fe-1458">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1458">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="ba7fe-1459">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1459">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="ba7fe-1460">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1460">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="ba7fe-1461"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>：僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1461"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>: Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="ba7fe-1462">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1462">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="ba7fe-1463">URI</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1463">URI</span></span>                    | <span data-ttu-id="ba7fe-1464">回應</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1464">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="ba7fe-1465">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1465">Hello!</span></span> <span data-ttu-id="ba7fe-1466">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1466">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="ba7fe-1467">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1467">Hello!</span></span> <span data-ttu-id="ba7fe-1468">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1468">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="ba7fe-1469">Hello!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1469">Hello!</span></span> <span data-ttu-id="ba7fe-1470">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1470">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="ba7fe-1471">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1471">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="ba7fe-1472">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1472">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="ba7fe-1473">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1473">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="ba7fe-1474">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1474">The request falls through, no match.</span></span>              |

<span data-ttu-id="ba7fe-1475">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1475">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="ba7fe-1476">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1476">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

<span data-ttu-id="ba7fe-1477">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1477">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="ba7fe-1478">其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1478">Some of listed methods, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, require a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="ba7fe-1479">路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」\*\* 使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1479">The <xref:Microsoft.AspNetCore.Http.RequestDelegate> is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="ba7fe-1480">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1480">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="ba7fe-1481">如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1481">If the `Map*` method doesn't accept a handler, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

<span data-ttu-id="ba7fe-1482">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1482">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="ba7fe-1483">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1483">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="ba7fe-1484">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1484">Route template reference</span></span>

<span data-ttu-id="ba7fe-1485">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1485">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="ba7fe-1486">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1486">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="ba7fe-1487">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1487">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="ba7fe-1488">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1488">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ba7fe-1489">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1489">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ba7fe-1490">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1490">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="ba7fe-1491">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1491">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="ba7fe-1492">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1492">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ba7fe-1493">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1493">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ba7fe-1494">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1494">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ba7fe-1495">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1495">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="ba7fe-1496">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1496">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="ba7fe-1497">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1497">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ba7fe-1498">這稱為「全部擷取」\*\* 參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1498">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="ba7fe-1499">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1499">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ba7fe-1500">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1500">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ba7fe-1501">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1501">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ba7fe-1502">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1502">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ba7fe-1503">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1503">Note the escaped forward slash.</span></span>

<span data-ttu-id="ba7fe-1504">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1504">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ba7fe-1505">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1505">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ba7fe-1506">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1506">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ba7fe-1507">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1507">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="ba7fe-1508">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1508">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ba7fe-1509">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1509">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ba7fe-1510">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1510">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="ba7fe-1511">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1511">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="ba7fe-1512">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1512">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="ba7fe-1513">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1513">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ba7fe-1514">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1514">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ba7fe-1515">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1515">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="ba7fe-1516">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1516">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="ba7fe-1517">路由範本</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1517">Route Template</span></span>                           | <span data-ttu-id="ba7fe-1518">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1518">Example Matching URI</span></span>    | <span data-ttu-id="ba7fe-1519">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1519">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ba7fe-1520">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1520">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ba7fe-1521">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1521">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ba7fe-1522">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1522">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ba7fe-1523">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1523">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ba7fe-1524">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1524">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="ba7fe-1525">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1525">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="ba7fe-1526">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1526">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ba7fe-1527">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1527">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="ba7fe-1528">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1528">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="ba7fe-1529">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1529">Route constraint reference</span></span>

<span data-ttu-id="ba7fe-1530">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1530">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ba7fe-1531">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1531">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="ba7fe-1532">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1532">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ba7fe-1533">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1533">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ba7fe-1534">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1534">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ba7fe-1535">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1535">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="ba7fe-1536">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1536">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="ba7fe-1537">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1537">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ba7fe-1538">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1538">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="ba7fe-1539">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1539">constraint</span></span> | <span data-ttu-id="ba7fe-1540">範例</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1540">Example</span></span> | <span data-ttu-id="ba7fe-1541">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1541">Example Matches</span></span> | <span data-ttu-id="ba7fe-1542">備註</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1542">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ba7fe-1543">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1543">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-1544">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1544">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ba7fe-1545">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1545">`true`, `FALSE`</span></span> | <span data-ttu-id="ba7fe-1546">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1546">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ba7fe-1547">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1547">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ba7fe-1548">符合不因文化特性而異的有效 `DateTime` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1548">Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1549">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1549">See  preceding warning.</span></span>|
| `decimal` | `{price:decimal}` | <span data-ttu-id="ba7fe-1550">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1550">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ba7fe-1551">符合不因文化特性而異的有效 `decimal` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1551">Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1552">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1552">See  preceding warning.</span></span>|
| `double` | `{weight:double}` | <span data-ttu-id="ba7fe-1553">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1553">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-1554">符合不因文化特性而異的有效 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1554">Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1555">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1555">See  preceding warning.</span></span>|
| `float` | `{weight:float}` | <span data-ttu-id="ba7fe-1556">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1556">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ba7fe-1557">符合不因文化特性而異的有效 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1557">Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="ba7fe-1558">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1558">See  preceding warning.</span></span>|
| `guid` | `{id:guid}` | <span data-ttu-id="ba7fe-1559">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1559">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ba7fe-1560">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1560">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ba7fe-1561">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1561">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ba7fe-1562">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1562">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ba7fe-1563">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1563">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="ba7fe-1564">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1564">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ba7fe-1565">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1565">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ba7fe-1566">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1566">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ba7fe-1567">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1567">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ba7fe-1568">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1568">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ba7fe-1569">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1569">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ba7fe-1570">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1570">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ba7fe-1571">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1571">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ba7fe-1572">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1572">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="ba7fe-1573">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1573">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ba7fe-1574">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1574">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ba7fe-1575">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1575">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ba7fe-1576">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1576">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="ba7fe-1577">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1577">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ba7fe-1578">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1578">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ba7fe-1579">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1579">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="ba7fe-1580">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1580">Regular expressions</span></span>

<span data-ttu-id="ba7fe-1581">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1581">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ba7fe-1582">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1582">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ba7fe-1583">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1583">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="ba7fe-1584">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1584">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ba7fe-1585">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1585">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="ba7fe-1586">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1586">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="ba7fe-1587">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1587">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="ba7fe-1588">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1588">Regular Expression</span></span>    | <span data-ttu-id="ba7fe-1589">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1589">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ba7fe-1590">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1590">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="ba7fe-1591">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1591">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="ba7fe-1592">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1592">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="ba7fe-1593">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1593">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ba7fe-1594">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1594">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="ba7fe-1595">運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1595">Expression</span></span>   | <span data-ttu-id="ba7fe-1596">String</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1596">String</span></span>    | <span data-ttu-id="ba7fe-1597">比對</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1597">Match</span></span> | <span data-ttu-id="ba7fe-1598">註解</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1598">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1599">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1599">hello</span></span>     | <span data-ttu-id="ba7fe-1600">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1600">Yes</span></span>   | <span data-ttu-id="ba7fe-1601">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1601">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1602">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1602">123abc456</span></span> | <span data-ttu-id="ba7fe-1603">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1603">Yes</span></span>   | <span data-ttu-id="ba7fe-1604">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1604">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1605">mz</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1605">mz</span></span>        | <span data-ttu-id="ba7fe-1606">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1606">Yes</span></span>   | <span data-ttu-id="ba7fe-1607">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1607">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ba7fe-1608">MZ</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1608">MZ</span></span>        | <span data-ttu-id="ba7fe-1609">是</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1609">Yes</span></span>   | <span data-ttu-id="ba7fe-1610">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1610">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-1611">hello</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1611">hello</span></span>     | <span data-ttu-id="ba7fe-1612">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1612">No</span></span>    | <span data-ttu-id="ba7fe-1613">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1613">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ba7fe-1614">123abc456</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1614">123abc456</span></span> | <span data-ttu-id="ba7fe-1615">No</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1615">No</span></span>    | <span data-ttu-id="ba7fe-1616">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1616">See `^` and `$` above</span></span> |

<span data-ttu-id="ba7fe-1617">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1617">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ba7fe-1618">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1618">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ba7fe-1619">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1619">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ba7fe-1620">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1620">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ba7fe-1621">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1621">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="ba7fe-1622">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1622">Custom Route Constraints</span></span>

<span data-ttu-id="ba7fe-1623">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1623">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="ba7fe-1624"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1624">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="ba7fe-1625">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1625">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="ba7fe-1626"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1626">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="ba7fe-1627">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1627">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="ba7fe-1628">例如：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1628">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="ba7fe-1629">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1629">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="ba7fe-1630">例如：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1630">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a><span data-ttu-id="ba7fe-1631">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1631">URL generation reference</span></span>

<span data-ttu-id="ba7fe-1632">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1632">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="ba7fe-1633">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1633">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="ba7fe-1634">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1634">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="ba7fe-1635">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1635">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="ba7fe-1636"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1636">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="ba7fe-1637">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1637">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="ba7fe-1638">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1638">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ba7fe-1639">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1639">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="ba7fe-1640">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1640">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="ba7fe-1641">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1641">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="ba7fe-1642">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1642">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="ba7fe-1643">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1643">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ba7fe-1644">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1644">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ba7fe-1645">環境值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1645">Ambient Values</span></span>                     | <span data-ttu-id="ba7fe-1646">明確值</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1646">Explicit Values</span></span>                        | <span data-ttu-id="ba7fe-1647">結果</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1647">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ba7fe-1648">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1648">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1649">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1649">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-1650">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1650">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1651">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1651">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ba7fe-1652">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1652">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ba7fe-1653">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1653">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ba7fe-1654">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1654">controller = "Home"</span></span>                | <span data-ttu-id="ba7fe-1655">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1655">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="ba7fe-1656">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1656">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="ba7fe-1657">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1657">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="ba7fe-1658">複雜區段</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1658">Complex segments</span></span>

<span data-ttu-id="ba7fe-1659">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1659">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="ba7fe-1660">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1660">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="ba7fe-1661">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="ba7fe-1661">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>

::: moniker-end
