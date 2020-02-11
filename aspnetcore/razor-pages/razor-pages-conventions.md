---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: d8377c0a0b8a29fe4b6a7fa67beeff84927c8b74
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114766"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="34706-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="34706-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="34706-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="34706-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="34706-105">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="34706-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="34706-106">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="34706-107">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="34706-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="34706-108">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="34706-109">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="34706-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="34706-110">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="34706-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="34706-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34706-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="34706-112">狀況</span><span class="sxs-lookup"><span data-stu-id="34706-112">Scenario</span></span> | <span data-ttu-id="34706-113">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="34706-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="34706-114">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="34706-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="34706-115">Conventions.Add</span></span><ul><li><span data-ttu-id="34706-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="34706-119">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="34706-120">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="34706-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="34706-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="34706-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="34706-124">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="34706-125">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="34706-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-128">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="34706-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="34706-129">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="34706-130">Razor Pages 慣例會使用 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 擴充方法來新增和設定，以在 `Startup` 類別的服務集合上 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>。</span><span class="sxs-lookup"><span data-stu-id="34706-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="34706-131">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="34706-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

## <a name="route-order"></a><span data-ttu-id="34706-132">路由順序</span><span class="sxs-lookup"><span data-stu-id="34706-132">Route order</span></span>

<span data-ttu-id="34706-133">路由會指定處理的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> （路由對應）。</span><span class="sxs-lookup"><span data-stu-id="34706-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="34706-134">單</span><span class="sxs-lookup"><span data-stu-id="34706-134">Order</span></span>            | <span data-ttu-id="34706-135">行為</span><span class="sxs-lookup"><span data-stu-id="34706-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="34706-136">-1</span><span class="sxs-lookup"><span data-stu-id="34706-136">-1</span></span>               | <span data-ttu-id="34706-137">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="34706-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="34706-138">0</span><span class="sxs-lookup"><span data-stu-id="34706-138">0</span></span>                | <span data-ttu-id="34706-139">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="34706-139">Order isn't specified (default value).</span></span> <span data-ttu-id="34706-140">未指派 `Order` （`Order = null`）預設會將路由 `Order` 為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="34706-141">1、2、&hellip; n</span><span class="sxs-lookup"><span data-stu-id="34706-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="34706-142">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="34706-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="34706-143">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="34706-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="34706-144">路由會依序處理（-1、0、1、2、&hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="34706-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="34706-145">當路由具有相同的 `Order`時，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="34706-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="34706-146">當具有相同 `Order` 且參數數目相同的路由符合要求 URL 時，會依將路由新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>的順序來進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="34706-147">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="34706-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="34706-148">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="34706-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="34706-149">如果您必須將路由 `Order` 屬性設定為正確路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="34706-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="34706-150">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="34706-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="34706-151">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="34706-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="34706-152">不過，您應該嘗試避免在生產應用程式中設定路由 `Order` 的做法。</span><span class="sxs-lookup"><span data-stu-id="34706-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="34706-153">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="34706-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="34706-154">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="34706-155">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-155">Model conventions</span></span>

