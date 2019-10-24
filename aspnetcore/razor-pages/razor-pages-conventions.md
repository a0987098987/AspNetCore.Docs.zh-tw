---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779210"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="35a44-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="35a44-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35a44-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="35a44-105">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="35a44-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="35a44-106">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="35a44-107">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="35a44-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="35a44-108">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="35a44-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="35a44-109">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="35a44-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="35a44-110">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="35a44-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="35a44-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35a44-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="35a44-112">情節</span><span class="sxs-lookup"><span data-stu-id="35a44-112">Scenario</span></span> | <span data-ttu-id="35a44-113">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="35a44-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="35a44-114">模型慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="35a44-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="35a44-115">Conventions.Add</span></span><ul><li><span data-ttu-id="35a44-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="35a44-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="35a44-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="35a44-119">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="35a44-120">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="35a44-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="35a44-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="35a44-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="35a44-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="35a44-124">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="35a44-125">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="35a44-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="35a44-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="35a44-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="35a44-128">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="35a44-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="35a44-129">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="35a44-130">Razor Pages 慣例會使用 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 擴充方法來新增和設定，以在 `Startup` 類別的服務集合上 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>。</span><span class="sxs-lookup"><span data-stu-id="35a44-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="35a44-131">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="35a44-131">The following convention examples are explained later in this topic:</span></span>

::: moniker range=">= aspnetcore-3.0"

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

## <a name="route-order"></a><span data-ttu-id="35a44-132">路由順序</span><span class="sxs-lookup"><span data-stu-id="35a44-132">Route order</span></span>

<span data-ttu-id="35a44-133">路由會指定處理的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> （路由對應）。</span><span class="sxs-lookup"><span data-stu-id="35a44-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="35a44-134">順序</span><span class="sxs-lookup"><span data-stu-id="35a44-134">Order</span></span>            | <span data-ttu-id="35a44-135">行為</span><span class="sxs-lookup"><span data-stu-id="35a44-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="35a44-136">-1</span><span class="sxs-lookup"><span data-stu-id="35a44-136">-1</span></span>               | <span data-ttu-id="35a44-137">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="35a44-138">0</span><span class="sxs-lookup"><span data-stu-id="35a44-138">0</span></span>                | <span data-ttu-id="35a44-139">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="35a44-139">Order isn't specified (default value).</span></span> <span data-ttu-id="35a44-140">未指派 `Order` （`Order = null`）預設會將路由 `Order` 為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="35a44-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="35a44-141">1、2、&hellip; n</span><span class="sxs-lookup"><span data-stu-id="35a44-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="35a44-142">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="35a44-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="35a44-143">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="35a44-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="35a44-144">路由會依序處理（-1、0、1、2、&hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="35a44-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="35a44-145">當路由具有相同的 `Order` 時，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="35a44-146">當具有相同 `Order` 且參數數目相同的路由符合要求 URL 時，會依將路由新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 的順序來進行處理。</span><span class="sxs-lookup"><span data-stu-id="35a44-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="35a44-147">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="35a44-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="35a44-148">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="35a44-149">如果您必須將路由 `Order` 屬性設定為正確路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="35a44-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="35a44-150">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="35a44-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="35a44-151">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="35a44-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="35a44-152">不過，您應該嘗試避免在生產應用程式中設定路由 `Order` 的做法。</span><span class="sxs-lookup"><span data-stu-id="35a44-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="35a44-153">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="35a44-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="35a44-154">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="35a44-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="35a44-155">模型慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-155">Model conventions</span></span>

