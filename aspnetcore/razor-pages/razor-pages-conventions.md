---
title: ASP.NET Core 中的 Razor 頁面路由和應用程式慣例
author: guardrex
description: 探索路由和應用程式模型提供者慣例如何協助您控制頁面路由、探索與處理。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/17/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: ea4f785dc8a64b430e312fd122a4d3184b61949e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011858"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="04d80-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="04d80-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="04d80-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="04d80-105">了解如何使用頁面[路由和應用程式模型提供者慣例](xref:mvc/controllers/application-model#conventions)，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="04d80-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="04d80-106">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="04d80-107">若要指定頁面路由、 新增路由區段，或將參數新增至路由，請使用頁面的`@page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="04d80-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="04d80-108">如需詳細資訊，請參閱 <<c0> [ 自訂路由](xref:razor-pages/index#custom-routes)。</span><span class="sxs-lookup"><span data-stu-id="04d80-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="04d80-109">有不能作為路由區段或參數名稱的保留的字。</span><span class="sxs-lookup"><span data-stu-id="04d80-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="04d80-110">如需詳細資訊，請參閱 <<c0> [ 路由： 保留路由名稱](xref:fundamentals/routing#reserved-routing-names)。</span><span class="sxs-lookup"><span data-stu-id="04d80-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="04d80-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="04d80-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range="= aspnetcore-2.0"

| <span data-ttu-id="04d80-112">情節</span><span class="sxs-lookup"><span data-stu-id="04d80-112">Scenario</span></span> | <span data-ttu-id="04d80-113">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="04d80-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="04d80-114">模型慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="04d80-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="04d80-115">Conventions.Add</span></span><ul><li><span data-ttu-id="04d80-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-117">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="04d80-118">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="04d80-119">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="04d80-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="04d80-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="04d80-123">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="04d80-124">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="04d80-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="04d80-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="04d80-127">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="04d80-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="04d80-128">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="04d80-129">預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="04d80-129">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="04d80-130">取代預設頁面模型提供者，以變更處理常式名稱的慣例。</span><span class="sxs-lookup"><span data-stu-id="04d80-130">Replace the default page model provider to change the conventions for handler names.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="04d80-131">情節</span><span class="sxs-lookup"><span data-stu-id="04d80-131">Scenario</span></span> | <span data-ttu-id="04d80-132">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="04d80-132">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="04d80-133">模型慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-133">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="04d80-134">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="04d80-134">Conventions.Add</span></span><ul><li><span data-ttu-id="04d80-135">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-135">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-136">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-136">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="04d80-137">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-137">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="04d80-138">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-138">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="04d80-139">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-139">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="04d80-140">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-140">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-141">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-141">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="04d80-142">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="04d80-142">AddPageRoute</span></span></li></ul> | <span data-ttu-id="04d80-143">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-143">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="04d80-144">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-144">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="04d80-145">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-145">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="04d80-146">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="04d80-146">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="04d80-147">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="04d80-147">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="04d80-148">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-148">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="04d80-149">預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="04d80-149">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="04d80-150">取代預設頁面模型提供者，以變更處理常式名稱的慣例。</span><span class="sxs-lookup"><span data-stu-id="04d80-150">Replace the default page model provider to change the conventions for handler names.</span></span> |

::: moniker-end

<span data-ttu-id="04d80-151">Razor 頁面慣例會使用 `Startup` 類別的服務集合上 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) 的 [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) 擴充方法進行新增和設定。</span><span class="sxs-lookup"><span data-stu-id="04d80-151">Razor Pages conventions are added and configured using the [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) extension method to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) on the service collection in the `Startup` class.</span></span> <span data-ttu-id="04d80-152">稍後在本主題會說明下列慣例範例：</span><span class="sxs-lookup"><span data-stu-id="04d80-152">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="04d80-153">路由順序</span><span class="sxs-lookup"><span data-stu-id="04d80-153">Route order</span></span>

