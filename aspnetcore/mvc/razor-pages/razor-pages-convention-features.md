---
title: "ASP.NET Core 中的 Razor 頁面路由和應用程式慣例功能"
author: guardrex
description: "探索路由和應用程式模型提供者慣例功能如何協助您控制頁面路由、探索與處理。"
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a><span data-ttu-id="613f7-103">ASP.NET Core 中的 Razor 頁面路由和應用程式慣例功能</span><span class="sxs-lookup"><span data-stu-id="613f7-103">Razor Pages route and app convention features in ASP.NET Core</span></span>

<span data-ttu-id="613f7-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="613f7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="613f7-105">了解如何使用頁面路由和應用程式模型提供者慣例功能，來控制 Razor 頁面應用程式中的頁面路由、探索與處理。</span><span class="sxs-lookup"><span data-stu-id="613f7-105">Learn how to use page route and app model provider convention features to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="613f7-106">當您需要設定個別頁面的自訂頁面路由時，請使用本主題稍後所述的 [AddPageRoute 慣例](#configure-a-page-route)來設定路由至頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="613f7-107">使用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample)) 來瀏覽本主題中所述的功能。</span><span class="sxs-lookup"><span data-stu-id="613f7-107">Use the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

| <span data-ttu-id="613f7-108">功能</span><span class="sxs-lookup"><span data-stu-id="613f7-108">Features</span></span> | <span data-ttu-id="613f7-109">範例會示範 ...</span><span class="sxs-lookup"><span data-stu-id="613f7-109">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="613f7-110">路由和應用程式模型慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-110">Route and app model conventions</span></span>](#add-route-and-app-model-conventions)<br><br><span data-ttu-id="613f7-111">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="613f7-111">Conventions.Add</span></span><ul><li><span data-ttu-id="613f7-112">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-112">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="613f7-113">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-113">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="613f7-114">將路由範本和標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-114">Adding a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="613f7-115">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-115">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="613f7-116">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-116">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="613f7-117">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-117">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="613f7-118">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="613f7-118">AddPageRoute</span></span></li></ul> | <span data-ttu-id="613f7-119">將路由範本新增至資料夾中的頁面，以及新增至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-119">Adding a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="613f7-120">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-120">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="613f7-121">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-121">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="613f7-122">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="613f7-122">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="613f7-123">ConfigureFilter (篩選類別、Lambda 運算式或篩選 Factory)</span><span class="sxs-lookup"><span data-stu-id="613f7-123">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="613f7-124">將標頭新增至資料夾中的頁面、將標頭新增至單一頁面，以及設定[篩選 Factory](xref:mvc/controllers/filters#ifilterfactory) 將標頭新增至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-124">Adding a header to pages in a folder, adding a header to a single page, and configuring a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="613f7-125">預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="613f7-125">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="613f7-126">取代預設頁面模型提供者，以變更處理常式命名的慣例。</span><span class="sxs-lookup"><span data-stu-id="613f7-126">Replacing the default page model provider to change the conventions for handler naming.</span></span> |

## <a name="add-route-and-app-model-conventions"></a><span data-ttu-id="613f7-127">新增路由和應用程式模型慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-127">Add route and app model conventions</span></span>

<span data-ttu-id="613f7-128">新增 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 的委派，以新增套用至 Razor 頁面的路由和應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="613f7-128">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add route and app model conventions that apply to Razor Pages.</span></span>

<span data-ttu-id="613f7-129">**將路由模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="613f7-129">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="613f7-130">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，並將其新增至路由和頁面模型建構期間所套用的 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。</span><span class="sxs-lookup"><span data-stu-id="613f7-130">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="613f7-131">範例應用程式將 `{globalTemplate?}` 路由範本新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="613f7-131">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="613f7-132">`AttributeRouteModel` 的 `Order` 屬性會設定為 `0` (零)。</span><span class="sxs-lookup"><span data-stu-id="613f7-132">The `Order` property for the `AttributeRouteModel` is set to `0` (zero).</span></span> <span data-ttu-id="613f7-133">這可確保當提供單一路由值時，此範本會指定第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="613f7-133">This ensures that this template is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="613f7-134">例如，範例會新增本主題稍後的 `{aboutTemplate?}` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="613f7-134">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="613f7-135">`{aboutTemplate?}` 範本會指定 `Order` 為 `1`。</span><span class="sxs-lookup"><span data-stu-id="613f7-135">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="613f7-136">在 `/About/RouteDataValue` 上要求 About 頁面時，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 0`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`)。</span><span class="sxs-lookup"><span data-stu-id="613f7-136">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="613f7-137">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="613f7-137">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="613f7-138">在 `localhost:5000/About/GlobalRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-138">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![使用路由區段 GlobalRouteValue 要求 About 頁面。](razor-pages-convention-features/_static/about-page-global-template.png)

<span data-ttu-id="613f7-141">**將應用程式模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="613f7-141">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="613f7-142">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)來建立 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，並將其新增至路由和頁面模型建構期間所套用之 [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) 執行個體的集合。</span><span class="sxs-lookup"><span data-stu-id="613f7-142">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="613f7-143">為了示範這個慣例和本主題稍後的其他慣例，範例應用程式包含了 `AddHeaderAttribute` 類別。</span><span class="sxs-lookup"><span data-stu-id="613f7-143">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="613f7-144">類別建構函式接受 `name` 字串和 `values` 字串陣列。</span><span class="sxs-lookup"><span data-stu-id="613f7-144">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="613f7-145">在其 `OnResultExecuting` 方法中使用這些值來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="613f7-145">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="613f7-146">完整的類別顯示在本主題稍後的[頁面模型動作慣例](#page-model-action-conventions)一節中。</span><span class="sxs-lookup"><span data-stu-id="613f7-146">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="613f7-147">範例應用程式會使用 `AddHeaderAttribute` 類別，將標頭 `GlobalHeader` 新增至應用程式中的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="613f7-147">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="613f7-148">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="613f7-148">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="613f7-149">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-149">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 GlobalHeader。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a><span data-ttu-id="613f7-151">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-151">Page route action conventions</span></span>

<span data-ttu-id="613f7-152">衍生自 [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) 的預設路由模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面路由的擴充點。</span><span class="sxs-lookup"><span data-stu-id="613f7-152">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="613f7-153">**資料夾路由模型慣例**</span><span class="sxs-lookup"><span data-stu-id="613f7-153">**Folder route model convention**</span></span>

<span data-ttu-id="613f7-154">使用 [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="613f7-154">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="613f7-155">範例應用程式會使用 `AddFolderRouteModelConvention`，將 `{otherPagesTemplate?}` 路由範本新增至 *OtherPages* 資料夾中的頁面：</span><span class="sxs-lookup"><span data-stu-id="613f7-155">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="613f7-156">`AttributeRouteModel` 的 `Order` 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="613f7-156">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="613f7-157">這可確保當提供單一路由值時，`{globalTemplate?}` 的範本 (稍早在本主題中設定) 會指定第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="613f7-157">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="613f7-158">如果在 `/OtherPages/Page1/RouteDataValue` 上要求 Page1 頁面，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 0`)，而不是 `RouteData.Values["otherPagesTemplate"]` (`Order = 1`)。</span><span class="sxs-lookup"><span data-stu-id="613f7-158">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="613f7-159">在 `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` 上要求範例的 Page1 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-159">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 OtherPagesRouteValue 的路由區段要求 OtherPages 資料夾中的 Page1。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="613f7-162">**頁面路由模型慣例**</span><span class="sxs-lookup"><span data-stu-id="613f7-162">**Page route model convention**</span></span>

