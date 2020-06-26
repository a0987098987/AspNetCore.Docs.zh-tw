---
title: RazorASP.NET Core 中的頁面路由和應用程式慣例
author: rick-anderson
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 308ca4401289a55e5dba8d61de50644cb2a53433
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405246"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Razor<span data-ttu-id="c656f-103">ASP.NET Core 中的頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-103"> Pages route and app conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c656f-104">瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-104">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="c656f-105">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-105">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="c656f-106">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-106">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="c656f-107">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-107">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="c656f-108">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="c656f-108">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="c656f-109">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:mvc/controllers/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="c656f-109">For more information, see [Routing: Reserved routing names](xref:mvc/controllers/routing#reserved-routing-names).</span></span>

<span data-ttu-id="c656f-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c656f-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="c656f-111">狀況</span><span class="sxs-lookup"><span data-stu-id="c656f-111">Scenario</span></span> | <span data-ttu-id="c656f-112">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="c656f-112">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="c656f-113">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-113">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="c656f-114">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="c656f-114">Conventions.Add</span></span><ul><li><span data-ttu-id="c656f-115">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-115">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-116">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-116">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-117">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-117">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="c656f-118">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="c656f-119">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="c656f-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="c656f-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="c656f-123">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="c656f-124">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="c656f-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-127">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="c656f-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="c656f-128">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

Razor<span data-ttu-id="c656f-129">在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-129"> Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="c656f-130">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="c656f-130">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="c656f-131">路由順序</span><span class="sxs-lookup"><span data-stu-id="c656f-131">Route order</span></span>

<span data-ttu-id="c656f-132">路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。</span><span class="sxs-lookup"><span data-stu-id="c656f-132">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="c656f-133">單</span><span class="sxs-lookup"><span data-stu-id="c656f-133">Order</span></span>            | <span data-ttu-id="c656f-134">行為</span><span class="sxs-lookup"><span data-stu-id="c656f-134">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="c656f-135">-1</span><span class="sxs-lookup"><span data-stu-id="c656f-135">-1</span></span>               | <span data-ttu-id="c656f-136">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-136">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="c656f-137">0</span><span class="sxs-lookup"><span data-stu-id="c656f-137">0</span></span>                | <span data-ttu-id="c656f-138">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="c656f-138">Order isn't specified (default value).</span></span> <span data-ttu-id="c656f-139">Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-139">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="c656f-140">1、2、 &hellip; n</span><span class="sxs-lookup"><span data-stu-id="c656f-140">1, 2, &hellip; n</span></span> | <span data-ttu-id="c656f-141">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="c656f-141">Specifies the route processing order.</span></span> |

<span data-ttu-id="c656f-142">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="c656f-142">Route processing is established by convention:</span></span>

* <span data-ttu-id="c656f-143">路由會依序處理（-1、0、1、2、 &hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="c656f-143">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="c656f-144">當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-144">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="c656f-145">當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。</span><span class="sxs-lookup"><span data-stu-id="c656f-145">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="c656f-146">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="c656f-146">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="c656f-147">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-147">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="c656f-148">如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="c656f-148">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="c656f-149">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="c656f-149">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="c656f-150">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="c656f-150">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="c656f-151">不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。</span><span class="sxs-lookup"><span data-stu-id="c656f-151">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

Razor<span data-ttu-id="c656f-152">頁面路由和 MVC 控制器路由會共用一個執行。</span><span class="sxs-lookup"><span data-stu-id="c656f-152"> Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="c656f-153">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-153">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="c656f-154">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-154">Model conventions</span></span>

<span data-ttu-id="c656f-155">新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-155">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="c656f-156">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-156">Add a route model convention to all pages</span></span>