<span data-ttu-id="04d80-154">路由會指定<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*>處理 （路由比對）。</span><span class="sxs-lookup"><span data-stu-id="04d80-154">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="04d80-155">訂單</span><span class="sxs-lookup"><span data-stu-id="04d80-155">Order</span></span>            | <span data-ttu-id="04d80-156">行為</span><span class="sxs-lookup"><span data-stu-id="04d80-156">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="04d80-157">-1</span><span class="sxs-lookup"><span data-stu-id="04d80-157">-1</span></span>               | <span data-ttu-id="04d80-158">在處理其他路由之前，會處理路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-158">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="04d80-159">0</span><span class="sxs-lookup"><span data-stu-id="04d80-159">0</span></span>                | <span data-ttu-id="04d80-160">未指定順序 （預設值）。</span><span class="sxs-lookup"><span data-stu-id="04d80-160">Order isn't specified (default value).</span></span> <span data-ttu-id="04d80-161">未指派`Order`(`Order = null`) 預設路由`Order`設為 0 （零） 進行處理。</span><span class="sxs-lookup"><span data-stu-id="04d80-161">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="04d80-162">1、 2、 &hellip; n</span><span class="sxs-lookup"><span data-stu-id="04d80-162">1, 2, &hellip; n</span></span> | <span data-ttu-id="04d80-163">指定路由處理順序。</span><span class="sxs-lookup"><span data-stu-id="04d80-163">Specifies the route processing order.</span></span> |

<span data-ttu-id="04d80-164">路由處理被藉由慣例：</span><span class="sxs-lookup"><span data-stu-id="04d80-164">Route processing is established by convention:</span></span>

