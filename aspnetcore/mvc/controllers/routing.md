---
<span data-ttu-id="82fb7-101">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-101">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-102">'Blazor'</span></span>
- <span data-ttu-id="82fb7-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-103">'Identity'</span></span>
- <span data-ttu-id="82fb7-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-105">'Razor'</span></span>
- <span data-ttu-id="82fb7-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-106">'SignalR' uid:</span></span> 

---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="82fb7-107">ASP.NET Core 中的路由至控制器動作</span><span class="sxs-lookup"><span data-stu-id="82fb7-107">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="82fb7-108">By [Ryan Nowak](https://github.com/rynowak)、 [Kirk Larkin](https://twitter.com/serpent5)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82fb7-108">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82fb7-109">ASP.NET Core 控制器會使用路由[中介軟體](xref:fundamentals/middleware/index)來比對傳入要求的 url，並將其對應至[動作](#action)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-109">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="82fb7-110">路由範本：</span><span class="sxs-lookup"><span data-stu-id="82fb7-110">Routes templates:</span></span>

* <span data-ttu-id="82fb7-111">會在啟始程式碼或屬性中定義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-111">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="82fb7-112">描述如何將 URL 路徑與[動作](#action)比對。</span><span class="sxs-lookup"><span data-stu-id="82fb7-112">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="82fb7-113">是用來產生連結的 Url。</span><span class="sxs-lookup"><span data-stu-id="82fb7-113">Are used to generate URLs for links.</span></span> <span data-ttu-id="82fb7-114">產生的連結通常會在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="82fb7-114">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="82fb7-115">動作可以是 [[傳統路由](#cr)] 或 [[屬性路由](#ar)]。</span><span class="sxs-lookup"><span data-stu-id="82fb7-115">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="82fb7-116">將路由放在控制器或[動作](#action)上，會使其成為屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-116">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="82fb7-117">如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-117">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="82fb7-118">本檔：</span><span class="sxs-lookup"><span data-stu-id="82fb7-118">This document:</span></span>

* <span data-ttu-id="82fb7-119">說明 MVC 與路由之間的互動：</span><span class="sxs-lookup"><span data-stu-id="82fb7-119">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="82fb7-120">一般 MVC 應用程式如何利用路由功能。</span><span class="sxs-lookup"><span data-stu-id="82fb7-120">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="82fb7-121">涵蓋兩者：</span><span class="sxs-lookup"><span data-stu-id="82fb7-121">Covers both:</span></span>
    * <span data-ttu-id="82fb7-122">一般[路由](#cr)通常與控制器和 views 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="82fb7-122">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="82fb7-123">搭配 REST Api 使用的*屬性路由*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-123">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="82fb7-124">如果您主要對 REST Api 的路由感興趣，請跳至[Rest api 的屬性路由](#ar)一節。</span><span class="sxs-lookup"><span data-stu-id="82fb7-124">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="82fb7-125">如需高階路由詳細資料，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-125">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="82fb7-126">指的是在 ASP.NET Core 3.0 中新增的預設路由系統，稱為端點路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-126">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="82fb7-127">基於相容性目的，您可以使用具有舊版路由的控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-127">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="82fb7-128">如需相關指示，請參閱[2.2-3.0 遷移指南](xref:migration/22-to-30)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-128">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="82fb7-129">如需舊版路由系統上的參考資料，請參閱[本檔的2.2 版本](xref:mvc/controllers/routing?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-129">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="82fb7-130">設定傳統路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-130">Set up conventional route</span></span>

<span data-ttu-id="82fb7-131">`Startup.Configure`使用[傳統路由](#crd)時，通常會有類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="82fb7-131">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="82fb7-132">在呼叫內 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> ， <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> 是用來建立單一路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-132">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="82fb7-133">單一路由的名稱為 `default` route。</span><span class="sxs-lookup"><span data-stu-id="82fb7-133">The single route is named `default` route.</span></span> <span data-ttu-id="82fb7-134">大部分具有控制器和視圖的應用程式會使用類似路由的路由範本 `default` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-134">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="82fb7-135">REST Api 應該使用[屬性路由](#ar)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-135">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="82fb7-136">路由範本 `"{controller=Home}/{action=Index}/{id?}"` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-136">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="82fb7-137">符合的 URL 路徑，例如`/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="82fb7-137">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="82fb7-138">藉 `{ controller = Products, action = Details, id = 5 }` 由 token 化路徑來解壓縮路由值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-138">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="82fb7-139">如果應用程式具有名為的控制器和動作，則提取路由值會產生相符的結果 `ProductsController` `Details` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-139">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

  [!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

* <span data-ttu-id="82fb7-140">`/Products/Details/5`model 會系結的值 `id = 5` ，將 `id` 參數設定為 `5` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-140">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="82fb7-141">如需詳細資訊，請參閱[模型](xref:mvc/models/model-binding)系結。</span><span class="sxs-lookup"><span data-stu-id="82fb7-141">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="82fb7-142">`{controller=Home}`定義 `Home` 為預設值 `controller` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-142">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="82fb7-143">`{action=Index}`定義 `Index` 為預設值 `action` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-143">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="82fb7-144">中的字元會將 `?` `{id?}` 定義 `id` 為選擇性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-144">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="82fb7-145">預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。</span><span class="sxs-lookup"><span data-stu-id="82fb7-145">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="82fb7-146">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-146">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="82fb7-147">符合 URL 路徑 `/` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-147">Matches the URL path `/`.</span></span>
* <span data-ttu-id="82fb7-148">產生路由值 `{ controller = Home, action = Index }` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-148">Produces the route values `{ controller = Home, action = Index }`.</span></span>

<span data-ttu-id="82fb7-149">和的值 `controller` 會 `action` 使用預設值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-149">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="82fb7-150">`id`不會產生值，因為 URL 路徑中沒有對應的區段。</span><span class="sxs-lookup"><span data-stu-id="82fb7-150">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="82fb7-151">`/`只有在有和動作存在時才會相符 `HomeController` `Index` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-151">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="82fb7-152">使用上述的控制器定義和路由範本， `HomeController.Index` 動作會針對下列 URL 路徑執行：</span><span class="sxs-lookup"><span data-stu-id="82fb7-152">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="82fb7-153">URL 路徑會 `/` 使用路由範本的預設 `Home` 控制器和 `Index` 動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-153">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="82fb7-154">URL 路徑會 `/Home` 使用路由範本預設 `Index` 動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-154">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="82fb7-155"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*> 方法很方便：</span><span class="sxs-lookup"><span data-stu-id="82fb7-155">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="82fb7-156">代替</span><span class="sxs-lookup"><span data-stu-id="82fb7-156">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

> [!IMPORTANT]
> <span data-ttu-id="82fb7-157">路由是使用 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> 和 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> 中介軟體來設定。</span><span class="sxs-lookup"><span data-stu-id="82fb7-157">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="82fb7-158">若要使用控制器：</span><span class="sxs-lookup"><span data-stu-id="82fb7-158">To use controllers:</span></span>
>
> * <span data-ttu-id="82fb7-159"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>在內部呼叫 `UseEndpoints` 來對應[屬性路由](#ar)控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-159">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
> * <span data-ttu-id="82fb7-160">呼叫 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> 或 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> ，以對應[傳統路由](#cr)控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-160">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="82fb7-161">慣例路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-161">Conventional routing</span></span>

<span data-ttu-id="82fb7-162">傳統路由會與控制器和 views 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="82fb7-162">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="82fb7-163">`default` 路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-163">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="82fb7-164">即為「慣例路由」\*\* 的範例。</span><span class="sxs-lookup"><span data-stu-id="82fb7-164">is an example of a *conventional routing*.</span></span> <span data-ttu-id="82fb7-165">這稱為「*傳統路由*」，因為它會建立 URL 路徑的*慣例*：</span><span class="sxs-lookup"><span data-stu-id="82fb7-165">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="82fb7-166">第一個路徑區段會 `{controller=Home}` 對應到控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-166">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="82fb7-167">第二個區段會 `{action=Index}` 對應到[動作](#action)名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-167">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="82fb7-168">第三個區段 `{id?}` 是用於選擇性的 `id` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-168">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="82fb7-169">`?`中的 `{id?}` 可讓它成為選擇性的。</span><span class="sxs-lookup"><span data-stu-id="82fb7-169">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="82fb7-170">`id`是用來對應至模型實體。</span><span class="sxs-lookup"><span data-stu-id="82fb7-170">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="82fb7-171">使用此 `default` 路由，URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="82fb7-171">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="82fb7-172">`/Products/List`對應至 `ProductsController.List` 動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-172">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="82fb7-173">`/Blog/Article/17`對應至 `BlogController.Article` ，而且通常會將 `id` 參數系結至17。</span><span class="sxs-lookup"><span data-stu-id="82fb7-173">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="82fb7-174">這種對應：</span><span class="sxs-lookup"><span data-stu-id="82fb7-174">This mapping:</span></span>

* <span data-ttu-id="82fb7-175">**僅**以控制器和[動作](#action)名稱為基礎。</span><span class="sxs-lookup"><span data-stu-id="82fb7-175">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="82fb7-176">不是以命名空間、來源檔案位置或方法參數為基礎。</span><span class="sxs-lookup"><span data-stu-id="82fb7-176">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="82fb7-177">使用傳統路由搭配預設路由可讓您建立應用程式，而不需要為每個動作加上新的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-177">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="82fb7-178">針對具有[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)樣式動作的應用程式，具有跨控制器的 url 一致性：</span><span class="sxs-lookup"><span data-stu-id="82fb7-178">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="82fb7-179">有助於簡化程式碼。</span><span class="sxs-lookup"><span data-stu-id="82fb7-179">Helps simplify the code.</span></span>
* <span data-ttu-id="82fb7-180">讓 UI 更容易預測。</span><span class="sxs-lookup"><span data-stu-id="82fb7-180">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="82fb7-181">`id`上述程式碼中的會透過路由範本定義為選擇性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-181">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="82fb7-182">動作可以執行，而不需要提供做為 URL 一部分的選擇性識別碼。</span><span class="sxs-lookup"><span data-stu-id="82fb7-182">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="82fb7-183">一般而言， `id` 從 URL 中省略時：</span><span class="sxs-lookup"><span data-stu-id="82fb7-183">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="82fb7-184">`id`由模型系結設定為 `0` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-184">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="82fb7-185">在資料庫中找不到任何符合的實體 `id == 0` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-185">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="82fb7-186">[屬性路由](#ar)提供更細微的控制，讓某些動作需要的識別碼，而不是其他動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-186">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="82fb7-187">依照慣例，檔會包含選擇性參數，例如 `id` 當它們可能出現在正確的使用方式時。</span><span class="sxs-lookup"><span data-stu-id="82fb7-187">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="82fb7-188">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-188">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="82fb7-189">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="82fb7-189">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="82fb7-190">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="82fb7-190">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="82fb7-191">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-191">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="82fb7-192">是許多 web UI 應用程式唯一需要的路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-192">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="82fb7-193">對於較大的 web UI 應用程式，如果經常需要，請使用[區域](#areas)的另一個路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-193">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="82fb7-194"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和 <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-194"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="82fb7-195">根據叫用的順序，自動指派**訂單**值給其端點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-195">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="82fb7-196">ASP.NET Core 3.0 和更新版本中的端點路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-196">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="82fb7-197">沒有路由的概念。</span><span class="sxs-lookup"><span data-stu-id="82fb7-197">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="82fb7-198">不會提供擴充性執行的順序保證，所有端點都會一次處理。</span><span class="sxs-lookup"><span data-stu-id="82fb7-198">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="82fb7-199">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="82fb7-199">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="82fb7-200">本檔稍後會說明[屬性路由](#ar)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-200">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="82fb7-201">多個傳統路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-201">Multiple conventional routes</span></span>

<span data-ttu-id="82fb7-202">藉[conventional routes](#cr) `UseEndpoints` 由新增更多對和的呼叫，可以在內加入多個傳統路由 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-202">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="82fb7-203">這麼做可讓您定義多個慣例，或新增特定[動作](#action)專用的傳統路由，例如：</span><span class="sxs-lookup"><span data-stu-id="82fb7-203">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="82fb7-204">`blog`上述程式碼中的路由是**專用的傳統路由**。</span><span class="sxs-lookup"><span data-stu-id="82fb7-204">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="82fb7-205">這稱為專用的傳統路由，因為：</span><span class="sxs-lookup"><span data-stu-id="82fb7-205">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="82fb7-206">它會使用[傳統路由](#cr)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-206">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="82fb7-207">其專屬於特定的[動作](#action)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-207">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="82fb7-208">因為 `controller` 和 `action` 不會出現在路由範本中 `"blog/{*article}"` 做為參數：</span><span class="sxs-lookup"><span data-stu-id="82fb7-208">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="82fb7-209">它們只能有預設值 `{ controller = "Blog", action = "Article" }` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-209">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="82fb7-210">此路由一律會對應至動作 `BlogController.Article` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-210">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="82fb7-211">`/Blog`、 `/Blog/Article` 和 `/Blog/{any-string}` 是唯一符合 blog 路由的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="82fb7-211">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="82fb7-212">上述範例：</span><span class="sxs-lookup"><span data-stu-id="82fb7-212">The preceding example:</span></span>

* <span data-ttu-id="82fb7-213">`blog`路由的比對優先順序高於 `default` 路由，因為它會先新增。</span><span class="sxs-lookup"><span data-stu-id="82fb7-213">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="82fb7-214">這是[輔助](https://developer.mozilla.org/docs/Glossary/Slug)專案樣式路由的範例，在此情況下，通常會有發行項名稱做為 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-214">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="82fb7-215">在 ASP.NET Core 3.0 和更新版本中，路由不會：</span><span class="sxs-lookup"><span data-stu-id="82fb7-215">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="82fb7-216">定義稱為「*路由*」的概念。</span><span class="sxs-lookup"><span data-stu-id="82fb7-216">Define a concept called a *route*.</span></span> <span data-ttu-id="82fb7-217">`UseRouting`將路由對應新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="82fb7-217">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="82fb7-218">`UseRouting`中介軟體會查看應用程式中所定義的端點集合，並根據要求選取最佳的端點相符項。</span><span class="sxs-lookup"><span data-stu-id="82fb7-218">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="82fb7-219">提供如或等擴充性執行順序的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 保證 <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-219">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="82fb7-220">如需路由的參考資料，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-220">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="82fb7-221">傳統路由順序</span><span class="sxs-lookup"><span data-stu-id="82fb7-221">Conventional routing order</span></span>

<span data-ttu-id="82fb7-222">傳統路由只會符合應用程式所定義的動作和控制器組合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-222">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="82fb7-223">這是為了簡化傳統路由重迭的情況。</span><span class="sxs-lookup"><span data-stu-id="82fb7-223">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="82fb7-224">使用、和新增路由，並根據叫用 <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*> <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> 的順序，自動指派順序值給其端點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-224">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="82fb7-225">從稍早出現的路由進行比對的優先順序較高。</span><span class="sxs-lookup"><span data-stu-id="82fb7-225">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="82fb7-226">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="82fb7-226">Conventional routing is order-dependent.</span></span> <span data-ttu-id="82fb7-227">一般而言，具有區域的路由應該放在較早的位置，因為它們比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="82fb7-227">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="82fb7-228">具有 catch-all 路由參數（例如）的[專用傳統路由](#dcr) `{*article}` 會使路由過於[貪婪](xref:fundamentals/routing#greedy)，這表示它會比對您要與其他路由比對的 url。</span><span class="sxs-lookup"><span data-stu-id="82fb7-228">[Dedicated conventional routes](#dcr) with catch-all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="82fb7-229">將貪婪路由放在路由表中，以避免發生貪婪的相符。</span><span class="sxs-lookup"><span data-stu-id="82fb7-229">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

[!INCLUDE[](~/includes/catchall.md)]

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="82fb7-230">解決不明確的動作</span><span class="sxs-lookup"><span data-stu-id="82fb7-230">Resolving ambiguous actions</span></span>

<span data-ttu-id="82fb7-231">當兩個端點透過路由比對時，路由必須執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="82fb7-231">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="82fb7-232">選擇最佳的候選。</span><span class="sxs-lookup"><span data-stu-id="82fb7-232">Choose the best candidate.</span></span>
* <span data-ttu-id="82fb7-233">擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="82fb7-233">Throw an exception.</span></span>

<span data-ttu-id="82fb7-234">例如：</span><span class="sxs-lookup"><span data-stu-id="82fb7-234">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="82fb7-235">先前的控制器會定義兩個符合的動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-235">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="82fb7-236">URL 路徑`/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="82fb7-236">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="82fb7-237">路由資料 `{ controller = Products33, action = Edit, id = 17 }` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-237">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="82fb7-238">這是 MVC 控制器的一般模式：</span><span class="sxs-lookup"><span data-stu-id="82fb7-238">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="82fb7-239">`Edit(int)`顯示編輯產品的表單。</span><span class="sxs-lookup"><span data-stu-id="82fb7-239">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="82fb7-240">`Edit(int, Product)`處理張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="82fb7-240">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="82fb7-241">若要解析正確的路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-241">To resolve the correct route:</span></span>

* <span data-ttu-id="82fb7-242">`Edit(int, Product)`當要求為 HTTP 時，會選取此選項 `POST` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-242">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="82fb7-243">`Edit(int)`當[HTTP 動詞](#verb)命令為其他任何專案時選取。</span><span class="sxs-lookup"><span data-stu-id="82fb7-243">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="82fb7-244">`Edit(int)`通常是透過來呼叫 `GET` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-244">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="82fb7-245"><xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute> `[HttpPost]` 會提供給路由，讓它可以根據要求的 HTTP 方法進行選擇。</span><span class="sxs-lookup"><span data-stu-id="82fb7-245">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="82fb7-246">`HttpPostAttribute`會 `Edit(int, Product)` 比更符合 `Edit(int)` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-246">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="82fb7-247">請務必瞭解屬性的角色，例如 `HttpPostAttribute` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-247">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="82fb7-248">也會針對其他[HTTP 動詞](#verb)命令定義類似的屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-248">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="82fb7-249">在[傳統路由](#cr)中，當動作屬於 [顯示表單]、[提交表單工作流程] 的一部分時，通常會使用相同的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-249">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="82fb7-250">例如，請參閱[檢查這兩個編輯動作方法](xref:tutorials/first-mvc-app/controller-methods-views#get-post)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-250">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="82fb7-251">如果路由無法選擇最佳候選， <xref:System.Reflection.AmbiguousMatchException> 則會擲回，並列出多個相符的端點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-251">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="82fb7-252">傳統路由名稱</span><span class="sxs-lookup"><span data-stu-id="82fb7-252">Conventional route names</span></span>

<span data-ttu-id="82fb7-253">`"blog"` `"default"` 下列範例中的字串和是傳統的路由名稱：</span><span class="sxs-lookup"><span data-stu-id="82fb7-253">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="82fb7-254">路由名稱提供路由邏輯名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-254">The route names give the route a logical name.</span></span> <span data-ttu-id="82fb7-255">命名路由可以用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-255">The named route can be used for URL generation.</span></span> <span data-ttu-id="82fb7-256">當路由順序可能會使 URL 產生變得複雜時，使用命名路由可簡化 URL 的建立。</span><span class="sxs-lookup"><span data-stu-id="82fb7-256">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="82fb7-257">路由名稱必須是唯一的應用程式範圍。</span><span class="sxs-lookup"><span data-stu-id="82fb7-257">Route names must be unique application wide.</span></span>

<span data-ttu-id="82fb7-258">路由名稱：</span><span class="sxs-lookup"><span data-stu-id="82fb7-258">Route names:</span></span>

* <span data-ttu-id="82fb7-259">不會影響對 URL 的對應或處理要求。</span><span class="sxs-lookup"><span data-stu-id="82fb7-259">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="82fb7-260">僅用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-260">Are used only for URL generation.</span></span>

<span data-ttu-id="82fb7-261">路由名稱概念會以[IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata)的形式呈現。</span><span class="sxs-lookup"><span data-stu-id="82fb7-261">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="82fb7-262">[**路由名稱**] 和 [**端點名稱**] 詞彙：</span><span class="sxs-lookup"><span data-stu-id="82fb7-262">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="82fb7-263">可互換。</span><span class="sxs-lookup"><span data-stu-id="82fb7-263">Are interchangeable.</span></span>
* <span data-ttu-id="82fb7-264">在檔和程式碼中使用哪一個，取決於所述的 API。</span><span class="sxs-lookup"><span data-stu-id="82fb7-264">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="82fb7-265">REST Api 的屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-265">Attribute routing for REST APIs</span></span>

<span data-ttu-id="82fb7-266">REST Api 應使用屬性路由，將應用程式的功能模型建立為一組資源，其中的作業是由[HTTP 指令動詞](#verb)表示。</span><span class="sxs-lookup"><span data-stu-id="82fb7-266">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="82fb7-267">屬性路由使用一組屬性，將動作直接對應至路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-267">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="82fb7-268">下列 `StartUp.Configure` 程式碼通常適用于 REST API，並在下一個範例中使用：</span><span class="sxs-lookup"><span data-stu-id="82fb7-268">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="82fb7-269">在上述程式碼中， <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> 會在內部呼叫 `UseEndpoints` 來對應屬性路由控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-269">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="82fb7-270">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="82fb7-270">In the following example:</span></span>

* <span data-ttu-id="82fb7-271">使用上述 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-271">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="82fb7-272">`HomeController`比對一組類似預設的慣例路由所符合的 Url `{controller=Home}/{action=Index}/{id?}` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-272">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="82fb7-273">`HomeController.Index`動作會針對任何 URL 路徑、、或執行 `/` `/Home` `/Home/Index` `/Home/Index/3` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-273">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="82fb7-274">此範例強調屬性路由和[傳統路由](#cr)之間的主要程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="82fb7-274">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="82fb7-275">屬性路由需要更多輸入才能指定路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-275">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="82fb7-276">傳統的預設路由會更簡潔地處理路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-276">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="82fb7-277">不過，屬性路由允許和需要精確控制每個[動作](#action)適用的路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-277">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="82fb7-278">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="82fb7-278">In the following code:</span></span>

* <span data-ttu-id="82fb7-279">控制器名稱和動作名稱**不**會扮演符合動作的角色。</span><span class="sxs-lookup"><span data-stu-id="82fb7-279">The controller name and action names play **no** role in which action is matched.</span></span>
* <span data-ttu-id="82fb7-280">符合與上一個範例相同的 Url：</span><span class="sxs-lookup"><span data-stu-id="82fb7-280">Matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="82fb7-281">下列程式碼會使用和的標記取代 `action` `controller` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-281">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="82fb7-282">下列程式碼適用于 `[Route("[controller]/[action]")]` 控制器：</span><span class="sxs-lookup"><span data-stu-id="82fb7-282">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="82fb7-283">在上述程式碼中， `Index` 方法樣板必須在 `/` 路由範本前面加上或 `~/` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-283">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="82fb7-284">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-284">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="82fb7-285">如需路由範本選取專案的詳細資訊，請參閱[路由範本優先順序](xref:fundamentals/routing#rtp)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-285">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="82fb7-286">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="82fb7-286">Reserved routing names</span></span>

<span data-ttu-id="82fb7-287">使用控制器或頁面時，下列關鍵字是保留的路由參數名稱 Razor ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-287">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="82fb7-288">使用 `page` 做為具有屬性路由的路由參數是常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="82fb7-288">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="82fb7-289">這樣做會導致 URL 產生不一致且混亂的行為。</span><span class="sxs-lookup"><span data-stu-id="82fb7-289">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="82fb7-290">URL 產生會使用特殊參數名稱來判斷 URL 產生作業是指 Razor 頁面還是控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-290">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="82fb7-291">HTTP 動詞範本</span><span class="sxs-lookup"><span data-stu-id="82fb7-291">HTTP verb templates</span></span>

<span data-ttu-id="82fb7-292">ASP.NET Core 具有下列 HTTP 動詞範本：</span><span class="sxs-lookup"><span data-stu-id="82fb7-292">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="82fb7-293">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-293">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="82fb7-294">[HttpPost](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-294">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="82fb7-295">[HttpPut](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-295">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="82fb7-296">[HttpDelete](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-296">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="82fb7-297">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-297">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="82fb7-298">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-298">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="82fb7-299">路由範本</span><span class="sxs-lookup"><span data-stu-id="82fb7-299">Route templates</span></span>

<span data-ttu-id="82fb7-300">ASP.NET Core 具有下列路由範本：</span><span class="sxs-lookup"><span data-stu-id="82fb7-300">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="82fb7-301">所有[HTTP 動詞範本](#verb)都是路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-301">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="82fb7-302">[料](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="82fb7-302">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="82fb7-303">具有 Http 動詞屬性的屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-303">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="82fb7-304">請考慮下列控制器：</span><span class="sxs-lookup"><span data-stu-id="82fb7-304">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="82fb7-305">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="82fb7-305">In the preceding code:</span></span>

* <span data-ttu-id="82fb7-306">每個動作都包含 `[HttpGet]` 屬性，這會限制僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="82fb7-306">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="82fb7-307">此 `GetProduct` 動作包含 `"{id}"` 範本，因此 `id` 會附加至 `"api/[controller]"` 控制器上的範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-307">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="82fb7-308">方法樣板為 `"api/[controller]/"{id}""` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-308">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="82fb7-309">因此，此動作僅符合的 GET 要求，格式為 `/api/test2/xyz` 、 `/api/test2/123` 、等等 `/api/test2/{any string}` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-309">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="82fb7-310">`GetIntProduct`動作包含 `"int/{id:int}")` 範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-310">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="82fb7-311">`:int`範本的部分 `id` 會將路由值限制為可轉換成整數的字串。</span><span class="sxs-lookup"><span data-stu-id="82fb7-311">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="82fb7-312">GET 要求 `/api/test2/int/abc` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-312">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="82fb7-313">不符合此動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-313">Doesn't match this action.</span></span>
  * <span data-ttu-id="82fb7-314">傳回[404 找不到](https://developer.mozilla.org/docs/Web/HTTP/Status/404)的錯誤。</span><span class="sxs-lookup"><span data-stu-id="82fb7-314">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="82fb7-315">`GetInt2Product`動作 `{id}` 會在範本中包含，但不會限制 `id` 為可以轉換成整數的值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-315">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="82fb7-316">GET 要求 `/api/test2/int2/abc` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-316">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="82fb7-317">符合此路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-317">Matches this route.</span></span>
  * <span data-ttu-id="82fb7-318">模型系結無法轉換成 `abc` 整數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-318">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="82fb7-319">`id`方法的參數是整數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-319">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="82fb7-320">傳回[400 不正確的要求](https://developer.mozilla.org/docs/Web/HTTP/Status/400)，因為模型系結無法轉換 `abc` 成整數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-320">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="82fb7-321">屬性路由可以使用 <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute> 、和等屬性 <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute> <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute> 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-321">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="82fb7-322">所有[HTTP 動詞](#verb)命令屬性都會接受路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-322">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="82fb7-323">下列範例顯示兩個符合相同路由範本的動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-323">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="82fb7-324">使用 URL 路徑 `/products3` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-324">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="82fb7-325">`MyProductsController.ListProducts`當[HTTP 動詞](#verb)命令為時，就會執行此動作 `GET` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-325">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="82fb7-326">`MyProductsController.CreateProduct`當[HTTP 動詞](#verb)命令為時，就會執行此動作 `POST` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-326">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="82fb7-327">建立 REST API 時，您很少需要 `[Route(...)]` 在動作方法上使用，因為動作會接受所有的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-327">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="82fb7-328">最好使用更明確的[HTTP verb 屬性](#verb)，以精確瞭解 API 支援的內容。</span><span class="sxs-lookup"><span data-stu-id="82fb7-328">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="82fb7-329">REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="82fb7-329">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="82fb7-330">REST Api 應使用屬性路由，將應用程式的功能模型建立為一組資源，其中的作業是由 HTTP 指令動詞表示。</span><span class="sxs-lookup"><span data-stu-id="82fb7-330">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="82fb7-331">這表示許多作業（例如，相同邏輯資源上的 GET 和 POST）都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-331">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="82fb7-332">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="82fb7-332">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="82fb7-333">由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-333">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="82fb7-334">在下列範例中， `id` 必須是 URL 路徑的一部分：</span><span class="sxs-lookup"><span data-stu-id="82fb7-334">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="82fb7-335">`Products2ApiController.GetProduct(int)`動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-335">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="82fb7-336">會以類似的 URL 路徑執行`/products2/3`</span><span class="sxs-lookup"><span data-stu-id="82fb7-336">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="82fb7-337">不是以 URL 路徑執行 `/products2` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-337">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="82fb7-338">[[使用]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)屬性允許動作限制支援的要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="82fb7-338">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="82fb7-339">如需詳細資訊，請參閱[使用使用屬性定義支援的要求內容類型](xref:web-api/index#consumes)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-339">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="82fb7-340">如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-340">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="82fb7-341">如需有關的詳細資訊 `[ApiController]` ，請參閱[ApiController 屬性](xref:web-api/index##apicontroller-attribute)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-341">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="82fb7-342">路由名稱</span><span class="sxs-lookup"><span data-stu-id="82fb7-342">Route name</span></span>

<span data-ttu-id="82fb7-343">下列程式碼會定義  的「路由名稱」`Products_List`：</span><span class="sxs-lookup"><span data-stu-id="82fb7-343">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="82fb7-344">您可以使用路由名稱根據特定路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-344">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="82fb7-345">路由名稱：</span><span class="sxs-lookup"><span data-stu-id="82fb7-345">Route names:</span></span>

* <span data-ttu-id="82fb7-346">不會影響路由的 URL 比對行為。</span><span class="sxs-lookup"><span data-stu-id="82fb7-346">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="82fb7-347">僅用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-347">Are only used for URL generation.</span></span>

<span data-ttu-id="82fb7-348">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="82fb7-348">Route names must be unique application-wide.</span></span>

<span data-ttu-id="82fb7-349">將上述程式碼與傳統預設路由相比較，其會將 `id` 參數定義為選擇性（ `{id?}` ）。</span><span class="sxs-lookup"><span data-stu-id="82fb7-349">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="82fb7-350">精確指定 Api 的功能有其優點，例如允許 `/products` 和 `/products/5` 分派至不同的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-350">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="82fb7-351">結合屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-351">Combining attribute routes</span></span>

<span data-ttu-id="82fb7-352">為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-352">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="82fb7-353">控制器上定義的任何路由範本都會附加到動作上的路由範本之前。</span><span class="sxs-lookup"><span data-stu-id="82fb7-353">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="82fb7-354">將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-354">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="82fb7-355">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="82fb7-355">In the preceding example:</span></span>

* <span data-ttu-id="82fb7-356">URL 路徑 `/products` 可以相符`ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="82fb7-356">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="82fb7-357">URL 路徑 `/products/5` 可以相符 `ProductsApi.GetProduct(int)` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-357">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="82fb7-358">這兩個動作都只會符合 HTTP， `GET` 因為它們是以 `[HttpGet]` 屬性標記。</span><span class="sxs-lookup"><span data-stu-id="82fb7-358">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="82fb7-359">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-359">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="82fb7-360">下列範例會比對一組類似于預設路由的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="82fb7-360">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="82fb7-361">下表說明上述程式 `[Route]` 代碼中的屬性：</span><span class="sxs-lookup"><span data-stu-id="82fb7-361">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="82fb7-362">屬性</span><span class="sxs-lookup"><span data-stu-id="82fb7-362">Attribute</span></span>               | <span data-ttu-id="82fb7-363">結合`[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="82fb7-363">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="82fb7-364">定義路由範本</span><span class="sxs-lookup"><span data-stu-id="82fb7-364">Defines route template</span></span> |
| ---
<span data-ttu-id="82fb7-365">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-365">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-366">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-366">'Blazor'</span></span>
- <span data-ttu-id="82fb7-367">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-367">'Identity'</span></span>
- <span data-ttu-id="82fb7-368">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-368">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-369">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-369">'Razor'</span></span>
- <span data-ttu-id="82fb7-370">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-370">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-371">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-371">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-372">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-372">'Blazor'</span></span>
- <span data-ttu-id="82fb7-373">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-373">'Identity'</span></span>
- <span data-ttu-id="82fb7-374">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-374">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-375">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-375">'Razor'</span></span>
- <span data-ttu-id="82fb7-376">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-376">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-377">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-377">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-378">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-378">'Blazor'</span></span>
- <span data-ttu-id="82fb7-379">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-379">'Identity'</span></span>
- <span data-ttu-id="82fb7-380">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-380">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-381">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-381">'Razor'</span></span>
- <span data-ttu-id="82fb7-382">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-382">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-383">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-383">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-384">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-384">'Blazor'</span></span>
- <span data-ttu-id="82fb7-385">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-385">'Identity'</span></span>
- <span data-ttu-id="82fb7-386">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-386">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-387">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-387">'Razor'</span></span>
- <span data-ttu-id="82fb7-388">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-388">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-389">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-389">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-390">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-390">'Blazor'</span></span>
- <span data-ttu-id="82fb7-391">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-391">'Identity'</span></span>
- <span data-ttu-id="82fb7-392">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-392">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-393">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-393">'Razor'</span></span>
- <span data-ttu-id="82fb7-394">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-394">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-395">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-395">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-396">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-396">'Blazor'</span></span>
- <span data-ttu-id="82fb7-397">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-397">'Identity'</span></span>
- <span data-ttu-id="82fb7-398">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-398">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-399">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-399">'Razor'</span></span>
- <span data-ttu-id="82fb7-400">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-400">'SignalR' uid:</span></span> 

<span data-ttu-id="82fb7-401">--------- |---標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-401">--------- | --- title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-402">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-402">'Blazor'</span></span>
- <span data-ttu-id="82fb7-403">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-403">'Identity'</span></span>
- <span data-ttu-id="82fb7-404">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-404">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-405">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-405">'Razor'</span></span>
- <span data-ttu-id="82fb7-406">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-406">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-407">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-407">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-408">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-408">'Blazor'</span></span>
- <span data-ttu-id="82fb7-409">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-409">'Identity'</span></span>
- <span data-ttu-id="82fb7-410">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-410">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-411">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-411">'Razor'</span></span>
- <span data-ttu-id="82fb7-412">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-412">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-413">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-413">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-414">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-414">'Blazor'</span></span>
- <span data-ttu-id="82fb7-415">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-415">'Identity'</span></span>
- <span data-ttu-id="82fb7-416">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-416">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-417">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-417">'Razor'</span></span>
- <span data-ttu-id="82fb7-418">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-418">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-419">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-419">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-420">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-420">'Blazor'</span></span>
- <span data-ttu-id="82fb7-421">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-421">'Identity'</span></span>
- <span data-ttu-id="82fb7-422">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-422">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-423">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-423">'Razor'</span></span>
- <span data-ttu-id="82fb7-424">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-424">'SignalR' uid:</span></span> 

<span data-ttu-id="82fb7-425">------ |---標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-425">------ | --- title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-426">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-426">'Blazor'</span></span>
- <span data-ttu-id="82fb7-427">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-427">'Identity'</span></span>
- <span data-ttu-id="82fb7-428">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-428">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-429">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-429">'Razor'</span></span>
- <span data-ttu-id="82fb7-430">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-430">'SignalR' uid:</span></span> 

-
<span data-ttu-id="82fb7-431">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="82fb7-431">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="82fb7-432">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-432">'Blazor'</span></span>
- <span data-ttu-id="82fb7-433">'Identity'</span><span class="sxs-lookup"><span data-stu-id="82fb7-433">'Identity'</span></span>
- <span data-ttu-id="82fb7-434">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="82fb7-434">'Let's Encrypt'</span></span>
- <span data-ttu-id="82fb7-435">'Razor'</span><span class="sxs-lookup"><span data-stu-id="82fb7-435">'Razor'</span></span>
- <span data-ttu-id="82fb7-436">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="82fb7-436">'SignalR' uid:</span></span> 

<span data-ttu-id="82fb7-437">----- || `[Route("")]` |是 |`"Home"` |
|`[Route("Index")]` |是 |`"Home/Index"` |
|`[Route("/")]` | **否** | `""` |
 | `[Route("About")]` |是 |`"Home/About"`|</span><span class="sxs-lookup"><span data-stu-id="82fb7-437">----- | | `[Route("")]` | Yes | `"Home"` |
| `[Route("Index")]` | Yes | `"Home/Index"` |
| `[Route("/")]` | **No** | `""` |
| `[Route("About")]` | Yes | `"Home/About"` |</span></span>

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="82fb7-438">屬性路由順序</span><span class="sxs-lookup"><span data-stu-id="82fb7-438">Attribute route order</span></span>

<span data-ttu-id="82fb7-439">路由會建立樹狀結構並同時符合所有端點：</span><span class="sxs-lookup"><span data-stu-id="82fb7-439">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="82fb7-440">路由專案的行為就像是放入理想的順序一樣。</span><span class="sxs-lookup"><span data-stu-id="82fb7-440">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="82fb7-441">最特定的路由有機會在較一般的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-441">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="82fb7-442">例如，之類的屬性路由比 `blog/search/{topic}` 屬性路由（例如）更明確 `blog/{*article}` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-442">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="82fb7-443">`blog/search/{topic}`根據預設，路由的優先順序較高，因為它是更明確的。</span><span class="sxs-lookup"><span data-stu-id="82fb7-443">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="82fb7-444">使用[傳統路由](#cr)時，開發人員會負責以所需的順序來放置路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-444">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="82fb7-445">屬性路由可以使用屬性來設定順序 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-445">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="82fb7-446">所有架構提供的[路由屬性](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)都包含 `Order` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-446">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="82fb7-447">路由會依 `Order` 屬性的遞增排序來處理。</span><span class="sxs-lookup"><span data-stu-id="82fb7-447">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="82fb7-448">預設順序為 `0`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-448">The default order is `0`.</span></span> <span data-ttu-id="82fb7-449">`Order = -1`在未設定順序的路由之前，使用執行來設定路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-449">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="82fb7-450">使用 `Order = 1` 預設路由順序之後的執行來設定路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-450">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="82fb7-451">請**避免**視而定 `Order` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-451">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="82fb7-452">如果應用程式的 URL 空間需要明確的順序值才能正確路由，那麼用戶端也可能會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="82fb7-452">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="82fb7-453">一般而言，屬性路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-453">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="82fb7-454">如果用於 URL 產生的預設順序無法運作，使用路由名稱做為覆寫通常會比套用屬性更為簡單 `Order` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-454">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="82fb7-455">請考慮下列兩個同時定義路由對應的控制器 `/home` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-455">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="82fb7-456">`/home`使用上述程式碼要求時，會擲回類似下列的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="82fb7-456">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="82fb7-457">新增 `Order` 至其中一個路由屬性可解決不明確的情況：</span><span class="sxs-lookup"><span data-stu-id="82fb7-457">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="82fb7-458">使用上述程式碼，會 `/home` 執行 `HomeController.Index` 端點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-458">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="82fb7-459">若要取得 `MyDemoController.MyIndex` ，請要求 `/home/MyIndex` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-459">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="82fb7-460">**注意**：</span><span class="sxs-lookup"><span data-stu-id="82fb7-460">**Note**:</span></span>

* <span data-ttu-id="82fb7-461">上述程式碼是一個範例或不良的路由設計。</span><span class="sxs-lookup"><span data-stu-id="82fb7-461">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="82fb7-462">它是用來說明 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-462">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="82fb7-463">`Order`屬性只會解析不明確的，該範本無法比對。</span><span class="sxs-lookup"><span data-stu-id="82fb7-463">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="82fb7-464">最好是移除 `[Route("Home")]` 範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-464">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="82fb7-465">請參閱[ Razor 頁面路由和應用程式慣例：路由順序](xref:razor-pages/razor-pages-conventions#route-order)以取得路由訂單與頁面的相關資訊 Razor 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-465">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="82fb7-466">在某些情況下，會傳回具有不明確路由的 HTTP 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="82fb7-466">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="82fb7-467">使用[記錄](xref:fundamentals/logging/index)來查看哪些端點造成 `AmbiguousMatchException` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-467">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="82fb7-468">路由範本中的權杖取代 [控制器]、[動作]、[區域]</span><span class="sxs-lookup"><span data-stu-id="82fb7-468">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="82fb7-469">為了方便起見，屬性路由支援保留路由參數的權杖取代，方法是使用下列其中一項來括住權杖：</span><span class="sxs-lookup"><span data-stu-id="82fb7-469">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="82fb7-470">方括弧：`[]`</span><span class="sxs-lookup"><span data-stu-id="82fb7-470">Square brackets: `[]`</span></span>
* <span data-ttu-id="82fb7-471">大括弧：`{}`</span><span class="sxs-lookup"><span data-stu-id="82fb7-471">Curly braces: `{}`</span></span>

<span data-ttu-id="82fb7-472">標記 `[action]` 、 `[area]` 和 `[controller]` 會取代為定義路由之動作中的動作名稱、區功能變數名稱稱和控制器名稱的值：</span><span class="sxs-lookup"><span data-stu-id="82fb7-472">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="82fb7-473">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="82fb7-473">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="82fb7-474">對應`/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="82fb7-474">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="82fb7-475">對應`/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="82fb7-475">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="82fb7-476">語彙基元取代會在建立屬性路由的最後一個步驟發生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-476">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="82fb7-477">上述範例的行為與下列程式碼相同：</span><span class="sxs-lookup"><span data-stu-id="82fb7-477">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="82fb7-478">屬性路由也可以透過繼承來合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-478">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="82fb7-479">這種功能強大結合了權杖取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-479">This is powerful combined with token replacement.</span></span> <span data-ttu-id="82fb7-480">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-480">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="82fb7-481">`[Route("[controller]/[action]", Name="[controller]_[action]")]`為每個動作產生唯一的路由名稱：</span><span class="sxs-lookup"><span data-stu-id="82fb7-481">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="82fb7-482">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-482">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="82fb7-483"> 會針對每個動作產生唯一的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-483">generates a unique route name for each action.</span></span>

<span data-ttu-id="82fb7-484">若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="82fb7-484">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="82fb7-485">使用參數轉換程式自訂語彙基元取代</span><span class="sxs-lookup"><span data-stu-id="82fb7-485">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="82fb7-486">可以使用參數轉換程式自訂語彙基元取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-486">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="82fb7-487">參數轉換程式會實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-487">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="82fb7-488">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-488">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="82fb7-489"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> 是一個應用程式模型慣例，它會：</span><span class="sxs-lookup"><span data-stu-id="82fb7-489">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="82fb7-490">將參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-490">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="82fb7-491">在取代屬性路由語彙基元值時自訂它們。</span><span class="sxs-lookup"><span data-stu-id="82fb7-491">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="82fb7-492">前面的 `ListAll` 方法會符合 `/subscription-management/list-all` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-492">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="82fb7-493">`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。</span><span class="sxs-lookup"><span data-stu-id="82fb7-493">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="82fb7-494">請參閱[MDN web](https://developer.mozilla.org/docs/Glossary/Slug)檔以取得輔助資訊的定義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-494">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

[!INCLUDE[](~/includes/regex.md)]
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="82fb7-495">多個屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-495">Multiple attribute routes</span></span>

<span data-ttu-id="82fb7-496">屬性路由支援定義多個路由來達到相同的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-496">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="82fb7-497">最常見的用法是模擬「預設慣例路由」的行為，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="82fb7-497">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="82fb7-498">將多個路由屬性放在控制器上，表示每一個都與動作方法上的每個路由屬性結合：</span><span class="sxs-lookup"><span data-stu-id="82fb7-498">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="82fb7-499">所有的[HTTP 動詞](#verb)命令路由條件約束都會執行 `IActionConstraint` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-499">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="82fb7-500">當執行的多個路由屬性 <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> 放在動作上時：</span><span class="sxs-lookup"><span data-stu-id="82fb7-500">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="82fb7-501">每個動作條件約束都會與套用至控制器的路由範本結合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-501">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="82fb7-502">在動作上使用多個路由看起來可能很有用且功能強大，因此最好讓應用程式的 URL 空間保持基本且妥善定義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-502">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="82fb7-503">只有在需要時，**才**在動作上使用多個路由，例如支援現有的用戶端。</span><span class="sxs-lookup"><span data-stu-id="82fb7-503">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="82fb7-504">指定屬性路由的選擇性參數、預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="82fb7-504">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="82fb7-505">屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。</span><span class="sxs-lookup"><span data-stu-id="82fb7-505">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="82fb7-506">在上述程式碼中，會 `[HttpPost("product/{id:int}")]` 套用路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="82fb7-506">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="82fb7-507">此 `ProductsController.ShowProduct` 動作只會與類似的 URL 路徑進行比對 `/product/3` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-507">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="82fb7-508">路由範本部分 `{id:int}` 會將該區段限制為只有整數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-508">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="82fb7-509">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-509">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="82fb7-510">使用 IRouteTemplateProvider 自訂路由屬性</span><span class="sxs-lookup"><span data-stu-id="82fb7-510">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="82fb7-511">所有的[路由屬性](#rt)都會執行 <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider> 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-511">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="82fb7-512">ASP.NET Core 執行時間：</span><span class="sxs-lookup"><span data-stu-id="82fb7-512">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="82fb7-513">在應用程式啟動時，尋找控制器類別和動作方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-513">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="82fb7-514">會使用可執行檔屬性 `IRouteTemplateProvider` 來建立初始的路由集合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-514">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="82fb7-515">執行 `IRouteTemplateProvider` 以定義自訂路由屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-515">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="82fb7-516">每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-516">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="82fb7-517">上述 `Get` 方法會傳回 `Order = 2, Template = api/MyTestApi` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-517">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="82fb7-518">使用應用程式模型自訂屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-518">Use application model to customize attribute routes</span></span>

<span data-ttu-id="82fb7-519">應用程式模型：</span><span class="sxs-lookup"><span data-stu-id="82fb7-519">The application model:</span></span>

* <span data-ttu-id="82fb7-520">是在啟動時建立的物件模型。</span><span class="sxs-lookup"><span data-stu-id="82fb7-520">Is an object model created at startup.</span></span>
* <span data-ttu-id="82fb7-521">包含 ASP.NET Core 用來在應用程式中路由及執行動作的所有中繼資料。</span><span class="sxs-lookup"><span data-stu-id="82fb7-521">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="82fb7-522">應用程式模型包含從路由屬性收集而來的所有資料。</span><span class="sxs-lookup"><span data-stu-id="82fb7-522">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="82fb7-523">路由屬性中的資料是由實作為提供 `IRouteTemplateProvider` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-523">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="82fb7-524">會議</span><span class="sxs-lookup"><span data-stu-id="82fb7-524">Conventions:</span></span>

* <span data-ttu-id="82fb7-525">可以撰寫來修改應用程式模型，以自訂路由的行為。</span><span class="sxs-lookup"><span data-stu-id="82fb7-525">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="82fb7-526">會在應用程式啟動時讀取。</span><span class="sxs-lookup"><span data-stu-id="82fb7-526">Are read at app startup.</span></span>

<span data-ttu-id="82fb7-527">本節顯示使用應用程式模型自訂路由的基本範例。</span><span class="sxs-lookup"><span data-stu-id="82fb7-527">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="82fb7-528">下列程式碼會讓路由大致對齊專案的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="82fb7-528">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="82fb7-529">下列程式碼可防止將 `namespace` 慣例套用至屬性路由的控制器：</span><span class="sxs-lookup"><span data-stu-id="82fb7-529">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="82fb7-530">例如，下列控制器不會使用 `NamespaceRoutingConvention` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-530">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="82fb7-531">`NamespaceRoutingConvention.Apply` 方法：</span><span class="sxs-lookup"><span data-stu-id="82fb7-531">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="82fb7-532">如果控制器是屬性路由，則不會執行任何操作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-532">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="82fb7-533">設定以為基礎的控制器範本 `namespace` ，基底 `namespace` 已移除。</span><span class="sxs-lookup"><span data-stu-id="82fb7-533">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="82fb7-534">`NamespaceRoutingConvention`可以套用於 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-534">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="82fb7-535">例如，請考慮下列控制器：</span><span class="sxs-lookup"><span data-stu-id="82fb7-535">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="82fb7-536">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="82fb7-536">In the preceding code:</span></span>

* <span data-ttu-id="82fb7-537">基底 `namespace` 為 `My.Application` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-537">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="82fb7-538">先前控制器的完整名稱是 `My.Application.Admin.Controllers.UsersController` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-538">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="82fb7-539">會 `NamespaceRoutingConvention` 將控制器範本設定為 `Admin/Controllers/Users/[action]/{id?` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-539">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="82fb7-540">`NamespaceRoutingConvention`也可以套用為控制器上的屬性：</span><span class="sxs-lookup"><span data-stu-id="82fb7-540">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="82fb7-541">混合路由：屬性路由與慣例路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-541">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="82fb7-542">ASP.NET Core 應用程式可以混合使用傳統路由與屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-542">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="82fb7-543">控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。</span><span class="sxs-lookup"><span data-stu-id="82fb7-543">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="82fb7-544">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-544">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="82fb7-545">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-545">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="82fb7-546">定義屬性路由的動作無法透過慣例路由到達，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="82fb7-546">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="82fb7-547">控制器上的**任何**路由屬性都會讓控制器屬性中的**所有動作都**已路由傳送。</span><span class="sxs-lookup"><span data-stu-id="82fb7-547">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="82fb7-548">屬性路由和傳統路由會使用相同的路由引擎。</span><span class="sxs-lookup"><span data-stu-id="82fb7-548">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="82fb7-549">URL 產生和環境值</span><span class="sxs-lookup"><span data-stu-id="82fb7-549">URL Generation and ambient values</span></span>

<span data-ttu-id="82fb7-550">應用程式可以使用路由 URL 產生功能來產生動作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="82fb7-550">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="82fb7-551">產生 Url 可排除硬式編碼的 Url，讓程式碼更健全且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="82fb7-551">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="82fb7-552">本節著重于 MVC 提供的 URL 產生功能，並僅涵蓋 URL 產生運作方式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="82fb7-552">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="82fb7-553">如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-553">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="82fb7-554"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper>介面是 MVC 與路由之間的基礎結構基礎元素，用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-554">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="82fb7-555">的實例 `IUrlHelper` 可透過 [控制器] `Url` 、[views] 和 [view] 元件中的屬性取得。</span><span class="sxs-lookup"><span data-stu-id="82fb7-555">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="82fb7-556">在下列範例中， `IUrlHelper` 會透過屬性來使用介面， `Controller.Url` 以產生另一個動作的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-556">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="82fb7-557">如果應用程式使用預設的慣例路由，則變數的值 `url` 會是 URL 路徑字串 `/UrlGeneration/Destination` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-557">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="82fb7-558">此 URL 路徑是藉由結合而建立：</span><span class="sxs-lookup"><span data-stu-id="82fb7-558">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="82fb7-559">來自目前要求的路由值，稱為「**環境值**」。</span><span class="sxs-lookup"><span data-stu-id="82fb7-559">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="82fb7-560">傳遞至的值 `Url.Action` ，並將這些值取代為路由範本：</span><span class="sxs-lookup"><span data-stu-id="82fb7-560">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="82fb7-561">路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-561">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="82fb7-562">沒有值的路由參數可以：</span><span class="sxs-lookup"><span data-stu-id="82fb7-562">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="82fb7-563">使用預設值（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="82fb7-563">Use a default value if it has one.</span></span>
* <span data-ttu-id="82fb7-564">如果是選擇性，則略過。</span><span class="sxs-lookup"><span data-stu-id="82fb7-564">Be skipped if it's optional.</span></span> <span data-ttu-id="82fb7-565">例如， `id` 來自路由範本的 `{controller}/{action}/{id?}` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-565">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="82fb7-566">如果任何必要的路由參數沒有對應的值，則 URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="82fb7-566">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="82fb7-567">如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。</span><span class="sxs-lookup"><span data-stu-id="82fb7-567">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="82fb7-568">上述範例 `Url.Action` 假設採用[傳統路由](#cr)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-568">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="82fb7-569">URL 產生的運作方式類似于[屬性路由](#ar)，不過概念不同。</span><span class="sxs-lookup"><span data-stu-id="82fb7-569">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="82fb7-570">使用傳統路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-570">With conventional routing:</span></span>

* <span data-ttu-id="82fb7-571">路由值是用來展開範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-571">The route values are used to expand a template.</span></span>
* <span data-ttu-id="82fb7-572">和的路由值 `controller` `action` 通常會出現在該範本中。</span><span class="sxs-lookup"><span data-stu-id="82fb7-572">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="82fb7-573">這是可行的，因為路由所比對的 Url 會遵循慣例。</span><span class="sxs-lookup"><span data-stu-id="82fb7-573">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="82fb7-574">下列範例會使用屬性路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-574">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="82fb7-575">`Source`上述程式碼中的動作會產生 `custom/url/to/destination` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-575">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="82fb7-576"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>已在 ASP.NET Core 3.0 中新增為的替代方案 `IUrlHelper` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-576"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="82fb7-577">`LinkGenerator`提供類似但更具彈性的功能。</span><span class="sxs-lookup"><span data-stu-id="82fb7-577">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="82fb7-578">上的每個方法 `IUrlHelper` 也都有對應的方法系列 `LinkGenerator` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-578">Each method on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="82fb7-579">由動作名稱產生 URL</span><span class="sxs-lookup"><span data-stu-id="82fb7-579">Generating URLs by action name</span></span>

<span data-ttu-id="82fb7-580">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) [LinkGenerator、GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)和所有相關的多載都是藉由指定控制器名稱和動作名稱，而設計來產生目標端點。</span><span class="sxs-lookup"><span data-stu-id="82fb7-580">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="82fb7-581">使用時 `Url.Action` ，和的目前路由值 `controller` `action` 是由執行時間所提供：</span><span class="sxs-lookup"><span data-stu-id="82fb7-581">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="82fb7-582">和的值 `controller` `action` 是[環境值](#ambient)和值的一部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-582">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="82fb7-583">方法 `Url.Action` 一律會使用和的目前值 `action` `controller` ，並產生會路由至目前動作的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="82fb7-583">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="82fb7-584">路由嘗試使用環境值中的值來填入產生 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="82fb7-584">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="82fb7-585">請考慮 `{a}/{b}/{c}/{d}` 使用與環境值類似的路由 `{ a = Alice, b = Bob, c = Carol, d = David }` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-585">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="82fb7-586">路由有足夠的資訊可產生不含任何其他值的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-586">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="82fb7-587">路由有足夠的資訊，因為所有路由參數都有值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-587">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="82fb7-588">如果已新增此值 `{ d = Donovan }` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-588">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="82fb7-589">此值 `{ d = David }` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="82fb7-589">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="82fb7-590">產生的 URL 路徑是 `Alice/Bob/Carol/Donovan` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-590">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="82fb7-591">**警告**： URL 路徑為階層式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-591">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="82fb7-592">在上述範例中，如果已 `{ c = Cheryl }` 加入值：</span><span class="sxs-lookup"><span data-stu-id="82fb7-592">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="82fb7-593">這兩個值 `{ c = Carol, d = David }` 都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="82fb7-593">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="82fb7-594">已不再有的值 `d` ，且 URL 產生失敗。</span><span class="sxs-lookup"><span data-stu-id="82fb7-594">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="82fb7-595">您必須指定和所需的值， `c` `d` 才能產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-595">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="82fb7-596">您可能預期會遇到此預設路由的問題 `{controller}/{action}/{id?}` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-596">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="82fb7-597">這個問題在實務上很罕見，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-597">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="82fb7-598">Url 的數個多載[。動作](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)會接受路由值物件，以提供和以外的路由參數值 `controller` `action` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-598">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="82fb7-599">「路由值」物件經常與搭配使用 `id` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-599">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="82fb7-600">例如 `Url.Action("Buy", "Products", new { id = 17 })`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-600">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="82fb7-601">路由值物件：</span><span class="sxs-lookup"><span data-stu-id="82fb7-601">The route values object:</span></span>

* <span data-ttu-id="82fb7-602">依照慣例，通常是匿名型別的物件。</span><span class="sxs-lookup"><span data-stu-id="82fb7-602">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="82fb7-603">可以是 `IDictionary<>` 或[POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)）。</span><span class="sxs-lookup"><span data-stu-id="82fb7-603">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="82fb7-604">不符合路由參數的任何額外路由值都會放在查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="82fb7-604">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="82fb7-605">上述程式碼會產生 `/Products/Buy/17?color=red` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-605">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="82fb7-606">下列程式碼會產生絕對 URL：</span><span class="sxs-lookup"><span data-stu-id="82fb7-606">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="82fb7-607">若要建立絕對 URL，請使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="82fb7-607">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="82fb7-608">接受的多載 `protocol` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-608">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="82fb7-609">例如，上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="82fb7-609">For example, the preceding code.</span></span>
* <span data-ttu-id="82fb7-610">[LinkGenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*)，預設會產生絕對 uri。</span><span class="sxs-lookup"><span data-stu-id="82fb7-610">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="82fb7-611">依路由產生 Url</span><span class="sxs-lookup"><span data-stu-id="82fb7-611">Generate URLs by route</span></span>

<span data-ttu-id="82fb7-612">上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-612">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="82fb7-613">`IUrlHelper`也提供[RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*)系列的方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-613">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="82fb7-614">這些方法類似于[Url. 動作](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)，但不會將和的目前值複製 `action` `controller` 到路由值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-614">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="82fb7-615">最常見的使用方式 `Url.RouteUrl` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-615">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="82fb7-616">指定用來產生 URL 的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-616">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="82fb7-617">通常不會指定控制器或動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-617">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="82fb7-618">下列檔案會 Razor 產生的 HTML 連結 `Destination_Route` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-618">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="82fb7-619">以 HTML 和產生 UrlRazor</span><span class="sxs-lookup"><span data-stu-id="82fb7-619">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="82fb7-620"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper>提供 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> [Html.beginform](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*)和[.html](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*)方法， `<form>` 分別產生和 `<a>` 元素。</span><span class="sxs-lookup"><span data-stu-id="82fb7-620"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="82fb7-621">這些方法會使用[url. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)方法來產生 url，並接受類似的引數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-621">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="82fb7-622">`HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。</span><span class="sxs-lookup"><span data-stu-id="82fb7-622">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="82fb7-623">TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-623">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="82fb7-624">這兩者使用 `IUrlHelper` 進行實作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-624">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="82fb7-625">如需詳細資訊，請參閱[表單中的標記](xref:mvc/views/working-with-forms)協助程式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-625">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="82fb7-626">在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-626">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="82fb7-627">動作結果中的 URL 產生</span><span class="sxs-lookup"><span data-stu-id="82fb7-627">URL generation in Action Results</span></span>

<span data-ttu-id="82fb7-628">先前的範例顯示 `IUrlHelper` 在控制器中使用。</span><span class="sxs-lookup"><span data-stu-id="82fb7-628">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="82fb7-629">控制器中最常見的用法是在動作結果中產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-629">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="82fb7-630"><xref:Microsoft.AspNetCore.Mvc.ControllerBase> 和 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別提供便利的方法讓動作結果可參考其他動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-630">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="82fb7-631">一個典型的使用方式是在接受使用者輸入之後重新導向：</span><span class="sxs-lookup"><span data-stu-id="82fb7-631">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="82fb7-632">動作結果處理站方法（例如 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> 和） <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 遵循類似于中方法的模式 `IUrlHelper` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-632">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="82fb7-633">專用慣例路由的特殊案例</span><span class="sxs-lookup"><span data-stu-id="82fb7-633">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="82fb7-634">[傳統路由](#cr)可以使用一種特殊的路由定義，稱為[專用](#dcr)的慣例路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-634">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="82fb7-635">在下列範例中，名為的路由 `blog` 是專用的傳統路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-635">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="82fb7-636">使用上述路由定義，會 `Url.Action("Index", "Home")` 使用路由產生 URL 路徑 `/` `default` ，但為何？</span><span class="sxs-lookup"><span data-stu-id="82fb7-636">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="82fb7-637">您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-637">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="82fb7-638">[專用的傳統路由](#dcr)會依賴預設值的特殊行為，而不會有對應的路由參數，以防止路由在 URL 產生時過於[貪婪](xref:fundamentals/routing#greedy)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-638">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="82fb7-639">在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-639">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="82fb7-640">當路由執行 URL 產生時，所提供的值必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-640">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="82fb7-641">使用的 URL 產生 `blog` 失敗，因為值 `{ controller = Home, action = Index }` 不相符 `{ controller = Blog, action = Article }` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-641">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="82fb7-642">路由會接著切換並嘗試 `default`，此時會成功。</span><span class="sxs-lookup"><span data-stu-id="82fb7-642">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="82fb7-643">區域</span><span class="sxs-lookup"><span data-stu-id="82fb7-643">Areas</span></span>

<span data-ttu-id="82fb7-644">[區域](xref:mvc/controllers/areas)是一種 MVC 功能，用來將相關功能組織成不同的群組：</span><span class="sxs-lookup"><span data-stu-id="82fb7-644">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="82fb7-645">控制器動作的路由命名空間。</span><span class="sxs-lookup"><span data-stu-id="82fb7-645">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="82fb7-646">Views 的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="82fb7-646">Folder structure for views.</span></span>

<span data-ttu-id="82fb7-647">使用區域可讓應用程式擁有多個具有相同名稱的控制器，前提是它們有不同的區域。</span><span class="sxs-lookup"><span data-stu-id="82fb7-647">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="82fb7-648">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-648">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="82fb7-649">本節討論路由與區域的互動方式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-649">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="82fb7-650">如需如何搭配使用區域與視圖的詳細資料，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-650">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="82fb7-651">下列範例會將 MVC 設定為使用預設的傳統路由和 `area` 名為的 `area` 路由 `Blog` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-651">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="82fb7-652">在上述程式碼中， <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> 會呼叫來建立 `"blog_route"` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-652">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="82fb7-653">第二個參數 `"Blog"` 是區功能變數名稱稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-653">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="82fb7-654">當符合之類的 URL 路徑時 `/Manage/Users/AddUser` ， `"blog_route"` 路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-654">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="82fb7-655">`area`路由值是由的預設值所產生 `area` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-655">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="82fb7-656">所建立的路由 `MapAreaControllerRoute` 相當於下列內容：</span><span class="sxs-lookup"><span data-stu-id="82fb7-656">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="82fb7-657">`MapAreaControllerRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-657">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="82fb7-658">預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-658">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="82fb7-659">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="82fb7-659">Conventional routing is order-dependent.</span></span> <span data-ttu-id="82fb7-660">一般而言，具有區域的路由應該放在較早的位置，因為它們比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="82fb7-660">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="82fb7-661">使用上述範例時，路由值會 `{ area = Blog, controller = Users, action = AddUser }` 符合下列動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-661">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="82fb7-662">[[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute)屬性（attribute）會將控制器表示為區域的一部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-662">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="82fb7-663">此控制器位於此 `Blog` 區域。</span><span class="sxs-lookup"><span data-stu-id="82fb7-663">This controller is in the `Blog` area.</span></span> <span data-ttu-id="82fb7-664">沒有 `[Area]` 屬性的控制器不是任何區域的成員，而且在**not** `area` 路由提供路由值時不會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-664">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="82fb7-665">在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-665">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="82fb7-666">這裡會顯示每個控制器的命名空間，以提供完整性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-666">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="82fb7-667">如果前面的控制器使用相同的命名空間，則會產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="82fb7-667">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="82fb7-668">類別命名空間不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-668">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="82fb7-669">前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-669">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="82fb7-670">第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-670">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="82fb7-671">在「未符合任何值」\*\* 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="82fb7-671">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="82fb7-672">在區域內執行動作時，的路由值 `area` 會以[環境值](#ambient)的形式提供，以供路由用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-672">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="82fb7-673">這表示區域預設會以「黏性」\*\* 方式來處理 URL 產生，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="82fb7-673">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="82fb7-674">下列程式碼會產生 URL，以 `/Zebra/Users/AddUser` ：</span><span class="sxs-lookup"><span data-stu-id="82fb7-674">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="82fb7-675">動作定義</span><span class="sxs-lookup"><span data-stu-id="82fb7-675">Action definition</span></span>

<span data-ttu-id="82fb7-676">控制器上的公用方法（除了具有[請以 nonaction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute)屬性的方法）是動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-676">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="82fb7-677">範例程式碼</span><span class="sxs-lookup"><span data-stu-id="82fb7-677">Sample code</span></span>

 * <span data-ttu-id="82fb7-678">[MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs)方法包含在[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)中，可用來顯示路由資訊。</span><span class="sxs-lookup"><span data-stu-id="82fb7-678">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="82fb7-679">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="82fb7-679">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82fb7-680">ASP.NET Core MVC 使用路由[中介軟體](xref:fundamentals/middleware/index)來比對內送要求的 URL，並將這些 URL 對應至動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-680">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="82fb7-681">路由是在啟動程式碼或屬性中定義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-681">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="82fb7-682">路由描述 URL 路徑應該如何與動作進行比對。</span><span class="sxs-lookup"><span data-stu-id="82fb7-682">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="82fb7-683">路由也可用來產生回應中所送出的連結 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-683">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="82fb7-684">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-684">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="82fb7-685">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-685">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="82fb7-686">如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-686">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="82fb7-687">本文件將說明 MVC 與路由之間的互動，並說明一般 MVC 應用程式如何使用路由功能。</span><span class="sxs-lookup"><span data-stu-id="82fb7-687">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="82fb7-688">如需進階路由的詳細資料，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-688">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="82fb7-689">設定路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="82fb7-689">Setting up Routing Middleware</span></span>

<span data-ttu-id="82fb7-690">在 *Configure* 方法中，您可能會看到類似如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="82fb7-690">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="82fb7-691">在 `UseMvc` 呼叫內，`MapRoute` 是用來建立單一路由，稱為 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-691">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="82fb7-692">大多數 MVC 應用程式會使用具有類似於 `default` 路由之範本的路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-692">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="82fb7-693">路由範本 `"{controller=Home}/{action=Index}/{id?}"` 可能符合某個 URL 路徑 (例如 `/Products/Details/5`)，並將透過 Token 化路徑來擷取路由值 `{ controller = Products, action = Details, id = 5 }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-693">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="82fb7-694">MVC 會嘗試尋找名為 `ProductsController` 的控制器，並執行動作 `Details`：</span><span class="sxs-lookup"><span data-stu-id="82fb7-694">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="82fb7-695">請注意，在此範例中，模型繫結會在叫用此動作時，使用 `id = 5` 值將 `id` 參數設定為 `5`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-695">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="82fb7-696">如需詳細資料，請參閱[模型繫結](../models/model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-696">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="82fb7-697">使用 `default` 路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-697">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="82fb7-698">路由範本：</span><span class="sxs-lookup"><span data-stu-id="82fb7-698">The route template:</span></span>

* <span data-ttu-id="82fb7-699">`{controller=Home}` 將 `Home` 定義為預設的 `controller`</span><span class="sxs-lookup"><span data-stu-id="82fb7-699">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="82fb7-700">`{action=Index}` 將 `Index` 定義為預設的 `action`</span><span class="sxs-lookup"><span data-stu-id="82fb7-700">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="82fb7-701">`{id?}` 將 `id` 定義為選擇性</span><span class="sxs-lookup"><span data-stu-id="82fb7-701">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="82fb7-702">預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。</span><span class="sxs-lookup"><span data-stu-id="82fb7-702">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="82fb7-703">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-703">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="82fb7-704">`"{controller=Home}/{action=Index}/{id?}"` 可能符合 URL 路徑 `/`，並將產生路由值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-704">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="82fb7-705">`controller` 和 `action` 的值使用預設值；`id` 不會產生值，因為 URL 路徑中沒有對應的區段。</span><span class="sxs-lookup"><span data-stu-id="82fb7-705">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="82fb7-706">MVC 會使用這些路由值來選取 `HomeController` 和 `Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-706">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="82fb7-707">使用此控制器定義和路由範本，就會對下列任何 URL 路徑執行 `HomeController.Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-707">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="82fb7-708">`UseMvcWithDefaultRoute` 方法很方便：</span><span class="sxs-lookup"><span data-stu-id="82fb7-708">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="82fb7-709">可用來取代：</span><span class="sxs-lookup"><span data-stu-id="82fb7-709">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="82fb7-710">`UseMvc` 和 `UseMvcWithDefaultRoute` 會將 `RouterMiddleware` 的執行個體新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="82fb7-710">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="82fb7-711">MVC 不會直接與中介軟體互動，而是使用路由來處理要求。</span><span class="sxs-lookup"><span data-stu-id="82fb7-711">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="82fb7-712">MVC 會透過 `MvcRouteHandler` 的執行個體連線到路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-712">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="82fb7-713">`UseMvc` 內的程式碼類似如下：</span><span class="sxs-lookup"><span data-stu-id="82fb7-713">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="82fb7-714">`UseMvc` 不會直接定義任何路由，而是將預留位置新增至 `attribute` 路由的路由集合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-714">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="82fb7-715">多載 `UseMvc(Action<IRouteBuilder>)` 可讓您新增自己的路由，同時支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-715">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="82fb7-716">`UseMvc`而且其所有變化都會新增屬性路由的預留位置，而不論您設定的方式為何，都可以使用屬性路由 `UseMvc` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-716">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="82fb7-717">`UseMvcWithDefaultRoute` 會定義預設路由並支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-717">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="82fb7-718">[屬性路由](#attribute-routing-ref-label)一節包含屬性路由的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="82fb7-718">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="82fb7-719">慣例路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-719">Conventional routing</span></span>

<span data-ttu-id="82fb7-720">`default` 路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-720">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="82fb7-721">上述程式碼是傳統路由的範例。</span><span class="sxs-lookup"><span data-stu-id="82fb7-721">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="82fb7-722">此樣式稱為「傳統路由」，因為它會建立 URL 路徑的*慣例*：</span><span class="sxs-lookup"><span data-stu-id="82fb7-722">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="82fb7-723">第一個路徑區段會對應到控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-723">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="82fb7-724">第二個對應至動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-724">The second maps to the action name.</span></span>
* <span data-ttu-id="82fb7-725">第三個區段用於選擇性的 `id` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-725">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="82fb7-726">`id`對應至模型實體。</span><span class="sxs-lookup"><span data-stu-id="82fb7-726">`id` maps to a model entity.</span></span>

<span data-ttu-id="82fb7-727">使用此 `default` 路由，URL 路徑 `/Products/List` 會對應至 `ProductsController.List` 動作；而 `/Blog/Article/17` 會對應至 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-727">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="82fb7-728">此對應**只會**根據控制器和動作名稱，而不會根據命名空間、來源檔案位置或方法參數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-728">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="82fb7-729">慣例路由與預設路由的搭配使用可讓您快速建置應用程式，而不需要針對每個定義的動作產生新的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-729">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="82fb7-730">針對具有 CRUD 樣式動作的應用程式，在控制器之間保持一致的 URL 有助於簡化程式碼，並讓 UI 更容易預測。</span><span class="sxs-lookup"><span data-stu-id="82fb7-730">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="82fb7-731">路由範本將 `id` 定義為選擇性，也就是您的動作可以直接執行，而不需要將識別碼當作 URL 的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="82fb7-731">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="82fb7-732">如果省略 URL 中的 `id`，通常表示它會由模型繫結設定為 `0`，因此在符合 `id == 0` 的資料庫中找不到任何實體。</span><span class="sxs-lookup"><span data-stu-id="82fb7-732">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="82fb7-733">屬性路由可提供您更細微的控制，讓某些動作需要此識別碼，而其他動作則不需要。</span><span class="sxs-lookup"><span data-stu-id="82fb7-733">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="82fb7-734">依照慣例，本文件將會包含可能出現在正確使用中的選擇性參數 (例如 `id`)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-734">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="82fb7-735">多個路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-735">Multiple routes</span></span>

<span data-ttu-id="82fb7-736">您可以將更多呼叫新增至 `MapRoute`，以在 `UseMvc` 內新增多個路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-736">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="82fb7-737">這樣做可讓您定義多個慣例，或新增特定動作專用的慣例路由，例如：</span><span class="sxs-lookup"><span data-stu-id="82fb7-737">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="82fb7-738">這裡的 `blog` 路由是「專用慣例路由」\*\*，也就是它會使用慣例路由系統，但專用於特定動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-738">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="82fb7-739">由於 `controller` 和 `action` 並未作為參數出現在路由範本中，它們只能有預設值，因此此路由一律會對應至動作 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-739">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="82fb7-740">路由集合中的路由已經過排序，並將依其新增順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="82fb7-740">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="82fb7-741">因此在此範例中，會先嘗試 `blog` 路由，再嘗試 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-741">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-742">*專用的傳統路由*通常會使用**catch-all**路由參數 `{*article}` 來捕捉 URL 路徑的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-742">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="82fb7-743">這可能會讓路由變得「太窮盡」，也就是它會比對您想要讓其他路由比對的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-743">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="82fb7-744">將這些「窮盡」路由放在路由表後面可解決此問題。</span><span class="sxs-lookup"><span data-stu-id="82fb7-744">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="82fb7-745">後援</span><span class="sxs-lookup"><span data-stu-id="82fb7-745">Fallback</span></span>

<span data-ttu-id="82fb7-746">在要求處理過程中，MVC 會確認路由值是否可用來尋找應用程式中的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-746">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="82fb7-747">如果路由值未符合任何動作，則不會將此路由視為一個相符項目，並將嘗試下一個路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-747">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="82fb7-748">這稱為「遞補」\*\*，其目的在於簡化慣例路由重疊的情況。</span><span class="sxs-lookup"><span data-stu-id="82fb7-748">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="82fb7-749">釐清動作</span><span class="sxs-lookup"><span data-stu-id="82fb7-749">Disambiguating actions</span></span>

<span data-ttu-id="82fb7-750">若透過路由符合兩個動作，MVC 必須釐清並選擇「最佳」候選項目，否則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="82fb7-750">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="82fb7-751">例如：</span><span class="sxs-lookup"><span data-stu-id="82fb7-751">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="82fb7-752">此控制器定義兩個符合 URL 路徑 `/Products/Edit/17` 和路由資料 `{ controller = Products, action = Edit, id = 17 }` 的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-752">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="82fb7-753">這是 MVC 控制器的典型模式，其中 `Edit(int)` 會顯示用以編輯產品的表單，而 `Edit(int, Product)` 會處理已張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="82fb7-753">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="82fb7-754">若要執行這項操作，MVC 必須在要求為 HTTP `POST` 時選擇 `Edit(int, Product)`，並在 HTTP 動詞命令為任何其他項目時選擇 `Edit(int)`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-754">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="82fb7-755">`HttpPostAttribute` ( `[HttpPost]` ) 是 `IActionConstraint` 的實作，只有在 HTTP 動詞命令為 `POST` 時，才能選取此動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-755">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="82fb7-756">`IActionConstraint` 的存在使 `Edit(int, Product)` 比 `Edit(int)`「更符合」，因此會先嘗試 `Edit(int, Product)`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-756">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="82fb7-757">您只需要在特殊情況下撰寫自訂 `IActionConstraint` 實作，但請務必了解 `HttpPostAttribute` 等屬性的角色 (其他 HTTP 動詞命令會定義類似的屬性)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-757">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="82fb7-758">在慣例路由中，當動作是 `show form -> submit form` 工作流程的一部分時，通常會使用相同的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-758">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="82fb7-759">檢閱[了解 IActionConstraint](#understanding-iactionconstraint) 一節之後，會更清楚此模式的便利性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-759">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="82fb7-760">如果多個路由相符，而且 MVC 找不到「最佳」路由，則會擲回 `AmbiguousActionException`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-760">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="82fb7-761">路由名稱</span><span class="sxs-lookup"><span data-stu-id="82fb7-761">Route names</span></span>

<span data-ttu-id="82fb7-762">下列範例中的字串 `"blog"` 和 `"default"` 是路由名稱：</span><span class="sxs-lookup"><span data-stu-id="82fb7-762">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="82fb7-763">路由名稱為路由提供邏輯名稱，因此可以使用具名路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-763">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="82fb7-764">當路由排序可能使 URL 產生作業變得複雜時，這樣做可大幅簡化 URL 產生作業。</span><span class="sxs-lookup"><span data-stu-id="82fb7-764">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="82fb7-765">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="82fb7-765">Route names must be unique application-wide.</span></span>

<span data-ttu-id="82fb7-766">路由名稱不會影響要求的 URL 比對或處理，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-766">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="82fb7-767">[路由](xref:fundamentals/routing)提供 URL 產生的詳細資訊，包括 MVC 特定協助程式中的 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-767">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="82fb7-768">屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-768">Attribute routing</span></span>

<span data-ttu-id="82fb7-769">屬性路由使用一組屬性，將動作直接對應至路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-769">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="82fb7-770">在下列範例中，在 `Configure` 方法中使用 `app.UseMvc();`，且未傳遞任何路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-770">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="82fb7-771">`HomeController` 會比對一組 類似於預設路由 `{controller=Home}/{action=Index}/{id?}` 所比對的 URL：</span><span class="sxs-lookup"><span data-stu-id="82fb7-771">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="82fb7-772">`HomeController.Index()` 動作會針對 URL 路徑 `/`、`/Home` 或 `/Home/Index` 的任何一個來執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-772">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-773">此範例醒目提示屬性路由與慣例路由之間的主要程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="82fb7-773">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="82fb7-774">屬性路由需要更多輸入才能指定路由，而慣例預設路由處理路由的方式則更簡潔。</span><span class="sxs-lookup"><span data-stu-id="82fb7-774">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="82fb7-775">不過，屬性路由允許 (並需要) 精確地控制套用至每個動作的路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-775">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="82fb7-776">使用屬性路由，選取動作時「不會」\*\*\*\* 根據控制器名稱和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-776">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="82fb7-777">此範例會符合與上一個範例相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-777">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="82fb7-778">上述路由範本未定義 `action`、`area` 和 `controller` 的路由參數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-778">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="82fb7-779">事實上，屬性路由中不允許有這些路由參數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-779">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="82fb7-780">由於路由範本已與一個動作建立關聯，因此剖析 URL 中的動作名稱並沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="82fb7-780">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="82fb7-781">使用 Http[Verb] 屬性的屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-781">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="82fb7-782">屬性路由也可以使用 `Http[Verb]` 屬性，例如 `HttpPostAttribute`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-782">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="82fb7-783">這些屬性都會接受路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-783">All of these attributes can accept a route template.</span></span> <span data-ttu-id="82fb7-784">此範例顯示兩個符合相同路由範本的動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-784">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="82fb7-785">針對 `/products` 等 URL 路徑，當 HTTP 動詞命令為 `GET` 時，會執行 `ProductsApi.ListProducts`；當 HTTP 動詞命令為 `POST` 時，會執行 `ProductsApi.CreateProduct`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-785">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="82fb7-786">屬性路由會先根據路由屬性所定義的一組路由範本來比對 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-786">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="82fb7-787">一旦有路由範本相符，就會套用 `IActionConstraint` 條件約束以決定可執行的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-787">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="82fb7-788">建立 REST API 時，您很少會想要 `[Route(...)]` 在動作方法上使用，因為動作會接受所有的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-788">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="82fb7-789">最好是使用更明確的 `Http*Verb*Attributes`，以精確地指定 API 的支援項目。</span><span class="sxs-lookup"><span data-stu-id="82fb7-789">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="82fb7-790">REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="82fb7-790">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="82fb7-791">由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-791">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="82fb7-792">在此範例中，`id` 是 URL 路徑的必要部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-792">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="82fb7-793">`ProductsApi.GetProduct(int)` 動作會針對 `/products/3` 等 URL 路徑來執行，但不會針對 `/products` 等 URL 路徑來執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-793">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="82fb7-794">如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-794">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="82fb7-795">路由名稱</span><span class="sxs-lookup"><span data-stu-id="82fb7-795">Route Name</span></span>

<span data-ttu-id="82fb7-796">下列程式碼會定義 `Products_List` 的「路由名稱」\*\*：</span><span class="sxs-lookup"><span data-stu-id="82fb7-796">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="82fb7-797">您可以使用路由名稱根據特定路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-797">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="82fb7-798">路由名稱不會影響路由的 URL 比對行為，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-798">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="82fb7-799">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="82fb7-799">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-800">慣例「預設路由」\*\* 與此相反，只會將 `id` 參數定義為選擇性 (`{id?}`)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-800">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="82fb7-801">能夠精確指定 API 有些優點，像是允許將 `/products` 和 `/products/5` 分派至不同的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-801">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="82fb7-802">合併路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-802">Combining routes</span></span>

<span data-ttu-id="82fb7-803">為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-803">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="82fb7-804">控制器上定義的任何路由範本都會附加到動作上的路由範本之前。</span><span class="sxs-lookup"><span data-stu-id="82fb7-804">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="82fb7-805">將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-805">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="82fb7-806">在此範例中，URL 路徑 `/products` 可能符合 `ProductsApi.ListProducts`，而 URL 路徑 `/products/5` 可能符合 `ProductsApi.GetProduct(int)`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-806">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="82fb7-807">這兩個動作都只會符合 HTTP， `GET` 因為它們是以標示 `HttpGetAttribute` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-807">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="82fb7-808">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-808">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="82fb7-809">此範例會比對一組類似於「預設路由」\*\* 的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="82fb7-809">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="82fb7-810">排序屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-810">Ordering attribute routes</span></span>

<span data-ttu-id="82fb7-811">相對於以定義的循序執行的傳統路由，屬性路由會建立樹狀結構並同時符合所有路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-811">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="82fb7-812">此行為如同依理想的排序來排列路由項目，最明確的路由會有機會在較泛型的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-812">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="82fb7-813">例如，`blog/search/{topic}` 等路由比 `blog/{*article}` 等路由更明確。</span><span class="sxs-lookup"><span data-stu-id="82fb7-813">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="82fb7-814">邏輯上來說，預設會先「執行」`blog/search/{topic}`，因為這是唯一合理的排序。</span><span class="sxs-lookup"><span data-stu-id="82fb7-814">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="82fb7-815">使用慣例路由，開發人員會負責依想要的順序來排列路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-815">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="82fb7-816">您可以使用架構提供之所有路由屬性的 `Order` 屬性，來設定屬性路由的順序。</span><span class="sxs-lookup"><span data-stu-id="82fb7-816">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="82fb7-817">路由會依 `Order` 屬性的遞增排序來處理。</span><span class="sxs-lookup"><span data-stu-id="82fb7-817">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="82fb7-818">預設順序為 `0`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-818">The default order is `0`.</span></span> <span data-ttu-id="82fb7-819">使用 `Order = -1` 設定的路由會在未設定順序的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-819">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="82fb7-820">使用 `Order = 1` 設定的路由會在預設路由排序之後執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-820">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="82fb7-821">請避免依賴 `Order`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-821">Avoid depending on `Order`.</span></span> <span data-ttu-id="82fb7-822">如果您的 URL 空間需要明確的順序值才能正確地路由，則同樣也可能會使用戶端混淆。</span><span class="sxs-lookup"><span data-stu-id="82fb7-822">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="82fb7-823">一般而言，屬性路由會透過 URL 比對來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-823">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="82fb7-824">如果用於 URL 產生的預設順序無效，使用路由名稱作為覆寫通常會比套用 `Order` 屬性更簡單。</span><span class="sxs-lookup"><span data-stu-id="82fb7-824">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

Razor<span data-ttu-id="82fb7-825">頁面路由和 MVC 控制器路由會共用一個執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-825"> Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="82fb7-826">頁面主題中的路由順序資訊 Razor 可在[ Razor 頁面路由和應用程式慣例中取得：路由順序](xref:razor-pages/razor-pages-conventions#route-order)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-826">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="82fb7-827">路由範本中的語彙基元取代 ([controller]、[action]、[area])</span><span class="sxs-lookup"><span data-stu-id="82fb7-827">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="82fb7-828">為了方便起見，屬性路由支援以方括弧（，）括住標記來*取代標記* `[` `]` 。</span><span class="sxs-lookup"><span data-stu-id="82fb7-828">For convenience, attribute routes support *token replacement* by enclosing a token in square-brackets (`[`, `]`).</span></span> <span data-ttu-id="82fb7-829">語彙基元 `[action]`、`[area]` 與 `[controller]` 會分別以定義路由之動作的動作名稱值、區域名稱值和控制器名稱值來取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-829">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="82fb7-830">在下列範例中，動作會符合註解中所述的 URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="82fb7-830">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="82fb7-831">語彙基元取代會在建立屬性路由的最後一個步驟發生。</span><span class="sxs-lookup"><span data-stu-id="82fb7-831">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="82fb7-832">上述範例的運作方式與下列程式碼相同：</span><span class="sxs-lookup"><span data-stu-id="82fb7-832">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="82fb7-833">屬性路由也可以透過繼承來合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-833">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="82fb7-834">這與語彙基元取代結合會特別強大。</span><span class="sxs-lookup"><span data-stu-id="82fb7-834">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="82fb7-835">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-835">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="82fb7-836">`[Route("[controller]/[action]", Name="[controller]_[action]")]` 會針對每個動作產生唯一的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-836">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="82fb7-837">若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="82fb7-837">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="82fb7-838">使用參數轉換程式自訂語彙基元取代</span><span class="sxs-lookup"><span data-stu-id="82fb7-838">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="82fb7-839">可以使用參數轉換程式自訂語彙基元取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-839">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="82fb7-840">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-840">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="82fb7-841">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-841">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="82fb7-842">`RouteTokenTransformerConvention` 是一個應用程式模型慣例，它會：</span><span class="sxs-lookup"><span data-stu-id="82fb7-842">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="82fb7-843">將參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-843">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="82fb7-844">在取代屬性路由語彙基元值時自訂它們。</span><span class="sxs-lookup"><span data-stu-id="82fb7-844">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="82fb7-845">`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。</span><span class="sxs-lookup"><span data-stu-id="82fb7-845">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="82fb7-846">多個路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-846">Multiple Routes</span></span>

<span data-ttu-id="82fb7-847">屬性路由支援定義多個路由來達到相同的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-847">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="82fb7-848">最常見的用法是模擬「預設慣例路由」\*\* 的行為，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="82fb7-848">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="82fb7-849">將多個路由屬性放在控制器上，表示這些屬性會各自與動作方法上的每個路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-849">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="82fb7-850">當一個動作上放有多個路由屬性 (實作`IActionConstraint`) 時，則每個動作條件約束都會與屬性中用來定義的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="82fb7-850">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="82fb7-851">雖然在動作上使用多個路由看似強大，但最好保持簡單且妥善定義的應用程式 URL 空間。</span><span class="sxs-lookup"><span data-stu-id="82fb7-851">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="82fb7-852">只有必要時，才在動作上使用多個路由；例如，為了支援現有的用戶端。</span><span class="sxs-lookup"><span data-stu-id="82fb7-852">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="82fb7-853">指定屬性路由的選擇性參數、預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="82fb7-853">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="82fb7-854">屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。</span><span class="sxs-lookup"><span data-stu-id="82fb7-854">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="82fb7-855">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-855">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="82fb7-856">使用 `IRouteTemplateProvider` 的自訂路由屬性</span><span class="sxs-lookup"><span data-stu-id="82fb7-856">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="82fb7-857">架構中提供的所有路由屬性 (`[Route(...)]`、`[HttpGet(...)]` 等) 都會實作 `IRouteTemplateProvider` 介面。</span><span class="sxs-lookup"><span data-stu-id="82fb7-857">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="82fb7-858">當應用程式啟動並使用實作 `IRouteTemplateProvider` 的屬性來建立初始路由集時，MVC 會尋找控制器類別和動作方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-858">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="82fb7-859">您可以實作 `IRouteTemplateProvider` 來定義自己的路由屬性。</span><span class="sxs-lookup"><span data-stu-id="82fb7-859">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="82fb7-860">每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-860">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="82fb7-861">上述範例中的屬性會在套用 `[MyApiController]` 時，自動將 `Template` 設定為 `"api/[controller]"`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-861">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="82fb7-862">使用應用程式模型自訂屬性路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-862">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="82fb7-863">「應用程式模型」\*\* 是在啟動時建立的物件模型，其中包含 MVC 用來路由及執行動作的所有中繼資料。</span><span class="sxs-lookup"><span data-stu-id="82fb7-863">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="82fb7-864">「應用程式模型」\*\* 包含從路由屬性收集的所有資料 (透過 `IRouteTemplateProvider`)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-864">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="82fb7-865">您可以撰寫「慣例」\*\* 來修改啟動時的應用程式模型，以自訂路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-865">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="82fb7-866">本節簡單示範如何使用應用程式模型來自訂路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-866">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="82fb7-867">混合路由：屬性路由與慣例路由</span><span class="sxs-lookup"><span data-stu-id="82fb7-867">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="82fb7-868">MVC 應用程式可以混用慣例路由與屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-868">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="82fb7-869">控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。</span><span class="sxs-lookup"><span data-stu-id="82fb7-869">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="82fb7-870">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-870">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="82fb7-871">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-871">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="82fb7-872">定義屬性路由的動作無法透過慣例路由到達，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="82fb7-872">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="82fb7-873">控制器上的**任何**路由屬性可讓控制器中的所有動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-873">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-874">這兩種路由系統的區別在於 URL 符合某個路由範本之後所套用的程序。</span><span class="sxs-lookup"><span data-stu-id="82fb7-874">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="82fb7-875">在慣例路由中，會使用相符項目中的路由值，從所有慣例路由動作的查閱資料表中選擇動作和控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-875">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="82fb7-876">在屬性路由中，每個範本已與一個動作建立關聯，因此不需要進一步查閱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-876">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="82fb7-877">複雜區段</span><span class="sxs-lookup"><span data-stu-id="82fb7-877">Complex segments</span></span>

<span data-ttu-id="82fb7-878">複雜區段 (例如，`[Route("/dog{token}cat")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="82fb7-878">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="82fb7-879">如需說明，請參閱[原始程式碼](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-879">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="82fb7-880">如需詳細資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8197)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-880">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="82fb7-881">URL 產生</span><span class="sxs-lookup"><span data-stu-id="82fb7-881">URL Generation</span></span>

<span data-ttu-id="82fb7-882">MVC 應用程式可以使用路由的 URL 產生功能，來產生動作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="82fb7-882">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="82fb7-883">產生 URL 可排除硬式編碼的 URL，讓程式碼更穩定且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="82fb7-883">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="82fb7-884">本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 產生運作方式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="82fb7-884">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="82fb7-885">如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-885">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="82fb7-886">`IUrlHelper` 介面是 MVC 與用於產生 URL 的路由之間的基礎結構部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-886">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="82fb7-887">您將會透過控制器、檢視和檢視元件中 `Url` 屬性，來尋找可用的 `IUrlHelper` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="82fb7-887">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="82fb7-888">在此範例中，會透過 `Controller.Url` 屬性使用 `IUrlHelper` 介面來產生另一個動作的 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-888">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="82fb7-889">如果應用程式使用預設慣例路由，`url` 變數的值會是 URL 路徑字串 `/UrlGeneration/Destination`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-889">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="82fb7-890">此 URL 路徑是透過路由所建立，結合了目前要求中的路由值 (環境值) 與傳遞至 `Url.Action` 的值，並在路由範本中取代這些值：</span><span class="sxs-lookup"><span data-stu-id="82fb7-890">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="82fb7-891">路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。</span><span class="sxs-lookup"><span data-stu-id="82fb7-891">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="82fb7-892">不具有任何值的路由參數可以使用預設值 (若有)；如果是選擇性，則可以略過 (如此範例中的 `id` 所示)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-892">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="82fb7-893">如果任何必要的路由參數沒有對應的值，URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="82fb7-893">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="82fb7-894">如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。</span><span class="sxs-lookup"><span data-stu-id="82fb7-894">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="82fb7-895">上述 `Url.Action` 範例假設使用慣例路由，但 URL 產生的運作方式會類似屬性路由，雖然概念完全不同。</span><span class="sxs-lookup"><span data-stu-id="82fb7-895">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="82fb7-896">在慣例路由中，使用路由值來展開範本，而且 `controller` 和 `action` 的路由值通常會出現在該範本中 (這是因為路由比對的 URL 符合「慣例」\*\*)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-896">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="82fb7-897">在屬性路由中，`controller` 和 `action` 的路由值不可以出現在範本中，而是用來查閱要使用的範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-897">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="82fb7-898">此範例使用屬性路由：</span><span class="sxs-lookup"><span data-stu-id="82fb7-898">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="82fb7-899">MVC 建立所有屬性路由動作的查閱資料表，並將比對 `controller` 和 `action` 值，以選取要用於 URL 產生的路由範本。</span><span class="sxs-lookup"><span data-stu-id="82fb7-899">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="82fb7-900">在上述範例中，會產生 `custom/url/to/destination`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-900">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="82fb7-901">由動作名稱產生 URL</span><span class="sxs-lookup"><span data-stu-id="82fb7-901">Generating URLs by action name</span></span>

<span data-ttu-id="82fb7-902">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="82fb7-902">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="82fb7-903">`Action`) 及所有相關多載的基本概念，都是您想要透過指定控制器名稱和動作名稱，來指定連結的項目。</span><span class="sxs-lookup"><span data-stu-id="82fb7-903">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-904">使用 `Url.Action` 時，會為您指定`controller` 和 `action` 的目前路由值 (`controller` 和 `action` 的值都屬於「環境值」\*\* **和「值」** \*\*)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-904">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="82fb7-905">方法 `Url.Action` 一律會使用 `action` 和 `controller` 的目前值，而且會產生路由至目前動作的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="82fb7-905">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="82fb7-906">路由會嘗試使用環境值中的值，來填入您在產生 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="82fb7-906">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="82fb7-907">使用 `{a}/{b}/{c}/{d}` 等路由和環境值 `{ a = Alice, b = Bob, c = Carol, d = David }`，路由就會有足夠的資訊來產生不含任何其他值的 URL (因為所有路由參數都具有值)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-907">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="82fb7-908">如果您新增值 `{ d = Donovan }`，則會忽略值 `{ d = David }`，而且產生的 URL 路徑會是 `Alice/Bob/Carol/Donovan`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-908">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="82fb7-909">URL 路徑是階層式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-909">URL paths are hierarchical.</span></span> <span data-ttu-id="82fb7-910">在上述範例中，如果您新增了值 `{ c = Cheryl }`，則會忽略 `{ c = Carol, d = David }` 這兩個值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-910">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="82fb7-911">在此情況下，不再有 `d` 的值，因此 URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="82fb7-911">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="82fb7-912">您必須指定 `c` 和 `d` 所需的值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-912">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="82fb7-913">使用預設路由 (`{controller}/{action}/{id?}`) 可能會遇到此問題，但實際上很少會遇到此行為，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-913">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="82fb7-914">較長的 `Url.Action` 多載還會接受一個額外的「路由值」\*\* 物件，來為 `controller` 和 `action` 以外的路由參數提供值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-914">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="82fb7-915">您最常會在搭配 `id` 時看到此用法，例如 `Url.Action("Buy", "Products", new { id = 17 })`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-915">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="82fb7-916">依照慣例，「路由值」\*\* 物件通常是匿名型別的物件，但也可以是 `IDictionary<>` 或「純舊 .NET 物件」\*\*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-916">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="82fb7-917">不符合路由參數的任何額外路由值都會放在查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="82fb7-917">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="82fb7-918">若要建立絕對 URL，請使用接受 `protocol` 的多載：`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="82fb7-918">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="82fb7-919">由路由產生 URL</span><span class="sxs-lookup"><span data-stu-id="82fb7-919">Generating URLs by route</span></span>

<span data-ttu-id="82fb7-920">上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-920">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="82fb7-921">`IUrlHelper` 也提供 `Url.RouteUrl` 系列方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-921">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="82fb7-922">這些方法類似於 `Url.Action`，但不會將 `action` 和 `controller` 的目前值複製到路由值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-922">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="82fb7-923">最常見的用法是指定路由名稱使用特定路由來產生 URL，通常「不需要」\*\* 指定控制器或動作名稱。</span><span class="sxs-lookup"><span data-stu-id="82fb7-923">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="82fb7-924">在 HTML 中產生 URL</span><span class="sxs-lookup"><span data-stu-id="82fb7-924">Generating URLs in HTML</span></span>

<span data-ttu-id="82fb7-925">`IHtmlHelper` 提供 `HtmlHelper` 方法 `Html.BeginForm` 和 `Html.ActionLink`，以分別產生 `<form>` 和 `<a>` 項目。</span><span class="sxs-lookup"><span data-stu-id="82fb7-925">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="82fb7-926">這些方法使用 `Url.Action` 方法來產生 URL，並接受類似的引數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-926">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="82fb7-927">`HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。</span><span class="sxs-lookup"><span data-stu-id="82fb7-927">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="82fb7-928">TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-928">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="82fb7-929">這兩者使用 `IUrlHelper` 進行實作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-929">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="82fb7-930">如需詳細資訊，請參閱[使用表單](../views/working-with-forms.md)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-930">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="82fb7-931">在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-931">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="82fb7-932">在動作結果中產生 URL</span><span class="sxs-lookup"><span data-stu-id="82fb7-932">Generating URLS in Action Results</span></span>

<span data-ttu-id="82fb7-933">上述範例示範如何在控制器中使用 `IUrlHelper`，但控制器中最常見的用法是產生 URL 以作為動作結果的一部分。</span><span class="sxs-lookup"><span data-stu-id="82fb7-933">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="82fb7-934">`ControllerBase` 和 `Controller` 基底類別提供便利的方法讓動作結果可參考其他動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-934">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="82fb7-935">一個典型的用法是在接受使用者輸入之後重新導向。</span><span class="sxs-lookup"><span data-stu-id="82fb7-935">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="82fb7-936">動作結果的 Factory 方法遵循類似於 `IUrlHelper` 上方法的模式。</span><span class="sxs-lookup"><span data-stu-id="82fb7-936">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="82fb7-937">專用慣例路由的特殊案例</span><span class="sxs-lookup"><span data-stu-id="82fb7-937">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="82fb7-938">慣例路由可使用一種特殊路由定義，稱為「專用慣例路由」\*\*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-938">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="82fb7-939">在下列範例中，名為 `blog` 的路由是專用慣例路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-939">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="82fb7-940">透過這些路由定義，`Url.Action("Index", "Home")` 會使用 `default` 路由產生 URL 路徑 `/`，但為什麼？</span><span class="sxs-lookup"><span data-stu-id="82fb7-940">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="82fb7-941">您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-941">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="82fb7-942">專用慣例路由依賴沒有對應路由參數之預設值的特殊行為，以防止用於 URL 產生的路由變得「太窮盡」。</span><span class="sxs-lookup"><span data-stu-id="82fb7-942">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="82fb7-943">在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="82fb7-943">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="82fb7-944">當路由執行 URL 產生時，所提供的值必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="82fb7-944">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="82fb7-945">使用 `blog` 產生 URL 會失敗，因為值 `{ controller = Home, action = Index }` 不符合 `{ controller = Blog, action = Article }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-945">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="82fb7-946">路由會接著切換並嘗試 `default`，此時會成功。</span><span class="sxs-lookup"><span data-stu-id="82fb7-946">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="82fb7-947">區域</span><span class="sxs-lookup"><span data-stu-id="82fb7-947">Areas</span></span>

<span data-ttu-id="82fb7-948">[區域](areas.md)是 MVC 功能，可將相關功能組織成群組，作為個別路由命名空間 (適用於控制器動作) 和資料夾結構 (適用於檢視)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-948">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="82fb7-949">使用區域可讓應用程式具有多個同名的控制器 (只要這些控制器具有不同的「區域」\*\* 即可)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-949">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="82fb7-950">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-950">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="82fb7-951">本節將討論路由如何與區域互動；如需區域如何與檢視搭配使用的詳細資料，請參閱[區域](areas.md)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-951">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="82fb7-952">下列範例會設定 MVC 使用預設慣例路由，並為名為 `Blog` 的區域設定「區域路由」\*\*：</span><span class="sxs-lookup"><span data-stu-id="82fb7-952">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="82fb7-953">當符合 `/Manage/Users/AddUser` 等 URL 路徑時，第一個路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-953">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="82fb7-954">`area` 路由值是由 `area` 的預設值所產生；事實上，`MapAreaRoute` 所建立的路由相當於：</span><span class="sxs-lookup"><span data-stu-id="82fb7-954">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="82fb7-955">`MapAreaRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-955">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="82fb7-956">預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="82fb7-956">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="82fb7-957">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="82fb7-957">Conventional routing is order-dependent.</span></span> <span data-ttu-id="82fb7-958">一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="82fb7-958">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="82fb7-959">使用上述範例，路由值會符合下列動作：</span><span class="sxs-lookup"><span data-stu-id="82fb7-959">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="82fb7-960">`AreaAttribute` 可用來指定控制器為區域的一部分，假設此控制器在 `Blog` 區域中。</span><span class="sxs-lookup"><span data-stu-id="82fb7-960">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="82fb7-961">不含 `[Area]` 屬性的控制器不是任何區域的成員，因此當路由提供 `area` 路由值時**不會**符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-961">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="82fb7-962">在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-962">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="82fb7-963">為求完整起見，此處顯示每個控制器的命名空間，否則控制器會有命名衝突並產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="82fb7-963">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="82fb7-964">類別命名空間不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="82fb7-964">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="82fb7-965">前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-965">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="82fb7-966">第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-966">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-967">在「未符合任何值」\*\* 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="82fb7-967">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="82fb7-968">執行區域中的動作時，`area` 的路由值會作為路由用於 URL 產生的「環境值」\*\*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-968">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="82fb7-969">這表示區域預設會以「黏性」\*\* 方式來處理 URL 產生，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="82fb7-969">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="82fb7-970">了解 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="82fb7-970">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="82fb7-971">本節深入探討架構內部及 MVC 如何選擇要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-971">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="82fb7-972">一般應用程式不需要自訂 `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="82fb7-972">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="82fb7-973">即使您不熟悉 `IActionConstraint`，也可能已使用過此介面。</span><span class="sxs-lookup"><span data-stu-id="82fb7-973">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="82fb7-974">`[HttpGet]` 屬性和類似的 `[Http-VERB]` 屬性會實作 `IActionConstraint` 來限制動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-974">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="82fb7-975">假設在預設慣例路由中，URL 路徑 `/Products/Edit` 會產生值 `{ controller = Products, action = Edit }`，該值符合此處所示的**兩個**動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-975">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="82fb7-976">在 `IActionConstraint` 術語中，我們會假設這兩個動作可視為候選項目 (因為它們都符合路由資料)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-976">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="82fb7-977">當 `HttpGetAttribute` 執行時，*Edit()* 會符合 *GET*，但不符合任何其他 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="82fb7-977">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="82fb7-978">`Edit(...)` 動作未定義任何條件約束，因此會符合任何 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="82fb7-978">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="82fb7-979">假設 `POST` - 只有 `Edit(...)` 相符。</span><span class="sxs-lookup"><span data-stu-id="82fb7-979">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="82fb7-980">但對於 `GET`，這兩個動作仍然相符；不過，具有 `IActionConstraint` 的動作一律比沒有的動作「更符合」\*\*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-980">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="82fb7-981">由於 `Edit()` 具有 `[HttpGet]`，因此更明確；如果兩個動作都相符，就會選取此動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-981">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="82fb7-982">就概念而言，`IActionConstraint` 是一種「多載」\*\* 形式，但它會在符合相同 URL　的動作之間進行多載，而不是多載具有相同名稱的方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-982">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="82fb7-983">屬性路由也會使用 `IActionConstraint`，因此可能導致來自不同控制器的動作被視為候選項目。</span><span class="sxs-lookup"><span data-stu-id="82fb7-983">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="82fb7-984">實作 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="82fb7-984">Implementing IActionConstraint</span></span>

<span data-ttu-id="82fb7-985">實作 `IActionConstraint` 的最簡單方式是建立衍生自 `System.Attribute` 的類別，並將它放在您的動作和控制器上。</span><span class="sxs-lookup"><span data-stu-id="82fb7-985">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="82fb7-986">MVC 會自動探索作為屬性套用的任何 `IActionConstraint`。</span><span class="sxs-lookup"><span data-stu-id="82fb7-986">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="82fb7-987">您可以使用應用程式模型來套用條件約束，由於您可以之後編寫其套用方式的程式，因此可能是最彈性的方法。</span><span class="sxs-lookup"><span data-stu-id="82fb7-987">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="82fb7-988">在下列範例中，條件約束會根據路由資料中的*國家（地區）代碼*來選擇動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-988">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="82fb7-989">[GitHub 上有完整範例](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。</span><span class="sxs-lookup"><span data-stu-id="82fb7-989">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="82fb7-990">您必須負責實作 `Accept` 方法，並選擇 'Order' 作為要執行的條件約束。</span><span class="sxs-lookup"><span data-stu-id="82fb7-990">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="82fb7-991">在本例中，`Accept` 方法會傳回 `true`，表示當 `country` 路由值相符時動作會符合。</span><span class="sxs-lookup"><span data-stu-id="82fb7-991">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="82fb7-992">這與 `RouteValueAttribute` 不同，後者允許切換回未使用屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="82fb7-992">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="82fb7-993">此範例顯示如果您定義 `en-US` 動作，則 `fr-FR` 等國碼 (地區碼) 會切換回未套用 `[CountrySpecific(...)]` 的更泛型控制器。</span><span class="sxs-lookup"><span data-stu-id="82fb7-993">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="82fb7-994">`Order` 屬性決定條件約束所屬的「階段」\*\*。</span><span class="sxs-lookup"><span data-stu-id="82fb7-994">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="82fb7-995">群組中的動作條件約束會根據 `Order` 來執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-995">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="82fb7-996">例如，架構提供的所有 HTTP 方法屬性都會使用相同的 `Order` 值，以便在同一個階段中執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-996">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="82fb7-997">您可以視需要擁有許多階段來實作所需的原則。</span><span class="sxs-lookup"><span data-stu-id="82fb7-997">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="82fb7-998">若要決定 `Order` 的值，請考慮是否應該在 HTTP 方法之前套用條件約束。</span><span class="sxs-lookup"><span data-stu-id="82fb7-998">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="82fb7-999">數值愈低會愈先執行。</span><span class="sxs-lookup"><span data-stu-id="82fb7-999">Lower numbers run first.</span></span>

::: moniker-end
