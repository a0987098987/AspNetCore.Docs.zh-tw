---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: rick-anderson
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 6124554d5f9859179edfb5c545cf0b082369c0c9
ms.sourcegitcommit: 3d07e21868dafc503530ecae2cfa18a7490b58a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642740"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="cceda-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cceda-104">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-104">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="cceda-105">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-105">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="cceda-106">要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。</span><span class="sxs-lookup"><span data-stu-id="cceda-106">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="cceda-107">有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-107">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="cceda-108">保留的單詞不能用作工藝路線段或參數名稱。</span><span class="sxs-lookup"><span data-stu-id="cceda-108">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="cceda-109">有關詳細資訊,請參閱[路由:保留路由名稱](xref:mvc/controllers/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="cceda-109">For more information, see [Routing: Reserved routing names](xref:mvc/controllers/routing#reserved-routing-names).</span></span>

<span data-ttu-id="cceda-110">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cceda-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="cceda-111">狀況</span><span class="sxs-lookup"><span data-stu-id="cceda-111">Scenario</span></span> | <span data-ttu-id="cceda-112">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="cceda-112">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="cceda-113">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-113">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="cceda-114">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="cceda-114">Conventions.Add</span></span><ul><li><span data-ttu-id="cceda-115">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-115">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-116">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-116">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-117">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-117">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="cceda-118">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="cceda-119">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="cceda-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="cceda-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="cceda-123">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="cceda-124">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="cceda-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-127">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="cceda-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="cceda-128">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="cceda-129">使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-129">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="cceda-130">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="cceda-130">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="cceda-131">工藝路線訂單</span><span class="sxs-lookup"><span data-stu-id="cceda-131">Route order</span></span>

<span data-ttu-id="cceda-132">路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。</span><span class="sxs-lookup"><span data-stu-id="cceda-132">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="cceda-133">單</span><span class="sxs-lookup"><span data-stu-id="cceda-133">Order</span></span>            | <span data-ttu-id="cceda-134">行為</span><span class="sxs-lookup"><span data-stu-id="cceda-134">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="cceda-135">-1</span><span class="sxs-lookup"><span data-stu-id="cceda-135">-1</span></span>               | <span data-ttu-id="cceda-136">在處理其他路由之前,將處理該路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-136">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="cceda-137">0</span><span class="sxs-lookup"><span data-stu-id="cceda-137">0</span></span>                | <span data-ttu-id="cceda-138">未指定訂單(預設值)。</span><span class="sxs-lookup"><span data-stu-id="cceda-138">Order isn't specified (default value).</span></span> <span data-ttu-id="cceda-139">不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-139">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="cceda-140">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="cceda-140">1, 2, &hellip; n</span></span> | <span data-ttu-id="cceda-141">指定工藝路線處理順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-141">Specifies the route processing order.</span></span> |

<span data-ttu-id="cceda-142">路由處理依慣例建立:</span><span class="sxs-lookup"><span data-stu-id="cceda-142">Route processing is established by convention:</span></span>

* <span data-ttu-id="cceda-143">路由按順序處理(-1、0、1、2、n)。 &hellip;</span><span class="sxs-lookup"><span data-stu-id="cceda-143">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="cceda-144">當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-144">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="cceda-145">當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。</span><span class="sxs-lookup"><span data-stu-id="cceda-145">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="cceda-146">如果可能,請根據已建立的工藝路線處理順序避免。</span><span class="sxs-lookup"><span data-stu-id="cceda-146">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="cceda-147">通常,路由選擇具有 URL 匹配的正確路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-147">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="cceda-148">如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。</span><span class="sxs-lookup"><span data-stu-id="cceda-148">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="cceda-149">尋求簡化應用程式的路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-149">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="cceda-150">示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-150">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="cceda-151">但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。</span><span class="sxs-lookup"><span data-stu-id="cceda-151">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="cceda-152">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="cceda-152">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="cceda-153">MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-153">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="cceda-154">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-154">Model conventions</span></span>

<span data-ttu-id="cceda-155">添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。</span><span class="sxs-lookup"><span data-stu-id="cceda-155">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="cceda-156">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-156">Add a route model convention to all pages</span></span>

<span data-ttu-id="cceda-157">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-157">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="cceda-158">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-158">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-159"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="cceda-159">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="cceda-160">這可確保範例的以下路由符合行為:</span><span class="sxs-lookup"><span data-stu-id="cceda-160">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="cceda-161">主題稍後將添加`TheContactPage/{text?}`的路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-161">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="cceda-162">連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。</span><span class="sxs-lookup"><span data-stu-id="cceda-162">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="cceda-163">主題`{aboutTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-163">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-164">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-164">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-165">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-165">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="cceda-166">主題`{otherPagesTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-166">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-167">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-167">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-168">當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-168">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-169">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-169">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-170">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-170">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-171">當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。</span><span class="sxs-lookup"><span data-stu-id="cceda-171">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cceda-172">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="cceda-172">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="cceda-173">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-173">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="cceda-176">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-176">Add an app model convention to all pages</span></span>

<span data-ttu-id="cceda-177">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-177">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="cceda-178">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="cceda-178">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="cceda-179">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="cceda-179">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="cceda-180">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-180">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="cceda-181">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="cceda-181">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="cceda-182">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-182">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-183">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-183">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="cceda-184">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-184">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="cceda-186">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-186">Add a handler model convention to all pages</span></span>

<span data-ttu-id="cceda-187">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-187">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-188">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-188">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="cceda-189">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-189">Page route action conventions</span></span>

<span data-ttu-id="cceda-190">派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。</span><span class="sxs-lookup"><span data-stu-id="cceda-190">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="cceda-191">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-191">Folder route model convention</span></span>

<span data-ttu-id="cceda-192">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-192">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="cceda-193">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-193">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="cceda-194"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-194">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-195">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-195">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-196">如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-196">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-197">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-197">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-198">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-198">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-199">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-199">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="cceda-202">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-202">Page route model convention</span></span>

<span data-ttu-id="cceda-203">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-203">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-204">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-204">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="cceda-205"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-205">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-206">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-206">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-207">如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-207">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-208">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-208">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-209">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-209">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-210">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-210">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="cceda-213">使用參數轉換器自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="cceda-213">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="cceda-214">ASP.NET核心生成的頁面路由可以使用參數變壓器進行自定義。</span><span class="sxs-lookup"><span data-stu-id="cceda-214">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="cceda-215">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="cceda-215">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="cceda-216">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="cceda-216">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="cceda-217">`PageRouteTransformerConvention`頁面路由模型約定將參數轉換器應用於應用中自動生成的頁面路由的資料夾和檔名段。</span><span class="sxs-lookup"><span data-stu-id="cceda-217">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="cceda-218">例如,剃刀頁面檔在 */Pages/訂閱管理/ViewAll.cshtml*將有其路`/SubscriptionManagement/ViewAll`由從`/subscription-management/view-all`重寫到 。</span><span class="sxs-lookup"><span data-stu-id="cceda-218">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="cceda-219">`PageRouteTransformerConvention`僅轉換來自 Razor Pages 資料夾和檔名的頁面路由的自動生成的段。</span><span class="sxs-lookup"><span data-stu-id="cceda-219">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="cceda-220">它不會轉換隨指令添加的`@page`路由段。</span><span class="sxs-lookup"><span data-stu-id="cceda-220">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="cceda-221">約定也不會轉換 添加<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-221">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="cceda-222">在`PageRouteTransformerConvention``Startup.ConfigureServices`中 註冊為選項:</span><span class="sxs-lookup"><span data-stu-id="cceda-222">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="cceda-223">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="cceda-223">Configure a page route</span></span>

<span data-ttu-id="cceda-224">用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-224">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="cceda-225">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-225">Generated links to the page use your specified route.</span></span> <span data-ttu-id="cceda-226">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-226">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="cceda-227">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-227">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="cceda-228">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="cceda-228">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="cceda-229">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-229">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="cceda-230">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-230">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="cceda-231">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-231">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="cceda-234">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-234">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="cceda-235">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="cceda-235">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="cceda-238">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-238">Page model action conventions</span></span>

<span data-ttu-id="cceda-239">實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-239">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="cceda-240">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="cceda-240">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="cceda-241">對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:</span><span class="sxs-lookup"><span data-stu-id="cceda-241">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="cceda-242">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-242">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="cceda-243">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-243">**Folder app model convention**</span></span>

<span data-ttu-id="cceda-244">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-244">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="cceda-245">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-245">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="cceda-246">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-246">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="cceda-248">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-248">**Page app model convention**</span></span>

<span data-ttu-id="cceda-249">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-249">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-250">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-250">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="cceda-251">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-251">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="cceda-253">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="cceda-253">**Configure a filter**</span></span>

<span data-ttu-id="cceda-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="cceda-255">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="cceda-255">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="cceda-256">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cceda-256">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="cceda-257">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-257">If the condition passes, a header is added.</span></span> <span data-ttu-id="cceda-258">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="cceda-258">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="cceda-259">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-259">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="cceda-260">由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。</span><span class="sxs-lookup"><span data-stu-id="cceda-260">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="cceda-261">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-261">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="cceda-263">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="cceda-263">**Configure a filter factory**</span></span>

<span data-ttu-id="cceda-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="cceda-265">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="cceda-265">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="cceda-266">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="cceda-266">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="cceda-267">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-267">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="cceda-269">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="cceda-269">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="cceda-270">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="cceda-270">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="cceda-271">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-271">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="cceda-272">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="cceda-272">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="cceda-273">頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-273">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="cceda-274">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="cceda-274">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cceda-275">其他資源</span><span class="sxs-lookup"><span data-stu-id="cceda-275">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cceda-276">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-276">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="cceda-277">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-277">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="cceda-278">要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。</span><span class="sxs-lookup"><span data-stu-id="cceda-278">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="cceda-279">有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-279">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="cceda-280">保留的單詞不能用作工藝路線段或參數名稱。</span><span class="sxs-lookup"><span data-stu-id="cceda-280">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="cceda-281">有關詳細資訊,請參閱[路由:保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="cceda-281">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="cceda-282">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cceda-282">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="cceda-283">狀況</span><span class="sxs-lookup"><span data-stu-id="cceda-283">Scenario</span></span> | <span data-ttu-id="cceda-284">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="cceda-284">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="cceda-285">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-285">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="cceda-286">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="cceda-286">Conventions.Add</span></span><ul><li><span data-ttu-id="cceda-287">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-287">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-288">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-288">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-289">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-289">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="cceda-290">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-290">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="cceda-291">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-291">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="cceda-292">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-292">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-293">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-293">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-294">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="cceda-294">AddPageRoute</span></span></li></ul> | <span data-ttu-id="cceda-295">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-295">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="cceda-296">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-296">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="cceda-297">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-297">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-298">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-298">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-299">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="cceda-299">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="cceda-300">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-300">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="cceda-301">使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-301">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="cceda-302">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="cceda-302">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="cceda-303">工藝路線訂單</span><span class="sxs-lookup"><span data-stu-id="cceda-303">Route order</span></span>

<span data-ttu-id="cceda-304">路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。</span><span class="sxs-lookup"><span data-stu-id="cceda-304">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="cceda-305">單</span><span class="sxs-lookup"><span data-stu-id="cceda-305">Order</span></span>            | <span data-ttu-id="cceda-306">行為</span><span class="sxs-lookup"><span data-stu-id="cceda-306">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="cceda-307">-1</span><span class="sxs-lookup"><span data-stu-id="cceda-307">-1</span></span>               | <span data-ttu-id="cceda-308">在處理其他路由之前,將處理該路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-308">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="cceda-309">0</span><span class="sxs-lookup"><span data-stu-id="cceda-309">0</span></span>                | <span data-ttu-id="cceda-310">未指定訂單(預設值)。</span><span class="sxs-lookup"><span data-stu-id="cceda-310">Order isn't specified (default value).</span></span> <span data-ttu-id="cceda-311">不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-311">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="cceda-312">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="cceda-312">1, 2, &hellip; n</span></span> | <span data-ttu-id="cceda-313">指定工藝路線處理順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-313">Specifies the route processing order.</span></span> |

<span data-ttu-id="cceda-314">路由處理依慣例建立:</span><span class="sxs-lookup"><span data-stu-id="cceda-314">Route processing is established by convention:</span></span>

* <span data-ttu-id="cceda-315">路由按順序處理(-1、0、1、2、n)。 &hellip;</span><span class="sxs-lookup"><span data-stu-id="cceda-315">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="cceda-316">當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-316">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="cceda-317">當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。</span><span class="sxs-lookup"><span data-stu-id="cceda-317">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="cceda-318">如果可能,請根據已建立的工藝路線處理順序避免。</span><span class="sxs-lookup"><span data-stu-id="cceda-318">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="cceda-319">通常,路由選擇具有 URL 匹配的正確路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-319">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="cceda-320">如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。</span><span class="sxs-lookup"><span data-stu-id="cceda-320">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="cceda-321">尋求簡化應用程式的路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-321">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="cceda-322">示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-322">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="cceda-323">但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。</span><span class="sxs-lookup"><span data-stu-id="cceda-323">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="cceda-324">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="cceda-324">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="cceda-325">MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-325">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="cceda-326">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-326">Model conventions</span></span>

<span data-ttu-id="cceda-327">添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。</span><span class="sxs-lookup"><span data-stu-id="cceda-327">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="cceda-328">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-328">Add a route model convention to all pages</span></span>

<span data-ttu-id="cceda-329">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-329">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="cceda-330">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-330">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-331"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="cceda-331">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="cceda-332">這可確保範例的以下路由符合行為:</span><span class="sxs-lookup"><span data-stu-id="cceda-332">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="cceda-333">主題稍後將添加`TheContactPage/{text?}`的路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-333">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="cceda-334">連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。</span><span class="sxs-lookup"><span data-stu-id="cceda-334">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="cceda-335">主題`{aboutTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-335">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-336">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-336">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-337">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-337">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="cceda-338">主題`{otherPagesTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-338">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-339">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-339">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-340">當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-340">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-341">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-341">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-342">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-342">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-343">當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。</span><span class="sxs-lookup"><span data-stu-id="cceda-343">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cceda-344">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="cceda-344">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="cceda-345">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-345">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="cceda-348">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-348">Add an app model convention to all pages</span></span>

<span data-ttu-id="cceda-349">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-349">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="cceda-350">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="cceda-350">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="cceda-351">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="cceda-351">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="cceda-352">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-352">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="cceda-353">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="cceda-353">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="cceda-354">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-354">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-355">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-355">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="cceda-356">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-356">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="cceda-358">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-358">Add a handler model convention to all pages</span></span>

<span data-ttu-id="cceda-359">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-359">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-360">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-360">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="cceda-361">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-361">Page route action conventions</span></span>

<span data-ttu-id="cceda-362">派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。</span><span class="sxs-lookup"><span data-stu-id="cceda-362">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="cceda-363">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-363">Folder route model convention</span></span>

<span data-ttu-id="cceda-364">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-364">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="cceda-365">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-365">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="cceda-366"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-366">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-367">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-367">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-368">如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-368">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-369">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-369">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-370">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-370">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-371">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-371">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="cceda-374">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-374">Page route model convention</span></span>

<span data-ttu-id="cceda-375">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-375">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-376">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-376">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="cceda-377"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-377">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-378">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-378">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-379">如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-379">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-380">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-380">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-381">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-381">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-382">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-382">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="cceda-385">使用參數轉換器自訂頁面路由</span><span class="sxs-lookup"><span data-stu-id="cceda-385">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="cceda-386">ASP.NET核心生成的頁面路由可以使用參數變壓器進行自定義。</span><span class="sxs-lookup"><span data-stu-id="cceda-386">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="cceda-387">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="cceda-387">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="cceda-388">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="cceda-388">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="cceda-389">`PageRouteTransformerConvention`頁面路由模型約定將參數轉換器應用於應用中自動生成的頁面路由的資料夾和檔名段。</span><span class="sxs-lookup"><span data-stu-id="cceda-389">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="cceda-390">例如,剃刀頁面檔在 */Pages/訂閱管理/ViewAll.cshtml*將有其路`/SubscriptionManagement/ViewAll`由從`/subscription-management/view-all`重寫到 。</span><span class="sxs-lookup"><span data-stu-id="cceda-390">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="cceda-391">`PageRouteTransformerConvention`僅轉換來自 Razor Pages 資料夾和檔名的頁面路由的自動生成的段。</span><span class="sxs-lookup"><span data-stu-id="cceda-391">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="cceda-392">它不會轉換隨指令添加的`@page`路由段。</span><span class="sxs-lookup"><span data-stu-id="cceda-392">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="cceda-393">約定也不會轉換 添加<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-393">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="cceda-394">在`PageRouteTransformerConvention``Startup.ConfigureServices`中 註冊為選項:</span><span class="sxs-lookup"><span data-stu-id="cceda-394">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="cceda-395">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="cceda-395">Configure a page route</span></span>

<span data-ttu-id="cceda-396">用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-396">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="cceda-397">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-397">Generated links to the page use your specified route.</span></span> <span data-ttu-id="cceda-398">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-398">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="cceda-399">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-399">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="cceda-400">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="cceda-400">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="cceda-401">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-401">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="cceda-402">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-402">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="cceda-403">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-403">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="cceda-406">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-406">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="cceda-407">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="cceda-407">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="cceda-410">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-410">Page model action conventions</span></span>

<span data-ttu-id="cceda-411">實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-411">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="cceda-412">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="cceda-412">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="cceda-413">對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:</span><span class="sxs-lookup"><span data-stu-id="cceda-413">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="cceda-414">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-414">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="cceda-415">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-415">**Folder app model convention**</span></span>

<span data-ttu-id="cceda-416">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-416">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="cceda-417">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-417">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="cceda-418">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-418">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="cceda-420">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-420">**Page app model convention**</span></span>

<span data-ttu-id="cceda-421">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-421">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-422">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-422">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="cceda-423">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-423">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="cceda-425">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="cceda-425">**Configure a filter**</span></span>

<span data-ttu-id="cceda-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="cceda-427">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="cceda-427">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="cceda-428">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cceda-428">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="cceda-429">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-429">If the condition passes, a header is added.</span></span> <span data-ttu-id="cceda-430">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="cceda-430">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="cceda-431">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-431">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="cceda-432">由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。</span><span class="sxs-lookup"><span data-stu-id="cceda-432">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="cceda-433">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-433">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="cceda-435">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="cceda-435">**Configure a filter factory**</span></span>

<span data-ttu-id="cceda-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="cceda-437">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="cceda-437">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="cceda-438">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="cceda-438">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="cceda-439">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-439">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="cceda-441">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="cceda-441">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="cceda-442">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="cceda-442">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="cceda-443">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-443">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="cceda-444">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="cceda-444">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="cceda-445">頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-445">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="cceda-446">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="cceda-446">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cceda-447">其他資源</span><span class="sxs-lookup"><span data-stu-id="cceda-447">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="cceda-448">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-448">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="cceda-449">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-449">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="cceda-450">要指定頁面路由、添加工藝路線段或向工藝路線添加參數,請使用頁面`@page`的指令。</span><span class="sxs-lookup"><span data-stu-id="cceda-450">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="cceda-451">有關詳細資訊,請參閱[自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-451">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="cceda-452">保留的單詞不能用作工藝路線段或參數名稱。</span><span class="sxs-lookup"><span data-stu-id="cceda-452">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="cceda-453">有關詳細資訊,請參閱[路由:保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="cceda-453">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="cceda-454">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cceda-454">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="cceda-455">狀況</span><span class="sxs-lookup"><span data-stu-id="cceda-455">Scenario</span></span> | <span data-ttu-id="cceda-456">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="cceda-456">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="cceda-457">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-457">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="cceda-458">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="cceda-458">Conventions.Add</span></span><ul><li><span data-ttu-id="cceda-459">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-459">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-460">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-460">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-461">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-461">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="cceda-462">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-462">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="cceda-463">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-463">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="cceda-464">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-464">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-465">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-465">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="cceda-466">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="cceda-466">AddPageRoute</span></span></li></ul> | <span data-ttu-id="cceda-467">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-467">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="cceda-468">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-468">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="cceda-469">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-469">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-470">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="cceda-470">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="cceda-471">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="cceda-471">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="cceda-472">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-472">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="cceda-473">使用<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>擴充方法<xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup`在類中的服務集合上添加和配置 Razor Pages 約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-473">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="cceda-474">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="cceda-474">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="cceda-475">工藝路線訂單</span><span class="sxs-lookup"><span data-stu-id="cceda-475">Route order</span></span>

<span data-ttu-id="cceda-476">路由指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>用於處理的 指定 (路由匹配)。</span><span class="sxs-lookup"><span data-stu-id="cceda-476">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="cceda-477">單</span><span class="sxs-lookup"><span data-stu-id="cceda-477">Order</span></span>            | <span data-ttu-id="cceda-478">行為</span><span class="sxs-lookup"><span data-stu-id="cceda-478">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="cceda-479">-1</span><span class="sxs-lookup"><span data-stu-id="cceda-479">-1</span></span>               | <span data-ttu-id="cceda-480">在處理其他路由之前,將處理該路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-480">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="cceda-481">0</span><span class="sxs-lookup"><span data-stu-id="cceda-481">0</span></span>                | <span data-ttu-id="cceda-482">未指定訂單(預設值)。</span><span class="sxs-lookup"><span data-stu-id="cceda-482">Order isn't specified (default value).</span></span> <span data-ttu-id="cceda-483">不分配`Order``Order = null`( )`Order`預設路由為 0 (零) 進行處理。</span><span class="sxs-lookup"><span data-stu-id="cceda-483">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="cceda-484">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="cceda-484">1, 2, &hellip; n</span></span> | <span data-ttu-id="cceda-485">指定工藝路線處理順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-485">Specifies the route processing order.</span></span> |

<span data-ttu-id="cceda-486">路由處理依慣例建立:</span><span class="sxs-lookup"><span data-stu-id="cceda-486">Route processing is established by convention:</span></span>

* <span data-ttu-id="cceda-487">路由按順序處理(-1、0、1、2、n)。 &hellip;</span><span class="sxs-lookup"><span data-stu-id="cceda-487">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="cceda-488">當路由相同`Order`時,最具體的路由首先匹配,然後是不太具體的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-488">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="cceda-489">當具有相同`Order`與相同的參數的路由與請求網址, 則會將設定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>此選項 。</span><span class="sxs-lookup"><span data-stu-id="cceda-489">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="cceda-490">如果可能,請根據已建立的工藝路線處理順序避免。</span><span class="sxs-lookup"><span data-stu-id="cceda-490">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="cceda-491">通常,路由選擇具有 URL 匹配的正確路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-491">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="cceda-492">如果必須正確設置路由`Order`屬性以路由請求,則應用的路由方案可能對用戶端造成混淆,並且難以維護。</span><span class="sxs-lookup"><span data-stu-id="cceda-492">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="cceda-493">尋求簡化應用程式的路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-493">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="cceda-494">示例應用需要顯式路由處理順序來演示使用單個應用的多個路由方案。</span><span class="sxs-lookup"><span data-stu-id="cceda-494">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="cceda-495">但是,您應該嘗試避免在生產應用中設置工藝路線`Order`的做法。</span><span class="sxs-lookup"><span data-stu-id="cceda-495">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="cceda-496">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="cceda-496">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="cceda-497">MVC 主題中有關路由順序的資訊可在[路由到控制器操作:訂購屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="cceda-497">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="cceda-498">模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-498">Model conventions</span></span>

<span data-ttu-id="cceda-499">添加用於<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>添加應用於 Razor 頁面的[模型約定](xref:mvc/controllers/application-model#conventions)的委託。</span><span class="sxs-lookup"><span data-stu-id="cceda-499">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="cceda-500">將路由模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-500">Add a route model convention to all pages</span></span>

<span data-ttu-id="cceda-501">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>到在頁面路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-501">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="cceda-502">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-502">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-503"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="cceda-503">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="cceda-504">這可確保範例的以下路由符合行為:</span><span class="sxs-lookup"><span data-stu-id="cceda-504">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="cceda-505">主題稍後將添加`TheContactPage/{text?}`的路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-505">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="cceda-506">連絡人頁路由的預設順序`null`為`Order = 0`( ),`{globalTemplate?}`因此它與 工藝路線範本之前匹配。</span><span class="sxs-lookup"><span data-stu-id="cceda-506">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="cceda-507">主題`{aboutTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-507">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-508">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-508">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-509">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-509">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="cceda-510">主題`{otherPagesTemplate?}`稍後將添加路由範本。</span><span class="sxs-lookup"><span data-stu-id="cceda-510">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="cceda-511">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-511">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="cceda-512">當使用路由參數(例如,RouteDataValue)`/OtherPages/Page1/RouteDataValue`請求 *「頁面/其他頁面*」資料夾中的任何頁面時,`Order`由於設置 屬性,`RouteData.Values["globalTemplate"]`將`Order = 1`載`RouteData.Values["otherPagesTemplate"]`入`Order = 2`到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-512">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-513">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-513">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-514">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-514">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-515">當將 MVC<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>添加`Startup.ConfigureServices`到 中的 服務集合時,將添加 Razor 頁面選項(如添加 )。</span><span class="sxs-lookup"><span data-stu-id="cceda-515">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cceda-516">如需範例，請參閱[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/)。</span><span class="sxs-lookup"><span data-stu-id="cceda-516">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="cceda-517">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-517">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="cceda-520">將應用程式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-520">Add an app model convention to all pages</span></span>

<span data-ttu-id="cceda-521">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>到在頁面應用<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-521">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="cceda-522">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="cceda-522">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="cceda-523">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="cceda-523">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="cceda-524">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-524">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="cceda-525">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="cceda-525">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="cceda-526">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-526">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-527">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-527">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="cceda-528">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-528">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="cceda-530">將處理常式模型慣例新增至所有頁面</span><span class="sxs-lookup"><span data-stu-id="cceda-530">Add a handler model convention to all pages</span></span>

<span data-ttu-id="cceda-531">用於<xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>創建和添加<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention>到在頁面處理程式<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>模型建構期間應用的實例的集合。</span><span class="sxs-lookup"><span data-stu-id="cceda-531">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="cceda-532">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cceda-532">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="cceda-533">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-533">Page route action conventions</span></span>

<span data-ttu-id="cceda-534">派生自<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider>調用的約定的預設路由模型提供者旨在為配置頁面路由提供擴展點。</span><span class="sxs-lookup"><span data-stu-id="cceda-534">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="cceda-535">資料夾路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-535">Folder route model convention</span></span>

<span data-ttu-id="cceda-536">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>指定 資料夾下所有頁面的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-536">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="cceda-537">範例應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-537">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="cceda-538"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-538">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-539">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-539">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-540">如果請求頁面 */其他頁面*資料夾中的頁面具有路由參數值(例如`/OtherPages/Page1/RouteDataValue`),`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["otherPagesTemplate"]``Order = 2``Order`則由於設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-540">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-541">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-541">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-542">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-542">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-543">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-543">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="cceda-546">頁面路由模型慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-546">Page route model convention</span></span>

<span data-ttu-id="cceda-547">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-547">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-548">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-548">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="cceda-549"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="cceda-549">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="cceda-550">這可確保在提供單個路由`{globalTemplate?}`值時,`1`為 (在主題中設置為 ) 的範本為 第一個路由數據值位置授予優先順序。</span><span class="sxs-lookup"><span data-stu-id="cceda-550">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="cceda-551">如果"關於"頁`/About/RouteDataValue`的請求時路由參數值為 ,則由於`RouteData.Values["globalTemplate"]``Order = 1``RouteData.Values["aboutTemplate"]``Order = 2``Order`設置 屬性,將載入到 ( ) 而不是 ( ) 中。</span><span class="sxs-lookup"><span data-stu-id="cceda-551">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="cceda-552">只要可能,不要設置`Order``Order = 0`會導致的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-552">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="cceda-553">依靠路由選擇正確的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-553">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="cceda-554">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-554">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="cceda-557">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="cceda-557">Configure a page route</span></span>

<span data-ttu-id="cceda-558">用於<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>指定的頁面路徑上配置到頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-558">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="cceda-559">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-559">Generated links to the page use your specified route.</span></span> <span data-ttu-id="cceda-560">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="cceda-560">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="cceda-561">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-561">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="cceda-562">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="cceda-562">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="cceda-563">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="cceda-563">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="cceda-564">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="cceda-564">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="cceda-565">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="cceda-565">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="cceda-568">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-568">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="cceda-569">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="cceda-569">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="cceda-572">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="cceda-572">Page model action conventions</span></span>

<span data-ttu-id="cceda-573">實現<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider>的預設頁面模型提供程式調用旨在為配置頁面模型提供擴展點的約定。</span><span class="sxs-lookup"><span data-stu-id="cceda-573">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="cceda-574">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="cceda-574">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="cceda-575">對於本節中的示例,示例應用使用一個`AddHeaderAttribute`類,該類是<xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>應用 回應標頭的 類:</span><span class="sxs-lookup"><span data-stu-id="cceda-575">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="cceda-576">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="cceda-576">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="cceda-577">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-577">**Folder app model convention**</span></span>

<span data-ttu-id="cceda-578">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫指定資料夾下所有頁面的<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>實體管理的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-578">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="cceda-579">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-579">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="cceda-580">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-580">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="cceda-582">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="cceda-582">**Page app model convention**</span></span>

<span data-ttu-id="cceda-583">建立<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>與新增<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>呼叫 以指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel>名稱的頁面上的作業的 。</span><span class="sxs-lookup"><span data-stu-id="cceda-583">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="cceda-584">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="cceda-584">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="cceda-585">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-585">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="cceda-587">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="cceda-587">**Configure a filter**</span></span>

<span data-ttu-id="cceda-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>配置要應用的指定篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="cceda-589">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="cceda-589">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="cceda-590">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="cceda-590">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="cceda-591">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="cceda-591">If the condition passes, a header is added.</span></span> <span data-ttu-id="cceda-592">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="cceda-592">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="cceda-593">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-593">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="cceda-594">由於 Razor Pages 會忽略操作`EmptyFilter`篩選器, 因此若路徑不包含`OtherPages/Page2`,則不起作用 。</span><span class="sxs-lookup"><span data-stu-id="cceda-594">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="cceda-595">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-595">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="cceda-597">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="cceda-597">**Configure a filter factory**</span></span>

<span data-ttu-id="cceda-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>將指定的工廠設定為對所有 Razor 頁面套[用篩選器](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="cceda-599">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="cceda-599">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="cceda-600">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="cceda-600">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="cceda-601">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="cceda-601">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="cceda-603">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="cceda-603">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="cceda-604">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="cceda-604">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="cceda-605">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="cceda-605">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="cceda-606">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="cceda-606">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="cceda-607">頁面篩選器<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>( ) 是應用於剃刀頁面的篩選器。</span><span class="sxs-lookup"><span data-stu-id="cceda-607">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="cceda-608">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="cceda-608">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cceda-609">其他資源</span><span class="sxs-lookup"><span data-stu-id="cceda-609">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