<span data-ttu-id="c656f-157">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-157">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="c656f-158">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-158">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-159"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="c656f-159">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="c656f-160">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="c656f-160">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="c656f-161">`TheContactPage/{text?}`稍後會在主題中新增的路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-161">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="c656f-162">連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-162">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="c656f-163">`{aboutTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-163">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-164">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-164">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-165">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-165">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="c656f-166">`{otherPagesTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-166">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-167">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-167">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-168">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-168">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-169">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-169">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-170">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-170">Rely on routing to select the correct route.</span></span>

Razor<span data-ttu-id="c656f-171"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-171"> Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c656f-172">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="c656f-172">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="c656f-173">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-173">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="c656f-176">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-176">Add an app model convention to all pages</span></span>

<span data-ttu-id="c656f-177">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-177">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="c656f-178">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="c656f-178">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="c656f-179">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="c656f-179">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="c656f-180">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-180">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="c656f-181">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="c656f-181">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="c656f-182">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-182">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-183">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-183">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="c656f-184">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-184">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="c656f-186">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-186">Add a handler model convention to all pages</span></span>

<span data-ttu-id="c656f-187">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-187">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-188">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-188">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="c656f-189">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-189">Page route action conventions</span></span>

<span data-ttu-id="c656f-190">衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-190">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="c656f-191">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-191">Folder route model convention</span></span>

<span data-ttu-id="c656f-192">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-192">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="c656f-193">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-193">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="c656f-194"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-194">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-195">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-195">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-196">如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-196">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-197">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-197">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-198">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-198">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-199">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-199">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="c656f-202">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-202">Page route model convention</span></span>

<span data-ttu-id="c656f-203">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-203">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-204">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-204">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="c656f-205"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-205">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-206">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-206">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-207">如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-207">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-208">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-208">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-209">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-209">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-210">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-210">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="c656f-213">使用參數轉換器來自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="c656f-213">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="c656f-214">ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。</span><span class="sxs-lookup"><span data-stu-id="c656f-214">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="c656f-215">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="c656f-215">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="c656f-216">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="c656f-216">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="c656f-217">`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。</span><span class="sxs-lookup"><span data-stu-id="c656f-217">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="c656f-218">例如， Razor 位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的分頁檔案會將其路由從重新寫入 `/SubscriptionManagement/ViewAll` 至 `/subscription-management/view-all` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-218">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="c656f-219">`PageRouteTransformerConvention`只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。</span><span class="sxs-lookup"><span data-stu-id="c656f-219">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="c656f-220">它不會轉換以指示詞新增的路由區段 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-220">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="c656f-221">慣例也不會轉換所加入的路由 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 。</span><span class="sxs-lookup"><span data-stu-id="c656f-221">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="c656f-222">`PageRouteTransformerConvention`會註冊為中的選項 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="c656f-222">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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
```

[!code-csharp[](~/mvc/controllers/routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

[!INCLUDE[](~/includes/regex.md)]

## <a name="configure-a-page-route"></a><span data-ttu-id="c656f-223">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="c656f-223">Configure a page route</span></span>

<span data-ttu-id="c656f-224">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-224">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="c656f-225">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-225">Generated links to the page use your specified route.</span></span> <span data-ttu-id="c656f-226">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-226">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="c656f-227">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-227">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="c656f-228">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="c656f-228">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="c656f-229">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-229">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="c656f-230">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-230">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="c656f-231">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-231">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="c656f-234">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-234">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="c656f-235">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="c656f-235">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="c656f-238">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-238">Page model action conventions</span></span>

<span data-ttu-id="c656f-239">執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-239">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="c656f-240">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="c656f-240">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="c656f-241">針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：</span><span class="sxs-lookup"><span data-stu-id="c656f-241">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="c656f-242">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-242">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="c656f-243">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-243">**Folder app model convention**</span></span>

<span data-ttu-id="c656f-244">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-244">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="c656f-245">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-245">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="c656f-246">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-246">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="c656f-248">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-248">**Page app model convention**</span></span>

<span data-ttu-id="c656f-249">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-249">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-250">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-250">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="c656f-251">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-251">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="c656f-253">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="c656f-253">**Configure a filter**</span></span>

<span data-ttu-id="c656f-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。</span><span class="sxs-lookup"><span data-stu-id="c656f-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="c656f-255">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="c656f-255">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="c656f-256">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="c656f-256">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="c656f-257">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-257">If the condition passes, a header is added.</span></span> <span data-ttu-id="c656f-258">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="c656f-258">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="c656f-259">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-259">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="c656f-260">由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-260">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="c656f-261">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-261">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="c656f-263">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="c656f-263">**Configure a filter factory**</span></span>

<span data-ttu-id="c656f-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="c656f-265">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="c656f-265">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="c656f-266">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-266">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="c656f-267">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-267">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="c656f-269">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="c656f-269">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="c656f-270">頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。</span><span class="sxs-lookup"><span data-stu-id="c656f-270">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="c656f-271">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-271">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="c656f-272">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="c656f-272">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="c656f-273">頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-273">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="c656f-274">如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="c656f-274">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c656f-275">其他資源</span><span class="sxs-lookup"><span data-stu-id="c656f-275">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="c656f-276">瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-276">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="c656f-277">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-277">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="c656f-278">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-278">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="c656f-279">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-279">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="c656f-280">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="c656f-280">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="c656f-281">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="c656f-281">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="c656f-282">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c656f-282">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="c656f-283">狀況</span><span class="sxs-lookup"><span data-stu-id="c656f-283">Scenario</span></span> | <span data-ttu-id="c656f-284">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="c656f-284">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="c656f-285">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-285">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="c656f-286">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="c656f-286">Conventions.Add</span></span><ul><li><span data-ttu-id="c656f-287">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-287">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-288">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-288">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-289">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-289">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="c656f-290">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-290">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="c656f-291">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-291">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="c656f-292">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-292">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-293">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-293">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-294">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="c656f-294">AddPageRoute</span></span></li></ul> | <span data-ttu-id="c656f-295">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-295">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="c656f-296">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-296">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="c656f-297">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-297">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-298">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-298">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-299">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="c656f-299">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="c656f-300">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-300">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

Razor<span data-ttu-id="c656f-301">在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-301"> Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="c656f-302">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="c656f-302">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="c656f-303">路由順序</span><span class="sxs-lookup"><span data-stu-id="c656f-303">Route order</span></span>

<span data-ttu-id="c656f-304">路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。</span><span class="sxs-lookup"><span data-stu-id="c656f-304">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="c656f-305">單</span><span class="sxs-lookup"><span data-stu-id="c656f-305">Order</span></span>            | <span data-ttu-id="c656f-306">行為</span><span class="sxs-lookup"><span data-stu-id="c656f-306">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="c656f-307">-1</span><span class="sxs-lookup"><span data-stu-id="c656f-307">-1</span></span>               | <span data-ttu-id="c656f-308">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-308">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="c656f-309">0</span><span class="sxs-lookup"><span data-stu-id="c656f-309">0</span></span>                | <span data-ttu-id="c656f-310">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="c656f-310">Order isn't specified (default value).</span></span> <span data-ttu-id="c656f-311">Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-311">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="c656f-312">1、2、 &hellip; n</span><span class="sxs-lookup"><span data-stu-id="c656f-312">1, 2, &hellip; n</span></span> | <span data-ttu-id="c656f-313">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="c656f-313">Specifies the route processing order.</span></span> |

<span data-ttu-id="c656f-314">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="c656f-314">Route processing is established by convention:</span></span>

* <span data-ttu-id="c656f-315">路由會依序處理（-1、0、1、2、 &hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="c656f-315">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="c656f-316">當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-316">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="c656f-317">當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。</span><span class="sxs-lookup"><span data-stu-id="c656f-317">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="c656f-318">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="c656f-318">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="c656f-319">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-319">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="c656f-320">如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="c656f-320">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="c656f-321">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="c656f-321">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="c656f-322">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="c656f-322">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="c656f-323">不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。</span><span class="sxs-lookup"><span data-stu-id="c656f-323">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

Razor<span data-ttu-id="c656f-324">頁面路由和 MVC 控制器路由會共用一個執行。</span><span class="sxs-lookup"><span data-stu-id="c656f-324"> Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="c656f-325">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-325">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="c656f-326">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-326">Model conventions</span></span>

<span data-ttu-id="c656f-327">新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-327">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="c656f-328">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-328">Add a route model convention to all pages</span></span>

<span data-ttu-id="c656f-329">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-329">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="c656f-330">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-330">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-331"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="c656f-331">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="c656f-332">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="c656f-332">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="c656f-333">`TheContactPage/{text?}`稍後會在主題中新增的路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-333">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="c656f-334">連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-334">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="c656f-335">`{aboutTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-335">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-336">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-336">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-337">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-337">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="c656f-338">`{otherPagesTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-338">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-339">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-339">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-340">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-340">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-341">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-341">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-342">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-342">Rely on routing to select the correct route.</span></span>

Razor<span data-ttu-id="c656f-343"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-343"> Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c656f-344">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="c656f-344">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="c656f-345">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-345">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="c656f-348">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-348">Add an app model convention to all pages</span></span>

<span data-ttu-id="c656f-349">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-349">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="c656f-350">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="c656f-350">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="c656f-351">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="c656f-351">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="c656f-352">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-352">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="c656f-353">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="c656f-353">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="c656f-354">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-354">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-355">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-355">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="c656f-356">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-356">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="c656f-358">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-358">Add a handler model convention to all pages</span></span>

<span data-ttu-id="c656f-359">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-359">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-360">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-360">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="c656f-361">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-361">Page route action conventions</span></span>

<span data-ttu-id="c656f-362">衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-362">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="c656f-363">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-363">Folder route model convention</span></span>

<span data-ttu-id="c656f-364">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-364">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="c656f-365">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-365">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="c656f-366"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-366">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-367">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-367">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-368">如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-368">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-369">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-369">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-370">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-370">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-371">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-371">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="c656f-374">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-374">Page route model convention</span></span>

<span data-ttu-id="c656f-375">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-375">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-376">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-376">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="c656f-377"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-377">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-378">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-378">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-379">如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-379">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-380">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-380">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-381">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-381">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-382">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-382">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="c656f-385">使用參數轉換器來自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="c656f-385">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="c656f-386">ASP.NET Core 所產生的頁面路由可使用參數轉換器進行自訂。</span><span class="sxs-lookup"><span data-stu-id="c656f-386">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="c656f-387">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="c656f-387">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="c656f-388">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="c656f-388">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="c656f-389">`PageRouteTransformerConvention`頁面路由模型慣例會將參數轉換器套用至應用程式中自動產生之頁面路由的資料夾和檔案名區段。</span><span class="sxs-lookup"><span data-stu-id="c656f-389">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="c656f-390">例如， Razor 位於 */Pages/SubscriptionManagement/ViewAll.cshtml*的分頁檔案會將其路由從重新寫入 `/SubscriptionManagement/ViewAll` 至 `/subscription-management/view-all` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-390">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="c656f-391">`PageRouteTransformerConvention`只會轉換來自 Razor Pages 資料夾和檔案名的頁面路由自動產生的區段。</span><span class="sxs-lookup"><span data-stu-id="c656f-391">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="c656f-392">它不會轉換以指示詞新增的路由區段 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-392">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="c656f-393">慣例也不會轉換所加入的路由 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> 。</span><span class="sxs-lookup"><span data-stu-id="c656f-393">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="c656f-394">`PageRouteTransformerConvention`會註冊為中的選項 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="c656f-394">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="c656f-395">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="c656f-395">Configure a page route</span></span>

<span data-ttu-id="c656f-396">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-396">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="c656f-397">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-397">Generated links to the page use your specified route.</span></span> <span data-ttu-id="c656f-398">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-398">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="c656f-399">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-399">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="c656f-400">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="c656f-400">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="c656f-401">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-401">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="c656f-402">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-402">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="c656f-403">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-403">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="c656f-406">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-406">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="c656f-407">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="c656f-407">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="c656f-410">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-410">Page model action conventions</span></span>

<span data-ttu-id="c656f-411">執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-411">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="c656f-412">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="c656f-412">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="c656f-413">針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：</span><span class="sxs-lookup"><span data-stu-id="c656f-413">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="c656f-414">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-414">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="c656f-415">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-415">**Folder app model convention**</span></span>

<span data-ttu-id="c656f-416">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-416">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="c656f-417">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-417">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="c656f-418">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-418">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="c656f-420">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-420">**Page app model convention**</span></span>

<span data-ttu-id="c656f-421">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-421">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-422">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-422">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="c656f-423">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-423">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="c656f-425">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="c656f-425">**Configure a filter**</span></span>

<span data-ttu-id="c656f-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。</span><span class="sxs-lookup"><span data-stu-id="c656f-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="c656f-427">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="c656f-427">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="c656f-428">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="c656f-428">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="c656f-429">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-429">If the condition passes, a header is added.</span></span> <span data-ttu-id="c656f-430">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="c656f-430">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="c656f-431">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-431">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="c656f-432">由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-432">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="c656f-433">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-433">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="c656f-435">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="c656f-435">**Configure a filter factory**</span></span>

<span data-ttu-id="c656f-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="c656f-437">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="c656f-437">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="c656f-438">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-438">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="c656f-439">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-439">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="c656f-441">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="c656f-441">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="c656f-442">頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。</span><span class="sxs-lookup"><span data-stu-id="c656f-442">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="c656f-443">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-443">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="c656f-444">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="c656f-444">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="c656f-445">頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-445">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="c656f-446">如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="c656f-446">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c656f-447">其他資源</span><span class="sxs-lookup"><span data-stu-id="c656f-447">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c656f-448">瞭解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，在 Razor 頁面應用程式中控制頁面路由、探索和處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-448">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="c656f-449">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-449">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="c656f-450">若要指定頁面路由、加入路由區段，或將參數加入至路由，請使用頁面的指示詞 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-450">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="c656f-451">如需詳細資訊，請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-451">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="c656f-452">有保留字無法做為路由區段或參數名稱使用。</span><span class="sxs-lookup"><span data-stu-id="c656f-452">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="c656f-453">如需詳細資訊，請參閱[路由：保留的路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="c656f-453">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="c656f-454">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c656f-454">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="c656f-455">狀況</span><span class="sxs-lookup"><span data-stu-id="c656f-455">Scenario</span></span> | <span data-ttu-id="c656f-456">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="c656f-456">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="c656f-457">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-457">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="c656f-458">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="c656f-458">Conventions.Add</span></span><ul><li><span data-ttu-id="c656f-459">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-459">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-460">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-460">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-461">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-461">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="c656f-462">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-462">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="c656f-463">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-463">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="c656f-464">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-464">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-465">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-465">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="c656f-466">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="c656f-466">AddPageRoute</span></span></li></ul> | <span data-ttu-id="c656f-467">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-467">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="c656f-468">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-468">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="c656f-469">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-469">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-470">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="c656f-470">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="c656f-471">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="c656f-471">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="c656f-472">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-472">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

Razor<span data-ttu-id="c656f-473">在 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 類別中的服務集合上，會使用擴充方法來新增和設定頁面慣例 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-473"> Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="c656f-474">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="c656f-474">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="c656f-475">路由順序</span><span class="sxs-lookup"><span data-stu-id="c656f-475">Route order</span></span>

<span data-ttu-id="c656f-476">路由會指定 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 進行處理（路由對應）。</span><span class="sxs-lookup"><span data-stu-id="c656f-476">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="c656f-477">單</span><span class="sxs-lookup"><span data-stu-id="c656f-477">Order</span></span>            | <span data-ttu-id="c656f-478">行為</span><span class="sxs-lookup"><span data-stu-id="c656f-478">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="c656f-479">-1</span><span class="sxs-lookup"><span data-stu-id="c656f-479">-1</span></span>               | <span data-ttu-id="c656f-480">在處理其他路由之前，會先處理路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-480">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="c656f-481">0</span><span class="sxs-lookup"><span data-stu-id="c656f-481">0</span></span>                | <span data-ttu-id="c656f-482">未指定順序（預設值）。</span><span class="sxs-lookup"><span data-stu-id="c656f-482">Order isn't specified (default value).</span></span> <span data-ttu-id="c656f-483">Not 指派 `Order` （ `Order = null` ）預設會將路由 `Order` 設為0（零）以進行處理。</span><span class="sxs-lookup"><span data-stu-id="c656f-483">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="c656f-484">1、2、 &hellip; n</span><span class="sxs-lookup"><span data-stu-id="c656f-484">1, 2, &hellip; n</span></span> | <span data-ttu-id="c656f-485">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="c656f-485">Specifies the route processing order.</span></span> |

<span data-ttu-id="c656f-486">路由處理是依照慣例所建立：</span><span class="sxs-lookup"><span data-stu-id="c656f-486">Route processing is established by convention:</span></span>

* <span data-ttu-id="c656f-487">路由會依序處理（-1、0、1、2、 &hellip; n）。</span><span class="sxs-lookup"><span data-stu-id="c656f-487">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="c656f-488">當路由具有相同的時 `Order` ，最特定的路由會先進行比對，後面接著較少的特定路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-488">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="c656f-489">當具有相同 `Order` 和相同數目之參數的路由符合要求 URL 時，路由會依其加入至的順序進行處理 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection> 。</span><span class="sxs-lookup"><span data-stu-id="c656f-489">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="c656f-490">可能的話，請根據已建立的路由處理順序來避免。</span><span class="sxs-lookup"><span data-stu-id="c656f-490">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="c656f-491">一般而言，路由會選取 URL 相符的正確路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-491">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="c656f-492">如果您必須設定路由 `Order` 屬性以正確地路由要求，則應用程式的路由配置可能會使用戶端變得困惑，而難以維護。</span><span class="sxs-lookup"><span data-stu-id="c656f-492">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="c656f-493">搜尋以簡化應用程式的路由配置。</span><span class="sxs-lookup"><span data-stu-id="c656f-493">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="c656f-494">範例應用程式需要明確的路由處理順序，才能使用單一應用程式來示範數個路由案例。</span><span class="sxs-lookup"><span data-stu-id="c656f-494">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="c656f-495">不過，您應該嘗試避免在 `Order` 生產應用程式中設定路由的做法。</span><span class="sxs-lookup"><span data-stu-id="c656f-495">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

Razor<span data-ttu-id="c656f-496">頁面路由和 MVC 控制器路由會共用一個執行。</span><span class="sxs-lookup"><span data-stu-id="c656f-496"> Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="c656f-497">如需有關 MVC 主題中路由順序的資訊，請前往[路由至控制器動作：排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="c656f-497">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="c656f-498">模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-498">Model conventions</span></span>

<span data-ttu-id="c656f-499">新增的委派， <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 以加入適用于頁面的[模型慣例](xref:mvc/controllers/application-model#conventions) Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-499">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="c656f-500">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-500">Add a route model convention to all pages</span></span>

<span data-ttu-id="c656f-501">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面路由模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-501">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="c656f-502">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-502">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-503"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="c656f-503">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="c656f-504">這可確保範例應用程式中的下列路由符合行為：</span><span class="sxs-lookup"><span data-stu-id="c656f-504">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="c656f-505">`TheContactPage/{text?}`稍後會在主題中新增的路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-505">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="c656f-506">連絡人頁面路由的預設順序為 `null` （ `Order = 0` ），因此它會符合 `{globalTemplate?}` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-506">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="c656f-507">`{aboutTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-507">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-508">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-508">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-509">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-509">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="c656f-510">`{otherPagesTemplate?}`稍後會在主題中新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="c656f-510">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="c656f-511">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-511">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="c656f-512">當使用路由參數要求*Pages/OtherPages*資料夾中的任何頁面時（例如， `/OtherPages/Page1/RouteDataValue` ），會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-512">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-513">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-513">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-514">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-514">Rely on routing to select the correct route.</span></span>

Razor<span data-ttu-id="c656f-515"><xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>當 MVC 加入至中的服務集合時，會加入 [頁面] 選項，例如 [加入] `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-515"> Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c656f-516">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="c656f-516">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="c656f-517">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-517">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="c656f-520">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-520">Add an app model convention to all pages</span></span>

<span data-ttu-id="c656f-521">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> 其新增至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面應用程式模型結構期間所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-521">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="c656f-522">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="c656f-522">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="c656f-523">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="c656f-523">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="c656f-524">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-524">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="c656f-525">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="c656f-525">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="c656f-526">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-526">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-527">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-527">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="c656f-528">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-528">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="c656f-530">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="c656f-530">Add a handler model convention to all pages</span></span>

<span data-ttu-id="c656f-531">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> 來建立，並將加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> 至 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> 頁面處理常式模型結構中所套用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="c656f-531">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="c656f-532">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-532">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="c656f-533">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-533">Page route action conventions</span></span>

<span data-ttu-id="c656f-534">衍生自的預設路由模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-534">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="c656f-535">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-535">Folder route model convention</span></span>

<span data-ttu-id="c656f-536">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對指定資料夾下的所有頁面，叫用上的動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-536">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="c656f-537">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-537">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="c656f-538"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-538">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-539">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-539">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-540">如果使用路由參數值要求*Pages/OtherPages*資料夾中的頁面（例如 `/OtherPages/Page1/RouteDataValue` ），則會將 "因此 routedatavalue" 載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["otherPagesTemplate"]` （）， `Order = 2` 因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-540">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-541">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-541">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-542">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-542">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-543">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-543">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="c656f-546">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-546">Page route model convention</span></span>

<span data-ttu-id="c656f-547">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-547">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-548">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-548">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="c656f-549"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="c656f-549">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="c656f-550">這可確保 `{globalTemplate?}` 當提供單一路由值時，的範本（在主題中稍早設定為 `1` ）會獲得第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="c656f-550">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="c656f-551">如果要求的是「關於」頁面，其中的路由參數值為 `/About/RouteDataValue` ，則 "因此 routedatavalue" 會載入至 `RouteData.Values["globalTemplate"]` （ `Order = 1` ），而不是 `RouteData.Values["aboutTemplate"]` （ `Order = 2` ），因為設定了 `Order` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c656f-551">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="c656f-552">盡可能不要設定 `Order` ，這會導致 `Order = 0` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-552">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="c656f-553">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-553">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="c656f-554">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-554">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="c656f-557">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="c656f-557">Configure a page route</span></span>

<span data-ttu-id="c656f-558">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> ，在指定的頁面路徑上設定頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-558">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="c656f-559">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-559">Generated links to the page use your specified route.</span></span> <span data-ttu-id="c656f-560">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="c656f-560">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="c656f-561">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-561">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="c656f-562">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="c656f-562">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="c656f-563">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="c656f-563">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="c656f-564">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="c656f-564">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="c656f-565">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="c656f-565">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="c656f-568">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-568">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="c656f-569">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="c656f-569">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="c656f-572">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="c656f-572">Page model action conventions</span></span>

<span data-ttu-id="c656f-573">執行的預設頁面模型提供者 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> 會叫用慣例，其設計目的是為了提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="c656f-573">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="c656f-574">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="c656f-574">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="c656f-575">針對本節中的範例，範例應用程式 `AddHeaderAttribute` 會使用類別，也就是 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute> ，它會套用回應標頭：</span><span class="sxs-lookup"><span data-stu-id="c656f-575">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="c656f-576">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-576">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="c656f-577">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-577">**Folder app model convention**</span></span>

<span data-ttu-id="c656f-578">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> 建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對指定資料夾下的所有頁面，在實例上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-578">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="c656f-579">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-579">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="c656f-580">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-580">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="c656f-582">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="c656f-582">**Page app model convention**</span></span>

<span data-ttu-id="c656f-583">使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> 來建立並加入 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> ，它會 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> 針對具有指定名稱的頁面，在上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="c656f-583">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="c656f-584">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="c656f-584">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="c656f-585">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-585">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="c656f-587">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="c656f-587">**Configure a filter**</span></span>

<span data-ttu-id="c656f-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定要套用的指定篩選。</span><span class="sxs-lookup"><span data-stu-id="c656f-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="c656f-589">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="c656f-589">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="c656f-590">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="c656f-590">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="c656f-591">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="c656f-591">If the condition passes, a header is added.</span></span> <span data-ttu-id="c656f-592">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="c656f-592">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="c656f-593">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-593">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="c656f-594">由於頁面會忽略動作篩選準則 Razor ， `EmptyFilter` 因此如果路徑不包含，就不會有任何作用 `OtherPages/Page2` 。</span><span class="sxs-lookup"><span data-stu-id="c656f-594">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="c656f-595">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-595">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="c656f-597">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="c656f-597">**Configure a filter factory**</span></span>

<span data-ttu-id="c656f-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>設定指定的 factory，將[篩選](xref:mvc/controllers/filters)套用至所有 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c656f-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="c656f-599">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="c656f-599">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="c656f-600">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="c656f-600">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="c656f-601">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="c656f-601">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="c656f-603">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="c656f-603">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="c656f-604">頁面會忽略 MVC[動作篩選準則](xref:mvc/controllers/filters#action-filters) Razor ，因為 Razor 頁面會使用處理程式方法。</span><span class="sxs-lookup"><span data-stu-id="c656f-604">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="c656f-605">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="c656f-605">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="c656f-606">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="c656f-606">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="c656f-607">頁面篩選準則（ <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ）是適用于頁面的篩選 Razor 。</span><span class="sxs-lookup"><span data-stu-id="c656f-607">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="c656f-608">如需詳細資訊，請參閱[ Razor 頁面的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="c656f-608">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c656f-609">其他資源</span><span class="sxs-lookup"><span data-stu-id="c656f-609">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
