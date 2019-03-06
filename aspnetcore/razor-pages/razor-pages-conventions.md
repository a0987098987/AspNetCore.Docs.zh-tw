---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/27/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 5cfcae5cffd5d9484ca64c3885b838ae0a2b4a0d
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346511"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="7a5ae-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="7a5ae-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7a5ae-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7a5ae-105">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="7a5ae-106">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="7a5ae-107">若要指定頁面路由、 新增路由區段，或將參數新增至路由，請使用頁面的`@page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="7a5ae-108">如需詳細資訊，請參閱 <<c0> [ 自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="7a5ae-109">有不能作為路由區段或參數名稱的保留的字。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="7a5ae-110">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="7a5ae-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7a5ae-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="7a5ae-112">情節</span><span class="sxs-lookup"><span data-stu-id="7a5ae-112">Scenario</span></span> | <span data-ttu-id="7a5ae-113">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="7a5ae-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="7a5ae-114">模型慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="7a5ae-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="7a5ae-115">Conventions.Add</span></span><ul><li><span data-ttu-id="7a5ae-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="7a5ae-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="7a5ae-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="7a5ae-119">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="7a5ae-120">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="7a5ae-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="7a5ae-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="7a5ae-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="7a5ae-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="7a5ae-124">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="7a5ae-125">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="7a5ae-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="7a5ae-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7a5ae-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="7a5ae-128">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="7a5ae-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="7a5ae-129">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="7a5ae-130">Razor 頁面慣例會新增及設定<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>中的服務集合上`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="7a5ae-131">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="7a5ae-132">路由順序</span><span class="sxs-lookup"><span data-stu-id="7a5ae-132">Route order</span></span>

<span data-ttu-id="7a5ae-133">路由會指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>處理 （路由比對）。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="7a5ae-134">順序</span><span class="sxs-lookup"><span data-stu-id="7a5ae-134">Order</span></span>            | <span data-ttu-id="7a5ae-135">行為</span><span class="sxs-lookup"><span data-stu-id="7a5ae-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="7a5ae-136">-1</span><span class="sxs-lookup"><span data-stu-id="7a5ae-136">-1</span></span>               | <span data-ttu-id="7a5ae-137">在處理其他路由之前，會處理路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="7a5ae-138">0</span><span class="sxs-lookup"><span data-stu-id="7a5ae-138">0</span></span>                | <span data-ttu-id="7a5ae-139">未指定順序 （預設值）。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-139">Order isn't specified (default value).</span></span> <span data-ttu-id="7a5ae-140">未指派`Order`(`Order = null`) 預設路由`Order`設為 0 （零） 進行處理。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="7a5ae-141">1、 2、 &hellip; n</span><span class="sxs-lookup"><span data-stu-id="7a5ae-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="7a5ae-142">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="7a5ae-143">路由處理被藉由慣例：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="7a5ae-144">路由會循序處理 (-1、 0、 1、 2、 &hellip; n)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="7a5ae-145">當路由具有相同`Order`、 最明確的路由會比對，第一次後面較不明確的路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="7a5ae-146">當具有相同的路由`Order`相同的參數數目符合要求 URL，將它們加入的順序來處理路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="7a5ae-147">可能的話，請避免根據已建立的路由的處理順序而定。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="7a5ae-148">一般而言，路由會選取正確的路由，透過 URL 比對。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="7a5ae-149">如果您必須設定路由`Order`屬性來路由要求是否正確，應用程式的路由傳送，進而可能造成混淆的用戶端和容易維護。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="7a5ae-150">若要簡化應用程式的路由傳送，進而搜尋。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="7a5ae-151">範例應用程式需要的處理順序，來示範數個使用單一的應用程式的路由案例的外顯路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="7a5ae-152">不過，您應該嘗試避免設定路由的練習`Order`在生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="7a5ae-153">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="7a5ae-154">在 MVC 主題中的路由順序的詳細資訊位於[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="7a5ae-155">模型慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-155">Model conventions</span></span>