<span data-ttu-id="613f7-163">使用 [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) 來建立並新增 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)，它會針對指定資料夾下的所有頁面在 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="613f7-163">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="613f7-164">範例應用程式會使用 `AddPageRouteModelConvention`，將 `{aboutTemplate?}` 路由範本新增至 About 頁面：</span><span class="sxs-lookup"><span data-stu-id="613f7-164">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="613f7-165">`AttributeRouteModel` 的 `Order` 屬性設定為 `1`。</span><span class="sxs-lookup"><span data-stu-id="613f7-165">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="613f7-166">這可確保當提供單一路由值時，`{globalTemplate?}` 的範本 (稍早在本主題中設定) 會指定第一個路由資料值位置的優先權。</span><span class="sxs-lookup"><span data-stu-id="613f7-166">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="613f7-167">如果在 `/About/RouteDataValue` 上要求 About 頁面，由於設定 `Order` 屬性之故，因此 "RouteDataValue" 會載入至 `RouteData.Values["globalTemplate"]` (`Order = 0`)，而不是 `RouteData.Values["aboutTemplate"]` (`Order = 1`)。</span><span class="sxs-lookup"><span data-stu-id="613f7-167">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="613f7-168">在 `localhost:5000/About/GlobalRouteValue/AboutRouteValue` 上要求範例的 About 頁面，並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-168">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![以 GlobalRouteValue 和 AboutRouteValue 的路由區段要求 About 頁面。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="613f7-171">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="613f7-171">Configure a page route</span></span>