<span data-ttu-id="34706-156">新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 的委派，以加入適用于 Razor Pages 的[模型慣例](xref:mvc/controllers/application-model#conventions)。</span><span class="sxs-lookup"><span data-stu-id="34706-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="34706-157">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="34706-158">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，並將其加入至頁面路由模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="34706-159">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-160"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="34706-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="34706-161">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="34706-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="34706-162">本主題稍後會新增 `TheContactPage/{text?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="34706-163">連絡人頁面路由的預設順序是 `null` （`Order = 0`），因此它會符合 `{globalTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="34706-164">本主題稍後會新增 `{aboutTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-165">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-166">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `RouteData.Values["globalTemplate"]` 屬性之故，因此 "RouteDataValue" 會載入至 `Order = 1` (`RouteData.Values["aboutTemplate"]`)，而不是 `Order = 2` (`Order`)。</span><span class="sxs-lookup"><span data-stu-id="34706-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="34706-167">本主題稍後會新增 `{otherPagesTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-168">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-169">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如 `/OtherPages/Page1/RouteDataValue`），會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-170">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-171">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-172">當 MVC 新增至 `Startup.ConfigureServices`中的服務集合時，會加入 Razor Pages 選項，例如新增 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>。</span><span class="sxs-lookup"><span data-stu-id="34706-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34706-173">如需範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="34706-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="34706-174">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="34706-177">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="34706-178">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，並將其新增至頁面應用程式模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="34706-179">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="34706-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="34706-180">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="34706-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="34706-181">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="34706-182">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="34706-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="34706-183">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-184">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="34706-185">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="34706-187">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="34706-188">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>，並將其新增至在頁面處理常式模型結構中套用的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例集合。</span><span class="sxs-lookup"><span data-stu-id="34706-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-189">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="34706-190">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-190">Page route action conventions</span></span>

<span data-ttu-id="34706-191">衍生自 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 的預設路由模型提供者會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="34706-192">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-192">Folder route model convention</span></span>

<span data-ttu-id="34706-193">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="34706-194">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="34706-195"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-196">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-197">如果*Pages/OtherPages*資料夾中的頁面是以路由參數值（例如 `/OtherPages/Page1/RouteDataValue`）要求，則 "因此 routedatavalue" 會載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`）中，因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-198">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-199">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-200">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="34706-203">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-203">Page route model convention</span></span>

<span data-ttu-id="34706-204">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-205">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="34706-206"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-207">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-208">如果在 `/About/RouteDataValue`使用路由參數值要求 [關於] 頁面，則會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["aboutTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-209">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-210">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-211">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="34706-214">使用參數轉換器來自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="34706-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="34706-215">ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。</span><span class="sxs-lookup"><span data-stu-id="34706-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="34706-216">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="34706-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="34706-217">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="34706-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="34706-218">`PageRouteTransformerConvention` 頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。</span><span class="sxs-lookup"><span data-stu-id="34706-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="34706-219">例如，位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的 Razor Pages 檔案會將其路由從 `/SubscriptionManagement/ViewAll` 重寫至 `/subscription-management/view-all`。</span><span class="sxs-lookup"><span data-stu-id="34706-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="34706-220">`PageRouteTransformerConvention` 只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。</span><span class="sxs-lookup"><span data-stu-id="34706-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="34706-221">它不會轉換使用 `@page` 指示詞新增的路由區段。</span><span class="sxs-lookup"><span data-stu-id="34706-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="34706-222">此慣例也不會轉換 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>所新增的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="34706-223">`PageRouteTransformerConvention` 會註冊為 `Startup.ConfigureServices`中的選項：</span><span class="sxs-lookup"><span data-stu-id="34706-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a><span data-ttu-id="34706-224">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="34706-224">Configure a page route</span></span>

<span data-ttu-id="34706-225">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="34706-226">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="34706-227">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="34706-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="34706-228">範例應用程式為 `/TheContactPage`Contact.cshtml*建立* 的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="34706-229">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="34706-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="34706-230">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="34706-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="34706-231">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="34706-232">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="34706-235">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="34706-236">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="34706-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="34706-239">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-239">Page model action conventions</span></span>

<span data-ttu-id="34706-240">實 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 的預設頁面模型提供者會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="34706-241">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="34706-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="34706-242">在本節的範例中，範例應用程式會使用 `AddHeaderAttribute` 類別，也就是會套用回應標頭的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>：</span><span class="sxs-lookup"><span data-stu-id="34706-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="34706-243">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="34706-244">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-244">**Folder app model convention**</span></span>

<span data-ttu-id="34706-245">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="34706-246">此範例示範如何將標頭 `AddFolderApplicationModelConvention` 新增至應用程式的 `OtherPagesHeader`OtherPages*資料夾內的頁面來使用*：</span><span class="sxs-lookup"><span data-stu-id="34706-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="34706-247">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="34706-249">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-249">**Page app model convention**</span></span>

<span data-ttu-id="34706-250">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-251">此範例示範如何將標頭 `AddPageApplicationModelConvention` 新增至 About 頁面來使用 `AboutHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="34706-252">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="34706-254">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="34706-254">**Configure a filter**</span></span>

<span data-ttu-id="34706-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 將指定的篩選準則設定為 [套用]。</span><span class="sxs-lookup"><span data-stu-id="34706-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="34706-256">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="34706-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="34706-257">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="34706-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="34706-258">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="34706-259">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="34706-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="34706-260">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="34706-261">由於 Razor Pages 會忽略動作篩選準則，因此如果路徑未包含 `OtherPages/Page2`，`EmptyFilter` 就不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="34706-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="34706-262">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="34706-264">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="34706-264">**Configure a filter factory**</span></span>

<span data-ttu-id="34706-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="34706-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="34706-266">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 [ 來使用](xref:mvc/controllers/filters#ifilterfactory)篩選條件 Factory`FilterFactoryHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="34706-267">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="34706-268">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="34706-270">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="34706-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="34706-271">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="34706-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="34706-272">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="34706-273">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="34706-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="34706-274">頁面篩選準則（<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>）是適用于 Razor Pages 的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="34706-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="34706-275">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="34706-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34706-276">其他資源</span><span class="sxs-lookup"><span data-stu-id="34706-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="34706-277">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="34706-277">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="34706-278">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-278">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="34706-279">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="34706-279">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="34706-280">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-280">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="34706-281">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="34706-281">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="34706-282">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="34706-282">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="34706-283">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34706-283">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="34706-284">狀況</span><span class="sxs-lookup"><span data-stu-id="34706-284">Scenario</span></span> | <span data-ttu-id="34706-285">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="34706-285">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="34706-286">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-286">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="34706-287">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="34706-287">Conventions.Add</span></span><ul><li><span data-ttu-id="34706-288">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-288">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-289">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-289">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-290">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-290">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="34706-291">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-291">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="34706-292">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-292">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="34706-293">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-293">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="34706-294">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-294">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-295">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="34706-295">AddPageRoute</span></span></li></ul> | <span data-ttu-id="34706-296">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-296">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="34706-297">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-297">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="34706-298">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-298">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-299">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-299">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-300">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="34706-300">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="34706-301">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-301">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="34706-302">Razor Pages 慣例會使用 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 擴充方法來新增和設定，以在 `Startup` 類別的服務集合上 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>。</span><span class="sxs-lookup"><span data-stu-id="34706-302">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="34706-303">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="34706-303">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

## <a name="route-order"></a><span data-ttu-id="34706-304">路由順序</span><span class="sxs-lookup"><span data-stu-id="34706-304">Route order</span></span>

<span data-ttu-id="34706-305">路由會指定處理的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> （路由對應）。</span><span class="sxs-lookup"><span data-stu-id="34706-305">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="34706-306">單</span><span class="sxs-lookup"><span data-stu-id="34706-306">Order</span></span>            | <span data-ttu-id="34706-307">行為</span><span class="sxs-lookup"><span data-stu-id="34706-307">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="34706-308">-1</span><span class="sxs-lookup"><span data-stu-id="34706-308">-1</span></span>               | <span data-ttu-id="34706-309">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="34706-309">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="34706-310">0</span><span class="sxs-lookup"><span data-stu-id="34706-310">0</span></span>                | <span data-ttu-id="34706-311">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="34706-311">Order isn't specified (default value).</span></span> <span data-ttu-id="34706-312">未指派 `Order` （`Order = null`）預設會將路由 `Order` 為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-312">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="34706-313">1、2、&hellip; n</span><span class="sxs-lookup"><span data-stu-id="34706-313">1, 2, &hellip; n</span></span> | <span data-ttu-id="34706-314">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="34706-314">Specifies the route processing order.</span></span> |

<span data-ttu-id="34706-315">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="34706-315">Route processing is established by convention:</span></span>

* <span data-ttu-id="34706-316">路由會依序處理（-1、0、1、2、&hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="34706-316">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="34706-317">當路由具有相同的 `Order`時，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="34706-317">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="34706-318">當具有相同 `Order` 且參數數目相同的路由符合要求 URL 時，會依將路由新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>的順序來進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-318">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="34706-319">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="34706-319">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="34706-320">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="34706-320">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="34706-321">如果您必須將路由 `Order` 屬性設定為正確路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="34706-321">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="34706-322">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="34706-322">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="34706-323">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="34706-323">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="34706-324">不過，您應該嘗試避免在生產應用程式中設定路由 `Order` 的做法。</span><span class="sxs-lookup"><span data-stu-id="34706-324">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="34706-325">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="34706-325">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="34706-326">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-326">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="34706-327">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-327">Model conventions</span></span>

<span data-ttu-id="34706-328">新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 的委派，以加入適用于 Razor Pages 的[模型慣例](xref:mvc/controllers/application-model#conventions)。</span><span class="sxs-lookup"><span data-stu-id="34706-328">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="34706-329">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-329">Add a route model convention to all pages</span></span>

<span data-ttu-id="34706-330">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，並將其加入至頁面路由模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-330">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="34706-331">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-331">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-332"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="34706-332">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="34706-333">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="34706-333">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="34706-334">本主題稍後會新增 `TheContactPage/{text?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-334">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="34706-335">連絡人頁面路由的預設順序是 `null` （`Order = 0`），因此它會符合 `{globalTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-335">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="34706-336">本主題稍後會新增 `{aboutTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-336">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-337">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-337">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-338">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `RouteData.Values["globalTemplate"]` 屬性之故，因此 "RouteDataValue" 會載入至 `Order = 1` (`RouteData.Values["aboutTemplate"]`)，而不是 `Order = 2` (`Order`)。</span><span class="sxs-lookup"><span data-stu-id="34706-338">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="34706-339">本主題稍後會新增 `{otherPagesTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-339">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-340">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-340">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-341">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如 `/OtherPages/Page1/RouteDataValue`），會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-341">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-342">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-342">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-343">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-343">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-344">當 MVC 新增至 `Startup.ConfigureServices`中的服務集合時，會加入 Razor Pages 選項，例如新增 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>。</span><span class="sxs-lookup"><span data-stu-id="34706-344">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34706-345">如需範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="34706-345">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="34706-346">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-346">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="34706-349">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-349">Add an app model convention to all pages</span></span>

<span data-ttu-id="34706-350">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，並將其新增至頁面應用程式模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-350">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="34706-351">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="34706-351">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="34706-352">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="34706-352">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="34706-353">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-353">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="34706-354">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="34706-354">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="34706-355">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-355">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-356">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-356">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="34706-357">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-357">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="34706-359">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-359">Add a handler model convention to all pages</span></span>

<span data-ttu-id="34706-360">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>，並將其新增至在頁面處理常式模型結構中套用的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例集合。</span><span class="sxs-lookup"><span data-stu-id="34706-360">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-361">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-361">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="34706-362">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-362">Page route action conventions</span></span>

<span data-ttu-id="34706-363">衍生自 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 的預設路由模型提供者會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-363">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="34706-364">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-364">Folder route model convention</span></span>

<span data-ttu-id="34706-365">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-365">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="34706-366">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-366">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="34706-367"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-367">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-368">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-368">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-369">如果*Pages/OtherPages*資料夾中的頁面是以路由參數值（例如 `/OtherPages/Page1/RouteDataValue`）要求，則 "因此 routedatavalue" 會載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`）中，因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-369">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-370">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-370">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-371">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-371">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-372">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-372">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="34706-375">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-375">Page route model convention</span></span>

<span data-ttu-id="34706-376">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-376">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-377">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-377">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="34706-378"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-378">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-379">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-379">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-380">如果在 `/About/RouteDataValue`使用路由參數值要求 [關於] 頁面，則會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["aboutTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-380">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-381">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-381">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-382">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-382">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-383">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-383">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="34706-386">使用參數轉換器來自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="34706-386">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="34706-387">ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。</span><span class="sxs-lookup"><span data-stu-id="34706-387">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="34706-388">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="34706-388">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="34706-389">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="34706-389">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="34706-390">`PageRouteTransformerConvention` 頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。</span><span class="sxs-lookup"><span data-stu-id="34706-390">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="34706-391">例如，位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的 Razor Pages 檔案會將其路由從 `/SubscriptionManagement/ViewAll` 重寫至 `/subscription-management/view-all`。</span><span class="sxs-lookup"><span data-stu-id="34706-391">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="34706-392">`PageRouteTransformerConvention` 只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。</span><span class="sxs-lookup"><span data-stu-id="34706-392">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="34706-393">它不會轉換使用 `@page` 指示詞新增的路由區段。</span><span class="sxs-lookup"><span data-stu-id="34706-393">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="34706-394">此慣例也不會轉換 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>所新增的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-394">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="34706-395">`PageRouteTransformerConvention` 會註冊為 `Startup.ConfigureServices`中的選項：</span><span class="sxs-lookup"><span data-stu-id="34706-395">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a><span data-ttu-id="34706-396">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="34706-396">Configure a page route</span></span>

<span data-ttu-id="34706-397">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-397">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="34706-398">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-398">Generated links to the page use your specified route.</span></span> <span data-ttu-id="34706-399">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="34706-399">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="34706-400">範例應用程式為 `/TheContactPage`Contact.cshtml*建立* 的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-400">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="34706-401">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="34706-401">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="34706-402">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="34706-402">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="34706-403">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-403">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="34706-404">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-404">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="34706-407">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-407">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="34706-408">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="34706-408">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="34706-411">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-411">Page model action conventions</span></span>

<span data-ttu-id="34706-412">實 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 的預設頁面模型提供者會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-412">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="34706-413">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="34706-413">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="34706-414">在本節的範例中，範例應用程式會使用 `AddHeaderAttribute` 類別，也就是會套用回應標頭的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>：</span><span class="sxs-lookup"><span data-stu-id="34706-414">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="34706-415">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-415">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="34706-416">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-416">**Folder app model convention**</span></span>

<span data-ttu-id="34706-417">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-417">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="34706-418">此範例示範如何將標頭 `AddFolderApplicationModelConvention` 新增至應用程式的 `OtherPagesHeader`OtherPages*資料夾內的頁面來使用*：</span><span class="sxs-lookup"><span data-stu-id="34706-418">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="34706-419">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-419">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="34706-421">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-421">**Page app model convention**</span></span>

<span data-ttu-id="34706-422">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-422">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-423">此範例示範如何將標頭 `AddPageApplicationModelConvention` 新增至 About 頁面來使用 `AboutHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-423">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="34706-424">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-424">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="34706-426">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="34706-426">**Configure a filter**</span></span>

<span data-ttu-id="34706-427"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 將指定的篩選準則設定為 [套用]。</span><span class="sxs-lookup"><span data-stu-id="34706-427"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="34706-428">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="34706-428">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="34706-429">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="34706-429">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="34706-430">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-430">If the condition passes, a header is added.</span></span> <span data-ttu-id="34706-431">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="34706-431">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="34706-432">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-432">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="34706-433">由於 Razor Pages 會忽略動作篩選準則，因此如果路徑未包含 `OtherPages/Page2`，`EmptyFilter` 就不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="34706-433">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="34706-434">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-434">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="34706-436">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="34706-436">**Configure a filter factory**</span></span>

<span data-ttu-id="34706-437"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="34706-437"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="34706-438">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 [ 來使用](xref:mvc/controllers/filters#ifilterfactory)篩選條件 Factory`FilterFactoryHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-438">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="34706-439">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-439">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="34706-440">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-440">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="34706-442">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="34706-442">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="34706-443">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="34706-443">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="34706-444">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-444">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="34706-445">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="34706-445">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="34706-446">頁面篩選準則（<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>）是適用于 Razor Pages 的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="34706-446">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="34706-447">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="34706-447">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34706-448">其他資源</span><span class="sxs-lookup"><span data-stu-id="34706-448">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="34706-449">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="34706-449">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="34706-450">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-450">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="34706-451">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="34706-451">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="34706-452">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-452">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="34706-453">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="34706-453">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="34706-454">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="34706-454">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="34706-455">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34706-455">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="34706-456">狀況</span><span class="sxs-lookup"><span data-stu-id="34706-456">Scenario</span></span> | <span data-ttu-id="34706-457">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="34706-457">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="34706-458">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-458">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="34706-459">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="34706-459">Conventions.Add</span></span><ul><li><span data-ttu-id="34706-460">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-460">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-461">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-461">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-462">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-462">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="34706-463">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-463">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="34706-464">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-464">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="34706-465">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-465">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="34706-466">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-466">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="34706-467">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="34706-467">AddPageRoute</span></span></li></ul> | <span data-ttu-id="34706-468">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-468">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="34706-469">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-469">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="34706-470">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-470">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-471">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="34706-471">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="34706-472">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="34706-472">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="34706-473">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-473">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="34706-474">Razor Pages 慣例會使用 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 擴充方法來新增和設定，以在 `Startup` 類別的服務集合上 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>。</span><span class="sxs-lookup"><span data-stu-id="34706-474">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="34706-475">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="34706-475">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

## <a name="route-order"></a><span data-ttu-id="34706-476">路由順序</span><span class="sxs-lookup"><span data-stu-id="34706-476">Route order</span></span>

<span data-ttu-id="34706-477">路由會指定處理的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> （路由對應）。</span><span class="sxs-lookup"><span data-stu-id="34706-477">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="34706-478">單</span><span class="sxs-lookup"><span data-stu-id="34706-478">Order</span></span>            | <span data-ttu-id="34706-479">行為</span><span class="sxs-lookup"><span data-stu-id="34706-479">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="34706-480">-1</span><span class="sxs-lookup"><span data-stu-id="34706-480">-1</span></span>               | <span data-ttu-id="34706-481">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="34706-481">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="34706-482">0</span><span class="sxs-lookup"><span data-stu-id="34706-482">0</span></span>                | <span data-ttu-id="34706-483">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="34706-483">Order isn't specified (default value).</span></span> <span data-ttu-id="34706-484">未指派 `Order` （`Order = null`）預設會將路由 `Order` 為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-484">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="34706-485">1、2、&hellip; n</span><span class="sxs-lookup"><span data-stu-id="34706-485">1, 2, &hellip; n</span></span> | <span data-ttu-id="34706-486">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="34706-486">Specifies the route processing order.</span></span> |

<span data-ttu-id="34706-487">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="34706-487">Route processing is established by convention:</span></span>

* <span data-ttu-id="34706-488">路由會依序處理（-1、0、1、2、&hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="34706-488">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="34706-489">當路由具有相同的 `Order`時，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="34706-489">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="34706-490">當具有相同 `Order` 且參數數目相同的路由符合要求 URL 時，會依將路由新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>的順序來進行處理。</span><span class="sxs-lookup"><span data-stu-id="34706-490">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="34706-491">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="34706-491">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="34706-492">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="34706-492">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="34706-493">如果您必須將路由 `Order` 屬性設定為正確路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="34706-493">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="34706-494">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="34706-494">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="34706-495">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="34706-495">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="34706-496">不過，您應該嘗試避免在生產應用程式中設定路由 `Order` 的做法。</span><span class="sxs-lookup"><span data-stu-id="34706-496">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="34706-497">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="34706-497">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="34706-498">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="34706-498">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="34706-499">模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-499">Model conventions</span></span>

<span data-ttu-id="34706-500">新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 的委派，以加入適用于 Razor Pages 的[模型慣例](xref:mvc/controllers/application-model#conventions)。</span><span class="sxs-lookup"><span data-stu-id="34706-500">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="34706-501">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-501">Add a route model convention to all pages</span></span>

<span data-ttu-id="34706-502">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，並將其加入至頁面路由模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-502">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="34706-503">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-503">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-504"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="34706-504">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="34706-505">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="34706-505">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="34706-506">本主題稍後會新增 `TheContactPage/{text?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-506">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="34706-507">連絡人頁面路由的預設順序是 `null` （`Order = 0`），因此它會符合 `{globalTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-507">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="34706-508">本主題稍後會新增 `{aboutTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-508">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-509">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-509">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-510">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `RouteData.Values["globalTemplate"]` 屬性之故，因此 "RouteDataValue" 會載入至 `Order = 1` (`RouteData.Values["aboutTemplate"]`)，而不是 `Order = 2` (`Order`)。</span><span class="sxs-lookup"><span data-stu-id="34706-510">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="34706-511">本主題稍後會新增 `{otherPagesTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="34706-511">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="34706-512">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-512">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="34706-513">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如 `/OtherPages/Page1/RouteDataValue`），會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-513">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-514">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-514">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-515">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-515">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-516">當 MVC 新增至 `Startup.ConfigureServices`中的服務集合時，會加入 Razor Pages 選項，例如新增 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>。</span><span class="sxs-lookup"><span data-stu-id="34706-516">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34706-517">如需範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="34706-517">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="34706-518">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-518">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="34706-521">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-521">Add an app model convention to all pages</span></span>

<span data-ttu-id="34706-522">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，並將其新增至頁面應用程式模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="34706-522">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="34706-523">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="34706-523">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="34706-524">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="34706-524">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="34706-525">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-525">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="34706-526">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="34706-526">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="34706-527">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-527">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-528">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-528">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="34706-529">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-529">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="34706-531">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="34706-531">Add a handler model convention to all pages</span></span>

<span data-ttu-id="34706-532">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>，並將其新增至在頁面處理常式模型結構中套用的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例集合。</span><span class="sxs-lookup"><span data-stu-id="34706-532">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="34706-533">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-533">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="34706-534">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-534">Page route action conventions</span></span>

<span data-ttu-id="34706-535">衍生自 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 的預設路由模型提供者會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-535">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="34706-536">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-536">Folder route model convention</span></span>

<span data-ttu-id="34706-537">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-537">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="34706-538">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-538">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="34706-539"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-539">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-540">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-540">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-541">如果*Pages/OtherPages*資料夾中的頁面是以路由參數值（例如 `/OtherPages/Page1/RouteDataValue`）要求，則 "因此 routedatavalue" 會載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`）中，因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-541">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-542">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-542">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-543">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-543">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-544">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-544">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="34706-547">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="34706-547">Page route model convention</span></span>

<span data-ttu-id="34706-548">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-548">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-549">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-549">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="34706-550"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="34706-550">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="34706-551">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="34706-551">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="34706-552">如果在 `/About/RouteDataValue`使用路由參數值要求 [關於] 頁面，則會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["aboutTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="34706-552">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="34706-553">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="34706-553">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="34706-554">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-554">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="34706-555">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="34706-555">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="34706-558">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="34706-558">Configure a page route</span></span>

<span data-ttu-id="34706-559">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-559">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="34706-560">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="34706-560">Generated links to the page use your specified route.</span></span> <span data-ttu-id="34706-561">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="34706-561">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="34706-562">範例應用程式為 `/TheContactPage`Contact.cshtml*建立* 的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-562">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="34706-563">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="34706-563">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="34706-564">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="34706-564">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="34706-565">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="34706-565">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="34706-566">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="34706-566">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="34706-569">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-569">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="34706-570">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="34706-570">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="34706-573">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="34706-573">Page model action conventions</span></span>

<span data-ttu-id="34706-574">實 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 的預設頁面模型提供者會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="34706-574">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="34706-575">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="34706-575">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="34706-576">在本節的範例中，範例應用程式會使用 `AddHeaderAttribute` 類別，也就是會套用回應標頭的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>：</span><span class="sxs-lookup"><span data-stu-id="34706-576">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="34706-577">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="34706-577">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="34706-578">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-578">**Folder app model convention**</span></span>

<span data-ttu-id="34706-579">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="34706-579">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="34706-580">此範例示範如何將標頭 `AddFolderApplicationModelConvention` 新增至應用程式的 `OtherPagesHeader`OtherPages*資料夾內的頁面來使用*：</span><span class="sxs-lookup"><span data-stu-id="34706-580">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="34706-581">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-581">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="34706-583">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="34706-583">**Page app model convention**</span></span>

<span data-ttu-id="34706-584">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="34706-584">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="34706-585">此範例示範如何將標頭 `AddPageApplicationModelConvention` 新增至 About 頁面來使用 `AboutHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-585">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="34706-586">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-586">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="34706-588">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="34706-588">**Configure a filter**</span></span>

<span data-ttu-id="34706-589"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 將指定的篩選準則設定為 [套用]。</span><span class="sxs-lookup"><span data-stu-id="34706-589"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="34706-590">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="34706-590">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="34706-591">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="34706-591">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="34706-592">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="34706-592">If the condition passes, a header is added.</span></span> <span data-ttu-id="34706-593">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="34706-593">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="34706-594">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-594">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="34706-595">由於 Razor Pages 會忽略動作篩選準則，因此如果路徑未包含 `OtherPages/Page2`，`EmptyFilter` 就不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="34706-595">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="34706-596">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-596">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="34706-598">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="34706-598">**Configure a filter factory**</span></span>

<span data-ttu-id="34706-599"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="34706-599"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="34706-600">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 [ 來使用](xref:mvc/controllers/filters#ifilterfactory)篩選條件 Factory`FilterFactoryHeader`：</span><span class="sxs-lookup"><span data-stu-id="34706-600">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="34706-601">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="34706-601">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="34706-602">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="34706-602">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="34706-604">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="34706-604">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="34706-605">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="34706-605">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="34706-606">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="34706-606">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="34706-607">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="34706-607">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="34706-608">頁面篩選準則（<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>）是適用于 Razor Pages 的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="34706-608">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="34706-609">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="34706-609">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34706-610">其他資源</span><span class="sxs-lookup"><span data-stu-id="34706-610">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