* <span data-ttu-id="04d80-165">路由會循序處理 (-1、 0、 1、 2、 &hellip; n)。</span><span class="sxs-lookup"><span data-stu-id="04d80-165">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="04d80-166">當路由具有相同`Order`、 最明確的路由會比對，第一次後面較不明確的路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-166">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="04d80-167">當具有相同的路由`Order`相同的參數數目符合要求 URL，將它們加入的順序來處理路由<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>。</span><span class="sxs-lookup"><span data-stu-id="04d80-167">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="04d80-168">可能的話，請避免根據已建立的路由的處理順序而定。</span><span class="sxs-lookup"><span data-stu-id="04d80-168">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="04d80-169">一般而言，路由會選取正確的路由，透過 URL 比對。</span><span class="sxs-lookup"><span data-stu-id="04d80-169">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="04d80-170">如果您必須設定路由`Order`屬性來路由要求是否正確，應用程式的路由傳送，進而可能造成混淆的用戶端和容易維護。</span><span class="sxs-lookup"><span data-stu-id="04d80-170">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="04d80-171">若要簡化應用程式的路由傳送，進而搜尋。</span><span class="sxs-lookup"><span data-stu-id="04d80-171">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="04d80-172">範例應用程式需要的處理順序，來示範數個使用單一的應用程式的路由案例的外顯路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-172">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="04d80-173">不過，您應該嘗試避免設定路由的練習`Order`在生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="04d80-173">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="04d80-174">Razor Pages 路由和 MVC 控制器路由共用實作。</span><span class="sxs-lookup"><span data-stu-id="04d80-174">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="04d80-175">在 MVC 主題中的路由順序的詳細資訊位於[路由至控制器動作： 排序屬性路由](xref:mvc/controllers/routing#ordering-attribute-routes)。</span><span class="sxs-lookup"><span data-stu-id="04d80-175">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="04d80-176">模型慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-176">Model conventions</span></span>

<span data-ttu-id="04d80-177">新增 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 的委派，以新增套用至 Razor 頁面的[模型慣例](xref:mvc/controllers/application-model#conventions)。</span><span class="sxs-lookup"><span data-stu-id="04d80-177">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

<span data-ttu-id="04d80-178">**將路由模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="04d80-178">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="04d80-179">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，並將其新增至頁面路由模型建構期間所套用的 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。</span><span class="sxs-lookup"><span data-stu-id="04d80-179">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page route model construction.</span></span>

<span data-ttu-id="04d80-180">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="04d80-180">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="04d80-181"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="04d80-181">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="04d80-182">這可確保下列路由比對範例應用程式的行為：</span><span class="sxs-lookup"><span data-stu-id="04d80-182">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="04d80-183">路由範本`TheContactPage/{text?}`本主題稍後會加入。</span><span class="sxs-lookup"><span data-stu-id="04d80-183">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="04d80-184">Contact 頁面路由會將預設順序`null`(`Order = 0`)，因此它會比對之前`{globalTemplate?}`路由範本。</span><span class="sxs-lookup"><span data-stu-id="04d80-184">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="04d80-185">`{aboutTemplate?}`路由範本新增本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="04d80-185">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="04d80-186">`{aboutTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="04d80-186">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="04d80-187">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 1`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 2`)。</span><span class="sxs-lookup"><span data-stu-id="04d80-187">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="04d80-188">`{otherPagesTemplate?}`路由範本新增本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="04d80-188">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="04d80-189">`{otherPagesTemplate?}` 範本會指定 `Order` 為 `2`。</span><span class="sxs-lookup"><span data-stu-id="04d80-189">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="04d80-190">當任何頁面*頁面/OtherPages*資料夾要求的路由參數 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="04d80-190">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="04d80-191">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="04d80-191">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="04d80-192">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-192">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="04d80-193">將 MVC 新增至 `Startup.ConfigureServices` 中的服務集合時，會新增 Razor 頁面選項，例如新增[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)。</span><span class="sxs-lookup"><span data-stu-id="04d80-193">Razor Pages options, such as adding [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="04d80-194">如需範例，請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/)。</span><span class="sxs-lookup"><span data-stu-id="04d80-194">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="04d80-195">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-195">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-conventions/_static/about-page-global-template.png)

<span data-ttu-id="04d80-198">**將應用程式模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="04d80-198">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="04d80-199">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，並將其新增至頁面模型建構期間所套用之 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。</span><span class="sxs-lookup"><span data-stu-id="04d80-199">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page app model construction.</span></span>

<span data-ttu-id="04d80-200">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="04d80-200">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="04d80-201">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="04d80-201">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="04d80-202">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="04d80-202">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="04d80-203">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="04d80-203">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="04d80-204">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="04d80-204">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="04d80-205">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="04d80-205">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="04d80-206">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-206">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="04d80-208">**將處理常式模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="04d80-208">**Add a handler model convention to all pages**</span></span>

<span data-ttu-id="04d80-209">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention)，並將其新增至頁面模型建構期間所套用之 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。</span><span class="sxs-lookup"><span data-stu-id="04d80-209">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page handler model construction.</span></span>

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

<span data-ttu-id="04d80-210">`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="04d80-210">`Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="04d80-211">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-211">Page route action conventions</span></span>

<span data-ttu-id="04d80-212">衍生自 [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) 的預設路由模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="04d80-212">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="04d80-213">**資料夾路由模型慣例**</span><span class="sxs-lookup"><span data-stu-id="04d80-213">**Folder route model convention**</span></span>

<span data-ttu-id="04d80-214">使用 [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="04d80-214">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="04d80-215">範例應用程式會使用 `AddFolderRouteModelConvention`，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="04d80-215">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="04d80-216"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="04d80-216">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="04d80-217">這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。</span><span class="sxs-lookup"><span data-stu-id="04d80-217">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="04d80-218">如果中的頁面*頁/OtherPages*資料夾要求的路由參數值 (例如`/OtherPages/Page1/RouteDataValue`)，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 2`)由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="04d80-218">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="04d80-219">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="04d80-219">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="04d80-220">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-220">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="04d80-221">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-221">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="04d80-224">**頁面路由模型慣例**</span><span class="sxs-lookup"><span data-stu-id="04d80-224">**Page route model convention**</span></span>

<span data-ttu-id="04d80-225">使用 [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="04d80-225">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="04d80-226">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="04d80-226">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="04d80-227"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> 的 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> 屬性設定為 `2`。</span><span class="sxs-lookup"><span data-stu-id="04d80-227">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="04d80-228">這可確保樣板`{globalTemplate?}`(設定主題中稍早`1`) 的第一個路由資料值的位置，提供單一路由值時，會指定優先權。</span><span class="sxs-lookup"><span data-stu-id="04d80-228">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="04d80-229">如果 [關於] 頁面會要求使用路由參數值在`/About/RouteDataValue`，"因此 RouteDataValue"會載入`RouteData.Values["globalTemplate"]`(`Order = 1`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 2`) 由於設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="04d80-229">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="04d80-230">可能的話，不需要設定`Order`，這會導致`Order = 0`。</span><span class="sxs-lookup"><span data-stu-id="04d80-230">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="04d80-231">依賴路由來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-231">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="04d80-232">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-232">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="04d80-235">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="04d80-235">Configure a page route</span></span>

<span data-ttu-id="04d80-236">使用 [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) 以針對指定頁面路徑上的頁面設定路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-236">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="04d80-237">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-237">Generated links to the page use your specified route.</span></span> <span data-ttu-id="04d80-238">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="04d80-238">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="04d80-239">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="04d80-239">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="04d80-240">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="04d80-240">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="04d80-241">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="04d80-241">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="04d80-242">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="04d80-242">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="04d80-243">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="04d80-243">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-conventions/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="04d80-246">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-246">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="04d80-247">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="04d80-247">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="04d80-250">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-250">Page model action conventions</span></span>

<span data-ttu-id="04d80-251">實作 [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) 的預設頁面模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="04d80-251">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="04d80-252">在建置和修改頁面探索與處理案例時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="04d80-252">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="04d80-253">對於本節中的範例，範例應用程式會使用 `AddHeaderAttribute` 類別，這是可套用回應標頭的 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)：</span><span class="sxs-lookup"><span data-stu-id="04d80-253">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="04d80-254">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="04d80-254">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="04d80-255">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="04d80-255">**Folder app model convention**</span></span>

<span data-ttu-id="04d80-256">使用 [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) 來建立並新增 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，它會針對指定資料夾下的所有頁面在 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 執行個體上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="04d80-256">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="04d80-257">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="04d80-257">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="04d80-258">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-258">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="04d80-260">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="04d80-260">**Page app model convention**</span></span>

<span data-ttu-id="04d80-261">使用[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)來建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)上叫用動作[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)頁面使用指定的名稱。</span><span class="sxs-lookup"><span data-stu-id="04d80-261">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the specified name.</span></span>

<span data-ttu-id="04d80-262">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="04d80-262">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="04d80-263">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-263">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="04d80-265">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="04d80-265">**Configure a filter**</span></span>

<span data-ttu-id="04d80-266">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) 可設定要套用的指定篩選條件。</span><span class="sxs-lookup"><span data-stu-id="04d80-266">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="04d80-267">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="04d80-267">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="04d80-268">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="04d80-268">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="04d80-269">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="04d80-269">If the condition passes, a header is added.</span></span> <span data-ttu-id="04d80-270">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="04d80-270">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="04d80-271">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="04d80-271">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="04d80-272">由於 Razor Pages 會忽略動作篩選條件，因此如果路徑未包含 `OtherPages/Page2`，則 `EmptyFilter` 不會如預期般生效。</span><span class="sxs-lookup"><span data-stu-id="04d80-272">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="04d80-273">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-273">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="04d80-275">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="04d80-275">**Configure a filter factory**</span></span>

<span data-ttu-id="04d80-276">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) 可設定指定的 Factory，以將[篩選條件](xref:mvc/controllers/filters)套用至所有 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="04d80-276">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="04d80-277">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="04d80-277">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="04d80-278">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="04d80-278">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="04d80-279">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="04d80-279">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="04d80-281">取代預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="04d80-281">Replace the default page app model provider</span></span>

<span data-ttu-id="04d80-282">Razor Pages 使用 `IPageApplicationModelProvider` 介面來建立 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。</span><span class="sxs-lookup"><span data-stu-id="04d80-282">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="04d80-283">您可以繼承自預設模型提供者，以提供您自己的實作邏輯來進行處理常式探索和處理。</span><span class="sxs-lookup"><span data-stu-id="04d80-283">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="04d80-284">預設實作 ([參考來源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) 會建立「未具名」和「具名」處理常式命名的慣例，如下所述。</span><span class="sxs-lookup"><span data-stu-id="04d80-284">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="04d80-285">**預設的未具名處理常式方法**</span><span class="sxs-lookup"><span data-stu-id="04d80-285">**Default unnamed handler methods**</span></span>

<span data-ttu-id="04d80-286">HTTP 指令動詞的處理常式方法 (「未具名」處理常式方法) 遵循一個慣例：`On<HTTP verb>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="04d80-286">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="04d80-287">未具名處理常式方法</span><span class="sxs-lookup"><span data-stu-id="04d80-287">Unnamed handler method</span></span>     | <span data-ttu-id="04d80-288">運算</span><span class="sxs-lookup"><span data-stu-id="04d80-288">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="04d80-289">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="04d80-289">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="04d80-290">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="04d80-290">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="04d80-291">處理 DELETE 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-291">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="04d80-292">處理 PUT 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-292">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="04d80-293">處理 PATCH 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-293">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="04d80-294">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="04d80-294">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="04d80-295">**預設的具名處理常式方法**</span><span class="sxs-lookup"><span data-stu-id="04d80-295">**Default named handler methods**</span></span>

<span data-ttu-id="04d80-296">開發人員所提供的處理常式方法 (「具名」處理常式方法) 遵循類似的慣例。</span><span class="sxs-lookup"><span data-stu-id="04d80-296">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="04d80-297">處理常式名稱會出現在 HTTP 指令動詞之後，或 HTTP 指令動詞與 `Async` 之間：`On<HTTP verb><handler name>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="04d80-297">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="04d80-298">例如，處理訊息的方法可能會採用下表顯示的命名。</span><span class="sxs-lookup"><span data-stu-id="04d80-298">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="04d80-299">範例具名處理常式方法</span><span class="sxs-lookup"><span data-stu-id="04d80-299">Example named handler method</span></span>             | <span data-ttu-id="04d80-300">範例作業</span><span class="sxs-lookup"><span data-stu-id="04d80-300">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="04d80-301">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-301">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="04d80-302">POST (張貼) 訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-302">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="04d80-303">DELETE (刪除) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-303">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="04d80-304">PUT (放置) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-304">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="04d80-305">PPATCH (修補) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-305">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="04d80-306">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="04d80-306">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="04d80-307">**自訂處理常式方法名稱**</span><span class="sxs-lookup"><span data-stu-id="04d80-307">**Customize handler method names**</span></span>

<span data-ttu-id="04d80-308">假設您想要變更未具名和具名處理常式方法的命名方式。</span><span class="sxs-lookup"><span data-stu-id="04d80-308">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="04d80-309">另一種命名配置是避免以 "On" 作為方法名稱開頭，並使用第一個文字區段來判斷 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="04d80-309">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="04d80-310">您可以進行其他變更，例如將 DELETE、PUT 和 PATCH 的指令動詞轉換為 POST。</span><span class="sxs-lookup"><span data-stu-id="04d80-310">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="04d80-311">這類配置會提供下表顯示的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="04d80-311">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="04d80-312">處理常式方法</span><span class="sxs-lookup"><span data-stu-id="04d80-312">Handler method</span></span>                       | <span data-ttu-id="04d80-313">運算</span><span class="sxs-lookup"><span data-stu-id="04d80-313">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="04d80-314">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="04d80-314">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="04d80-315">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="04d80-315">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="04d80-316">處理 DELETE 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-316">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="04d80-317">處理 PUT 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-317">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="04d80-318">處理 PATCH 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="04d80-318">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="04d80-319">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-319">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="04d80-320">POST (張貼) 訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-320">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="04d80-321">POST (張貼) 要刪除的訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-321">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="04d80-322">POST (張貼) 要放置的訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-322">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="04d80-323">POST (張貼) 要修補的訊息。</span><span class="sxs-lookup"><span data-stu-id="04d80-323">POST a message to patch.</span></span>       |

<span data-ttu-id="04d80-324">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="04d80-324">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="04d80-325">若要建立這種配置，請自 `DefaultPageApplicationModelProvider` 類別繼承並覆寫 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) 方法，以提供自訂邏輯來解析 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="04d80-325">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="04d80-326">範例應用程式示範如何在 `CustomPageApplicationModelProvider` 類別中完成這項處理：</span><span class="sxs-lookup"><span data-stu-id="04d80-326">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="04d80-327">類別的反白顯示項目包括：</span><span class="sxs-lookup"><span data-stu-id="04d80-327">Highlights of the class include:</span></span>

* <span data-ttu-id="04d80-328">類別繼承自 `DefaultPageApplicationModelProvider`。</span><span class="sxs-lookup"><span data-stu-id="04d80-328">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="04d80-329">在建立 `PageHandlerModel` 時，`TryParseHandlerMethod` 處理一個處理常式來判斷 HTTP 指令動詞 (`httpMethod`) 和具名處理常式名稱 (`handlerName`)。</span><span class="sxs-lookup"><span data-stu-id="04d80-329">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="04d80-330">會忽略 `Async` 後置詞 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="04d80-330">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="04d80-331">大小寫用來剖析方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="04d80-331">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="04d80-332">如果方法名稱 (不含 `Async`) 等於 HTTP 指令動詞名稱，則沒有具名處理常式。</span><span class="sxs-lookup"><span data-stu-id="04d80-332">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="04d80-333">`handlerName` 會設定為 `null`，而方法名稱為 `Get`、`Post`、`Delete`、`Put` 或 `Patch`。</span><span class="sxs-lookup"><span data-stu-id="04d80-333">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="04d80-334">如果方法名稱 (不含 `Async`) 長度超過 HTTP 指令動詞名稱，則有具名處理常式。</span><span class="sxs-lookup"><span data-stu-id="04d80-334">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="04d80-335">`handlerName` 已設為 `<method name (less 'Async', if present)>`。</span><span class="sxs-lookup"><span data-stu-id="04d80-335">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="04d80-336">例如，"GetMessage" 和 "GetMessageAsync" 都會產生 "GetMessage" 的處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="04d80-336">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="04d80-337">DELETE、PUT 和 PATCH 的 HTTP 指令動詞會轉換為 POST。</span><span class="sxs-lookup"><span data-stu-id="04d80-337">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="04d80-338">在 `Startup` 類別中註冊 `CustomPageApplicationModelProvider`：</span><span class="sxs-lookup"><span data-stu-id="04d80-338">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="04d80-339">*Index.cshtml.cs* 中的頁面模型顯示一般處理常式方法的命名慣例如何針對應用程式中的頁面變更。</span><span class="sxs-lookup"><span data-stu-id="04d80-339">The page model in *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="04d80-340">移除了與 Razor Pages 搭配使用的一般 "On" 前置詞命名。</span><span class="sxs-lookup"><span data-stu-id="04d80-340">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="04d80-341">初始化頁面狀態的方法現在已命名為 `Get`。</span><span class="sxs-lookup"><span data-stu-id="04d80-341">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="04d80-342">如果您針對任一頁面開啟任何頁面模型，則可以看到在整個應用程式中使用的這個慣例。</span><span class="sxs-lookup"><span data-stu-id="04d80-342">You can see this convention used throughout the app if you open any page model for any of the pages.</span></span>

<span data-ttu-id="04d80-343">每個其他方法會以描述其處理的 HTTP 指令動詞為開頭。</span><span class="sxs-lookup"><span data-stu-id="04d80-343">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="04d80-344">以 `Delete` 開頭的兩個方法通常會視為 DELETE HTTP 指令動詞，但 `TryParseHandlerMethod` 中的邏輯會針對這兩個處理常式將指令動詞明確設定為 POST。</span><span class="sxs-lookup"><span data-stu-id="04d80-344">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="04d80-345">請注意，在 `DeleteAllMessages` 和 `DeleteMessageAsync` 之間，`Async` 是選擇性項目。</span><span class="sxs-lookup"><span data-stu-id="04d80-345">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="04d80-346">它們都是非同步方法，但是您可以選擇使用 `Async` 後置詞或不使用；建議您使用。</span><span class="sxs-lookup"><span data-stu-id="04d80-346">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="04d80-347">`DeleteAllMessages` 在這裡作為示範用途，但建議您將這種方法命名為 `DeleteAllMessagesAsync`。</span><span class="sxs-lookup"><span data-stu-id="04d80-347">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="04d80-348">它不會影響範例實作的處理，但是使用 `Async` 後置詞表明它是非同步方法。</span><span class="sxs-lookup"><span data-stu-id="04d80-348">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="04d80-349">請注意，*Index.cshtml* 中提供的處理常式名稱　符合 `DeleteAllMessages` 和 `DeleteMessageAsync` 處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="04d80-349">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="04d80-350">`TryParseHandlerMethod` 會針對 POST 要求與方法的處理常式比對排除 `DeleteMessageAsync` 中的 `Async`。</span><span class="sxs-lookup"><span data-stu-id="04d80-350">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="04d80-351">`DeleteMessage` 的 `asp-page-handler` 名稱會與處理常式方法 `DeleteMessageAsync` 相符。</span><span class="sxs-lookup"><span data-stu-id="04d80-351">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="04d80-352">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="04d80-352">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="04d80-353">Razor Pages 會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor Pages 使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="04d80-353">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="04d80-354">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="04d80-354">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="04d80-355">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="04d80-355">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="04d80-356">頁面篩選條件 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 是可套用至 Razor 頁面的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="04d80-356">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="04d80-357">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="04d80-357">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04d80-358">其他資源</span><span class="sxs-lookup"><span data-stu-id="04d80-358">Additional resources</span></span>

* [<span data-ttu-id="04d80-359">Razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="04d80-359">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