<span data-ttu-id="613f7-172">使用 [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) 以針對指定頁面路徑上的頁面設定路由。</span><span class="sxs-lookup"><span data-stu-id="613f7-172">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="613f7-173">頁面的所產生連結會使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="613f7-173">Generated links to the page use your specified route.</span></span> <span data-ttu-id="613f7-174">`AddPageRoute` 使用 `AddPageRouteModelConvention` 來建立路由。</span><span class="sxs-lookup"><span data-stu-id="613f7-174">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="613f7-175">範例應用程式為 *Contact.cshtml* 建立 `/TheContactPage` 的路由：</span><span class="sxs-lookup"><span data-stu-id="613f7-175">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="613f7-176">Contact 頁面也可以透過其預設路由在 `/Contact` 上連線。</span><span class="sxs-lookup"><span data-stu-id="613f7-176">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="613f7-177">範例應用程式的 Contact 頁面自訂路由允許使用選擇性的 `text` 路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="613f7-177">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="613f7-178">該頁面也會在其 `@page` 指示詞中包含這個選擇性區段，以防訪客在其 `/Contact` 路由上存取頁面：</span><span class="sxs-lookup"><span data-stu-id="613f7-178">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="613f7-179">請注意，呈現頁面中針對 **Contact** 連結所產生的 URL 會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="613f7-179">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式 Contact 連結](razor-pages-convention-features/_static/contact-link.png)

![檢查所呈現 HTML 中的 Contact 連結會指出 href 設定為 '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

<span data-ttu-id="613f7-182">請在其一般路由 `/Contact` 或自訂路由 `/TheContactPage` 上瀏覽 Contact 頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-182">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="613f7-183">如果您提供額外的 `text` 路由區段，該頁面會顯示您提供的 HTML 編碼區段：</span><span class="sxs-lookup"><span data-stu-id="613f7-183">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![在 URL 中提供 'TextValue' 的選擇性 'text' 路由區段的 Edge 瀏覽器範例。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="613f7-186">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-186">Page model action conventions</span></span>

<span data-ttu-id="613f7-187">實作 [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) 的預設頁面模型提供者會叫用慣例，這些慣例的設計目的是要提供設定頁面模型的擴充點。</span><span class="sxs-lookup"><span data-stu-id="613f7-187">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="613f7-188">在建置和修改頁面探索與處理功能時，這些慣例很有用。</span><span class="sxs-lookup"><span data-stu-id="613f7-188">These conventions are useful when building and modifying page discovery and processing features.</span></span>

<span data-ttu-id="613f7-189">對於本節中的範例，範例應用程式會使用 `AddHeaderAttribute` 類別，這是可套用回應標頭的 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)：</span><span class="sxs-lookup"><span data-stu-id="613f7-189">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="613f7-190">透過慣例，此範例示範如何將屬性套用至資料夾中的所有頁面，以及套用至單一頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-190">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="613f7-191">**資料夾應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="613f7-191">**Folder app model convention**</span></span>