<span data-ttu-id="7a5ae-156">加入的委派<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>來新增[模型慣例](xref:mvc/controllers/application-model#conventions)，套用至 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="7a5ae-157">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="7a5ae-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="7a5ae-158">使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>頁面路由期間所套用的執行個體模型建構。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="7a5ae-159">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="7a5ae-160"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="7a5ae-161">這可確保下列路由比對範例應用程式的行為：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="7a5ae-162">路由範本`TheContactPage/{text?}`本主題稍後會加入。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="7a5ae-163">Contact 頁面路由會將預設順序`null`(`Order = 0`)，因此它會比對之前`{globalTemplate?}`路由範本。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="7a5ae-164">`{aboutTemplate?}`路由範本新增本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="7a5ae-165">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="7a5ae-166">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="7a5ae-167">`{otherPagesTemplate?}`路由範本新增本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="7a5ae-168">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="7a5ae-169">當任何頁面*頁面/OtherPages*資料夾要求的路由參數 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7a5ae-170">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7a5ae-171">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7a5ae-172">Razor 頁面選項，例如新增<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>，會新增至服務集合中新增 MVC 時`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7a5ae-173">如需範例，請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-173">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="7a5ae-174">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="7a5ae-177">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="7a5ae-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="7a5ae-178">使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>期間所套用的執行個體頁面上的應用程式模型建構。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="7a5ae-179">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="7a5ae-180">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="7a5ae-181">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="7a5ae-182">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="7a5ae-183">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="7a5ae-184">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="7a5ae-185">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="7a5ae-187">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="7a5ae-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="7a5ae-188">使用<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>的集合<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>期間所套用的執行個體頁面處理常式模型建構。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="7a5ae-189">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="7a5ae-190">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-190">Page route action conventions</span></span>

<span data-ttu-id="7a5ae-191">預設路由模型提供者會衍生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>叫用慣例的設計來提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="7a5ae-192">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-192">Folder route model convention</span></span>

<span data-ttu-id="7a5ae-193">使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>所有指定的資料夾下的頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="7a5ae-194">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="7a5ae-195"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="7a5ae-196">這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="7a5ae-197">如果中的頁面*頁/OtherPages*資料夾要求的路由參數值 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7a5ae-198">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7a5ae-199">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7a5ae-200">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="7a5ae-203">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-203">Page route model convention</span></span>

<span data-ttu-id="7a5ae-204">使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>頁面具有指定名稱。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="7a5ae-205">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="7a5ae-206"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="7a5ae-207">這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="7a5ae-208">如果 [關於] 頁面會要求使用路由參數值在`/About/RouteDataValue`，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 2`) 由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7a5ae-209">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7a5ae-210">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7a5ae-211">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="7a5ae-214">使用自訂頁面路由的參數轉換程式</span><span class="sxs-lookup"><span data-stu-id="7a5ae-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="7a5ae-215">您可以使用參數的轉換程式自訂由 ASP.NET Core 所產生的頁面路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="7a5ae-216">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="7a5ae-217">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="7a5ae-218">`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換程式用於應用程式中自動產生的頁面路由的資料夾和檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="7a5ae-219">在 Razor 頁面的檔案，例如 */Pages/SubscriptionManagement/ViewAll.cshtml*就必須改寫其路由`/SubscriptionManagement/ViewAll`至`/subscription-management/view-all`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="7a5ae-220">`PageRouteTransformerConvention` 只會轉換來自的 Razor Pages 資料夾和檔案名稱自動產生的區段的頁面路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="7a5ae-221">它不會轉換與新增的路由區段`@page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="7a5ae-222">慣例也不會轉換所新增的路由<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="7a5ae-223">`PageRouteTransformerConvention`登錄中的選項為`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7a5ae-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="7a5ae-224">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="7a5ae-224">Configure a page route</span></span>

<span data-ttu-id="7a5ae-225">使用<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>設定頁面的路由，在指定的頁面路徑。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="7a5ae-226">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="7a5ae-227">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="7a5ae-228">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="7a5ae-229">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="7a5ae-230">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="7a5ae-231">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="7a5ae-232">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="7a5ae-235">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="7a5ae-236">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="7a5ae-239">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-239">Page model action conventions</span></span>

<span data-ttu-id="7a5ae-240">預設頁面模型提供者會實作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>叫用慣例的設計來設定頁面模型提供擴充點。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="7a5ae-241">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="7a5ae-242">在本節中的範例，範例應用程式會使用`AddHeaderAttribute`類別，這是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>，套用回應標頭：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="7a5ae-243">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="7a5ae-244">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="7a5ae-244">**Folder app model convention**</span></span>

<span data-ttu-id="7a5ae-245">使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>指定的資料夾下的所有頁面的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="7a5ae-246">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="7a5ae-247">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="7a5ae-249">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="7a5ae-249">**Page app model convention**</span></span>

<span data-ttu-id="7a5ae-250">使用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>來建立並新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>，在叫用動作<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>頁面具有指定名稱。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="7a5ae-251">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="7a5ae-252">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="7a5ae-254">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="7a5ae-254">**Configure a filter**</span></span>

<span data-ttu-id="7a5ae-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定要套用指定的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="7a5ae-256">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="7a5ae-257">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="7a5ae-258">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="7a5ae-259">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="7a5ae-260">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="7a5ae-261">由於 Razor Pages 會忽略動作篩選條件，因此如果路徑未包含 `OtherPages/Page2`，則 `EmptyFilter` 不會如預期般生效。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="7a5ae-262">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="7a5ae-264">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="7a5ae-264">**Configure a filter factory**</span></span>

<span data-ttu-id="7a5ae-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> 設定要套用指定的 factory[篩選器](xref:mvc/controllers/filters)至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="7a5ae-266">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="7a5ae-267">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="7a5ae-268">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="7a5ae-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="7a5ae-270">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="7a5ae-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="7a5ae-271">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="7a5ae-272">其他類型的 MVC 篩選條件可供您使用：[授權](xref:mvc/controllers/filters#authorization-filters)，[例外狀況](xref:mvc/controllers/filters#exception-filters)，[資源](xref:mvc/controllers/filters#resource-filters)，和[結果](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="7a5ae-273">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="7a5ae-274">頁面篩選條件 (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) 篩選會套用至 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="7a5ae-275">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="7a5ae-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7a5ae-276">其他資源</span><span class="sxs-lookup"><span data-stu-id="7a5ae-276">Additional resources</span></span>

* [<span data-ttu-id="7a5ae-277">Razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="7a5ae-277">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
