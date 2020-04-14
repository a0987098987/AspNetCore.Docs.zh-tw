---
title: ASP.NET Core 中的路由至控制器動作
author: rick-anderson
description: 了解 ASP.NET Core MVC 如何使用路由中介軟體來比對內送要求的 URL，並將這些 URL 對應至動作。
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: 9f7a26a482cb115697a0a3d7439c14a062677c92
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277128"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="664e5-103">ASP.NET Core 中的路由至控制器動作</span><span class="sxs-lookup"><span data-stu-id="664e5-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="664e5-104">由[里安·諾瓦克](https://github.com/rynowak),[柯克·拉金](https://twitter.com/serpent5)和[里克·安德森](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="664e5-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="664e5-105">ASP.NET核心控制器使用路由[中間件](xref:fundamentals/middleware/index)來符合傳入請求的網址 並將其對應到[操作](#action)。</span><span class="sxs-lookup"><span data-stu-id="664e5-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="664e5-106">路由範本:</span><span class="sxs-lookup"><span data-stu-id="664e5-106">Routes templates:</span></span>

* <span data-ttu-id="664e5-107">在啟動代碼或屬性中定義。</span><span class="sxs-lookup"><span data-stu-id="664e5-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="664e5-108">描述 URL 路徑如何與[操作](#action)匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="664e5-109">用於生成連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="664e5-110">生成的連結通常在回應中返回。</span><span class="sxs-lookup"><span data-stu-id="664e5-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="664e5-111">操作是[一般路由](#cr)的,要麼是[屬性路由的](#ar)。</span><span class="sxs-lookup"><span data-stu-id="664e5-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="664e5-112">在控制器或[操作](#action)上放置路由可使其屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="664e5-113">如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。</span><span class="sxs-lookup"><span data-stu-id="664e5-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="664e5-114">本文件:</span><span class="sxs-lookup"><span data-stu-id="664e5-114">This document:</span></span>

* <span data-ttu-id="664e5-115">解釋 MVC 和路由之間的互動:</span><span class="sxs-lookup"><span data-stu-id="664e5-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="664e5-116">典型的MVC應用如何使用路由功能。</span><span class="sxs-lookup"><span data-stu-id="664e5-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="664e5-117">涵蓋兩者:</span><span class="sxs-lookup"><span data-stu-id="664e5-117">Covers both:</span></span>
    * <span data-ttu-id="664e5-118">[常規路由](#cr)通常與控制器和視圖一起使用。</span><span class="sxs-lookup"><span data-stu-id="664e5-118">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="664e5-119">與 REST API 一起使用*的屬性路由*。</span><span class="sxs-lookup"><span data-stu-id="664e5-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="664e5-120">如果您主要對 REST API 的路由感興趣,請跳轉到[REST API 部份的屬性路由](#ar)。</span><span class="sxs-lookup"><span data-stu-id="664e5-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="664e5-121">有關進階路由詳細資訊,請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="664e5-122">指在ASP.NET Core 3.0 中添加的預設路由系統,稱為終結點路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="664e5-123">出於相容性目的,可以使用控制器與早期版本的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="664e5-124">有關說明,請參閱[2.2-3.0 遷移指南](xref:migration/22-to-30)。</span><span class="sxs-lookup"><span data-stu-id="664e5-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="664e5-125">有關舊路由系統的參考資料,請參閱[本文件的 2.2 版本](xref:mvc/controllers/routing?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="664e5-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="664e5-126">設定一般路線</span><span class="sxs-lookup"><span data-stu-id="664e5-126">Set up conventional route</span></span>

<span data-ttu-id="664e5-127">`Startup.Configure`使用[一般路由](#crd)時,通常具有類似於以下內容的代碼:</span><span class="sxs-lookup"><span data-stu-id="664e5-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="664e5-128">調用<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>中<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>用於創建單個路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="664e5-129">單個路由名為`default`路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-129">The single route is named `default` route.</span></span> <span data-ttu-id="664e5-130">大多數具有控制器和視圖的應用使用與`default`路由類似的路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="664e5-131">REST API 應使用[屬性路由](#ar)。</span><span class="sxs-lookup"><span data-stu-id="664e5-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="664e5-132">路由樣本`"{controller=Home}/{action=Index}/{id?}"`:</span><span class="sxs-lookup"><span data-stu-id="664e5-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="664e5-133">符合網址路徑,如`/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="664e5-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="664e5-134">通過標記路徑來提取`{ controller = Products, action = Details, id = 5 }`路由值。</span><span class="sxs-lookup"><span data-stu-id="664e5-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="664e5-135">如果應用程式具有名為`ProductsController`控制器`Details`和 操作的控制器,則路由值的提取將生成匹配:</span><span class="sxs-lookup"><span data-stu-id="664e5-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

  [!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

* <span data-ttu-id="664e5-136">`/Products/Details/5`模型將 的`id = 5`值 繫結`id`為 將`5`參數設定為 。</span><span class="sxs-lookup"><span data-stu-id="664e5-136">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="664e5-137">有關詳細資訊,請參閱[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="664e5-137">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="664e5-138">`{controller=Home}`定義為`Home`預設`controller`值 。</span><span class="sxs-lookup"><span data-stu-id="664e5-138">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="664e5-139">`{action=Index}`定義為`Index`預設`action`值 。</span><span class="sxs-lookup"><span data-stu-id="664e5-139">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="664e5-140">中的`?``{id?}`字`id`元 定義為可選。</span><span class="sxs-lookup"><span data-stu-id="664e5-140">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="664e5-141">預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。</span><span class="sxs-lookup"><span data-stu-id="664e5-141">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="664e5-142">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="664e5-142">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="664e5-143">符合網`/`址路徑 。</span><span class="sxs-lookup"><span data-stu-id="664e5-143">Matches the URL path `/`.</span></span>
* <span data-ttu-id="664e5-144">產生路由值`{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-144">Produces the route values `{ controller = Home, action = Index }`.</span></span>

<span data-ttu-id="664e5-145">的值`controller``action`和 預設值的使用。</span><span class="sxs-lookup"><span data-stu-id="664e5-145">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="664e5-146">`id`不會生成值,因為 URL 路徑中沒有相應的段。</span><span class="sxs-lookup"><span data-stu-id="664e5-146">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="664e5-147">`/`只當存在與`HomeController``Index`操作時符合:</span><span class="sxs-lookup"><span data-stu-id="664e5-147">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="664e5-148">使用前面控制器定義與路由樣本,`HomeController.Index`操作會針對以下網址路徑執行:</span><span class="sxs-lookup"><span data-stu-id="664e5-148">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="664e5-149">URL`/`路徑 使用路由範`Home`本預設`Index`控制器和 操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-149">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="664e5-150">URL`/Home`路徑 使用路由範`Index`本預設 操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-150">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="664e5-151"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*> 方法很方便：</span><span class="sxs-lookup"><span data-stu-id="664e5-151">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="664e5-152">取代:</span><span class="sxs-lookup"><span data-stu-id="664e5-152">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

> [!IMPORTANT]
> <span data-ttu-id="664e5-153">路由使用<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*>和<xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>中間件進行配置。</span><span class="sxs-lookup"><span data-stu-id="664e5-153">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="664e5-154">要使用控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-154">To use controllers:</span></span>
>
> * <span data-ttu-id="664e5-155">調用<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>`UseEndpoints`內部 以映射[屬性路由](#ar)控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-155">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
> * <span data-ttu-id="664e5-156">呼叫<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>或,以映射[一般路由](#cr)的控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-156">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="664e5-157">慣例路由</span><span class="sxs-lookup"><span data-stu-id="664e5-157">Conventional routing</span></span>

<span data-ttu-id="664e5-158">常規路由與控制器和視圖一起使用。</span><span class="sxs-lookup"><span data-stu-id="664e5-158">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="664e5-159">`default` 路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-159">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="664e5-160">即為「慣例路由」\*\* 的範例。</span><span class="sxs-lookup"><span data-stu-id="664e5-160">is an example of a *conventional routing*.</span></span> <span data-ttu-id="664e5-161">它被稱為*傳統路由*,因為它為 URL 路徑建立了*約定*:</span><span class="sxs-lookup"><span data-stu-id="664e5-161">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="664e5-162">第一個路徑段`{controller=Home}`映射到控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-162">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="664e5-163">第二個段`{action=Index}`, 映射到[操作](#action)名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-163">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="664e5-164">第三個段`{id?}`可以選擇`id`。</span><span class="sxs-lookup"><span data-stu-id="664e5-164">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="664e5-165">in`?``{id?}`使其可選。</span><span class="sxs-lookup"><span data-stu-id="664e5-165">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="664e5-166">`id`用於映射到模型實體。</span><span class="sxs-lookup"><span data-stu-id="664e5-166">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="664e5-167">使用此`default`路由,URL 路徑:</span><span class="sxs-lookup"><span data-stu-id="664e5-167">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="664e5-168">`/Products/List`映射到`ProductsController.List`操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-168">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="664e5-169">`/Blog/Article/17`映射到`BlogController.Article`和通常模型`id`將 參數綁定到 17。</span><span class="sxs-lookup"><span data-stu-id="664e5-169">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="664e5-170">此對應:</span><span class="sxs-lookup"><span data-stu-id="664e5-170">This mapping:</span></span>

* <span data-ttu-id="664e5-171">**僅**基於控制器和[操作](#action)名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-171">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="664e5-172">不基於命名空間、源檔位置或方法參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-172">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="664e5-173">使用常規路由與預設路由允許創建應用,而無需為每個操作提出新的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="664e5-173">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="664e5-174">對於具有[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)樣式操作的應用,具有跨控制器的網址的一致性:</span><span class="sxs-lookup"><span data-stu-id="664e5-174">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="664e5-175">有助於簡化代碼。</span><span class="sxs-lookup"><span data-stu-id="664e5-175">Helps simplify the code.</span></span>
* <span data-ttu-id="664e5-176">使 UI 更加可預測。</span><span class="sxs-lookup"><span data-stu-id="664e5-176">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="664e5-177">前面的`id`代碼中由工藝路線範本定義為可選。</span><span class="sxs-lookup"><span data-stu-id="664e5-177">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="664e5-178">操作可以在沒有作為 URL 的一部分提供的可選 ID 的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-178">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="664e5-179">通常,從`id`網址中省略時:</span><span class="sxs-lookup"><span data-stu-id="664e5-179">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="664e5-180">`id`由模型綁定`0`設置為。</span><span class="sxs-lookup"><span data-stu-id="664e5-180">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="664e5-181">在資料庫匹配`id == 0`中找不到任何實體。</span><span class="sxs-lookup"><span data-stu-id="664e5-181">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="664e5-182">[屬性路由](#ar)提供細粒度控制項,使某些操作不需要 ID,而不是其他操作的 ID。</span><span class="sxs-lookup"><span data-stu-id="664e5-182">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="664e5-183">按照慣例,文檔包括可選參數,`id`例如它們可能以正確使用的方式出現。</span><span class="sxs-lookup"><span data-stu-id="664e5-183">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="664e5-184">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="664e5-184">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="664e5-185">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="664e5-185">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="664e5-186">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="664e5-186">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="664e5-187">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="664e5-187">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="664e5-188">是許多 Web UI 應用所需的唯一路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-188">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="664e5-189">對於較大的 Web UI 應用,使用[「區域](#areas)」的另一個路由(如果經常是所有所需的內容)。</span><span class="sxs-lookup"><span data-stu-id="664e5-189">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="664e5-190"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>與<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:</span><span class="sxs-lookup"><span data-stu-id="664e5-190"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="664e5-191">根據調用其終結點的順序自動為其終結點分配**訂單**值。</span><span class="sxs-lookup"><span data-stu-id="664e5-191">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="664e5-192">ASP.NET Core 3.0 及更高版本中的終結點路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-192">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="664e5-193">沒有路線的概念。</span><span class="sxs-lookup"><span data-stu-id="664e5-193">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="664e5-194">不為執行擴充性提供命令保證,所有終結點都一次性處理。</span><span class="sxs-lookup"><span data-stu-id="664e5-194">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="664e5-195">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="664e5-195">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="664e5-196">[屬性路由](#ar)將在本文中稍後介紹。</span><span class="sxs-lookup"><span data-stu-id="664e5-196">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="664e5-197">多條常規路線</span><span class="sxs-lookup"><span data-stu-id="664e5-197">Multiple conventional routes</span></span>

<span data-ttu-id="664e5-198">通過向<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>和添加更多調用`UseEndpoints`,可以添加多個[常規路由。](#cr) <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*></span><span class="sxs-lookup"><span data-stu-id="664e5-198">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="664e5-199">這樣做允許定義多個約定,或添加專用於特定[操作](#action)的傳統路由,例如:</span><span class="sxs-lookup"><span data-stu-id="664e5-199">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="664e5-200">前面`blog`代碼中的路由是**專用的一般路由**。</span><span class="sxs-lookup"><span data-stu-id="664e5-200">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="664e5-201">它被稱為專用常規路由,因為:</span><span class="sxs-lookup"><span data-stu-id="664e5-201">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="664e5-202">它使用[傳統的路由](#cr)。</span><span class="sxs-lookup"><span data-stu-id="664e5-202">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="664e5-203">它致力於一個具體[的行動](#action)。</span><span class="sxs-lookup"><span data-stu-id="664e5-203">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="664e5-204">因為`controller``action`並且不以參數顯示在工藝`"blog/{*article}"`路線 樣本中:</span><span class="sxs-lookup"><span data-stu-id="664e5-204">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="664e5-205">它們只能具有預設值`{ controller = "Blog", action = "Article" }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-205">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="664e5-206">這條路線總是映射到操作`BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="664e5-206">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="664e5-207">`/Blog``/Blog/Article`,並且`/Blog/{any-string}`是唯一與博客路由匹配的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="664e5-207">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="664e5-208">前面的範例:</span><span class="sxs-lookup"><span data-stu-id="664e5-208">The preceding example:</span></span>

* <span data-ttu-id="664e5-209">`blog`路由的匹配優先順序高於路由,`default`因為它首先添加。</span><span class="sxs-lookup"><span data-stu-id="664e5-209">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="664e5-210">是[Slug](https://developer.mozilla.org/docs/Glossary/Slug)樣式路由的範例,其中通常將文章名稱作為網址的一部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-210">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="664e5-211">在ASP.NET核心 3.0 及更高版本中,路由不會:</span><span class="sxs-lookup"><span data-stu-id="664e5-211">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="664e5-212">定義一個稱為*路由*的概念。</span><span class="sxs-lookup"><span data-stu-id="664e5-212">Define a concept called a *route*.</span></span> <span data-ttu-id="664e5-213">`UseRouting`將路由匹配添加到中間件管道。</span><span class="sxs-lookup"><span data-stu-id="664e5-213">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="664e5-214">中間`UseRouting`件查看應用中定義的終結點集,並根據請求選擇最佳終結點匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-214">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="664e5-215">提供有關擴充性執行順序的保證,例如<xref:Microsoft.AspNetCore.Routing.IRouteConstraint>或<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>。</span><span class="sxs-lookup"><span data-stu-id="664e5-215">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="664e5-216">有關路由的參考資料,請參閱[工藝路線](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-216">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="664e5-217">一般路由順序</span><span class="sxs-lookup"><span data-stu-id="664e5-217">Conventional routing order</span></span>

<span data-ttu-id="664e5-218">常規路由僅與應用定義的操作和控制器的組合匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-218">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="664e5-219">這旨在簡化傳統路由重疊的情況。</span><span class="sxs-lookup"><span data-stu-id="664e5-219">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="664e5-220">使用<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>添加路由,並<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>根據調用其終結點的順序自動為其終結點分配訂單值。</span><span class="sxs-lookup"><span data-stu-id="664e5-220">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="664e5-221">前面顯示的路由的匹配具有更高的優先順序。</span><span class="sxs-lookup"><span data-stu-id="664e5-221">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="664e5-222">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="664e5-222">Conventional routing is order-dependent.</span></span> <span data-ttu-id="664e5-223">通常,具有區域的路由應更早放置,因為它們比沒有區域的路由更具體。</span><span class="sxs-lookup"><span data-stu-id="664e5-223">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="664e5-224">具有 Catch 所有路由`{*article}`參數的[專用常規路由](#dcr)會使路由過於[貪婪](xref:fundamentals/routing#greedy),這意味著它匹配了您打算與其他路由匹配的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-224">[Dedicated conventional routes](#dcr) with catch all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="664e5-225">稍後將貪婪路由放在路由表中,以防止貪婪匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-225">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="664e5-226">解決模稜兩可的操作</span><span class="sxs-lookup"><span data-stu-id="664e5-226">Resolving ambiguous actions</span></span>

<span data-ttu-id="664e5-227">當兩個終結點通過路由匹配時,路由必須執行以下操作之一:</span><span class="sxs-lookup"><span data-stu-id="664e5-227">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="664e5-228">選擇最佳候選人。</span><span class="sxs-lookup"><span data-stu-id="664e5-228">Choose the best candidate.</span></span>
* <span data-ttu-id="664e5-229">擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="664e5-229">Throw an exception.</span></span>

<span data-ttu-id="664e5-230">例如：</span><span class="sxs-lookup"><span data-stu-id="664e5-230">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="664e5-231">前面控制器定義了兩個符合的操作:</span><span class="sxs-lookup"><span data-stu-id="664e5-231">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="664e5-232">網址路徑`/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="664e5-232">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="664e5-233">路由資料`{ controller = Products33, action = Edit, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-233">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="664e5-234">這是 MVC 控制器的典型模式:</span><span class="sxs-lookup"><span data-stu-id="664e5-234">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="664e5-235">`Edit(int)`顯示用於編輯產品的窗體。</span><span class="sxs-lookup"><span data-stu-id="664e5-235">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="664e5-236">`Edit(int, Product)`處理過帳的表單。</span><span class="sxs-lookup"><span data-stu-id="664e5-236">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="664e5-237">要解析正確的路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-237">To resolve the correct route:</span></span>

* <span data-ttu-id="664e5-238">`Edit(int, Product)`當要求為`POST`HTTP 時,將選擇 。</span><span class="sxs-lookup"><span data-stu-id="664e5-238">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="664e5-239">`Edit(int)`當 HTTP[謂詞](#verb)是任何其他內容時,將選中。</span><span class="sxs-lookup"><span data-stu-id="664e5-239">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="664e5-240">`Edit(int)`通常透過`GET`呼叫 。</span><span class="sxs-lookup"><span data-stu-id="664e5-240">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="664e5-241">提供<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>`[HttpPost]`到路由,以便它可以根據請求的 HTTP 方法進行選擇。</span><span class="sxs-lookup"><span data-stu-id="664e5-241">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="664e5-242">比比符合`Edit(int)` `HttpPostAttribute` `Edit(int, Product)`</span><span class="sxs-lookup"><span data-stu-id="664e5-242">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="664e5-243">瞭解屬性(如`HttpPostAttribute`)的作用非常重要。</span><span class="sxs-lookup"><span data-stu-id="664e5-243">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="664e5-244">其他[HTTP 謂詞](#verb)定義了類似的屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-244">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="664e5-245">[在常規路由](#cr)中,當操作是顯示表單的一部分時,通常使用相同的操作名稱,提交表單工作流。</span><span class="sxs-lookup"><span data-stu-id="664e5-245">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="664e5-246">例如,請參閱[檢查兩種編輯操作方法](xref:tutorials/first-mvc-app/controller-methods-views#get-post)。</span><span class="sxs-lookup"><span data-stu-id="664e5-246">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="664e5-247">如果路由無法選擇最佳候選項,則引發<xref:System.Reflection.AmbiguousMatchException>,列出多個匹配的終結點。</span><span class="sxs-lookup"><span data-stu-id="664e5-247">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="664e5-248">一般路由名稱</span><span class="sxs-lookup"><span data-stu-id="664e5-248">Conventional route names</span></span>

<span data-ttu-id="664e5-249">字串`"blog"`與`"default"`以下範例中是一般路由名稱:</span><span class="sxs-lookup"><span data-stu-id="664e5-249">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="664e5-250">路由名稱為路由指定一個邏輯名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-250">The route names give the route a logical name.</span></span> <span data-ttu-id="664e5-251">命名路由可用於 URL 生成。</span><span class="sxs-lookup"><span data-stu-id="664e5-251">The named route can be used for URL generation.</span></span> <span data-ttu-id="664e5-252">當路由的順序會使 URL 生成變得複雜時,使用命名路由可簡化 URL 創建。</span><span class="sxs-lookup"><span data-stu-id="664e5-252">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="664e5-253">路由名稱必須是唯一的應用程式寬。</span><span class="sxs-lookup"><span data-stu-id="664e5-253">Route names must be unique application wide.</span></span>

<span data-ttu-id="664e5-254">路由名稱:</span><span class="sxs-lookup"><span data-stu-id="664e5-254">Route names:</span></span>

* <span data-ttu-id="664e5-255">對 URL 匹配或請求處理沒有影響。</span><span class="sxs-lookup"><span data-stu-id="664e5-255">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="664e5-256">僅用於 URL 生成。</span><span class="sxs-lookup"><span data-stu-id="664e5-256">Are used only for URL generation.</span></span>

<span data-ttu-id="664e5-257">路由名稱概念在路由中表示為[IEndpointName 中繼資料](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata)。</span><span class="sxs-lookup"><span data-stu-id="664e5-257">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="664e5-258">字語**路由名稱**與**終結點名稱**:</span><span class="sxs-lookup"><span data-stu-id="664e5-258">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="664e5-259">可互換。</span><span class="sxs-lookup"><span data-stu-id="664e5-259">Are interchangeable.</span></span>
* <span data-ttu-id="664e5-260">在文件和代碼中使用的哪個取決於描述的 API。</span><span class="sxs-lookup"><span data-stu-id="664e5-260">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="664e5-261">REST API 屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-261">Attribute routing for REST APIs</span></span>

<span data-ttu-id="664e5-262">REST API 應使用屬性路由將應用的功能建模為一組資源,其中操作由[HTTP 謂詞](#verb)表示。</span><span class="sxs-lookup"><span data-stu-id="664e5-262">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="664e5-263">屬性路由使用一組屬性，將動作直接對應至路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-263">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="664e5-264">以下`StartUp.Configure`代碼是 REST API 的典型代碼,在下一個範例中使用:</span><span class="sxs-lookup"><span data-stu-id="664e5-264">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="664e5-265">在前面的代碼中,<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>在`UseEndpoints`內部 調用映射屬性路由控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-265">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="664e5-266">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="664e5-266">In the following example:</span></span>

* <span data-ttu-id="664e5-267">使用上述`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-267">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="664e5-268">`HomeController`匹配一組類似於預設常規路由`{controller=Home}/{action=Index}/{id?}`匹配的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-268">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="664e5-269">操作`HomeController.Index`為任何`/``/Home`URL`/Home/Index``/Home/Index/3`路徑 、或運行。</span><span class="sxs-lookup"><span data-stu-id="664e5-269">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="664e5-270">此示例突出顯示屬性路由[和傳統路由](#cr)之間的關鍵程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="664e5-270">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="664e5-271">屬性路由需要更多的輸入來指定路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-271">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="664e5-272">傳統的預設路由處理路由更簡潔。</span><span class="sxs-lookup"><span data-stu-id="664e5-272">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="664e5-273">但是,屬性路由允許並且需要精確控制應用於每個[操作](#action)的路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-273">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="664e5-274">使用屬性路由時,控制器名稱和操作名稱在匹配操作時**不扮演任何**角色。</span><span class="sxs-lookup"><span data-stu-id="664e5-274">With attribute routing, the controller name and action names play **no** role in which action is matched.</span></span> <span data-ttu-id="664e5-275">以下範例與上一範例符合相同的網址:</span><span class="sxs-lookup"><span data-stu-id="664e5-275">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="664e5-276">以下代碼使用和`action``controller`的權杖替換。</span><span class="sxs-lookup"><span data-stu-id="664e5-276">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="664e5-277">以下代碼適用於`[Route("[controller]/[action]")]`控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-277">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="664e5-278">在前面的代碼中,`Index`方法樣本必須準備`/``~/`或 到工藝路線範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-278">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="664e5-279">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-279">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="664e5-280">有關工藝路線範本選擇的資訊,請參閱[工藝路線範本優先順序](xref:fundamentals/routing#rtp)。</span><span class="sxs-lookup"><span data-stu-id="664e5-280">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="664e5-281">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="664e5-281">Reserved routing names</span></span>

<span data-ttu-id="664e5-282">使用控制器或剃刀頁時,以下關鍵字為保留路由參數名稱:</span><span class="sxs-lookup"><span data-stu-id="664e5-282">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="664e5-283">使用`page`作為具有屬性路由的路由參數是一個常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="664e5-283">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="664e5-284">這樣做會導致與 URL 生成不一致且令人困惑的行為。</span><span class="sxs-lookup"><span data-stu-id="664e5-284">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="664e5-285">URL 生成使用特殊參數名稱來確定網址生成操作是引用 Razor 頁還是控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-285">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="664e5-286">HTTP 動詞樣本</span><span class="sxs-lookup"><span data-stu-id="664e5-286">HTTP verb templates</span></span>

<span data-ttu-id="664e5-287">ASP.NET核心具有以下 HTTP 謂詞範本:</span><span class="sxs-lookup"><span data-stu-id="664e5-287">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="664e5-288">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-288">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="664e5-289">[[ HTTPPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-289">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="664e5-290">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-290">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="664e5-291">[[ HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-291">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="664e5-292">[[HTTPHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-292">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="664e5-293">[[HTTPPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-293">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="664e5-294">路由範本</span><span class="sxs-lookup"><span data-stu-id="664e5-294">Route templates</span></span>

<span data-ttu-id="664e5-295">ASP.NET核心具有以下路由範本:</span><span class="sxs-lookup"><span data-stu-id="664e5-295">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="664e5-296">所有[HTTP 謂詞範本](#verb)都是路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-296">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="664e5-297">[[路線]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="664e5-297">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="664e5-298">具有 HTTP 謂詞屬性的屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-298">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="664e5-299">請考慮以下控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-299">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="664e5-300">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="664e5-300">In the preceding code:</span></span>

* <span data-ttu-id="664e5-301">每個操作都包含該`[HttpGet]`屬性,該屬性僅約束與 HTTP GET 請求的匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-301">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="664e5-302">該`GetProduct`操作`"{id}"`包括 範本`id`,因此將追加到控制`"api/[controller]"`器上的 範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-302">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="664e5-303">方法樣本為`"api/[controller]/"{id}""`。</span><span class="sxs-lookup"><span data-stu-id="664e5-303">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="664e5-304">因此,此操作僅匹配窗體`/api/test2/xyz`的`/api/test2/123`GET`/api/test2/{any string}`請求, 、 等。</span><span class="sxs-lookup"><span data-stu-id="664e5-304">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="664e5-305">該`GetIntProduct`操作`"int/{id:int}")`包含範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-305">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="664e5-306">`:int`樣本部分將`id`路由值約束到可以轉換為整數的字串。</span><span class="sxs-lookup"><span data-stu-id="664e5-306">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="664e5-307">到`/api/test2/int/abc`的 GET 要求:</span><span class="sxs-lookup"><span data-stu-id="664e5-307">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="664e5-308">與此操作不匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-308">Doesn't match this action.</span></span>
  * <span data-ttu-id="664e5-309">返回[404 未找到](https://developer.mozilla.org/docs/Web/HTTP/Status/404)錯誤。</span><span class="sxs-lookup"><span data-stu-id="664e5-309">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="664e5-310">該`GetInt2Product`操作包含`{id}`在 範本中,但`id`不限制 為可轉換為整數的值。</span><span class="sxs-lookup"><span data-stu-id="664e5-310">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="664e5-311">到`/api/test2/int2/abc`的 GET 要求:</span><span class="sxs-lookup"><span data-stu-id="664e5-311">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="664e5-312">匹配此路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-312">Matches this route.</span></span>
  * <span data-ttu-id="664e5-313">模型綁定無法轉換為`abc`整數。</span><span class="sxs-lookup"><span data-stu-id="664e5-313">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="664e5-314">該方法`id`的參數為整數。</span><span class="sxs-lookup"><span data-stu-id="664e5-314">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="664e5-315">傳[回 400 錯誤請求](https://developer.mozilla.org/docs/Web/HTTP/Status/400),因為`abc`模型綁定無法 轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="664e5-315">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="664e5-316">屬性路由可以使用<xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute>屬性,<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute><xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>如<xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>。 與 。</span><span class="sxs-lookup"><span data-stu-id="664e5-316">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="664e5-317">所有[HTTP 謂詞](#verb)屬性都接受路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-317">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="664e5-318">下面的範例顯示了與同一路由樣本符合的兩個操作:</span><span class="sxs-lookup"><span data-stu-id="664e5-318">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="664e5-319">使用網`/products3`址路徑 :</span><span class="sxs-lookup"><span data-stu-id="664e5-319">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="664e5-320">當`MyProductsController.ListProducts` [HTTP 謂詞](#verb)為`GET`時,操作將運行。</span><span class="sxs-lookup"><span data-stu-id="664e5-320">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="664e5-321">當`MyProductsController.CreateProduct` [HTTP 謂詞](#verb)為`POST`時,操作將運行。</span><span class="sxs-lookup"><span data-stu-id="664e5-321">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="664e5-322">構建 REST API 時,很少需要在操作`[Route(...)]`方法上使用 ,因為操作接受所有 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-322">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="664e5-323">最好使用更具體的[HTTP 動詞屬性](#verb)來精確瞭解 API 支援的內容。</span><span class="sxs-lookup"><span data-stu-id="664e5-323">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="664e5-324">REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="664e5-324">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="664e5-325">REST API 應使用屬性路由將應用的功能建模為一組資源,其中操作由 HTTP 謂詞表示。</span><span class="sxs-lookup"><span data-stu-id="664e5-325">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="664e5-326">這意味著,同一邏輯資源上的許多操作(例如,GET 和 POST)使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-326">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="664e5-327">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="664e5-327">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="664e5-328">由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-328">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="664e5-329">在以下範例中,`id`作為網址路徑的一部分是必要的:</span><span class="sxs-lookup"><span data-stu-id="664e5-329">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="664e5-330">操作`Products2ApiController.GetProduct(int)`:</span><span class="sxs-lookup"><span data-stu-id="664e5-330">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="664e5-331">使用 URL 路徑執行,如`/products2/3`</span><span class="sxs-lookup"><span data-stu-id="664e5-331">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="664e5-332">不使用 URL`/products2`路徑執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-332">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="664e5-333">[[消耗]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)屬性允許操作限制受支援的請求內容類型。</span><span class="sxs-lookup"><span data-stu-id="664e5-333">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="664e5-334">有關詳細資訊,請參閱使用[使用 屬性定義受支援的要求內容類型](xref:web-api/index#consumes)。</span><span class="sxs-lookup"><span data-stu-id="664e5-334">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="664e5-335">如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-335">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="664e5-336">有關 的詳細`[ApiController]`資訊 ,請參閱[ApiController 屬性](xref:web-api/index##apicontroller-attribute)。</span><span class="sxs-lookup"><span data-stu-id="664e5-336">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="664e5-337">路由名稱</span><span class="sxs-lookup"><span data-stu-id="664e5-337">Route name</span></span>

<span data-ttu-id="664e5-338">下列程式碼會定義  的「路由名稱」`Products_List`：</span><span class="sxs-lookup"><span data-stu-id="664e5-338">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="664e5-339">您可以使用路由名稱根據特定路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-339">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="664e5-340">路由名稱:</span><span class="sxs-lookup"><span data-stu-id="664e5-340">Route names:</span></span>

* <span data-ttu-id="664e5-341">對路由的 URL 匹配行為沒有影響。</span><span class="sxs-lookup"><span data-stu-id="664e5-341">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="664e5-342">僅用於 URL 生成。</span><span class="sxs-lookup"><span data-stu-id="664e5-342">Are only used for URL generation.</span></span>

<span data-ttu-id="664e5-343">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="664e5-343">Route names must be unique application-wide.</span></span>

<span data-ttu-id="664e5-344">將前面的代碼與傳統預設路由進行對比,後者將`id`參數定義為可選`{id?}`( 。</span><span class="sxs-lookup"><span data-stu-id="664e5-344">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="664e5-345">精確指定 API 的能力具有優勢,`/products`例如`/products/5`允許 並發送到不同的操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-345">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="664e5-346">組合屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-346">Combining attribute routes</span></span>

<span data-ttu-id="664e5-347">為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-347">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="664e5-348">控制器上定義的任何路由範本都會附加到動作上的路由範本之前。</span><span class="sxs-lookup"><span data-stu-id="664e5-348">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="664e5-349">將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-349">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="664e5-350">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="664e5-350">In the preceding example:</span></span>

* <span data-ttu-id="664e5-351">網址`/products`路徑可以符合`ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="664e5-351">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="664e5-352">網址`/products/5`路徑`ProductsApi.GetProduct(int)`可以符合 。</span><span class="sxs-lookup"><span data-stu-id="664e5-352">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="664e5-353">這兩個操作都僅與`GET`HTTP 匹配,因為`[HttpGet]`它們被標記為屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-353">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="664e5-354">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-354">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="664e5-355">下面的範例匹配一組類似於預設路由的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="664e5-355">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="664e5-356">下表解釋了前面代碼中`[Route]`的屬性:</span><span class="sxs-lookup"><span data-stu-id="664e5-356">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="664e5-357">屬性</span><span class="sxs-lookup"><span data-stu-id="664e5-357">Attribute</span></span>               | <span data-ttu-id="664e5-358">與`[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="664e5-358">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="664e5-359">定義式路線範本</span><span class="sxs-lookup"><span data-stu-id="664e5-359">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="664e5-360">是</span><span class="sxs-lookup"><span data-stu-id="664e5-360">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="664e5-361">是</span><span class="sxs-lookup"><span data-stu-id="664e5-361">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="664e5-362">**否**</span><span class="sxs-lookup"><span data-stu-id="664e5-362">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="664e5-363">是</span><span class="sxs-lookup"><span data-stu-id="664e5-363">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="664e5-364">屬性路由順序</span><span class="sxs-lookup"><span data-stu-id="664e5-364">Attribute route order</span></span>

<span data-ttu-id="664e5-365">路由產生樹並同時符合所有終結點:</span><span class="sxs-lookup"><span data-stu-id="664e5-365">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="664e5-366">路由條目的行像放置在理想的順序中一樣。</span><span class="sxs-lookup"><span data-stu-id="664e5-366">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="664e5-367">最具體的路由有機會在更常規的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-367">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="664e5-368">例如,這樣的`blog/search/{topic}`屬性路由比 屬性路由`blog/{*article}`(如 )更具體。</span><span class="sxs-lookup"><span data-stu-id="664e5-368">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="664e5-369">默認情況下`blog/search/{topic}`,路由具有更高的優先順序,因為它更具體。</span><span class="sxs-lookup"><span data-stu-id="664e5-369">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="664e5-370">使用[常規路由](#cr),開發人員負責按所需順序放置路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-370">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="664e5-371">屬性路由可以使用<xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>屬性 配置訂單。</span><span class="sxs-lookup"><span data-stu-id="664e5-371">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="664e5-372">提供的所有框架[路由屬性](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)都`Order`包括 。</span><span class="sxs-lookup"><span data-stu-id="664e5-372">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="664e5-373">路由會依 `Order` 屬性的遞增排序來處理。</span><span class="sxs-lookup"><span data-stu-id="664e5-373">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="664e5-374">預設順序為 `0`。</span><span class="sxs-lookup"><span data-stu-id="664e5-374">The default order is `0`.</span></span> <span data-ttu-id="664e5-375">使用`Order = -1`未設置訂單的路由之前使用運行設置路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-375">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="664e5-376">在預設路由排序`Order = 1`后使用運行設置路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-376">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="664e5-377">**避免**`Order`。</span><span class="sxs-lookup"><span data-stu-id="664e5-377">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="664e5-378">如果應用的 URL 空間需要顯式順序值才能正確路由,則用戶端也可能感到困惑。</span><span class="sxs-lookup"><span data-stu-id="664e5-378">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="664e5-379">通常,屬性路由選擇具有 URL 匹配的正確路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-379">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="664e5-380">如果用於 URL 生成的替代預設順序不起作用,則使用路由名稱作為重寫通常比應用`Order`屬性 更簡單。</span><span class="sxs-lookup"><span data-stu-id="664e5-380">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="664e5-381">請考慮以下兩個控制器,它們都定義了路由符合`/home`:</span><span class="sxs-lookup"><span data-stu-id="664e5-381">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="664e5-382">`/home`使用上述代碼要求將引發類似於以下內容的異常:</span><span class="sxs-lookup"><span data-stu-id="664e5-382">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="664e5-383">新增到`Order`其中一個路由屬性可解決歧義:</span><span class="sxs-lookup"><span data-stu-id="664e5-383">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="664e5-384">使用前面的代碼,`/home``HomeController.Index`運行 終結點。</span><span class="sxs-lookup"><span data-stu-id="664e5-384">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="664e5-385">要存取`MyDemoController.MyIndex``/home/MyIndex`要求 。</span><span class="sxs-lookup"><span data-stu-id="664e5-385">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="664e5-386">**注意**：</span><span class="sxs-lookup"><span data-stu-id="664e5-386">**Note**:</span></span>

* <span data-ttu-id="664e5-387">前面的代碼是一個範例或糟糕的路由設計。</span><span class="sxs-lookup"><span data-stu-id="664e5-387">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="664e5-388">它被用來說明屬性`Order`。</span><span class="sxs-lookup"><span data-stu-id="664e5-388">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="664e5-389">該`Order`屬性僅解決歧義,該範本無法匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-389">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="664e5-390">最好刪除`[Route("Home")]`範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-390">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="664e5-391">請參閱[Razor 頁面路由和應用約定:使用](xref:razor-pages/razor-pages-conventions#route-order)Razor 頁面獲取有關工藝路線順序的資訊的路由順序。</span><span class="sxs-lookup"><span data-stu-id="664e5-391">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="664e5-392">在某些情況下,HTTP 500 錯誤返回與不明確的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-392">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="664e5-393">使用[日誌記錄](xref:fundamentals/logging/index)查看導致`AmbiguousMatchException`的終結點。</span><span class="sxs-lookup"><span data-stu-id="664e5-393">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="664e5-394">路由範本中的權杖替換 [控制器]、[操作]、[區域]</span><span class="sxs-lookup"><span data-stu-id="664e5-394">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="664e5-395">為方便起見,屬性路由通過將令牌包裝在以下其中一個來支援保留路由參數的權杖替換:</span><span class="sxs-lookup"><span data-stu-id="664e5-395">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="664e5-396">方形大括號:`[]`</span><span class="sxs-lookup"><span data-stu-id="664e5-396">Square braces: `[]`</span></span>
* <span data-ttu-id="664e5-397">大括弧:`{}`</span><span class="sxs-lookup"><span data-stu-id="664e5-397">Curly braces: `{}`</span></span>

<span data-ttu-id="664e5-398">標記`[action]`,`[area]``[controller]`與取代為操作名稱、區域名稱與控制器名稱的值,這些值來自定義路由的操作:</span><span class="sxs-lookup"><span data-stu-id="664e5-398">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="664e5-399">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="664e5-399">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="664e5-400">比賽`/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="664e5-400">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="664e5-401">比賽`/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="664e5-401">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="664e5-402">語彙基元取代會在建立屬性路由的最後一個步驟發生。</span><span class="sxs-lookup"><span data-stu-id="664e5-402">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="664e5-403">前面的範例的行為與以下代碼相同:</span><span class="sxs-lookup"><span data-stu-id="664e5-403">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="664e5-404">屬性路由也可以透過繼承來合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-404">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="664e5-405">這是強大的結合權杖更換。</span><span class="sxs-lookup"><span data-stu-id="664e5-405">This is powerful combined with token replacement.</span></span> <span data-ttu-id="664e5-406">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-406">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="664e5-407">`[Route("[controller]/[action]", Name="[controller]_[action]")]`為每個操作產生唯一的路由名稱:</span><span class="sxs-lookup"><span data-stu-id="664e5-407">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="664e5-408">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-408">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="664e5-409"> 會針對每個動作產生唯一的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-409">generates a unique route name for each action.</span></span>

<span data-ttu-id="664e5-410">若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="664e5-410">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="664e5-411">使用參數轉換程式自訂語彙基元取代</span><span class="sxs-lookup"><span data-stu-id="664e5-411">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="664e5-412">可以使用參數轉換程式自訂語彙基元取代。</span><span class="sxs-lookup"><span data-stu-id="664e5-412">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="664e5-413">參數轉換程式會實作 <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="664e5-413">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="664e5-414">例如,自訂`SlugifyParameterTransformer`參數轉換器`SubscriptionManagement`將 路由值`subscription-management`更改為 :</span><span class="sxs-lookup"><span data-stu-id="664e5-414">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="664e5-415"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> 是一個應用程式模型慣例，它會：</span><span class="sxs-lookup"><span data-stu-id="664e5-415">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="664e5-416">將參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-416">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="664e5-417">在取代屬性路由語彙基元值時自訂它們。</span><span class="sxs-lookup"><span data-stu-id="664e5-417">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="664e5-418">前面的`ListAll`方法`/subscription-management/list-all`符合 。</span><span class="sxs-lookup"><span data-stu-id="664e5-418">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="664e5-419">`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。</span><span class="sxs-lookup"><span data-stu-id="664e5-419">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="664e5-420">有關 Slug 的定義,請參閱[Slug 上的 MDN Web 文件](https://developer.mozilla.org/docs/Glossary/Slug)。</span><span class="sxs-lookup"><span data-stu-id="664e5-420">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

[!INCLUDE[](~/includes/regex.md)]
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="664e5-421">多屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-421">Multiple attribute routes</span></span>

<span data-ttu-id="664e5-422">屬性路由支援定義多個路由來達到相同的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-422">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="664e5-423">最常見的用法是模擬「預設慣例路由」的行為，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="664e5-423">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="664e5-424">在控制器上放置多個路由屬性意味著每個路由屬性都與操作方法上的每個路由屬性結合在一起:</span><span class="sxs-lookup"><span data-stu-id="664e5-424">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="664e5-425">所有[HTTP 謂詞](#verb)路由`IActionConstraint`約束實現 。</span><span class="sxs-lookup"><span data-stu-id="664e5-425">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="664e5-426">將實現<xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>實現的多個路由屬性放置在操作上時:</span><span class="sxs-lookup"><span data-stu-id="664e5-426">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="664e5-427">每個操作約束與應用於控制器的路由範本結合使用。</span><span class="sxs-lookup"><span data-stu-id="664e5-427">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="664e5-428">在操作上使用多個路由可能看起來有用且功能強大,最好保持應用的 URL 空間基本且定義良好。</span><span class="sxs-lookup"><span data-stu-id="664e5-428">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="664e5-429">僅在需要時**對**操作使用多個路由,例如,支援現有用戶端。</span><span class="sxs-lookup"><span data-stu-id="664e5-429">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="664e5-430">指定屬性路由的選擇性參數、預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="664e5-430">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="664e5-431">屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。</span><span class="sxs-lookup"><span data-stu-id="664e5-431">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="664e5-432">在前面的代碼中,`[HttpPost("product/{id:int}")]`應用路由約束。</span><span class="sxs-lookup"><span data-stu-id="664e5-432">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="664e5-433">操作`ProductsController.ShowProduct`僅由 URL`/product/3`路徑(如 )匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-433">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="664e5-434">工藝路線範本部分`{id:int}`僅將該段限制為整數。</span><span class="sxs-lookup"><span data-stu-id="664e5-434">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="664e5-435">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="664e5-435">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="664e5-436">使用 IRouteTemplate 提供者的自訂路由屬性</span><span class="sxs-lookup"><span data-stu-id="664e5-436">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="664e5-437">所有[路由屬性](#rt)。<xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider></span><span class="sxs-lookup"><span data-stu-id="664e5-437">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="664e5-438">ASP.NET核心執行時:</span><span class="sxs-lookup"><span data-stu-id="664e5-438">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="664e5-439">在應用啟動時查找控制器類和操作方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-439">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="664e5-440">使用實現`IRouteTemplateProvider`的屬性生成初始路由集。</span><span class="sxs-lookup"><span data-stu-id="664e5-440">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="664e5-441">實現`IRouteTemplateProvider`定義自定義路由屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-441">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="664e5-442">每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-442">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="664e5-443">前面的`Get`方法`Order = 2, Template = api/MyTestApi`傳回 。</span><span class="sxs-lookup"><span data-stu-id="664e5-443">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="664e5-444">使用應用程式模型自訂屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-444">Use application model to customize attribute routes</span></span>

<span data-ttu-id="664e5-445">應用程式模型:</span><span class="sxs-lookup"><span data-stu-id="664e5-445">The application model:</span></span>

* <span data-ttu-id="664e5-446">是在啟動時創建的物件模型。</span><span class="sxs-lookup"><span data-stu-id="664e5-446">Is an object model created at startup.</span></span>
* <span data-ttu-id="664e5-447">包含ASP.NET核心用於路由和執行應用中操作的所有元數據。</span><span class="sxs-lookup"><span data-stu-id="664e5-447">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="664e5-448">應用程式模型包括從路由屬性收集的所有數據。</span><span class="sxs-lookup"><span data-stu-id="664e5-448">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="664e5-449">路由屬性中的數據由`IRouteTemplateProvider`實現提供。</span><span class="sxs-lookup"><span data-stu-id="664e5-449">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="664e5-450">約定:</span><span class="sxs-lookup"><span data-stu-id="664e5-450">Conventions:</span></span>

* <span data-ttu-id="664e5-451">可以編寫以修改應用程式模型以自定義路由的行為。</span><span class="sxs-lookup"><span data-stu-id="664e5-451">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="664e5-452">在應用啟動時讀取。</span><span class="sxs-lookup"><span data-stu-id="664e5-452">Are read at app startup.</span></span>

<span data-ttu-id="664e5-453">本節顯示了使用應用程式模型自定義路由的基本示例。</span><span class="sxs-lookup"><span data-stu-id="664e5-453">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="664e5-454">以下代碼使路由大致與專案的文件結構一致。</span><span class="sxs-lookup"><span data-stu-id="664e5-454">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="664e5-455">以下代碼可防止將`namespace`約定應用於屬性路由的控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-455">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="664e5-456">例如,以下控制器不使用`NamespaceRoutingConvention`:</span><span class="sxs-lookup"><span data-stu-id="664e5-456">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="664e5-457">`NamespaceRoutingConvention.Apply` 方法：</span><span class="sxs-lookup"><span data-stu-id="664e5-457">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="664e5-458">如果控制器是屬性路由的,則不執行任何操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-458">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="664e5-459">根據設定控制器樣本,`namespace`刪除底座。 `namespace`</span><span class="sxs-lookup"><span data-stu-id="664e5-459">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="664e5-460">`NamespaceRoutingConvention`可套用`Startup.ConfigureServices`於 :</span><span class="sxs-lookup"><span data-stu-id="664e5-460">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="664e5-461">例如,請考慮以下控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-461">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="664e5-462">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="664e5-462">In the preceding code:</span></span>

* <span data-ttu-id="664e5-463">基地`namespace``My.Application`為 。</span><span class="sxs-lookup"><span data-stu-id="664e5-463">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="664e5-464">前面的控制器的全名是`My.Application.Admin.Controllers.UsersController`。</span><span class="sxs-lookup"><span data-stu-id="664e5-464">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="664e5-465">將`NamespaceRoutingConvention`控制器樣本設定到`Admin/Controllers/Users/[action]/{id?`。</span><span class="sxs-lookup"><span data-stu-id="664e5-465">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="664e5-466">`NamespaceRoutingConvention`也可以作為屬性應用於控制器:</span><span class="sxs-lookup"><span data-stu-id="664e5-466">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="664e5-467">混合路由：屬性路由與慣例路由</span><span class="sxs-lookup"><span data-stu-id="664e5-467">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="664e5-468">ASP.NET核心應用可以混合使用傳統的路由和屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-468">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="664e5-469">控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。</span><span class="sxs-lookup"><span data-stu-id="664e5-469">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="664e5-470">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-470">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="664e5-471">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-471">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="664e5-472">定義屬性路由的動作無法透過慣例路由到達，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="664e5-472">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="664e5-473">控制器上**的任何**路由**屬性使控制器**屬性中的所有操作都路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-473">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="664e5-474">屬性路由和傳統路由使用相同的路由引擎。</span><span class="sxs-lookup"><span data-stu-id="664e5-474">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="664e5-475">網址產生與環境值</span><span class="sxs-lookup"><span data-stu-id="664e5-475">URL Generation and ambient values</span></span>

<span data-ttu-id="664e5-476">應用可以使用路由 URL 生成功能生成指向操作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="664e5-476">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="664e5-477">生成 URL 消除了硬編碼 URL,使代碼更加可靠且可維護。</span><span class="sxs-lookup"><span data-stu-id="664e5-477">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="664e5-478">本節重點介紹 MVC 提供的 URL 生成功能,僅介紹 URL 生成工作原理的基礎知識。</span><span class="sxs-lookup"><span data-stu-id="664e5-478">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="664e5-479">如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-479">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="664e5-480">該<xref:Microsoft.AspNetCore.Mvc.IUrlHelper>介面是 MVC 和 URL 生成路由之間的基礎結構的基礎元素。</span><span class="sxs-lookup"><span data-stu-id="664e5-480">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="664e5-481">中的`IUrlHelper`實例可通過控制器、檢視和`Url`檢視元件中的屬性提供。</span><span class="sxs-lookup"><span data-stu-id="664e5-481">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="664e5-482">在下面的範例中,`IUrlHelper`介面`Controller.Url`透過屬性生成另一個操作的網址。</span><span class="sxs-lookup"><span data-stu-id="664e5-482">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="664e5-483">如果使用預設的一般路由,`url`則變數的值是網址 路徑字`/UrlGeneration/Destination`串 。</span><span class="sxs-lookup"><span data-stu-id="664e5-483">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="664e5-484">此網址路徑是透過合併路由建立的:</span><span class="sxs-lookup"><span data-stu-id="664e5-484">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="664e5-485">目前請求的路由值,稱為**環境值**。</span><span class="sxs-lookup"><span data-stu-id="664e5-485">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="664e5-486">傳遞給`Url.Action`這些值並將這些值取代到製程的範本中的值:</span><span class="sxs-lookup"><span data-stu-id="664e5-486">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="664e5-487">路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。</span><span class="sxs-lookup"><span data-stu-id="664e5-487">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="664e5-488">沒有值的路由參數可以:</span><span class="sxs-lookup"><span data-stu-id="664e5-488">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="664e5-489">如果預設值有預設值,請使用預設值。</span><span class="sxs-lookup"><span data-stu-id="664e5-489">Use a default value if it has one.</span></span>
* <span data-ttu-id="664e5-490">如果可選,則跳過。</span><span class="sxs-lookup"><span data-stu-id="664e5-490">Be skipped if it's optional.</span></span> <span data-ttu-id="664e5-491">例如,`id`從路由樣本`{controller}/{action}/{id?}`。</span><span class="sxs-lookup"><span data-stu-id="664e5-491">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="664e5-492">如果任何必需的路由參數沒有相應的值,則 URL 生成將失敗。</span><span class="sxs-lookup"><span data-stu-id="664e5-492">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="664e5-493">如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。</span><span class="sxs-lookup"><span data-stu-id="664e5-493">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="664e5-494">前面的假設`Url.Action`[路由](#cr)示例。</span><span class="sxs-lookup"><span data-stu-id="664e5-494">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="664e5-495">URL 生成與[屬性路由](#ar)類似,儘管概念不同。</span><span class="sxs-lookup"><span data-stu-id="664e5-495">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="664e5-496">使用傳統路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-496">With conventional routing:</span></span>

* <span data-ttu-id="664e5-497">工藝路線值用於展開範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-497">The route values are used to expand a template.</span></span>
* <span data-ttu-id="664e5-498">和`action`通常`controller`出現在 該範本中的路由值。</span><span class="sxs-lookup"><span data-stu-id="664e5-498">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="664e5-499">這之所以有效,是因為路由匹配的 URL 符合約定。</span><span class="sxs-lookup"><span data-stu-id="664e5-499">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="664e5-500">以下範例使用屬性路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-500">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="664e5-501">前面的`Source`程式碼中的操作會`custom/url/to/destination`產生 。</span><span class="sxs-lookup"><span data-stu-id="664e5-501">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="664e5-502"><xref:Microsoft.AspNetCore.Routing.LinkGenerator>在 ASP.NET 酷 3.0 中添加`IUrlHelper`,作為的替代方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-502"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="664e5-503">`LinkGenerator`提供類似但更靈活的功能。</span><span class="sxs-lookup"><span data-stu-id="664e5-503">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="664e5-504">上`IUrlHelper`的每種方法都有相應的方法`LinkGenerator`系列。</span><span class="sxs-lookup"><span data-stu-id="664e5-504">Each method on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="664e5-505">由動作名稱產生 URL</span><span class="sxs-lookup"><span data-stu-id="664e5-505">Generating URLs by action name</span></span>

<span data-ttu-id="664e5-506">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) [Url.action,LinkGenerator.GetPathByAction,](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)以及所有相關重載都旨在通過指定控制器名稱和操作名稱來生成目標終結點。</span><span class="sxs-lookup"><span data-stu-id="664e5-506">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="664e5-507">使用`Url.Action`時,目前的目前路由`controller``action`值 由執行時提供:</span><span class="sxs-lookup"><span data-stu-id="664e5-507">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="664e5-508">的值`controller``action`是[環境值和值](#ambient)的一部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-508">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="664e5-509">該方法`Url.Action`始終使用`action``controller`和的當前值,並生成路由到當前操作的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="664e5-509">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="664e5-510">路由嘗試使用環境值中的值來填充生成 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="664e5-510">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="664e5-511">考慮環境值`{ a = Alice, b = Bob, c = Carol, d = David }``{a}/{b}/{c}/{d}`這樣的 路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-511">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="664e5-512">路由有足夠的資訊來生成 URL,而不需要任何其他值。</span><span class="sxs-lookup"><span data-stu-id="664e5-512">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="664e5-513">路由具有足夠的資訊,因為所有路由參數都有一個值。</span><span class="sxs-lookup"><span data-stu-id="664e5-513">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="664e5-514">如果新增該`{ d = Donovan }`值:</span><span class="sxs-lookup"><span data-stu-id="664e5-514">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="664e5-515">該值`{ d = David }`將被忽略。</span><span class="sxs-lookup"><span data-stu-id="664e5-515">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="664e5-516">產生的網址`Alice/Bob/Carol/Donovan`路徑為 。</span><span class="sxs-lookup"><span data-stu-id="664e5-516">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="664e5-517">**警告**: 網址路徑是分層的。</span><span class="sxs-lookup"><span data-stu-id="664e5-517">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="664e5-518">在前面的範例中, 如果新增該`{ c = Cheryl }`值 :</span><span class="sxs-lookup"><span data-stu-id="664e5-518">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="664e5-519">將忽略這兩`{ c = Carol, d = David }`個值。</span><span class="sxs-lookup"><span data-stu-id="664e5-519">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="664e5-520">不再有值`d`,URL 產生失敗。</span><span class="sxs-lookup"><span data-stu-id="664e5-520">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="664e5-521">必須指定的`c``d`所需值才能生成 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-521">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="664e5-522">您可能期望使用預設路由`{controller}/{action}/{id?}`遇到此問題。</span><span class="sxs-lookup"><span data-stu-id="664e5-522">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="664e5-523">此問題在實踐中很少見,因為`Url.Action`始終顯式指定`controller``action`和 值。</span><span class="sxs-lookup"><span data-stu-id="664e5-523">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="664e5-524">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)的多個重載採用路由值對象來為 路由`controller``action`參數提供 和 以外的值。</span><span class="sxs-lookup"><span data-stu-id="664e5-524">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="664e5-525">路由值物件經常與 一起`id`使用 。</span><span class="sxs-lookup"><span data-stu-id="664e5-525">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="664e5-526">例如： `Url.Action("Buy", "Products", new { id = 17 })` 。</span><span class="sxs-lookup"><span data-stu-id="664e5-526">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="664e5-527">路由值物件:</span><span class="sxs-lookup"><span data-stu-id="664e5-527">The route values object:</span></span>

* <span data-ttu-id="664e5-528">根據約定,通常是匿名類型的物件。</span><span class="sxs-lookup"><span data-stu-id="664e5-528">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="664e5-529">可以是 POCO`IDictionary<>`或[POCO。](https://wikipedia.org/wiki/Plain_old_CLR_object)</span><span class="sxs-lookup"><span data-stu-id="664e5-529">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="664e5-530">不符合路由參數的任何額外路由值都會放在查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="664e5-530">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="664e5-531">前面的代碼產生`/Products/Buy/17?color=red`。</span><span class="sxs-lookup"><span data-stu-id="664e5-531">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="664e5-532">以下代碼產生絕對網址:</span><span class="sxs-lookup"><span data-stu-id="664e5-532">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="664e5-533">要建立絕對網址,請使用以下網址之一:</span><span class="sxs-lookup"><span data-stu-id="664e5-533">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="664e5-534">接受的`protocol`重載。</span><span class="sxs-lookup"><span data-stu-id="664e5-534">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="664e5-535">例如,前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="664e5-535">For example, the preceding code.</span></span>
* <span data-ttu-id="664e5-536">[連結生成器.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*),默認情況下生成絕對URI。</span><span class="sxs-lookup"><span data-stu-id="664e5-536">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="664e5-537">依路由產生網址</span><span class="sxs-lookup"><span data-stu-id="664e5-537">Generate URLs by route</span></span>

<span data-ttu-id="664e5-538">前面的代碼演示通過傳入控制器和操作名稱來生成 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-538">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="664e5-539">`IUrlHelper`還提供[Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*)方法系列。</span><span class="sxs-lookup"><span data-stu-id="664e5-539">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="664e5-540">這些方法類似於[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*),但它們不會`action``controller`將和的當前值複製到路由值。</span><span class="sxs-lookup"><span data-stu-id="664e5-540">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="664e5-541">最常見的用法`Url.RouteUrl`:</span><span class="sxs-lookup"><span data-stu-id="664e5-541">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="664e5-542">指定要生成 URL 的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-542">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="664e5-543">通常不指定控制器或操作名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-543">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="664e5-544">以下 Razor 檔案產生`Destination_Route`指向的 HTML 連結:</span><span class="sxs-lookup"><span data-stu-id="664e5-544">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="664e5-545">在 HTML 與 Razor 中建立網址</span><span class="sxs-lookup"><span data-stu-id="664e5-545">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="664e5-546"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper>提供了分別<xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper>`<form>`生成`<a>`[的方法 Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*)和[Html.ActionLink。](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*)</span><span class="sxs-lookup"><span data-stu-id="664e5-546"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="664e5-547">這些方法使用[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*)方法生成 URL,並且它們接受類似的參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-547">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="664e5-548">`HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。</span><span class="sxs-lookup"><span data-stu-id="664e5-548">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="664e5-549">TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-549">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="664e5-550">這兩者使用 `IUrlHelper` 進行實作。</span><span class="sxs-lookup"><span data-stu-id="664e5-550">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="664e5-551">有關詳細資訊[,請參閱表單中標記幫助人員](xref:mvc/views/working-with-forms)。</span><span class="sxs-lookup"><span data-stu-id="664e5-551">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="664e5-552">在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-552">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="664e5-553">操作結果中的網址產生</span><span class="sxs-lookup"><span data-stu-id="664e5-553">URL generation in Action Results</span></span>

<span data-ttu-id="664e5-554">前面的示例顯示了`IUrlHelper`在控制器中使用。</span><span class="sxs-lookup"><span data-stu-id="664e5-554">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="664e5-555">控制器中最常見的用法是生成 URL 作為操作結果的一部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-555">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="664e5-556"><xref:Microsoft.AspNetCore.Mvc.ControllerBase> 和 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別提供便利的方法讓動作結果可參考其他動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-556">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="664e5-557">一個典型的用法是在接受使用者輸入后重定向:</span><span class="sxs-lookup"><span data-stu-id="664e5-557">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="664e5-558">操作結果工廠方法,如<xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>和遵循與`IUrlHelper`上 的方法類似的模式。</span><span class="sxs-lookup"><span data-stu-id="664e5-558">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="664e5-559">專用慣例路由的特殊案例</span><span class="sxs-lookup"><span data-stu-id="664e5-559">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="664e5-560">[常規路由](#cr)可以使用一種稱為[專用常規路由](#dcr)的特殊路由定義。</span><span class="sxs-lookup"><span data-stu-id="664e5-560">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="664e5-561">在下面的範例中,命名的`blog`路由是專用的常規路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-561">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="664e5-562">使用前面的路由定義,`Url.Action("Index", "Home")``/``default`使用路由產生 URL 路徑,但為什麼?</span><span class="sxs-lookup"><span data-stu-id="664e5-562">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="664e5-563">您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="664e5-563">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="664e5-564">[專用常規路由](#dcr)依賴於預設值的特殊行為,這些預設值沒有相應的路由參數,以防止路由在 URL 產生時過於[貪婪](xref:fundamentals/routing#greedy)。</span><span class="sxs-lookup"><span data-stu-id="664e5-564">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="664e5-565">在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-565">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="664e5-566">當路由執行 URL 產生時，所提供的值必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="664e5-566">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="664e5-567">使用`blog`網址產生失敗`{ controller = Home, action = Index }`, 因為值`{ controller = Blog, action = Article }`不符合 。</span><span class="sxs-lookup"><span data-stu-id="664e5-567">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="664e5-568">路由會接著切換並嘗試 `default`，此時會成功。</span><span class="sxs-lookup"><span data-stu-id="664e5-568">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="664e5-569">區域</span><span class="sxs-lookup"><span data-stu-id="664e5-569">Areas</span></span>

<span data-ttu-id="664e5-570">[區域](xref:mvc/controllers/areas)是 MVC 功能,用於將相關功能組織到群組中作為單獨的功能:</span><span class="sxs-lookup"><span data-stu-id="664e5-570">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="664e5-571">為控制器操作路由命名空間。</span><span class="sxs-lookup"><span data-stu-id="664e5-571">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="664e5-572">視圖的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="664e5-572">Folder structure for views.</span></span>

<span data-ttu-id="664e5-573">使用區域允許應用具有多個具有相同名稱的控制器,只要它們具有不同的區域。</span><span class="sxs-lookup"><span data-stu-id="664e5-573">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="664e5-574">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="664e5-574">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="664e5-575">本節討論路由如何與區域交互。</span><span class="sxs-lookup"><span data-stu-id="664e5-575">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="664e5-576">有關如何使用檢視區域的詳細資訊,請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="664e5-576">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="664e5-577">下面的範例將 MVC 設定為使用預設常規路`area``area`由與`Blog`命名路由 的路由:</span><span class="sxs-lookup"><span data-stu-id="664e5-577">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="664e5-578">在前面的代碼中,<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>呼叫以建立`"blog_route"`。</span><span class="sxs-lookup"><span data-stu-id="664e5-578">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="664e5-579">第二個`"Blog"`參數 , 是區域名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-579">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="664e5-580">符合網址`/Manage/Users/AddUser`路徑`"blog_route"`( 如 )時`{ area = Blog, controller = Users, action = AddUser }`,路由將生成路由值。</span><span class="sxs-lookup"><span data-stu-id="664e5-580">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="664e5-581">路由`area`值由`area`的 預設值生成。</span><span class="sxs-lookup"><span data-stu-id="664e5-581">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="664e5-582">由 建立`MapAreaControllerRoute`的 路由等效於以下內容:</span><span class="sxs-lookup"><span data-stu-id="664e5-582">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="664e5-583">`MapAreaControllerRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-583">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="664e5-584">預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-584">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="664e5-585">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="664e5-585">Conventional routing is order-dependent.</span></span> <span data-ttu-id="664e5-586">通常,具有區域的路由應更早放置,因為它們比沒有區域的路由更具體。</span><span class="sxs-lookup"><span data-stu-id="664e5-586">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="664e5-587">使用前面的範例,路由值`{ area = Blog, controller = Users, action = AddUser }`比以下操作:</span><span class="sxs-lookup"><span data-stu-id="664e5-587">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="664e5-588">[[區域]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute)屬性表示控制器是區域的一部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-588">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="664e5-589">此控制器位於該區域`Blog`。</span><span class="sxs-lookup"><span data-stu-id="664e5-589">This controller is in the `Blog` area.</span></span> <span data-ttu-id="664e5-590">沒有屬性的`[Area]`控制器不是任何區域的成員,並且**與**路由值由路由提供時`area`不匹配。</span><span class="sxs-lookup"><span data-stu-id="664e5-590">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="664e5-591">在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-591">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="664e5-592">此處顯示每個控制器的命名空間,以便完整性。</span><span class="sxs-lookup"><span data-stu-id="664e5-592">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="664e5-593">如果前面的控制器使用相同的命名空間,則將生成編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="664e5-593">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="664e5-594">類別命名空間不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-594">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="664e5-595">前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-595">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="664e5-596">第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-596">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="664e5-597">在「未符合任何值」\*\* 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="664e5-597">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="664e5-598">在區域內執行作業時,for`area`的路由值可作為用於網址產生的路線[的環境值](#ambient)。</span><span class="sxs-lookup"><span data-stu-id="664e5-598">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="664e5-599">這表示區域預設會以「黏性」\*\* 方式來處理 URL 產生，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="664e5-599">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="664e5-600">以下代碼產生到的`/Zebra/Users/AddUser`網址:</span><span class="sxs-lookup"><span data-stu-id="664e5-600">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="664e5-601">操作定義</span><span class="sxs-lookup"><span data-stu-id="664e5-601">Action definition</span></span>

<span data-ttu-id="664e5-602">控制器上的公共方法(具有[非操作](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute)屬性的這些方法除外)是操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-602">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="664e5-603">範例程式碼</span><span class="sxs-lookup"><span data-stu-id="664e5-603">Sample code</span></span>

 * <span data-ttu-id="664e5-604">[MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs)方法包含在[示例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)中,用於顯示路由資訊。</span><span class="sxs-lookup"><span data-stu-id="664e5-604">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="664e5-605">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="664e5-605">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="664e5-606">ASP.NET Core MVC 使用路由[中介軟體](xref:fundamentals/middleware/index)來比對內送要求的 URL，並將這些 URL 對應至動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-606">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="664e5-607">路由是在啟動程式碼或屬性中定義。</span><span class="sxs-lookup"><span data-stu-id="664e5-607">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="664e5-608">路由描述 URL 路徑應該如何與動作進行比對。</span><span class="sxs-lookup"><span data-stu-id="664e5-608">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="664e5-609">路由也可用來產生回應中所送出的連結 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-609">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="664e5-610">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-610">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="664e5-611">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-611">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="664e5-612">如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。</span><span class="sxs-lookup"><span data-stu-id="664e5-612">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="664e5-613">本文件將說明 MVC 與路由之間的互動，並說明一般 MVC 應用程式如何使用路由功能。</span><span class="sxs-lookup"><span data-stu-id="664e5-613">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="664e5-614">如需進階路由的詳細資料，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-614">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="664e5-615">設定路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="664e5-615">Setting up Routing Middleware</span></span>

<span data-ttu-id="664e5-616">在 *Configure* 方法中，您可能會看到類似如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="664e5-616">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="664e5-617">在 `UseMvc` 呼叫內，`MapRoute` 是用來建立單一路由，稱為 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-617">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="664e5-618">大多數 MVC 應用程式會使用具有類似於 `default` 路由之範本的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-618">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="664e5-619">路由範本 `"{controller=Home}/{action=Index}/{id?}"` 可能符合某個 URL 路徑 (例如 `/Products/Details/5`)，並將透過 Token 化路徑來擷取路由值 `{ controller = Products, action = Details, id = 5 }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-619">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="664e5-620">MVC 會嘗試尋找名為 `ProductsController` 的控制器，並執行動作 `Details`：</span><span class="sxs-lookup"><span data-stu-id="664e5-620">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="664e5-621">請注意，在此範例中，模型繫結會在叫用此動作時，使用 `id = 5` 值將 `id` 參數設定為 `5`。</span><span class="sxs-lookup"><span data-stu-id="664e5-621">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="664e5-622">如需詳細資料，請參閱[模型繫結](../models/model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="664e5-622">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="664e5-623">使用 `default` 路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-623">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="664e5-624">路由範本：</span><span class="sxs-lookup"><span data-stu-id="664e5-624">The route template:</span></span>

* <span data-ttu-id="664e5-625">`{controller=Home}` 將 `Home` 定義為預設的 `controller`</span><span class="sxs-lookup"><span data-stu-id="664e5-625">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="664e5-626">`{action=Index}` 將 `Index` 定義為預設的 `action`</span><span class="sxs-lookup"><span data-stu-id="664e5-626">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="664e5-627">`{id?}` 將 `id` 定義為選擇性</span><span class="sxs-lookup"><span data-stu-id="664e5-627">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="664e5-628">預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。</span><span class="sxs-lookup"><span data-stu-id="664e5-628">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="664e5-629">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="664e5-629">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="664e5-630">`"{controller=Home}/{action=Index}/{id?}"` 可能符合 URL 路徑 `/`，並將產生路由值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-630">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="664e5-631">`controller` 和 `action` 的值使用預設值；`id` 不會產生值，因為 URL 路徑中沒有對應的區段。</span><span class="sxs-lookup"><span data-stu-id="664e5-631">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="664e5-632">MVC 會使用這些路由值來選取 `HomeController` 和 `Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="664e5-632">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="664e5-633">使用此控制器定義和路由範本，就會對下列任何 URL 路徑執行 `HomeController.Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="664e5-633">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="664e5-634">`UseMvcWithDefaultRoute` 方法很方便：</span><span class="sxs-lookup"><span data-stu-id="664e5-634">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="664e5-635">可用來取代：</span><span class="sxs-lookup"><span data-stu-id="664e5-635">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="664e5-636">`UseMvc` 和 `UseMvcWithDefaultRoute` 會將 `RouterMiddleware` 的執行個體新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="664e5-636">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="664e5-637">MVC 不會直接與中介軟體互動，而是使用路由來處理要求。</span><span class="sxs-lookup"><span data-stu-id="664e5-637">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="664e5-638">MVC 會透過 `MvcRouteHandler` 的執行個體連線到路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-638">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="664e5-639">`UseMvc` 內的程式碼類似如下：</span><span class="sxs-lookup"><span data-stu-id="664e5-639">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="664e5-640">`UseMvc` 不會直接定義任何路由，而是將預留位置新增至 `attribute` 路由的路由集合。</span><span class="sxs-lookup"><span data-stu-id="664e5-640">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="664e5-641">多載 `UseMvc(Action<IRouteBuilder>)` 可讓您新增自己的路由，同時支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-641">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="664e5-642">`UseMvc`其所有變體都為屬性路由新增佔位元 ─屬性路由始終可用,無論您如何`UseMvc`設定 。</span><span class="sxs-lookup"><span data-stu-id="664e5-642">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="664e5-643">`UseMvcWithDefaultRoute` 會定義預設路由並支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-643">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="664e5-644">[屬性路由](#attribute-routing-ref-label)一節包含屬性路由的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="664e5-644">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="664e5-645">慣例路由</span><span class="sxs-lookup"><span data-stu-id="664e5-645">Conventional routing</span></span>

<span data-ttu-id="664e5-646">`default` 路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-646">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="664e5-647">前面的代碼是常規路由的範例。</span><span class="sxs-lookup"><span data-stu-id="664e5-647">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="664e5-648">此樣式稱為傳統路由,因為它為網址路徑建立了*約定*:</span><span class="sxs-lookup"><span data-stu-id="664e5-648">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="664e5-649">第一個路徑段映射到控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-649">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="664e5-650">第二個映射到操作名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-650">The second maps to the action name.</span></span>
* <span data-ttu-id="664e5-651">第三個段用於選擇`id`。</span><span class="sxs-lookup"><span data-stu-id="664e5-651">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="664e5-652">`id`映射到模型實體。</span><span class="sxs-lookup"><span data-stu-id="664e5-652">`id` maps to a model entity.</span></span>

<span data-ttu-id="664e5-653">使用此 `default` 路由，URL 路徑 `/Products/List` 會對應至 `ProductsController.List` 動作；而 `/Blog/Article/17` 會對應至 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="664e5-653">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="664e5-654">此對應**只會**根據控制器和動作名稱，而不會根據命名空間、來源檔案位置或方法參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-654">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="664e5-655">慣例路由與預設路由的搭配使用可讓您快速建置應用程式，而不需要針對每個定義的動作產生新的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="664e5-655">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="664e5-656">針對具有 CRUD 樣式動作的應用程式，在控制器之間保持一致的 URL 有助於簡化程式碼，並讓 UI 更容易預測。</span><span class="sxs-lookup"><span data-stu-id="664e5-656">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="664e5-657">路由範本將 `id` 定義為選擇性，也就是您的動作可以直接執行，而不需要將識別碼當作 URL 的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="664e5-657">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="664e5-658">如果省略 URL 中的 `id`，通常表示它會由模型繫結設定為 `0`，因此在符合 `id == 0` 的資料庫中找不到任何實體。</span><span class="sxs-lookup"><span data-stu-id="664e5-658">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="664e5-659">屬性路由可提供您更細微的控制，讓某些動作需要此識別碼，而其他動作則不需要。</span><span class="sxs-lookup"><span data-stu-id="664e5-659">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="664e5-660">依照慣例，本文件將會包含可能出現在正確使用中的選擇性參數 (例如 `id`)。</span><span class="sxs-lookup"><span data-stu-id="664e5-660">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="664e5-661">多個路由</span><span class="sxs-lookup"><span data-stu-id="664e5-661">Multiple routes</span></span>

<span data-ttu-id="664e5-662">您可以將更多呼叫新增至 `MapRoute`，以在 `UseMvc` 內新增多個路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-662">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="664e5-663">這樣做可讓您定義多個慣例，或新增特定動作專用的慣例路由，例如：</span><span class="sxs-lookup"><span data-stu-id="664e5-663">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="664e5-664">這裡的 `blog` 路由是「專用慣例路由」\*\*，也就是它會使用慣例路由系統，但專用於特定動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-664">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="664e5-665">由於 `controller` 和 `action` 並未作為參數出現在路由範本中，它們只能有預設值，因此此路由一律會對應至動作 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="664e5-665">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="664e5-666">路由集合中的路由已經過排序，並將依其新增順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="664e5-666">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="664e5-667">因此在此範例中，會先嘗試 `blog` 路由，再嘗試 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-667">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-668">*專用常規路由*通常使用**捕獲所有**路由`{*article}`參數,如捕獲 URL 路徑的剩餘部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-668">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="664e5-669">這可能會讓路由變得「太窮盡」，也就是它會比對您想要讓其他路由比對的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-669">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="664e5-670">將這些「窮盡」路由放在路由表後面可解決此問題。</span><span class="sxs-lookup"><span data-stu-id="664e5-670">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="664e5-671">後援</span><span class="sxs-lookup"><span data-stu-id="664e5-671">Fallback</span></span>

<span data-ttu-id="664e5-672">在要求處理過程中，MVC 會確認路由值是否可用來尋找應用程式中的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-672">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="664e5-673">如果路由值未符合任何動作，則不會將此路由視為一個相符項目，並將嘗試下一個路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-673">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="664e5-674">這稱為「遞補」\*\*，其目的在於簡化慣例路由重疊的情況。</span><span class="sxs-lookup"><span data-stu-id="664e5-674">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="664e5-675">釐清動作</span><span class="sxs-lookup"><span data-stu-id="664e5-675">Disambiguating actions</span></span>

<span data-ttu-id="664e5-676">若透過路由符合兩個動作，MVC 必須釐清並選擇「最佳」候選項目，否則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="664e5-676">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="664e5-677">例如：</span><span class="sxs-lookup"><span data-stu-id="664e5-677">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="664e5-678">此控制器定義兩個符合 URL 路徑 `/Products/Edit/17` 和路由資料 `{ controller = Products, action = Edit, id = 17 }` 的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-678">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="664e5-679">這是 MVC 控制器的典型模式，其中 `Edit(int)` 會顯示用以編輯產品的表單，而 `Edit(int, Product)` 會處理已張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="664e5-679">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="664e5-680">若要執行這項操作，MVC 必須在要求為 HTTP `POST` 時選擇 `Edit(int, Product)`，並在 HTTP 動詞命令為任何其他項目時選擇 `Edit(int)`。</span><span class="sxs-lookup"><span data-stu-id="664e5-680">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="664e5-681">`HttpPostAttribute` ( `[HttpPost]` ) 是 `IActionConstraint` 的實作，只有在 HTTP 動詞命令為 `POST` 時，才能選取此動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-681">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="664e5-682">`IActionConstraint` 的存在使 `Edit(int, Product)` 比 `Edit(int)`「更符合」，因此會先嘗試 `Edit(int, Product)`。</span><span class="sxs-lookup"><span data-stu-id="664e5-682">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="664e5-683">您只需要在特殊情況下撰寫自訂 `IActionConstraint` 實作，但請務必了解 `HttpPostAttribute` 等屬性的角色 (其他 HTTP 動詞命令會定義類似的屬性)。</span><span class="sxs-lookup"><span data-stu-id="664e5-683">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="664e5-684">在慣例路由中，當動作是 `show form -> submit form` 工作流程的一部分時，通常會使用相同的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-684">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="664e5-685">檢閱[了解 IActionConstraint](#understanding-iactionconstraint) 一節之後，會更清楚此模式的便利性。</span><span class="sxs-lookup"><span data-stu-id="664e5-685">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="664e5-686">如果多個路由相符，而且 MVC 找不到「最佳」路由，則會擲回 `AmbiguousActionException`。</span><span class="sxs-lookup"><span data-stu-id="664e5-686">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="664e5-687">路由名稱</span><span class="sxs-lookup"><span data-stu-id="664e5-687">Route names</span></span>

<span data-ttu-id="664e5-688">下列範例中的字串 `"blog"` 和 `"default"` 是路由名稱：</span><span class="sxs-lookup"><span data-stu-id="664e5-688">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="664e5-689">路由名稱為路由提供邏輯名稱，因此可以使用具名路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-689">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="664e5-690">當路由排序可能使 URL 產生作業變得複雜時，這樣做可大幅簡化 URL 產生作業。</span><span class="sxs-lookup"><span data-stu-id="664e5-690">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="664e5-691">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="664e5-691">Route names must be unique application-wide.</span></span>

<span data-ttu-id="664e5-692">路由名稱不會影響要求的 URL 比對或處理，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="664e5-692">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="664e5-693">[路由](xref:fundamentals/routing)提供 URL 產生的詳細資訊，包括 MVC 特定協助程式中的 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="664e5-693">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="664e5-694">屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-694">Attribute routing</span></span>

<span data-ttu-id="664e5-695">屬性路由使用一組屬性，將動作直接對應至路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-695">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="664e5-696">在下列範例中，在 `Configure` 方法中使用 `app.UseMvc();`，且未傳遞任何路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-696">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="664e5-697">`HomeController` 會比對一組 類似於預設路由 `{controller=Home}/{action=Index}/{id?}` 所比對的 URL：</span><span class="sxs-lookup"><span data-stu-id="664e5-697">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="664e5-698">`HomeController.Index()` 動作會針對 URL 路徑 `/`、`/Home` 或 `/Home/Index` 的任何一個來執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-698">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-699">此範例醒目提示屬性路由與慣例路由之間的主要程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="664e5-699">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="664e5-700">屬性路由需要更多輸入才能指定路由，而慣例預設路由處理路由的方式則更簡潔。</span><span class="sxs-lookup"><span data-stu-id="664e5-700">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="664e5-701">不過，屬性路由允許 (並需要) 精確地控制套用至每個動作的路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-701">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="664e5-702">使用屬性路由，選取動作時「不會」\*\*\*\* 根據控制器名稱和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-702">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="664e5-703">此範例會符合與上一個範例相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-703">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="664e5-704">上述路由範本未定義 `action`、`area` 和 `controller` 的路由參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-704">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="664e5-705">事實上，屬性路由中不允許有這些路由參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-705">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="664e5-706">由於路由範本已與一個動作建立關聯，因此剖析 URL 中的動作名稱並沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="664e5-706">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="664e5-707">使用 Http[Verb] 屬性的屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-707">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="664e5-708">屬性路由也可以使用 `Http[Verb]` 屬性，例如 `HttpPostAttribute`。</span><span class="sxs-lookup"><span data-stu-id="664e5-708">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="664e5-709">這些屬性都會接受路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-709">All of these attributes can accept a route template.</span></span> <span data-ttu-id="664e5-710">此範例顯示兩個符合相同路由範本的動作：</span><span class="sxs-lookup"><span data-stu-id="664e5-710">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="664e5-711">針對 `/products` 等 URL 路徑，當 HTTP 動詞命令為 `GET` 時，會執行 `ProductsApi.ListProducts`；當 HTTP 動詞命令為 `POST` 時，會執行 `ProductsApi.CreateProduct`。</span><span class="sxs-lookup"><span data-stu-id="664e5-711">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="664e5-712">屬性路由會先根據路由屬性所定義的一組路由範本來比對 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-712">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="664e5-713">一旦有路由範本相符，就會套用 `IActionConstraint` 條件約束以決定可執行的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-713">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="664e5-714">構建 REST API 時,`[Route(...)]`很少需要使用 操作方法,因為操作將接受所有 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-714">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="664e5-715">最好是使用更明確的 `Http*Verb*Attributes`，以精確地指定 API 的支援項目。</span><span class="sxs-lookup"><span data-stu-id="664e5-715">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="664e5-716">REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="664e5-716">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="664e5-717">由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-717">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="664e5-718">在此範例中，`id` 是 URL 路徑的必要部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-718">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="664e5-719">`ProductsApi.GetProduct(int)` 動作會針對 `/products/3` 等 URL 路徑來執行，但不會針對 `/products` 等 URL 路徑來執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-719">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="664e5-720">如需路由範本和相關選項的完整描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-720">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="664e5-721">路由名稱</span><span class="sxs-lookup"><span data-stu-id="664e5-721">Route Name</span></span>

<span data-ttu-id="664e5-722">下列程式碼會定義 `Products_List` 的「路由名稱」\*\*：</span><span class="sxs-lookup"><span data-stu-id="664e5-722">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="664e5-723">您可以使用路由名稱根據特定路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-723">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="664e5-724">路由名稱不會影響路由的 URL 比對行為，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="664e5-724">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="664e5-725">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="664e5-725">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-726">慣例「預設路由」\*\* 與此相反，只會將 `id` 參數定義為選擇性 (`{id?}`)。</span><span class="sxs-lookup"><span data-stu-id="664e5-726">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="664e5-727">能夠精確指定 API 有些優點，像是允許將 `/products` 和 `/products/5` 分派至不同的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-727">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="664e5-728">合併路由</span><span class="sxs-lookup"><span data-stu-id="664e5-728">Combining routes</span></span>

<span data-ttu-id="664e5-729">為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-729">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="664e5-730">控制器上定義的任何路由範本都會附加到動作上的路由範本之前。</span><span class="sxs-lookup"><span data-stu-id="664e5-730">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="664e5-731">將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-731">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="664e5-732">在此範例中，URL 路徑 `/products` 可能符合 `ProductsApi.ListProducts`，而 URL 路徑 `/products/5` 可能符合 `ProductsApi.GetProduct(int)`。</span><span class="sxs-lookup"><span data-stu-id="664e5-732">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="664e5-733">這兩個操作都僅與`GET`HTTP 符合,因為`HttpGetAttribute`它們被標記為 。</span><span class="sxs-lookup"><span data-stu-id="664e5-733">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="664e5-734">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-734">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="664e5-735">此範例會比對一組類似於「預設路由」\*\* 的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="664e5-735">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="664e5-736">排序屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-736">Ordering attribute routes</span></span>

<span data-ttu-id="664e5-737">與按定義順序執行的傳統路由不同,屬性路由生成樹並同時匹配所有路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-737">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="664e5-738">此行為如同依理想的排序來排列路由項目，最明確的路由會有機會在較泛型的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-738">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="664e5-739">例如，`blog/search/{topic}` 等路由比 `blog/{*article}` 等路由更明確。</span><span class="sxs-lookup"><span data-stu-id="664e5-739">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="664e5-740">邏輯上來說，預設會先「執行」`blog/search/{topic}`，因為這是唯一合理的排序。</span><span class="sxs-lookup"><span data-stu-id="664e5-740">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="664e5-741">使用慣例路由，開發人員會負責依想要的順序來排列路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-741">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="664e5-742">您可以使用架構提供之所有路由屬性的 `Order` 屬性，來設定屬性路由的順序。</span><span class="sxs-lookup"><span data-stu-id="664e5-742">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="664e5-743">路由會依 `Order` 屬性的遞增排序來處理。</span><span class="sxs-lookup"><span data-stu-id="664e5-743">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="664e5-744">預設順序為 `0`。</span><span class="sxs-lookup"><span data-stu-id="664e5-744">The default order is `0`.</span></span> <span data-ttu-id="664e5-745">使用 `Order = -1` 設定的路由會在未設定順序的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-745">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="664e5-746">使用 `Order = 1` 設定的路由會在預設路由排序之後執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-746">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="664e5-747">請避免依賴 `Order`。</span><span class="sxs-lookup"><span data-stu-id="664e5-747">Avoid depending on `Order`.</span></span> <span data-ttu-id="664e5-748">如果您的 URL 空間需要明確的順序值才能正確地路由，則同樣也可能會使用戶端混淆。</span><span class="sxs-lookup"><span data-stu-id="664e5-748">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="664e5-749">一般而言，屬性路由會透過 URL 比對來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-749">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="664e5-750">如果用於 URL 產生的預設順序無效，使用路由名稱作為覆寫通常會比套用 `Order` 屬性更簡單。</span><span class="sxs-lookup"><span data-stu-id="664e5-750">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="664e5-751">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="664e5-751">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="664e5-752">如需 Razor Pages 主題中路由順序的資訊，請參閱 [Razor Pages 路由和應用程式慣例：路由順序](xref:razor-pages/razor-pages-conventions#route-order)。</span><span class="sxs-lookup"><span data-stu-id="664e5-752">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="664e5-753">路由範本中的語彙基元取代 ([controller]、[action]、[area])</span><span class="sxs-lookup"><span data-stu-id="664e5-753">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="664e5-754">為方便起見,屬性路由通過將權杖包裝在方形括弧 (,`[` `]`) 支援*權杖 。*</span><span class="sxs-lookup"><span data-stu-id="664e5-754">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="664e5-755">語彙基元 `[action]`、`[area]` 與 `[controller]` 會分別以定義路由之動作的動作名稱值、區域名稱值和控制器名稱值來取代。</span><span class="sxs-lookup"><span data-stu-id="664e5-755">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="664e5-756">在下列範例中，動作會符合註解中所述的 URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="664e5-756">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="664e5-757">語彙基元取代會在建立屬性路由的最後一個步驟發生。</span><span class="sxs-lookup"><span data-stu-id="664e5-757">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="664e5-758">上述範例的運作方式與下列程式碼相同：</span><span class="sxs-lookup"><span data-stu-id="664e5-758">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="664e5-759">屬性路由也可以透過繼承來合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-759">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="664e5-760">這與語彙基元取代結合會特別強大。</span><span class="sxs-lookup"><span data-stu-id="664e5-760">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="664e5-761">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-761">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="664e5-762">`[Route("[controller]/[action]", Name="[controller]_[action]")]` 會針對每個動作產生唯一的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-762">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="664e5-763">若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="664e5-763">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="664e5-764">使用參數轉換程式自訂語彙基元取代</span><span class="sxs-lookup"><span data-stu-id="664e5-764">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="664e5-765">可以使用參數轉換程式自訂語彙基元取代。</span><span class="sxs-lookup"><span data-stu-id="664e5-765">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="664e5-766">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="664e5-766">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="664e5-767">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="664e5-767">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="664e5-768">`RouteTokenTransformerConvention` 是一個應用程式模型慣例，它會：</span><span class="sxs-lookup"><span data-stu-id="664e5-768">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="664e5-769">將參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-769">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="664e5-770">在取代屬性路由語彙基元值時自訂它們。</span><span class="sxs-lookup"><span data-stu-id="664e5-770">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="664e5-771">`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。</span><span class="sxs-lookup"><span data-stu-id="664e5-771">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

### <a name="multiple-routes"></a><span data-ttu-id="664e5-772">多個路由</span><span class="sxs-lookup"><span data-stu-id="664e5-772">Multiple Routes</span></span>

<span data-ttu-id="664e5-773">屬性路由支援定義多個路由來達到相同的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-773">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="664e5-774">最常見的用法是模擬「預設慣例路由」\*\* 的行為，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="664e5-774">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="664e5-775">將多個路由屬性放在控制器上，表示這些屬性會各自與動作方法上的每個路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-775">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="664e5-776">當一個動作上放有多個路由屬性 (實作`IActionConstraint`) 時，則每個動作條件約束都會與屬性中用來定義的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="664e5-776">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="664e5-777">雖然在動作上使用多個路由看似強大，但最好保持簡單且妥善定義的應用程式 URL 空間。</span><span class="sxs-lookup"><span data-stu-id="664e5-777">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="664e5-778">只有必要時，才在動作上使用多個路由；例如，為了支援現有的用戶端。</span><span class="sxs-lookup"><span data-stu-id="664e5-778">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="664e5-779">指定屬性路由的選擇性參數、預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="664e5-779">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="664e5-780">屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。</span><span class="sxs-lookup"><span data-stu-id="664e5-780">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="664e5-781">如需路由範本語法的詳細描述，請參閱[路由範本參考](xref:fundamentals/routing#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="664e5-781">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="664e5-782">使用 `IRouteTemplateProvider` 的自訂路由屬性</span><span class="sxs-lookup"><span data-stu-id="664e5-782">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="664e5-783">架構中提供的所有路由屬性 (`[Route(...)]`、`[HttpGet(...)]` 等) 都會實作 `IRouteTemplateProvider` 介面。</span><span class="sxs-lookup"><span data-stu-id="664e5-783">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="664e5-784">當應用程式啟動並使用實作 `IRouteTemplateProvider` 的屬性來建立初始路由集時，MVC 會尋找控制器類別和動作方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-784">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="664e5-785">您可以實作 `IRouteTemplateProvider` 來定義自己的路由屬性。</span><span class="sxs-lookup"><span data-stu-id="664e5-785">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="664e5-786">每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-786">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="664e5-787">上述範例中的屬性會在套用 `[MyApiController]` 時，自動將 `Template` 設定為 `"api/[controller]"`。</span><span class="sxs-lookup"><span data-stu-id="664e5-787">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="664e5-788">使用應用程式模型自訂屬性路由</span><span class="sxs-lookup"><span data-stu-id="664e5-788">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="664e5-789">「應用程式模型」\*\* 是在啟動時建立的物件模型，其中包含 MVC 用來路由及執行動作的所有中繼資料。</span><span class="sxs-lookup"><span data-stu-id="664e5-789">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="664e5-790">「應用程式模型」\*\* 包含從路由屬性收集的所有資料 (透過 `IRouteTemplateProvider`)。</span><span class="sxs-lookup"><span data-stu-id="664e5-790">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="664e5-791">您可以撰寫「慣例」\*\* 來修改啟動時的應用程式模型，以自訂路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="664e5-791">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="664e5-792">本節簡單示範如何使用應用程式模型來自訂路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-792">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="664e5-793">混合路由：屬性路由與慣例路由</span><span class="sxs-lookup"><span data-stu-id="664e5-793">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="664e5-794">MVC 應用程式可以混用慣例路由與屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-794">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="664e5-795">控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。</span><span class="sxs-lookup"><span data-stu-id="664e5-795">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="664e5-796">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-796">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="664e5-797">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-797">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="664e5-798">定義屬性路由的動作無法透過慣例路由到達，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="664e5-798">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="664e5-799">控制器上的**任何**路由屬性可讓控制器中的所有動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-799">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-800">這兩種路由系統的區別在於 URL 符合某個路由範本之後所套用的程序。</span><span class="sxs-lookup"><span data-stu-id="664e5-800">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="664e5-801">在慣例路由中，會使用相符項目中的路由值，從所有慣例路由動作的查閱資料表中選擇動作和控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-801">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="664e5-802">在屬性路由中，每個範本已與一個動作建立關聯，因此不需要進一步查閱。</span><span class="sxs-lookup"><span data-stu-id="664e5-802">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="664e5-803">複雜區段</span><span class="sxs-lookup"><span data-stu-id="664e5-803">Complex segments</span></span>

<span data-ttu-id="664e5-804">複雜區段 (例如，`[Route("/dog{token}cat")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="664e5-804">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="664e5-805">如需說明，請參閱[原始程式碼](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296)。</span><span class="sxs-lookup"><span data-stu-id="664e5-805">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="664e5-806">如需詳細資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8197)。</span><span class="sxs-lookup"><span data-stu-id="664e5-806">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="664e5-807">URL 產生</span><span class="sxs-lookup"><span data-stu-id="664e5-807">URL Generation</span></span>

<span data-ttu-id="664e5-808">MVC 應用程式可以使用路由的 URL 產生功能，來產生動作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="664e5-808">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="664e5-809">產生 URL 可排除硬式編碼的 URL，讓程式碼更穩定且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="664e5-809">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="664e5-810">本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 產生運作方式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="664e5-810">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="664e5-811">如需 URL 產生的詳細描述，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="664e5-811">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="664e5-812">`IUrlHelper` 介面是 MVC 與用於產生 URL 的路由之間的基礎結構部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-812">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="664e5-813">您將會透過控制器、檢視和檢視元件中 `Url` 屬性，來尋找可用的 `IUrlHelper` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="664e5-813">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="664e5-814">在此範例中，會透過 `Controller.Url` 屬性使用 `IUrlHelper` 介面來產生另一個動作的 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-814">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="664e5-815">如果應用程式使用預設慣例路由，`url` 變數的值會是 URL 路徑字串 `/UrlGeneration/Destination`。</span><span class="sxs-lookup"><span data-stu-id="664e5-815">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="664e5-816">此 URL 路徑是透過路由所建立，結合了目前要求中的路由值 (環境值) 與傳遞至 `Url.Action` 的值，並在路由範本中取代這些值：</span><span class="sxs-lookup"><span data-stu-id="664e5-816">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="664e5-817">路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。</span><span class="sxs-lookup"><span data-stu-id="664e5-817">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="664e5-818">不具有任何值的路由參數可以使用預設值 (若有)；如果是選擇性，則可以略過 (如此範例中的 `id` 所示)。</span><span class="sxs-lookup"><span data-stu-id="664e5-818">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="664e5-819">如果任何必要的路由參數沒有對應的值，URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="664e5-819">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="664e5-820">如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。</span><span class="sxs-lookup"><span data-stu-id="664e5-820">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="664e5-821">上述 `Url.Action` 範例假設使用慣例路由，但 URL 產生的運作方式會類似屬性路由，雖然概念完全不同。</span><span class="sxs-lookup"><span data-stu-id="664e5-821">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="664e5-822">在慣例路由中，使用路由值來展開範本，而且 `controller` 和 `action` 的路由值通常會出現在該範本中 (這是因為路由比對的 URL 符合「慣例」\*\*)。</span><span class="sxs-lookup"><span data-stu-id="664e5-822">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="664e5-823">在屬性路由中，`controller` 和 `action` 的路由值不可以出現在範本中，而是用來查閱要使用的範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-823">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="664e5-824">此範例使用屬性路由：</span><span class="sxs-lookup"><span data-stu-id="664e5-824">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="664e5-825">MVC 建立所有屬性路由動作的查閱資料表，並將比對 `controller` 和 `action` 值，以選取要用於 URL 產生的路由範本。</span><span class="sxs-lookup"><span data-stu-id="664e5-825">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="664e5-826">在上述範例中，會產生 `custom/url/to/destination`。</span><span class="sxs-lookup"><span data-stu-id="664e5-826">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="664e5-827">由動作名稱產生 URL</span><span class="sxs-lookup"><span data-stu-id="664e5-827">Generating URLs by action name</span></span>

<span data-ttu-id="664e5-828">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="664e5-828">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="664e5-829">`Action`) 及所有相關多載的基本概念，都是您想要透過指定控制器名稱和動作名稱，來指定連結的項目。</span><span class="sxs-lookup"><span data-stu-id="664e5-829">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-830">使用 `Url.Action` 時，會為您指定`controller` 和 `action` 的目前路由值 (`controller` 和 `action` 的值都屬於「環境值」\*\* **和「值」** \*\*)。</span><span class="sxs-lookup"><span data-stu-id="664e5-830">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="664e5-831">方法 `Url.Action` 一律會使用 `action` 和 `controller` 的目前值，而且會產生路由至目前動作的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="664e5-831">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="664e5-832">路由會嘗試使用環境值中的值，來填入您在產生 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="664e5-832">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="664e5-833">使用 `{a}/{b}/{c}/{d}` 等路由和環境值 `{ a = Alice, b = Bob, c = Carol, d = David }`，路由就會有足夠的資訊來產生不含任何其他值的 URL (因為所有路由參數都具有值)。</span><span class="sxs-lookup"><span data-stu-id="664e5-833">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="664e5-834">如果您新增值 `{ d = Donovan }`，則會忽略值 `{ d = David }`，而且產生的 URL 路徑會是 `Alice/Bob/Carol/Donovan`。</span><span class="sxs-lookup"><span data-stu-id="664e5-834">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="664e5-835">URL 路徑是階層式。</span><span class="sxs-lookup"><span data-stu-id="664e5-835">URL paths are hierarchical.</span></span> <span data-ttu-id="664e5-836">在上述範例中，如果您新增了值 `{ c = Cheryl }`，則會忽略 `{ c = Carol, d = David }` 這兩個值。</span><span class="sxs-lookup"><span data-stu-id="664e5-836">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="664e5-837">在此情況下，不再有 `d` 的值，因此 URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="664e5-837">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="664e5-838">您必須指定 `c` 和 `d` 所需的值。</span><span class="sxs-lookup"><span data-stu-id="664e5-838">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="664e5-839">使用預設路由 (`{controller}/{action}/{id?}`) 可能會遇到此問題，但實際上很少會遇到此行為，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。</span><span class="sxs-lookup"><span data-stu-id="664e5-839">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="664e5-840">較長的 `Url.Action` 多載還會接受一個額外的「路由值」\*\* 物件，來為 `controller` 和 `action` 以外的路由參數提供值。</span><span class="sxs-lookup"><span data-stu-id="664e5-840">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="664e5-841">您最常會在搭配 `id` 時看到此用法，例如 `Url.Action("Buy", "Products", new { id = 17 })`。</span><span class="sxs-lookup"><span data-stu-id="664e5-841">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="664e5-842">依照慣例，「路由值」\*\* 物件通常是匿名型別的物件，但也可以是 `IDictionary<>` 或「純舊 .NET 物件」\*\*。</span><span class="sxs-lookup"><span data-stu-id="664e5-842">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="664e5-843">不符合路由參數的任何額外路由值都會放在查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="664e5-843">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="664e5-844">若要建立絕對 URL，請使用接受 `protocol` 的多載：`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="664e5-844">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="664e5-845">由路由產生 URL</span><span class="sxs-lookup"><span data-stu-id="664e5-845">Generating URLs by route</span></span>

<span data-ttu-id="664e5-846">上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-846">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="664e5-847">`IUrlHelper` 也提供 `Url.RouteUrl` 系列方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-847">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="664e5-848">這些方法類似於 `Url.Action`，但不會將 `action` 和 `controller` 的目前值複製到路由值。</span><span class="sxs-lookup"><span data-stu-id="664e5-848">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="664e5-849">最常見的用法是指定路由名稱使用特定路由來產生 URL，通常「不需要」\*\* 指定控制器或動作名稱。</span><span class="sxs-lookup"><span data-stu-id="664e5-849">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="664e5-850">在 HTML 中產生 URL</span><span class="sxs-lookup"><span data-stu-id="664e5-850">Generating URLs in HTML</span></span>

<span data-ttu-id="664e5-851">`IHtmlHelper` 提供 `HtmlHelper` 方法 `Html.BeginForm` 和 `Html.ActionLink`，以分別產生 `<form>` 和 `<a>` 項目。</span><span class="sxs-lookup"><span data-stu-id="664e5-851">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="664e5-852">這些方法使用 `Url.Action` 方法來產生 URL，並接受類似的引數。</span><span class="sxs-lookup"><span data-stu-id="664e5-852">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="664e5-853">`HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。</span><span class="sxs-lookup"><span data-stu-id="664e5-853">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="664e5-854">TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-854">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="664e5-855">這兩者使用 `IUrlHelper` 進行實作。</span><span class="sxs-lookup"><span data-stu-id="664e5-855">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="664e5-856">如需詳細資訊，請參閱[使用表單](../views/working-with-forms.md)。</span><span class="sxs-lookup"><span data-stu-id="664e5-856">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="664e5-857">在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-857">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="664e5-858">在動作結果中產生 URL</span><span class="sxs-lookup"><span data-stu-id="664e5-858">Generating URLS in Action Results</span></span>

<span data-ttu-id="664e5-859">上述範例示範如何在控制器中使用 `IUrlHelper`，但控制器中最常見的用法是產生 URL 以作為動作結果的一部分。</span><span class="sxs-lookup"><span data-stu-id="664e5-859">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="664e5-860">`ControllerBase` 和 `Controller` 基底類別提供便利的方法讓動作結果可參考其他動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-860">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="664e5-861">一個典型的用法是在接受使用者輸入之後重新導向。</span><span class="sxs-lookup"><span data-stu-id="664e5-861">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="664e5-862">動作結果的 Factory 方法遵循類似於 `IUrlHelper` 上方法的模式。</span><span class="sxs-lookup"><span data-stu-id="664e5-862">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="664e5-863">專用慣例路由的特殊案例</span><span class="sxs-lookup"><span data-stu-id="664e5-863">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="664e5-864">慣例路由可使用一種特殊路由定義，稱為「專用慣例路由」\*\*。</span><span class="sxs-lookup"><span data-stu-id="664e5-864">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="664e5-865">在下列範例中，名為 `blog` 的路由是專用慣例路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-865">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="664e5-866">透過這些路由定義，`Url.Action("Index", "Home")` 會使用 `default` 路由產生 URL 路徑 `/`，但為什麼？</span><span class="sxs-lookup"><span data-stu-id="664e5-866">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="664e5-867">您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="664e5-867">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="664e5-868">專用慣例路由依賴沒有對應路由參數之預設值的特殊行為，以防止用於 URL 產生的路由變得「太窮盡」。</span><span class="sxs-lookup"><span data-stu-id="664e5-868">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="664e5-869">在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="664e5-869">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="664e5-870">當路由執行 URL 產生時，所提供的值必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="664e5-870">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="664e5-871">使用 `blog` 產生 URL 會失敗，因為值 `{ controller = Home, action = Index }` 不符合 `{ controller = Blog, action = Article }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-871">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="664e5-872">路由會接著切換並嘗試 `default`，此時會成功。</span><span class="sxs-lookup"><span data-stu-id="664e5-872">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="664e5-873">區域</span><span class="sxs-lookup"><span data-stu-id="664e5-873">Areas</span></span>

<span data-ttu-id="664e5-874">[區域](areas.md)是 MVC 功能，可將相關功能組織成群組，作為個別路由命名空間 (適用於控制器動作) 和資料夾結構 (適用於檢視)。</span><span class="sxs-lookup"><span data-stu-id="664e5-874">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="664e5-875">使用區域可讓應用程式具有多個同名的控制器 (只要這些控制器具有不同的「區域」\*\* 即可)。</span><span class="sxs-lookup"><span data-stu-id="664e5-875">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="664e5-876">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="664e5-876">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="664e5-877">本節將討論路由如何與區域互動；如需區域如何與檢視搭配使用的詳細資料，請參閱[區域](areas.md)。</span><span class="sxs-lookup"><span data-stu-id="664e5-877">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="664e5-878">下列範例會設定 MVC 使用預設慣例路由，並為名為 `Blog` 的區域設定「區域路由」\*\*：</span><span class="sxs-lookup"><span data-stu-id="664e5-878">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="664e5-879">當符合 `/Manage/Users/AddUser` 等 URL 路徑時，第一個路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-879">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="664e5-880">`area` 路由值是由 `area` 的預設值所產生；事實上，`MapAreaRoute` 所建立的路由相當於：</span><span class="sxs-lookup"><span data-stu-id="664e5-880">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="664e5-881">`MapAreaRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-881">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="664e5-882">預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="664e5-882">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="664e5-883">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="664e5-883">Conventional routing is order-dependent.</span></span> <span data-ttu-id="664e5-884">一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="664e5-884">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="664e5-885">使用上述範例，路由值會符合下列動作：</span><span class="sxs-lookup"><span data-stu-id="664e5-885">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="664e5-886">`AreaAttribute` 可用來指定控制器為區域的一部分，假設此控制器在 `Blog` 區域中。</span><span class="sxs-lookup"><span data-stu-id="664e5-886">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="664e5-887">不含 `[Area]` 屬性的控制器不是任何區域的成員，因此當路由提供 `area` 路由值時**不會**符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-887">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="664e5-888">在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="664e5-888">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="664e5-889">為求完整起見，此處顯示每個控制器的命名空間，否則控制器會有命名衝突並產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="664e5-889">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="664e5-890">類別命名空間不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="664e5-890">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="664e5-891">前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-891">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="664e5-892">第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-892">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-893">在「未符合任何值」\*\* 的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="664e5-893">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="664e5-894">執行區域中的動作時，`area` 的路由值會作為路由用於 URL 產生的「環境值」\*\*。</span><span class="sxs-lookup"><span data-stu-id="664e5-894">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="664e5-895">這表示區域預設會以「黏性」\*\* 方式來處理 URL 產生，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="664e5-895">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="664e5-896">了解 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="664e5-896">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="664e5-897">本節深入探討架構內部及 MVC 如何選擇要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-897">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="664e5-898">一般應用程式不需要自訂 `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="664e5-898">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="664e5-899">即使您不熟悉 `IActionConstraint`，也可能已使用過此介面。</span><span class="sxs-lookup"><span data-stu-id="664e5-899">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="664e5-900">`[HttpGet]` 屬性和類似的 `[Http-VERB]` 屬性會實作 `IActionConstraint` 來限制動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-900">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="664e5-901">假設在預設慣例路由中，URL 路徑 `/Products/Edit` 會產生值 `{ controller = Products, action = Edit }`，該值符合此處所示的**兩個**動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-901">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="664e5-902">在 `IActionConstraint` 術語中，我們會假設這兩個動作可視為候選項目 (因為它們都符合路由資料)。</span><span class="sxs-lookup"><span data-stu-id="664e5-902">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="664e5-903">當 `HttpGetAttribute` 執行時，*Edit()* 會符合 *GET*，但不符合任何其他 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="664e5-903">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="664e5-904">`Edit(...)` 動作未定義任何條件約束，因此會符合任何 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="664e5-904">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="664e5-905">假設 `POST` - 只有 `Edit(...)` 相符。</span><span class="sxs-lookup"><span data-stu-id="664e5-905">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="664e5-906">但對於 `GET`，這兩個動作仍然相符；不過，具有 `IActionConstraint` 的動作一律比沒有的動作「更符合」\*\*。</span><span class="sxs-lookup"><span data-stu-id="664e5-906">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="664e5-907">由於 `Edit()` 具有 `[HttpGet]`，因此更明確；如果兩個動作都相符，就會選取此動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-907">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="664e5-908">就概念而言，`IActionConstraint` 是一種「多載」\*\* 形式，但它會在符合相同 URL　的動作之間進行多載，而不是多載具有相同名稱的方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-908">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="664e5-909">屬性路由也會使用 `IActionConstraint`，因此可能導致來自不同控制器的動作被視為候選項目。</span><span class="sxs-lookup"><span data-stu-id="664e5-909">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="664e5-910">實作 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="664e5-910">Implementing IActionConstraint</span></span>

<span data-ttu-id="664e5-911">實作 `IActionConstraint` 的最簡單方式是建立衍生自 `System.Attribute` 的類別，並將它放在您的動作和控制器上。</span><span class="sxs-lookup"><span data-stu-id="664e5-911">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="664e5-912">MVC 會自動探索作為屬性套用的任何 `IActionConstraint`。</span><span class="sxs-lookup"><span data-stu-id="664e5-912">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="664e5-913">您可以使用應用程式模型來套用條件約束，由於您可以之後編寫其套用方式的程式，因此可能是最彈性的方法。</span><span class="sxs-lookup"><span data-stu-id="664e5-913">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="664e5-914">在下面的示例中,約束根據路由數據*中的國家/地區代碼*選擇操作。</span><span class="sxs-lookup"><span data-stu-id="664e5-914">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="664e5-915">[GitHub 上有完整範例](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。</span><span class="sxs-lookup"><span data-stu-id="664e5-915">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="664e5-916">您必須負責實作 `Accept` 方法，並選擇 'Order' 作為要執行的條件約束。</span><span class="sxs-lookup"><span data-stu-id="664e5-916">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="664e5-917">在本例中，`Accept` 方法會傳回 `true`，表示當 `country` 路由值相符時動作會符合。</span><span class="sxs-lookup"><span data-stu-id="664e5-917">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="664e5-918">這與 `RouteValueAttribute` 不同，後者允許切換回未使用屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="664e5-918">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="664e5-919">此範例顯示如果您定義 `en-US` 動作，則 `fr-FR` 等國碼 (地區碼) 會切換回未套用 `[CountrySpecific(...)]` 的更泛型控制器。</span><span class="sxs-lookup"><span data-stu-id="664e5-919">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="664e5-920">`Order` 屬性決定條件約束所屬的「階段」\*\*。</span><span class="sxs-lookup"><span data-stu-id="664e5-920">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="664e5-921">群組中的動作條件約束會根據 `Order` 來執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-921">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="664e5-922">例如，架構提供的所有 HTTP 方法屬性都會使用相同的 `Order` 值，以便在同一個階段中執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-922">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="664e5-923">您可以視需要擁有許多階段來實作所需的原則。</span><span class="sxs-lookup"><span data-stu-id="664e5-923">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="664e5-924">若要決定 `Order` 的值，請考慮是否應該在 HTTP 方法之前套用條件約束。</span><span class="sxs-lookup"><span data-stu-id="664e5-924">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="664e5-925">數值愈低會愈先執行。</span><span class="sxs-lookup"><span data-stu-id="664e5-925">Lower numbers run first.</span></span>

::: moniker-end