<span data-ttu-id="613f7-192">使用 [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) 來建立並新增 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，它會針對指定資料夾下的所有頁面在 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 執行個體上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="613f7-192">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="613f7-193">此範例示範如何將標頭 `OtherPagesHeader` 新增至應用程式的 *OtherPages* 資料夾內的頁面來使用 `AddFolderApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="613f7-193">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="613f7-194">在 `localhost:5000/OtherPages/Page1` 上要求範例的 Page1 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-194">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已新增 OtherPagesHeader。](razor-pages-convention-features/_static/page1-otherpages-header.png)

<span data-ttu-id="613f7-196">**頁面應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="613f7-196">**Page app model convention**</span></span>

<span data-ttu-id="613f7-197">使用 [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) 來建立並新增 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)，它會針對具有指定名稱的頁面在 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) 上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="613f7-197">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="613f7-198">此範例示範如何將標頭 `AboutHeader` 新增至 About 頁面來使用 `AddPageApplicationModelConvention`：</span><span class="sxs-lookup"><span data-stu-id="613f7-198">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="613f7-199">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-199">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增 AboutHeader。](razor-pages-convention-features/_static/about-page-about-header.png)

<span data-ttu-id="613f7-201">**設定篩選條件**</span><span class="sxs-lookup"><span data-stu-id="613f7-201">**Configure a filter**</span></span>

<span data-ttu-id="613f7-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) 可設定要套用的指定篩選條件。</span><span class="sxs-lookup"><span data-stu-id="613f7-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="613f7-203">您可以實作篩選條件類別，但範例應用程式是示範如何在 Lambda 運算式中實作篩選條件，該運算式會在幕後實作為 Factory 以傳回篩選條件：</span><span class="sxs-lookup"><span data-stu-id="613f7-203">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="613f7-204">使用頁面應用程式模型，可針對指向 *OtherPages* 資料夾內 Page2 頁面的區段檢查相對路徑。</span><span class="sxs-lookup"><span data-stu-id="613f7-204">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="613f7-205">如果通過條件，則會新增標頭。</span><span class="sxs-lookup"><span data-stu-id="613f7-205">If the condition passes, a header is added.</span></span> <span data-ttu-id="613f7-206">如果沒有，則會套用 `EmptyFilter`。</span><span class="sxs-lookup"><span data-stu-id="613f7-206">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="613f7-207">`EmptyFilter` 是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="613f7-207">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="613f7-208">由於 Razor 頁面會忽略動作篩選條件，因此如果路徑未包含 `OtherPages/Page2`，則 `EmptyFilter` 不會如預期般生效。</span><span class="sxs-lookup"><span data-stu-id="613f7-208">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="613f7-209">在 `localhost:5000/OtherPages/Page2` 上要求範例的 Page2 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-209">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header 會新增至 Page2 的回應。](razor-pages-convention-features/_static/page2-filter-header.png)

<span data-ttu-id="613f7-211">**設定篩選條件 Factory**</span><span class="sxs-lookup"><span data-stu-id="613f7-211">**Configure a filter factory**</span></span>

<span data-ttu-id="613f7-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) 可設定指定的 Factory，以將[篩選條件](xref:mvc/controllers/filters)套用至所有 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="613f7-213">範例應用程式示範如何以應用程式頁面的兩個值新增標頭 `FilterFactoryHeader` 來使用[篩選條件 Factory](xref:mvc/controllers/filters#ifilterfactory)：</span><span class="sxs-lookup"><span data-stu-id="613f7-213">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="613f7-214">*AddHeaderWithFactory.cs*：</span><span class="sxs-lookup"><span data-stu-id="613f7-214">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="613f7-215">在 `localhost:5000/About` 上要求範例的 About 頁面，並檢查標頭來檢視結果：</span><span class="sxs-lookup"><span data-stu-id="613f7-215">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![About 頁面的回應標頭會顯示已新增兩個 FilterFactoryHeader 標頭。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="613f7-217">取代預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="613f7-217">Replace the default page app model provider</span></span>

<span data-ttu-id="613f7-218">Razor 頁面使用 `IPageApplicationModelProvider` 介面來建立 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。</span><span class="sxs-lookup"><span data-stu-id="613f7-218">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="613f7-219">您可以繼承自預設模型提供者，以提供您自己的實作邏輯來進行處理常式探索和處理。</span><span class="sxs-lookup"><span data-stu-id="613f7-219">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="613f7-220">預設實作 ([參考來源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) 會建立「未具名」和「具名」處理常式命名的慣例，如下所述。</span><span class="sxs-lookup"><span data-stu-id="613f7-220">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="613f7-221">**預設的未具名處理常式方法**</span><span class="sxs-lookup"><span data-stu-id="613f7-221">**Default unnamed handler methods**</span></span>

<span data-ttu-id="613f7-222">HTTP 指令動詞的處理常式方法 (「未具名」處理常式方法) 遵循一個慣例：`On<HTTP verb>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="613f7-222">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="613f7-223">未具名處理常式方法</span><span class="sxs-lookup"><span data-stu-id="613f7-223">Unnamed handler method</span></span>     | <span data-ttu-id="613f7-224">運算</span><span class="sxs-lookup"><span data-stu-id="613f7-224">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="613f7-225">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="613f7-225">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="613f7-226">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="613f7-226">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="613f7-227">處理 DELETE 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-227">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="613f7-228">處理 PUT 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-228">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="613f7-229">處理 PATCH 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-229">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="613f7-230">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="613f7-230">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="613f7-231">**預設的具名處理常式方法**</span><span class="sxs-lookup"><span data-stu-id="613f7-231">**Default named handler methods**</span></span>

