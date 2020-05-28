---
<span data-ttu-id="6dd91-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-102">'Blazor'</span></span>
- <span data-ttu-id="6dd91-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-103">'Identity'</span></span>
- <span data-ttu-id="6dd91-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-105">'Razor'</span></span>
- <span data-ttu-id="6dd91-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-106">'SignalR' uid:</span></span> 

---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="6dd91-107">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="6dd91-107">Routing in ASP.NET Core</span></span>

<span data-ttu-id="6dd91-108">By [Ryan Nowak](https://github.com/rynowak)、 [Kirk Larkin](https://twitter.com/serpent5)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6dd91-108">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6dd91-109">路由會負責比對傳入 HTTP 要求，並將這些要求分派至應用程式的可執行端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-109">Routing is responsible for matching incoming HTTP requests and dispatching those requests to the app's executable endpoints.</span></span> <span data-ttu-id="6dd91-110">[端點](#endpoint)是應用程式的可執行要求處理常式代碼單位。</span><span class="sxs-lookup"><span data-stu-id="6dd91-110">[Endpoints](#endpoint) are the app's units of executable request-handling code.</span></span> <span data-ttu-id="6dd91-111">端點會在應用程式中定義，並在應用程式啟動時設定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-111">Endpoints are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="6dd91-112">端點比對程式可以從要求的 URL 中解壓縮值，並提供這些值來進行要求處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-112">The endpoint matching process can extract values from the request's URL and provide those values for request processing.</span></span> <span data-ttu-id="6dd91-113">使用來自應用程式的端點資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="6dd91-113">Using endpoint information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="6dd91-114">應用程式可以使用下列內容來設定路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-114">Apps can configure routing using:</span></span>

- <span data-ttu-id="6dd91-115">控制器</span><span class="sxs-lookup"><span data-stu-id="6dd91-115">Controllers</span></span>
- Razor<span data-ttu-id="6dd91-116">頁面</span><span class="sxs-lookup"><span data-stu-id="6dd91-116"> Pages</span></span>
- SignalR
- <span data-ttu-id="6dd91-117">gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="6dd91-117">gRPC Services</span></span>
- <span data-ttu-id="6dd91-118">端點啟用的[中介軟體](xref:fundamentals/middleware/index)，例如[健康情況檢查](xref:host-and-deploy/health-checks)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-118">Endpoint-enabled [middleware](xref:fundamentals/middleware/index) such as [Health Checks](xref:host-and-deploy/health-checks).</span></span>
- <span data-ttu-id="6dd91-119">已向路由註冊的委派和 lambda。</span><span class="sxs-lookup"><span data-stu-id="6dd91-119">Delegates and lambdas registered with routing.</span></span>

<span data-ttu-id="6dd91-120">本檔涵蓋 ASP.NET Core 路由的低層級詳細資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-120">This document covers low-level details of ASP.NET Core routing.</span></span> <span data-ttu-id="6dd91-121">如需設定路由的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="6dd91-121">For information on configuring routing:</span></span>

* <span data-ttu-id="6dd91-122">針對控制器，請參閱 <xref:mvc/controllers/routing> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-122">For controllers, see <xref:mvc/controllers/routing>.</span></span>
* <span data-ttu-id="6dd91-123">如需 Razor 頁面慣例，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-123">For Razor Pages conventions, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="6dd91-124">本檔中所述的端點路由系統適用于 ASP.NET Core 3.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="6dd91-124">The endpoint routing system described in this document applies to ASP.NET Core 3.0 and later.</span></span> <span data-ttu-id="6dd91-125">如需以為基礎的先前路由系統的詳細資訊 <xref:Microsoft.AspNetCore.Routing.IRouter> ，請使用下列其中一種方法來選取 ASP.NET Core 2.1 版本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-125">For information on the previous routing system based on <xref:Microsoft.AspNetCore.Routing.IRouter>, select the ASP.NET Core 2.1 version using one of the following approaches:</span></span>

* <span data-ttu-id="6dd91-126">先前版本的版本選取器。</span><span class="sxs-lookup"><span data-stu-id="6dd91-126">The version selector for a previous version.</span></span>
* <span data-ttu-id="6dd91-127">選取 [ [ASP.NET Core 2.1 路由](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)]。</span><span class="sxs-lookup"><span data-stu-id="6dd91-127">Select [ASP.NET Core 2.1 routing](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="6dd91-128">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="6dd91-128">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6dd91-129">此檔的下載範例會由特定 `Startup` 類別啟用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-129">The download samples for this document are enabled by a specific `Startup` class.</span></span> <span data-ttu-id="6dd91-130">若要執行特定的範例，請修改*Program.cs*以呼叫所需的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="6dd91-130">To run a specific sample, modify *Program.cs* to call the desired `Startup` class.</span></span>

## <a name="routing-basics"></a><span data-ttu-id="6dd91-131">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="6dd91-131">Routing basics</span></span>

<span data-ttu-id="6dd91-132">所有 ASP.NET Core 範本都會在產生的程式碼中包含路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-132">All ASP.NET Core templates include routing in the generated code.</span></span> <span data-ttu-id="6dd91-133">路由是在的[中介軟體](xref:fundamentals/middleware/index)管線中註冊 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-133">Routing is registered in the [middleware](xref:fundamentals/middleware/index) pipeline in `Startup.Configure`.</span></span>

<span data-ttu-id="6dd91-134">下列程式碼顯示路由的基本範例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-134">The following code shows a basic example of routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet&highlight=8,10)]

<span data-ttu-id="6dd91-135">路由會使用一對由和註冊的中介軟體 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-135">Routing uses a pair of middleware, registered by <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>:</span></span>

* <span data-ttu-id="6dd91-136">`UseRouting`將路由對應新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-136">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="6dd91-137">此中介軟體會查看應用程式中定義的端點集合，並根據要求選取[最符合的項](#urlm)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-137">This middleware looks at the set of endpoints defined in the app, and selects the [best match](#urlm) based on the request.</span></span>
* <span data-ttu-id="6dd91-138">`UseEndpoints`將端點執行新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-138">`UseEndpoints` adds endpoint execution to the middleware pipeline.</span></span> <span data-ttu-id="6dd91-139">它會執行與所選端點相關聯的委派。</span><span class="sxs-lookup"><span data-stu-id="6dd91-139">It runs the delegate associated with the selected endpoint.</span></span>

<span data-ttu-id="6dd91-140">上述範例包含使用[MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*)方法*對程式碼*端點的單一路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-140">The preceding example includes a single *route to code* endpoint using the [MapGet](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*) method:</span></span>

* <span data-ttu-id="6dd91-141">當 HTTP `GET` 要求傳送至根 URL 時 `/` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-141">When an HTTP `GET` request is sent to the root URL `/`:</span></span>
  * <span data-ttu-id="6dd91-142">所顯示的要求委派會執行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-142">The request delegate shown executes.</span></span>
  * <span data-ttu-id="6dd91-143">`Hello World!`會寫入 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="6dd91-143">`Hello World!` is written to the HTTP response.</span></span> <span data-ttu-id="6dd91-144">根據預設，根 URL `/` 是 `https://localhost:5001/` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-144">By default, the root URL `/` is `https://localhost:5001/`.</span></span>
* <span data-ttu-id="6dd91-145">如果要求方法不是 `GET` ，或根 URL 不是 `/` ，則不會符合任何路由，且會傳回 HTTP 404。</span><span class="sxs-lookup"><span data-stu-id="6dd91-145">If the request method is not `GET` or the root URL is not `/`, no route matches and an HTTP 404 is returned.</span></span>

### <a name="endpoint"></a><span data-ttu-id="6dd91-146">端點</span><span class="sxs-lookup"><span data-stu-id="6dd91-146">Endpoint</span></span>

<a name="endpoint"></a>

<span data-ttu-id="6dd91-147">`MapGet`方法是用來定義**端點**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-147">The `MapGet` method is used to define an **endpoint**.</span></span> <span data-ttu-id="6dd91-148">端點可以是：</span><span class="sxs-lookup"><span data-stu-id="6dd91-148">An endpoint is something that can be:</span></span>

* <span data-ttu-id="6dd91-149">已選取，藉由比對 URL 和 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-149">Selected, by matching the URL and HTTP method.</span></span>
* <span data-ttu-id="6dd91-150">執行委派。</span><span class="sxs-lookup"><span data-stu-id="6dd91-150">Executed, by running the delegate.</span></span>

<span data-ttu-id="6dd91-151">應用程式可比對並執行的端點會在中設定 `UseEndpoints` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-151">Endpoints that can be matched and executed by the app are configured in `UseEndpoints`.</span></span> <span data-ttu-id="6dd91-152">例如，、 <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*> <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*> 和類似的[方法](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions)會將要求委派連接至路由系統。</span><span class="sxs-lookup"><span data-stu-id="6dd91-152">For example, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>, and [similar methods](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions) connect request delegates to the routing system.</span></span>
<span data-ttu-id="6dd91-153">您可以使用其他方法，將 ASP.NET Core framework 功能連接到路由系統：</span><span class="sxs-lookup"><span data-stu-id="6dd91-153">Additional methods can be used to connect ASP.NET Core framework features to the routing system:</span></span>
- <span data-ttu-id="6dd91-154">[頁面的 MapRazorPages Razor](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)</span><span class="sxs-lookup"><span data-stu-id="6dd91-154">[MapRazorPages for Razor Pages](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*)</span></span>
- [<span data-ttu-id="6dd91-155">適用于控制器的 MapControllers</span><span class="sxs-lookup"><span data-stu-id="6dd91-155">MapControllers for controllers</span></span>](xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*)
- <span data-ttu-id="6dd91-156">[\<THub>適用于的 MapHubSignalR](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*)</span><span class="sxs-lookup"><span data-stu-id="6dd91-156">[MapHub\<THub> for SignalR](xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub*)</span></span> 
- [<span data-ttu-id="6dd91-157">\<TService>適用于 gRPC 的 MapGrpcService</span><span class="sxs-lookup"><span data-stu-id="6dd91-157">MapGrpcService\<TService> for gRPC</span></span>](xref:grpc/aspnetcore)

<span data-ttu-id="6dd91-158">下列範例顯示具有更複雜路由範本的路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-158">The following example shows routing with a more sophisticated route template:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/RouteTemplateStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-159">此字串 `/hello/{name:alpha}` 為**路由範本**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-159">The string `/hello/{name:alpha}` is a **route template**.</span></span> <span data-ttu-id="6dd91-160">它是用來設定端點的相符方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-160">It is used to configure how the endpoint is matched.</span></span> <span data-ttu-id="6dd91-161">在此情況下，範本會符合：</span><span class="sxs-lookup"><span data-stu-id="6dd91-161">In this case, the template matches:</span></span>

* <span data-ttu-id="6dd91-162">URL，例如`/hello/Ryan`</span><span class="sxs-lookup"><span data-stu-id="6dd91-162">A URL like `/hello/Ryan`</span></span>
* <span data-ttu-id="6dd91-163">開頭為的任何 URL 路徑， `/hello/` 後面接著一連串的字母字元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-163">Any URL path that begins with `/hello/` followed by a sequence of alphabetic characters.</span></span>  <span data-ttu-id="6dd91-164">`:alpha`套用僅符合字母字元的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-164">`:alpha` applies a route constraint that matches only alphabetic characters.</span></span> <span data-ttu-id="6dd91-165">本檔稍後會說明[路由條件約束](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-165">[Route constraints](#route-constraint-reference) are explained later in this document.</span></span>

<span data-ttu-id="6dd91-166">URL 路徑的第二個區段 `{name:alpha}` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-166">The second segment of the URL path, `{name:alpha}`:</span></span>

* <span data-ttu-id="6dd91-167">已系結至 `name` 參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-167">Is bound to the `name` parameter.</span></span>
* <span data-ttu-id="6dd91-168">會被捕捉並儲存在[HttpRequest 中。 RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-168">Is captured and stored in [HttpRequest.RouteValues](xref:Microsoft.AspNetCore.Http.HttpRequest.RouteValues*).</span></span>

<span data-ttu-id="6dd91-169">本檔中所述的端點路由系統是 ASP.NET Core 3.0 的新功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-169">The endpoint routing system described in this document is new as of ASP.NET Core 3.0.</span></span> <span data-ttu-id="6dd91-170">不過，ASP.NET Core 的所有版本都支援一組相同的路由範本功能和路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-170">However, all versions of ASP.NET Core support the same set of route template features and route constraints.</span></span>

<span data-ttu-id="6dd91-171">下列範例顯示具有健康情況[檢查](xref:host-and-deploy/health-checks)和授權的路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-171">The following example shows routing with [health checks](xref:host-and-deploy/health-checks) and authorization:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="6dd91-172">上述範例示範如何：</span><span class="sxs-lookup"><span data-stu-id="6dd91-172">The preceding example demonstrates how:</span></span>

* <span data-ttu-id="6dd91-173">授權中介軟體可以與路由搭配使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-173">The authorization middleware can be used with routing.</span></span>
* <span data-ttu-id="6dd91-174">端點可以用來設定授權行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-174">Endpoints can be used to configure authorization behavior.</span></span>

<span data-ttu-id="6dd91-175"><xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*>呼叫會新增健康情況檢查端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-175">The <xref:Microsoft.AspNetCore.Builder.HealthCheckEndpointRouteBuilderExtensions.MapHealthChecks*> call adds a health check endpoint.</span></span> <span data-ttu-id="6dd91-176">此呼叫的連結會將 <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> 授權原則附加至端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-176">Chaining <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*> on to this call attaches an authorization policy to the endpoint.</span></span>

<span data-ttu-id="6dd91-177">呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> 新增驗證和授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-177">Calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*> adds the authentication and authorization middleware.</span></span> <span data-ttu-id="6dd91-178">這些中介軟體會放在 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> 和之間 `UseEndpoints` ，使其可以：</span><span class="sxs-lookup"><span data-stu-id="6dd91-178">These middleware are placed between <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and `UseEndpoints` so that they can:</span></span>

* <span data-ttu-id="6dd91-179">查看所選取的端點 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-179">See which endpoint was selected by `UseRouting`.</span></span>
* <span data-ttu-id="6dd91-180">將分派給端點之前，請先套用授權原則 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-180">Apply an authorization policy before <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> dispatches to the endpoint.</span></span>

<a name="metadata"></a>

### <a name="endpoint-metadata"></a><span data-ttu-id="6dd91-181">端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="6dd91-181">Endpoint metadata</span></span>

<span data-ttu-id="6dd91-182">在上述範例中，有兩個端點，但只有健全狀況檢查端點已附加授權原則。</span><span class="sxs-lookup"><span data-stu-id="6dd91-182">In the preceding example, there are two endpoints, but only the health check endpoint has an authorization policy attached.</span></span> <span data-ttu-id="6dd91-183">如果要求符合健康狀態檢查端點， `/healthz` 則會執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="6dd91-183">If the request matches the health check endpoint, `/healthz`, an authorization check is performed.</span></span> <span data-ttu-id="6dd91-184">這會示範端點可以附加額外的資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-184">This demonstrates that endpoints can have extra data attached to them.</span></span> <span data-ttu-id="6dd91-185">這種額外的資料稱為端點**中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="6dd91-185">This extra data is called endpoint **metadata**:</span></span>

* <span data-ttu-id="6dd91-186">可由路由感知中介軟體處理中繼資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-186">The metadata can be processed by routing-aware middleware.</span></span>
* <span data-ttu-id="6dd91-187">中繼資料可以是任何 .NET 型別。</span><span class="sxs-lookup"><span data-stu-id="6dd91-187">The metadata can be of any .NET type.</span></span>

## <a name="routing-concepts"></a><span data-ttu-id="6dd91-188">路由概念</span><span class="sxs-lookup"><span data-stu-id="6dd91-188">Routing concepts</span></span>

<span data-ttu-id="6dd91-189">路由系統會藉由新增強大的**端點**概念，在中介軟體管線之上建立。</span><span class="sxs-lookup"><span data-stu-id="6dd91-189">The routing system builds on top of the middleware pipeline by adding the powerful **endpoint** concept.</span></span> <span data-ttu-id="6dd91-190">端點代表應用程式功能的單位，在路由、授權及任何數目的 ASP.NET Core 系統之間彼此不同。</span><span class="sxs-lookup"><span data-stu-id="6dd91-190">Endpoints represent units of the app's functionality that are distinct from each other in terms of routing, authorization, and any number of ASP.NET Core's systems.</span></span>

<a name="endpoint"></a>

### <a name="aspnet-core-endpoint-definition"></a><span data-ttu-id="6dd91-191">ASP.NET Core 端點定義</span><span class="sxs-lookup"><span data-stu-id="6dd91-191">ASP.NET Core endpoint definition</span></span>

<span data-ttu-id="6dd91-192">ASP.NET Core 的端點為：</span><span class="sxs-lookup"><span data-stu-id="6dd91-192">An ASP.NET Core endpoint is:</span></span>

* <span data-ttu-id="6dd91-193">可執行檔：具有 <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-193">Executable: Has a <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate>.</span></span>
* <span data-ttu-id="6dd91-194">可擴充：具有[中繼資料](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*)集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-194">Extensible: Has a [Metadata](xref:Microsoft.AspNetCore.Http.Endpoint.Metadata*) collection.</span></span>
* <span data-ttu-id="6dd91-195">可選取：選擇性地擁有[路由資訊](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-195">Selectable: Optionally, has [routing information](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern*).</span></span>
* <span data-ttu-id="6dd91-196">可列舉：藉由從 DI 抓取來列出端點的 <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> 集合[DI](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-196">Enumerable: The collection of endpoints can be listed by retrieving the <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> from [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="6dd91-197">下列程式碼示範如何抓取和檢查符合目前要求的端點：</span><span class="sxs-lookup"><span data-stu-id="6dd91-197">The following code shows how to retrieve and inspect the endpoint matching the current request:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/EndpointInspectorStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-198">若已選取，則可以從抓取端點 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-198">The endpoint, if selected, can be retrieved from the `HttpContext`.</span></span> <span data-ttu-id="6dd91-199">可以檢查其屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-199">Its properties can be inspected.</span></span> <span data-ttu-id="6dd91-200">端點物件是不可變的，而且無法在建立之後修改。</span><span class="sxs-lookup"><span data-stu-id="6dd91-200">Endpoint objects are immutable and cannot be modified after creation.</span></span> <span data-ttu-id="6dd91-201">最常見的端點類型是 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-201">The most common type of endpoint is a <xref:Microsoft.AspNetCore.Routing.RouteEndpoint>.</span></span> <span data-ttu-id="6dd91-202">`RouteEndpoint`包含可讓路由系統選取的資訊。</span><span class="sxs-lookup"><span data-stu-id="6dd91-202">`RouteEndpoint` includes information that allows it to be to selected by the routing system.</span></span>

<span data-ttu-id="6dd91-203">在上述程式碼中， [app。使用](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*)會設定內嵌[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-203">In the preceding code, [app.Use](xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*) configures an in-line [middleware](xref:fundamentals/middleware/index).</span></span>

<a name="mt"></a>

<span data-ttu-id="6dd91-204">下列程式碼顯示，視管線中呼叫的位置而定， `app.Use` 可能不會有端點：</span><span class="sxs-lookup"><span data-stu-id="6dd91-204">The following code shows that, depending on where `app.Use` is called in the pipeline, there may not be an endpoint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/MiddlewareFlowStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-205">先前 `Console.WriteLine` 的範例會加入語句，以顯示是否已選取端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-205">This preceding sample adds `Console.WriteLine` statements that display whether or not an endpoint has been selected.</span></span> <span data-ttu-id="6dd91-206">為了清楚起見，此範例會將顯示名稱指派給提供的 `/` 端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-206">For clarity, the sample assigns a display name to the provided `/` endpoint.</span></span>

<span data-ttu-id="6dd91-207">以顯示的 URL 執行此程式碼 `/` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-207">Running this code with a URL of `/` displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: Hello
3. Endpoint: Hello
```

<span data-ttu-id="6dd91-208">執行此程式碼時，會顯示任何其他 URL：</span><span class="sxs-lookup"><span data-stu-id="6dd91-208">Running this code with any other URL displays:</span></span>

```txt
1. Endpoint: (null)
2. Endpoint: (null)
4. Endpoint: (null)
```

<span data-ttu-id="6dd91-209">此輸出示範：</span><span class="sxs-lookup"><span data-stu-id="6dd91-209">This output demonstrates that:</span></span>

* <span data-ttu-id="6dd91-210">在呼叫之前，端點一律為 null `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-210">The endpoint is always null before `UseRouting` is called.</span></span>
* <span data-ttu-id="6dd91-211">如果找到相符的，則與之間的端點為非 null `UseRouting` <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-211">If a match is found, the endpoint is non-null between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="6dd91-212">`UseEndpoints`當找到相符的時，中介軟體就是**終端**機。</span><span class="sxs-lookup"><span data-stu-id="6dd91-212">The `UseEndpoints` middleware is **terminal** when a match is found.</span></span> <span data-ttu-id="6dd91-213">[終端機中介軟體](#tm)會在本檔稍後定義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-213">[Terminal middleware](#tm) is defined later in this document.</span></span>
* <span data-ttu-id="6dd91-214">`UseEndpoints`只有當找不到相符的時，才會在執行後中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-214">The middleware after `UseEndpoints` execute only when no match is found.</span></span>

<span data-ttu-id="6dd91-215">`UseRouting`中介軟體會使用[task.setendpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*)方法，將端點附加至目前的內容。</span><span class="sxs-lookup"><span data-stu-id="6dd91-215">The `UseRouting` middleware uses the [SetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.SetEndpoint*) method to attach the endpoint to the current context.</span></span> <span data-ttu-id="6dd91-216">您可以 `UseRouting` 使用自訂邏輯來取代中介軟體，但仍可獲得使用端點的優點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-216">It's possible to replace the `UseRouting` middleware with custom logic and still get the benefits of using endpoints.</span></span> <span data-ttu-id="6dd91-217">端點是低層級的基本型別，例如中介軟體，而且不會與路由執行結合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-217">Endpoints are a low-level primitive like middleware, and aren't coupled to the routing implementation.</span></span> <span data-ttu-id="6dd91-218">大部分的應用程式不需要以 `UseRouting` 自訂邏輯取代。</span><span class="sxs-lookup"><span data-stu-id="6dd91-218">Most apps don't need to replace `UseRouting` with custom logic.</span></span>

<span data-ttu-id="6dd91-219">`UseEndpoints`中介軟體是設計用來與中介軟體一起使用 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-219">The `UseEndpoints` middleware is designed to be used in tandem with the `UseRouting` middleware.</span></span> <span data-ttu-id="6dd91-220">執行端點的核心邏輯並不複雜。</span><span class="sxs-lookup"><span data-stu-id="6dd91-220">The core logic to execute an endpoint isn't complicated.</span></span> <span data-ttu-id="6dd91-221">使用 <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> 來取出端點，然後叫用其 <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> 屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-221">Use <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*> to retrieve the endpoint, and then invoke its <xref:Microsoft.AspNetCore.Http.Endpoint.RequestDelegate> property.</span></span>

<span data-ttu-id="6dd91-222">下列程式碼示範中介軟體會如何影響或回應路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-222">The following code demonstrates how middleware can influence or react to routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/IntegratedMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-223">上述範例示範兩個重要概念：</span><span class="sxs-lookup"><span data-stu-id="6dd91-223">The preceding example demonstrates two important concepts:</span></span>

* <span data-ttu-id="6dd91-224">中介軟體可以在前執行， `UseRouting` 以修改路由運作的資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-224">Middleware can run before `UseRouting` to modify the data that routing operates upon.</span></span>
    * <span data-ttu-id="6dd91-225">通常在路由之前出現的中介軟體會修改要求的某些屬性，例如 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 、 <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*> 或 <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-225">Usually middleware that appears before routing modifies some property of the request, such as <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>, <xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions.UseHttpMethodOverride*>, or <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>.</span></span>
* <span data-ttu-id="6dd91-226">中介軟體可在 `UseRouting` 和之間執行， <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 以便在執行端點之前處理路由的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-226">Middleware can run between `UseRouting` and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> to process the results of routing before the endpoint is executed.</span></span>
    * <span data-ttu-id="6dd91-227">在和之間執行的中介軟體 `UseRouting` `UseEndpoints` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-227">Middleware that runs between `UseRouting` and `UseEndpoints`:</span></span>
      * <span data-ttu-id="6dd91-228">通常會檢查中繼資料以瞭解端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-228">Usually inspects metadata to understand the endpoints.</span></span>
      * <span data-ttu-id="6dd91-229">通常會依照和完成安全性決策 `UseAuthorization` `UseCors` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-229">Often makes security decisions, as done by `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="6dd91-230">中介軟體和中繼資料的組合，可讓您設定每個端點的原則。</span><span class="sxs-lookup"><span data-stu-id="6dd91-230">The combination of middleware and metadata allows configuring policies per-endpoint.</span></span>

<span data-ttu-id="6dd91-231">上述程式碼顯示支援每個端點原則的自訂中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-231">The preceding code shows an example of a custom middleware that supports per-endpoint policies.</span></span> <span data-ttu-id="6dd91-232">中介軟體會將敏感性資料之存取權的*審核記錄*寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="6dd91-232">The middleware writes an *audit log* of access to sensitive data to the console.</span></span> <span data-ttu-id="6dd91-233">中介軟體可以設定為使用中繼資料來*審核*端點 `AuditPolicyAttribute` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-233">The middleware can be configured to *audit* an endpoint with the `AuditPolicyAttribute` metadata.</span></span> <span data-ttu-id="6dd91-234">這個範例會示範*加入宣告*模式，其中只會審核標示為機密的端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-234">This sample demonstrates an *opt-in* pattern where only endpoints that are marked as sensitive are audited.</span></span> <span data-ttu-id="6dd91-235">例如，您可以反向定義此邏輯，以審核未標示為安全的所有專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-235">It's possible to define this logic in reverse, auditing everything that isn't marked as safe, for example.</span></span> <span data-ttu-id="6dd91-236">端點中繼資料系統很有彈性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-236">The endpoint metadata system is flexible.</span></span> <span data-ttu-id="6dd91-237">這個邏輯可以設計成符合使用案例的任何方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-237">This logic could be designed in whatever way suits the use case.</span></span>

<span data-ttu-id="6dd91-238">先前的範例程式碼主要是用來示範端點的基本概念。</span><span class="sxs-lookup"><span data-stu-id="6dd91-238">The preceding sample code is intended to demonstrate the basic concepts of endpoints.</span></span> <span data-ttu-id="6dd91-239">**此範例並非供生產環境使用**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-239">**The sample is not intended for production use**.</span></span> <span data-ttu-id="6dd91-240">更完整的*audit 記錄*檔中介軟體版本如下：</span><span class="sxs-lookup"><span data-stu-id="6dd91-240">A more complete version of an *audit log* middleware would:</span></span>

* <span data-ttu-id="6dd91-241">記錄到檔案或資料庫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-241">Log to a file or database.</span></span>
* <span data-ttu-id="6dd91-242">包含詳細資料，例如使用者、IP 位址、機密端點的名稱等等。</span><span class="sxs-lookup"><span data-stu-id="6dd91-242">Include details such as the user, IP address, name of the sensitive endpoint, and more.</span></span>

<span data-ttu-id="6dd91-243">稽核原則中繼資料 `AuditPolicyAttribute` 會定義為 `Attribute` ，以便更容易與以類別為基礎的架構（例如控制器和）搭配使用 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-243">The audit policy metadata `AuditPolicyAttribute` is defined as an `Attribute` for easier use with class-based frameworks such as controllers and SignalR.</span></span> <span data-ttu-id="6dd91-244">使用*路由至程式碼*時：</span><span class="sxs-lookup"><span data-stu-id="6dd91-244">When using *route to code*:</span></span>

* <span data-ttu-id="6dd91-245">中繼資料是使用產生器 API 所附加。</span><span class="sxs-lookup"><span data-stu-id="6dd91-245">Metadata is attached with a builder API.</span></span>
* <span data-ttu-id="6dd91-246">以類別為基礎的架構在建立端點時，會在對應的方法和類別上包含所有屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-246">Class-based frameworks include all attributes on the corresponding method and class when creating endpoints.</span></span>

<span data-ttu-id="6dd91-247">元資料類型的最佳作法是將它們定義為介面或屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-247">The best practices for metadata types are to define them either as interfaces or attributes.</span></span> <span data-ttu-id="6dd91-248">介面和屬性允許程式碼重複使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-248">Interfaces and attributes allow code reuse.</span></span> <span data-ttu-id="6dd91-249">中繼資料系統具有彈性，而且不會強加任何限制。</span><span class="sxs-lookup"><span data-stu-id="6dd91-249">The metadata system is flexible and doesn't impose any limitations.</span></span>

<a name="tm"></a>

### <a name="comparing-a-terminal-middleware-and-routing"></a><span data-ttu-id="6dd91-250">比較終端中介軟體和路由</span><span class="sxs-lookup"><span data-stu-id="6dd91-250">Comparing a terminal middleware and routing</span></span>

<span data-ttu-id="6dd91-251">下列程式碼範例會將中介軟體與使用路由進行對比：</span><span class="sxs-lookup"><span data-stu-id="6dd91-251">The following code sample contrasts using middleware with using routing:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/TerminalMiddlewareStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-252">以顯示的中介軟體樣式 `Approach 1:` 是**終端中介軟體**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-252">The style of middleware shown with `Approach 1:` is **terminal middleware**.</span></span> <span data-ttu-id="6dd91-253">這稱為「終端中介軟體」，因為它會執行比對作業：</span><span class="sxs-lookup"><span data-stu-id="6dd91-253">It's called terminal middleware because it does a matching operation:</span></span>

* <span data-ttu-id="6dd91-254">前述範例中的比對作業 `Path == "/"` 適用于中介軟體和 `Path == "/Movie"` 路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-254">The matching operation in the preceding sample is `Path == "/"` for the middleware and `Path == "/Movie"` for routing.</span></span>
* <span data-ttu-id="6dd91-255">當相符項成功時，它會執行一些功能並傳回，而不是叫用 `next` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-255">When a match is successful, it executes some functionality and returns, rather than invoking the `next` middleware.</span></span>

<span data-ttu-id="6dd91-256">這稱為「終端中介軟體」，因為它會終止搜尋、執行一些功能，然後傳回。</span><span class="sxs-lookup"><span data-stu-id="6dd91-256">It's called terminal middleware because it terminates the search, executes some functionality, and then returns.</span></span>

<span data-ttu-id="6dd91-257">比較終端機中介軟體和路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-257">Comparing a terminal middleware and routing:</span></span>
* <span data-ttu-id="6dd91-258">這兩種方法都允許終止處理管線：</span><span class="sxs-lookup"><span data-stu-id="6dd91-258">Both approaches allow terminating the processing pipeline:</span></span>
    * <span data-ttu-id="6dd91-259">中介軟體會藉由傳回而不是叫用來終止管線 `next` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-259">Middleware terminates the pipeline by returning rather than invoking `next`.</span></span>
    * <span data-ttu-id="6dd91-260">端點一律是 [終端機]。</span><span class="sxs-lookup"><span data-stu-id="6dd91-260">Endpoints are always terminal.</span></span>
* <span data-ttu-id="6dd91-261">終端中介軟體允許將中介軟體放在管線中的任意位置：</span><span class="sxs-lookup"><span data-stu-id="6dd91-261">Terminal middleware allows positioning the middleware at an arbitrary place in the pipeline:</span></span>
    * <span data-ttu-id="6dd91-262">端點會在的位置執行 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-262">Endpoints execute at the position of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>
* <span data-ttu-id="6dd91-263">終端中介軟體可讓任意程式碼判斷中介軟體何時符合：</span><span class="sxs-lookup"><span data-stu-id="6dd91-263">Terminal middleware allows arbitrary code to determine when the middleware matches:</span></span>
    * <span data-ttu-id="6dd91-264">自訂路由比對程式碼可能很詳細，而且很容易正確寫入。</span><span class="sxs-lookup"><span data-stu-id="6dd91-264">Custom route matching code can be verbose and difficult to write correctly.</span></span>
    * <span data-ttu-id="6dd91-265">路由為一般應用程式提供直接的解決方案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-265">Routing provides straightforward solutions for typical apps.</span></span> <span data-ttu-id="6dd91-266">大部分的應用程式都不需要自訂路由比對程式碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-266">Most apps don't require custom route matching code.</span></span>
* <span data-ttu-id="6dd91-267">具有中介軟體的端點介面，例如 `UseAuthorization` 和 `UseCors` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-267">Endpoints interface with middleware such as `UseAuthorization` and `UseCors`.</span></span>
    * <span data-ttu-id="6dd91-268">搭配或使用終端中介軟體時， `UseAuthorization` `UseCors` 必須手動與授權系統互動。</span><span class="sxs-lookup"><span data-stu-id="6dd91-268">Using a terminal middleware with `UseAuthorization` or `UseCors` requires manual interfacing with the authorization system.</span></span>

<span data-ttu-id="6dd91-269">[端點](#endpoint)會定義兩者：</span><span class="sxs-lookup"><span data-stu-id="6dd91-269">An [endpoint](#endpoint) defines both:</span></span>

* <span data-ttu-id="6dd91-270">用來處理要求的委派。</span><span class="sxs-lookup"><span data-stu-id="6dd91-270">A delegate to process requests.</span></span>
* <span data-ttu-id="6dd91-271">任意中繼資料的集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-271">A collection of arbitrary metadata.</span></span> <span data-ttu-id="6dd91-272">中繼資料是用來根據附加至每個端點的原則和設定來執行跨領域考慮。</span><span class="sxs-lookup"><span data-stu-id="6dd91-272">The metadata is used to implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="6dd91-273">終端中介軟體可以是有效的工具，但可能需要：</span><span class="sxs-lookup"><span data-stu-id="6dd91-273">Terminal middleware can be an effective tool, but can require:</span></span>

* <span data-ttu-id="6dd91-274">大量的編碼和測試。</span><span class="sxs-lookup"><span data-stu-id="6dd91-274">A significant amount of coding and testing.</span></span>
* <span data-ttu-id="6dd91-275">與其他系統進行手動整合，以達到所需的彈性層級。</span><span class="sxs-lookup"><span data-stu-id="6dd91-275">Manual integration with other systems to achieve the desired level of flexibility.</span></span>

<span data-ttu-id="6dd91-276">撰寫終端中介軟體之前，請考慮與路由整合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-276">Consider integrating with routing before writing a terminal middleware.</span></span>

<span data-ttu-id="6dd91-277">與[Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)或整合的現有終端機中介軟體， <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 通常可以轉換成路由感知端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-277">Existing terminal middleware that integrates with [Map](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) or <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> can usually be turned into a routing aware endpoint.</span></span> <span data-ttu-id="6dd91-278">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16)示範路由器的模式：</span><span class="sxs-lookup"><span data-stu-id="6dd91-278">[MapHealthChecks](https://github.com/aspnet/AspNetCore/blob/master/src/Middleware/HealthChecks/src/Builder/HealthCheckEndpointRouteBuilderExtensions.cs#L16) demonstrates the pattern for router-ware:</span></span>
* <span data-ttu-id="6dd91-279">在上撰寫擴充方法 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-279">Write an extension method on <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span>
* <span data-ttu-id="6dd91-280">使用建立嵌套中介軟體管線 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-280">Create a nested middleware pipeline using <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder.CreateApplicationBuilder*>.</span></span>
* <span data-ttu-id="6dd91-281">將中介軟體附加至新管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-281">Attach the middleware to the new pipeline.</span></span> <span data-ttu-id="6dd91-282">在此案例中為 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-282">In this case, <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>.</span></span>
* <span data-ttu-id="6dd91-283"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*>中介軟體管線至 <xref:Microsoft.AspNetCore.Http.RequestDelegate> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-283"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.Build*> the middleware pipeline into a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="6dd91-284">呼叫 `Map` 並提供新的中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-284">Call `Map` and provide the new middleware pipeline.</span></span>
* <span data-ttu-id="6dd91-285">從擴充方法傳回提供的產生器物件 `Map` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-285">Return the builder object provided by `Map` from the extension method.</span></span>

<span data-ttu-id="6dd91-286">下列程式碼示範如何使用[MapHealthChecks](xref:host-and-deploy/health-checks)：</span><span class="sxs-lookup"><span data-stu-id="6dd91-286">The following code shows use of [MapHealthChecks](xref:host-and-deploy/health-checks):</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/AuthorizationStartup.cs?name=snippet)]

<span data-ttu-id="6dd91-287">上述範例顯示為何傳回產生器物件是很重要的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-287">The preceding sample shows why returning the builder object is important.</span></span> <span data-ttu-id="6dd91-288">傳回 builder 物件可讓應用程式開發人員設定原則，例如端點的授權。</span><span class="sxs-lookup"><span data-stu-id="6dd91-288">Returning the builder object allows the app developer to configure policies such as authorization for the endpoint.</span></span> <span data-ttu-id="6dd91-289">在此範例中，健康狀態檢查中介軟體沒有與授權系統的直接整合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-289">In this example, the health checks middleware has no direct integration with the authorization system.</span></span>

<span data-ttu-id="6dd91-290">已建立中繼資料系統，以回應擴充性作者使用終端機中介軟體所遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="6dd91-290">The metadata system was created in response to the problems encountered by extensibility authors using terminal middleware.</span></span> <span data-ttu-id="6dd91-291">每個中介軟體都有問題，使其與授權系統整合在一起。</span><span class="sxs-lookup"><span data-stu-id="6dd91-291">It's problematic for each middleware to implement its own integration with the authorization system.</span></span>

<a name="urlm"></a>

### <a name="url-matching"></a><span data-ttu-id="6dd91-292">URL 比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-292">URL matching</span></span>

* <span data-ttu-id="6dd91-293">這是路由符合連入要求至[端點](#endpoint)的進程。</span><span class="sxs-lookup"><span data-stu-id="6dd91-293">Is the process by which routing matches an incoming request to an [endpoint](#endpoint).</span></span>
* <span data-ttu-id="6dd91-294">是以 URL 路徑和標頭中的資料為基礎。</span><span class="sxs-lookup"><span data-stu-id="6dd91-294">Is based on data in the URL path and headers.</span></span>
* <span data-ttu-id="6dd91-295">可以擴充以考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-295">Can be extended to consider any data in the request.</span></span>

<span data-ttu-id="6dd91-296">當路由中介軟體執行時，它會 `Endpoint` 從目前的要求將和路由值設定為上的[要求功能](xref:fundamentals/request-features) <xref:Microsoft.AspNetCore.Http.HttpContext> ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-296">When a routing middleware executes, it sets an `Endpoint` and route values to a [request feature](xref:fundamentals/request-features) on the <xref:Microsoft.AspNetCore.Http.HttpContext> from the current request:</span></span>

* <span data-ttu-id="6dd91-297">呼叫[HttpCoNtext GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>)會取得端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-297">Calling [HttpContext.GetEndpoint](<xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint*>) gets the endpoint.</span></span>
* <span data-ttu-id="6dd91-298">`HttpRequest.RouteValues` 會取得路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-298">`HttpRequest.RouteValues` gets the collection of route values.</span></span>

<span data-ttu-id="6dd91-299">在路由中介軟體之後執行的[中介軟體](xref:fundamentals/middleware/index)可以檢查端點並採取動作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-299">[Middleware](xref:fundamentals/middleware/index) running after the routing middleware can inspect the endpoint and take action.</span></span> <span data-ttu-id="6dd91-300">例如，授權中介軟體可以針對授權原則詢問端點的元資料集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-300">For example, an authorization middleware can interrogate the endpoint's metadata collection for an authorization policy.</span></span> <span data-ttu-id="6dd91-301">執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。</span><span class="sxs-lookup"><span data-stu-id="6dd91-301">After all of the middleware in the request processing pipeline is executed, the selected endpoint's delegate is invoked.</span></span>

<span data-ttu-id="6dd91-302">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-302">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="6dd91-303">因為中介軟體會根據選取的端點套用原則，所以請務必：</span><span class="sxs-lookup"><span data-stu-id="6dd91-303">Because the middleware applies policies based on the selected endpoint, it's important that:</span></span>

* <span data-ttu-id="6dd91-304">任何可能影響分派或應用安全性原則的決策都是在路由系統內進行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-304">Any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd91-305">針對回溯相容性，當執行控制器或 Razor 頁面端點委派時，會根據到目前為止執行的要求處理，將[routecoNtext.routedata](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData)的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-305">For backwards-compatibility, when a Controller or Razor Pages endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>
>
> <span data-ttu-id="6dd91-306">在 `RouteContext` 未來的版本中，此類型將會標示為已淘汰：</span><span class="sxs-lookup"><span data-stu-id="6dd91-306">The `RouteContext` type will be marked obsolete in a future release:</span></span>
>
> * <span data-ttu-id="6dd91-307">遷移 `RouteData.Values` 至 `HttpRequest.RouteValues` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-307">Migrate `RouteData.Values` to `HttpRequest.RouteValues`.</span></span>
> * <span data-ttu-id="6dd91-308">遷移 `RouteData.DataTokens` 以從端點中繼資料取得[IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-308">Migrate `RouteData.DataTokens` to retrieve [IDataTokensMetadata](xref:Microsoft.AspNetCore.Routing.IDataTokensMetadata) from the endpoint metadata.</span></span>

<span data-ttu-id="6dd91-309">URL 比對作業會以一組可設定的階段來運作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-309">URL matching operates in a configurable set of phases.</span></span> <span data-ttu-id="6dd91-310">在每個階段中，輸出是一組相符專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-310">In each phase, the output is a set of matches.</span></span> <span data-ttu-id="6dd91-311">下一個階段可進一步縮小一組相符專案的範圍。</span><span class="sxs-lookup"><span data-stu-id="6dd91-311">The set of matches can be narrowed down further by the next phase.</span></span> <span data-ttu-id="6dd91-312">路由執行並不保證符合端點的處理順序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-312">The routing implementation does not guarantee a processing order for matching endpoints.</span></span> <span data-ttu-id="6dd91-313">**所有**可能的相符專案都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-313">**All** possible matches are processed at once.</span></span> <span data-ttu-id="6dd91-314">URL 符合的階段會依照下列順序發生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-314">The URL matching phases occur in the following order.</span></span> <span data-ttu-id="6dd91-315">ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="6dd91-315">ASP.NET Core:</span></span>

1. <span data-ttu-id="6dd91-316">會處理一組端點及其路由範本的 URL 路徑，並收集**所有**相符專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-316">Processes the URL path against the set of endpoints and their route templates, collecting **all** of the matches.</span></span>
1. <span data-ttu-id="6dd91-317">接受上述清單，並移除已套用路由條件約束而失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-317">Takes the preceding list and removes matches that fail with route constraints applied.</span></span>
1. <span data-ttu-id="6dd91-318">接受上述清單並移除[MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy)實例集失敗的相符專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-318">Takes the preceding list and removes matches that fail the set of [MatcherPolicy](xref:Microsoft.AspNetCore.Routing.MatcherPolicy) instances.</span></span>
1. <span data-ttu-id="6dd91-319">會使用[EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) ，從上述清單中進行最後的決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-319">Uses the [EndpointSelector](xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector) to make a final decision from the preceding list.</span></span>

<span data-ttu-id="6dd91-320">端點清單的優先順序會根據：</span><span class="sxs-lookup"><span data-stu-id="6dd91-320">The list of endpoints is prioritized according to:</span></span>

* <span data-ttu-id="6dd91-321">[RouteEndpoint。](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span><span class="sxs-lookup"><span data-stu-id="6dd91-321">The [RouteEndpoint.Order](xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order*)</span></span>
* <span data-ttu-id="6dd91-322">[路由範本優先順序](#rtp)</span><span class="sxs-lookup"><span data-stu-id="6dd91-322">The [route template precedence](#rtp)</span></span>

<span data-ttu-id="6dd91-323">所有相符的端點都會在每個階段中處理，直到 <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> 到達為止。</span><span class="sxs-lookup"><span data-stu-id="6dd91-323">All matching endpoints are processed in each phase until the <xref:Microsoft.AspNetCore.Routing.Matching.EndpointSelector> is reached.</span></span> <span data-ttu-id="6dd91-324">`EndpointSelector`是最後一個階段。</span><span class="sxs-lookup"><span data-stu-id="6dd91-324">The `EndpointSelector` is the final phase.</span></span> <span data-ttu-id="6dd91-325">它會從相符專案中選擇最高優先順序的端點，以做為最符合專案。</span><span class="sxs-lookup"><span data-stu-id="6dd91-325">It chooses the highest priority endpoint from the matches as the best match.</span></span> <span data-ttu-id="6dd91-326">如果有其他符合的優先權與最符合的專案相同，則會擲回不明確的相符例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6dd91-326">If there are other matches with the same priority as the best match, an ambiguous match exception is thrown.</span></span>

<span data-ttu-id="6dd91-327">路由優先順序是根據指定較高優先順序的較**特定**路由範本來計算。</span><span class="sxs-lookup"><span data-stu-id="6dd91-327">The route precedence is computed based on a **more specific** route template being given a higher priority.</span></span> <span data-ttu-id="6dd91-328">例如，請考慮範本 `/hello` 和 `/{message}` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-328">For example, consider the templates `/hello` and `/{message}`:</span></span>

* <span data-ttu-id="6dd91-329">兩者都符合 URL 路徑 `/hello` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-329">Both match the URL path `/hello`.</span></span>
* <span data-ttu-id="6dd91-330">`/hello`更明確，因此優先順序較高。</span><span class="sxs-lookup"><span data-stu-id="6dd91-330">`/hello`  is more specific and therefore higher priority.</span></span>

<span data-ttu-id="6dd91-331">一般來說，路由優先順序會針對實務中使用的 URL 配置類型選擇最符合的工作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-331">In general, route precedence does a good job of choosing the best match for the kinds of URL schemes used in practice.</span></span> <span data-ttu-id="6dd91-332"><xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order>只有在必要時才使用，以避免發生不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="6dd91-332">Use <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.Order> only when necessary to avoid an ambiguity.</span></span>

<span data-ttu-id="6dd91-333">由於路由所提供的擴充性種類，路由系統無法在不明確的路由時間之前計算。</span><span class="sxs-lookup"><span data-stu-id="6dd91-333">Due to the kinds of extensibility provided by routing, it isn't possible for the routing system to compute ahead of time the ambiguous routes.</span></span> <span data-ttu-id="6dd91-334">假設有一個範例，例如路由範本 `/{message:alpha}` 和 `/{message:int}` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-334">Consider an example such as the route templates `/{message:alpha}` and `/{message:int}`:</span></span>

* <span data-ttu-id="6dd91-335">`alpha`條件約束只符合字母字元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-335">The `alpha` constraint matches only alphabetic characters.</span></span>
* <span data-ttu-id="6dd91-336">`int`條件約束只符合數位。</span><span class="sxs-lookup"><span data-stu-id="6dd91-336">The `int` constraint matches only numbers.</span></span>
* <span data-ttu-id="6dd91-337">這些範本具有相同的路由優先順序，但沒有相符的單一 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-337">These templates have the same route precedence, but there's no single URL they both match.</span></span>
* <span data-ttu-id="6dd91-338">如果路由系統在啟動時報告了不明確的錯誤，則會封鎖此有效的使用案例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-338">If the routing system reported an ambiguity error at startup, it would block this valid use case.</span></span>

> [!WARNING]
>
> <span data-ttu-id="6dd91-339">內部作業的順序 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 不會影響路由的行為，但有一個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6dd91-339">The order of operations inside <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> doesn't influence the behavior of routing, with one exception.</span></span> <span data-ttu-id="6dd91-340"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和會根據叫用 <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> 的順序，自動指派訂單值給其端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-340"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="6dd91-341">這會模擬控制器的長時間行為，而不會有路由系統提供與舊版路由執行相同的保證。</span><span class="sxs-lookup"><span data-stu-id="6dd91-341">This simulates long-time behavior of controllers without the routing system providing the same guarantees as older routing implementations.</span></span>
>
> <span data-ttu-id="6dd91-342">在路由的舊版執行中，可以根據路由的處理順序來執行路由擴充性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-342">In the legacy implementation of routing, it's possible to implement routing extensibility that has a dependency on the order in which routes are processed.</span></span> <span data-ttu-id="6dd91-343">ASP.NET Core 3.0 和更新版本中的端點路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-343">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>
> 
> * <span data-ttu-id="6dd91-344">沒有路由的概念。</span><span class="sxs-lookup"><span data-stu-id="6dd91-344">Doesn't have a concept of routes.</span></span>
> * <span data-ttu-id="6dd91-345">不提供排序保證。</span><span class="sxs-lookup"><span data-stu-id="6dd91-345">Doesn't provide ordering guarantees.</span></span> <span data-ttu-id="6dd91-346">所有端點都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-346">All endpoints are processed at once.</span></span>
>
> <span data-ttu-id="6dd91-347">如果這表示您在使用舊版路由系統時停滯，請[開啟 GitHub 問題以取得協助](https://github.com/dotnet/aspnetcore/issues)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-347">If this means you're stuck using the legacy routing system, [open a GitHub issue for assistance](https://github.com/dotnet/aspnetcore/issues).</span></span>

<a name="rtp"></a>

### <a name="route-template-precedence-and-endpoint-selection-order"></a><span data-ttu-id="6dd91-348">路由範本優先順序和端點選取順序</span><span class="sxs-lookup"><span data-stu-id="6dd91-348">Route template precedence and endpoint selection order</span></span>

<span data-ttu-id="6dd91-349">[路由範本優先順序](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16)是一種系統，它會根據特定的方式，為每個路由範本指派一個值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-349">[Route template precedence](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L16) is a system that assigns each route template a value based on how specific it is.</span></span> <span data-ttu-id="6dd91-350">路由範本優先順序：</span><span class="sxs-lookup"><span data-stu-id="6dd91-350">Route template precedence:</span></span>

* <span data-ttu-id="6dd91-351">避免在一般情況下調整端點順序的需求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-351">Avoids the need to adjust the order of endpoints in common cases.</span></span>
* <span data-ttu-id="6dd91-352">嘗試符合常見的路由行為預期。</span><span class="sxs-lookup"><span data-stu-id="6dd91-352">Attempts to match the common-sense expectations of routing behavior.</span></span>

<span data-ttu-id="6dd91-353">例如，請考慮範本 `/Products/List` 和 `/Products/{id}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-353">For example, consider templates `/Products/List` and `/Products/{id}`.</span></span> <span data-ttu-id="6dd91-354">假設 `/Products/List` 比起 URL 路徑的比對更相符，這是合理的做法 `/Products/{id}` `/Products/List` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-354">It would be reasonable to assume that `/Products/List` is a better match than `/Products/{id}` for the URL path `/Products/List`.</span></span> <span data-ttu-id="6dd91-355">的作用是因為常 `/List` 值區段被視為具有比參數區段更好的優先順序 `/{id}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-355">The works because the literal segment `/List` is considered to have better precedence than the parameter segment `/{id}`.</span></span>

<span data-ttu-id="6dd91-356">優先順序運作方式的詳細資料會結合如何定義路由範本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-356">The details of how precedence works are coupled to how route templates are defined:</span></span>

* <span data-ttu-id="6dd91-357">具有更多區段的範本會被視為更明確。</span><span class="sxs-lookup"><span data-stu-id="6dd91-357">Templates with more segments are considered more specific.</span></span>
* <span data-ttu-id="6dd91-358">具有常值文字的區段會被視為比參數區段更明確。</span><span class="sxs-lookup"><span data-stu-id="6dd91-358">A segment with literal text is considered more specific than a parameter segment.</span></span>
* <span data-ttu-id="6dd91-359">具有條件約束的參數區段在沒有的情況下會被視為較明確。</span><span class="sxs-lookup"><span data-stu-id="6dd91-359">A parameter segment with a constraint is considered more specific than one without.</span></span>
* <span data-ttu-id="6dd91-360">複雜的區段會被視為具有條件約束的參數區段。</span><span class="sxs-lookup"><span data-stu-id="6dd91-360">A complex segment is considered as specific as a parameter segment with a constraint.</span></span>
* <span data-ttu-id="6dd91-361">Catch-all 參數是最不明確的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-361">Catch-all parameters are the least specific.</span></span> <span data-ttu-id="6dd91-362">如需有關 catch all 路由的重要資訊，請參閱[路由範本參考](#rtr)中的**全部攔截**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-362">See **catch-all** in the [Route template reference](#rtr) for important information on catch-all routes.</span></span>

<span data-ttu-id="6dd91-363">如需確切值的參考，請參閱[GitHub 上的原始程式碼](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-363">See the [source code on GitHub](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Template/RoutePrecedence.cs#L189) for a reference of exact values.</span></span>

<a name="lg"></a>

### <a name="url-generation-concepts"></a><span data-ttu-id="6dd91-364">URL 產生概念</span><span class="sxs-lookup"><span data-stu-id="6dd91-364">URL generation concepts</span></span>

<span data-ttu-id="6dd91-365">URL 產生：</span><span class="sxs-lookup"><span data-stu-id="6dd91-365">URL generation:</span></span>

* <span data-ttu-id="6dd91-366">這是路由可以根據一組路由值建立 URL 路徑的進程。</span><span class="sxs-lookup"><span data-stu-id="6dd91-366">Is the process by which routing can create a URL path based on a set of route values.</span></span>
* <span data-ttu-id="6dd91-367">允許端點與存取它們的 Url 之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-367">Allows for a logical separation between endpoints and the URLs that access them.</span></span>

<span data-ttu-id="6dd91-368">端點路由包含 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API。</span><span class="sxs-lookup"><span data-stu-id="6dd91-368">Endpoint routing includes the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API.</span></span> <span data-ttu-id="6dd91-369">`LinkGenerator`是[DI](xref:fundamentals/dependency-injection)提供的單一服務。</span><span class="sxs-lookup"><span data-stu-id="6dd91-369">`LinkGenerator` is a singleton service available from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6dd91-370">`LinkGenerator`API 可以在執行中要求的內容之外使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-370">The `LinkGenerator` API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="6dd91-371">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper)和依賴的案例，例如標籤協助程式 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)，會在[Tag Helpers](xref:mvc/views/tag-helpers/intro)內部使用 `LinkGenerator` API 來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-371">[Mvc.IUrlHelper](xref:Microsoft.AspNetCore.Mvc.IUrlHelper) and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the `LinkGenerator` API internally to provide link generating capabilities.</span></span>

<span data-ttu-id="6dd91-372">連結產生器背後支援的概念為「位址」\*\*\*\* 和「位址配置」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-372">The link generator is backed by the concept of an **address** and **address schemes**.</span></span> <span data-ttu-id="6dd91-373">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-373">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="6dd91-374">例如，許多使用者熟悉的路由名稱和路由值案例會將控制器和頁面實 Razor 作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-374">For example, the route name and route values scenarios many users are familiar with from controllers and Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="6dd91-375">連結產生器可以透過下列擴充方法連結至控制器和 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="6dd91-375">The link generator can link to controllers and Razor Pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="6dd91-376">這些方法的多載會接受包含的引數 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-376">Overloads of these methods accept arguments that include the `HttpContext`.</span></span> <span data-ttu-id="6dd91-377">這些方法在功能上相當於[url. 動作](xref:System.Web.Mvc.UrlHelper.Action*)和[url](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)，但提供額外的彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="6dd91-377">These methods are functionally equivalent to [Url.Action](xref:System.Web.Mvc.UrlHelper.Action*) and [Url.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*), but offer additional flexibility and options.</span></span>

<span data-ttu-id="6dd91-378">這些 `GetPath*` 方法與和最相似 `Url.Action` `Url.Page` ，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-378">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page`, in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="6dd91-379">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-379">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="6dd91-380">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-380">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="6dd91-381">除非覆寫，否則會使用執行中要求的[環境](#ambient)路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="6dd91-381">The [ambient](#ambient) route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="6dd91-382">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="6dd91-382"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="6dd91-383">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="6dd91-383">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="6dd91-384">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="6dd91-384">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="6dd91-385">評估每個端點的 <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern>，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-385">Each endpoint's <xref:Microsoft.AspNetCore.Routing.RouteEndpoint.RoutePattern> is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="6dd91-386">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="6dd91-386">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="6dd91-387"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-387">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="6dd91-388">使用連結產生器最方便的方式，就是透過擴充方法來執行特定網址類別型的作業：</span><span class="sxs-lookup"><span data-stu-id="6dd91-388">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type:</span></span>

| <span data-ttu-id="6dd91-389">擴充方法</span><span class="sxs-lookup"><span data-stu-id="6dd91-389">Extension Method</span></span> | <span data-ttu-id="6dd91-390">描述</span><span class="sxs-lookup"><span data-stu-id="6dd91-390">Description</span></span> |
| ---
<span data-ttu-id="6dd91-391">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-391">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-392">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-392">'Blazor'</span></span>
- <span data-ttu-id="6dd91-393">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-393">'Identity'</span></span>
- <span data-ttu-id="6dd91-394">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-394">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-395">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-395">'Razor'</span></span>
- <span data-ttu-id="6dd91-396">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-396">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-397">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-397">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-398">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-398">'Blazor'</span></span>
- <span data-ttu-id="6dd91-399">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-399">'Identity'</span></span>
- <span data-ttu-id="6dd91-400">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-400">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-401">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-401">'Razor'</span></span>
- <span data-ttu-id="6dd91-402">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-402">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-403">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-403">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-404">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-404">'Blazor'</span></span>
- <span data-ttu-id="6dd91-405">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-405">'Identity'</span></span>
- <span data-ttu-id="6dd91-406">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-406">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-407">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-407">'Razor'</span></span>
- <span data-ttu-id="6dd91-408">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-408">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-409">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-409">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-410">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-410">'Blazor'</span></span>
- <span data-ttu-id="6dd91-411">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-411">'Identity'</span></span>
- <span data-ttu-id="6dd91-412">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-412">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-413">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-413">'Razor'</span></span>
- <span data-ttu-id="6dd91-414">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-414">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-415">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-415">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-416">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-416">'Blazor'</span></span>
- <span data-ttu-id="6dd91-417">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-417">'Identity'</span></span>
- <span data-ttu-id="6dd91-418">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-418">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-419">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-419">'Razor'</span></span>
- <span data-ttu-id="6dd91-420">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-420">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-421">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-421">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-422">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-422">'Blazor'</span></span>
- <span data-ttu-id="6dd91-423">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-423">'Identity'</span></span>
- <span data-ttu-id="6dd91-424">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-424">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-425">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-425">'Razor'</span></span>
- <span data-ttu-id="6dd91-426">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-426">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-427">-------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-427">-------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-428">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-428">'Blazor'</span></span>
- <span data-ttu-id="6dd91-429">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-429">'Identity'</span></span>
- <span data-ttu-id="6dd91-430">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-430">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-431">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-431">'Razor'</span></span>
- <span data-ttu-id="6dd91-432">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-432">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-433">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-433">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-434">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-434">'Blazor'</span></span>
- <span data-ttu-id="6dd91-435">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-435">'Identity'</span></span>
- <span data-ttu-id="6dd91-436">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-436">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-437">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-437">'Razor'</span></span>
- <span data-ttu-id="6dd91-438">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-438">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-439">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-439">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-440">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-440">'Blazor'</span></span>
- <span data-ttu-id="6dd91-441">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-441">'Identity'</span></span>
- <span data-ttu-id="6dd91-442">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-442">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-443">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-443">'Razor'</span></span>
- <span data-ttu-id="6dd91-444">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-444">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-445">------ | |<xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> |根據提供的值，產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-445">------ | | <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Generates a URI with an absolute path based on the provided values.</span></span> <span data-ttu-id="6dd91-446">| |<xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> |根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-446">| | <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="6dd91-447">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="6dd91-447">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="6dd91-448">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="6dd91-448">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="6dd91-449">如果 `Host` 未驗證傳入要求的標頭，則不受信任的要求輸入可以在視圖或頁面的 uri 中傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="6dd91-449">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view or page.</span></span> <span data-ttu-id="6dd91-450">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-450">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="6dd91-451">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-451">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="6dd91-452">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-452">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="6dd91-453">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-453">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="6dd91-454">指定空的基底路徑，以復原 `Map*` 連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="6dd91-454">Specify an empty base path to undo the `Map*` affect on link generation.</span></span>

### <a name="middleware-example"></a><span data-ttu-id="6dd91-455">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="6dd91-455">Middleware example</span></span>

<span data-ttu-id="6dd91-456">在下列範例中，中介軟體會使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 來建立列出商店產品之動作方法的連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-456">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create a link to an action method that lists store products.</span></span> <span data-ttu-id="6dd91-457">藉由將連結產生器插入類別並呼叫， `GenerateLink` 可供應用程式中的任何類別使用：</span><span class="sxs-lookup"><span data-stu-id="6dd91-457">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Middleware/ProductsLinkMiddleware.cs?name=snippet)]

<a name="rtr"></a>

## <a name="route-template-reference"></a><span data-ttu-id="6dd91-458">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-458">Route template reference</span></span>

<span data-ttu-id="6dd91-459">內 `{}` 的 token 會定義路由符合時所系結的路由參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-459">Tokens within `{}` define route parameters that are bound if the route is matched.</span></span> <span data-ttu-id="6dd91-460">在路由區段中可以定義一個以上的路由參數，但路由參數必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-460">More than one route parameter can be defined in a route segment, but route parameters  must be separated by a literal value.</span></span> <span data-ttu-id="6dd91-461">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-461">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span>  <span data-ttu-id="6dd91-462">路由參數必須有名稱，而且可能會指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-462">Route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="6dd91-463">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="6dd91-463">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="6dd91-464">文字比對不區分大小寫，而且會根據 URL 路徑的解碼標記法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-464">Text matching is case-insensitive and based on the decoded representation of the URL's path.</span></span> <span data-ttu-id="6dd91-465">若要比對常值路由參數分隔符號 `{` 或 `}` ，請重複字元來將分隔符號轉義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-465">To match a literal route parameter delimiter `{` or `}`, escape the delimiter by repeating the character.</span></span> <span data-ttu-id="6dd91-466">例如， `{{` 或 `}}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-466">For example `{{` or `}}`.</span></span>

<span data-ttu-id="6dd91-467">星號 `*` 或雙星號 `**` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-467">Asterisk `*` or double asterisk `**`:</span></span>

* <span data-ttu-id="6dd91-468">可以做為路由參數的前置詞，以系結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="6dd91-468">Can be used as a prefix to a route parameter to bind to the rest of the URI.</span></span>
* <span data-ttu-id="6dd91-469">稱為「**全部攔截**」參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-469">Are called a **catch-all** parameters.</span></span> <span data-ttu-id="6dd91-470">例如，`blog/{**slug}`：</span><span class="sxs-lookup"><span data-stu-id="6dd91-470">For example, `blog/{**slug}`:</span></span>
  * <span data-ttu-id="6dd91-471">會比對開頭為的任何 URI `/blog` ，並在其後面加上任何值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-471">Matches any URI that starts with `/blog` and has any value following it.</span></span>
  * <span data-ttu-id="6dd91-472">下列值 `/blog` 會指派給 [[資訊](https://developer.mozilla.org/docs/Glossary/Slug)區路線] 路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-472">The value following `/blog` is assigned to the [slug](https://developer.mozilla.org/docs/Glossary/Slug) route value.</span></span>

[!INCLUDE[](~/includes/catchall.md)]

<span data-ttu-id="6dd91-473">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-473">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="6dd91-474">當使用路由來產生 URL （包括路徑分隔符號）時，catch-all 參數會將適當的字元進行轉義 `/` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-474">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator `/` characters.</span></span> <span data-ttu-id="6dd91-475">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-475">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="6dd91-476">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-476">Note the escaped forward slash.</span></span> <span data-ttu-id="6dd91-477">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="6dd91-477">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="6dd91-478">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-478">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="6dd91-479">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="6dd91-479">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="6dd91-480">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-480">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="6dd91-481">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-481">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="6dd91-482">如果 `filename` URL 中只有存在的值，則路由會符合，因為尾端 `.` 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-482">If only a value for `filename` exists in the URL, the route matches because the trailing `.` is  optional.</span></span> <span data-ttu-id="6dd91-483">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-483">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="6dd91-484">路由參數可能有「預設值」\*\*\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-484">Route parameters may have **default values** designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="6dd91-485">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-485">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="6dd91-486">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-486">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="6dd91-487">將問號（ `?` ）附加至參數名稱的結尾，可將路由參數設為選擇性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-487">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name.</span></span> <span data-ttu-id="6dd91-488">例如 `id?`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-488">For example, `id?`.</span></span> <span data-ttu-id="6dd91-489">選擇性值與預設路由參數之間的差異如下：</span><span class="sxs-lookup"><span data-stu-id="6dd91-489">The difference between optional values and default route parameters is:</span></span>

* <span data-ttu-id="6dd91-490">具有預設值的路由參數一律會產生值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-490">A route parameter with a default value always produces a value.</span></span>
* <span data-ttu-id="6dd91-491">選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-491">An optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="6dd91-492">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-492">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="6dd91-493">在 `:` 路由參數名稱之後加入和條件約束名稱，會在路由參數上指定內嵌條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-493">Adding `:` and constraint name after the route parameter name specifies an inline constraint on a route parameter.</span></span> <span data-ttu-id="6dd91-494">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 `(...)` 括住。</span><span class="sxs-lookup"><span data-stu-id="6dd91-494">If the constraint requires arguments, they're enclosed in parentheses `(...)` after the constraint name.</span></span> <span data-ttu-id="6dd91-495">藉由附加另一個和條件約束名稱，可以指定多個*內嵌條件約束* `:` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-495">Multiple *inline constraints* can be specified by appending another `:` and constraint name.</span></span>

<span data-ttu-id="6dd91-496">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-496">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="6dd91-497">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-497">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="6dd91-498">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-498">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="6dd91-499">路由參數也可以具有參數轉換器。</span><span class="sxs-lookup"><span data-stu-id="6dd91-499">Route parameters may also have parameter transformers.</span></span> <span data-ttu-id="6dd91-500">參數轉換器會在產生連結並比對動作和頁面到 Url 時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-500">Parameter transformers transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="6dd91-501">如同條件約束，參數轉換器可以藉由 `:` 在路由參數名稱之後新增和轉換器名稱，以內嵌方式加入至路由參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-501">Like constraints, parameter transformers can be added inline to a route parameter by adding a `:` and transformer name after the route parameter name.</span></span> <span data-ttu-id="6dd91-502">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="6dd91-502">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="6dd91-503">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-503">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="6dd91-504">下表示范範例路由範本及其行為：</span><span class="sxs-lookup"><span data-stu-id="6dd91-504">The following table demonstrates example route templates and their behavior:</span></span>

| <span data-ttu-id="6dd91-505">路由範本</span><span class="sxs-lookup"><span data-stu-id="6dd91-505">Route Template</span></span>                           | <span data-ttu-id="6dd91-506">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="6dd91-506">Example Matching URI</span></span>    | <span data-ttu-id="6dd91-507">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="6dd91-507">The request URI&hellip;</span></span>                                                    |
| ---
<span data-ttu-id="6dd91-508">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-508">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-509">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-509">'Blazor'</span></span>
- <span data-ttu-id="6dd91-510">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-510">'Identity'</span></span>
- <span data-ttu-id="6dd91-511">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-511">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-512">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-512">'Razor'</span></span>
- <span data-ttu-id="6dd91-513">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-513">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-514">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-514">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-515">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-515">'Blazor'</span></span>
- <span data-ttu-id="6dd91-516">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-516">'Identity'</span></span>
- <span data-ttu-id="6dd91-517">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-517">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-518">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-518">'Razor'</span></span>
- <span data-ttu-id="6dd91-519">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-519">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-520">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-520">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-521">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-521">'Blazor'</span></span>
- <span data-ttu-id="6dd91-522">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-522">'Identity'</span></span>
- <span data-ttu-id="6dd91-523">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-523">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-524">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-524">'Razor'</span></span>
- <span data-ttu-id="6dd91-525">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-525">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-526">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-526">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-527">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-527">'Blazor'</span></span>
- <span data-ttu-id="6dd91-528">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-528">'Identity'</span></span>
- <span data-ttu-id="6dd91-529">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-529">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-530">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-530">'Razor'</span></span>
- <span data-ttu-id="6dd91-531">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-531">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-532">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-532">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-533">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-533">'Blazor'</span></span>
- <span data-ttu-id="6dd91-534">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-534">'Identity'</span></span>
- <span data-ttu-id="6dd91-535">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-535">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-536">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-536">'Razor'</span></span>
- <span data-ttu-id="6dd91-537">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-537">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-538">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-538">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-539">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-539">'Blazor'</span></span>
- <span data-ttu-id="6dd91-540">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-540">'Identity'</span></span>
- <span data-ttu-id="6dd91-541">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-541">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-542">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-542">'Razor'</span></span>
- <span data-ttu-id="6dd91-543">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-543">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-544">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-544">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-545">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-545">'Blazor'</span></span>
- <span data-ttu-id="6dd91-546">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-546">'Identity'</span></span>
- <span data-ttu-id="6dd91-547">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-547">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-548">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-548">'Razor'</span></span>
- <span data-ttu-id="6dd91-549">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-549">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-550">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-550">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-551">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-551">'Blazor'</span></span>
- <span data-ttu-id="6dd91-552">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-552">'Identity'</span></span>
- <span data-ttu-id="6dd91-553">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-553">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-554">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-554">'Razor'</span></span>
- <span data-ttu-id="6dd91-555">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-555">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-556">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-556">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-557">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-557">'Blazor'</span></span>
- <span data-ttu-id="6dd91-558">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-558">'Identity'</span></span>
- <span data-ttu-id="6dd91-559">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-559">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-560">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-560">'Razor'</span></span>
- <span data-ttu-id="6dd91-561">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-561">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-562">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-562">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-563">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-563">'Blazor'</span></span>
- <span data-ttu-id="6dd91-564">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-564">'Identity'</span></span>
- <span data-ttu-id="6dd91-565">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-565">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-566">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-566">'Razor'</span></span>
- <span data-ttu-id="6dd91-567">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-567">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-568">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-568">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-569">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-569">'Blazor'</span></span>
- <span data-ttu-id="6dd91-570">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-570">'Identity'</span></span>
- <span data-ttu-id="6dd91-571">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-571">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-572">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-572">'Razor'</span></span>
- <span data-ttu-id="6dd91-573">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-573">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-574">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-574">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-575">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-575">'Blazor'</span></span>
- <span data-ttu-id="6dd91-576">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-576">'Identity'</span></span>
- <span data-ttu-id="6dd91-577">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-577">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-578">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-578">'Razor'</span></span>
- <span data-ttu-id="6dd91-579">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-579">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-580">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-580">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-581">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-581">'Blazor'</span></span>
- <span data-ttu-id="6dd91-582">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-582">'Identity'</span></span>
- <span data-ttu-id="6dd91-583">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-583">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-584">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-584">'Razor'</span></span>
- <span data-ttu-id="6dd91-585">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-585">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-586">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-586">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-587">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-587">'Blazor'</span></span>
- <span data-ttu-id="6dd91-588">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-588">'Identity'</span></span>
- <span data-ttu-id="6dd91-589">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-589">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-590">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-590">'Razor'</span></span>
- <span data-ttu-id="6dd91-591">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-591">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-592">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-592">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-593">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-593">'Blazor'</span></span>
- <span data-ttu-id="6dd91-594">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-594">'Identity'</span></span>
- <span data-ttu-id="6dd91-595">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-595">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-596">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-596">'Razor'</span></span>
- <span data-ttu-id="6dd91-597">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-597">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-598">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-598">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-599">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-599">'Blazor'</span></span>
- <span data-ttu-id="6dd91-600">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-600">'Identity'</span></span>
- <span data-ttu-id="6dd91-601">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-601">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-602">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-602">'Razor'</span></span>
- <span data-ttu-id="6dd91-603">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-603">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-604">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-604">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-605">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-605">'Blazor'</span></span>
- <span data-ttu-id="6dd91-606">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-606">'Identity'</span></span>
- <span data-ttu-id="6dd91-607">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-607">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-608">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-608">'Razor'</span></span>
- <span data-ttu-id="6dd91-609">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-609">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-610">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-610">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-611">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-611">'Blazor'</span></span>
- <span data-ttu-id="6dd91-612">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-612">'Identity'</span></span>
- <span data-ttu-id="6dd91-613">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-613">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-614">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-614">'Razor'</span></span>
- <span data-ttu-id="6dd91-615">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-615">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-616">-------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-616">-------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-617">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-617">'Blazor'</span></span>
- <span data-ttu-id="6dd91-618">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-618">'Identity'</span></span>
- <span data-ttu-id="6dd91-619">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-619">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-620">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-620">'Razor'</span></span>
- <span data-ttu-id="6dd91-621">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-621">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-622">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-622">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-623">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-623">'Blazor'</span></span>
- <span data-ttu-id="6dd91-624">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-624">'Identity'</span></span>
- <span data-ttu-id="6dd91-625">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-625">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-626">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-626">'Razor'</span></span>
- <span data-ttu-id="6dd91-627">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-627">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-628">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-628">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-629">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-629">'Blazor'</span></span>
- <span data-ttu-id="6dd91-630">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-630">'Identity'</span></span>
- <span data-ttu-id="6dd91-631">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-631">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-632">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-632">'Razor'</span></span>
- <span data-ttu-id="6dd91-633">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-633">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-634">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-634">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-635">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-635">'Blazor'</span></span>
- <span data-ttu-id="6dd91-636">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-636">'Identity'</span></span>
- <span data-ttu-id="6dd91-637">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-637">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-638">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-638">'Razor'</span></span>
- <span data-ttu-id="6dd91-639">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-639">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-640">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-640">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-641">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-641">'Blazor'</span></span>
- <span data-ttu-id="6dd91-642">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-642">'Identity'</span></span>
- <span data-ttu-id="6dd91-643">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-643">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-644">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-644">'Razor'</span></span>
- <span data-ttu-id="6dd91-645">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-645">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-646">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-646">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-647">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-647">'Blazor'</span></span>
- <span data-ttu-id="6dd91-648">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-648">'Identity'</span></span>
- <span data-ttu-id="6dd91-649">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-649">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-650">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-650">'Razor'</span></span>
- <span data-ttu-id="6dd91-651">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-651">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-652">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-652">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-653">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-653">'Blazor'</span></span>
- <span data-ttu-id="6dd91-654">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-654">'Identity'</span></span>
- <span data-ttu-id="6dd91-655">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-655">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-656">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-656">'Razor'</span></span>
- <span data-ttu-id="6dd91-657">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-657">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-658">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-658">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-659">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-659">'Blazor'</span></span>
- <span data-ttu-id="6dd91-660">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-660">'Identity'</span></span>
- <span data-ttu-id="6dd91-661">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-661">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-662">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-662">'Razor'</span></span>
- <span data-ttu-id="6dd91-663">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-663">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-664">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-664">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-665">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-665">'Blazor'</span></span>
- <span data-ttu-id="6dd91-666">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-666">'Identity'</span></span>
- <span data-ttu-id="6dd91-667">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-667">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-668">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-668">'Razor'</span></span>
- <span data-ttu-id="6dd91-669">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-669">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-670">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-670">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-671">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-671">'Blazor'</span></span>
- <span data-ttu-id="6dd91-672">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-672">'Identity'</span></span>
- <span data-ttu-id="6dd91-673">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-673">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-674">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-674">'Razor'</span></span>
- <span data-ttu-id="6dd91-675">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-675">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-676">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-676">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-677">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-677">'Blazor'</span></span>
- <span data-ttu-id="6dd91-678">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-678">'Identity'</span></span>
- <span data-ttu-id="6dd91-679">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-679">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-680">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-680">'Razor'</span></span>
- <span data-ttu-id="6dd91-681">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-681">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-682">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-682">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-683">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-683">'Blazor'</span></span>
- <span data-ttu-id="6dd91-684">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-684">'Identity'</span></span>
- <span data-ttu-id="6dd91-685">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-685">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-686">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-686">'Razor'</span></span>
- <span data-ttu-id="6dd91-687">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-687">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-688">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-688">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-689">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-689">'Blazor'</span></span>
- <span data-ttu-id="6dd91-690">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-690">'Identity'</span></span>
- <span data-ttu-id="6dd91-691">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-691">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-692">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-692">'Razor'</span></span>
- <span data-ttu-id="6dd91-693">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-693">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-694">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-694">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-695">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-695">'Blazor'</span></span>
- <span data-ttu-id="6dd91-696">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-696">'Identity'</span></span>
- <span data-ttu-id="6dd91-697">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-697">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-698">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-698">'Razor'</span></span>
- <span data-ttu-id="6dd91-699">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-699">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-700">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-700">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-701">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-701">'Blazor'</span></span>
- <span data-ttu-id="6dd91-702">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-702">'Identity'</span></span>
- <span data-ttu-id="6dd91-703">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-703">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-704">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-704">'Razor'</span></span>
- <span data-ttu-id="6dd91-705">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-705">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-706">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-706">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-707">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-707">'Blazor'</span></span>
- <span data-ttu-id="6dd91-708">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-708">'Identity'</span></span>
- <span data-ttu-id="6dd91-709">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-709">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-710">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-710">'Razor'</span></span>
- <span data-ttu-id="6dd91-711">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-711">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-712">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-712">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-713">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-713">'Blazor'</span></span>
- <span data-ttu-id="6dd91-714">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-714">'Identity'</span></span>
- <span data-ttu-id="6dd91-715">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-715">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-716">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-716">'Razor'</span></span>
- <span data-ttu-id="6dd91-717">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-717">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-718">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-718">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-719">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-719">'Blazor'</span></span>
- <span data-ttu-id="6dd91-720">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-720">'Identity'</span></span>
- <span data-ttu-id="6dd91-721">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-721">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-722">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-722">'Razor'</span></span>
- <span data-ttu-id="6dd91-723">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-723">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-724">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-724">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-725">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-725">'Blazor'</span></span>
- <span data-ttu-id="6dd91-726">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-726">'Identity'</span></span>
- <span data-ttu-id="6dd91-727">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-727">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-728">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-728">'Razor'</span></span>
- <span data-ttu-id="6dd91-729">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-729">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-730">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-730">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-731">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-731">'Blazor'</span></span>
- <span data-ttu-id="6dd91-732">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-732">'Identity'</span></span>
- <span data-ttu-id="6dd91-733">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-733">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-734">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-734">'Razor'</span></span>
- <span data-ttu-id="6dd91-735">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-735">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-736">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-736">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-737">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-737">'Blazor'</span></span>
- <span data-ttu-id="6dd91-738">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-738">'Identity'</span></span>
- <span data-ttu-id="6dd91-739">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-739">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-740">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-740">'Razor'</span></span>
- <span data-ttu-id="6dd91-741">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-741">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-742">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-742">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-743">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-743">'Blazor'</span></span>
- <span data-ttu-id="6dd91-744">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-744">'Identity'</span></span>
- <span data-ttu-id="6dd91-745">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-745">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-746">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-746">'Razor'</span></span>
- <span data-ttu-id="6dd91-747">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-747">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-748">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-748">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-749">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-749">'Blazor'</span></span>
- <span data-ttu-id="6dd91-750">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-750">'Identity'</span></span>
- <span data-ttu-id="6dd91-751">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-751">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-752">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-752">'Razor'</span></span>
- <span data-ttu-id="6dd91-753">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-753">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-754">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-754">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-755">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-755">'Blazor'</span></span>
- <span data-ttu-id="6dd91-756">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-756">'Identity'</span></span>
- <span data-ttu-id="6dd91-757">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-757">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-758">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-758">'Razor'</span></span>
- <span data-ttu-id="6dd91-759">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-759">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-760">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-760">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-761">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-761">'Blazor'</span></span>
- <span data-ttu-id="6dd91-762">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-762">'Identity'</span></span>
- <span data-ttu-id="6dd91-763">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-763">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-764">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-764">'Razor'</span></span>
- <span data-ttu-id="6dd91-765">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-765">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-766">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-766">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-767">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-767">'Blazor'</span></span>
- <span data-ttu-id="6dd91-768">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-768">'Identity'</span></span>
- <span data-ttu-id="6dd91-769">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-769">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-770">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-770">'Razor'</span></span>
- <span data-ttu-id="6dd91-771">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-771">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-772">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-772">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-773">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-773">'Blazor'</span></span>
- <span data-ttu-id="6dd91-774">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-774">'Identity'</span></span>
- <span data-ttu-id="6dd91-775">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-775">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-776">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-776">'Razor'</span></span>
- <span data-ttu-id="6dd91-777">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-777">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-778">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-778">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-779">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-779">'Blazor'</span></span>
- <span data-ttu-id="6dd91-780">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-780">'Identity'</span></span>
- <span data-ttu-id="6dd91-781">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-781">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-782">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-782">'Razor'</span></span>
- <span data-ttu-id="6dd91-783">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-783">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-784">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-784">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-785">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-785">'Blazor'</span></span>
- <span data-ttu-id="6dd91-786">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-786">'Identity'</span></span>
- <span data-ttu-id="6dd91-787">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-787">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-788">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-788">'Razor'</span></span>
- <span data-ttu-id="6dd91-789">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-789">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-790">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-790">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-791">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-791">'Blazor'</span></span>
- <span data-ttu-id="6dd91-792">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-792">'Identity'</span></span>
- <span data-ttu-id="6dd91-793">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-793">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-794">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-794">'Razor'</span></span>
- <span data-ttu-id="6dd91-795">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-795">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-796">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-796">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-797">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-797">'Blazor'</span></span>
- <span data-ttu-id="6dd91-798">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-798">'Identity'</span></span>
- <span data-ttu-id="6dd91-799">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-799">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-800">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-800">'Razor'</span></span>
- <span data-ttu-id="6dd91-801">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-801">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-802">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-802">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-803">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-803">'Blazor'</span></span>
- <span data-ttu-id="6dd91-804">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-804">'Identity'</span></span>
- <span data-ttu-id="6dd91-805">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-805">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-806">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-806">'Razor'</span></span>
- <span data-ttu-id="6dd91-807">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-807">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-808">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-808">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-809">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-809">'Blazor'</span></span>
- <span data-ttu-id="6dd91-810">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-810">'Identity'</span></span>
- <span data-ttu-id="6dd91-811">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-811">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-812">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-812">'Razor'</span></span>
- <span data-ttu-id="6dd91-813">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-813">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-814">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-814">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-815">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-815">'Blazor'</span></span>
- <span data-ttu-id="6dd91-816">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-816">'Identity'</span></span>
- <span data-ttu-id="6dd91-817">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-817">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-818">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-818">'Razor'</span></span>
- <span data-ttu-id="6dd91-819">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-819">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-820">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-820">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-821">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-821">'Blazor'</span></span>
- <span data-ttu-id="6dd91-822">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-822">'Identity'</span></span>
- <span data-ttu-id="6dd91-823">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-823">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-824">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-824">'Razor'</span></span>
- <span data-ttu-id="6dd91-825">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-825">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-826">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-826">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-827">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-827">'Blazor'</span></span>
- <span data-ttu-id="6dd91-828">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-828">'Identity'</span></span>
- <span data-ttu-id="6dd91-829">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-829">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-830">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-830">'Razor'</span></span>
- <span data-ttu-id="6dd91-831">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-831">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-832">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-832">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-833">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-833">'Blazor'</span></span>
- <span data-ttu-id="6dd91-834">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-834">'Identity'</span></span>
- <span data-ttu-id="6dd91-835">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-835">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-836">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-836">'Razor'</span></span>
- <span data-ttu-id="6dd91-837">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-837">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-838">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-838">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-839">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-839">'Blazor'</span></span>
- <span data-ttu-id="6dd91-840">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-840">'Identity'</span></span>
- <span data-ttu-id="6dd91-841">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-841">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-842">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-842">'Razor'</span></span>
- <span data-ttu-id="6dd91-843">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-843">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-844">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-844">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-845">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-845">'Blazor'</span></span>
- <span data-ttu-id="6dd91-846">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-846">'Identity'</span></span>
- <span data-ttu-id="6dd91-847">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-847">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-848">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-848">'Razor'</span></span>
- <span data-ttu-id="6dd91-849">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-849">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-850">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-850">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-851">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-851">'Blazor'</span></span>
- <span data-ttu-id="6dd91-852">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-852">'Identity'</span></span>
- <span data-ttu-id="6dd91-853">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-853">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-854">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-854">'Razor'</span></span>
- <span data-ttu-id="6dd91-855">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-855">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-856">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-856">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-857">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-857">'Blazor'</span></span>
- <span data-ttu-id="6dd91-858">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-858">'Identity'</span></span>
- <span data-ttu-id="6dd91-859">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-859">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-860">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-860">'Razor'</span></span>
- <span data-ttu-id="6dd91-861">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-861">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-862">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-862">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-863">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-863">'Blazor'</span></span>
- <span data-ttu-id="6dd91-864">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-864">'Identity'</span></span>
- <span data-ttu-id="6dd91-865">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-865">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-866">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-866">'Razor'</span></span>
- <span data-ttu-id="6dd91-867">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-867">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-868">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-868">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-869">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-869">'Blazor'</span></span>
- <span data-ttu-id="6dd91-870">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-870">'Identity'</span></span>
- <span data-ttu-id="6dd91-871">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-871">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-872">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-872">'Razor'</span></span>
- <span data-ttu-id="6dd91-873">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-873">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-874">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-874">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-875">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-875">'Blazor'</span></span>
- <span data-ttu-id="6dd91-876">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-876">'Identity'</span></span>
- <span data-ttu-id="6dd91-877">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-877">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-878">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-878">'Razor'</span></span>
- <span data-ttu-id="6dd91-879">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-879">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-880">------------------------------------- | |`hello`                                  | `/hello`               |僅符合單一路徑 `/hello` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-880">------------------------------------- | | `hello`                                  | `/hello`                | Only matches the single path `/hello`.</span></span>                                     <span data-ttu-id="6dd91-881">| |`{Page=Home}`                            | `/`                    |符合並將設定 `Page` 為 `Home` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-881">| | `{Page=Home}`                            | `/`                     | Matches and sets `Page` to `Home`.</span></span>                                         <span data-ttu-id="6dd91-882">| |`{Page=Home}`                            | `/Contact`             |符合並將設定 `Page` 為 `Contact` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-882">| | `{Page=Home}`                            | `/Contact`              | Matches and sets `Page` to `Contact`.</span></span>                                      <span data-ttu-id="6dd91-883">| |`{controller}/{action}/{id?}`            | `/Products/List`       |對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-883">| | `{controller}/{action}/{id?}`            | `/Products/List`        | Maps to the `Products` controller and `List` action.</span></span>                       <span data-ttu-id="6dd91-884">| |`{controller}/{action}/{id?}`            | `/Products/Details/123`|對應至 `Products` 控制器和 `Details` 動作，並 `id` 將設定為123。</span><span class="sxs-lookup"><span data-stu-id="6dd91-884">| | `{controller}/{action}/{id?}`            | `/Products/Details/123` | Maps to the `Products` controller and  `Details` action with`id` set to 123.</span></span> <span data-ttu-id="6dd91-885">| |`{controller=Home}/{action=Index}/{id?}` | `/`                    |對應至 `Home` 控制器和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-885">| | `{controller=Home}/{action=Index}/{id?}` | `/`                     | Maps to the `Home` controller and `Index` method.</span></span> <span data-ttu-id="6dd91-886">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="6dd91-886">`id` is ignored.</span></span>        <span data-ttu-id="6dd91-887">| |`{controller=Home}/{action=Index}/{id?}` | `/Products`        |對應至 `Products` 控制器和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-887">| | `{controller=Home}/{action=Index}/{id?}` | `/Products`         | Maps to the `Products` controller and `Index` method.</span></span> <span data-ttu-id="6dd91-888">`id` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="6dd91-888">`id` is ignored.</span></span>        |

<span data-ttu-id="6dd91-889">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-889">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="6dd91-890">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-890">Constraints and defaults can also be specified outside the route template.</span></span>

### <a name="complex-segments"></a><span data-ttu-id="6dd91-891">複雜區段</span><span class="sxs-lookup"><span data-stu-id="6dd91-891">Complex segments</span></span>

<span data-ttu-id="6dd91-892">複雜區段的處理方式是以[非貪婪](#greedy)的方式，將常值分隔符號從右至左比對。</span><span class="sxs-lookup"><span data-stu-id="6dd91-892">Complex segments are processed by matching up literal delimiters from right to left in a [non-greedy](#greedy) way.</span></span> <span data-ttu-id="6dd91-893">例如， `[Route("/a{b}c{d}")]` 是一個複雜的區段。</span><span class="sxs-lookup"><span data-stu-id="6dd91-893">For example, `[Route("/a{b}c{d}")]` is a complex segment.</span></span>
<span data-ttu-id="6dd91-894">複雜的區段會以特定的方式工作，必須瞭解才能成功使用它們。</span><span class="sxs-lookup"><span data-stu-id="6dd91-894">Complex segments work in a particular way that must be understood to use them successfully.</span></span> <span data-ttu-id="6dd91-895">本章節中的範例將示範當分隔符號文字不會出現在參數值內時，複雜區段的效果為何。</span><span class="sxs-lookup"><span data-stu-id="6dd91-895">The example in this section demonstrates why complex segments only really work well when the delimiter text doesn't appear inside the parameter values.</span></span> <span data-ttu-id="6dd91-896">在較複雜的情況下，需要使用[RegEx](/dotnet/standard/base-types/regular-expressions) ，然後手動將值解壓縮。</span><span class="sxs-lookup"><span data-stu-id="6dd91-896">Using a [regex](/dotnet/standard/base-types/regular-expressions) and then manually extracting the values is needed for more complex cases.</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="6dd91-897">這是路由執行的步驟 `/a{b}c{d}` 和範本和 URL 路徑的摘要 `/abcd` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-897">This is a summary of the steps that routing performs with the template `/a{b}c{d}` and the URL path `/abcd`.</span></span> <span data-ttu-id="6dd91-898">`|`是用來協助視覺化演算法的運作方式：</span><span class="sxs-lookup"><span data-stu-id="6dd91-898">The `|` is used to help visualize how the algorithm works:</span></span>

* <span data-ttu-id="6dd91-899">第一個常值（由右至左）為 `c` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-899">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="6dd91-900">因此， `/abcd` 會向右搜尋並尋找 `/ab|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-900">So `/abcd` is searched from right and finds `/ab|c|d`.</span></span>
* <span data-ttu-id="6dd91-901">右邊的所有專案（ `d` ）現在都符合路由參數 `{d}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-901">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="6dd91-902">下一個常值（由右至左）為 `a` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-902">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="6dd91-903">因此， `/ab|c|d` 在從離開的地方開始搜尋，然後 `a` 找到 `/|a|b|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-903">So `/ab|c|d` is searched starting where we left off, then `a` is found `/|a|b|c|d`.</span></span>
* <span data-ttu-id="6dd91-904">右邊的值（ `b` ）現在符合路由參數 `{b}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-904">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="6dd91-905">沒有任何剩餘的文字，也沒有剩餘的路由範本，因此這是相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-905">There is no remaining text and no remaining route template, so this is a match.</span></span>

<span data-ttu-id="6dd91-906">以下是使用相同範本和 URL 路徑的負面案例範例 `/a{b}c{d}` `/aabcd` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-906">Here's an example of a negative case using the same template `/a{b}c{d}` and the URL path `/aabcd`.</span></span> <span data-ttu-id="6dd91-907">`|`是用來協助視覺化演算法的運作方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-907">The `|` is used to help visualize how the algorithm works.</span></span> <span data-ttu-id="6dd91-908">這種情況並不相符，這會透過相同的演算法來說明：</span><span class="sxs-lookup"><span data-stu-id="6dd91-908">This case isn't a match, which is explained by the same algorithm:</span></span>
* <span data-ttu-id="6dd91-909">第一個常值（由右至左）為 `c` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-909">The first literal, right to left, is `c`.</span></span> <span data-ttu-id="6dd91-910">因此， `/aabcd` 會向右搜尋並尋找 `/aab|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-910">So `/aabcd` is searched from right and finds `/aab|c|d`.</span></span>
* <span data-ttu-id="6dd91-911">右邊的所有專案（ `d` ）現在都符合路由參數 `{d}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-911">Everything to the right (`d`) is now matched to the route parameter `{d}`.</span></span>
* <span data-ttu-id="6dd91-912">下一個常值（由右至左）為 `a` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-912">The next literal, right to left, is `a`.</span></span> <span data-ttu-id="6dd91-913">因此， `/aab|c|d` 在從離開的地方開始搜尋，然後 `a` 找到 `/a|a|b|c|d` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-913">So `/aab|c|d` is searched starting where we left off, then `a` is found `/a|a|b|c|d`.</span></span>
* <span data-ttu-id="6dd91-914">右邊的值（ `b` ）現在符合路由參數 `{b}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-914">The value to the right (`b`) is now matched to the route parameter `{b}`.</span></span>
* <span data-ttu-id="6dd91-915">此時會有剩餘的文字 `a` ，但演算法已用完路由範本進行剖析，因此這不是相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-915">At this point there is remaining text `a`, but the algorithm has run out of route template to parse, so this is not a match.</span></span>

<span data-ttu-id="6dd91-916">因為比對演算法[是非貪婪](#greedy)的：</span><span class="sxs-lookup"><span data-stu-id="6dd91-916">Since the matching algorithm is [non-greedy](#greedy):</span></span>

* <span data-ttu-id="6dd91-917">它會比對每個步驟中可能的最小文字數量。</span><span class="sxs-lookup"><span data-stu-id="6dd91-917">It matches the smallest amount of text possible in each step.</span></span>
* <span data-ttu-id="6dd91-918">在參數值內出現分隔符號值的任何情況下，都會導致不相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-918">Any case where the delimiter value appears inside the parameter values results in not matching.</span></span>

<span data-ttu-id="6dd91-919">正則運算式可讓您更充分掌控其比對行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-919">Regular expressions provide much more control over their matching behavior.</span></span>

<a name="greedy"></a>

<span data-ttu-id="6dd91-920">貪婪比對（也稱為[延遲](https://wikipedia.org/wiki/Regular_expression#Lazy_matching)比對）符合最大的可能字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-920">Greedy matching, also know as [lazy matching](https://wikipedia.org/wiki/Regular_expression#Lazy_matching), matches the largest possible string.</span></span> <span data-ttu-id="6dd91-921">非貪婪的會符合最小的可能字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-921">Non-greedy matches the smallest possible string.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="6dd91-922">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-922">Route constraint reference</span></span>

<span data-ttu-id="6dd91-923">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="6dd91-923">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="6dd91-924">路由條件約束通常會檢查透過路由範本相關聯的路由值，並對值是否可接受進行 true 或 false 的決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-924">Route constraints generally inspect the route value associated via the route template and make a true or false decision about whether the value is acceptable.</span></span> <span data-ttu-id="6dd91-925">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-925">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="6dd91-926">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-926">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="6dd91-927">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-927">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd91-928">請勿針對輸入驗證使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-928">Don't use constraints for input validation.</span></span> <span data-ttu-id="6dd91-929">如果條件約束用於輸入驗證，則不正確輸入會導致找不到的 `404` 回應。</span><span class="sxs-lookup"><span data-stu-id="6dd91-929">If constraints are used for input validation, invalid input results in a `404` Not Found response.</span></span> <span data-ttu-id="6dd91-930">不正確輸入應該會產生不 `400` 正確的要求，並出現適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="6dd91-930">Invalid input should produce a `400` Bad Request with an appropriate error message.</span></span> <span data-ttu-id="6dd91-931">路由條件約束會用來釐清類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="6dd91-931">Route constraints are used to disambiguate similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="6dd91-932">下表示范範例路由條件約束及其預期行為：</span><span class="sxs-lookup"><span data-stu-id="6dd91-932">The following table demonstrates example route constraints and their expected behavior:</span></span>

| <span data-ttu-id="6dd91-933">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="6dd91-933">constraint</span></span> | <span data-ttu-id="6dd91-934">範例</span><span class="sxs-lookup"><span data-stu-id="6dd91-934">Example</span></span> | <span data-ttu-id="6dd91-935">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="6dd91-935">Example Matches</span></span> | <span data-ttu-id="6dd91-936">備忘錄</span><span class="sxs-lookup"><span data-stu-id="6dd91-936">Notes</span></span> |
| ---
<span data-ttu-id="6dd91-937">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-937">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-938">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-938">'Blazor'</span></span>
- <span data-ttu-id="6dd91-939">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-939">'Identity'</span></span>
- <span data-ttu-id="6dd91-940">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-940">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-941">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-941">'Razor'</span></span>
- <span data-ttu-id="6dd91-942">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-942">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-943">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-943">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-944">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-944">'Blazor'</span></span>
- <span data-ttu-id="6dd91-945">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-945">'Identity'</span></span>
- <span data-ttu-id="6dd91-946">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-946">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-947">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-947">'Razor'</span></span>
- <span data-ttu-id="6dd91-948">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-948">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-949">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-949">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-950">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-950">'Blazor'</span></span>
- <span data-ttu-id="6dd91-951">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-951">'Identity'</span></span>
- <span data-ttu-id="6dd91-952">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-952">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-953">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-953">'Razor'</span></span>
- <span data-ttu-id="6dd91-954">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-954">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-955">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-955">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-956">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-956">'Blazor'</span></span>
- <span data-ttu-id="6dd91-957">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-957">'Identity'</span></span>
- <span data-ttu-id="6dd91-958">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-958">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-959">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-959">'Razor'</span></span>
- <span data-ttu-id="6dd91-960">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-960">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-961">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-961">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-962">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-962">'Blazor'</span></span>
- <span data-ttu-id="6dd91-963">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-963">'Identity'</span></span>
- <span data-ttu-id="6dd91-964">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-964">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-965">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-965">'Razor'</span></span>
- <span data-ttu-id="6dd91-966">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-966">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-967">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-967">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-968">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-968">'Blazor'</span></span>
- <span data-ttu-id="6dd91-969">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-969">'Identity'</span></span>
- <span data-ttu-id="6dd91-970">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-970">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-971">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-971">'Razor'</span></span>
- <span data-ttu-id="6dd91-972">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-972">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-973">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-973">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-974">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-974">'Blazor'</span></span>
- <span data-ttu-id="6dd91-975">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-975">'Identity'</span></span>
- <span data-ttu-id="6dd91-976">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-976">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-977">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-977">'Razor'</span></span>
- <span data-ttu-id="6dd91-978">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-978">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-979">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-979">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-980">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-980">'Blazor'</span></span>
- <span data-ttu-id="6dd91-981">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-981">'Identity'</span></span>
- <span data-ttu-id="6dd91-982">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-982">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-983">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-983">'Razor'</span></span>
- <span data-ttu-id="6dd91-984">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-984">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-985">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-985">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-986">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-986">'Blazor'</span></span>
- <span data-ttu-id="6dd91-987">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-987">'Identity'</span></span>
- <span data-ttu-id="6dd91-988">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-988">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-989">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-989">'Razor'</span></span>
- <span data-ttu-id="6dd91-990">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-990">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-991">-------- |----- | |`int` | `{id:int}` | `123456789`, `-123456789` |符合任何整數 | |`bool` | `{active:bool}` | `true`, `FALSE` |符合 `true` 或 `false` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-991">-------- | ----- | | `int` | `{id:int}` | `123456789`, `-123456789` | Matches any integer | | `bool` | `{active:bool}` | `true`, `FALSE` | Matches `true` or `false`.</span></span> <span data-ttu-id="6dd91-992">不區分大小寫 | |`datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` |符合不因文化特性而異的有效 `DateTime` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-992">Case-insensitive | | `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-993">請參閱先前的警告。</span><span class="sxs-lookup"><span data-stu-id="6dd91-993">See preceding warning.</span></span> <span data-ttu-id="6dd91-994">| |`decimal` | `{price:decimal}` | `49.99`, `-1,000.01` |符合不因文化特性而異的有效 `decimal` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-994">| | `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-995">請參閱先前的警告。 ||`double` | `{weight:double}` | `1.234`, `-1,001.01e8` |符合不因文化特性而異的有效 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-995">See preceding warning.| | `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-996">請參閱先前的警告。 ||`float` | `{weight:float}` | `1.234`, `-1,001.01e8` |符合不因文化特性而異的有效 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-996">See preceding warning.| | `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-997">請參閱先前的警告。 ||`guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`|符合有效的 `Guid` 值 | | `long`  |  `{ticks:long}`  |  `123456789` 、 `-123456789` |符合有效的 `long` 值 | | `minlength(value)`  |  `{username:minlength(4)}`  |  `Rick` |字串必須至少有4個字元 | |`maxlength(value)` | `{filename:maxlength(8)}` | `MyFile`|字串不能超過8個字元 | |`length(length)` | `{filename:length(12)}` | `somefile.txt`|字串的長度必須剛好是12個字元 | |`length(min,max)` | `{filename:length(8,16)}` | `somefile.txt`|字串必須至少為8，且長度不能超過16個字元 | |`min(value)` | `{age:min(18)}` | `19`|整數值必須至少為 18 | |`max(value)` | `{age:max(120)}` | `91`|整數值不能超過 120 | |`range(min,max)` | `{age:range(18,120)}` | `91`|整數值必須至少為18，但不能超過 120 | |`alpha` | `{name:alpha}` | `Rick`|字串必須包含一或多個字母字元， `a` - `z` 並不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-997">See preceding warning.| | `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638` | Matches a valid `Guid` value | | `long` | `{ticks:long}` | `123456789`, `-123456789` | Matches a valid `long` value | | `minlength(value)` | `{username:minlength(4)}` | `Rick` | String must be at least 4 characters | | `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | String must be no more than 8 characters | | `length(length)` | `{filename:length(12)}` | `somefile.txt` | String must be exactly 12 characters long | | `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | String must be at least 8 and no more than 16 characters long | | `min(value)` | `{age:min(18)}` | `19` | Integer value must be at least 18 | | `max(value)` | `{age:max(120)}` | `91` | Integer value must be no more than 120 | | `range(min,max)` | `{age:range(18,120)}` | `91` | Integer value must be at least 18 but no more than 120 | | `alpha` | `{name:alpha}` | `Rick` | String must consist of one or more alphabetical characters, `a`-`z` and case-insensitive.</span></span> <span data-ttu-id="6dd91-998">| |`regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789`|字串必須符合正則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-998">| | `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | String must match the regular expression.</span></span> <span data-ttu-id="6dd91-999">請參閱定義正則運算式的秘訣。</span><span class="sxs-lookup"><span data-stu-id="6dd91-999">See tips about defining a regular expression.</span></span> <span data-ttu-id="6dd91-1000">| |`required` | `{name:required}` | `Rick`|用來強制在 URL 產生期間出現非參數值 |</span><span class="sxs-lookup"><span data-stu-id="6dd91-1000">| | `required` | `{name:required}` | `Rick` | Used to enforce that a non-parameter value is present during URL generation |</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="6dd91-1001">多個以冒號分隔的條件約束可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1001">Multiple, colon delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="6dd91-1002">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1002">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="6dd91-1003">驗證 URL 並轉換成 CLR 類型的路由條件約束，一律會使用不因文化特性而異。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1003">Route constraints that verify the URL and are converted to a CLR type always use the invariant culture.</span></span> <span data-ttu-id="6dd91-1004">例如，轉換成 CLR 型別 `int` 或 `DateTime` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1004">For example, conversion to the CLR type `int` or `DateTime`.</span></span> <span data-ttu-id="6dd91-1005">這些條件約束假設 URL 無法當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1005">These constraints assume that the URL is not localizable.</span></span> <span data-ttu-id="6dd91-1006">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1006">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="6dd91-1007">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1007">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="6dd91-1008">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1008">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

### <a name="regular-expressions-in-constraints"></a><span data-ttu-id="6dd91-1009">條件約束中的正則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-1009">Regular expressions in constraints</span></span>

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="6dd91-1010">正則運算式可以指定為使用路由條件約束的內嵌條件約束 `regex(...)` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1010">Regular expressions can be specified as inline constraints using the `regex(...)` route constraint.</span></span> <span data-ttu-id="6dd91-1011">系列中的方法 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> 也會接受條件約束的物件常值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1011">Methods in the <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> family also accept an object literal of constraints.</span></span> <span data-ttu-id="6dd91-1012">如果使用該表單，字串值就會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1012">If that form is used, string values are interpreted as regular expressions.</span></span>

<span data-ttu-id="6dd91-1013">下列程式碼會使用內嵌 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1013">The following code uses an inline regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex.cs?name=snippet)]

<span data-ttu-id="6dd91-1014">下列程式碼會使用物件常值來指定 RegEx 條件約束：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1014">The following code uses an object literal to specify a regex constraint:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRegex2.cs?name=snippet)]

<span data-ttu-id="6dd91-1015">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1015">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="6dd91-1016">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1016">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="6dd91-1017">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1017">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="6dd91-1018">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1018">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="6dd91-1019">若要 `^\d{3}-\d{2}-\d{4}$` 在內嵌條件約束中使用正則運算式，請使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1019">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in an inline constraint, use one of the following:</span></span>

* <span data-ttu-id="6dd91-1020">將 `\` 字串中提供的字元取代為 c # 原始檔中的字元，以便 `\\` 將 `\` 字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1020">Replace `\` characters provided in the string as `\\` characters in the C# source file in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="6dd91-1021">[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1021">[Verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="6dd91-1022">若要轉義路由參數分隔符號 `{` 、 `}` 、 `[` 、，請將 `]` 運算式中的字元加倍，例如、 `{{` 、 `}}` `[[` 、 `]]` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1022">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression, for example, `{{`, `}}`, `[[`, `]]`.</span></span> <span data-ttu-id="6dd91-1023">下表顯示正則運算式和其轉義版本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1023">The following table shows a regular expression and its escaped version:</span></span>

| <span data-ttu-id="6dd91-1024">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-1024">Regular expression</span></span>    | <span data-ttu-id="6dd91-1025">已轉義的正則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-1025">Escaped regular expression</span></span>     |
| ---
<span data-ttu-id="6dd91-1026">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1026">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1027">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1027">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1028">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1028">'Identity'</span></span>
- <span data-ttu-id="6dd91-1029">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1029">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1030">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1030">'Razor'</span></span>
- <span data-ttu-id="6dd91-1031">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1031">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1032">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1032">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1033">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1033">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1034">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1034">'Identity'</span></span>
- <span data-ttu-id="6dd91-1035">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1035">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1036">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1036">'Razor'</span></span>
- <span data-ttu-id="6dd91-1037">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1037">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1038">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1038">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1039">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1039">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1040">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1040">'Identity'</span></span>
- <span data-ttu-id="6dd91-1041">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1041">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1042">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1042">'Razor'</span></span>
- <span data-ttu-id="6dd91-1043">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1043">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1044">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1044">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1045">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1045">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1046">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1046">'Identity'</span></span>
- <span data-ttu-id="6dd91-1047">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1047">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1048">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1048">'Razor'</span></span>
- <span data-ttu-id="6dd91-1049">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1049">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1050">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1050">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1051">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1051">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1052">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1052">'Identity'</span></span>
- <span data-ttu-id="6dd91-1053">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1053">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1054">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1054">'Razor'</span></span>
- <span data-ttu-id="6dd91-1055">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1055">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1056">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1056">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1057">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1057">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1058">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1058">'Identity'</span></span>
- <span data-ttu-id="6dd91-1059">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1059">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1060">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1060">'Razor'</span></span>
- <span data-ttu-id="6dd91-1061">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1061">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1062">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1062">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1063">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1063">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1064">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1064">'Identity'</span></span>
- <span data-ttu-id="6dd91-1065">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1065">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1066">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1066">'Razor'</span></span>
- <span data-ttu-id="6dd91-1067">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1067">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1068">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1068">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1069">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1069">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1070">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1070">'Identity'</span></span>
- <span data-ttu-id="6dd91-1071">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1071">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1072">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1072">'Razor'</span></span>
- <span data-ttu-id="6dd91-1073">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1073">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1074">----------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1074">----------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1075">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1075">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1076">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1076">'Identity'</span></span>
- <span data-ttu-id="6dd91-1077">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1077">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1078">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1078">'Razor'</span></span>
- <span data-ttu-id="6dd91-1079">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1079">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1080">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1080">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1081">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1081">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1082">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1082">'Identity'</span></span>
- <span data-ttu-id="6dd91-1083">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1083">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1084">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1084">'Razor'</span></span>
- <span data-ttu-id="6dd91-1085">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1085">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1086">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1086">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1087">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1087">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1088">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1088">'Identity'</span></span>
- <span data-ttu-id="6dd91-1089">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1089">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1090">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1090">'Razor'</span></span>
- <span data-ttu-id="6dd91-1091">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1091">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1092">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1092">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1093">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1093">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1094">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1094">'Identity'</span></span>
- <span data-ttu-id="6dd91-1095">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1095">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1096">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1096">'Razor'</span></span>
- <span data-ttu-id="6dd91-1097">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1097">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1098">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1098">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1099">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1099">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1100">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1100">'Identity'</span></span>
- <span data-ttu-id="6dd91-1101">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1101">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1102">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1102">'Razor'</span></span>
- <span data-ttu-id="6dd91-1103">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1103">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1104">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1104">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1105">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1105">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1106">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1106">'Identity'</span></span>
- <span data-ttu-id="6dd91-1107">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1107">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1108">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1108">'Razor'</span></span>
- <span data-ttu-id="6dd91-1109">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1109">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1110">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1110">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1111">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1111">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1112">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1112">'Identity'</span></span>
- <span data-ttu-id="6dd91-1113">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1113">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1114">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1114">'Razor'</span></span>
- <span data-ttu-id="6dd91-1115">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1115">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1116">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1116">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1117">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1117">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1118">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1118">'Identity'</span></span>
- <span data-ttu-id="6dd91-1119">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1119">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1120">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1120">'Razor'</span></span>
- <span data-ttu-id="6dd91-1121">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1121">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1122">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1122">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1123">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1123">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1124">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1124">'Identity'</span></span>
- <span data-ttu-id="6dd91-1125">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1125">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1126">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1126">'Razor'</span></span>
- <span data-ttu-id="6dd91-1127">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1127">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1128">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1128">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1129">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1129">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1130">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1130">'Identity'</span></span>
- <span data-ttu-id="6dd91-1131">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1131">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1132">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1132">'Razor'</span></span>
- <span data-ttu-id="6dd91-1133">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1133">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1134">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1134">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1135">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1135">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1136">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1136">'Identity'</span></span>
- <span data-ttu-id="6dd91-1137">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1137">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1138">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1138">'Razor'</span></span>
- <span data-ttu-id="6dd91-1139">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1139">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1140">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1140">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1141">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1141">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1142">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1142">'Identity'</span></span>
- <span data-ttu-id="6dd91-1143">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1143">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1144">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1144">'Razor'</span></span>
- <span data-ttu-id="6dd91-1145">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1145">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1146">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1146">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1147">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1147">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1148">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1148">'Identity'</span></span>
- <span data-ttu-id="6dd91-1149">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1149">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1150">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1150">'Razor'</span></span>
- <span data-ttu-id="6dd91-1151">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1151">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1152">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span><span class="sxs-lookup"><span data-stu-id="6dd91-1152">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span></span>

<span data-ttu-id="6dd91-1153">路由中使用的正則運算式通常以 `^` 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1153">Regular expressions used in routing often start with the `^` character and match the starting position of the string.</span></span> <span data-ttu-id="6dd91-1154">運算式通常以 `$` 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1154">The expressions often end with the `$` character and match the end of the string.</span></span> <span data-ttu-id="6dd91-1155">`^`和 `$` 字元可確保正則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1155">The `^` and `$` characters ensure that the regular expression matches the entire route parameter value.</span></span> <span data-ttu-id="6dd91-1156">如果沒有 `^` 和 `$` 字元，正則運算式會比對字串內的任何子字串，這通常是不需要的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1156">Without the `^` and `$` characters, the regular expression matches any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="6dd91-1157">下表提供範例，並說明它們符合或無法符合的原因：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1157">The following table provides examples and explains why they match or fail to match:</span></span>

| <span data-ttu-id="6dd91-1158">運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-1158">Expression</span></span>   | <span data-ttu-id="6dd91-1159">String</span><span class="sxs-lookup"><span data-stu-id="6dd91-1159">String</span></span>    | <span data-ttu-id="6dd91-1160">比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-1160">Match</span></span> | <span data-ttu-id="6dd91-1161">註解</span><span class="sxs-lookup"><span data-stu-id="6dd91-1161">Comment</span></span>               |
| ---
<span data-ttu-id="6dd91-1162">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1162">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1163">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1163">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1164">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1164">'Identity'</span></span>
- <span data-ttu-id="6dd91-1165">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1165">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1166">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1166">'Razor'</span></span>
- <span data-ttu-id="6dd91-1167">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1167">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1168">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1168">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1169">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1170">'Identity'</span></span>
- <span data-ttu-id="6dd91-1171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1171">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1172">'Razor'</span></span>
- <span data-ttu-id="6dd91-1173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1174">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1174">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1175">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1175">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1176">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1176">'Identity'</span></span>
- <span data-ttu-id="6dd91-1177">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1177">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1178">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1178">'Razor'</span></span>
- <span data-ttu-id="6dd91-1179">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1179">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1180">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1180">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1181">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1181">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1182">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1182">'Identity'</span></span>
- <span data-ttu-id="6dd91-1183">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1183">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1184">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1184">'Razor'</span></span>
- <span data-ttu-id="6dd91-1185">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1185">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1186">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1186">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1187">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1187">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1188">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1188">'Identity'</span></span>
- <span data-ttu-id="6dd91-1189">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1189">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1190">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1190">'Razor'</span></span>
- <span data-ttu-id="6dd91-1191">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1191">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1192">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1192">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1193">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1193">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1194">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1194">'Identity'</span></span>
- <span data-ttu-id="6dd91-1195">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1195">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1196">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1196">'Razor'</span></span>
- <span data-ttu-id="6dd91-1197">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1197">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1198">----- |:---: | ---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1198">----- | :---: |  --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1199">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1199">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1200">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1200">'Identity'</span></span>
- <span data-ttu-id="6dd91-1201">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1201">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1202">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1202">'Razor'</span></span>
- <span data-ttu-id="6dd91-1203">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1203">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1204">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1204">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1205">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1205">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1206">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1206">'Identity'</span></span>
- <span data-ttu-id="6dd91-1207">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1207">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1208">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1208">'Razor'</span></span>
- <span data-ttu-id="6dd91-1209">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1209">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1210">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1210">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1211">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1211">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1212">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1212">'Identity'</span></span>
- <span data-ttu-id="6dd91-1213">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1213">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1214">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1214">'Razor'</span></span>
- <span data-ttu-id="6dd91-1215">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1215">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1216">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1216">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1217">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1217">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1218">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1218">'Identity'</span></span>
- <span data-ttu-id="6dd91-1219">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1219">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1220">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1220">'Razor'</span></span>
- <span data-ttu-id="6dd91-1221">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1221">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1222">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1222">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1223">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1223">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1224">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1224">'Identity'</span></span>
- <span data-ttu-id="6dd91-1225">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1225">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1226">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1226">'Razor'</span></span>
- <span data-ttu-id="6dd91-1227">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1227">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1228">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1228">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1229">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1229">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1230">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1230">'Identity'</span></span>
- <span data-ttu-id="6dd91-1231">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1231">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1232">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1232">'Razor'</span></span>
- <span data-ttu-id="6dd91-1233">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1233">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1234">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1234">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1235">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1235">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1236">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1236">'Identity'</span></span>
- <span data-ttu-id="6dd91-1237">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1237">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1238">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1238">'Razor'</span></span>
- <span data-ttu-id="6dd91-1239">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1239">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1240">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1240">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1241">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1242">'Identity'</span></span>
- <span data-ttu-id="6dd91-1243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1243">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1244">'Razor'</span></span>
- <span data-ttu-id="6dd91-1245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1245">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1246">---------- || `[a-z]{2}`   |您好 |是 |子字串符合 || `[a-z]{2}`   |123abc456 |是 |子字串符合 || `[a-z]{2}`   |mz |是 |符合運算式 || `[a-z]{2}`   |MZ |是 |不區分大小寫 || `^[a-z]{2}$` |您好 |否 |查看 `^` 和更新版本 `$` | | `^[a-z]{2}$` | 123abc456 |否 |請參閱和更新版本 `^` `$` |</span><span class="sxs-lookup"><span data-stu-id="6dd91-1246">---------- | | `[a-z]{2}`   | hello     | Yes   | Substring matches     | | `[a-z]{2}`   | 123abc456 | Yes   | Substring matches     | | `[a-z]{2}`   | mz        | Yes   | Matches expression    | | `[a-z]{2}`   | MZ        | Yes   | Not case sensitive    | | `^[a-z]{2}$` | hello     | No    | See `^` and `$` above | | `^[a-z]{2}$` | 123abc456 | No    | See `^` and `$` above |</span></span>

<span data-ttu-id="6dd91-1247">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1247">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="6dd91-1248">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1248">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="6dd91-1249">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1249">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="6dd91-1250">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1250">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="6dd91-1251">在條件約束字典中傳遞並不符合其中一個已知條件約束的條件約束，也會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1251">Constraints that are passed in the constraints dictionary that don't match one of the known constraints are also treated as regular expressions.</span></span> <span data-ttu-id="6dd91-1252">在不符合其中一個已知條件約束的範本內傳遞的條件約束，不會被視為正則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1252">Constraints that are passed  within a template that don't match one of the known constraints are not treated as regular expressions.</span></span>

### <a name="custom-route-constraints"></a><span data-ttu-id="6dd91-1253">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="6dd91-1253">Custom route constraints</span></span>

<span data-ttu-id="6dd91-1254">您可以藉由執行介面來建立自訂路由條件約束 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1254">Custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="6dd91-1255">`IRouteConstraint`介面包含 <xref:System.Web.Routing.IRouteConstraint.Match*> ， `true` 如果條件約束已滿足，則會傳回，否則會傳回 `false` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1255">The `IRouteConstraint` interface contains <xref:System.Web.Routing.IRouteConstraint.Match*>, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="6dd91-1256">很少需要自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1256">Custom route constraints are rarely needed.</span></span> <span data-ttu-id="6dd91-1257">在執行自訂路由條件約束之前，請考慮使用替代專案，例如模型系結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1257">Before implementing a custom route constraint, consider alternatives, such as model binding.</span></span>

<span data-ttu-id="6dd91-1258">[ASP.NET Core[條件約束](https://github.com/dotnet/aspnetcore/tree/master/src/Http/Routing/src/Constraints)] 資料夾會提供建立條件約束的良好範例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1258">The ASP.NET Core [Constraints](https://github.com/dotnet/aspnetcore/tree/master/src/Http/Routing/src/Constraints) folder provides good examples of creating a constraints.</span></span> <span data-ttu-id="6dd91-1259">例如， [GuidRouteConstraint](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Constraints/GuidRouteConstraint.cs#L18)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1259">For example, [GuidRouteConstraint](https://github.com/dotnet/aspnetcore/blob/master/src/Http/Routing/src/Constraints/GuidRouteConstraint.cs#L18).</span></span>

<span data-ttu-id="6dd91-1260">若要使用自訂 `IRouteConstraint` ，則必須向 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 服務容器中的應用程式註冊路由條件約束類型。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1260">To use a custom `IRouteConstraint`, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the service container.</span></span> <span data-ttu-id="6dd91-1261">`ConstraintMap` 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 `IRouteConstraint` 實作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1261">A `ConstraintMap` is a dictionary that maps route constraint keys to `IRouteConstraint` implementations that validate those constraints.</span></span> <span data-ttu-id="6dd91-1262">更新應用程式的 `ConstraintMap` 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1262">An app's `ConstraintMap` can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="6dd91-1263">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1263">For example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet)]

<span data-ttu-id="6dd91-1264">上述條件約束會套用至下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1264">The preceding constraint is applied in the following code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet&highlight=6,13)]

[!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

<span data-ttu-id="6dd91-1265">的執行 `MyCustomConstraint` 會防止套用 `0` 至路由參數：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1265">The implementation of `MyCustomConstraint` prevents `0` being applied to a route parameter:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

<span data-ttu-id="6dd91-1266">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1266">The preceding code:</span></span>

* <span data-ttu-id="6dd91-1267">防止 `0` 在 `{id}` 路由的區段中。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1267">Prevents `0` in the `{id}` segment of the route.</span></span>
* <span data-ttu-id="6dd91-1268">會顯示以提供執行自訂條件約束的基本範例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1268">Is shown to provide a basic example of implementing a custom constraint.</span></span> <span data-ttu-id="6dd91-1269">不應在生產應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1269">It should not be used in a production app.</span></span>

<span data-ttu-id="6dd91-1270">下列程式碼是避免 `id` 處理包含之的較佳方法 `0` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1270">The following code is a better approach to preventing an `id` containing a `0` from being processed:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="6dd91-1271">上述程式碼在此方法上具有下列優點 `MyCustomConstraint` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1271">The preceding code has the following advantages over the `MyCustomConstraint` approach:</span></span>

* <span data-ttu-id="6dd91-1272">不需要自訂條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1272">It doesn't require a custom constraint.</span></span>
* <span data-ttu-id="6dd91-1273">當路由參數包含時，它會傳回更具描述性的錯誤 `0` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1273">It returns a more descriptive error when the route parameter includes `0`.</span></span>

## <a name="parameter-transformer-reference"></a><span data-ttu-id="6dd91-1274">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-1274">Parameter transformer reference</span></span>

<span data-ttu-id="6dd91-1275">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1275">Parameter transformers:</span></span>

* <span data-ttu-id="6dd91-1276">使用產生連結時執行 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1276">Execute when generating a link using <xref:Microsoft.AspNetCore.Routing.LinkGenerator>.</span></span>
* <span data-ttu-id="6dd91-1277">實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1277">Implement <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer?displayProperty=fullName>.</span></span>
* <span data-ttu-id="6dd91-1278">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1278">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="6dd91-1279">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1279">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="6dd91-1280">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1280">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="6dd91-1281">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1281">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="6dd91-1282">請考慮下列執行 `IOutboundParameterTransformer` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1282">Consider the following `IOutboundParameterTransformer` implementation:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet2)]

<span data-ttu-id="6dd91-1283">若要在路由模式中使用參數轉換器，請使用中的來設定它 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1283">To use a parameter transformer in a route pattern, configure it using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupConstraint2.cs?name=snippet)]

<span data-ttu-id="6dd91-1284">ASP.NET Core framework 會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1284">The ASP.NET Core framework uses parameter transformers to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="6dd91-1285">例如，參數轉換器會轉換用來比對 `area` 、、和的路由值 `controller` `action` `page` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1285">For example, parameter transformers transform the route values used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapControllerRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="6dd91-1286">使用上述路由範本，動作 `SubscriptionManagementController.GetAll` 會與 URI 相符 `/subscription-management/get-all` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1286">With the preceding route template, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="6dd91-1287">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1287">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="6dd91-1288">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1288">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="6dd91-1289">ASP.NET Core 提供使用參數轉換器搭配產生的路由的 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1289">ASP.NET Core provides API conventions for using parameter transformers with generated routes:</span></span>

* <span data-ttu-id="6dd91-1290"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName>MVC 慣例會將指定的參數轉換器套用至應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1290">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention?displayProperty=fullName> MVC convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="6dd91-1291">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1291">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="6dd91-1292">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1292">For more information, see [Use a parameter transformer to customize token replacement](xref:mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* Razor<span data-ttu-id="6dd91-1293">頁面會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API 慣例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1293"> Pages uses the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention> API convention.</span></span> <span data-ttu-id="6dd91-1294">此慣例會將指定的參數轉換器套用至所有自動探索的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1294">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="6dd91-1295">參數轉換器會轉換頁面路由的資料夾和檔案名區段 Razor 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1295">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="6dd91-1296">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1296">For more information, see [Use a parameter transformer to customize page routes](xref:razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

<a name="ugr"></a>

## <a name="url-generation-reference"></a><span data-ttu-id="6dd91-1297">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-1297">URL generation reference</span></span>

<span data-ttu-id="6dd91-1298">此章節包含 URL 產生所實作為演算法的參考。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1298">This section contains a reference for the algorithm implemented by URL generation.</span></span> <span data-ttu-id="6dd91-1299">實際上，最複雜的 URL 產生範例會使用控制器或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1299">In practice, most complex examples of URL generation use controllers or Razor Pages.</span></span> <span data-ttu-id="6dd91-1300">如需其他資訊，請參閱[控制器中的路由](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1300">See  [routing in controllers](xref:mvc/controllers/routing) for additional information.</span></span>

<span data-ttu-id="6dd91-1301">URL 產生進程會從呼叫[LinkGenerator. GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*)或類似的方法開始。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1301">The URL generation process begins with a call to [LinkGenerator.GetPathByAddress](xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*) or a similar method.</span></span> <span data-ttu-id="6dd91-1302">提供的方法包含位址、一組路由值，以及來自的目前要求的選擇性資訊 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1302">The method is provided with an address, a set of route values, and optionally information about the current request from `HttpContext`.</span></span>

<span data-ttu-id="6dd91-1303">第一個步驟是使用位址來解析一組候選端點，並使用 [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) 符合網址類別型的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1303">The first step is to use the address to resolve a set of candidate endpoints using an [`IEndpointAddressScheme<TAddress>`](xref:Microsoft.AspNetCore.Routing.IEndpointAddressScheme`1) that matches the address's type.</span></span>

<span data-ttu-id="6dd91-1304">位址配置找到一組候選項目之後，就會反復排序並處理端點，直到 URL 產生作業成功為止。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1304">Once of set of candidates is found by the address scheme, the endpoints are ordered and processed iteratively until a URL generation operation succeeds.</span></span> <span data-ttu-id="6dd91-1305">URL**產生不會檢查是否**有多義性，傳回的第一個結果是最終的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1305">URL generation does **not** check for ambiguities, the first result returned is the final result.</span></span>

### <a name="troubleshooting-url-generation-with-logging"></a><span data-ttu-id="6dd91-1306">使用記錄進行 URL 產生的疑難排解</span><span class="sxs-lookup"><span data-stu-id="6dd91-1306">Troubleshooting URL generation with logging</span></span>

<span data-ttu-id="6dd91-1307">針對 URL 產生進行疑難排解的第一個步驟是將的記錄層級設定 `Microsoft.AspNetCore.Routing` 為 `TRACE` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1307">The first step in troubleshooting URL generation is setting the logging level of `Microsoft.AspNetCore.Routing` to `TRACE`.</span></span> <span data-ttu-id="6dd91-1308">`LinkGenerator`記錄其處理的許多詳細資料，這有助於疑難排解問題。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1308">`LinkGenerator` logs many details about its processing which can be useful to troubleshoot problems.</span></span>

<span data-ttu-id="6dd91-1309">如需 URL 產生的詳細資訊，請參閱[url 產生參考](#ugr)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1309">See [URL generation reference](#ugr) for details on URL generation.</span></span>

### <a name="addresses"></a><span data-ttu-id="6dd91-1310">地址</span><span class="sxs-lookup"><span data-stu-id="6dd91-1310">Addresses</span></span>

<span data-ttu-id="6dd91-1311">位址是 URL 產生的概念，用來將連結產生器的呼叫系結至一組候選端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1311">Addresses are the concept in URL generation used to bind a call into the link generator to a set of candidate endpoints.</span></span>

<span data-ttu-id="6dd91-1312">位址是一種可延伸的概念，預設會隨附兩個執行：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1312">Addresses are an extensible concept that come with two implementations by default:</span></span>

* <span data-ttu-id="6dd91-1313">使用*端點名稱*（ `string` ）作為位址：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1313">Using *endpoint name* (`string`) as the address:</span></span>
    * <span data-ttu-id="6dd91-1314">為 MVC 的路由名稱提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1314">Provides similar functionality to MVC's route name.</span></span>
    * <span data-ttu-id="6dd91-1315">使用 <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> 元資料類型。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1315">Uses the <xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata> metadata type.</span></span>
    * <span data-ttu-id="6dd91-1316">針對所有已註冊端點的中繼資料，解析提供的字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1316">Resolves the provided string against the metadata of all registered endpoints.</span></span>
    * <span data-ttu-id="6dd91-1317">如果多個端點使用相同的名稱，則會在啟動時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1317">Throws an exception on startup if multiple endpoints use the same name.</span></span>
    * <span data-ttu-id="6dd91-1318">建議用於在控制器和頁面以外的一般用途 Razor 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1318">Recommended for general-purpose use outside of controllers and Razor Pages.</span></span>
* <span data-ttu-id="6dd91-1319">使用*路由值*（ <xref:Microsoft.AspNetCore.Routing.RouteValuesAddress> ）做為位址：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1319">Using *route values* (<xref:Microsoft.AspNetCore.Routing.RouteValuesAddress>) as the address:</span></span>
    * <span data-ttu-id="6dd91-1320">提供類似的功能來進行控制器和 Razor 分頁的舊版 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1320">Provides similar functionality to controllers and Razor Pages legacy URL generation.</span></span>
    * <span data-ttu-id="6dd91-1321">擴充和調試非常複雜。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1321">Very complex to extend and debug.</span></span>
    * <span data-ttu-id="6dd91-1322">提供所使用的實作為 `IUrlHelper` 、標記協助程式、HTML helper、動作結果等。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1322">Provides the implementation used by `IUrlHelper`, Tag Helpers, HTML Helpers, Action Results, etc.</span></span>

<span data-ttu-id="6dd91-1323">位址配置的角色是讓位址和相符端點之間的關聯依照任意準則：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1323">The role of the address scheme is to make the association between the address and matching endpoints by arbitrary criteria:</span></span>

* <span data-ttu-id="6dd91-1324">端點名稱配置會執行基本的字典查閱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1324">The endpoint name scheme performs a basic dictionary lookup.</span></span>
* <span data-ttu-id="6dd91-1325">路由值配置具有集合演算法的複雜最佳子集。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1325">The route values scheme has a complex best subset of set algorithm.</span></span>

<a name="ambient"></a>

### <a name="ambient-values-and-explicit-values"></a><span data-ttu-id="6dd91-1326">環境值和明確的值</span><span class="sxs-lookup"><span data-stu-id="6dd91-1326">Ambient values and explicit values</span></span>

<span data-ttu-id="6dd91-1327">根據目前的要求，路由會存取目前要求的路由值 `HttpContext.Request.RouteValues` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1327">From the current request, routing accesses the route values of the current request `HttpContext.Request.RouteValues`.</span></span> <span data-ttu-id="6dd91-1328">與目前要求相關聯的值稱為「**環境值**」。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1328">The values associated with the current request are referred to as the **ambient values**.</span></span> <span data-ttu-id="6dd91-1329">為了清楚起見，檔是指傳遞至方法做為**明確值**的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1329">For the purpose of clarity, the documentation refers to the route values passed in to methods as **explicit values**.</span></span>

<span data-ttu-id="6dd91-1330">下列範例會顯示環境值和明確的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1330">The following example shows ambient values and explicit values.</span></span> <span data-ttu-id="6dd91-1331">它會從目前的要求和明確的值中提供環境 `{ id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1331">It provides ambient values from the current request and explicit values: `{ id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet)]

<span data-ttu-id="6dd91-1332">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1332">The preceding code:</span></span>

* <span data-ttu-id="6dd91-1333">傳回 `/Widget/Index/17`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1333">Returns `/Widget/Index/17`</span></span>
* <span data-ttu-id="6dd91-1334">透過 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> [DI](xref:fundamentals/dependency-injection)取得。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1334">Gets <xref:Microsoft.AspNetCore.Routing.LinkGenerator> via [DI](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="6dd91-1335">下列程式碼不會提供環境值和明確的 `{ controller = "Home", action = "Subscribe", id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1335">The following code provides no ambient values and explicit values: `{ controller = "Home", action = "Subscribe", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet2)]

<span data-ttu-id="6dd91-1336">上述方法會傳回`/Home/Subscribe/17`</span><span class="sxs-lookup"><span data-stu-id="6dd91-1336">The preceding  method returns `/Home/Subscribe/17`</span></span>

<span data-ttu-id="6dd91-1337">中的下列程式碼會傳回 `WidgetController` `/Widget/Subscribe/17` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1337">The following code in the `WidgetController` returns `/Widget/Subscribe/17`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/WidgetController.cs?name=snippet3)]

<span data-ttu-id="6dd91-1338">下列程式碼會從目前要求中的環境值和明確值提供 `{ action = "Edit", id = 17, }` 控制器：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1338">The following code provides the controller from ambient values in the current request and explicit values: `{ action = "Edit", id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/GadgetController.cs?name=snippet)]

<span data-ttu-id="6dd91-1339">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1339">In the preceding code:</span></span>

* <span data-ttu-id="6dd91-1340">`/Gadget/Edit/17`傳回。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1340">`/Gadget/Edit/17` is returned.</span></span>
* <span data-ttu-id="6dd91-1341"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url>取得 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1341"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.Url> gets the <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>.</span></span>
* <xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*>   
<span data-ttu-id="6dd91-1342">產生具有動作方法之絕對路徑的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1342">generates a URL with an absolute path for an action method.</span></span> <span data-ttu-id="6dd91-1343">URL 包含指定的 `action` 名稱和 `route` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1343">The URL contains the specified `action` name and `route` values.</span></span>

<span data-ttu-id="6dd91-1344">下列程式碼提供來自目前要求和明確值的環境 `{ page = "./Edit, id = 17, }` 值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1344">The following code provides ambient values from the current request and explicit values: `{ page = "./Edit, id = 17, }`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="6dd91-1345">上述程式碼會 `url` 在 `/Edit/17` [編輯] Razor 頁面包含下列頁面指示詞時，將設為：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1345">The preceding code sets `url` to  `/Edit/17` when the Edit Razor Page contains the following page directive:</span></span>

 `@page "{id:int}"`

<span data-ttu-id="6dd91-1346">如果 [編輯] 頁面不包含 `"{id:int}"` 路由範本， `url` 則為 `/Edit?id=17` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1346">If the Edit page doesn't contain the `"{id:int}"` route template, `url` is `/Edit?id=17`.</span></span>

<span data-ttu-id="6dd91-1347"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper>除了此處所述的規則之外，MVC 的行為也會增加一層複雜度：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1347">The behavior of MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> adds a layer of complexity in addition to the rules described here:</span></span>

* <span data-ttu-id="6dd91-1348">`IUrlHelper`一律會提供來自目前要求的路由值做為環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1348">`IUrlHelper` always provides the route values from the current request as ambient values.</span></span>
* <span data-ttu-id="6dd91-1349">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*)一律會將目前的 `action` 和 `controller` 路由值複製成明確的值，除非由開發人員覆寫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1349">[IUrlHelper.Action](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Action*) always copies the current `action` and `controller` route values as explicit values unless overridden by the developer.</span></span>
* <span data-ttu-id="6dd91-1350">[IUrlHelper](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*)一律會將目前的 `page` 路由值複製成明確的值，除非予以覆寫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1350">[IUrlHelper.Page](xref:Microsoft.AspNetCore.Mvc.UrlHelperExtensions.Page*) always copies the current `page` route value as an explicit value unless overridden.</span></span> <!--by the user-->
* <span data-ttu-id="6dd91-1351">`IUrlHelper.Page`除非覆寫，否則一律會以明確的值覆寫目前的 `handler` 路由值 `null` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1351">`IUrlHelper.Page` always overrides the current `handler` route value with `null` as an explicit values unless overridden.</span></span>

<span data-ttu-id="6dd91-1352">使用者通常會因為環境值的行為詳細資料而感到驚訝，因為 MVC 似乎不會遵循其本身的規則。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1352">Users are often surprised by the behavioral details of ambient values, because MVC doesn't seem to follow its own rules.</span></span> <span data-ttu-id="6dd91-1353">基於歷史和相容性的原因，某些路由值（例如 `action` 、 `controller` 、 `page` 和） `handler` 有自己的特殊案例行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1353">For historical and compatibility reasons, certain route values such as `action`, `controller`, `page`, and `handler` have their own special-case behavior.</span></span>

<span data-ttu-id="6dd91-1354">所提供的對等功能 `LinkGenerator.GetPathByAction` ，會 `LinkGenerator.GetPathByPage` 複製這些異常的 `IUrlHelper` 以實現相容性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1354">The equivalent functionality provided by `LinkGenerator.GetPathByAction` and `LinkGenerator.GetPathByPage` duplicates these anomalies of `IUrlHelper` for compatibility.</span></span>

### <a name="url-generation-process"></a><span data-ttu-id="6dd91-1355">URL 產生進程</span><span class="sxs-lookup"><span data-stu-id="6dd91-1355">URL generation process</span></span>

<span data-ttu-id="6dd91-1356">一旦找到候選端點集合之後，URL 產生演算法就會：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1356">Once the set of candidate endpoints are found, the URL generation algorithm:</span></span>

* <span data-ttu-id="6dd91-1357">反復處理端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1357">Processes the endpoints iteratively.</span></span>
* <span data-ttu-id="6dd91-1358">傳回第一個成功的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1358">Returns the first successful result.</span></span>

<span data-ttu-id="6dd91-1359">此程式的第一個步驟是所謂的**路由值失效**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1359">The first step in this process is called **route value invalidation**.</span></span>  <span data-ttu-id="6dd91-1360">路由值失效是路由決定應該使用哪些來自環境值的路由值以及應該忽略的進程。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1360">Route value invalidation is the process by which routing decides which route values from the ambient values should be used and which should be ignored.</span></span> <span data-ttu-id="6dd91-1361">會考慮每個環境值，並將其與明確值結合，或予以忽略。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1361">Each ambient value is considered and either combined with the explicit values, or ignored.</span></span>

<span data-ttu-id="6dd91-1362">考慮環境值角色的最佳方式，就是他們會嘗試儲存應用程式開發人員在某些常見的情況下輸入。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1362">The best way to think about the role of ambient values is that they attempt to save application developers typing, in some common cases.</span></span> <span data-ttu-id="6dd91-1363">傳統上，環境值很有用的案例與 MVC 相關：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1363">Traditionally, the scenarios where ambient values are helpful are related to MVC:</span></span>

* <span data-ttu-id="6dd91-1364">連結至相同控制器中的另一個動作時，不需要指定控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1364">When linking to another action in the same controller, the controller name doesn't need to be specified.</span></span>
* <span data-ttu-id="6dd91-1365">連結至相同區域中的另一個控制器時，不需要指定區功能變數名稱稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1365">When linking to another controller in the same area, the area name doesn't need to be specified.</span></span>
* <span data-ttu-id="6dd91-1366">連結至相同的動作方法時，不需要指定路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1366">When linking to the same action method, route values don't need to be specified.</span></span>
* <span data-ttu-id="6dd91-1367">當連結到應用程式的另一個部分時，您不會想要在應用程式的該部分中執行不具意義的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1367">When linking to another part of the app, you don't want to carry over route values that have no meaning in that part of the app.</span></span>

<span data-ttu-id="6dd91-1368">呼叫 `LinkGenerator` 或傳回 `IUrlHelper` 的 `null` 通常是因為不了解路由值失效所造成。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1368">Calls to `LinkGenerator` or `IUrlHelper` that return `null` are usually caused by not understanding route value invalidation.</span></span> <span data-ttu-id="6dd91-1369">明確指定更多的路由值，以查看是否能解決問題，以針對路由值失效進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1369">Troubleshoot route value invalidation by explicitly specifying more of the route values to see if that solves the problem.</span></span>

<span data-ttu-id="6dd91-1370">路由值失效的假設是應用程式的 URL 配置是階層式的，並以從左至右形成的階層。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1370">Route value invalidation works on the assumption that the app's URL scheme is hierarchical, with a hierarchy formed from left-to-right.</span></span> <span data-ttu-id="6dd91-1371">請考慮使用「基本控制器路由」範本 `{controller}/{action}/{id?}` ，以直覺的方式來瞭解這在實務上如何運作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1371">Consider the basic controller route template `{controller}/{action}/{id?}` to get an intuitive sense of how this works in practice.</span></span> <span data-ttu-id="6dd91-1372">值的**變更**會使顯示在右側的所有路由值**失效**。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1372">A **change** to a value **invalidates** all of the route values that appear to the right.</span></span> <span data-ttu-id="6dd91-1373">這反映了關於階層的假設。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1373">This reflects the assumption about hierarchy.</span></span> <span data-ttu-id="6dd91-1374">如果應用程式具有的環境值 `id` ，且作業為指定了不同的值 `controller` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1374">If the app has an ambient value for `id`, and the operation specifies a different value for the `controller`:</span></span>

* <span data-ttu-id="6dd91-1375">`id`不會重複使用 `{controller}` ，因為位於的左邊 `{id?}` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1375">`id` won't be reused because `{controller}` is to the left of `{id?}`.</span></span>

<span data-ttu-id="6dd91-1376">示範此原則的一些範例如下：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1376">Some examples demonstrating this principle:</span></span>

* <span data-ttu-id="6dd91-1377">如果明確的值包含的值 `id` ，則會忽略的環境值 `id` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1377">If the explicit values contain a value for `id`, the ambient value for `id` is ignored.</span></span> <span data-ttu-id="6dd91-1378">您 `controller` 可以使用和的環境值 `action` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1378">The ambient values for `controller` and `action` can be used.</span></span>
* <span data-ttu-id="6dd91-1379">如果明確的值包含的值 `action` ，則會忽略的任何環境值 `action` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1379">If the explicit values contain a value for `action`, any ambient value for `action` is ignored.</span></span> <span data-ttu-id="6dd91-1380">可以使用的環境值 `controller` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1380">The ambient values for `controller` can be used.</span></span> <span data-ttu-id="6dd91-1381">如果的明確值與 `action` 的環境值不同 `action` ，則 `id` 不會使用此值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1381">If the explicit value for `action` is different from the ambient value for `action`, the `id` value won't be used.</span></span>  <span data-ttu-id="6dd91-1382">如果的明確值與 `action` 的環境值相同 `action` ，則 `id` 可以使用值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1382">If the explicit value for `action` is the same as the ambient value for `action`, the `id` value can be used.</span></span>
* <span data-ttu-id="6dd91-1383">如果明確的值包含的值 `controller` ，則會忽略的任何環境值 `controller` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1383">If the explicit values contain a value for `controller`, any ambient value for `controller` is ignored.</span></span> <span data-ttu-id="6dd91-1384">如果的明確值與 `controller` 的環境值不同 `controller` ，則 `action` `id` 不會使用和值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1384">If the explicit value for `controller` is different from the ambient value for `controller`, the `action` and `id` values won't be used.</span></span> <span data-ttu-id="6dd91-1385">如果的明確值與 `controller` 的環境值相同 `controller` ，則 `action` `id` 可以使用和值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1385">If the explicit value for `controller` is the same as the ambient value for `controller`, the `action` and `id` values can be used.</span></span>

<span data-ttu-id="6dd91-1386">這個程式會因為屬性路由的存在和專用的慣例路由而變得更複雜。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1386">This process is further complicated by the existence of attribute routes and dedicated conventional routes.</span></span> <span data-ttu-id="6dd91-1387">控制器慣例路由，例如 `{controller}/{action}/{id?}` 使用路由參數來指定階層。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1387">Controller conventional routes such as `{controller}/{action}/{id?}` specify a hierarchy using route parameters.</span></span> <span data-ttu-id="6dd91-1388">針對[專用的傳統路由](xref:mvc/controllers/routing#dcr)，以及控制器和頁面的[屬性路由](xref:mvc/controllers/routing#ar) Razor ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1388">For [dedicated conventional routes](xref:mvc/controllers/routing#dcr) and [attribute routes](xref:mvc/controllers/routing#ar) to controllers and Razor Pages:</span></span>

* <span data-ttu-id="6dd91-1389">有路由值的階層。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1389">There is a hierarchy of route values.</span></span>
* <span data-ttu-id="6dd91-1390">它們不會出現在範本中。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1390">They don't appear in the template.</span></span>

<span data-ttu-id="6dd91-1391">在這些情況下，URL 產生會定義**必要的值**概念。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1391">For these cases, URL generation defines the **required values** concept.</span></span> <span data-ttu-id="6dd91-1392">控制器和頁面所建立的端點， Razor 所指定的必要值可讓路由值失效。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1392">Endpoints created by controllers and Razor Pages have required values specified that allow route value invalidation to work.</span></span>

<span data-ttu-id="6dd91-1393">路由值失效演算法詳細資料：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1393">The route value invalidation algorithm in detail:</span></span>

* <span data-ttu-id="6dd91-1394">必要的值名稱會與路由參數結合，然後由左至右處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1394">The required value names are combined with the route parameters, then processed from left-to-right.</span></span>
* <span data-ttu-id="6dd91-1395">針對每個參數，會比較環境值和明確的值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1395">For each parameter, the ambient value and explicit value are compared:</span></span>
    * <span data-ttu-id="6dd91-1396">如果環境值和明確的值相同，則進程會繼續進行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1396">If the ambient value and explicit value are the same, the process continues.</span></span>
    * <span data-ttu-id="6dd91-1397">如果環境值存在且明確的值不是，則會在產生 URL 時使用環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1397">If the ambient value is present and the explicit value isn't, the ambient value is used when generating the URL.</span></span>
    * <span data-ttu-id="6dd91-1398">如果環境值不存在，且明確的值為，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1398">If the ambient value isn't present and the explicit value is, reject the ambient value and all subsequent ambient values.</span></span>
    * <span data-ttu-id="6dd91-1399">如果環境值和明確值存在，而且這兩個值不同，則會拒絕環境值和所有後續的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1399">If the ambient value and the explicit value are present, and the two values are different, reject the ambient value and all subsequent ambient values.</span></span>

<span data-ttu-id="6dd91-1400">此時，URL 產生作業已準備好評估路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1400">At this point, the URL generation operation is ready to evaluate route constraints.</span></span> <span data-ttu-id="6dd91-1401">一組接受的值會與提供給條件約束的參數預設值結合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1401">The set of accepted values is combined with the parameter default values, which is provided to constraints.</span></span> <span data-ttu-id="6dd91-1402">如果條件約束都通過，作業就會繼續。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1402">If the constraints all pass, the operation continues.</span></span>

<span data-ttu-id="6dd91-1403">接下來，可以使用**接受的值**來展開路由範本。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1403">Next, the **accepted values** can be used to expand the route template.</span></span> <span data-ttu-id="6dd91-1404">會處理路由範本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1404">The route template is processed:</span></span>

* <span data-ttu-id="6dd91-1405">從左至右。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1405">From left-to-right.</span></span>
* <span data-ttu-id="6dd91-1406">每個參數都有其接受的值取代。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1406">Each parameter has its accepted value substituted.</span></span>
* <span data-ttu-id="6dd91-1407">有下列特殊案例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1407">With the following special cases:</span></span>
  * <span data-ttu-id="6dd91-1408">如果接受的值遺漏值，且參數具有預設值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1408">If the accepted values is missing a value and the parameter has a default value, the default value is used.</span></span>
  * <span data-ttu-id="6dd91-1409">如果接受的值遺漏值，而且參數是選擇性的，則會繼續處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1409">If the accepted values is missing a value and the parameter is optional, processing continues.</span></span>
  * <span data-ttu-id="6dd91-1410">如果遺漏選擇性參數右邊的任何路由參數具有值，則作業會失敗。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1410">If any route parameter to the right of a missing optional parameter has a value, the operation fails.</span></span>
  * <!-- review default-valued parameters optional parameters --> <span data-ttu-id="6dd91-1411">連續的預設值參數和選擇性參數會盡可能折迭。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1411">Contiguous default-valued parameters and optional parameters are collapsed where possible.</span></span>

<span data-ttu-id="6dd91-1412">明確提供並不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1412">Values explicitly provided that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="6dd91-1413">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1413">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="6dd91-1414">環境值</span><span class="sxs-lookup"><span data-stu-id="6dd91-1414">Ambient Values</span></span>                     | <span data-ttu-id="6dd91-1415">明確值</span><span class="sxs-lookup"><span data-stu-id="6dd91-1415">Explicit Values</span></span>                        | <span data-ttu-id="6dd91-1416">結果</span><span class="sxs-lookup"><span data-stu-id="6dd91-1416">Result</span></span>                  |
| ---
<span data-ttu-id="6dd91-1417">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1417">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1418">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1418">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1419">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1419">'Identity'</span></span>
- <span data-ttu-id="6dd91-1420">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1420">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1421">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1421">'Razor'</span></span>
- <span data-ttu-id="6dd91-1422">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1422">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1423">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1423">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1424">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1424">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1425">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1425">'Identity'</span></span>
- <span data-ttu-id="6dd91-1426">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1426">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1427">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1427">'Razor'</span></span>
- <span data-ttu-id="6dd91-1428">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1428">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1429">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1429">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1430">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1430">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1431">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1431">'Identity'</span></span>
- <span data-ttu-id="6dd91-1432">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1432">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1433">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1433">'Razor'</span></span>
- <span data-ttu-id="6dd91-1434">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1434">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1435">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1435">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1436">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1436">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1437">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1437">'Identity'</span></span>
- <span data-ttu-id="6dd91-1438">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1438">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1439">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1439">'Razor'</span></span>
- <span data-ttu-id="6dd91-1440">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1440">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1441">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1441">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1442">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1442">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1443">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1443">'Identity'</span></span>
- <span data-ttu-id="6dd91-1444">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1444">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1445">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1445">'Razor'</span></span>
- <span data-ttu-id="6dd91-1446">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1446">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1447">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1447">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1448">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1448">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1449">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1449">'Identity'</span></span>
- <span data-ttu-id="6dd91-1450">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1450">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1451">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1451">'Razor'</span></span>
- <span data-ttu-id="6dd91-1452">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1452">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1453">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1453">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1454">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1454">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1455">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1455">'Identity'</span></span>
- <span data-ttu-id="6dd91-1456">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1456">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1457">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1457">'Razor'</span></span>
- <span data-ttu-id="6dd91-1458">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1458">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1459">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1459">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1460">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1460">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1461">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1461">'Identity'</span></span>
- <span data-ttu-id="6dd91-1462">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1462">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1463">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1463">'Razor'</span></span>
- <span data-ttu-id="6dd91-1464">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1464">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1466">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1467">'Identity'</span></span>
- <span data-ttu-id="6dd91-1468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1468">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1469">'Razor'</span></span>
- <span data-ttu-id="6dd91-1470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1472">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1473">'Identity'</span></span>
- <span data-ttu-id="6dd91-1474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1474">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1475">'Razor'</span></span>
- <span data-ttu-id="6dd91-1476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1476">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1477">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1477">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1478">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1479">'Identity'</span></span>
- <span data-ttu-id="6dd91-1480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1480">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1481">'Razor'</span></span>
- <span data-ttu-id="6dd91-1482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1484">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1485">'Identity'</span></span>
- <span data-ttu-id="6dd91-1486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1486">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1487">'Razor'</span></span>
- <span data-ttu-id="6dd91-1488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1490">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1491">'Identity'</span></span>
- <span data-ttu-id="6dd91-1492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1492">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1493">'Razor'</span></span>
- <span data-ttu-id="6dd91-1494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1494">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1495">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1495">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1496">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1497">'Identity'</span></span>
- <span data-ttu-id="6dd91-1498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1498">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1499">'Razor'</span></span>
- <span data-ttu-id="6dd91-1500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1502">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1503">'Identity'</span></span>
- <span data-ttu-id="6dd91-1504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1504">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1505">'Razor'</span></span>
- <span data-ttu-id="6dd91-1506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1506">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1507">----------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1507">----------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1508">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1509">'Identity'</span></span>
- <span data-ttu-id="6dd91-1510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1510">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1511">'Razor'</span></span>
- <span data-ttu-id="6dd91-1512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1512">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1513">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1513">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1514">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1514">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1515">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1515">'Identity'</span></span>
- <span data-ttu-id="6dd91-1516">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1516">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1517">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1517">'Razor'</span></span>
- <span data-ttu-id="6dd91-1518">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1518">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1519">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1519">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1520">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1520">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1521">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1521">'Identity'</span></span>
- <span data-ttu-id="6dd91-1522">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1522">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1523">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1523">'Razor'</span></span>
- <span data-ttu-id="6dd91-1524">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1524">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1525">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1525">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1526">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1526">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1527">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1527">'Identity'</span></span>
- <span data-ttu-id="6dd91-1528">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1528">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1529">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1529">'Razor'</span></span>
- <span data-ttu-id="6dd91-1530">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1530">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1531">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1531">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1532">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1532">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1533">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1533">'Identity'</span></span>
- <span data-ttu-id="6dd91-1534">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1534">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1535">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1535">'Razor'</span></span>
- <span data-ttu-id="6dd91-1536">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1536">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1537">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1537">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1538">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1539">'Identity'</span></span>
- <span data-ttu-id="6dd91-1540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1540">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1541">'Razor'</span></span>
- <span data-ttu-id="6dd91-1542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1544">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1545">'Identity'</span></span>
- <span data-ttu-id="6dd91-1546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1546">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1547">'Razor'</span></span>
- <span data-ttu-id="6dd91-1548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1550">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1551">'Identity'</span></span>
- <span data-ttu-id="6dd91-1552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1552">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1553">'Razor'</span></span>
- <span data-ttu-id="6dd91-1554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1556">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1557">'Identity'</span></span>
- <span data-ttu-id="6dd91-1558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1558">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1559">'Razor'</span></span>
- <span data-ttu-id="6dd91-1560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1562">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1563">'Identity'</span></span>
- <span data-ttu-id="6dd91-1564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1564">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1565">'Razor'</span></span>
- <span data-ttu-id="6dd91-1566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1568">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1569">'Identity'</span></span>
- <span data-ttu-id="6dd91-1570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1570">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1571">'Razor'</span></span>
- <span data-ttu-id="6dd91-1572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1574">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1575">'Identity'</span></span>
- <span data-ttu-id="6dd91-1576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1576">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1577">'Razor'</span></span>
- <span data-ttu-id="6dd91-1578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1578">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1579">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1579">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1580">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1581">'Identity'</span></span>
- <span data-ttu-id="6dd91-1582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1582">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1583">'Razor'</span></span>
- <span data-ttu-id="6dd91-1584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1584">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1585">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1585">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1586">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1586">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1587">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1587">'Identity'</span></span>
- <span data-ttu-id="6dd91-1588">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1588">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1589">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1589">'Razor'</span></span>
- <span data-ttu-id="6dd91-1590">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1590">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1591">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1591">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1592">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1592">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1593">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1593">'Identity'</span></span>
- <span data-ttu-id="6dd91-1594">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1594">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1595">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1595">'Razor'</span></span>
- <span data-ttu-id="6dd91-1596">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1596">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1597">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1597">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1598">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1598">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1599">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1599">'Identity'</span></span>
- <span data-ttu-id="6dd91-1600">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1600">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1601">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1601">'Razor'</span></span>
- <span data-ttu-id="6dd91-1602">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1602">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1604">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1605">'Identity'</span></span>
- <span data-ttu-id="6dd91-1606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1606">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1607">'Razor'</span></span>
- <span data-ttu-id="6dd91-1608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1608">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1609">------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1609">------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1610">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1611">'Identity'</span></span>
- <span data-ttu-id="6dd91-1612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1612">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1613">'Razor'</span></span>
- <span data-ttu-id="6dd91-1614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1614">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1615">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1615">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1616">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1617">'Identity'</span></span>
- <span data-ttu-id="6dd91-1618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1618">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1619">'Razor'</span></span>
- <span data-ttu-id="6dd91-1620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1622">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1623">'Identity'</span></span>
- <span data-ttu-id="6dd91-1624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1624">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1625">'Razor'</span></span>
- <span data-ttu-id="6dd91-1626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1627">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1627">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1628">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1628">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1629">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1629">'Identity'</span></span>
- <span data-ttu-id="6dd91-1630">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1630">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1631">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1631">'Razor'</span></span>
- <span data-ttu-id="6dd91-1632">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1632">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1633">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1633">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1634">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1634">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1635">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1635">'Identity'</span></span>
- <span data-ttu-id="6dd91-1636">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1636">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1637">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1637">'Razor'</span></span>
- <span data-ttu-id="6dd91-1638">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1638">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1639">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1639">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1640">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1640">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1641">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1641">'Identity'</span></span>
- <span data-ttu-id="6dd91-1642">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1642">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1643">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1643">'Razor'</span></span>
- <span data-ttu-id="6dd91-1644">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1644">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1645">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1645">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1646">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1646">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1647">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1647">'Identity'</span></span>
- <span data-ttu-id="6dd91-1648">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1648">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1649">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1649">'Razor'</span></span>
- <span data-ttu-id="6dd91-1650">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1650">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1651">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1651">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1652">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1652">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1653">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1653">'Identity'</span></span>
- <span data-ttu-id="6dd91-1654">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1654">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1655">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1655">'Razor'</span></span>
- <span data-ttu-id="6dd91-1656">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1656">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1657">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1657">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1658">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1658">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1659">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1659">'Identity'</span></span>
- <span data-ttu-id="6dd91-1660">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1660">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1661">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1661">'Razor'</span></span>
- <span data-ttu-id="6dd91-1662">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1662">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1663">------------ | |控制器 = "Home" |action = "About" |`/Home/About`|
|控制器 = "Home" |控制器 = "Order"，action = "About" |`/Order/About`|
|控制器 = "Home"，color = "Red" |action = "About" |`/Home/About`|
|控制器 = "Home" |action = "About"，color = "Red" |`/Home/About?color=Red`                                |</span><span class="sxs-lookup"><span data-stu-id="6dd91-1663">------------ | | controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |</span></span>

### <a name="problems-with-route-value-invalidation"></a><span data-ttu-id="6dd91-1664">路由值失效的問題</span><span class="sxs-lookup"><span data-stu-id="6dd91-1664">Problems with route value invalidation</span></span>

<span data-ttu-id="6dd91-1665">從 ASP.NET Core 3.0，先前 ASP.NET Core 版本中使用的某些 URL 產生配置，不適用於 URL 產生的效果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1665">As of ASP.NET Core 3.0, some URL generation schemes used in earlier ASP.NET Core versions don't work well with URL generation.</span></span> <span data-ttu-id="6dd91-1666">ASP.NET Core 小組計畫在未來的版本中新增功能，以解決這些需求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1666">The ASP.NET Core team plans to add features to address these needs in a future release.</span></span> <span data-ttu-id="6dd91-1667">現在最好的解決方案是使用舊版路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1667">For now the best solution is to use legacy routing.</span></span>

<span data-ttu-id="6dd91-1668">下列程式碼顯示路由不支援的 URL 產生配置範例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1668">The following code shows an example of a URL generation scheme that's not supported by routing.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupUnsupported.cs?name=snippet)]

<span data-ttu-id="6dd91-1669">在上述程式碼中， `culture` route 參數是用於當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1669">In the preceding code, the `culture` route parameter is used for localization.</span></span> <span data-ttu-id="6dd91-1670">想要讓 `culture` 參數一律接受為環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1670">The desire is to have the `culture` parameter always accepted as an ambient value.</span></span> <span data-ttu-id="6dd91-1671">不過， `culture` 因為必要值的使用方式，所以不接受參數作為環境值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1671">However, the `culture` parameter is not accepted as an ambient value because of the way required values work:</span></span>

* <span data-ttu-id="6dd91-1672">在 `"default"` 路由範本中， `culture` 路由參數是在的左邊 `controller` ，因此變更 `controller` 不會使失效 `culture` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1672">In the `"default"` route template, the `culture` route parameter is to the left of `controller`, so changes to `controller` won't invalidate `culture`.</span></span>
* <span data-ttu-id="6dd91-1673">在 `"blog"` 路由範本中， `culture` 路由參數會被視為位於的右邊 `controller` ，這會出現在必要的值中。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1673">In the `"blog"` route template, the `culture` route parameter is considered to be to the right of `controller`, which appears in the required values.</span></span>

## <a name="configuring-endpoint-metadata"></a><span data-ttu-id="6dd91-1674">設定端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="6dd91-1674">Configuring endpoint metadata</span></span>

<span data-ttu-id="6dd91-1675">下列連結提供設定端點中繼資料的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1675">The following links provide information on configuring endpoint metadata:</span></span>

* [<span data-ttu-id="6dd91-1676">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="6dd91-1676">Enable Cors with endpoint routing</span></span>](xref:security/cors#enable-cors-with-endpoint-routing)
* <span data-ttu-id="6dd91-1677">使用自訂屬性的[IAuthorizationPolicyProvider 範例](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) `[MinimumAgeAuthorize]`</span><span class="sxs-lookup"><span data-stu-id="6dd91-1677">[IAuthorizationPolicyProvider sample](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) using a custom `[MinimumAgeAuthorize]` attribute</span></span>
* <span data-ttu-id="6dd91-1678">[使用 [授權] 屬性測試驗證](xref:security/authentication/identity#test-identity)</span><span class="sxs-lookup"><span data-stu-id="6dd91-1678">[Test authentication with the [Authorize] attribute](xref:security/authentication/identity#test-identity)</span></span>
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* <span data-ttu-id="6dd91-1679">[選取具有 [授權] 屬性的配置](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span><span class="sxs-lookup"><span data-stu-id="6dd91-1679">[Selecting the scheme with the [Authorize] attribute](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span></span>
* <span data-ttu-id="6dd91-1680">[使用 [授權] 屬性套用原則](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span><span class="sxs-lookup"><span data-stu-id="6dd91-1680">[Applying policies using the [Authorize] attribute](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span></span>
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a><span data-ttu-id="6dd91-1681">搭配 RequireHost 的路由中的主機比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-1681">Host matching in routes with RequireHost</span></span>

<span data-ttu-id="6dd91-1682"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*>將條件約束套用至需要指定之主控制項的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1682"><xref:Microsoft.AspNetCore.Builder.RoutingEndpointConventionBuilderExtensions.RequireHost*> applies a constraint to the route which requires the specified host.</span></span> <span data-ttu-id="6dd91-1683">`RequireHost`或[[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute)參數可以是：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1683">The `RequireHost` or [[Host]](xref:Microsoft.AspNetCore.Routing.HostAttribute) parameter can be:</span></span>

* <span data-ttu-id="6dd91-1684">主機： `www.domain.com` ，符合 `www.domain.com` 任何埠。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1684">Host: `www.domain.com`, matches `www.domain.com` with any port.</span></span>
* <span data-ttu-id="6dd91-1685">具有萬用字元的主機： `*.domain.com` 、符合 `www.domain.com` 、 `subdomain.domain.com` 或 `www.subdomain.domain.com` 任何埠上的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1685">Host with wildcard: `*.domain.com`, matches `www.domain.com`, `subdomain.domain.com`, or `www.subdomain.domain.com` on any port.</span></span>
* <span data-ttu-id="6dd91-1686">埠： `*:5000` ，符合任何主機的通訊埠5000。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1686">Port: `*:5000`, matches port 5000 with any host.</span></span>
* <span data-ttu-id="6dd91-1687">主機和埠： `www.domain.com:5000` 或 `*.domain.com:5000` 符合主機和埠。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1687">Host and port: `www.domain.com:5000` or `*.domain.com:5000`, matches host and port.</span></span>

<span data-ttu-id="6dd91-1688">您可以使用或來指定多個參數 `RequireHost` `[Host]` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1688">Multiple parameters can be specified using `RequireHost` or `[Host]`.</span></span> <span data-ttu-id="6dd91-1689">條件約束符合任何參數的有效主機。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1689">The constraint  matches hosts valid for any of the parameters.</span></span> <span data-ttu-id="6dd91-1690">例如，會比對 `[Host("domain.com", "*.domain.com")]` `domain.com` 、 `www.domain.com` 和 `subdomain.domain.com` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1690">For example, `[Host("domain.com", "*.domain.com")]` matches `domain.com`, `www.domain.com`, and `subdomain.domain.com`.</span></span>

<span data-ttu-id="6dd91-1691">下列程式碼會使用 `RequireHost` 來要求路由上指定的主機：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1691">The following code uses `RequireHost` to require the specified host on the route:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupRequireHost.cs?name=snippet)]

<span data-ttu-id="6dd91-1692">下列程式碼會在 `[Host]` 控制器上使用屬性來要求任何指定的主機：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1692">The following code uses the `[Host]` attribute on the controller to require any of the specified hosts:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Controllers/ProductController.cs?name=snippet)]

<span data-ttu-id="6dd91-1693">當 `[Host]` 屬性同時套用至控制器和動作方法時：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1693">When the `[Host]` attribute is applied to both the controller and action method:</span></span>

* <span data-ttu-id="6dd91-1694">會使用動作上的屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1694">The attribute on the action is used.</span></span>
* <span data-ttu-id="6dd91-1695">已忽略控制器屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1695">The controller attribute is ignored.</span></span>

## <a name="performance-guidance-for-routing"></a><span data-ttu-id="6dd91-1696">路由的效能指引</span><span class="sxs-lookup"><span data-stu-id="6dd91-1696">Performance guidance for routing</span></span>

<span data-ttu-id="6dd91-1697">大部分的路由都已在 ASP.NET Core 3.0 中更新，以提高效能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1697">Most of routing was updated in ASP.NET Core 3.0 to increase performance.</span></span>

<span data-ttu-id="6dd91-1698">當應用程式發生效能問題時，通常會懷疑路由是問題。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1698">When an app has performance problems, routing is often suspected as the problem.</span></span> <span data-ttu-id="6dd91-1699">路由的原因是，控制器和頁面之類的架構，會 Razor 回報在其記錄訊息內所花費的時間量。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1699">The reason routing is suspected is that frameworks like controllers and Razor Pages report the amount of time spent inside the framework in their logging messages.</span></span> <span data-ttu-id="6dd91-1700">當控制器回報的時間與要求的總時間之間有顯著的差異時：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1700">When there's a significant difference between the time reported by controllers and the total time of the request:</span></span>

* <span data-ttu-id="6dd91-1701">開發人員會將其應用程式程式碼排除為問題的來源。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1701">Developers eliminate their app code as the source of the problem.</span></span>
* <span data-ttu-id="6dd91-1702">通常會假設路由是原因。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1702">It's common to assume routing is the cause.</span></span>

<span data-ttu-id="6dd91-1703">路由會使用數千個端點來測試效能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1703">Routing is performance tested using thousands of endpoints.</span></span> <span data-ttu-id="6dd91-1704">一般的應用程式不太可能會遇到太大的效能問題。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1704">It's unlikely that a typical app will encounter a performance problem just by being too large.</span></span> <span data-ttu-id="6dd91-1705">緩慢路由效能最常見的根本原因通常是行為不佳的自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1705">The most common root cause of slow routing performance is usually a badly-behaving custom middleware.</span></span>

<span data-ttu-id="6dd91-1706">下列程式碼範例示範縮小延遲來源的基本技巧：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1706">This following code sample demonstrates a basic technique for narrowing down the source of delay:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupDelay.cs?name=snippet)]

<span data-ttu-id="6dd91-1707">若要進行時間路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1707">To time routing:</span></span>

* <span data-ttu-id="6dd91-1708">使用上述程式碼中所示的計時中介軟體複本來交錯每個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1708">Interleave each middleware with a copy of the timing middleware shown in the preceding code.</span></span>
* <span data-ttu-id="6dd91-1709">新增唯一識別碼，將計時資料與程式碼相互關聯。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1709">Add a unique identifier to correlate the timing data with the code.</span></span>

<span data-ttu-id="6dd91-1710">這是將延遲縮小到最大的基本方式，例如，超過 `10ms` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1710">This is a basic way to narrow down the delay when it's significant, for example, more than `10ms`.</span></span>  <span data-ttu-id="6dd91-1711">`Time 2`從 `Time 1` 報表減去中介軟體內所花費的時間 `UseRouting` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1711">Subtracting `Time 2` from `Time 1` reports the time spent inside the `UseRouting` middleware.</span></span>

<span data-ttu-id="6dd91-1712">下列程式碼會針對上述計時程式碼使用更精簡的方法：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1712">The following code uses a more compact approach to the preceding timing code:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippetSW)]

[!code-csharp[](routing/samples/3.x/RoutingSample/StartupSW.cs?name=snippet)]

### <a name="potentially-expensive-routing-features"></a><span data-ttu-id="6dd91-1713">可能昂貴的路由功能</span><span class="sxs-lookup"><span data-stu-id="6dd91-1713">Potentially expensive routing features</span></span>

<span data-ttu-id="6dd91-1714">下列清單提供一些路由功能的深入解析，相較于基本的路由範本，這會相當耗費資源：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1714">The following list provides some insight into routing features that are relatively expensive compared with basic route templates:</span></span>

* <span data-ttu-id="6dd91-1715">正則運算式：可以撰寫複雜的正則運算式，或具有少量輸入的長期執行時間。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1715">Regular expressions: It's possible to write regular expressions that are complex, or have long running time with a small amount of input.</span></span>

* <span data-ttu-id="6dd91-1716">複雜區段（ `{x}-{y}-{z}` ）：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1716">Complex segments (`{x}-{y}-{z}`):</span></span> 
  * <span data-ttu-id="6dd91-1717">比剖析一般 URL 路徑區段高得多。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1717">Are significantly more expensive than parsing a regular URL path segment.</span></span>
  * <span data-ttu-id="6dd91-1718">導致已配置更多子字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1718">Result in many more substrings being allocated.</span></span>
  * <span data-ttu-id="6dd91-1719">在 ASP.NET Core 3.0 路由效能更新中，未更新複雜的區段邏輯。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1719">The complex segment logic was not updated in ASP.NET Core 3.0 routing performance update.</span></span>

* <span data-ttu-id="6dd91-1720">同步資料存取：許多複雜的應用程式在其路由中都具有資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1720">Synchronous data access: Many complex apps have database access as part of their routing.</span></span> <span data-ttu-id="6dd91-1721">ASP.NET Core 2.2 和較舊的路由可能不會提供支援資料庫存取路由的正確擴充點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1721">ASP.NET Core 2.2 and earlier routing might not provide the right extensibility points to support database access routing.</span></span> <span data-ttu-id="6dd91-1722">例如，、 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 和 <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> 都是同步的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1722">For example, <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, and <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are synchronous.</span></span> <span data-ttu-id="6dd91-1723">和等擴充點 <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> 都是非同步。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1723">Extensibility points such as <xref:Microsoft.AspNetCore.Routing.MatcherPolicy> and <xref:Microsoft.AspNetCore.Routing.EndpointSelectorContext> are asynchronous.</span></span>

## <a name="guidance-for-library-authors"></a><span data-ttu-id="6dd91-1724">程式庫作者指引</span><span class="sxs-lookup"><span data-stu-id="6dd91-1724">Guidance for library authors</span></span>

<span data-ttu-id="6dd91-1725">本章節包含程式庫作者在路由之上建立的指導方針。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1725">This section contains guidance for library authors building on top of routing.</span></span> <span data-ttu-id="6dd91-1726">這些詳細資料的目的是要確保應用程式開發人員能夠使用延伸路由的程式庫和架構。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1726">These details are intended to ensure that app developers have a good experience using libraries and frameworks that extend routing.</span></span>

### <a name="define-endpoints"></a><span data-ttu-id="6dd91-1727">定義端點</span><span class="sxs-lookup"><span data-stu-id="6dd91-1727">Define endpoints</span></span>

<span data-ttu-id="6dd91-1728">若要建立使用路由進行 URL 比對的架構，請先定義以為基礎的使用者體驗 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1728">To create a framework that uses routing for URL matching, start by defining a user experience that builds on top of <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span>

<span data-ttu-id="6dd91-1729">**請**在之上建立組建 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1729">**DO** build on top of <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span> <span data-ttu-id="6dd91-1730">這可讓使用者使用其他 ASP.NET Core 功能來撰寫您的架構，而不會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1730">This allows users to compose your framework with other ASP.NET Core features without confusion.</span></span> <span data-ttu-id="6dd91-1731">每個 ASP.NET Core 範本都包含路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1731">Every ASP.NET Core template includes routing.</span></span> <span data-ttu-id="6dd91-1732">假設路由存在且熟悉使用者。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1732">Assume routing is present and familiar for users.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="6dd91-1733">**確實**從對所執行的呼叫傳回密封的實體型別 `MapMyFramework(...)` <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1733">**DO** return a sealed concrete type from a call to `MapMyFramework(...)` that implements <xref:Microsoft.AspNetCore.Builder.IEndpointConventionBuilder>.</span></span> <span data-ttu-id="6dd91-1734">大部分 `Map...` 的架構方法會遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1734">Most framework `Map...` methods follow this pattern.</span></span> <span data-ttu-id="6dd91-1735">`IEndpointConventionBuilder`介面：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1735">The `IEndpointConventionBuilder` interface:</span></span>

* <span data-ttu-id="6dd91-1736">允許複合性中繼資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1736">Allows composability of metadata.</span></span>
* <span data-ttu-id="6dd91-1737">的目標是各種擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1737">Is targeted by a variety of extension methods.</span></span>

<span data-ttu-id="6dd91-1738">宣告您自己的型別可讓您將自己的架構特定功能加入至產生器。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1738">Declaring your own type allows you to add your own framework-specific functionality to the builder.</span></span> <span data-ttu-id="6dd91-1739">將架構宣告的產生器和轉送呼叫包裝在一起是正常的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1739">It's ok to wrap a framework-declared builder and forward calls to it.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization()
                                 .WithMyFrameworkFeature(awesome: true);

    endpoints.MapHealthChecks("/healthz");
});
```

<span data-ttu-id="6dd91-1740">**請考慮**撰寫自己 <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> 的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1740">**CONSIDER** writing your own <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>.</span></span> <span data-ttu-id="6dd91-1741">`EndpointDataSource`是用來宣告和更新端點集合的低層級基本類型。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1741">`EndpointDataSource` is the low-level primitive for declaring and updating a collection of endpoints.</span></span> <span data-ttu-id="6dd91-1742">`EndpointDataSource`是控制器和頁面所使用的強大 API Razor 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1742">`EndpointDataSource` is a powerful API used by controllers and Razor Pages.</span></span>

<span data-ttu-id="6dd91-1743">路由測試有一個不會更新資料來源的[基本範例](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1743">The routing tests have a [basic example](https://github.com/aspnet/AspNetCore/blob/master/src/Http/Routing/test/testassets/RoutingSandbox/Framework/FrameworkEndpointDataSource.cs#L17) of a non-updating data source.</span></span>

<span data-ttu-id="6dd91-1744">預設**不會**嘗試註冊 `EndpointDataSource` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1744">**DO NOT** attempt to register an `EndpointDataSource` by default.</span></span> <span data-ttu-id="6dd91-1745">要求使用者在中註冊您的架構 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1745">Require users to register your framework in <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>.</span></span> <span data-ttu-id="6dd91-1746">路由的原理是預設不會包含任何內容，也 `UseEndpoints` 就是註冊端點的位置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1746">The philosophy of routing is that nothing is included by default, and that `UseEndpoints` is the place to register endpoints.</span></span>

### <a name="creating-routing-integrated-middleware"></a><span data-ttu-id="6dd91-1747">建立路由整合中介軟體</span><span class="sxs-lookup"><span data-stu-id="6dd91-1747">Creating routing-integrated middleware</span></span>

<span data-ttu-id="6dd91-1748">**請考慮**將元資料類型定義為介面。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1748">**CONSIDER** defining metadata types as an interface.</span></span>

<span data-ttu-id="6dd91-1749">您可以使用中繼資料**類型做為**類別和方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1749">**DO** make it possible to use metadata types as an attribute on classes and methods.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet2)]

<span data-ttu-id="6dd91-1750">控制器和頁面之類 Razor 的架構支援將中繼資料屬性套用至類型和方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1750">Frameworks like controllers and Razor Pages support applying metadata attributes to types and methods.</span></span> <span data-ttu-id="6dd91-1751">如果您宣告元資料類型：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1751">If you declare metadata types:</span></span>

* <span data-ttu-id="6dd91-1752">使其可做為[屬性](/dotnet/csharp/programming-guide/concepts/attributes/)存取。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1752">Make them accessible as [attributes](/dotnet/csharp/programming-guide/concepts/attributes/).</span></span>
* <span data-ttu-id="6dd91-1753">大部分的使用者都很熟悉套用屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1753">Most users are familiar with applying attributes.</span></span>

<span data-ttu-id="6dd91-1754">將元資料類型宣告為介面，會增加另一層彈性：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1754">Declaring a metadata type as an interface adds another layer of flexibility:</span></span>

* <span data-ttu-id="6dd91-1755">介面是可組合的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1755">Interfaces are composable.</span></span>
* <span data-ttu-id="6dd91-1756">開發人員可以宣告自己的類型，結合多個原則。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1756">Developers can declare their own types that combine multiple policies.</span></span>

<span data-ttu-id="6dd91-1757">**確實**能夠覆寫中繼資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1757">**DO** make it possible to override metadata, as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/ICoolMetadata.cs?name=snippet)]

<span data-ttu-id="6dd91-1758">遵循這些指導方針的最佳方式是避免定義**標記中繼資料**：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1758">The best way to follow these guidelines is to avoid defining **marker metadata**:</span></span>

* <span data-ttu-id="6dd91-1759">請不要只尋找元資料類型是否存在。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1759">Don't just look for the presence of a metadata type.</span></span>
* <span data-ttu-id="6dd91-1760">在中繼資料上定義屬性，並檢查屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1760">Define a property on the metadata and check the property.</span></span>

<span data-ttu-id="6dd91-1761">元資料集合會進行排序，並依優先順序支援覆寫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1761">The metadata collection is ordered and supports overriding by priority.</span></span> <span data-ttu-id="6dd91-1762">在控制器的案例中，動作方法上的中繼資料是最明確的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1762">In the case of controllers, metadata on the action method is most specific.</span></span>

<span data-ttu-id="6dd91-1763">**讓中間**件適用于且不使用路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1763">**DO** make middleware useful with and without routing.</span></span>

```csharp
app.UseRouting();

app.UseAuthorization(new AuthorizationPolicy() { ... });

app.UseEndpoints(endpoints =>
{
    // Your framework
    endpoints.MapMyFramework(...).RequrireAuthorization();
});
```

<span data-ttu-id="6dd91-1764">作為此指導方針的範例，請考慮 `UseAuthorization` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1764">As an example of this guideline, consider the `UseAuthorization` middleware.</span></span> <span data-ttu-id="6dd91-1765">授權中介軟體可讓您傳入回退原則。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1765">The authorization middleware allows you to pass in a fallback policy.</span></span> <!-- shown where?  (shown here) --> <span data-ttu-id="6dd91-1766">如果指定了回溯原則，則會套用至兩者：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1766">The fallback policy, if specified, applies to both:</span></span>

* <span data-ttu-id="6dd91-1767">沒有指定之原則的端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1767">Endpoints without a specified policy.</span></span>
* <span data-ttu-id="6dd91-1768">不符合端點的要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1768">Requests that don't match an endpoint.</span></span>

<span data-ttu-id="6dd91-1769">這可讓授權中介軟體適用于路由內容以外的地方。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1769">This makes the authorization middleware useful outside of the context of routing.</span></span> <span data-ttu-id="6dd91-1770">授權中介軟體可用於傳統中介軟體程式設計。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1770">The authorization middleware can be used for traditional middleware programming.</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="6dd91-1771">路由會負責將要求 Uri 對應至端點，並將傳入要求分派至這些端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1771">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="6dd91-1772">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1772">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="6dd91-1773">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1773">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="6dd91-1774">使用來自應用程式的路由資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1774">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="6dd91-1775">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1775">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="6dd91-1776"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1776">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="6dd91-1777">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1777">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="6dd91-1778">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1778">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="6dd91-1779">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1779">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dd91-1780">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1780">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="6dd91-1781">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1781">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="6dd91-1782">如需頁面中路由慣例的詳細資訊 Razor ，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1782">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="6dd91-1783">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="6dd91-1783">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="6dd91-1784">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="6dd91-1784">Routing basics</span></span>

<span data-ttu-id="6dd91-1785">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1785">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="6dd91-1786">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1786">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="6dd91-1787">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1787">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="6dd91-1788">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1788">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="6dd91-1789">開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專用的慣例路由，在特殊情況下，將額外的簡易路由新增到應用程式的高流量區域。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1789">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span> <span data-ttu-id="6dd91-1790">特殊情況的範例包括： blog 和電子商務端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1790">Specialized situations examples include, blog and ecommerce endpoints.</span></span>

<span data-ttu-id="6dd91-1791">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1791">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="6dd91-1792">這表示相同邏輯資源上的許多作業（例如 GET 和 POST）都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1792">This means that many operations, for example, GET, and POST, on the same logical resource use the same URL.</span></span> <span data-ttu-id="6dd91-1793">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1793">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

Razor<span data-ttu-id="6dd91-1794">頁面應用程式會使用預設的傳統路由，在應用程式的*Pages*資料夾中提供已命名的資源。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1794"> Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="6dd91-1795">還有其他慣例可讓您自訂 Razor 頁面路由行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1795">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="6dd91-1796">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1796">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="6dd91-1797">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1797">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="6dd91-1798">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1798">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="6dd91-1799">路由會使用*端點*（ `Endpoint` ）來代表應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1799">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="6dd91-1800">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1800">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="6dd91-1801">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1801">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="6dd91-1802">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1802">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="6dd91-1803">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1803">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="6dd91-1804">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1804">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="6dd91-1805"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1805"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="6dd91-1806">應用程式模型（例如 MVC/ Razor Pages）會註冊其所有端點，其具有可預測的路由案例執行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1806">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="6dd91-1807">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1807">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="6dd91-1808">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1808">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="6dd91-1809">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1809">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="6dd91-1810">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1810">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="6dd91-1811">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1811">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="6dd91-1812">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1812">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="6dd91-1813">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1813">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="6dd91-1814">隨著 ASP.NET Core 2.2 中的端點路由發行，端點連結僅限於 MVC/ Razor pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1814">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="6dd91-1815">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1815">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="6dd91-1816">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1816">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="6dd91-1817">[ASP.NET CORE mvc](xref:mvc/overview)會將路由新增至中介軟體管線，作為其設定的一部分，並處理 MVC 和 Razor 頁面應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1817">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="6dd91-1818">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1818">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="6dd91-1819">URL 比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-1819">URL matching</span></span>

<span data-ttu-id="6dd91-1820">URL 比對是路由用來將傳入要求分派給「端點」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1820">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="6dd91-1821">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1821">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="6dd91-1822">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1822">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="6dd91-1823">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1823">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="6dd91-1824">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1824">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="6dd91-1825">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1825">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="6dd91-1826">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1826">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="6dd91-1827">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1827">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="6dd91-1828">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1828">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="6dd91-1829">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1829"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="6dd91-1830">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1830">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="6dd91-1831">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1831">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="6dd91-1832">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1832">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="6dd91-1833">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1833">Routes can be nested inside of one another.</span></span> <span data-ttu-id="6dd91-1834"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1834">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="6dd91-1835">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1835">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="6dd91-1836"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1836">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="6dd91-1837">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="6dd91-1837">URL generation with LinkGenerator</span></span>

<span data-ttu-id="6dd91-1838">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1838">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="6dd91-1839">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1839">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="6dd91-1840">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1840">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="6dd91-1841"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>是可從[DI](xref:fundamentals/dependency-injection)抓取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1841"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from [DI](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6dd91-1842">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1842">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="6dd91-1843">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1843">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="6dd91-1844">連結產生器背後支援的概念為「位址」\*\* 和「位址配置」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1844">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="6dd91-1845">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1845">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="6dd91-1846">例如，許多使用者熟悉的路由名稱和路由值案例， Razor 都是以位址配置的形式來執行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1846">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="6dd91-1847">連結產生器可以透過下列擴充方法連結至 MVC/ Razor pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1847">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="6dd91-1848">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1848">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="6dd91-1849">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1849">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="6dd91-1850">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1850">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="6dd91-1851">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1851">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="6dd91-1852">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1852">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="6dd91-1853">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1853">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="6dd91-1854">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1854"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="6dd91-1855">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1855">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="6dd91-1856">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1856">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="6dd91-1857">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1857">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="6dd91-1858">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1858">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="6dd91-1859"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1859">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="6dd91-1860">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-1860">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="6dd91-1861">擴充方法</span><span class="sxs-lookup"><span data-stu-id="6dd91-1861">Extension Method</span></span>   | <span data-ttu-id="6dd91-1862">描述</span><span class="sxs-lookup"><span data-stu-id="6dd91-1862">Description</span></span>                                                         |
| ---
<span data-ttu-id="6dd91-1863">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1863">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1864">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1864">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1865">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1865">'Identity'</span></span>
- <span data-ttu-id="6dd91-1866">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1866">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1867">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1867">'Razor'</span></span>
- <span data-ttu-id="6dd91-1868">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1868">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1869">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1869">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1870">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1870">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1871">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1871">'Identity'</span></span>
- <span data-ttu-id="6dd91-1872">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1872">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1873">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1873">'Razor'</span></span>
- <span data-ttu-id="6dd91-1874">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1874">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1875">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1875">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1876">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1876">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1877">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1877">'Identity'</span></span>
- <span data-ttu-id="6dd91-1878">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1878">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1879">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1879">'Razor'</span></span>
- <span data-ttu-id="6dd91-1880">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1880">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1881">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1881">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1882">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1882">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1883">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1883">'Identity'</span></span>
- <span data-ttu-id="6dd91-1884">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1884">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1885">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1885">'Razor'</span></span>
- <span data-ttu-id="6dd91-1886">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1886">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1887">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1887">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1888">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1888">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1889">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1889">'Identity'</span></span>
- <span data-ttu-id="6dd91-1890">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1890">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1891">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1891">'Razor'</span></span>
- <span data-ttu-id="6dd91-1892">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1892">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1893">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1893">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1894">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1894">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1895">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1895">'Identity'</span></span>
- <span data-ttu-id="6dd91-1896">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1896">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1897">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1897">'Razor'</span></span>
- <span data-ttu-id="6dd91-1898">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1898">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1899">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1899">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1900">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1900">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1901">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1901">'Identity'</span></span>
- <span data-ttu-id="6dd91-1902">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1902">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1903">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1903">'Razor'</span></span>
- <span data-ttu-id="6dd91-1904">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1904">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-1905">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1905">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1906">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1906">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1907">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1907">'Identity'</span></span>
- <span data-ttu-id="6dd91-1908">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1908">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1909">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1909">'Razor'</span></span>
- <span data-ttu-id="6dd91-1910">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1910">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1911">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1911">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1912">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1912">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1913">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1913">'Identity'</span></span>
- <span data-ttu-id="6dd91-1914">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1914">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1915">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1915">'Razor'</span></span>
- <span data-ttu-id="6dd91-1916">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1916">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1917">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1917">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1918">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1918">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1919">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1919">'Identity'</span></span>
- <span data-ttu-id="6dd91-1920">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1920">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1921">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1921">'Razor'</span></span>
- <span data-ttu-id="6dd91-1922">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1922">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1923">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1923">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1924">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1924">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1925">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1925">'Identity'</span></span>
- <span data-ttu-id="6dd91-1926">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1926">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1927">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1927">'Razor'</span></span>
- <span data-ttu-id="6dd91-1928">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1928">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1929">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1929">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1930">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1930">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1931">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1931">'Identity'</span></span>
- <span data-ttu-id="6dd91-1932">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1932">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1933">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1933">'Razor'</span></span>
- <span data-ttu-id="6dd91-1934">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1934">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1935">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1935">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1936">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1936">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1937">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1937">'Identity'</span></span>
- <span data-ttu-id="6dd91-1938">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1938">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1939">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1939">'Razor'</span></span>
- <span data-ttu-id="6dd91-1940">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1940">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1941">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1941">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1942">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1942">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1943">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1943">'Identity'</span></span>
- <span data-ttu-id="6dd91-1944">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1944">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1945">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1945">'Razor'</span></span>
- <span data-ttu-id="6dd91-1946">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1946">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1947">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1947">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1948">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1948">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1949">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1949">'Identity'</span></span>
- <span data-ttu-id="6dd91-1950">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1950">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1951">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1951">'Razor'</span></span>
- <span data-ttu-id="6dd91-1952">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1952">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1953">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1953">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1954">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1954">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1955">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1955">'Identity'</span></span>
- <span data-ttu-id="6dd91-1956">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1956">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1957">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1957">'Razor'</span></span>
- <span data-ttu-id="6dd91-1958">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1958">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1959">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1959">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1960">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1960">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1961">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1961">'Identity'</span></span>
- <span data-ttu-id="6dd91-1962">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1962">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1963">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1963">'Razor'</span></span>
- <span data-ttu-id="6dd91-1964">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1964">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1965">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1965">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1966">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1966">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1967">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1967">'Identity'</span></span>
- <span data-ttu-id="6dd91-1968">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1968">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1969">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1969">'Razor'</span></span>
- <span data-ttu-id="6dd91-1970">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1970">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1971">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1971">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1972">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1972">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1973">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1973">'Identity'</span></span>
- <span data-ttu-id="6dd91-1974">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1974">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1975">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1975">'Razor'</span></span>
- <span data-ttu-id="6dd91-1976">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1976">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1977">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1977">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1978">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1978">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1979">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1979">'Identity'</span></span>
- <span data-ttu-id="6dd91-1980">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1980">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1981">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1981">'Razor'</span></span>
- <span data-ttu-id="6dd91-1982">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1982">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1983">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1983">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1984">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1984">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1985">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1985">'Identity'</span></span>
- <span data-ttu-id="6dd91-1986">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1986">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1987">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1987">'Razor'</span></span>
- <span data-ttu-id="6dd91-1988">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1988">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1989">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1989">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1990">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1990">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1991">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1991">'Identity'</span></span>
- <span data-ttu-id="6dd91-1992">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1992">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1993">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1993">'Razor'</span></span>
- <span data-ttu-id="6dd91-1994">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1994">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-1995">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-1995">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-1996">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1996">'Blazor'</span></span>
- <span data-ttu-id="6dd91-1997">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1997">'Identity'</span></span>
- <span data-ttu-id="6dd91-1998">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1998">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-1999">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-1999">'Razor'</span></span>
- <span data-ttu-id="6dd91-2000">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2000">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2001">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2001">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2002">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2002">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2003">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2003">'Identity'</span></span>
- <span data-ttu-id="6dd91-2004">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2004">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2005">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2005">'Razor'</span></span>
- <span data-ttu-id="6dd91-2006">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2006">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2007">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2007">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2008">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2008">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2009">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2009">'Identity'</span></span>
- <span data-ttu-id="6dd91-2010">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2010">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2011">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2011">'Razor'</span></span>
- <span data-ttu-id="6dd91-2012">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2012">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2013">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2013">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2014">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2014">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2015">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2015">'Identity'</span></span>
- <span data-ttu-id="6dd91-2016">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2016">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2017">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2017">'Razor'</span></span>
- <span data-ttu-id="6dd91-2018">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2018">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2019">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2019">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2020">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2020">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2021">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2021">'Identity'</span></span>
- <span data-ttu-id="6dd91-2022">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2022">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2023">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2023">'Razor'</span></span>
- <span data-ttu-id="6dd91-2024">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2024">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2025">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2025">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2026">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2026">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2027">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2027">'Identity'</span></span>
- <span data-ttu-id="6dd91-2028">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2028">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2029">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2029">'Razor'</span></span>
- <span data-ttu-id="6dd91-2030">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2030">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2031">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2031">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2032">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2032">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2033">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2033">'Identity'</span></span>
- <span data-ttu-id="6dd91-2034">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2034">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2035">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2035">'Razor'</span></span>
- <span data-ttu-id="6dd91-2036">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2036">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2037">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2037">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2038">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2038">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2039">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2039">'Identity'</span></span>
- <span data-ttu-id="6dd91-2040">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2040">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2041">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2041">'Razor'</span></span>
- <span data-ttu-id="6dd91-2042">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2042">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2043">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2043">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2044">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2044">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2045">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2045">'Identity'</span></span>
- <span data-ttu-id="6dd91-2046">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2046">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2047">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2047">'Razor'</span></span>
- <span data-ttu-id="6dd91-2048">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2048">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2049">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2049">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2050">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2050">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2051">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2051">'Identity'</span></span>
- <span data-ttu-id="6dd91-2052">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2052">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2053">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2053">'Razor'</span></span>
- <span data-ttu-id="6dd91-2054">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2054">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2055">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2055">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2056">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2056">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2057">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2057">'Identity'</span></span>
- <span data-ttu-id="6dd91-2058">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2058">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2059">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2059">'Razor'</span></span>
- <span data-ttu-id="6dd91-2060">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2060">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2061">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2061">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2062">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2062">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2063">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2063">'Identity'</span></span>
- <span data-ttu-id="6dd91-2064">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2064">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2065">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2065">'Razor'</span></span>
- <span data-ttu-id="6dd91-2066">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2066">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2067">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2067">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2068">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2068">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2069">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2069">'Identity'</span></span>
- <span data-ttu-id="6dd91-2070">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2070">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2071">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2071">'Razor'</span></span>
- <span data-ttu-id="6dd91-2072">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2072">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2073">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2073">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2074">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2074">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2075">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2075">'Identity'</span></span>
- <span data-ttu-id="6dd91-2076">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2076">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2077">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2077">'Razor'</span></span>
- <span data-ttu-id="6dd91-2078">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2078">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2079">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2079">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2080">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2080">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2081">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2081">'Identity'</span></span>
- <span data-ttu-id="6dd91-2082">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2082">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2083">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2083">'Razor'</span></span>
- <span data-ttu-id="6dd91-2084">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2084">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2085">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2085">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2086">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2086">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2087">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2087">'Identity'</span></span>
- <span data-ttu-id="6dd91-2088">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2088">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2089">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2089">'Razor'</span></span>
- <span data-ttu-id="6dd91-2090">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2090">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2091">---------------------------------- | |<xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> |根據提供的值，產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2091">---------------------------------- | | <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Generates a URI with an absolute path based on the provided values.</span></span> <span data-ttu-id="6dd91-2092">| |<xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> |根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2092">| | <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="6dd91-2093">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2093">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="6dd91-2094">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2094">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="6dd91-2095">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2095">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="6dd91-2096">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2096">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="6dd91-2097">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2097">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="6dd91-2098">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2098">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="6dd91-2099">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2099">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="6dd91-2100">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2100">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="6dd91-2101">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="6dd91-2101">Differences from earlier versions of routing</span></span>

<span data-ttu-id="6dd91-2102">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2102">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="6dd91-2103">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2103">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="6dd91-2104">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2104">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="6dd91-2105">請使用 2.1[相容性版本](xref:mvc/compatibility-version)（ `.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)` ）繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2105">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="6dd91-2106">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2106">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="6dd91-2107">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2107">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="6dd91-2108">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2108">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="6dd91-2109">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2109">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="6dd91-2110">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2110">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="6dd91-2111">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2111">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="6dd91-2112">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2112">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="6dd91-2113">Razor當嘗試連結至不存在的控制器/動作或頁面時，MVC/Pages 搭配傳統路由使用的連結產生會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2113">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="6dd91-2114">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2114">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="6dd91-2115">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2115">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="6dd91-2116">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2116">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="6dd91-2117">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2117">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="6dd91-2118">不過，如果動作不存在，則端點路由會產生空字串。\*\*</span><span class="sxs-lookup"><span data-stu-id="6dd91-2118">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="6dd91-2119">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2119">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="6dd91-2120">連結產生「環境值失效演算法」\*\* 在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2120">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="6dd91-2121">「環境值失效」\*\* 是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2121">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="6dd91-2122">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2122">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="6dd91-2123">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2123">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="6dd91-2124">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2124">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="6dd91-2125">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2125">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="6dd91-2126">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2126">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="6dd91-2127">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2127">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="6dd91-2128">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2128">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="6dd91-2129">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2129">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="6dd91-2130">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2130">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="6dd91-2131">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2131">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="6dd91-2132">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2132">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="6dd91-2133">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2133">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="6dd91-2134">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2134">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="6dd91-2135">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2135">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="6dd91-2136">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2136">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="6dd91-2137">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2137">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="6dd91-2138">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2138">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="6dd91-2139">路由</span><span class="sxs-lookup"><span data-stu-id="6dd91-2139">Route</span></span>              | <span data-ttu-id="6dd91-2140">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2140">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ---
<span data-ttu-id="6dd91-2141">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2141">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2142">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2142">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2143">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2143">'Identity'</span></span>
- <span data-ttu-id="6dd91-2144">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2144">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2145">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2145">'Razor'</span></span>
- <span data-ttu-id="6dd91-2146">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2146">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2147">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2147">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2148">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2148">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2149">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2149">'Identity'</span></span>
- <span data-ttu-id="6dd91-2150">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2150">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2151">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2151">'Razor'</span></span>
- <span data-ttu-id="6dd91-2152">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2152">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2153">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2153">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2154">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2154">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2155">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2155">'Identity'</span></span>
- <span data-ttu-id="6dd91-2156">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2156">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2157">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2157">'Razor'</span></span>
- <span data-ttu-id="6dd91-2158">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2158">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2159">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2159">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2160">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2160">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2161">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2161">'Identity'</span></span>
- <span data-ttu-id="6dd91-2162">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2162">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2163">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2163">'Razor'</span></span>
- <span data-ttu-id="6dd91-2164">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2164">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2165">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2165">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2166">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2166">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2167">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2167">'Identity'</span></span>
- <span data-ttu-id="6dd91-2168">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2168">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2169">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2169">'Razor'</span></span>
- <span data-ttu-id="6dd91-2170">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2170">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2171">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2171">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2172">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2172">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2173">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2173">'Identity'</span></span>
- <span data-ttu-id="6dd91-2174">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2174">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2175">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2175">'Razor'</span></span>
- <span data-ttu-id="6dd91-2176">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2176">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2177">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2177">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2178">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2179">'Identity'</span></span>
- <span data-ttu-id="6dd91-2180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2180">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2181">'Razor'</span></span>
- <span data-ttu-id="6dd91-2182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2182">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2183">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2183">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2184">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2184">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2185">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2185">'Identity'</span></span>
- <span data-ttu-id="6dd91-2186">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2186">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2187">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2187">'Razor'</span></span>
- <span data-ttu-id="6dd91-2188">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2188">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2189">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2189">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2190">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2190">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2191">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2191">'Identity'</span></span>
- <span data-ttu-id="6dd91-2192">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2192">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2193">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2193">'Razor'</span></span>
- <span data-ttu-id="6dd91-2194">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2194">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2195">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2195">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2196">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2196">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2197">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2197">'Identity'</span></span>
- <span data-ttu-id="6dd91-2198">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2198">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2199">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2199">'Razor'</span></span>
- <span data-ttu-id="6dd91-2200">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2200">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2201">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2201">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2202">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2202">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2203">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2203">'Identity'</span></span>
- <span data-ttu-id="6dd91-2204">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2204">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2205">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2205">'Razor'</span></span>
- <span data-ttu-id="6dd91-2206">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2206">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2207">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2207">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2208">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2208">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2209">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2209">'Identity'</span></span>
- <span data-ttu-id="6dd91-2210">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2210">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2211">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2211">'Razor'</span></span>
- <span data-ttu-id="6dd91-2212">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2212">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2213">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2213">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2214">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2214">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2215">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2215">'Identity'</span></span>
- <span data-ttu-id="6dd91-2216">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2216">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2217">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2217">'Razor'</span></span>
- <span data-ttu-id="6dd91-2218">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2218">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2219">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2219">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2220">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2221">'Identity'</span></span>
- <span data-ttu-id="6dd91-2222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2222">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2223">'Razor'</span></span>
- <span data-ttu-id="6dd91-2224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2225">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2225">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2226">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2226">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2227">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2227">'Identity'</span></span>
- <span data-ttu-id="6dd91-2228">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2228">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2229">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2229">'Razor'</span></span>
- <span data-ttu-id="6dd91-2230">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2230">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2231">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2231">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2232">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2232">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2233">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2233">'Identity'</span></span>
- <span data-ttu-id="6dd91-2234">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2234">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2235">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2235">'Razor'</span></span>
- <span data-ttu-id="6dd91-2236">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2236">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2237">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2237">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2238">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2238">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2239">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2239">'Identity'</span></span>
- <span data-ttu-id="6dd91-2240">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2240">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2241">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2241">'Razor'</span></span>
- <span data-ttu-id="6dd91-2242">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2242">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2243">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2243">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2244">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2244">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2245">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2245">'Identity'</span></span>
- <span data-ttu-id="6dd91-2246">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2246">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2247">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2247">'Razor'</span></span>
- <span data-ttu-id="6dd91-2248">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2248">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2249">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2249">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2250">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2250">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2251">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2251">'Identity'</span></span>
- <span data-ttu-id="6dd91-2252">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2252">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2253">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2253">'Razor'</span></span>
- <span data-ttu-id="6dd91-2254">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2254">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2255">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2255">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2256">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2256">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2257">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2257">'Identity'</span></span>
- <span data-ttu-id="6dd91-2258">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2258">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2259">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2259">'Razor'</span></span>
- <span data-ttu-id="6dd91-2260">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2260">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2261">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2261">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2262">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2263">'Identity'</span></span>
- <span data-ttu-id="6dd91-2264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2264">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2265">'Razor'</span></span>
- <span data-ttu-id="6dd91-2266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2266">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2267">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2267">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2268">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2268">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2269">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2269">'Identity'</span></span>
- <span data-ttu-id="6dd91-2270">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2270">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2271">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2271">'Razor'</span></span>
- <span data-ttu-id="6dd91-2272">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2272">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2273">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2273">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2274">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2274">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2275">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2275">'Identity'</span></span>
- <span data-ttu-id="6dd91-2276">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2276">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2277">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2277">'Razor'</span></span>
- <span data-ttu-id="6dd91-2278">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2278">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2279">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2279">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2280">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2280">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2281">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2281">'Identity'</span></span>
- <span data-ttu-id="6dd91-2282">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2282">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2283">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2283">'Razor'</span></span>
- <span data-ttu-id="6dd91-2284">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2284">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2285">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2285">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2286">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2286">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2287">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2287">'Identity'</span></span>
- <span data-ttu-id="6dd91-2288">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2288">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2289">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2289">'Razor'</span></span>
- <span data-ttu-id="6dd91-2290">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2290">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2291">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2291">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2292">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2292">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2293">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2293">'Identity'</span></span>
- <span data-ttu-id="6dd91-2294">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2294">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2295">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2295">'Razor'</span></span>
- <span data-ttu-id="6dd91-2296">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2296">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2297">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2297">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2298">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2298">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2299">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2299">'Identity'</span></span>
- <span data-ttu-id="6dd91-2300">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2300">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2301">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2301">'Razor'</span></span>
- <span data-ttu-id="6dd91-2302">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2302">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2303">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2303">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2304">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2305">'Identity'</span></span>
- <span data-ttu-id="6dd91-2306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2306">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2307">'Razor'</span></span>
- <span data-ttu-id="6dd91-2308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2308">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2309">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2309">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2310">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2310">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2311">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2311">'Identity'</span></span>
- <span data-ttu-id="6dd91-2312">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2312">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2313">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2313">'Razor'</span></span>
- <span data-ttu-id="6dd91-2314">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2314">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2315">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2315">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2316">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2316">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2317">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2317">'Identity'</span></span>
- <span data-ttu-id="6dd91-2318">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2318">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2319">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2319">'Razor'</span></span>
- <span data-ttu-id="6dd91-2320">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2320">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2321">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2321">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2322">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2322">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2323">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2323">'Identity'</span></span>
- <span data-ttu-id="6dd91-2324">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2324">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2325">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2325">'Razor'</span></span>
- <span data-ttu-id="6dd91-2326">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2326">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2327">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2327">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2328">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2328">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2329">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2329">'Identity'</span></span>
- <span data-ttu-id="6dd91-2330">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2330">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2331">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2331">'Razor'</span></span>
- <span data-ttu-id="6dd91-2332">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2332">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2333">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2333">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2334">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2335">'Identity'</span></span>
- <span data-ttu-id="6dd91-2336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2336">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2337">'Razor'</span></span>
- <span data-ttu-id="6dd91-2338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2339">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2339">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2340">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2341">'Identity'</span></span>
- <span data-ttu-id="6dd91-2342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2342">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2343">'Razor'</span></span>
- <span data-ttu-id="6dd91-2344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2346">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2347">'Identity'</span></span>
- <span data-ttu-id="6dd91-2348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2348">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2349">'Razor'</span></span>
- <span data-ttu-id="6dd91-2350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2352">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2353">'Identity'</span></span>
- <span data-ttu-id="6dd91-2354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2354">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2355">'Razor'</span></span>
- <span data-ttu-id="6dd91-2356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2357">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2357">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2358">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2359">'Identity'</span></span>
- <span data-ttu-id="6dd91-2360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2360">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2361">'Razor'</span></span>
- <span data-ttu-id="6dd91-2362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2362">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2363">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2363">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2364">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2364">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2365">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2365">'Identity'</span></span>
- <span data-ttu-id="6dd91-2366">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2366">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2367">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2367">'Razor'</span></span>
- <span data-ttu-id="6dd91-2368">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2368">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2369">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2369">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2370">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2370">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2371">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2371">'Identity'</span></span>
- <span data-ttu-id="6dd91-2372">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2372">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2373">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2373">'Razor'</span></span>
- <span data-ttu-id="6dd91-2374">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2374">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2375">----------------------------------- |  | `/search/{*page}`|`/search/admin%2Fproducts`（正斜線已編碼） |  | `/search/{**page}` |  `/search/admin/products`                                              |</span><span class="sxs-lookup"><span data-stu-id="6dd91-2375">----------------------------------- |   | `/search/{*page}`  | `/search/admin%2Fproducts` (the forward slash is encoded)             |   | `/search/{**page}` | `/search/admin/products`                                              |</span></span>

### <a name="middleware-example"></a><span data-ttu-id="6dd91-2376">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="6dd91-2376">Middleware example</span></span>

<span data-ttu-id="6dd91-2377">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2377">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="6dd91-2378">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2378">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

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

### <a name="create-routes"></a><span data-ttu-id="6dd91-2379">建立路由</span><span class="sxs-lookup"><span data-stu-id="6dd91-2379">Create routes</span></span>

<span data-ttu-id="6dd91-2380">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2380">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="6dd91-2381">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2381">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="6dd91-2382"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2382"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="6dd91-2383"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2383"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="6dd91-2384">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2384">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="6dd91-2385">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2385">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6dd91-2386">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2386">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="6dd91-2387">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2387">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="6dd91-2388">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2388">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="6dd91-2389">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2389">Route parameters are named.</span></span> <span data-ttu-id="6dd91-2390">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2390">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="6dd91-2391">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2391">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="6dd91-2392">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2392">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="6dd91-2393">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2393">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="6dd91-2394">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2394">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="6dd91-2395">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2395">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="6dd91-2396">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2396">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="6dd91-2397">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2397">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="6dd91-2398">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2398">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="6dd91-2399">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2399">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="6dd91-2400">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2400">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="6dd91-2401">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2401">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="6dd91-2402">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2402">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="6dd91-2403"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2403">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="6dd91-2404">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2404">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="6dd91-2405">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2405">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="6dd91-2406">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2406">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="6dd91-2407">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2407">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="6dd91-2408">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2408">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="6dd91-2409">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2409">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="6dd91-2410">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2410">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="6dd91-2411">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2411">Default values can be specified in the route template.</span></span> <span data-ttu-id="6dd91-2412">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2412">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="6dd91-2413">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2413">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="6dd91-2414">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2414">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="6dd91-2415">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2415">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="6dd91-2417">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="6dd91-2417">Route class URL generation</span></span>

<span data-ttu-id="6dd91-2418"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2418">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="6dd91-2419">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2419">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="6dd91-2420">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2420">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="6dd91-2421">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="6dd91-2421">What values would be produced?</span></span> <span data-ttu-id="6dd91-2422">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2422">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="6dd91-2423">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2423">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6dd91-2424">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2424">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="6dd91-2425">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2425">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="6dd91-2426">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2426">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="6dd91-2427">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2427">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="6dd91-2428">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2428">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="6dd91-2429">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2429">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="6dd91-2430">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2430">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="6dd91-2431">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2431">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="6dd91-2432">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="6dd91-2432">Use Routing Middleware</span></span>

<span data-ttu-id="6dd91-2433">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2433">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="6dd91-2434">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2434">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="6dd91-2435">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2435">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="6dd91-2436">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2436">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="6dd91-2437"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>：僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2437"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>: Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="6dd91-2438">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2438">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="6dd91-2439">URI</span><span class="sxs-lookup"><span data-stu-id="6dd91-2439">URI</span></span>                    | <span data-ttu-id="6dd91-2440">回應</span><span class="sxs-lookup"><span data-stu-id="6dd91-2440">Response</span></span>                                          |
| ---
<span data-ttu-id="6dd91-2441">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2441">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2442">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2442">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2443">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2443">'Identity'</span></span>
- <span data-ttu-id="6dd91-2444">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2444">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2445">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2445">'Razor'</span></span>
- <span data-ttu-id="6dd91-2446">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2446">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2447">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2447">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2448">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2448">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2449">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2449">'Identity'</span></span>
- <span data-ttu-id="6dd91-2450">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2450">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2451">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2451">'Razor'</span></span>
- <span data-ttu-id="6dd91-2452">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2452">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2453">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2453">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2454">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2454">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2455">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2455">'Identity'</span></span>
- <span data-ttu-id="6dd91-2456">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2456">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2457">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2457">'Razor'</span></span>
- <span data-ttu-id="6dd91-2458">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2458">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2459">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2459">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2460">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2460">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2461">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2461">'Identity'</span></span>
- <span data-ttu-id="6dd91-2462">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2462">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2463">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2463">'Razor'</span></span>
- <span data-ttu-id="6dd91-2464">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2464">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2466">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2467">'Identity'</span></span>
- <span data-ttu-id="6dd91-2468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2468">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2469">'Razor'</span></span>
- <span data-ttu-id="6dd91-2470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2472">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2473">'Identity'</span></span>
- <span data-ttu-id="6dd91-2474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2474">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2475">'Razor'</span></span>
- <span data-ttu-id="6dd91-2476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2476">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2477">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2477">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2478">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2479">'Identity'</span></span>
- <span data-ttu-id="6dd91-2480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2480">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2481">'Razor'</span></span>
- <span data-ttu-id="6dd91-2482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2484">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2485">'Identity'</span></span>
- <span data-ttu-id="6dd91-2486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2486">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2487">'Razor'</span></span>
- <span data-ttu-id="6dd91-2488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2490">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2491">'Identity'</span></span>
- <span data-ttu-id="6dd91-2492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2492">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2493">'Razor'</span></span>
- <span data-ttu-id="6dd91-2494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2494">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2495">----------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2495">----------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2496">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2497">'Identity'</span></span>
- <span data-ttu-id="6dd91-2498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2498">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2499">'Razor'</span></span>
- <span data-ttu-id="6dd91-2500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2502">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2503">'Identity'</span></span>
- <span data-ttu-id="6dd91-2504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2504">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2505">'Razor'</span></span>
- <span data-ttu-id="6dd91-2506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2506">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2507">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2507">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2508">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2509">'Identity'</span></span>
- <span data-ttu-id="6dd91-2510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2510">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2511">'Razor'</span></span>
- <span data-ttu-id="6dd91-2512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2512">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2513">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2513">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2514">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2514">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2515">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2515">'Identity'</span></span>
- <span data-ttu-id="6dd91-2516">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2516">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2517">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2517">'Razor'</span></span>
- <span data-ttu-id="6dd91-2518">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2518">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2519">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2519">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2520">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2520">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2521">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2521">'Identity'</span></span>
- <span data-ttu-id="6dd91-2522">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2522">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2523">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2523">'Razor'</span></span>
- <span data-ttu-id="6dd91-2524">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2524">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2525">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2525">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2526">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2526">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2527">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2527">'Identity'</span></span>
- <span data-ttu-id="6dd91-2528">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2528">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2529">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2529">'Razor'</span></span>
- <span data-ttu-id="6dd91-2530">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2530">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2531">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2531">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2532">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2532">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2533">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2533">'Identity'</span></span>
- <span data-ttu-id="6dd91-2534">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2534">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2535">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2535">'Razor'</span></span>
- <span data-ttu-id="6dd91-2536">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2536">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2537">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2537">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2538">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2539">'Identity'</span></span>
- <span data-ttu-id="6dd91-2540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2540">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2541">'Razor'</span></span>
- <span data-ttu-id="6dd91-2542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2544">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2545">'Identity'</span></span>
- <span data-ttu-id="6dd91-2546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2546">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2547">'Razor'</span></span>
- <span data-ttu-id="6dd91-2548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2550">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2551">'Identity'</span></span>
- <span data-ttu-id="6dd91-2552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2552">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2553">'Razor'</span></span>
- <span data-ttu-id="6dd91-2554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2556">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2557">'Identity'</span></span>
- <span data-ttu-id="6dd91-2558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2558">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2559">'Razor'</span></span>
- <span data-ttu-id="6dd91-2560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2562">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2563">'Identity'</span></span>
- <span data-ttu-id="6dd91-2564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2564">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2565">'Razor'</span></span>
- <span data-ttu-id="6dd91-2566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2568">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2569">'Identity'</span></span>
- <span data-ttu-id="6dd91-2570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2570">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2571">'Razor'</span></span>
- <span data-ttu-id="6dd91-2572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2574">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2575">'Identity'</span></span>
- <span data-ttu-id="6dd91-2576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2576">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2577">'Razor'</span></span>
- <span data-ttu-id="6dd91-2578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2578">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2579">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2579">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2580">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2581">'Identity'</span></span>
- <span data-ttu-id="6dd91-2582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2582">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2583">'Razor'</span></span>
- <span data-ttu-id="6dd91-2584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2584">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2585">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2585">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2586">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2586">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2587">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2587">'Identity'</span></span>
- <span data-ttu-id="6dd91-2588">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2588">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2589">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2589">'Razor'</span></span>
- <span data-ttu-id="6dd91-2590">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2590">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2591">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2591">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2592">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2592">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2593">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2593">'Identity'</span></span>
- <span data-ttu-id="6dd91-2594">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2594">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2595">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2595">'Razor'</span></span>
- <span data-ttu-id="6dd91-2596">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2596">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2597">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2597">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2598">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2598">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2599">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2599">'Identity'</span></span>
- <span data-ttu-id="6dd91-2600">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2600">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2601">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2601">'Razor'</span></span>
- <span data-ttu-id="6dd91-2602">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2602">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2604">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2605">'Identity'</span></span>
- <span data-ttu-id="6dd91-2606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2606">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2607">'Razor'</span></span>
- <span data-ttu-id="6dd91-2608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2608">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2609">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2609">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2610">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2611">'Identity'</span></span>
- <span data-ttu-id="6dd91-2612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2612">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2613">'Razor'</span></span>
- <span data-ttu-id="6dd91-2614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2614">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2615">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2615">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2616">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2617">'Identity'</span></span>
- <span data-ttu-id="6dd91-2618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2618">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2619">'Razor'</span></span>
- <span data-ttu-id="6dd91-2620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2622">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2623">'Identity'</span></span>
- <span data-ttu-id="6dd91-2624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2624">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2625">'Razor'</span></span>
- <span data-ttu-id="6dd91-2626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2626">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2627">------------------------- | |`/package/create/3`    |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-2627">------------------------- | | `/package/create/3`    | Hello!</span></span> <span data-ttu-id="6dd91-2628">路由值： [operation，create]，[id，3] | |`/package/track/-3`    |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-2628">Route values: [operation, create], [id, 3] | | `/package/track/-3`    | Hello!</span></span> <span data-ttu-id="6dd91-2629">路由值： [operation，track]，[id，-3] | |`/package/track/-3/`   |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-2629">Route values: [operation, track], [id, -3] | | `/package/track/-3/`   | Hello!</span></span> <span data-ttu-id="6dd91-2630">路由值： [operation，track]，[id，-3] | |`/package/track/`      |要求已通過，沒有相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2630">Route values: [operation, track], [id, -3] | | `/package/track/`      | The request falls through, no match.</span></span>              <span data-ttu-id="6dd91-2631">| |`GET /hello/Joe`       |Joe！</span><span class="sxs-lookup"><span data-stu-id="6dd91-2631">| | `GET /hello/Joe`       | Hi, Joe!</span></span>                                          <span data-ttu-id="6dd91-2632">| |`POST /hello/Joe`      |要求會通過，只符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2632">| | `POST /hello/Joe`      | The request falls through, matches HTTP GET only.</span></span> <span data-ttu-id="6dd91-2633">| |`GET /hello/Joe/Smith` |要求已通過，沒有相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2633">| | `GET /hello/Joe/Smith` | The request falls through, no match.</span></span>              |

<span data-ttu-id="6dd91-2634">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2634">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="6dd91-2635">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2635">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="6dd91-2636">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2636">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="6dd91-2637">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-2637">Route template reference</span></span>

<span data-ttu-id="6dd91-2638">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2638">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="6dd91-2639">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2639">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="6dd91-2640">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2640">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="6dd91-2641">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2641">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="6dd91-2642">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2642">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="6dd91-2643">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2643">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="6dd91-2644">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2644">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="6dd91-2645">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2645">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="6dd91-2646">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2646">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="6dd91-2647">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2647">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="6dd91-2648">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2648">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="6dd91-2649">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2649">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="6dd91-2650">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2650">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="6dd91-2651">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2651">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="6dd91-2652">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2652">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="6dd91-2653">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2653">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="6dd91-2654">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2654">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="6dd91-2655">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2655">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="6dd91-2656">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2656">Note the escaped forward slash.</span></span> <span data-ttu-id="6dd91-2657">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2657">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="6dd91-2658">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2658">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="6dd91-2659">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2659">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="6dd91-2660">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2660">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="6dd91-2661">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2661">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="6dd91-2662">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2662">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="6dd91-2663">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2663">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="6dd91-2664">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2664">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="6dd91-2665">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2665">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="6dd91-2666">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2666">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="6dd91-2667">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2667">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="6dd91-2668">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2668">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="6dd91-2669">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2669">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="6dd91-2670">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2670">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="6dd91-2671">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2671">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="6dd91-2672">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2672">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="6dd91-2673">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2673">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="6dd91-2674">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2674">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="6dd91-2675">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-2675">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="6dd91-2676">路由範本</span><span class="sxs-lookup"><span data-stu-id="6dd91-2676">Route Template</span></span>                           | <span data-ttu-id="6dd91-2677">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="6dd91-2677">Example Matching URI</span></span>    | <span data-ttu-id="6dd91-2678">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="6dd91-2678">The request URI&hellip;</span></span>                                                    |
| ---
<span data-ttu-id="6dd91-2679">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2679">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2680">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2680">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2681">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2681">'Identity'</span></span>
- <span data-ttu-id="6dd91-2682">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2682">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2683">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2683">'Razor'</span></span>
- <span data-ttu-id="6dd91-2684">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2684">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2685">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2685">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2686">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2686">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2687">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2687">'Identity'</span></span>
- <span data-ttu-id="6dd91-2688">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2688">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2689">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2689">'Razor'</span></span>
- <span data-ttu-id="6dd91-2690">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2690">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2691">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2691">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2692">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2692">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2693">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2693">'Identity'</span></span>
- <span data-ttu-id="6dd91-2694">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2694">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2695">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2695">'Razor'</span></span>
- <span data-ttu-id="6dd91-2696">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2696">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2697">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2697">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2698">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2698">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2699">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2699">'Identity'</span></span>
- <span data-ttu-id="6dd91-2700">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2700">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2701">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2701">'Razor'</span></span>
- <span data-ttu-id="6dd91-2702">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2702">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2703">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2703">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2704">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2704">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2705">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2705">'Identity'</span></span>
- <span data-ttu-id="6dd91-2706">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2706">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2707">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2707">'Razor'</span></span>
- <span data-ttu-id="6dd91-2708">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2708">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2709">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2709">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2710">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2710">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2711">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2711">'Identity'</span></span>
- <span data-ttu-id="6dd91-2712">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2712">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2713">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2713">'Razor'</span></span>
- <span data-ttu-id="6dd91-2714">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2714">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2715">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2715">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2716">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2716">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2717">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2717">'Identity'</span></span>
- <span data-ttu-id="6dd91-2718">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2718">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2719">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2719">'Razor'</span></span>
- <span data-ttu-id="6dd91-2720">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2720">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2721">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2721">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2722">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2722">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2723">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2723">'Identity'</span></span>
- <span data-ttu-id="6dd91-2724">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2724">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2725">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2725">'Razor'</span></span>
- <span data-ttu-id="6dd91-2726">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2726">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2727">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2727">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2728">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2728">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2729">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2729">'Identity'</span></span>
- <span data-ttu-id="6dd91-2730">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2730">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2731">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2731">'Razor'</span></span>
- <span data-ttu-id="6dd91-2732">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2732">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2733">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2733">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2734">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2734">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2735">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2735">'Identity'</span></span>
- <span data-ttu-id="6dd91-2736">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2736">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2737">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2737">'Razor'</span></span>
- <span data-ttu-id="6dd91-2738">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2738">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2739">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2739">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2740">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2740">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2741">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2741">'Identity'</span></span>
- <span data-ttu-id="6dd91-2742">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2742">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2743">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2743">'Razor'</span></span>
- <span data-ttu-id="6dd91-2744">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2744">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2745">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2745">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2746">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2746">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2747">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2747">'Identity'</span></span>
- <span data-ttu-id="6dd91-2748">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2748">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2749">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2749">'Razor'</span></span>
- <span data-ttu-id="6dd91-2750">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2750">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2751">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2751">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2752">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2752">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2753">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2753">'Identity'</span></span>
- <span data-ttu-id="6dd91-2754">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2754">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2755">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2755">'Razor'</span></span>
- <span data-ttu-id="6dd91-2756">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2756">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2757">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2757">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2758">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2758">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2759">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2759">'Identity'</span></span>
- <span data-ttu-id="6dd91-2760">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2760">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2761">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2761">'Razor'</span></span>
- <span data-ttu-id="6dd91-2762">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2762">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2763">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2763">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2764">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2764">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2765">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2765">'Identity'</span></span>
- <span data-ttu-id="6dd91-2766">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2766">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2767">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2767">'Razor'</span></span>
- <span data-ttu-id="6dd91-2768">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2768">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2769">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2769">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2770">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2770">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2771">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2771">'Identity'</span></span>
- <span data-ttu-id="6dd91-2772">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2772">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2773">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2773">'Razor'</span></span>
- <span data-ttu-id="6dd91-2774">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2774">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2775">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2775">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2776">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2776">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2777">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2777">'Identity'</span></span>
- <span data-ttu-id="6dd91-2778">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2778">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2779">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2779">'Razor'</span></span>
- <span data-ttu-id="6dd91-2780">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2780">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2781">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2781">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2782">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2782">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2783">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2783">'Identity'</span></span>
- <span data-ttu-id="6dd91-2784">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2784">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2785">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2785">'Razor'</span></span>
- <span data-ttu-id="6dd91-2786">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2786">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2787">-------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2787">-------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2788">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2788">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2789">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2789">'Identity'</span></span>
- <span data-ttu-id="6dd91-2790">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2790">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2791">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2791">'Razor'</span></span>
- <span data-ttu-id="6dd91-2792">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2792">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2793">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2793">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2794">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2794">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2795">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2795">'Identity'</span></span>
- <span data-ttu-id="6dd91-2796">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2796">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2797">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2797">'Razor'</span></span>
- <span data-ttu-id="6dd91-2798">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2798">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2799">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2799">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2800">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2800">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2801">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2801">'Identity'</span></span>
- <span data-ttu-id="6dd91-2802">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2802">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2803">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2803">'Razor'</span></span>
- <span data-ttu-id="6dd91-2804">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2804">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2805">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2805">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2806">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2806">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2807">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2807">'Identity'</span></span>
- <span data-ttu-id="6dd91-2808">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2808">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2809">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2809">'Razor'</span></span>
- <span data-ttu-id="6dd91-2810">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2810">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2811">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2811">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2812">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2812">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2813">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2813">'Identity'</span></span>
- <span data-ttu-id="6dd91-2814">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2814">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2815">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2815">'Razor'</span></span>
- <span data-ttu-id="6dd91-2816">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2816">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2817">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2817">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2818">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2818">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2819">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2819">'Identity'</span></span>
- <span data-ttu-id="6dd91-2820">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2820">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2821">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2821">'Razor'</span></span>
- <span data-ttu-id="6dd91-2822">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2822">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2823">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2823">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2824">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2824">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2825">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2825">'Identity'</span></span>
- <span data-ttu-id="6dd91-2826">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2826">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2827">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2827">'Razor'</span></span>
- <span data-ttu-id="6dd91-2828">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2828">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2829">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2829">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2830">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2830">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2831">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2831">'Identity'</span></span>
- <span data-ttu-id="6dd91-2832">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2832">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2833">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2833">'Razor'</span></span>
- <span data-ttu-id="6dd91-2834">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2834">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2835">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2835">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2836">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2836">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2837">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2837">'Identity'</span></span>
- <span data-ttu-id="6dd91-2838">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2838">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2839">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2839">'Razor'</span></span>
- <span data-ttu-id="6dd91-2840">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2840">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-2841">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2841">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2842">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2842">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2843">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2843">'Identity'</span></span>
- <span data-ttu-id="6dd91-2844">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2844">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2845">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2845">'Razor'</span></span>
- <span data-ttu-id="6dd91-2846">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2846">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2847">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2847">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2848">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2848">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2849">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2849">'Identity'</span></span>
- <span data-ttu-id="6dd91-2850">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2850">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2851">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2851">'Razor'</span></span>
- <span data-ttu-id="6dd91-2852">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2852">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2853">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2853">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2854">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2854">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2855">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2855">'Identity'</span></span>
- <span data-ttu-id="6dd91-2856">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2856">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2857">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2857">'Razor'</span></span>
- <span data-ttu-id="6dd91-2858">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2858">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2859">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2859">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2860">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2860">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2861">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2861">'Identity'</span></span>
- <span data-ttu-id="6dd91-2862">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2862">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2863">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2863">'Razor'</span></span>
- <span data-ttu-id="6dd91-2864">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2864">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2865">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2865">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2866">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2866">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2867">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2867">'Identity'</span></span>
- <span data-ttu-id="6dd91-2868">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2868">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2869">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2869">'Razor'</span></span>
- <span data-ttu-id="6dd91-2870">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2870">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2871">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2871">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2872">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2872">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2873">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2873">'Identity'</span></span>
- <span data-ttu-id="6dd91-2874">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2874">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2875">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2875">'Razor'</span></span>
- <span data-ttu-id="6dd91-2876">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2876">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2877">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2877">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2878">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2878">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2879">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2879">'Identity'</span></span>
- <span data-ttu-id="6dd91-2880">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2880">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2881">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2881">'Razor'</span></span>
- <span data-ttu-id="6dd91-2882">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2882">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2883">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2883">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2884">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2884">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2885">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2885">'Identity'</span></span>
- <span data-ttu-id="6dd91-2886">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2886">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2887">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2887">'Razor'</span></span>
- <span data-ttu-id="6dd91-2888">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2888">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2889">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2889">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2890">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2890">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2891">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2891">'Identity'</span></span>
- <span data-ttu-id="6dd91-2892">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2892">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2893">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2893">'Razor'</span></span>
- <span data-ttu-id="6dd91-2894">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2894">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2895">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2895">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2896">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2896">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2897">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2897">'Identity'</span></span>
- <span data-ttu-id="6dd91-2898">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2898">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2899">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2899">'Razor'</span></span>
- <span data-ttu-id="6dd91-2900">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2900">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2901">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2901">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2902">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2902">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2903">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2903">'Identity'</span></span>
- <span data-ttu-id="6dd91-2904">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2904">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2905">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2905">'Razor'</span></span>
- <span data-ttu-id="6dd91-2906">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2906">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2907">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2907">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2908">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2908">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2909">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2909">'Identity'</span></span>
- <span data-ttu-id="6dd91-2910">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2910">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2911">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2911">'Razor'</span></span>
- <span data-ttu-id="6dd91-2912">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2912">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2913">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2913">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2914">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2914">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2915">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2915">'Identity'</span></span>
- <span data-ttu-id="6dd91-2916">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2916">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2917">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2917">'Razor'</span></span>
- <span data-ttu-id="6dd91-2918">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2918">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2919">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2919">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2920">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2920">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2921">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2921">'Identity'</span></span>
- <span data-ttu-id="6dd91-2922">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2922">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2923">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2923">'Razor'</span></span>
- <span data-ttu-id="6dd91-2924">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2924">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2925">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2925">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2926">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2926">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2927">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2927">'Identity'</span></span>
- <span data-ttu-id="6dd91-2928">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2928">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2929">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2929">'Razor'</span></span>
- <span data-ttu-id="6dd91-2930">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2930">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2931">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2931">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2932">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2932">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2933">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2933">'Identity'</span></span>
- <span data-ttu-id="6dd91-2934">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2934">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2935">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2935">'Razor'</span></span>
- <span data-ttu-id="6dd91-2936">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2936">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2937">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2937">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2938">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2938">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2939">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2939">'Identity'</span></span>
- <span data-ttu-id="6dd91-2940">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2940">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2941">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2941">'Razor'</span></span>
- <span data-ttu-id="6dd91-2942">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2942">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2943">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2943">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2944">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2944">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2945">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2945">'Identity'</span></span>
- <span data-ttu-id="6dd91-2946">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2946">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2947">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2947">'Razor'</span></span>
- <span data-ttu-id="6dd91-2948">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2948">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2949">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2949">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2950">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2950">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2951">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2951">'Identity'</span></span>
- <span data-ttu-id="6dd91-2952">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2952">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2953">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2953">'Razor'</span></span>
- <span data-ttu-id="6dd91-2954">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2954">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2955">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2955">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2956">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2956">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2957">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2957">'Identity'</span></span>
- <span data-ttu-id="6dd91-2958">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2958">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2959">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2959">'Razor'</span></span>
- <span data-ttu-id="6dd91-2960">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2960">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2961">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2961">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2962">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2962">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2963">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2963">'Identity'</span></span>
- <span data-ttu-id="6dd91-2964">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2964">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2965">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2965">'Razor'</span></span>
- <span data-ttu-id="6dd91-2966">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2966">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2967">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2967">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2968">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2968">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2969">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2969">'Identity'</span></span>
- <span data-ttu-id="6dd91-2970">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2970">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2971">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2971">'Razor'</span></span>
- <span data-ttu-id="6dd91-2972">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2972">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2973">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2973">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2974">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2974">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2975">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2975">'Identity'</span></span>
- <span data-ttu-id="6dd91-2976">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2976">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2977">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2977">'Razor'</span></span>
- <span data-ttu-id="6dd91-2978">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2978">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2979">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2979">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2980">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2980">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2981">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2981">'Identity'</span></span>
- <span data-ttu-id="6dd91-2982">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2982">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2983">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2983">'Razor'</span></span>
- <span data-ttu-id="6dd91-2984">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2984">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2985">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2985">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2986">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2986">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2987">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2987">'Identity'</span></span>
- <span data-ttu-id="6dd91-2988">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2988">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2989">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2989">'Razor'</span></span>
- <span data-ttu-id="6dd91-2990">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2990">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2991">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2991">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2992">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2992">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2993">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2993">'Identity'</span></span>
- <span data-ttu-id="6dd91-2994">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2994">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-2995">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2995">'Razor'</span></span>
- <span data-ttu-id="6dd91-2996">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2996">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-2997">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-2997">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-2998">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2998">'Blazor'</span></span>
- <span data-ttu-id="6dd91-2999">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-2999">'Identity'</span></span>
- <span data-ttu-id="6dd91-3000">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3000">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3001">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3001">'Razor'</span></span>
- <span data-ttu-id="6dd91-3002">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3002">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3003">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3003">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3004">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3004">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3005">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3005">'Identity'</span></span>
- <span data-ttu-id="6dd91-3006">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3006">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3007">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3007">'Razor'</span></span>
- <span data-ttu-id="6dd91-3008">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3008">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3009">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3009">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3010">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3010">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3011">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3011">'Identity'</span></span>
- <span data-ttu-id="6dd91-3012">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3012">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3013">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3013">'Razor'</span></span>
- <span data-ttu-id="6dd91-3014">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3014">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3015">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3015">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3016">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3016">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3017">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3017">'Identity'</span></span>
- <span data-ttu-id="6dd91-3018">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3018">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3019">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3019">'Razor'</span></span>
- <span data-ttu-id="6dd91-3020">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3020">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3021">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3021">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3022">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3022">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3023">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3023">'Identity'</span></span>
- <span data-ttu-id="6dd91-3024">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3024">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3025">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3025">'Razor'</span></span>
- <span data-ttu-id="6dd91-3026">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3026">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3027">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3027">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3028">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3028">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3029">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3029">'Identity'</span></span>
- <span data-ttu-id="6dd91-3030">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3030">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3031">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3031">'Razor'</span></span>
- <span data-ttu-id="6dd91-3032">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3032">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3033">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3033">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3034">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3034">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3035">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3035">'Identity'</span></span>
- <span data-ttu-id="6dd91-3036">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3036">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3037">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3037">'Razor'</span></span>
- <span data-ttu-id="6dd91-3038">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3038">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3039">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3039">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3040">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3040">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3041">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3041">'Identity'</span></span>
- <span data-ttu-id="6dd91-3042">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3042">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3043">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3043">'Razor'</span></span>
- <span data-ttu-id="6dd91-3044">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3044">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3045">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3045">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3046">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3046">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3047">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3047">'Identity'</span></span>
- <span data-ttu-id="6dd91-3048">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3048">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3049">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3049">'Razor'</span></span>
- <span data-ttu-id="6dd91-3050">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3050">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3051">------------------------------------- | |`hello`                                  | `/hello`               |僅符合單一路徑 `/hello` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3051">------------------------------------- | | `hello`                                  | `/hello`                | Only matches the single path `/hello`.</span></span>                                     <span data-ttu-id="6dd91-3052">| |`{Page=Home}`                            | `/`                    |符合並將設定 `Page` 為 `Home` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3052">| | `{Page=Home}`                            | `/`                     | Matches and sets `Page` to `Home`.</span></span>                                         <span data-ttu-id="6dd91-3053">| |`{Page=Home}`                            | `/Contact`             |符合並將設定 `Page` 為 `Contact` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3053">| | `{Page=Home}`                            | `/Contact`              | Matches and sets `Page` to `Contact`.</span></span>                                      <span data-ttu-id="6dd91-3054">| |`{controller}/{action}/{id?}`            | `/Products/List`       |對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3054">| | `{controller}/{action}/{id?}`            | `/Products/List`        | Maps to the `Products` controller and `List` action.</span></span>                       <span data-ttu-id="6dd91-3055">| |`{controller}/{action}/{id?}`            | `/Products/Details/123`|對應至 `Products` 控制器和 `Details` 動作（ `id` 設定為123）。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3055">| | `{controller}/{action}/{id?}`            | `/Products/Details/123` | Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> <span data-ttu-id="6dd91-3056">| |`{controller=Home}/{action=Index}/{id?}` | `/`                    |對應至 `Home` 控制器和 `Index` 方法（ `id` 會被忽略）。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3056">| | `{controller=Home}/{action=Index}/{id?}` | `/`                     | Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="6dd91-3057">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3057">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="6dd91-3058">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3058">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="6dd91-3059">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3059">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="6dd91-3060">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="6dd91-3060">Reserved routing names</span></span>

<span data-ttu-id="6dd91-3061">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3061">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="6dd91-3062">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-3062">Route constraint reference</span></span>

<span data-ttu-id="6dd91-3063">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3063">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="6dd91-3064">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3064">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="6dd91-3065">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3065">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="6dd91-3066">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3066">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="6dd91-3067">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3067">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd91-3068">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3068">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="6dd91-3069">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3069">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="6dd91-3070">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3070">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="6dd91-3071">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3071">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="6dd91-3072">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="6dd91-3072">constraint</span></span> | <span data-ttu-id="6dd91-3073">範例</span><span class="sxs-lookup"><span data-stu-id="6dd91-3073">Example</span></span> | <span data-ttu-id="6dd91-3074">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="6dd91-3074">Example Matches</span></span> | <span data-ttu-id="6dd91-3075">備忘錄</span><span class="sxs-lookup"><span data-stu-id="6dd91-3075">Notes</span></span> |
| ---
<span data-ttu-id="6dd91-3076">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3076">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3077">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3077">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3078">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3078">'Identity'</span></span>
- <span data-ttu-id="6dd91-3079">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3079">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3080">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3080">'Razor'</span></span>
- <span data-ttu-id="6dd91-3081">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3081">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3082">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3082">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3083">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3083">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3084">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3084">'Identity'</span></span>
- <span data-ttu-id="6dd91-3085">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3085">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3086">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3086">'Razor'</span></span>
- <span data-ttu-id="6dd91-3087">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3087">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3088">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3088">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3089">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3089">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3090">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3090">'Identity'</span></span>
- <span data-ttu-id="6dd91-3091">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3091">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3092">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3092">'Razor'</span></span>
- <span data-ttu-id="6dd91-3093">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3093">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3094">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3094">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3095">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3095">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3096">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3096">'Identity'</span></span>
- <span data-ttu-id="6dd91-3097">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3097">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3098">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3098">'Razor'</span></span>
- <span data-ttu-id="6dd91-3099">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3099">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3100">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3100">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3101">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3101">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3102">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3102">'Identity'</span></span>
- <span data-ttu-id="6dd91-3103">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3103">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3104">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3104">'Razor'</span></span>
- <span data-ttu-id="6dd91-3105">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3105">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3106">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3106">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3107">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3107">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3108">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3108">'Identity'</span></span>
- <span data-ttu-id="6dd91-3109">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3109">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3110">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3110">'Razor'</span></span>
- <span data-ttu-id="6dd91-3111">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3111">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3112">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3112">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3113">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3113">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3114">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3114">'Identity'</span></span>
- <span data-ttu-id="6dd91-3115">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3115">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3116">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3116">'Razor'</span></span>
- <span data-ttu-id="6dd91-3117">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3117">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3118">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3118">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3119">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3119">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3120">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3120">'Identity'</span></span>
- <span data-ttu-id="6dd91-3121">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3121">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3122">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3122">'Razor'</span></span>
- <span data-ttu-id="6dd91-3123">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3123">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3124">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3124">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3125">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3125">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3126">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3126">'Identity'</span></span>
- <span data-ttu-id="6dd91-3127">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3127">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3128">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3128">'Razor'</span></span>
- <span data-ttu-id="6dd91-3129">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3129">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3130">-------- |----- | |`int` | `{id:int}` | `123456789`, `-123456789` |符合任何整數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3130">-------- | ----- | | `int` | `{id:int}` | `123456789`, `-123456789` | Matches any integer.</span></span> <span data-ttu-id="6dd91-3131">| |`bool` | `{active:bool}` | `true`, `FALSE` |符合 `true` 或 `false. Case-insensitive. |
| ` datetime ` | ` {dob： datetime} ` | ` 2016-12-31 `, ` 2016-12-31 7： 32pm ` | Matches a valid ` datetime ` value in the invariant culture. See  preceding warning.|
| ` decimal ` | ` {price： decimal} ` | ` 49.99 `, ` -1000.01 ` | Matches a valid ` decimal ` value in the invariant culture. See  preceding warning.|
| ` double ` | ` {權數： double} ` | ` 1.234 `, ` -1，001.01 e8 ` | Matches a valid ` double ` value in the invariant culture. See  preceding warning.|
| ` float ` | ` {權數： float} ` | ` 1.234 `, ` -1，001.01 e8 ` | Matches a valid ` float ` value in the invariant culture. See  preceding warning.|
| ` guid ` | ` {id： guid} ` | ` CD2C1638-1638-72D5-1638-DEADBEEF1638 `, ` {CD2C1638-1638-72D5-1638-DEADBEEF1638} ` | Matches a valid ` guid ` value. |
| ` long ` | ` {滴答： long} ` | ` 123456789 `, ` -123456789 ` | Matches a valid ` 長 ` value. |
| ` minlength （值） ` | ` {username： minlength （4）} ` | ` Rick ` | String must be at least 4 characters. |
| ` maxlength （值） { ` | ` filename： maxlength （8）} ` | ` myfile.txt ` | String has maximum of 8 characters. |
| ` length （長度） ` | ` {filename： length （12）} ` | ` somefile.txt。 txt ` | String must be exactly 12 characters long. |
| ` 長度（最小值，最大值） ` | ` {檔案名：長度（8，16）} ` | ` somefile.txt .txt ` | String must be at least 8 and has maximum of 16 characters. |
| ` 分鐘（值） ` | ` {age： min （18）} ` | ` 19 ` | Integer value must be at least 18. |
| ` max （值） ` | ` {age： max （120）} ` | ` 91 ` | Integer value maximum of 120. |
| ` 範圍（min，max） ` | ` {年齡：範圍（18120）} ` | ` 91 ` | Integer value must be at least 18 and maximum of 120. |
| ` Alpha ` | ` {name： Alpha} ` | ` Rick ` | String must consist of one or more alphabetical characters ` a `-` z `.  Case-insensitive. |
| ` RegEx （expression） ` | ` {ssn： RegEx （^ \\ d { {3} }- \\ d { {2} }- \\ d { {4} } $）} ` | ` 123-45-6789 ` | String must match the regular expression. See tips about defining a regular expression. |
| ` 必要 ` | ` {name： required} ` | ` Rick ' |用來強制在 URL 產生期間出現非參數值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3131">| | `bool` | `{active:bool}` | `true`, `FALSE` | Matches `true` or `false. Case-insensitive. |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Matches a valid `DateTime` value in the invariant culture. See  preceding warning.|
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Matches a valid `decimal` value in the invariant culture. See  preceding warning.|
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Matches a valid `double` value in the invariant culture. See  preceding warning.|
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Matches a valid `float` value in the invariant culture. See  preceding warning.|
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Matches a valid `Guid` value. |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Matches a valid `long` value. |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | String must be at least 4 characters. |
| `maxlength(value)` | `{filename:maxlength(8)}` | `MyFile` | String has maximum of 8 characters. |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | String must be exactly 12 characters long. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | String must be at least 8 and has maximum of 16 characters. |
| `min(value)` | `{age:min(18)}` | `19` | Integer value must be at least 18. |
| `max(value)` | `{age:max(120)}` | `91` | Integer value maximum of 120. |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Integer value must be at least 18 and maximum of 120. |
| `alpha` | `{name:alpha}` | `Rick` | String must consist of one or more alphabetical characters `a`-`z`.  Case-insensitive. |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | String must match the regular expression. See tips about defining a regular expression. |
| `required` | `{name:required}` | `Rick\` | Used to enforce that a non-parameter value is present during URL generation.</span></span> |

<span data-ttu-id="6dd91-3132">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3132">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="6dd91-3133">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3133">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="6dd91-3134">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3134">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="6dd91-3135">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3135">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="6dd91-3136">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3136">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="6dd91-3137">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3137">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="6dd91-3138">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3138">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="6dd91-3139">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-3139">Regular expressions</span></span>

<span data-ttu-id="6dd91-3140">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3140">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="6dd91-3141">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3141">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="6dd91-3142">正則運算式會使用類似路由和 c # 語言所使用的分隔符號和標記。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3142">Regular expressions use delimiters and tokens similar to those used by routing and the C# language.</span></span> <span data-ttu-id="6dd91-3143">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3143">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="6dd91-3144">若要在路由中使用正則運算式 `^\d{3}-\d{2}-\d{4}$` ：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3144">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing:</span></span>

* <span data-ttu-id="6dd91-3145">運算式必須在字串中提供單一反斜線 `\` 字元，做為原始程式碼中的雙反斜線 `\\` 字元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3145">The expression must have the single backslash `\` characters provided in the string as double backslash `\\` characters in the source code.</span></span>
* <span data-ttu-id="6dd91-3146">正則運算式必須是， `\\` 才能將 `\` 字串 escape 字元轉義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3146">The regular expression must us `\\` in order to escape the `\` string escape character.</span></span>
* <span data-ttu-id="6dd91-3147">`\\`使用[逐字字串常](/dotnet/csharp/language-reference/keywords/string)值時，不需要正則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3147">The regular expression doesn't require `\\` when using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string).</span></span>

<span data-ttu-id="6dd91-3148">若要 escape 路由參數分隔符號 `{` 、 `}` 、 `[` 、，請將 `]` 運算式中的字元、、、加倍 `{{` `}` `[[` `]]` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3148">To escape routing parameter delimiter characters `{`, `}`, `[`, `]`, double the characters in the expression `{{`, `}`, `[[`, `]]`.</span></span> <span data-ttu-id="6dd91-3149">下表顯示正則運算式和已轉義的版本：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3149">The following table shows a regular expression and the escaped version:</span></span>

| <span data-ttu-id="6dd91-3150">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-3150">Regular Expression</span></span>    | <span data-ttu-id="6dd91-3151">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-3151">Escaped Regular Expression</span></span>     |
| ---
<span data-ttu-id="6dd91-3152">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3152">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3153">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3153">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3154">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3154">'Identity'</span></span>
- <span data-ttu-id="6dd91-3155">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3155">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3156">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3156">'Razor'</span></span>
- <span data-ttu-id="6dd91-3157">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3157">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3158">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3158">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3159">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3159">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3160">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3160">'Identity'</span></span>
- <span data-ttu-id="6dd91-3161">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3161">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3162">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3162">'Razor'</span></span>
- <span data-ttu-id="6dd91-3163">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3163">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3164">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3164">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3165">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3165">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3166">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3166">'Identity'</span></span>
- <span data-ttu-id="6dd91-3167">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3167">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3168">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3168">'Razor'</span></span>
- <span data-ttu-id="6dd91-3169">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3169">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3170">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3170">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3171">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3171">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3172">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3172">'Identity'</span></span>
- <span data-ttu-id="6dd91-3173">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3173">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3174">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3174">'Razor'</span></span>
- <span data-ttu-id="6dd91-3175">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3175">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3176">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3176">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3177">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3177">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3178">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3178">'Identity'</span></span>
- <span data-ttu-id="6dd91-3179">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3179">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3180">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3180">'Razor'</span></span>
- <span data-ttu-id="6dd91-3181">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3181">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3182">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3182">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3183">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3183">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3184">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3184">'Identity'</span></span>
- <span data-ttu-id="6dd91-3185">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3185">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3186">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3186">'Razor'</span></span>
- <span data-ttu-id="6dd91-3187">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3187">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3188">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3188">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3189">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3189">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3190">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3190">'Identity'</span></span>
- <span data-ttu-id="6dd91-3191">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3191">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3192">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3192">'Razor'</span></span>
- <span data-ttu-id="6dd91-3193">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3193">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3194">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3194">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3195">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3195">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3196">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3196">'Identity'</span></span>
- <span data-ttu-id="6dd91-3197">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3197">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3198">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3198">'Razor'</span></span>
- <span data-ttu-id="6dd91-3199">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3199">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3200">----------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3200">----------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3201">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3201">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3202">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3202">'Identity'</span></span>
- <span data-ttu-id="6dd91-3203">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3203">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3204">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3204">'Razor'</span></span>
- <span data-ttu-id="6dd91-3205">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3205">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3206">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3206">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3207">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3207">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3208">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3208">'Identity'</span></span>
- <span data-ttu-id="6dd91-3209">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3209">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3210">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3210">'Razor'</span></span>
- <span data-ttu-id="6dd91-3211">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3211">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3212">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3212">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3213">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3213">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3214">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3214">'Identity'</span></span>
- <span data-ttu-id="6dd91-3215">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3215">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3216">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3216">'Razor'</span></span>
- <span data-ttu-id="6dd91-3217">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3217">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3218">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3218">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3219">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3219">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3220">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3220">'Identity'</span></span>
- <span data-ttu-id="6dd91-3221">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3221">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3222">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3222">'Razor'</span></span>
- <span data-ttu-id="6dd91-3223">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3223">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3224">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3224">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3225">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3225">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3226">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3226">'Identity'</span></span>
- <span data-ttu-id="6dd91-3227">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3227">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3228">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3228">'Razor'</span></span>
- <span data-ttu-id="6dd91-3229">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3229">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3230">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3230">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3231">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3231">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3232">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3232">'Identity'</span></span>
- <span data-ttu-id="6dd91-3233">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3233">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3234">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3234">'Razor'</span></span>
- <span data-ttu-id="6dd91-3235">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3235">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3236">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3236">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3237">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3237">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3238">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3238">'Identity'</span></span>
- <span data-ttu-id="6dd91-3239">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3239">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3240">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3240">'Razor'</span></span>
- <span data-ttu-id="6dd91-3241">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3241">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3242">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3242">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3243">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3243">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3244">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3244">'Identity'</span></span>
- <span data-ttu-id="6dd91-3245">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3245">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3246">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3246">'Razor'</span></span>
- <span data-ttu-id="6dd91-3247">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3247">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3248">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3248">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3249">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3249">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3250">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3250">'Identity'</span></span>
- <span data-ttu-id="6dd91-3251">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3251">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3252">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3252">'Razor'</span></span>
- <span data-ttu-id="6dd91-3253">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3253">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3254">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3254">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3255">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3256">'Identity'</span></span>
- <span data-ttu-id="6dd91-3257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3257">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3258">'Razor'</span></span>
- <span data-ttu-id="6dd91-3259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3259">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3260">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3260">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3261">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3261">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3262">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3262">'Identity'</span></span>
- <span data-ttu-id="6dd91-3263">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3263">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3264">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3264">'Razor'</span></span>
- <span data-ttu-id="6dd91-3265">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3265">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3266">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3266">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3267">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3267">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3268">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3268">'Identity'</span></span>
- <span data-ttu-id="6dd91-3269">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3269">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3270">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3270">'Razor'</span></span>
- <span data-ttu-id="6dd91-3271">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3271">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3272">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3272">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3273">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3273">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3274">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3274">'Identity'</span></span>
- <span data-ttu-id="6dd91-3275">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3275">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3276">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3276">'Razor'</span></span>
- <span data-ttu-id="6dd91-3277">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3277">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3278">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span><span class="sxs-lookup"><span data-stu-id="6dd91-3278">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span></span>

<span data-ttu-id="6dd91-3279">路由中使用的正則運算式通常會以插入 `^` 號字元開頭，並比對字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3279">Regular expressions used in routing often start with the caret `^` character and match starting position of the string.</span></span> <span data-ttu-id="6dd91-3280">這些運算式通常會以貨幣符號 `$` 字元結尾，並比對字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3280">The expressions often end with the dollar sign `$` character and match end of the string.</span></span> <span data-ttu-id="6dd91-3281">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3281">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="6dd91-3282">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3282">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="6dd91-3283">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3283">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="6dd91-3284">運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-3284">Expression</span></span>   | <span data-ttu-id="6dd91-3285">String</span><span class="sxs-lookup"><span data-stu-id="6dd91-3285">String</span></span>    | <span data-ttu-id="6dd91-3286">比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-3286">Match</span></span> | <span data-ttu-id="6dd91-3287">註解</span><span class="sxs-lookup"><span data-stu-id="6dd91-3287">Comment</span></span>               |
| ---
<span data-ttu-id="6dd91-3288">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3288">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3289">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3289">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3290">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3290">'Identity'</span></span>
- <span data-ttu-id="6dd91-3291">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3291">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3292">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3292">'Razor'</span></span>
- <span data-ttu-id="6dd91-3293">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3293">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3294">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3294">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3295">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3295">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3296">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3296">'Identity'</span></span>
- <span data-ttu-id="6dd91-3297">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3297">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3298">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3298">'Razor'</span></span>
- <span data-ttu-id="6dd91-3299">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3299">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3300">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3300">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3301">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3301">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3302">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3302">'Identity'</span></span>
- <span data-ttu-id="6dd91-3303">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3303">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3304">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3304">'Razor'</span></span>
- <span data-ttu-id="6dd91-3305">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3305">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3306">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3306">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3307">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3308">'Identity'</span></span>
- <span data-ttu-id="6dd91-3309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3309">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3310">'Razor'</span></span>
- <span data-ttu-id="6dd91-3311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3311">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3312">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3312">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3313">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3313">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3314">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3314">'Identity'</span></span>
- <span data-ttu-id="6dd91-3315">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3315">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3316">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3316">'Razor'</span></span>
- <span data-ttu-id="6dd91-3317">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3317">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3318">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3318">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3319">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3319">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3320">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3320">'Identity'</span></span>
- <span data-ttu-id="6dd91-3321">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3321">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3322">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3322">'Razor'</span></span>
- <span data-ttu-id="6dd91-3323">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3323">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3324">----- |:---: | ---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3324">----- | :---: |  --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3325">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3326">'Identity'</span></span>
- <span data-ttu-id="6dd91-3327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3327">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3328">'Razor'</span></span>
- <span data-ttu-id="6dd91-3329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3329">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3330">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3330">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3331">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3331">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3332">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3332">'Identity'</span></span>
- <span data-ttu-id="6dd91-3333">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3333">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3334">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3334">'Razor'</span></span>
- <span data-ttu-id="6dd91-3335">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3335">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3336">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3336">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3337">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3337">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3338">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3338">'Identity'</span></span>
- <span data-ttu-id="6dd91-3339">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3339">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3340">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3340">'Razor'</span></span>
- <span data-ttu-id="6dd91-3341">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3341">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3342">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3342">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3343">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3343">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3344">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3344">'Identity'</span></span>
- <span data-ttu-id="6dd91-3345">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3345">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3346">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3346">'Razor'</span></span>
- <span data-ttu-id="6dd91-3347">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3347">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3348">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3348">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3349">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3349">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3350">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3350">'Identity'</span></span>
- <span data-ttu-id="6dd91-3351">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3351">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3352">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3352">'Razor'</span></span>
- <span data-ttu-id="6dd91-3353">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3353">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3354">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3354">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3355">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3355">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3356">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3356">'Identity'</span></span>
- <span data-ttu-id="6dd91-3357">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3357">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3358">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3358">'Razor'</span></span>
- <span data-ttu-id="6dd91-3359">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3359">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3360">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3360">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3361">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3362">'Identity'</span></span>
- <span data-ttu-id="6dd91-3363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3363">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3364">'Razor'</span></span>
- <span data-ttu-id="6dd91-3365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3365">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3366">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3366">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3367">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3367">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3368">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3368">'Identity'</span></span>
- <span data-ttu-id="6dd91-3369">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3369">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3370">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3370">'Razor'</span></span>
- <span data-ttu-id="6dd91-3371">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3371">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3372">---------- || `[a-z]{2}`   |您好 |是 |子字串符合 || `[a-z]{2}`   |123abc456 |是 |子字串符合 || `[a-z]{2}`   |mz |是 |符合運算式 || `[a-z]{2}`   |MZ |是 |不區分大小寫 || `^[a-z]{2}$` |您好 |否 |查看 `^` 和更新版本 `$` | | `^[a-z]{2}$` | 123abc456 |否 |請參閱和更新版本 `^` `$` |</span><span class="sxs-lookup"><span data-stu-id="6dd91-3372">---------- | | `[a-z]{2}`   | hello     | Yes   | Substring matches     | | `[a-z]{2}`   | 123abc456 | Yes   | Substring matches     | | `[a-z]{2}`   | mz        | Yes   | Matches expression    | | `[a-z]{2}`   | MZ        | Yes   | Not case sensitive    | | `^[a-z]{2}$` | hello     | No    | See `^` and `$` above | | `^[a-z]{2}$` | 123abc456 | No    | See `^` and `$` above |</span></span>

<span data-ttu-id="6dd91-3373">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3373">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="6dd91-3374">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3374">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="6dd91-3375">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3375">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="6dd91-3376">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3376">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="6dd91-3377">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3377">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="6dd91-3378">自訂路由條件約束</span><span class="sxs-lookup"><span data-stu-id="6dd91-3378">Custom route constraints</span></span>

<span data-ttu-id="6dd91-3379">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3379">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="6dd91-3380"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3380">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="6dd91-3381">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3381">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="6dd91-3382"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3382">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="6dd91-3383">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3383">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="6dd91-3384">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3384">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="6dd91-3385">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3385">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="6dd91-3386">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3386">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="6dd91-3387">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-3387">Parameter transformer reference</span></span>

<span data-ttu-id="6dd91-3388">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3388">Parameter transformers:</span></span>

* <span data-ttu-id="6dd91-3389">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3389">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="6dd91-3390">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3390">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="6dd91-3391">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3391">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="6dd91-3392">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3392">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="6dd91-3393">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3393">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="6dd91-3394">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3394">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="6dd91-3395">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3395">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="6dd91-3396">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3396">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="6dd91-3397">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3397">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="6dd91-3398">使用上述的路由，動作 `SubscriptionManagementController.GetAll` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3398">With the preceding route, the action `SubscriptionManagementController.GetAll` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="6dd91-3399">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3399">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="6dd91-3400">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3400">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="6dd91-3401">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3401">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="6dd91-3402">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3402">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="6dd91-3403">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3403">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="6dd91-3404">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3404">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="6dd91-3405">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3405">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* Razor<span data-ttu-id="6dd91-3406">頁面具有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3406"> Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="6dd91-3407">此慣例會將指定的參數轉換器套用至所有自動探索的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3407">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="6dd91-3408">參數轉換器會轉換頁面路由的資料夾和檔案名區段 Razor 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3408">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="6dd91-3409">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3409">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="6dd91-3410">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-3410">URL generation reference</span></span>

<span data-ttu-id="6dd91-3411">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3411">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="6dd91-3412">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3412">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="6dd91-3413">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3413">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="6dd91-3414">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3414">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="6dd91-3415"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3415">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="6dd91-3416">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3416">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="6dd91-3417">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3417">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6dd91-3418">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3418">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="6dd91-3419">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3419">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="6dd91-3420">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3420">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="6dd91-3421">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3421">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="6dd91-3422">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3422">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="6dd91-3423">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3423">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="6dd91-3424">環境值</span><span class="sxs-lookup"><span data-stu-id="6dd91-3424">Ambient Values</span></span>                     | <span data-ttu-id="6dd91-3425">明確值</span><span class="sxs-lookup"><span data-stu-id="6dd91-3425">Explicit Values</span></span>                        | <span data-ttu-id="6dd91-3426">結果</span><span class="sxs-lookup"><span data-stu-id="6dd91-3426">Result</span></span>                  |
| ---
<span data-ttu-id="6dd91-3427">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3427">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3428">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3428">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3429">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3429">'Identity'</span></span>
- <span data-ttu-id="6dd91-3430">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3430">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3431">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3431">'Razor'</span></span>
- <span data-ttu-id="6dd91-3432">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3432">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3433">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3433">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3434">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3434">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3435">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3435">'Identity'</span></span>
- <span data-ttu-id="6dd91-3436">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3436">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3437">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3437">'Razor'</span></span>
- <span data-ttu-id="6dd91-3438">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3438">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3439">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3439">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3440">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3440">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3441">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3441">'Identity'</span></span>
- <span data-ttu-id="6dd91-3442">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3442">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3443">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3443">'Razor'</span></span>
- <span data-ttu-id="6dd91-3444">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3444">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3445">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3445">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3446">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3446">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3447">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3447">'Identity'</span></span>
- <span data-ttu-id="6dd91-3448">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3448">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3449">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3449">'Razor'</span></span>
- <span data-ttu-id="6dd91-3450">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3450">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3451">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3451">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3452">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3452">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3453">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3453">'Identity'</span></span>
- <span data-ttu-id="6dd91-3454">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3454">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3455">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3455">'Razor'</span></span>
- <span data-ttu-id="6dd91-3456">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3456">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3457">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3457">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3458">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3458">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3459">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3459">'Identity'</span></span>
- <span data-ttu-id="6dd91-3460">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3460">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3461">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3461">'Razor'</span></span>
- <span data-ttu-id="6dd91-3462">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3462">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3463">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3463">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3464">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3464">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3465">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3465">'Identity'</span></span>
- <span data-ttu-id="6dd91-3466">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3466">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3467">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3467">'Razor'</span></span>
- <span data-ttu-id="6dd91-3468">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3468">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3469">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3469">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3470">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3470">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3471">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3471">'Identity'</span></span>
- <span data-ttu-id="6dd91-3472">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3472">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3473">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3473">'Razor'</span></span>
- <span data-ttu-id="6dd91-3474">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3474">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3475">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3475">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3476">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3476">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3477">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3477">'Identity'</span></span>
- <span data-ttu-id="6dd91-3478">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3478">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3479">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3479">'Razor'</span></span>
- <span data-ttu-id="6dd91-3480">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3480">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3481">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3481">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3482">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3482">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3483">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3483">'Identity'</span></span>
- <span data-ttu-id="6dd91-3484">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3484">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3485">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3485">'Razor'</span></span>
- <span data-ttu-id="6dd91-3486">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3486">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3487">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3487">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3488">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3488">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3489">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3489">'Identity'</span></span>
- <span data-ttu-id="6dd91-3490">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3490">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3491">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3491">'Razor'</span></span>
- <span data-ttu-id="6dd91-3492">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3492">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3493">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3493">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3494">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3494">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3495">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3495">'Identity'</span></span>
- <span data-ttu-id="6dd91-3496">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3496">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3497">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3497">'Razor'</span></span>
- <span data-ttu-id="6dd91-3498">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3498">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3499">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3499">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3500">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3500">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3501">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3501">'Identity'</span></span>
- <span data-ttu-id="6dd91-3502">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3502">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3503">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3503">'Razor'</span></span>
- <span data-ttu-id="6dd91-3504">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3504">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3505">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3505">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3506">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3506">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3507">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3507">'Identity'</span></span>
- <span data-ttu-id="6dd91-3508">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3508">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3509">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3509">'Razor'</span></span>
- <span data-ttu-id="6dd91-3510">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3510">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3511">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3511">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3512">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3512">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3513">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3513">'Identity'</span></span>
- <span data-ttu-id="6dd91-3514">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3514">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3515">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3515">'Razor'</span></span>
- <span data-ttu-id="6dd91-3516">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3516">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3517">----------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3517">----------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3518">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3518">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3519">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3519">'Identity'</span></span>
- <span data-ttu-id="6dd91-3520">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3520">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3521">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3521">'Razor'</span></span>
- <span data-ttu-id="6dd91-3522">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3522">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3523">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3523">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3524">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3524">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3525">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3525">'Identity'</span></span>
- <span data-ttu-id="6dd91-3526">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3526">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3527">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3527">'Razor'</span></span>
- <span data-ttu-id="6dd91-3528">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3528">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3529">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3529">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3530">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3530">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3531">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3531">'Identity'</span></span>
- <span data-ttu-id="6dd91-3532">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3532">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3533">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3533">'Razor'</span></span>
- <span data-ttu-id="6dd91-3534">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3534">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3535">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3535">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3536">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3536">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3537">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3537">'Identity'</span></span>
- <span data-ttu-id="6dd91-3538">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3538">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3539">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3539">'Razor'</span></span>
- <span data-ttu-id="6dd91-3540">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3540">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3541">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3541">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3542">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3542">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3543">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3543">'Identity'</span></span>
- <span data-ttu-id="6dd91-3544">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3544">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3545">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3545">'Razor'</span></span>
- <span data-ttu-id="6dd91-3546">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3546">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3547">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3547">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3548">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3548">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3549">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3549">'Identity'</span></span>
- <span data-ttu-id="6dd91-3550">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3550">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3551">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3551">'Razor'</span></span>
- <span data-ttu-id="6dd91-3552">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3552">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3553">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3553">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3554">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3554">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3555">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3555">'Identity'</span></span>
- <span data-ttu-id="6dd91-3556">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3556">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3557">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3557">'Razor'</span></span>
- <span data-ttu-id="6dd91-3558">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3558">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3559">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3559">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3560">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3560">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3561">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3561">'Identity'</span></span>
- <span data-ttu-id="6dd91-3562">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3562">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3563">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3563">'Razor'</span></span>
- <span data-ttu-id="6dd91-3564">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3564">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3565">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3565">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3566">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3566">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3567">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3567">'Identity'</span></span>
- <span data-ttu-id="6dd91-3568">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3568">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3569">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3569">'Razor'</span></span>
- <span data-ttu-id="6dd91-3570">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3570">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3571">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3571">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3572">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3572">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3573">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3573">'Identity'</span></span>
- <span data-ttu-id="6dd91-3574">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3574">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3575">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3575">'Razor'</span></span>
- <span data-ttu-id="6dd91-3576">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3576">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3577">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3577">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3578">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3578">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3579">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3579">'Identity'</span></span>
- <span data-ttu-id="6dd91-3580">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3580">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3581">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3581">'Razor'</span></span>
- <span data-ttu-id="6dd91-3582">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3582">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3583">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3583">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3584">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3584">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3585">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3585">'Identity'</span></span>
- <span data-ttu-id="6dd91-3586">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3586">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3587">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3587">'Razor'</span></span>
- <span data-ttu-id="6dd91-3588">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3588">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3589">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3589">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3590">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3590">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3591">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3591">'Identity'</span></span>
- <span data-ttu-id="6dd91-3592">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3592">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3593">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3593">'Razor'</span></span>
- <span data-ttu-id="6dd91-3594">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3594">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3595">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3595">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3596">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3596">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3597">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3597">'Identity'</span></span>
- <span data-ttu-id="6dd91-3598">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3598">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3599">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3599">'Razor'</span></span>
- <span data-ttu-id="6dd91-3600">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3600">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3601">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3601">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3602">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3602">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3603">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3603">'Identity'</span></span>
- <span data-ttu-id="6dd91-3604">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3604">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3605">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3605">'Razor'</span></span>
- <span data-ttu-id="6dd91-3606">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3606">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3607">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3607">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3608">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3608">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3609">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3609">'Identity'</span></span>
- <span data-ttu-id="6dd91-3610">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3610">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3611">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3611">'Razor'</span></span>
- <span data-ttu-id="6dd91-3612">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3612">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3613">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3613">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3614">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3614">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3615">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3615">'Identity'</span></span>
- <span data-ttu-id="6dd91-3616">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3616">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3617">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3617">'Razor'</span></span>
- <span data-ttu-id="6dd91-3618">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3618">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3619">------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3619">------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3620">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3620">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3621">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3621">'Identity'</span></span>
- <span data-ttu-id="6dd91-3622">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3622">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3623">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3623">'Razor'</span></span>
- <span data-ttu-id="6dd91-3624">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3624">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3625">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3625">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3626">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3626">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3627">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3627">'Identity'</span></span>
- <span data-ttu-id="6dd91-3628">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3628">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3629">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3629">'Razor'</span></span>
- <span data-ttu-id="6dd91-3630">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3630">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3631">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3631">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3632">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3632">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3633">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3633">'Identity'</span></span>
- <span data-ttu-id="6dd91-3634">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3634">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3635">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3635">'Razor'</span></span>
- <span data-ttu-id="6dd91-3636">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3636">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3637">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3637">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3638">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3638">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3639">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3639">'Identity'</span></span>
- <span data-ttu-id="6dd91-3640">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3640">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3641">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3641">'Razor'</span></span>
- <span data-ttu-id="6dd91-3642">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3642">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3643">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3643">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3644">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3644">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3645">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3645">'Identity'</span></span>
- <span data-ttu-id="6dd91-3646">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3646">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3647">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3647">'Razor'</span></span>
- <span data-ttu-id="6dd91-3648">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3648">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3649">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3649">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3650">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3650">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3651">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3651">'Identity'</span></span>
- <span data-ttu-id="6dd91-3652">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3652">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3653">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3653">'Razor'</span></span>
- <span data-ttu-id="6dd91-3654">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3654">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3655">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3655">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3656">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3656">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3657">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3657">'Identity'</span></span>
- <span data-ttu-id="6dd91-3658">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3658">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3659">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3659">'Razor'</span></span>
- <span data-ttu-id="6dd91-3660">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3660">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3661">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3661">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3662">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3662">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3663">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3663">'Identity'</span></span>
- <span data-ttu-id="6dd91-3664">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3664">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3665">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3665">'Razor'</span></span>
- <span data-ttu-id="6dd91-3666">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3666">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3667">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3667">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3668">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3668">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3669">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3669">'Identity'</span></span>
- <span data-ttu-id="6dd91-3670">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3670">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3671">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3671">'Razor'</span></span>
- <span data-ttu-id="6dd91-3672">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3672">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3673">------------ | |控制器 = "Home" |action = "About" |`/Home/About`|
|控制器 = "Home" |控制器 = "Order"，action = "About" |`/Order/About`|
|控制器 = "Home"，color = "Red" |action = "About" |`/Home/About`|
|控制器 = "Home" |action = "About"，color = "Red" |`/Home/About?color=Red`                                |</span><span class="sxs-lookup"><span data-stu-id="6dd91-3673">------------ | | controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |</span></span>

<span data-ttu-id="6dd91-3674">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3674">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="6dd91-3675">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3675">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="6dd91-3676">複雜區段</span><span class="sxs-lookup"><span data-stu-id="6dd91-3676">Complex segments</span></span>

<span data-ttu-id="6dd91-3677">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3677">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="6dd91-3678">請參閱[此程式碼](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3678">See [this code](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="6dd91-3679">[程式法範例](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3679">The [code sample](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6dd91-3680">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3680">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="6dd91-3681">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3681">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="6dd91-3682">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3682">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="6dd91-3683">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3683">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

<span data-ttu-id="6dd91-3684">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3684">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> <span data-ttu-id="6dd91-3685">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3685">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="6dd91-3686">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3686">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="6dd91-3687">如需頁面中路由慣例的詳細資訊 Razor ，請參閱 <xref:razor-pages/razor-pages-conventions> 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3687">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="6dd91-3688">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="6dd91-3688">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="6dd91-3689">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="6dd91-3689">Routing basics</span></span>

<span data-ttu-id="6dd91-3690">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3690">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="6dd91-3691">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3691">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="6dd91-3692">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3692">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="6dd91-3693">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3693">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="6dd91-3694">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3694">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="6dd91-3695">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3695">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="6dd91-3696">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3696">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="6dd91-3697">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3697">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

Razor<span data-ttu-id="6dd91-3698">頁面應用程式會使用預設的傳統路由，在應用程式的*Pages*資料夾中提供已命名的資源。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3698"> Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="6dd91-3699">還有其他慣例可讓您自訂 Razor 頁面路由行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3699">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="6dd91-3700">如需詳細資訊，請參閱 <xref:razor-pages/index> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3700">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="6dd91-3701">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3701">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="6dd91-3702">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3702">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="6dd91-3703">路由會使用的路由 <xref:Microsoft.AspNetCore.Routing.IRouter> 執行來：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3703">Routing uses routes implementations of <xref:Microsoft.AspNetCore.Routing.IRouter> to:</span></span>

* <span data-ttu-id="6dd91-3704">將傳入要求對應至「路由處理常式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3704">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="6dd91-3705">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3705">Generate the URLs used in responses.</span></span>

<span data-ttu-id="6dd91-3706">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3706">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="6dd91-3707">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3707">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="6dd91-3708">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3708">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="6dd91-3709">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3709">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="6dd91-3710">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3710">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="6dd91-3711">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3711">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="6dd91-3712">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3712">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="6dd91-3713"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3713"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="6dd91-3714">應用程式模型（例如 MVC/ Razor Pages）會註冊其所有的路由，其具有可預測的路由案例執行。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3714">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="6dd91-3715">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3715">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="6dd91-3716">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3716">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="6dd91-3717"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3717"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>
<!-- fix [middleware](xref:fundamentals/middleware/index) -->
<span data-ttu-id="6dd91-3718">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3718">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="6dd91-3719">[ASP.NET CORE mvc](xref:mvc/overview)會將路由新增至中介軟體管線，作為其設定的一部分，並處理 MVC 和 Razor 頁面應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3719">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="6dd91-3720">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3720">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="6dd91-3721">URL 比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-3721">URL matching</span></span>

<span data-ttu-id="6dd91-3722">URL 比對是路由用來將傳入要求分派給「處理常式」\*\* 的處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3722">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="6dd91-3723">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3723">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="6dd91-3724">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3724">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="6dd91-3725">傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3725">Incoming requests enter the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="6dd91-3726"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」\*\* 要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3726">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="6dd91-3727">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3727">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="6dd91-3728">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3728">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="6dd91-3729"><xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3729">The primary input to <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="6dd91-3730">[RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3730">The [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="6dd91-3731">呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3731">A match that calls <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> also sets the properties of the [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="6dd91-3732">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」\*\* 的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3732">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="6dd91-3733">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3733">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="6dd91-3734">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3734">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="6dd91-3735">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3735"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="6dd91-3736">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3736">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="6dd91-3737">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3737">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="6dd91-3738">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3738">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="6dd91-3739">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3739">Routes can be nested inside of one another.</span></span> <span data-ttu-id="6dd91-3740"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3740">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="6dd91-3741">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3741">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="6dd91-3742"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3742">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation"></a><span data-ttu-id="6dd91-3743">URL 產生</span><span class="sxs-lookup"><span data-stu-id="6dd91-3743">URL generation</span></span>

<span data-ttu-id="6dd91-3744">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3744">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="6dd91-3745">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3745">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="6dd91-3746">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3746">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="6dd91-3747">每個「路由」\*\* 會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3747">Each *route* has its <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="6dd91-3748"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3748">The primary inputs to <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> are:</span></span>

* [<span data-ttu-id="6dd91-3749">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="6dd91-3749">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [<span data-ttu-id="6dd91-3750">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="6dd91-3750">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [<span data-ttu-id="6dd91-3751">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="6dd91-3751">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

<span data-ttu-id="6dd91-3752">路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3752">Routes primarily use the route values provided by <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> and <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="6dd91-3753"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3753">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="6dd91-3754">相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3754">In contrast, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="6dd91-3755">提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3755">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="6dd91-3756">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3756">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="6dd91-3757">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3757">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="6dd91-3758"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3758">The output of <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is a <xref:Microsoft.AspNetCore.Routing.VirtualPathData>.</span></span> <span data-ttu-id="6dd91-3759"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3759"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> is a parallel of <xref:Microsoft.AspNetCore.Routing.RouteData>.</span></span> <span data-ttu-id="6dd91-3760"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3760"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> contains the <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="6dd91-3761">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3761">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="6dd91-3762">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3762">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="6dd91-3763">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3763">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="6dd91-3764">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3764">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="6dd91-3765">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3765">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="6dd91-3766">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3766">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

### <a name="create-routes"></a><span data-ttu-id="6dd91-3767">建立路由</span><span class="sxs-lookup"><span data-stu-id="6dd91-3767">Create routes</span></span>

<span data-ttu-id="6dd91-3768">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3768">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="6dd91-3769">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」\*\* 語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3769"><xref:Microsoft.AspNetCore.Routing.Route> uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="6dd91-3770">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3770"><xref:Microsoft.AspNetCore.Routing.Route> uses the same route template to generate a URL when <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is called.</span></span>

<span data-ttu-id="6dd91-3771">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3771">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="6dd91-3772">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3772">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="6dd91-3773"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3773"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="6dd91-3774"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3774"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="6dd91-3775">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3775">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="6dd91-3776">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3776">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="6dd91-3777">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3777">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="6dd91-3778">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3778">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6dd91-3779">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3779">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="6dd91-3780">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3780">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="6dd91-3781">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」\*\* 名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3781">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="6dd91-3782">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3782">Route parameters are named.</span></span> <span data-ttu-id="6dd91-3783">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3783">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="6dd91-3784">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3784">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="6dd91-3785">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3785">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="6dd91-3786">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3786">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="6dd91-3787">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3787">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="6dd91-3788">在路由相符時，具有預設值的路由參數一定\*\* 會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3788">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="6dd91-3789">如果沒有對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3789">Optional parameters don't produce a route value if there is no corresponding URL path segment.</span></span> <span data-ttu-id="6dd91-3790">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3790">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="6dd91-3791">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3791">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="6dd91-3792">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3792">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="6dd91-3793">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3793">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="6dd91-3794">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3794">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="6dd91-3795">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3795">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="6dd91-3796"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3796">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="6dd91-3797">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3797">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="6dd91-3798">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3798">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

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
> <span data-ttu-id="6dd91-3799">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3799">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="6dd91-3800">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3800">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="6dd91-3801">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3801">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="6dd91-3802">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3802">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="6dd91-3803">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3803">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="6dd91-3804">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3804">Default values can be specified in the route template.</span></span> <span data-ttu-id="6dd91-3805">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3805">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="6dd91-3806">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3806">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="6dd91-3807">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3807">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="6dd91-3808">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3808">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="6dd91-3810">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="6dd91-3810">Route class URL generation</span></span>

<span data-ttu-id="6dd91-3811"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3811">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="6dd91-3812">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3812">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="6dd91-3813">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3813">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="6dd91-3814">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="6dd91-3814">What values would be produced?</span></span> <span data-ttu-id="6dd91-3815">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3815">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="6dd91-3816">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3816">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6dd91-3817">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3817">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="6dd91-3818">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3818">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="6dd91-3819">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3819">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="6dd91-3820">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3820">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="6dd91-3821">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3821">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="6dd91-3822">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3822">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="6dd91-3823">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3823">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="6dd91-3824">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3824">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="6dd91-3825">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="6dd91-3825">Use routing middleware</span></span>

<span data-ttu-id="6dd91-3826">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3826">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="6dd91-3827">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3827">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="6dd91-3828">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3828">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="6dd91-3829">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3829">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="6dd91-3830"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>：僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3830"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>: Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="6dd91-3831">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="6dd91-3831">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="6dd91-3832">URI</span><span class="sxs-lookup"><span data-stu-id="6dd91-3832">URI</span></span>                    | <span data-ttu-id="6dd91-3833">回應</span><span class="sxs-lookup"><span data-stu-id="6dd91-3833">Response</span></span>                                          |
| ---
<span data-ttu-id="6dd91-3834">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3834">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3835">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3835">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3836">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3836">'Identity'</span></span>
- <span data-ttu-id="6dd91-3837">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3837">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3838">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3838">'Razor'</span></span>
- <span data-ttu-id="6dd91-3839">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3839">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3840">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3840">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3841">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3841">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3842">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3842">'Identity'</span></span>
- <span data-ttu-id="6dd91-3843">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3843">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3844">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3844">'Razor'</span></span>
- <span data-ttu-id="6dd91-3845">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3845">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3846">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3846">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3847">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3847">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3848">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3848">'Identity'</span></span>
- <span data-ttu-id="6dd91-3849">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3849">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3850">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3850">'Razor'</span></span>
- <span data-ttu-id="6dd91-3851">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3851">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3852">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3852">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3853">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3853">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3854">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3854">'Identity'</span></span>
- <span data-ttu-id="6dd91-3855">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3855">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3856">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3856">'Razor'</span></span>
- <span data-ttu-id="6dd91-3857">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3857">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3858">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3858">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3859">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3859">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3860">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3860">'Identity'</span></span>
- <span data-ttu-id="6dd91-3861">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3861">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3862">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3862">'Razor'</span></span>
- <span data-ttu-id="6dd91-3863">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3863">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3864">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3864">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3865">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3865">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3866">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3866">'Identity'</span></span>
- <span data-ttu-id="6dd91-3867">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3867">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3868">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3868">'Razor'</span></span>
- <span data-ttu-id="6dd91-3869">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3869">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3870">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3870">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3871">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3871">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3872">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3872">'Identity'</span></span>
- <span data-ttu-id="6dd91-3873">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3873">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3874">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3874">'Razor'</span></span>
- <span data-ttu-id="6dd91-3875">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3875">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3876">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3876">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3877">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3877">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3878">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3878">'Identity'</span></span>
- <span data-ttu-id="6dd91-3879">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3879">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3880">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3880">'Razor'</span></span>
- <span data-ttu-id="6dd91-3881">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3881">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3882">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3882">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3883">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3883">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3884">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3884">'Identity'</span></span>
- <span data-ttu-id="6dd91-3885">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3885">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3886">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3886">'Razor'</span></span>
- <span data-ttu-id="6dd91-3887">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3887">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-3888">----------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3888">----------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3889">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3889">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3890">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3890">'Identity'</span></span>
- <span data-ttu-id="6dd91-3891">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3891">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3892">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3892">'Razor'</span></span>
- <span data-ttu-id="6dd91-3893">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3893">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3894">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3894">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3895">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3895">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3896">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3896">'Identity'</span></span>
- <span data-ttu-id="6dd91-3897">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3897">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3898">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3898">'Razor'</span></span>
- <span data-ttu-id="6dd91-3899">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3899">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3900">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3900">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3901">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3901">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3902">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3902">'Identity'</span></span>
- <span data-ttu-id="6dd91-3903">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3903">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3904">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3904">'Razor'</span></span>
- <span data-ttu-id="6dd91-3905">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3905">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3906">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3906">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3907">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3907">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3908">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3908">'Identity'</span></span>
- <span data-ttu-id="6dd91-3909">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3909">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3910">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3910">'Razor'</span></span>
- <span data-ttu-id="6dd91-3911">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3911">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3912">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3912">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3913">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3913">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3914">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3914">'Identity'</span></span>
- <span data-ttu-id="6dd91-3915">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3915">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3916">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3916">'Razor'</span></span>
- <span data-ttu-id="6dd91-3917">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3917">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3918">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3918">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3919">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3919">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3920">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3920">'Identity'</span></span>
- <span data-ttu-id="6dd91-3921">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3921">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3922">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3922">'Razor'</span></span>
- <span data-ttu-id="6dd91-3923">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3923">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3924">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3924">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3925">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3925">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3926">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3926">'Identity'</span></span>
- <span data-ttu-id="6dd91-3927">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3927">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3928">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3928">'Razor'</span></span>
- <span data-ttu-id="6dd91-3929">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3929">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3930">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3930">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3931">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3931">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3932">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3932">'Identity'</span></span>
- <span data-ttu-id="6dd91-3933">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3933">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3934">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3934">'Razor'</span></span>
- <span data-ttu-id="6dd91-3935">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3935">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3936">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3936">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3937">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3937">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3938">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3938">'Identity'</span></span>
- <span data-ttu-id="6dd91-3939">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3939">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3940">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3940">'Razor'</span></span>
- <span data-ttu-id="6dd91-3941">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3941">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3942">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3942">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3943">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3943">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3944">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3944">'Identity'</span></span>
- <span data-ttu-id="6dd91-3945">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3945">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3946">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3946">'Razor'</span></span>
- <span data-ttu-id="6dd91-3947">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3947">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3948">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3948">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3949">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3949">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3950">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3950">'Identity'</span></span>
- <span data-ttu-id="6dd91-3951">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3951">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3952">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3952">'Razor'</span></span>
- <span data-ttu-id="6dd91-3953">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3953">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3954">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3954">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3955">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3955">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3956">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3956">'Identity'</span></span>
- <span data-ttu-id="6dd91-3957">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3957">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3958">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3958">'Razor'</span></span>
- <span data-ttu-id="6dd91-3959">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3959">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3960">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3960">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3961">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3961">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3962">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3962">'Identity'</span></span>
- <span data-ttu-id="6dd91-3963">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3963">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3964">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3964">'Razor'</span></span>
- <span data-ttu-id="6dd91-3965">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3965">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3966">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3966">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3967">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3967">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3968">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3968">'Identity'</span></span>
- <span data-ttu-id="6dd91-3969">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3969">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3970">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3970">'Razor'</span></span>
- <span data-ttu-id="6dd91-3971">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3971">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3972">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3972">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3973">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3973">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3974">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3974">'Identity'</span></span>
- <span data-ttu-id="6dd91-3975">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3975">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3976">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3976">'Razor'</span></span>
- <span data-ttu-id="6dd91-3977">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3977">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3978">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3978">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3979">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3979">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3980">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3980">'Identity'</span></span>
- <span data-ttu-id="6dd91-3981">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3981">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3982">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3982">'Razor'</span></span>
- <span data-ttu-id="6dd91-3983">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3983">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3984">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3984">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3985">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3985">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3986">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3986">'Identity'</span></span>
- <span data-ttu-id="6dd91-3987">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3987">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3988">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3988">'Razor'</span></span>
- <span data-ttu-id="6dd91-3989">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3989">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3990">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3990">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3991">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3991">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3992">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3992">'Identity'</span></span>
- <span data-ttu-id="6dd91-3993">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3993">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-3994">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3994">'Razor'</span></span>
- <span data-ttu-id="6dd91-3995">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3995">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-3996">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-3996">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-3997">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3997">'Blazor'</span></span>
- <span data-ttu-id="6dd91-3998">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3998">'Identity'</span></span>
- <span data-ttu-id="6dd91-3999">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-3999">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4000">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4000">'Razor'</span></span>
- <span data-ttu-id="6dd91-4001">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4001">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4002">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4002">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4003">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4003">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4004">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4004">'Identity'</span></span>
- <span data-ttu-id="6dd91-4005">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4005">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4006">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4006">'Razor'</span></span>
- <span data-ttu-id="6dd91-4007">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4007">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4008">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4008">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4009">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4009">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4010">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4010">'Identity'</span></span>
- <span data-ttu-id="6dd91-4011">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4011">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4012">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4012">'Razor'</span></span>
- <span data-ttu-id="6dd91-4013">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4013">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4014">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4014">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4015">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4015">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4016">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4016">'Identity'</span></span>
- <span data-ttu-id="6dd91-4017">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4017">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4018">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4018">'Razor'</span></span>
- <span data-ttu-id="6dd91-4019">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4019">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4020">------------------------- | |`/package/create/3`    |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-4020">------------------------- | | `/package/create/3`    | Hello!</span></span> <span data-ttu-id="6dd91-4021">路由值： [operation，create]，[id，3] | |`/package/track/-3`    |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-4021">Route values: [operation, create], [id, 3] | | `/package/track/-3`    | Hello!</span></span> <span data-ttu-id="6dd91-4022">路由值： [operation，track]，[id，-3] | |`/package/track/-3/`   |各位!</span><span class="sxs-lookup"><span data-stu-id="6dd91-4022">Route values: [operation, track], [id, -3] | | `/package/track/-3/`   | Hello!</span></span> <span data-ttu-id="6dd91-4023">路由值： [operation，track]，[id，-3] | |`/package/track/`      |要求已通過，沒有相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4023">Route values: [operation, track], [id, -3] | | `/package/track/`      | The request falls through, no match.</span></span>              <span data-ttu-id="6dd91-4024">| |`GET /hello/Joe`       |Joe！</span><span class="sxs-lookup"><span data-stu-id="6dd91-4024">| | `GET /hello/Joe`       | Hi, Joe!</span></span>                                          <span data-ttu-id="6dd91-4025">| |`POST /hello/Joe`      |要求會通過，只符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4025">| | `POST /hello/Joe`      | The request falls through, matches HTTP GET only.</span></span> <span data-ttu-id="6dd91-4026">| |`GET /hello/Joe/Smith` |要求已通過，沒有相符的。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4026">| | `GET /hello/Joe/Smith` | The request falls through, no match.</span></span>              |

<span data-ttu-id="6dd91-4027">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4027">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="6dd91-4028">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4028">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

<span data-ttu-id="6dd91-4029">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4029">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

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

<span data-ttu-id="6dd91-4030">其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4030">Some of listed methods, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, require a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="6dd91-4031">路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」\*\* 使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4031">The <xref:Microsoft.AspNetCore.Http.RequestDelegate> is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="6dd91-4032">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4032">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="6dd91-4033">如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4033">If the `Map*` method doesn't accept a handler, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

<span data-ttu-id="6dd91-4034">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4034">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="6dd91-4035">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4035">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="6dd91-4036">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-4036">Route template reference</span></span>

<span data-ttu-id="6dd91-4037">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4037">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="6dd91-4038">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4038">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="6dd91-4039">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4039">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="6dd91-4040">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4040">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="6dd91-4041">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4041">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="6dd91-4042">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4042">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="6dd91-4043">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4043">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="6dd91-4044">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4044">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="6dd91-4045">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4045">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="6dd91-4046">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4046">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="6dd91-4047">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4047">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="6dd91-4048">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4048">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="6dd91-4049">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4049">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="6dd91-4050">這稱為「全部擷取」\*\* 參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4050">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="6dd91-4051">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4051">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="6dd91-4052">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4052">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="6dd91-4053">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4053">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="6dd91-4054">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4054">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="6dd91-4055">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4055">Note the escaped forward slash.</span></span>

<span data-ttu-id="6dd91-4056">路由參數可能有「預設值」\*\*，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4056">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="6dd91-4057">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4057">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="6dd91-4058">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4058">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="6dd91-4059">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4059">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="6dd91-4060">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4060">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="6dd91-4061">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4061">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="6dd91-4062">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」\*\*。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4062">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="6dd91-4063">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4063">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="6dd91-4064">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4064">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="6dd91-4065">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4065">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="6dd91-4066">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4066">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="6dd91-4067">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4067">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="6dd91-4068">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4068">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="6dd91-4069">路由範本</span><span class="sxs-lookup"><span data-stu-id="6dd91-4069">Route Template</span></span>                           | <span data-ttu-id="6dd91-4070">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="6dd91-4070">Example Matching URI</span></span>    | <span data-ttu-id="6dd91-4071">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="6dd91-4071">The request URI&hellip;</span></span>                                                    |
| ---
<span data-ttu-id="6dd91-4072">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4072">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4073">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4073">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4074">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4074">'Identity'</span></span>
- <span data-ttu-id="6dd91-4075">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4075">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4076">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4076">'Razor'</span></span>
- <span data-ttu-id="6dd91-4077">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4077">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4078">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4078">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4079">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4079">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4080">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4080">'Identity'</span></span>
- <span data-ttu-id="6dd91-4081">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4081">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4082">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4082">'Razor'</span></span>
- <span data-ttu-id="6dd91-4083">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4083">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4084">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4084">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4085">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4085">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4086">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4086">'Identity'</span></span>
- <span data-ttu-id="6dd91-4087">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4087">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4088">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4088">'Razor'</span></span>
- <span data-ttu-id="6dd91-4089">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4089">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4090">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4090">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4091">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4091">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4092">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4092">'Identity'</span></span>
- <span data-ttu-id="6dd91-4093">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4093">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4094">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4094">'Razor'</span></span>
- <span data-ttu-id="6dd91-4095">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4095">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4096">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4096">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4097">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4097">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4098">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4098">'Identity'</span></span>
- <span data-ttu-id="6dd91-4099">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4099">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4100">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4100">'Razor'</span></span>
- <span data-ttu-id="6dd91-4101">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4101">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4102">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4102">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4103">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4104">'Identity'</span></span>
- <span data-ttu-id="6dd91-4105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4105">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4106">'Razor'</span></span>
- <span data-ttu-id="6dd91-4107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4107">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4108">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4108">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4109">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4109">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4110">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4110">'Identity'</span></span>
- <span data-ttu-id="6dd91-4111">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4111">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4112">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4112">'Razor'</span></span>
- <span data-ttu-id="6dd91-4113">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4113">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4114">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4114">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4115">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4115">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4116">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4116">'Identity'</span></span>
- <span data-ttu-id="6dd91-4117">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4117">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4118">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4118">'Razor'</span></span>
- <span data-ttu-id="6dd91-4119">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4119">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4120">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4120">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4121">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4121">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4122">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4122">'Identity'</span></span>
- <span data-ttu-id="6dd91-4123">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4123">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4124">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4124">'Razor'</span></span>
- <span data-ttu-id="6dd91-4125">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4125">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4126">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4126">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4127">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4127">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4128">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4128">'Identity'</span></span>
- <span data-ttu-id="6dd91-4129">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4129">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4130">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4130">'Razor'</span></span>
- <span data-ttu-id="6dd91-4131">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4131">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4132">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4132">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4133">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4133">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4134">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4134">'Identity'</span></span>
- <span data-ttu-id="6dd91-4135">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4135">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4136">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4136">'Razor'</span></span>
- <span data-ttu-id="6dd91-4137">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4137">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4138">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4138">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4139">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4139">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4140">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4140">'Identity'</span></span>
- <span data-ttu-id="6dd91-4141">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4141">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4142">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4142">'Razor'</span></span>
- <span data-ttu-id="6dd91-4143">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4143">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4144">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4144">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4145">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4145">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4146">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4146">'Identity'</span></span>
- <span data-ttu-id="6dd91-4147">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4147">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4148">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4148">'Razor'</span></span>
- <span data-ttu-id="6dd91-4149">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4149">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4150">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4150">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4151">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4151">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4152">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4152">'Identity'</span></span>
- <span data-ttu-id="6dd91-4153">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4153">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4154">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4154">'Razor'</span></span>
- <span data-ttu-id="6dd91-4155">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4155">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4156">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4156">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4157">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4157">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4158">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4158">'Identity'</span></span>
- <span data-ttu-id="6dd91-4159">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4159">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4160">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4160">'Razor'</span></span>
- <span data-ttu-id="6dd91-4161">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4161">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4162">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4162">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4163">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4163">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4164">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4164">'Identity'</span></span>
- <span data-ttu-id="6dd91-4165">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4165">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4166">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4166">'Razor'</span></span>
- <span data-ttu-id="6dd91-4167">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4167">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4168">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4168">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4169">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4170">'Identity'</span></span>
- <span data-ttu-id="6dd91-4171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4171">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4172">'Razor'</span></span>
- <span data-ttu-id="6dd91-4173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4174">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4174">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4175">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4175">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4176">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4176">'Identity'</span></span>
- <span data-ttu-id="6dd91-4177">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4177">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4178">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4178">'Razor'</span></span>
- <span data-ttu-id="6dd91-4179">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4179">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4180">-------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4180">-------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4181">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4181">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4182">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4182">'Identity'</span></span>
- <span data-ttu-id="6dd91-4183">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4183">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4184">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4184">'Razor'</span></span>
- <span data-ttu-id="6dd91-4185">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4185">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4186">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4186">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4187">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4187">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4188">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4188">'Identity'</span></span>
- <span data-ttu-id="6dd91-4189">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4189">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4190">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4190">'Razor'</span></span>
- <span data-ttu-id="6dd91-4191">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4191">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4192">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4192">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4193">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4193">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4194">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4194">'Identity'</span></span>
- <span data-ttu-id="6dd91-4195">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4195">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4196">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4196">'Razor'</span></span>
- <span data-ttu-id="6dd91-4197">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4197">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4198">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4198">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4199">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4199">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4200">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4200">'Identity'</span></span>
- <span data-ttu-id="6dd91-4201">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4201">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4202">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4202">'Razor'</span></span>
- <span data-ttu-id="6dd91-4203">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4203">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4204">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4204">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4205">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4205">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4206">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4206">'Identity'</span></span>
- <span data-ttu-id="6dd91-4207">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4207">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4208">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4208">'Razor'</span></span>
- <span data-ttu-id="6dd91-4209">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4209">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4210">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4210">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4211">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4211">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4212">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4212">'Identity'</span></span>
- <span data-ttu-id="6dd91-4213">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4213">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4214">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4214">'Razor'</span></span>
- <span data-ttu-id="6dd91-4215">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4215">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4216">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4216">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4217">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4217">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4218">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4218">'Identity'</span></span>
- <span data-ttu-id="6dd91-4219">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4219">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4220">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4220">'Razor'</span></span>
- <span data-ttu-id="6dd91-4221">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4221">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4222">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4222">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4223">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4223">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4224">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4224">'Identity'</span></span>
- <span data-ttu-id="6dd91-4225">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4225">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4226">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4226">'Razor'</span></span>
- <span data-ttu-id="6dd91-4227">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4227">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4228">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4228">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4229">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4229">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4230">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4230">'Identity'</span></span>
- <span data-ttu-id="6dd91-4231">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4231">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4232">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4232">'Razor'</span></span>
- <span data-ttu-id="6dd91-4233">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4233">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4234">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4234">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4235">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4235">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4236">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4236">'Identity'</span></span>
- <span data-ttu-id="6dd91-4237">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4237">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4238">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4238">'Razor'</span></span>
- <span data-ttu-id="6dd91-4239">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4239">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4240">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4240">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4241">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4242">'Identity'</span></span>
- <span data-ttu-id="6dd91-4243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4243">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4244">'Razor'</span></span>
- <span data-ttu-id="6dd91-4245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4246">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4246">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4247">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4247">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4248">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4248">'Identity'</span></span>
- <span data-ttu-id="6dd91-4249">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4249">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4250">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4250">'Razor'</span></span>
- <span data-ttu-id="6dd91-4251">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4251">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4252">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4252">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4253">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4253">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4254">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4254">'Identity'</span></span>
- <span data-ttu-id="6dd91-4255">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4255">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4256">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4256">'Razor'</span></span>
- <span data-ttu-id="6dd91-4257">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4257">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4258">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4258">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4259">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4259">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4260">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4260">'Identity'</span></span>
- <span data-ttu-id="6dd91-4261">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4261">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4262">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4262">'Razor'</span></span>
- <span data-ttu-id="6dd91-4263">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4263">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4264">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4264">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4265">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4265">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4266">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4266">'Identity'</span></span>
- <span data-ttu-id="6dd91-4267">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4267">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4268">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4268">'Razor'</span></span>
- <span data-ttu-id="6dd91-4269">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4269">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4270">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4270">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4271">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4271">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4272">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4272">'Identity'</span></span>
- <span data-ttu-id="6dd91-4273">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4273">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4274">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4274">'Razor'</span></span>
- <span data-ttu-id="6dd91-4275">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4275">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4276">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4276">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4277">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4277">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4278">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4278">'Identity'</span></span>
- <span data-ttu-id="6dd91-4279">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4279">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4280">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4280">'Razor'</span></span>
- <span data-ttu-id="6dd91-4281">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4281">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4282">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4282">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4283">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4283">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4284">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4284">'Identity'</span></span>
- <span data-ttu-id="6dd91-4285">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4285">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4286">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4286">'Razor'</span></span>
- <span data-ttu-id="6dd91-4287">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4287">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4288">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4288">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4289">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4289">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4290">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4290">'Identity'</span></span>
- <span data-ttu-id="6dd91-4291">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4291">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4292">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4292">'Razor'</span></span>
- <span data-ttu-id="6dd91-4293">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4293">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4294">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4294">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4295">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4295">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4296">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4296">'Identity'</span></span>
- <span data-ttu-id="6dd91-4297">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4297">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4298">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4298">'Razor'</span></span>
- <span data-ttu-id="6dd91-4299">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4299">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4300">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4300">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4301">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4301">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4302">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4302">'Identity'</span></span>
- <span data-ttu-id="6dd91-4303">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4303">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4304">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4304">'Razor'</span></span>
- <span data-ttu-id="6dd91-4305">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4305">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4306">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4306">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4307">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4308">'Identity'</span></span>
- <span data-ttu-id="6dd91-4309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4309">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4310">'Razor'</span></span>
- <span data-ttu-id="6dd91-4311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4311">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4312">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4312">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4313">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4313">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4314">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4314">'Identity'</span></span>
- <span data-ttu-id="6dd91-4315">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4315">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4316">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4316">'Razor'</span></span>
- <span data-ttu-id="6dd91-4317">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4317">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4318">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4318">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4319">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4319">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4320">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4320">'Identity'</span></span>
- <span data-ttu-id="6dd91-4321">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4321">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4322">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4322">'Razor'</span></span>
- <span data-ttu-id="6dd91-4323">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4323">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4324">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4324">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4325">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4326">'Identity'</span></span>
- <span data-ttu-id="6dd91-4327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4327">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4328">'Razor'</span></span>
- <span data-ttu-id="6dd91-4329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4329">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4330">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4330">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4331">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4331">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4332">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4332">'Identity'</span></span>
- <span data-ttu-id="6dd91-4333">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4333">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4334">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4334">'Razor'</span></span>
- <span data-ttu-id="6dd91-4335">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4335">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4336">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4336">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4337">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4337">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4338">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4338">'Identity'</span></span>
- <span data-ttu-id="6dd91-4339">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4339">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4340">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4340">'Razor'</span></span>
- <span data-ttu-id="6dd91-4341">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4341">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4342">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4342">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4343">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4343">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4344">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4344">'Identity'</span></span>
- <span data-ttu-id="6dd91-4345">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4345">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4346">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4346">'Razor'</span></span>
- <span data-ttu-id="6dd91-4347">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4347">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4348">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4348">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4349">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4349">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4350">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4350">'Identity'</span></span>
- <span data-ttu-id="6dd91-4351">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4351">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4352">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4352">'Razor'</span></span>
- <span data-ttu-id="6dd91-4353">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4353">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4354">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4354">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4355">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4355">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4356">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4356">'Identity'</span></span>
- <span data-ttu-id="6dd91-4357">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4357">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4358">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4358">'Razor'</span></span>
- <span data-ttu-id="6dd91-4359">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4359">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4360">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4360">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4361">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4362">'Identity'</span></span>
- <span data-ttu-id="6dd91-4363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4363">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4364">'Razor'</span></span>
- <span data-ttu-id="6dd91-4365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4365">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4366">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4366">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4367">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4367">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4368">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4368">'Identity'</span></span>
- <span data-ttu-id="6dd91-4369">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4369">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4370">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4370">'Razor'</span></span>
- <span data-ttu-id="6dd91-4371">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4371">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4372">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4372">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4373">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4373">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4374">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4374">'Identity'</span></span>
- <span data-ttu-id="6dd91-4375">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4375">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4376">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4376">'Razor'</span></span>
- <span data-ttu-id="6dd91-4377">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4377">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4378">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4378">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4379">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4379">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4380">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4380">'Identity'</span></span>
- <span data-ttu-id="6dd91-4381">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4381">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4382">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4382">'Razor'</span></span>
- <span data-ttu-id="6dd91-4383">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4383">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4384">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4384">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4385">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4385">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4386">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4386">'Identity'</span></span>
- <span data-ttu-id="6dd91-4387">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4387">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4388">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4388">'Razor'</span></span>
- <span data-ttu-id="6dd91-4389">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4389">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4390">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4390">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4391">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4391">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4392">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4392">'Identity'</span></span>
- <span data-ttu-id="6dd91-4393">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4393">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4394">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4394">'Razor'</span></span>
- <span data-ttu-id="6dd91-4395">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4395">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4396">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4396">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4397">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4397">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4398">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4398">'Identity'</span></span>
- <span data-ttu-id="6dd91-4399">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4399">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4400">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4400">'Razor'</span></span>
- <span data-ttu-id="6dd91-4401">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4401">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4402">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4402">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4403">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4403">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4404">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4404">'Identity'</span></span>
- <span data-ttu-id="6dd91-4405">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4405">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4406">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4406">'Razor'</span></span>
- <span data-ttu-id="6dd91-4407">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4407">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4408">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4408">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4409">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4409">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4410">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4410">'Identity'</span></span>
- <span data-ttu-id="6dd91-4411">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4411">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4412">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4412">'Razor'</span></span>
- <span data-ttu-id="6dd91-4413">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4413">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4414">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4414">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4415">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4415">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4416">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4416">'Identity'</span></span>
- <span data-ttu-id="6dd91-4417">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4417">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4418">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4418">'Razor'</span></span>
- <span data-ttu-id="6dd91-4419">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4419">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4420">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4420">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4421">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4421">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4422">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4422">'Identity'</span></span>
- <span data-ttu-id="6dd91-4423">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4423">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4424">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4424">'Razor'</span></span>
- <span data-ttu-id="6dd91-4425">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4425">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4426">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4426">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4427">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4427">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4428">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4428">'Identity'</span></span>
- <span data-ttu-id="6dd91-4429">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4429">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4430">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4430">'Razor'</span></span>
- <span data-ttu-id="6dd91-4431">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4431">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4432">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4432">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4433">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4433">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4434">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4434">'Identity'</span></span>
- <span data-ttu-id="6dd91-4435">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4435">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4436">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4436">'Razor'</span></span>
- <span data-ttu-id="6dd91-4437">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4437">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4438">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4438">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4439">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4439">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4440">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4440">'Identity'</span></span>
- <span data-ttu-id="6dd91-4441">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4441">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4442">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4442">'Razor'</span></span>
- <span data-ttu-id="6dd91-4443">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4443">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4444">------------------------------------- | |`hello`                                  | `/hello`               |僅符合單一路徑 `/hello` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4444">------------------------------------- | | `hello`                                  | `/hello`                | Only matches the single path `/hello`.</span></span>                                     <span data-ttu-id="6dd91-4445">| |`{Page=Home}`                            | `/`                    |符合並將設定 `Page` 為 `Home` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4445">| | `{Page=Home}`                            | `/`                     | Matches and sets `Page` to `Home`.</span></span>                                         <span data-ttu-id="6dd91-4446">| |`{Page=Home}`                            | `/Contact`             |符合並將設定 `Page` 為 `Contact` 。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4446">| | `{Page=Home}`                            | `/Contact`              | Matches and sets `Page` to `Contact`.</span></span>                                      <span data-ttu-id="6dd91-4447">| |`{controller}/{action}/{id?}`            | `/Products/List`       |對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4447">| | `{controller}/{action}/{id?}`            | `/Products/List`        | Maps to the `Products` controller and `List` action.</span></span>                       <span data-ttu-id="6dd91-4448">| |`{controller}/{action}/{id?}`            | `/Products/Details/123`|對應至 `Products` 控制器和 `Details` 動作（ `id` 設定為123）。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4448">| | `{controller}/{action}/{id?}`            | `/Products/Details/123` | Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> <span data-ttu-id="6dd91-4449">| |`{controller=Home}/{action=Index}/{id?}` | `/`                    |對應至 `Home` 控制器和 `Index` 方法（ `id` 會被忽略）。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4449">| | `{controller=Home}/{action=Index}/{id?}` | `/`                     | Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="6dd91-4450">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4450">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="6dd91-4451">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4451">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="6dd91-4452">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4452">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="6dd91-4453">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-4453">Route constraint reference</span></span>

<span data-ttu-id="6dd91-4454">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4454">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="6dd91-4455">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4455">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="6dd91-4456">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4456">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="6dd91-4457">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4457">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="6dd91-4458">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4458">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd91-4459">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4459">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="6dd91-4460">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」\*\* 回應，而不是「400 - 錯誤要求」\*\* 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4460">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="6dd91-4461">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4461">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="6dd91-4462">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4462">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="6dd91-4463">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="6dd91-4463">constraint</span></span> | <span data-ttu-id="6dd91-4464">範例</span><span class="sxs-lookup"><span data-stu-id="6dd91-4464">Example</span></span> | <span data-ttu-id="6dd91-4465">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="6dd91-4465">Example Matches</span></span> | <span data-ttu-id="6dd91-4466">備忘錄</span><span class="sxs-lookup"><span data-stu-id="6dd91-4466">Notes</span></span> |
| ---
<span data-ttu-id="6dd91-4467">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4467">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4468">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4468">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4469">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4469">'Identity'</span></span>
- <span data-ttu-id="6dd91-4470">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4470">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4471">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4471">'Razor'</span></span>
- <span data-ttu-id="6dd91-4472">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4472">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4473">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4473">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4474">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4474">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4475">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4475">'Identity'</span></span>
- <span data-ttu-id="6dd91-4476">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4476">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4477">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4477">'Razor'</span></span>
- <span data-ttu-id="6dd91-4478">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4478">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4479">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4479">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4480">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4480">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4481">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4481">'Identity'</span></span>
- <span data-ttu-id="6dd91-4482">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4482">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4483">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4483">'Razor'</span></span>
- <span data-ttu-id="6dd91-4484">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4484">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4485">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4485">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4486">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4486">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4487">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4487">'Identity'</span></span>
- <span data-ttu-id="6dd91-4488">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4488">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4489">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4489">'Razor'</span></span>
- <span data-ttu-id="6dd91-4490">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4490">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4491">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4491">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4492">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4492">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4493">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4493">'Identity'</span></span>
- <span data-ttu-id="6dd91-4494">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4494">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4495">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4495">'Razor'</span></span>
- <span data-ttu-id="6dd91-4496">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4496">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4497">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4497">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4498">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4498">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4499">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4499">'Identity'</span></span>
- <span data-ttu-id="6dd91-4500">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4500">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4501">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4501">'Razor'</span></span>
- <span data-ttu-id="6dd91-4502">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4502">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4503">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4503">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4504">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4504">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4505">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4505">'Identity'</span></span>
- <span data-ttu-id="6dd91-4506">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4506">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4507">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4507">'Razor'</span></span>
- <span data-ttu-id="6dd91-4508">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4508">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4509">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4509">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4510">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4510">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4511">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4511">'Identity'</span></span>
- <span data-ttu-id="6dd91-4512">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4512">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4513">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4513">'Razor'</span></span>
- <span data-ttu-id="6dd91-4514">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4514">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4515">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4515">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4516">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4516">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4517">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4517">'Identity'</span></span>
- <span data-ttu-id="6dd91-4518">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4518">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4519">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4519">'Razor'</span></span>
- <span data-ttu-id="6dd91-4520">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4520">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4521">-------- |----- | |`int` | `{id:int}` | `123456789`, `-123456789` |符合任何整數 | |`bool` | `{active:bool}` | `true`, `FALSE` |符合 `true` 或 `false` （不區分大小寫） | | `datetime`  |  `{dob:datetime}`  |  `2016-12-31` 、 `2016-12-31 7:32pm` |符合不因文化特性而異的有效 `DateTime` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4521">-------- | ----- | | `int` | `{id:int}` | `123456789`, `-123456789` | Matches any integer | | `bool` | `{active:bool}` | `true`, `FALSE` | Matches `true` or `false` (case-insensitive) | | `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Matches a valid `DateTime` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-4522">請參閱先前的警告。 ||`decimal` | `{price:decimal}` | `49.99`, `-1,000.01` |符合不因文化特性而異的有效 `decimal` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4522">See  preceding warning.| | `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Matches a valid `decimal` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-4523">請參閱先前的警告。 ||`double` | `{weight:double}` | `1.234`, `-1,001.01e8` |符合不因文化特性而異的有效 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4523">See  preceding warning.| | `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Matches a valid `double` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-4524">請參閱先前的警告。 ||`float` | `{weight:float}` | `1.234`, `-1,001.01e8` |符合不因文化特性而異的有效 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4524">See  preceding warning.| | `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Matches a valid `float` value in the invariant culture.</span></span> <span data-ttu-id="6dd91-4525">請參閱先前的警告。 ||`guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` |符合有效的 `Guid` 值 | | `long`  |  `{ticks:long}`  |  `123456789` 、 `-123456789` |符合有效的 `long` 值 | | `minlength(value)`  |  `{username:minlength(4)}`  |  `Rick` |字串必須至少有4個字元 | |`maxlength(value)` | `{filename:maxlength(8)}` | `Richard`|字串不能超過8個字元 | |`length(length)` | `{filename:length(12)}` | `somefile.txt`|字串的長度必須剛好是12個字元 | |`length(min,max)` | `{filename:length(8,16)}` | `somefile.txt`|字串必須至少為8，且長度不能超過16個字元 | |`min(value)` | `{age:min(18)}` | `19`|整數值必須至少為 18 | |`max(value)` | `{age:max(120)}` | `91`|整數值不能超過 120 | |`range(min,max)` | `{age:range(18,120)}` | `91`|整數值必須至少為18，但不能超過 120 | |`alpha` | `{name:alpha}` | `Rick`|字串必須包含一或多個字母字元（ `a` - `z` ，不區分大小寫） | | `regex(expression)`  |  `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}`  |  `123-45-6789` |字串必須符合正則運算式（請參閱定義正則運算式的秘訣） | |`required` | `{name:required}` | `Rick`|用來強制在 URL 產生期間出現非參數值 |</span><span class="sxs-lookup"><span data-stu-id="6dd91-4525">See  preceding warning.| | `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Matches a valid `Guid` value | | `long` | `{ticks:long}` | `123456789`, `-123456789` | Matches a valid `long` value | | `minlength(value)` | `{username:minlength(4)}` | `Rick` | String must be at least 4 characters | | `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | String must be no more than 8 characters | | `length(length)` | `{filename:length(12)}` | `somefile.txt` | String must be exactly 12 characters long | | `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | String must be at least 8 and no more than 16 characters long | | `min(value)` | `{age:min(18)}` | `19` | Integer value must be at least 18 | | `max(value)` | `{age:max(120)}` | `91` | Integer value must be no more than 120 | | `range(min,max)` | `{age:range(18,120)}` | `91` | Integer value must be at least 18 but no more than 120 | | `alpha` | `{name:alpha}` | `Rick` | String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive) | | `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | String must match the regular expression (see tips about defining a regular expression) | | `required` | `{name:required}` | `Rick` | Used to enforce that a non-parameter value is present during URL generation |</span></span>

<span data-ttu-id="6dd91-4526">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4526">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="6dd91-4527">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4527">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="6dd91-4528">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4528">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="6dd91-4529">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4529">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="6dd91-4530">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4530">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="6dd91-4531">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4531">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="6dd91-4532">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4532">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="6dd91-4533">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-4533">Regular expressions</span></span>

<span data-ttu-id="6dd91-4534">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4534">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="6dd91-4535">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4535">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="6dd91-4536">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4536">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="6dd91-4537">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4537">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="6dd91-4538">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4538">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="6dd91-4539">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="6dd91-4539">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="6dd91-4540">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4540">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="6dd91-4541">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-4541">Regular Expression</span></span>    | <span data-ttu-id="6dd91-4542">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-4542">Escaped Regular Expression</span></span>     |
| ---
<span data-ttu-id="6dd91-4543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4544">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4545">'Identity'</span></span>
- <span data-ttu-id="6dd91-4546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4546">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4547">'Razor'</span></span>
- <span data-ttu-id="6dd91-4548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4550">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4551">'Identity'</span></span>
- <span data-ttu-id="6dd91-4552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4552">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4553">'Razor'</span></span>
- <span data-ttu-id="6dd91-4554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4556">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4557">'Identity'</span></span>
- <span data-ttu-id="6dd91-4558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4558">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4559">'Razor'</span></span>
- <span data-ttu-id="6dd91-4560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4562">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4563">'Identity'</span></span>
- <span data-ttu-id="6dd91-4564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4564">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4565">'Razor'</span></span>
- <span data-ttu-id="6dd91-4566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4568">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4569">'Identity'</span></span>
- <span data-ttu-id="6dd91-4570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4570">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4571">'Razor'</span></span>
- <span data-ttu-id="6dd91-4572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4574">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4575">'Identity'</span></span>
- <span data-ttu-id="6dd91-4576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4576">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4577">'Razor'</span></span>
- <span data-ttu-id="6dd91-4578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4578">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4579">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4579">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4580">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4581">'Identity'</span></span>
- <span data-ttu-id="6dd91-4582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4582">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4583">'Razor'</span></span>
- <span data-ttu-id="6dd91-4584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4584">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4585">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4585">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4586">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4586">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4587">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4587">'Identity'</span></span>
- <span data-ttu-id="6dd91-4588">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4588">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4589">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4589">'Razor'</span></span>
- <span data-ttu-id="6dd91-4590">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4590">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4591">----------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4591">----------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4592">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4592">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4593">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4593">'Identity'</span></span>
- <span data-ttu-id="6dd91-4594">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4594">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4595">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4595">'Razor'</span></span>
- <span data-ttu-id="6dd91-4596">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4596">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4597">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4597">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4598">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4598">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4599">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4599">'Identity'</span></span>
- <span data-ttu-id="6dd91-4600">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4600">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4601">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4601">'Razor'</span></span>
- <span data-ttu-id="6dd91-4602">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4602">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4604">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4605">'Identity'</span></span>
- <span data-ttu-id="6dd91-4606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4606">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4607">'Razor'</span></span>
- <span data-ttu-id="6dd91-4608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4608">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4609">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4609">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4610">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4611">'Identity'</span></span>
- <span data-ttu-id="6dd91-4612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4612">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4613">'Razor'</span></span>
- <span data-ttu-id="6dd91-4614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4614">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4615">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4615">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4616">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4617">'Identity'</span></span>
- <span data-ttu-id="6dd91-4618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4618">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4619">'Razor'</span></span>
- <span data-ttu-id="6dd91-4620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4622">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4623">'Identity'</span></span>
- <span data-ttu-id="6dd91-4624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4624">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4625">'Razor'</span></span>
- <span data-ttu-id="6dd91-4626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4627">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4627">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4628">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4628">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4629">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4629">'Identity'</span></span>
- <span data-ttu-id="6dd91-4630">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4630">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4631">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4631">'Razor'</span></span>
- <span data-ttu-id="6dd91-4632">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4632">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4633">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4633">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4634">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4634">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4635">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4635">'Identity'</span></span>
- <span data-ttu-id="6dd91-4636">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4636">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4637">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4637">'Razor'</span></span>
- <span data-ttu-id="6dd91-4638">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4638">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4639">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4639">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4640">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4640">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4641">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4641">'Identity'</span></span>
- <span data-ttu-id="6dd91-4642">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4642">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4643">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4643">'Razor'</span></span>
- <span data-ttu-id="6dd91-4644">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4644">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4645">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4645">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4646">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4646">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4647">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4647">'Identity'</span></span>
- <span data-ttu-id="6dd91-4648">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4648">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4649">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4649">'Razor'</span></span>
- <span data-ttu-id="6dd91-4650">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4650">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4651">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4651">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4652">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4652">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4653">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4653">'Identity'</span></span>
- <span data-ttu-id="6dd91-4654">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4654">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4655">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4655">'Razor'</span></span>
- <span data-ttu-id="6dd91-4656">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4656">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4657">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4657">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4658">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4658">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4659">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4659">'Identity'</span></span>
- <span data-ttu-id="6dd91-4660">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4660">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4661">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4661">'Razor'</span></span>
- <span data-ttu-id="6dd91-4662">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4662">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4663">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4663">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4664">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4664">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4665">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4665">'Identity'</span></span>
- <span data-ttu-id="6dd91-4666">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4666">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4667">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4667">'Razor'</span></span>
- <span data-ttu-id="6dd91-4668">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4668">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4669">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span><span class="sxs-lookup"><span data-stu-id="6dd91-4669">--------------- | | `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |</span></span>

<span data-ttu-id="6dd91-4670">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4670">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="6dd91-4671">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4671">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="6dd91-4672">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4672">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="6dd91-4673">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4673">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="6dd91-4674">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4674">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="6dd91-4675">運算式</span><span class="sxs-lookup"><span data-stu-id="6dd91-4675">Expression</span></span>   | <span data-ttu-id="6dd91-4676">String</span><span class="sxs-lookup"><span data-stu-id="6dd91-4676">String</span></span>    | <span data-ttu-id="6dd91-4677">比對</span><span class="sxs-lookup"><span data-stu-id="6dd91-4677">Match</span></span> | <span data-ttu-id="6dd91-4678">註解</span><span class="sxs-lookup"><span data-stu-id="6dd91-4678">Comment</span></span>               |
| ---
<span data-ttu-id="6dd91-4679">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4679">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4680">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4680">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4681">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4681">'Identity'</span></span>
- <span data-ttu-id="6dd91-4682">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4682">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4683">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4683">'Razor'</span></span>
- <span data-ttu-id="6dd91-4684">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4684">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4685">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4685">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4686">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4686">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4687">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4687">'Identity'</span></span>
- <span data-ttu-id="6dd91-4688">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4688">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4689">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4689">'Razor'</span></span>
- <span data-ttu-id="6dd91-4690">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4690">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4691">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4691">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4692">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4692">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4693">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4693">'Identity'</span></span>
- <span data-ttu-id="6dd91-4694">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4694">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4695">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4695">'Razor'</span></span>
- <span data-ttu-id="6dd91-4696">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4696">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4697">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4697">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4698">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4698">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4699">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4699">'Identity'</span></span>
- <span data-ttu-id="6dd91-4700">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4700">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4701">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4701">'Razor'</span></span>
- <span data-ttu-id="6dd91-4702">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4702">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4703">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4703">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4704">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4704">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4705">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4705">'Identity'</span></span>
- <span data-ttu-id="6dd91-4706">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4706">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4707">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4707">'Razor'</span></span>
- <span data-ttu-id="6dd91-4708">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4708">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4709">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4709">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4710">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4710">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4711">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4711">'Identity'</span></span>
- <span data-ttu-id="6dd91-4712">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4712">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4713">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4713">'Razor'</span></span>
- <span data-ttu-id="6dd91-4714">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4714">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4715">----- |:---: | ---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4715">----- | :---: |  --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4716">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4716">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4717">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4717">'Identity'</span></span>
- <span data-ttu-id="6dd91-4718">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4718">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4719">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4719">'Razor'</span></span>
- <span data-ttu-id="6dd91-4720">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4720">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4721">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4721">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4722">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4722">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4723">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4723">'Identity'</span></span>
- <span data-ttu-id="6dd91-4724">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4724">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4725">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4725">'Razor'</span></span>
- <span data-ttu-id="6dd91-4726">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4726">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4727">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4727">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4728">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4728">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4729">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4729">'Identity'</span></span>
- <span data-ttu-id="6dd91-4730">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4730">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4731">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4731">'Razor'</span></span>
- <span data-ttu-id="6dd91-4732">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4732">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4733">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4733">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4734">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4734">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4735">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4735">'Identity'</span></span>
- <span data-ttu-id="6dd91-4736">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4736">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4737">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4737">'Razor'</span></span>
- <span data-ttu-id="6dd91-4738">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4738">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4739">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4739">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4740">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4740">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4741">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4741">'Identity'</span></span>
- <span data-ttu-id="6dd91-4742">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4742">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4743">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4743">'Razor'</span></span>
- <span data-ttu-id="6dd91-4744">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4744">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4745">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4745">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4746">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4746">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4747">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4747">'Identity'</span></span>
- <span data-ttu-id="6dd91-4748">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4748">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4749">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4749">'Razor'</span></span>
- <span data-ttu-id="6dd91-4750">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4750">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4751">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4751">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4752">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4752">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4753">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4753">'Identity'</span></span>
- <span data-ttu-id="6dd91-4754">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4754">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4755">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4755">'Razor'</span></span>
- <span data-ttu-id="6dd91-4756">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4756">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4757">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4757">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4758">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4758">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4759">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4759">'Identity'</span></span>
- <span data-ttu-id="6dd91-4760">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4760">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4761">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4761">'Razor'</span></span>
- <span data-ttu-id="6dd91-4762">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4762">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4763">---------- || `[a-z]{2}`   |您好 |是 |子字串符合 || `[a-z]{2}`   |123abc456 |是 |子字串符合 || `[a-z]{2}`   |mz |是 |符合運算式 || `[a-z]{2}`   |MZ |是 |不區分大小寫 || `^[a-z]{2}$` |您好 |否 |查看 `^` 和更新版本 `$` | | `^[a-z]{2}$` | 123abc456 |否 |請參閱和更新版本 `^` `$` |</span><span class="sxs-lookup"><span data-stu-id="6dd91-4763">---------- | | `[a-z]{2}`   | hello     | Yes   | Substring matches     | | `[a-z]{2}`   | 123abc456 | Yes   | Substring matches     | | `[a-z]{2}`   | mz        | Yes   | Matches expression    | | `[a-z]{2}`   | MZ        | Yes   | Not case sensitive    | | `^[a-z]{2}$` | hello     | No    | See `^` and `$` above | | `^[a-z]{2}$` | 123abc456 | No    | See `^` and `$` above |</span></span>

<span data-ttu-id="6dd91-4764">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4764">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="6dd91-4765">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4765">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="6dd91-4766">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4766">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="6dd91-4767">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4767">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="6dd91-4768">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4768">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="6dd91-4769">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="6dd91-4769">Custom Route Constraints</span></span>

<span data-ttu-id="6dd91-4770">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4770">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="6dd91-4771"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4771">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="6dd91-4772">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4772">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="6dd91-4773"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4773">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="6dd91-4774">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4774">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="6dd91-4775">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4775">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="6dd91-4776">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4776">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="6dd91-4777">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4777">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a><span data-ttu-id="6dd91-4778">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="6dd91-4778">URL generation reference</span></span>

<span data-ttu-id="6dd91-4779">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4779">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="6dd91-4780">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4780">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="6dd91-4781">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4781">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="6dd91-4782">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4782">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="6dd91-4783"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」\*\* 的集合。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4783">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="6dd91-4784">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4784">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="6dd91-4785">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4785">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6dd91-4786">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4786">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="6dd91-4787">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4787">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="6dd91-4788">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4788">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="6dd91-4789">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4789">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="6dd91-4790">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4790">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="6dd91-4791">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="6dd91-4791">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="6dd91-4792">環境值</span><span class="sxs-lookup"><span data-stu-id="6dd91-4792">Ambient Values</span></span>                     | <span data-ttu-id="6dd91-4793">明確值</span><span class="sxs-lookup"><span data-stu-id="6dd91-4793">Explicit Values</span></span>                        | <span data-ttu-id="6dd91-4794">結果</span><span class="sxs-lookup"><span data-stu-id="6dd91-4794">Result</span></span>                  |
| ---
<span data-ttu-id="6dd91-4795">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4795">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4796">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4796">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4797">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4797">'Identity'</span></span>
- <span data-ttu-id="6dd91-4798">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4798">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4799">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4799">'Razor'</span></span>
- <span data-ttu-id="6dd91-4800">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4800">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4801">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4801">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4802">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4802">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4803">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4803">'Identity'</span></span>
- <span data-ttu-id="6dd91-4804">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4804">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4805">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4805">'Razor'</span></span>
- <span data-ttu-id="6dd91-4806">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4806">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4807">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4807">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4808">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4808">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4809">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4809">'Identity'</span></span>
- <span data-ttu-id="6dd91-4810">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4810">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4811">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4811">'Razor'</span></span>
- <span data-ttu-id="6dd91-4812">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4812">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4813">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4813">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4814">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4814">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4815">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4815">'Identity'</span></span>
- <span data-ttu-id="6dd91-4816">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4816">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4817">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4817">'Razor'</span></span>
- <span data-ttu-id="6dd91-4818">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4818">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4819">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4819">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4820">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4820">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4821">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4821">'Identity'</span></span>
- <span data-ttu-id="6dd91-4822">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4822">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4823">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4823">'Razor'</span></span>
- <span data-ttu-id="6dd91-4824">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4824">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4825">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4825">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4826">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4826">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4827">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4827">'Identity'</span></span>
- <span data-ttu-id="6dd91-4828">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4828">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4829">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4829">'Razor'</span></span>
- <span data-ttu-id="6dd91-4830">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4830">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4831">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4831">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4832">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4832">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4833">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4833">'Identity'</span></span>
- <span data-ttu-id="6dd91-4834">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4834">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4835">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4835">'Razor'</span></span>
- <span data-ttu-id="6dd91-4836">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4836">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4837">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4837">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4838">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4838">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4839">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4839">'Identity'</span></span>
- <span data-ttu-id="6dd91-4840">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4840">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4841">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4841">'Razor'</span></span>
- <span data-ttu-id="6dd91-4842">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4842">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4843">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4843">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4844">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4844">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4845">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4845">'Identity'</span></span>
- <span data-ttu-id="6dd91-4846">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4846">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4847">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4847">'Razor'</span></span>
- <span data-ttu-id="6dd91-4848">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4848">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4849">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4849">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4850">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4850">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4851">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4851">'Identity'</span></span>
- <span data-ttu-id="6dd91-4852">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4852">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4853">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4853">'Razor'</span></span>
- <span data-ttu-id="6dd91-4854">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4854">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4855">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4855">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4856">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4856">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4857">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4857">'Identity'</span></span>
- <span data-ttu-id="6dd91-4858">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4858">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4859">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4859">'Razor'</span></span>
- <span data-ttu-id="6dd91-4860">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4860">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4861">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4861">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4862">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4862">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4863">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4863">'Identity'</span></span>
- <span data-ttu-id="6dd91-4864">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4864">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4865">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4865">'Razor'</span></span>
- <span data-ttu-id="6dd91-4866">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4866">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4867">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4867">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4868">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4868">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4869">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4869">'Identity'</span></span>
- <span data-ttu-id="6dd91-4870">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4870">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4871">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4871">'Razor'</span></span>
- <span data-ttu-id="6dd91-4872">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4872">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4873">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4873">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4874">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4874">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4875">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4875">'Identity'</span></span>
- <span data-ttu-id="6dd91-4876">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4876">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4877">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4877">'Razor'</span></span>
- <span data-ttu-id="6dd91-4878">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4878">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4879">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4879">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4880">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4880">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4881">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4881">'Identity'</span></span>
- <span data-ttu-id="6dd91-4882">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4882">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4883">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4883">'Razor'</span></span>
- <span data-ttu-id="6dd91-4884">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4884">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4885">----------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4885">----------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4886">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4886">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4887">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4887">'Identity'</span></span>
- <span data-ttu-id="6dd91-4888">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4888">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4889">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4889">'Razor'</span></span>
- <span data-ttu-id="6dd91-4890">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4890">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4891">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4891">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4892">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4892">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4893">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4893">'Identity'</span></span>
- <span data-ttu-id="6dd91-4894">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4894">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4895">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4895">'Razor'</span></span>
- <span data-ttu-id="6dd91-4896">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4896">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4897">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4897">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4898">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4898">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4899">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4899">'Identity'</span></span>
- <span data-ttu-id="6dd91-4900">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4900">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4901">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4901">'Razor'</span></span>
- <span data-ttu-id="6dd91-4902">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4902">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4903">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4903">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4904">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4904">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4905">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4905">'Identity'</span></span>
- <span data-ttu-id="6dd91-4906">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4906">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4907">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4907">'Razor'</span></span>
- <span data-ttu-id="6dd91-4908">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4908">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4909">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4909">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4910">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4910">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4911">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4911">'Identity'</span></span>
- <span data-ttu-id="6dd91-4912">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4912">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4913">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4913">'Razor'</span></span>
- <span data-ttu-id="6dd91-4914">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4914">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4915">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4915">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4916">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4916">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4917">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4917">'Identity'</span></span>
- <span data-ttu-id="6dd91-4918">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4918">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4919">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4919">'Razor'</span></span>
- <span data-ttu-id="6dd91-4920">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4920">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4921">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4921">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4922">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4922">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4923">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4923">'Identity'</span></span>
- <span data-ttu-id="6dd91-4924">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4924">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4925">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4925">'Razor'</span></span>
- <span data-ttu-id="6dd91-4926">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4926">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4927">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4927">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4928">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4928">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4929">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4929">'Identity'</span></span>
- <span data-ttu-id="6dd91-4930">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4930">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4931">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4931">'Razor'</span></span>
- <span data-ttu-id="6dd91-4932">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4932">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4933">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4933">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4934">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4934">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4935">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4935">'Identity'</span></span>
- <span data-ttu-id="6dd91-4936">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4936">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4937">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4937">'Razor'</span></span>
- <span data-ttu-id="6dd91-4938">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4938">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4939">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4939">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4940">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4940">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4941">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4941">'Identity'</span></span>
- <span data-ttu-id="6dd91-4942">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4942">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4943">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4943">'Razor'</span></span>
- <span data-ttu-id="6dd91-4944">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4944">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4945">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4945">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4946">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4946">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4947">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4947">'Identity'</span></span>
- <span data-ttu-id="6dd91-4948">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4948">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4949">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4949">'Razor'</span></span>
- <span data-ttu-id="6dd91-4950">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4950">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4951">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4951">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4952">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4952">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4953">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4953">'Identity'</span></span>
- <span data-ttu-id="6dd91-4954">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4954">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4955">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4955">'Razor'</span></span>
- <span data-ttu-id="6dd91-4956">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4956">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4957">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4957">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4958">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4958">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4959">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4959">'Identity'</span></span>
- <span data-ttu-id="6dd91-4960">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4960">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4961">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4961">'Razor'</span></span>
- <span data-ttu-id="6dd91-4962">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4962">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4963">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4963">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4964">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4964">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4965">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4965">'Identity'</span></span>
- <span data-ttu-id="6dd91-4966">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4966">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4967">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4967">'Razor'</span></span>
- <span data-ttu-id="6dd91-4968">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4968">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4969">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4969">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4970">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4970">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4971">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4971">'Identity'</span></span>
- <span data-ttu-id="6dd91-4972">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4972">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4973">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4973">'Razor'</span></span>
- <span data-ttu-id="6dd91-4974">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4974">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4975">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4975">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4976">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4976">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4977">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4977">'Identity'</span></span>
- <span data-ttu-id="6dd91-4978">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4978">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4979">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4979">'Razor'</span></span>
- <span data-ttu-id="6dd91-4980">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4980">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4981">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4981">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4982">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4982">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4983">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4983">'Identity'</span></span>
- <span data-ttu-id="6dd91-4984">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4984">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4985">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4985">'Razor'</span></span>
- <span data-ttu-id="6dd91-4986">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4986">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-4987">------------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4987">------------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4988">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4988">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4989">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4989">'Identity'</span></span>
- <span data-ttu-id="6dd91-4990">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4990">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4991">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4991">'Razor'</span></span>
- <span data-ttu-id="6dd91-4992">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4992">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4993">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4993">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-4994">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4994">'Blazor'</span></span>
- <span data-ttu-id="6dd91-4995">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4995">'Identity'</span></span>
- <span data-ttu-id="6dd91-4996">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4996">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-4997">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-4997">'Razor'</span></span>
- <span data-ttu-id="6dd91-4998">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4998">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-4999">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-4999">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5000">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5000">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5001">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5001">'Identity'</span></span>
- <span data-ttu-id="6dd91-5002">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5002">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5003">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5003">'Razor'</span></span>
- <span data-ttu-id="6dd91-5004">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5004">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5005">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5005">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5006">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5006">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5007">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5007">'Identity'</span></span>
- <span data-ttu-id="6dd91-5008">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5008">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5009">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5009">'Razor'</span></span>
- <span data-ttu-id="6dd91-5010">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5010">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5011">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5011">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5012">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5012">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5013">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5013">'Identity'</span></span>
- <span data-ttu-id="6dd91-5014">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5014">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5015">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5015">'Razor'</span></span>
- <span data-ttu-id="6dd91-5016">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5016">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5017">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5017">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5018">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5018">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5019">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5019">'Identity'</span></span>
- <span data-ttu-id="6dd91-5020">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5020">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5021">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5021">'Razor'</span></span>
- <span data-ttu-id="6dd91-5022">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5022">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5023">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5023">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5024">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5024">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5025">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5025">'Identity'</span></span>
- <span data-ttu-id="6dd91-5026">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5026">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5027">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5027">'Razor'</span></span>
- <span data-ttu-id="6dd91-5028">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5028">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5029">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5029">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5030">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5030">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5031">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5031">'Identity'</span></span>
- <span data-ttu-id="6dd91-5032">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5032">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5033">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5033">'Razor'</span></span>
- <span data-ttu-id="6dd91-5034">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5034">'SignalR' uid:</span></span> 

-
<span data-ttu-id="6dd91-5035">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5035">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6dd91-5036">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5036">'Blazor'</span></span>
- <span data-ttu-id="6dd91-5037">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5037">'Identity'</span></span>
- <span data-ttu-id="6dd91-5038">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5038">'Let's Encrypt'</span></span>
- <span data-ttu-id="6dd91-5039">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6dd91-5039">'Razor'</span></span>
- <span data-ttu-id="6dd91-5040">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5040">'SignalR' uid:</span></span> 

<span data-ttu-id="6dd91-5041">------------ | |控制器 = "Home" |action = "About" |`/Home/About`|
|控制器 = "Home" |控制器 = "Order"，action = "About" |`/Order/About`|
|控制器 = "Home"，color = "Red" |action = "About" |`/Home/About`|
|控制器 = "Home" |action = "About"，color = "Red" |`/Home/About?color=Red`                                |</span><span class="sxs-lookup"><span data-stu-id="6dd91-5041">------------ | | controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |</span></span>

<span data-ttu-id="6dd91-5042">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="6dd91-5042">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="6dd91-5043">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6dd91-5043">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="6dd91-5044">複雜區段</span><span class="sxs-lookup"><span data-stu-id="6dd91-5044">Complex segments</span></span>

<span data-ttu-id="6dd91-5045">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="6dd91-5045">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="6dd91-5046">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="6dd91-5046">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="6dd91-5047">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="6dd91-5047">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>

::: moniker-end