<span data-ttu-id="35a44-156">新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 的委派，以加入適用于 Razor Pages 的[模型慣例](xref:mvc/controllers/application-model#conventions)。</span><span class="sxs-lookup"><span data-stu-id="35a44-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="35a44-157">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="35a44-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="35a44-158">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，並將其加入至頁面路由模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="35a44-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="35a44-159">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="35a44-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-160"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="35a44-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="35a44-161">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="35a44-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="35a44-162">本主題稍後會新增 `TheContactPage/{text?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="35a44-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="35a44-163">連絡人頁面路由的預設順序是 `null` （`Order = 0`），因此它會符合 `{globalTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="35a44-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="35a44-164">本主題稍後會新增 `{aboutTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="35a44-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="35a44-165">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="35a44-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="35a44-166">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="35a44-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="35a44-167">本主題稍後會新增 `{otherPagesTemplate?}` 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="35a44-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="35a44-168">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="35a44-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="35a44-169">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如 `/OtherPages/Page1/RouteDataValue`），會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="35a44-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="35a44-170">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="35a44-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="35a44-171">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="35a44-172">當 MVC 新增至 `Startup.ConfigureServices` 中的服務集合時，會加入 Razor Pages 選項，例如新增 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>。</span><span class="sxs-lookup"><span data-stu-id="35a44-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="35a44-173">如需範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="35a44-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-174">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="35a44-177">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="35a44-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="35a44-178">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，並將其新增至頁面應用程式模型結構期間所套用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例的集合。</span><span class="sxs-lookup"><span data-stu-id="35a44-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="35a44-179">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="35a44-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="35a44-180">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="35a44-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="35a44-181">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="35a44-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="35a44-182">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="35a44-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="35a44-183">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="35a44-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-184">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="35a44-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="35a44-185">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="35a44-187">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="35a44-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="35a44-188">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>，並將其新增至在頁面處理常式模型結構中套用的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 實例集合。</span><span class="sxs-lookup"><span data-stu-id="35a44-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-189">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="35a44-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="35a44-190">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-190">Page route action conventions</span></span>

<span data-ttu-id="35a44-191">衍生自 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 的預設路由模型提供者會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="35a44-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="35a44-192">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-192">Folder route model convention</span></span>

<span data-ttu-id="35a44-193">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="35a44-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="35a44-194">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="35a44-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="35a44-195"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="35a44-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="35a44-196">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="35a44-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="35a44-197">如果*Pages/OtherPages*資料夾中的頁面是以路由參數值（例如 `/OtherPages/Page1/RouteDataValue`）要求，則 "因此 routedatavalue" 會載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["otherPagesTemplate"]` （`Order = 2`）中，因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="35a44-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="35a44-198">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="35a44-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="35a44-199">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="35a44-200">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="35a44-203">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-203">Page route model convention</span></span>

<span data-ttu-id="35a44-204">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="35a44-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="35a44-205">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="35a44-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="35a44-206"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="35a44-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="35a44-207">這可確保當提供單一路由值時，第一個路由資料值位置的 `{globalTemplate?}` （在主題中稍早設定 `1`）的範本會獲得優先權。</span><span class="sxs-lookup"><span data-stu-id="35a44-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="35a44-208">如果在 `/About/RouteDataValue` 使用路由參數值要求 [關於] 頁面，則會將 "因此 routedatavalue" 載入 `RouteData.Values["globalTemplate"]` （`Order = 1`），而不是 `RouteData.Values["aboutTemplate"]` （`Order = 2`），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="35a44-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="35a44-209">盡可能不要設定 `Order`，這會導致 `Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="35a44-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="35a44-210">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="35a44-211">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="35a44-214">使用參數轉換器來自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="35a44-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="35a44-215">ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。</span><span class="sxs-lookup"><span data-stu-id="35a44-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="35a44-216">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="35a44-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="35a44-217">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="35a44-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="35a44-218">@No__t_0 頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。</span><span class="sxs-lookup"><span data-stu-id="35a44-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="35a44-219">例如，位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的 Razor Pages 檔案會將其路由從 `/SubscriptionManagement/ViewAll` 重寫至 `/subscription-management/view-all`。</span><span class="sxs-lookup"><span data-stu-id="35a44-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="35a44-220">`PageRouteTransformerConvention` 只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。</span><span class="sxs-lookup"><span data-stu-id="35a44-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="35a44-221">它不會轉換使用 `@page` 指示詞新增的路由區段。</span><span class="sxs-lookup"><span data-stu-id="35a44-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="35a44-222">此慣例也不會轉換 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 所新增的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="35a44-223">@No__t_0 會註冊為 `Startup.ConfigureServices` 中的選項：</span><span class="sxs-lookup"><span data-stu-id="35a44-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

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

::: moniker-end

::: moniker range="= aspnetcore-2.2"

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

::: moniker-end

## <a name="configure-a-page-route"></a><span data-ttu-id="35a44-224">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="35a44-224">Configure a page route</span></span>

<span data-ttu-id="35a44-225">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="35a44-226">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="35a44-227">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="35a44-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="35a44-228">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="35a44-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="35a44-229">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="35a44-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="35a44-230">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="35a44-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="35a44-231">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="35a44-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="35a44-232">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="35a44-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="35a44-235">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="35a44-236">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="35a44-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="35a44-239">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="35a44-239">Page model action conventions</span></span>

<span data-ttu-id="35a44-240">實 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 的預設頁面模型提供者會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="35a44-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="35a44-241">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="35a44-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="35a44-242">在本節的範例中，範例應用程式會使用 `AddHeaderAttribute` 類別，也就是會套用回應標頭的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>：</span><span class="sxs-lookup"><span data-stu-id="35a44-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-243">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="35a44-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="35a44-244">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="35a44-244">**Folder app model convention**</span></span>

<span data-ttu-id="35a44-245">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 來建立和新增 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以針對指定資料夾下的所有頁面，在 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="35a44-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="35a44-246">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="35a44-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="35a44-247">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="35a44-249">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="35a44-249">**Page app model convention**</span></span>

<span data-ttu-id="35a44-250">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 建立和加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，以在具有指定名稱的頁面上，叫用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 上的動作。</span><span class="sxs-lookup"><span data-stu-id="35a44-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="35a44-251">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="35a44-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="35a44-252">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="35a44-254">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="35a44-254">**Configure a filter**</span></span>

<span data-ttu-id="35a44-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 將指定的篩選準則設定為 [套用]。</span><span class="sxs-lookup"><span data-stu-id="35a44-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="35a44-256">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="35a44-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="35a44-257">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="35a44-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="35a44-258">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="35a44-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="35a44-259">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="35a44-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="35a44-260">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="35a44-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="35a44-261">由於 Razor Pages 會忽略動作篩選準則，因此如果路徑未包含 `OtherPages/Page2`，`EmptyFilter` 就不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="35a44-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="35a44-262">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="35a44-264">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="35a44-264">**Configure a filter factory**</span></span>

<span data-ttu-id="35a44-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="35a44-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="35a44-266">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="35a44-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="35a44-267">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="35a44-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="35a44-268">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="35a44-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="35a44-270">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="35a44-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="35a44-271">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="35a44-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="35a44-272">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="35a44-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="35a44-273">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="35a44-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="35a44-274">頁面篩選準則（<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>）是適用于 Razor Pages 的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="35a44-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="35a44-275">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="35a44-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35a44-276">其他資源</span><span class="sxs-lookup"><span data-stu-id="35a44-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