<span data-ttu-id="613f7-232">開發人員所提供的處理常式方法 (「具名」處理常式方法) 遵循類似的慣例。</span><span class="sxs-lookup"><span data-stu-id="613f7-232">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="613f7-233">處理常式名稱會出現在 HTTP 指令動詞之後，或 HTTP 指令動詞與 `Async` 之間：`On<HTTP verb><handler name>[Async]` (附加 `Async` 是選擇性項目，但建議用於非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="613f7-233">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="613f7-234">例如，處理訊息的方法可能會採用下表顯示的命名。</span><span class="sxs-lookup"><span data-stu-id="613f7-234">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="613f7-235">範例具名處理常式方法</span><span class="sxs-lookup"><span data-stu-id="613f7-235">Example named handler method</span></span>             | <span data-ttu-id="613f7-236">範例作業</span><span class="sxs-lookup"><span data-stu-id="613f7-236">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="613f7-237">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-237">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="613f7-238">POST (張貼) 訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-238">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="613f7-239">DELETE (刪除) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-239">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="613f7-240">PUT (放置) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-240">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="613f7-241">PPATCH (修補) 訊息&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-241">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="613f7-242">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="613f7-242">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="613f7-243">**自訂處理常式方法名稱**</span><span class="sxs-lookup"><span data-stu-id="613f7-243">**Customize handler method names**</span></span>

<span data-ttu-id="613f7-244">假設您想要變更未具名和具名處理常式方法的命名方式。</span><span class="sxs-lookup"><span data-stu-id="613f7-244">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="613f7-245">另一種命名配置是避免以 "On" 作為方法名稱開頭，並使用第一個文字區段來判斷 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="613f7-245">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="613f7-246">您可以進行其他變更，例如將 DELETE、PUT 和 PATCH 的指令動詞轉換為 POST。</span><span class="sxs-lookup"><span data-stu-id="613f7-246">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="613f7-247">這類配置會提供下表顯示的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="613f7-247">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="613f7-248">處理常式方法</span><span class="sxs-lookup"><span data-stu-id="613f7-248">Handler method</span></span>                       | <span data-ttu-id="613f7-249">運算</span><span class="sxs-lookup"><span data-stu-id="613f7-249">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="613f7-250">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="613f7-250">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="613f7-251">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="613f7-251">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="613f7-252">處理 DELETE 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-252">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="613f7-253">處理 PUT 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-253">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="613f7-254">處理 PATCH 要求&#8224;。</span><span class="sxs-lookup"><span data-stu-id="613f7-254">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="613f7-255">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-255">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="613f7-256">POST (張貼) 訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-256">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="613f7-257">POST (張貼) 要刪除的訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-257">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="613f7-258">POST (張貼) 要放置的訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-258">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="613f7-259">POST (張貼) 要修補的訊息。</span><span class="sxs-lookup"><span data-stu-id="613f7-259">POST a message to patch.</span></span>       |

<span data-ttu-id="613f7-260">&#8224;用於對頁面進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="613f7-260">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="613f7-261">若要建立這種配置，請自 `DefaultPageApplicationModelProvider` 類別繼承並覆寫 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) 方法，以提供自訂邏輯來解析 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="613f7-261">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="613f7-262">範例應用程式示範如何在 `CustomPageApplicationModelProvider` 類別中完成這項處理：</span><span class="sxs-lookup"><span data-stu-id="613f7-262">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="613f7-263">類別的反白顯示項目包括：</span><span class="sxs-lookup"><span data-stu-id="613f7-263">Highlights of the class include:</span></span>

* <span data-ttu-id="613f7-264">類別繼承自 `DefaultPageApplicationModelProvider`。</span><span class="sxs-lookup"><span data-stu-id="613f7-264">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="613f7-265">在建立 `PageHandlerModel` 時，`TryParseHandlerMethod` 處理一個處理常式來判斷 HTTP 指令動詞 (`httpMethod`) 和具名處理常式名稱 (`handlerName`)。</span><span class="sxs-lookup"><span data-stu-id="613f7-265">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="613f7-266">會忽略 `Async` 後置詞 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="613f7-266">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="613f7-267">大小寫用來剖析方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="613f7-267">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="613f7-268">如果方法名稱 (不含 `Async`) 等於 HTTP 指令動詞名稱，則沒有具名處理常式。</span><span class="sxs-lookup"><span data-stu-id="613f7-268">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="613f7-269">`handlerName` 會設定為 `null`，而方法名稱為 `Get`、`Post`、`Delete`、`Put` 或 `Patch`。</span><span class="sxs-lookup"><span data-stu-id="613f7-269">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="613f7-270">如果方法名稱 (不含 `Async`) 長度超過 HTTP 指令動詞名稱，則有具名處理常式。</span><span class="sxs-lookup"><span data-stu-id="613f7-270">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="613f7-271">`handlerName` 已設為 `<method name (less 'Async', if present)>`。</span><span class="sxs-lookup"><span data-stu-id="613f7-271">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="613f7-272">例如，"GetMessage" 和 "GetMessageAsync" 都會產生 "GetMessage" 的處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="613f7-272">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="613f7-273">DELETE、PUT 和 PATCH 的 HTTP 指令動詞會轉換為 POST。</span><span class="sxs-lookup"><span data-stu-id="613f7-273">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="613f7-274">在 `Startup` 類別中註冊 `CustomPageApplicationModelProvider`：</span><span class="sxs-lookup"><span data-stu-id="613f7-274">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="613f7-275">*Index.cshtml.cs* 中的頁面模型顯示一般處理常式方法的命名慣例如何針對應用程式中的頁面變更。</span><span class="sxs-lookup"><span data-stu-id="613f7-275">The page model in *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="613f7-276">移除了與 Razor 頁面搭配使用的一般 "On" 前置詞命名。</span><span class="sxs-lookup"><span data-stu-id="613f7-276">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="613f7-277">初始化頁面狀態的方法現在已命名為 `Get`。</span><span class="sxs-lookup"><span data-stu-id="613f7-277">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="613f7-278">如果您針對任一頁面開啟任何頁面模型，則可以看到在整個應用程式中使用的這個慣例。</span><span class="sxs-lookup"><span data-stu-id="613f7-278">You can see this convention used throughout the app if you open any page model for any of the pages.</span></span>

<span data-ttu-id="613f7-279">每個其他方法會以描述其處理的 HTTP 指令動詞為開頭。</span><span class="sxs-lookup"><span data-stu-id="613f7-279">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="613f7-280">以 `Delete` 開頭的兩個方法通常會視為 DELETE HTTP 指令動詞，但 `TryParseHandlerMethod` 中的邏輯會針對這兩個處理常式將指令動詞明確設定為 POST。</span><span class="sxs-lookup"><span data-stu-id="613f7-280">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="613f7-281">請注意，在 `DeleteAllMessages` 和 `DeleteMessageAsync` 之間，`Async` 是選擇性項目。</span><span class="sxs-lookup"><span data-stu-id="613f7-281">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="613f7-282">它們都是非同步方法，但是您可以選擇使用 `Async` 後置詞或不使用；建議您使用。</span><span class="sxs-lookup"><span data-stu-id="613f7-282">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="613f7-283">`DeleteAllMessages` 在這裡作為示範用途，但建議您將這種方法命名為 `DeleteAllMessagesAsync`。</span><span class="sxs-lookup"><span data-stu-id="613f7-283">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="613f7-284">它不會影響範例實作的處理，但是使用 `Async` 後置詞表明它是非同步方法。</span><span class="sxs-lookup"><span data-stu-id="613f7-284">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="613f7-285">請注意，*Index.cshtml* 中提供的處理常式名稱　符合 `DeleteAllMessages` 和 `DeleteMessageAsync` 處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="613f7-285">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="613f7-286">`TryParseHandlerMethod` 會針對 POST 要求與方法的處理常式比對排除 `DeleteMessageAsync` 中的 `Async`。</span><span class="sxs-lookup"><span data-stu-id="613f7-286">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="613f7-287">`DeleteMessage` 的 `asp-page-handler` 名稱會與處理常式方法 `DeleteMessageAsync` 相符。</span><span class="sxs-lookup"><span data-stu-id="613f7-287">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="613f7-288">MVC 篩選條件和頁面篩選條件 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="613f7-288">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="613f7-289">Razor 頁面會忽略 MVC [動作篩選條件](xref:mvc/controllers/filters#action-filters)，因為 Razor 頁面使用處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="613f7-289">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="613f7-290">其他類型的 MVC 篩選條件可供您使用：[Authorization](xref:mvc/controllers/filters#authorization-filters)、[Exception](xref:mvc/controllers/filters#exception-filters)、[Resource](xref:mvc/controllers/filters#resource-filters) 和 [Result](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="613f7-290">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="613f7-291">如需詳細資訊，請參閱[篩選條件](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="613f7-291">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="613f7-292">頁面篩選條件 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 是可套用至 Razor 頁面的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="613f7-292">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="613f7-293">它會環繞頁面處理常式方法的執行。</span><span class="sxs-lookup"><span data-stu-id="613f7-293">It surrounds the execution of a page handler method.</span></span> <span data-ttu-id="613f7-294">它可讓您在頁面處理常式方法的執行階段處理自訂程式碼。</span><span class="sxs-lookup"><span data-stu-id="613f7-294">It allows you to process custom code at stages of page handler method execution.</span></span> <span data-ttu-id="613f7-295">以下是範例應用程式中的範例：</span><span class="sxs-lookup"><span data-stu-id="613f7-295">Here's an example from the sample app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

<span data-ttu-id="613f7-296">此篩選條件會檢查 "TriggerValue" 的 `globalTemplate` 路由值，並在 "ReplacementValue" 中進行交換。</span><span class="sxs-lookup"><span data-stu-id="613f7-296">This filter checks for a `globalTemplate` route value of "TriggerValue" and swaps in "ReplacementValue".</span></span>

<span data-ttu-id="613f7-297">`ReplaceRouteValueFilter` 屬性可以直接套用至 `PageModel`：</span><span class="sxs-lookup"><span data-stu-id="613f7-297">The `ReplaceRouteValueFilter` attribute can be applied directly to a `PageModel`:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

<span data-ttu-id="613f7-298">從範例應用程式使用 `localhost:5000/OtherPages/Page3/TriggerValue` 要求 Page3 頁面。</span><span class="sxs-lookup"><span data-stu-id="613f7-298">Request the Page3 page from the sample app with at `localhost:5000/OtherPages/Page3/TriggerValue`.</span></span> <span data-ttu-id="613f7-299">請注意篩選條件取代路由值的方式：</span><span class="sxs-lookup"><span data-stu-id="613f7-299">Notice how the filter replaces the route value:</span></span>

![使用 TriggerValue 路由區段的 OtherPages/Page3 要求導致篩選條件以 ReplacementValue 取代路由值。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a><span data-ttu-id="613f7-301">另請參閱</span><span class="sxs-lookup"><span data-stu-id="613f7-301">See also</span></span>

* [<span data-ttu-id="613f7-302">Razor 頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="613f7-302">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
