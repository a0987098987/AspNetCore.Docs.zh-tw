---
title: "Razor 頁面路由和應用程式慣例功能 ASP.NET Core"
author: guardrex
description: "探索如何路由和應用程式模型提供者慣例功能有助於控制頁面路由、 探索與處理。"
keywords: "ASP.NET Core、 Razor 頁面、 慣例，AddFolderRouteModelConvention、 AddPageRouteModelConvention、 AddPageRoute、 AddFolderApplicationModelConvention、 AddPageApplicationModelConvention，ConfigureFilter，篩選"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a><span data-ttu-id="ff67c-104">Razor 頁面路由和應用程式慣例功能 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff67c-104">Razor Pages route and app convention features in ASP.NET Core</span></span>

<span data-ttu-id="ff67c-105">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ff67c-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ff67c-106">了解如何控制頁面路由、 探索與處理 Razor 網頁應用程式中的使用頁面路由和應用程式模型提供者慣例的功能。</span><span class="sxs-lookup"><span data-stu-id="ff67c-106">Learn how to use page route and app model provider convention features to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="ff67c-107">當您需要設定自訂頁面路由的個別頁面時，設定路由傳送至含有 pages [AddPageRoute 慣例](#configure-a-page-route)本主題稍後所述。</span><span class="sxs-lookup"><span data-stu-id="ff67c-107">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="ff67c-108">使用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 來瀏覽本主題中所述的功能。</span><span class="sxs-lookup"><span data-stu-id="ff67c-108">Use the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

| <span data-ttu-id="ff67c-109">功能</span><span class="sxs-lookup"><span data-stu-id="ff67c-109">Features</span></span> | <span data-ttu-id="ff67c-110">此範例會示範...</span><span class="sxs-lookup"><span data-stu-id="ff67c-110">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="ff67c-111">路由和應用程式模型慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-111">Route and app model conventions</span></span>](#add-route-and-app-model-conventions)<br><br><span data-ttu-id="ff67c-112">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="ff67c-112">Conventions.Add</span></span><ul><li><span data-ttu-id="ff67c-113">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-113">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="ff67c-114">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-114">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="ff67c-115">加入應用程式的網頁中的路由範本和標頭。</span><span class="sxs-lookup"><span data-stu-id="ff67c-115">Adding a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="ff67c-116">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-116">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="ff67c-117">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-117">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="ff67c-118">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-118">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="ff67c-119">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="ff67c-119">AddPageRoute</span></span></li></ul> | <span data-ttu-id="ff67c-120">資料夾中的頁面，並在單一頁面，請新增路由範本。</span><span class="sxs-lookup"><span data-stu-id="ff67c-120">Adding a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="ff67c-121">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-121">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="ff67c-122">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-122">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="ff67c-123">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="ff67c-123">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="ff67c-124">ConfigureFilter （篩選類別、 lambda 運算式或篩選器 factory）</span><span class="sxs-lookup"><span data-stu-id="ff67c-124">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="ff67c-125">將標頭加入至資料夾中的頁面，將標頭加入至單一的頁面上，及設定[篩選 factory](xref:mvc/controllers/filters#ifilterfactory)將標頭加入至應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-125">Adding a header to pages in a folder, adding a header to a single page, and configuring a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="ff67c-126">預設頁面應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="ff67c-126">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="ff67c-127">取代預設頁面模型提供者，若要變更的處理常式命名慣例。</span><span class="sxs-lookup"><span data-stu-id="ff67c-127">Replacing the default page model provider to change the conventions for handler naming.</span></span> |

## <a name="add-route-and-app-model-conventions"></a><span data-ttu-id="ff67c-128">新增路由和應用程式模型慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-128">Add route and app model conventions</span></span>

<span data-ttu-id="ff67c-129">加入的委派[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)將套用至網頁 Razor 的路由和應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="ff67c-129">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add route and app model conventions that apply to Razor Pages.</span></span>

<span data-ttu-id="ff67c-130">**將路由模型慣例新增至所有頁面**</span><span class="sxs-lookup"><span data-stu-id="ff67c-130">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="ff67c-131">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)集合[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)路由和頁面模型期間所套用的執行個體建構。</span><span class="sxs-lookup"><span data-stu-id="ff67c-131">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="ff67c-132">範例應用程式將`{globalTemplate?}`所有應用程式中頁面的路由範本：</span><span class="sxs-lookup"><span data-stu-id="ff67c-132">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="ff67c-133">`Order`屬性`AttributeRouteModel`設`0`（零）。</span><span class="sxs-lookup"><span data-stu-id="ff67c-133">The `Order` property for the `AttributeRouteModel` is set to `0` (zero).</span></span> <span data-ttu-id="ff67c-134">這可確保此範本指定的第一個路由資料值位置的優先順序時提供的單一路由值。</span><span class="sxs-lookup"><span data-stu-id="ff67c-134">This ensures that this template is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="ff67c-135">例如，將範例加入`{aboutTemplate?}`本主題稍後的路由範本。</span><span class="sxs-lookup"><span data-stu-id="ff67c-135">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="ff67c-136">`{aboutTemplate?}`範本提供`Order`的`1`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-136">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="ff67c-137">在 [關於] 頁面要求時`/About/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 1`) 因為設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="ff67c-137">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="ff67c-138">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ff67c-138">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="ff67c-139">要求在範例的相關網頁`localhost:5000/About/GlobalRouteValue`並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="ff67c-139">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![關於頁面要求以 GlobalRouteValue 的路由區段。](razor-pages-convention-features/_static/about-page-global-template.png)

<span data-ttu-id="ff67c-142">**所有網頁新增的應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="ff67c-142">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="ff67c-143">使用[慣例](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)集合[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)路由和頁面期間所套用的執行個體建構模型。</span><span class="sxs-lookup"><span data-stu-id="ff67c-143">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="ff67c-144">為了示範這個及其他本主題稍後的慣例，包括範例應用程式`AddHeaderAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="ff67c-144">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="ff67c-145">類別建構函式接受`name`字串和`values`字串陣列。</span><span class="sxs-lookup"><span data-stu-id="ff67c-145">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="ff67c-146">這些值會以其`OnResultExecuting`方法來設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="ff67c-146">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="ff67c-147">完整的類別如下所示[頁面模型動作慣例](#page-model-action-conventions)本主題稍後的章節。</span><span class="sxs-lookup"><span data-stu-id="ff67c-147">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="ff67c-148">範例應用程式會使用`AddHeaderAttribute`類別，以新增標頭， `GlobalHeader`，所有的應用程式中的頁面：</span><span class="sxs-lookup"><span data-stu-id="ff67c-148">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="ff67c-149">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ff67c-149">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="ff67c-150">要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-150">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![關於網頁的回應標頭會顯示已加入 GlobalHeader。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a><span data-ttu-id="ff67c-152">頁面路由動作慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-152">Page route action conventions</span></span>

<span data-ttu-id="ff67c-153">衍生自預設路由模型提供者[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)叫用的設計來提供擴充性點的設定頁面的路由慣例。</span><span class="sxs-lookup"><span data-stu-id="ff67c-153">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="ff67c-154">**資料夾路徑模型慣例**</span><span class="sxs-lookup"><span data-stu-id="ff67c-154">**Folder route model convention**</span></span>

<span data-ttu-id="ff67c-155">使用[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)會叫用動作上[PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)所有下頁面指定的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff67c-155">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="ff67c-156">範例應用程式會使用`AddFolderRouteModelConvention`新增`{otherPagesTemplate?}`中頁面的路由範本*OtherPages*資料夾：</span><span class="sxs-lookup"><span data-stu-id="ff67c-156">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="ff67c-157">`Order`屬性`AttributeRouteModel`設`1`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-157">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="ff67c-158">如此可確保針對範本`{globalTemplate?}`（稍早在本主題中的設定） 的第一個路由資料值的位置，提供單一路由值時指定優先權。</span><span class="sxs-lookup"><span data-stu-id="ff67c-158">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="ff67c-159">如果在要求 Page1 頁面`/OtherPages/Page1/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["otherPagesTemplate"]`(`Order = 1`) 因為設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="ff67c-159">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="ff67c-160">要求在此範例 Page1 頁面`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue`並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="ff67c-160">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![以路由區段 GlobalRouteValue 和 OtherPagesRouteValue 要求 Page1 OtherPages 資料夾中。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="ff67c-163">**頁面路由模型慣例**</span><span class="sxs-lookup"><span data-stu-id="ff67c-163">**Page route model convention**</span></span>

<span data-ttu-id="ff67c-164">使用[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention)建立並新增[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)會叫用動作上[PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)與指定頁面名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-164">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="ff67c-165">範例應用程式會使用`AddPageRouteModelConvention`新增`{aboutTemplate?}`關於頁面的路由範本：</span><span class="sxs-lookup"><span data-stu-id="ff67c-165">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="ff67c-166">`Order`屬性`AttributeRouteModel`設`1`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-166">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="ff67c-167">如此可確保針對範本`{globalTemplate?}`（稍早在本主題中的設定） 的第一個路由資料值的位置，提供單一路由值時指定優先權。</span><span class="sxs-lookup"><span data-stu-id="ff67c-167">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="ff67c-168">如果在要求關於頁面`/About/RouteDataValue`，"RouteDataValue 「 載入至`RouteData.Values["globalTemplate"]`(`Order = 0`) 而非`RouteData.Values["aboutTemplate"]`(`Order = 1`) 因為設定`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="ff67c-168">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="ff67c-169">要求在範例的相關網頁`localhost:5000/About/GlobalRouteValue/AboutRouteValue`並檢查結果：</span><span class="sxs-lookup"><span data-stu-id="ff67c-169">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![有關頁面要求的路由區段 GlobalRouteValue 和 AboutRouteValue。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="ff67c-172">設定頁面路由</span><span class="sxs-lookup"><span data-stu-id="ff67c-172">Configure a page route</span></span>

<span data-ttu-id="ff67c-173">使用[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute)頁面的路由設定在指定的頁面的路徑。</span><span class="sxs-lookup"><span data-stu-id="ff67c-173">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="ff67c-174">產生的連結至頁面使用您指定的路由。</span><span class="sxs-lookup"><span data-stu-id="ff67c-174">Generated links to the page use your specified route.</span></span> <span data-ttu-id="ff67c-175">`AddPageRoute`使用`AddPageRouteModelConvention`建立路由。</span><span class="sxs-lookup"><span data-stu-id="ff67c-175">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="ff67c-176">範例應用程式建立的路由`/TheContactPage`如*Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ff67c-176">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="ff67c-177">連絡人頁面也可以達到在`/Contact`透過其預設路由。</span><span class="sxs-lookup"><span data-stu-id="ff67c-177">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="ff67c-178">範例應用程式的自訂路由至 [連絡人] 頁面可讓選擇性`text`路由區段 (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-178">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="ff67c-179">這個頁面也包含在這個選擇性區段及其`@page`萬一訪客存取的頁面指示詞其`/Contact`路由：</span><span class="sxs-lookup"><span data-stu-id="ff67c-179">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="ff67c-180">請注意，針對產生 URL**連絡人**中呈現的網頁連結會反映更新的路由：</span><span class="sxs-lookup"><span data-stu-id="ff67c-180">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![導覽列中的範例應用程式，請連絡連結](razor-pages-convention-features/_static/contact-link.png)

![檢查呈現的 HTML 中的 [連絡人] 連結表示 href 設定為 ' / TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

<span data-ttu-id="ff67c-183">請瀏覽 [連絡人] 頁面，在其一般的路由， `/Contact`，或自訂的路由， `/TheContactPage`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-183">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="ff67c-184">如果您提供額外`text`路由區段頁面會顯示的 HTML 編碼的區段，提供：</span><span class="sxs-lookup"><span data-stu-id="ff67c-184">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Edge 瀏覽器範例提供的 URL 中的 'TextValue' 選擇性 'text' 的路由區段。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="ff67c-187">頁面模型動作慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-187">Page model action conventions</span></span>

<span data-ttu-id="ff67c-188">預設頁面模型提供者實作[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider)會叫用的慣例，這設計來提供擴充點設定的頁面模型。</span><span class="sxs-lookup"><span data-stu-id="ff67c-188">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="ff67c-189">這些慣例會建置和修改頁面探索和處理功能時很有用的。</span><span class="sxs-lookup"><span data-stu-id="ff67c-189">These conventions are useful when building and modifying page discovery and processing features.</span></span>

<span data-ttu-id="ff67c-190">本節中的範例，範例應用程式會使用`AddHeaderAttribute`類別，這是[ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)，可套用的回應標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-190">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="ff67c-191">使用慣例，此範例會示範如何將屬性套用所有資料夾中的頁面，並在單一頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-191">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="ff67c-192">**資料夾的應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="ff67c-192">**Folder app model convention**</span></span>

<span data-ttu-id="ff67c-193">使用[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)會叫用動作上[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)的執行個體指定的資料夾下的所有網頁。</span><span class="sxs-lookup"><span data-stu-id="ff67c-193">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="ff67c-194">此範例示範如何使用`AddFolderApplicationModelConvention`所新增的標頭， `OtherPagesHeader`，內部網頁*OtherPages*之應用程式資料夾：</span><span class="sxs-lookup"><span data-stu-id="ff67c-194">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="ff67c-195">要求在此範例 Page1 頁面`localhost:5000/OtherPages/Page1`和檢查以檢視結果的標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-195">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 頁面的回應標頭會顯示已加入 OtherPagesHeader。](razor-pages-convention-features/_static/page1-otherpages-header.png)

<span data-ttu-id="ff67c-197">**網頁應用程式模型慣例**</span><span class="sxs-lookup"><span data-stu-id="ff67c-197">**Page app model convention**</span></span>

<span data-ttu-id="ff67c-198">使用[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)建立並新增[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)會叫用動作上[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)頁面speciifed 名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-198">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="ff67c-199">此範例示範如何使用`AddPageApplicationModelConvention`所新增的標頭， `AboutHeader`，關於頁面：</span><span class="sxs-lookup"><span data-stu-id="ff67c-199">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="ff67c-200">要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-200">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![關於網頁的回應標頭會顯示已加入 AboutHeader。](razor-pages-convention-features/_static/about-page-about-header.png)

<span data-ttu-id="ff67c-202">**設定的篩選條件**</span><span class="sxs-lookup"><span data-stu-id="ff67c-202">**Configure a filter**</span></span>

<span data-ttu-id="ff67c-203">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter)設定指定要套用的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ff67c-203">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="ff67c-204">您可以實作的篩選器類別，但是範例應用程式示範如何在 lambda 運算式中，實作幕後作業以傳回篩選條件的處理站實作的篩選器：</span><span class="sxs-lookup"><span data-stu-id="ff67c-204">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="ff67c-205">網頁應用程式模型用來檢查導致 Page2 頁面中的區段的相對路徑*OtherPages*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff67c-205">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="ff67c-206">如果通過條件，則會加入標頭。</span><span class="sxs-lookup"><span data-stu-id="ff67c-206">If the condition passes, a header is added.</span></span> <span data-ttu-id="ff67c-207">如果沒有，則`EmptyFilter`套用。</span><span class="sxs-lookup"><span data-stu-id="ff67c-207">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="ff67c-208">`EmptyFilter`是[動作篩選條件](xref:mvc/controllers/filters#action-filters)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-208">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="ff67c-209">因為動作篩選條件會忽略 Razor 頁面`EmptyFilter`否 ops 如預期般如果路徑未包含`OtherPages/Page2`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-209">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="ff67c-210">要求在此範例 Page2 頁面`localhost:5000/OtherPages/Page2`和檢查以檢視結果的標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-210">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![Page2 OtherPagesPage2Header 新增至回應。](razor-pages-convention-features/_static/page2-filter-header.png)

<span data-ttu-id="ff67c-212">**設定篩選器 factory**</span><span class="sxs-lookup"><span data-stu-id="ff67c-212">**Configure a filter factory**</span></span>

<span data-ttu-id="ff67c-213">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__)設定要套用指定的 factory[篩選](xref:mvc/controllers/filters)所有 Razor 網頁。</span><span class="sxs-lookup"><span data-stu-id="ff67c-213">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="ff67c-214">範例應用程式提供的使用範例[篩選 factory](xref:mvc/controllers/filters#ifilterfactory)所新增的標頭， `FilterFactoryHeader`，具有兩個值的應用程式頁面：</span><span class="sxs-lookup"><span data-stu-id="ff67c-214">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="ff67c-215">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="ff67c-215">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="ff67c-216">要求在範例的相關網頁`localhost:5000/About`和檢查以檢視結果的標頭：</span><span class="sxs-lookup"><span data-stu-id="ff67c-216">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![關於網頁的回應標頭會顯示已加入兩個 FilterFactoryHeader 標頭。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="ff67c-218">取代預設的網頁應用程式模型提供者</span><span class="sxs-lookup"><span data-stu-id="ff67c-218">Replace the default page app model provider</span></span>

<span data-ttu-id="ff67c-219">使用 razor 頁面`IPageApplicationModelProvider`介面來建立[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-219">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="ff67c-220">您可以繼承自預設模型提供者，以提供您自己的實作邏輯處理常式探索和處理。</span><span class="sxs-lookup"><span data-stu-id="ff67c-220">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="ff67c-221">預設實作 ([參考來源](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) 建立的慣例*命名*和*名為*處理常式命名，如下所述。</span><span class="sxs-lookup"><span data-stu-id="ff67c-221">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="ff67c-222">**未命名的處理常式方法的預設值**</span><span class="sxs-lookup"><span data-stu-id="ff67c-222">**Default unnamed handler methods**</span></span>

<span data-ttu-id="ff67c-223">HTTP 指令動詞 （「 未命名的 」 的處理常式方法） 的處理常式方法遵循的慣例： `On<HTTP verb>[Async]` (附加`Async`是選用項但建議的非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-223">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="ff67c-224">未命名的處理常式方法</span><span class="sxs-lookup"><span data-stu-id="ff67c-224">Unnamed handler method</span></span>     | <span data-ttu-id="ff67c-225">運算</span><span class="sxs-lookup"><span data-stu-id="ff67c-225">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="ff67c-226">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="ff67c-226">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="ff67c-227">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="ff67c-227">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="ff67c-228">處理 DELETE 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-228">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="ff67c-229">處理 PUT 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-229">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="ff67c-230">處理 PATCH 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-230">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="ff67c-231">&#8224;用於 API 呼叫的頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-231">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="ff67c-232">**預設處理常式方法的名稱**</span><span class="sxs-lookup"><span data-stu-id="ff67c-232">**Default named handler methods**</span></span>

<span data-ttu-id="ff67c-233">（「 已命名 」 處理常式方法） 的開發人員所提供的處理常式方法會依照類似的慣例。</span><span class="sxs-lookup"><span data-stu-id="ff67c-233">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="ff67c-234">處理常式名稱會出現在 HTTP 動詞命令之後或之間的 HTTP 動詞命令和`Async`: `On<HTTP verb><handler name>[Async]` (附加`Async`是選用項但建議的非同步方法)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-234">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="ff67c-235">例如，處理訊息的方法可能會接受命名如下表所示。</span><span class="sxs-lookup"><span data-stu-id="ff67c-235">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="ff67c-236">名為處理常式方法的範例</span><span class="sxs-lookup"><span data-stu-id="ff67c-236">Example named handler method</span></span>             | <span data-ttu-id="ff67c-237">範例作業</span><span class="sxs-lookup"><span data-stu-id="ff67c-237">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="ff67c-238">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="ff67c-238">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="ff67c-239">張貼訊息。</span><span class="sxs-lookup"><span data-stu-id="ff67c-239">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="ff67c-240">刪除訊息 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-240">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="ff67c-241">將訊息 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-241">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="ff67c-242">修補訊息 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-242">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="ff67c-243">&#8224;用於 API 呼叫的頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-243">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="ff67c-244">**自訂處理常式方法名稱**</span><span class="sxs-lookup"><span data-stu-id="ff67c-244">**Customize handler method names**</span></span>

<span data-ttu-id="ff67c-245">假設您想要變更未具名和具名的處理常式方法的命名的方式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-245">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="ff67c-246">避免方法名稱，以 「 On 」 開頭，並用於判斷 HTTP 動詞命令的第一個文字區段是替代的命名配置。</span><span class="sxs-lookup"><span data-stu-id="ff67c-246">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="ff67c-247">您可以進行其他變更，例如轉換動詞命令刪除時，PUT 和 POST 的修補程式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-247">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="ff67c-248">這樣的配置會提供下表所示的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-248">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="ff67c-249">處理常式方法</span><span class="sxs-lookup"><span data-stu-id="ff67c-249">Handler method</span></span>                       | <span data-ttu-id="ff67c-250">運算</span><span class="sxs-lookup"><span data-stu-id="ff67c-250">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="ff67c-251">初始化頁面狀態。</span><span class="sxs-lookup"><span data-stu-id="ff67c-251">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="ff67c-252">處理 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="ff67c-252">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="ff67c-253">處理 DELETE 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-253">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="ff67c-254">處理 PUT 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-254">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="ff67c-255">處理 PATCH 要求 &#8224;。</span><span class="sxs-lookup"><span data-stu-id="ff67c-255">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="ff67c-256">取得訊息。</span><span class="sxs-lookup"><span data-stu-id="ff67c-256">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="ff67c-257">張貼訊息。</span><span class="sxs-lookup"><span data-stu-id="ff67c-257">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="ff67c-258">張貼訊息至刪除。</span><span class="sxs-lookup"><span data-stu-id="ff67c-258">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="ff67c-259">張貼訊息至放置。</span><span class="sxs-lookup"><span data-stu-id="ff67c-259">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="ff67c-260">張貼訊息至修補程式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-260">POST a message to patch.</span></span>       |

<span data-ttu-id="ff67c-261">&#8224;用於 API 呼叫的頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-261">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="ff67c-262">若要建立這種配置，繼承自`DefaultPageApplicationModelProvider`類別並覆寫[CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel)方法，以提供自訂邏輯來解析[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-262">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="ff67c-263">範例應用程式會顯示您如何這是其`CustomPageApplicationModelProvider`類別：</span><span class="sxs-lookup"><span data-stu-id="ff67c-263">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="ff67c-264">反白顯示的類別包括：</span><span class="sxs-lookup"><span data-stu-id="ff67c-264">Highlights of the class include:</span></span>

* <span data-ttu-id="ff67c-265">類別繼承自`DefaultPageApplicationModelProvider`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-265">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="ff67c-266">`TryParseHandlerMethod`程序來判斷 HTTP 動詞命令的處理常式 (`httpMethod`) 以及具名的處理常式名稱 (`handlerName`) 時建立`PageHandlerModel`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-266">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="ff67c-267">`Async`後置會被忽略，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="ff67c-267">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="ff67c-268">大小寫用來剖析 HTTP 指令動詞，與方法名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-268">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="ff67c-269">當方法名稱 (不含`Async`) 是 HTTP 動詞命令名稱相同，沒有任何具名的處理常式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-269">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="ff67c-270">`handlerName`設`null`，方法名稱，且`Get`， `Post`， `Delete`， `Put`，或`Patch`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-270">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="ff67c-271">當方法名稱 (不含`Async`) 長度超過 HTTP 動詞命令的名稱，具名的處理常式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-271">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="ff67c-272">`handlerName` 已設為 `<method name (less 'Async', if present)>`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-272">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="ff67c-273">例如，"GetMessage"和"GetMessageAsync 」 產生 「 GetMessage"的處理常式名稱。</span><span class="sxs-lookup"><span data-stu-id="ff67c-273">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="ff67c-274">DELETE、 PUT 和修補程式的 HTTP 動詞命令會轉換為 POST。</span><span class="sxs-lookup"><span data-stu-id="ff67c-274">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="ff67c-275">註冊`CustomPageApplicationModelProvider`中`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="ff67c-275">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="ff67c-276">程式碼後置檔案*Index.cshtml.cs*顯示如何將一般處理常式方法的命名慣例變更應用程式中的頁面。</span><span class="sxs-lookup"><span data-stu-id="ff67c-276">The code-behind file *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="ff67c-277">會移除一般 「 On 」 前置詞命名 Razor 頁面搭配使用。</span><span class="sxs-lookup"><span data-stu-id="ff67c-277">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="ff67c-278">初始化頁面狀態的方法現在已命名為`Get`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-278">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="ff67c-279">您可以看到這個慣例用整個應用程式，如果您開啟的任何網頁的任何程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="ff67c-279">You can see this convention used throughout the app if you open any code-behind file for any of the pages.</span></span>

<span data-ttu-id="ff67c-280">每個其他方法的開頭描述其處理的 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="ff67c-280">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="ff67c-281">開頭為兩個方法`Delete`通常會當做刪除 HTTP 動詞命令，但在邏輯來處理`TryParseHandlerMethod`明確設定為 POST 動詞命令的這兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="ff67c-281">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="ff67c-282">請注意，`Async`是選擇性之間`DeleteAllMessages`和`DeleteMessageAsync`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-282">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="ff67c-283">它們是兩個非同步方法，但您可以選擇使用`Async`後置或未; 我們建議您執行。</span><span class="sxs-lookup"><span data-stu-id="ff67c-283">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="ff67c-284">`DeleteAllMessages`這裡做為示範用途，但我們建議您命名此類方法`DeleteAllMessagesAsync`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-284">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="ff67c-285">它不會影響處理的範例實作，但是使用`Async`後置的事實，它會是一個非同步方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="ff67c-285">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="ff67c-286">請注意中提供的處理常式名稱*Index.cshtml*符合`DeleteAllMessages`和`DeleteMessageAsync`處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="ff67c-286">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="ff67c-287">`Async`處理常式方法名稱中`DeleteMessageAsync`逾時由分解`TryParseHandlerMethod`的處理常式方法的 POST 要求比對。</span><span class="sxs-lookup"><span data-stu-id="ff67c-287">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="ff67c-288">`asp-page-handler`名稱`DeleteMessage`相符的處理常式方法`DeleteMessageAsync`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-288">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="ff67c-289">MVC 篩選條件和頁面篩選 (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="ff67c-289">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="ff67c-290">MVC[動作篩選條件](xref:mvc/controllers/filters#action-filters)Razor 頁面，因為 Razor 頁面使用的處理常式方法會忽略。</span><span class="sxs-lookup"><span data-stu-id="ff67c-290">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="ff67c-291">其他類型的 MVC 篩選條件可供您使用：[授權](xref:mvc/controllers/filters#authorization-filters)，[例外狀況](xref:mvc/controllers/filters#exception-filters)，[資源](xref:mvc/controllers/filters#resource-filters)，和[結果](xref:mvc/controllers/filters#result-filters)。</span><span class="sxs-lookup"><span data-stu-id="ff67c-291">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="ff67c-292">如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)主題。</span><span class="sxs-lookup"><span data-stu-id="ff67c-292">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="ff67c-293">頁面篩選 ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) 套用到 Razor 頁面的篩選。</span><span class="sxs-lookup"><span data-stu-id="ff67c-293">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="ff67c-294">它會包圍網頁處理常式方法的執行。</span><span class="sxs-lookup"><span data-stu-id="ff67c-294">It surrounds the execution of a page handler method.</span></span> <span data-ttu-id="ff67c-295">它可讓您處理自訂程式碼的網頁處理常式方法的執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff67c-295">It allows you to process custom code at stages of page handler method execution.</span></span> <span data-ttu-id="ff67c-296">以下是範例應用程式中的範例：</span><span class="sxs-lookup"><span data-stu-id="ff67c-296">Here's an example from the sample app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

<span data-ttu-id="ff67c-297">此篩選條件會檢查`globalTemplate`"ReplacementValue 「 路由 」 TriggerValue"和交換的值。</span><span class="sxs-lookup"><span data-stu-id="ff67c-297">This filter checks for a `globalTemplate` route value of "TriggerValue" and swaps in "ReplacementValue".</span></span>

<span data-ttu-id="ff67c-298">`ReplaceRouteValueFilter`屬性直接套用`PageModel`中程式碼後置：</span><span class="sxs-lookup"><span data-stu-id="ff67c-298">The `ReplaceRouteValueFilter` attribute can be applied directly to a `PageModel` in code-behind:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

<span data-ttu-id="ff67c-299">在使用範例應用程式從要求 Page3 頁面`localhost:5000/OtherPages/Page3/TriggerValue`。</span><span class="sxs-lookup"><span data-stu-id="ff67c-299">Request the Page3 page from the sample app with at `localhost:5000/OtherPages/Page3/TriggerValue`.</span></span> <span data-ttu-id="ff67c-300">請注意，篩選會路由值的取代：</span><span class="sxs-lookup"><span data-stu-id="ff67c-300">Notice how the filter replaces the route value:</span></span>

![要求 OtherPages/Page3 TriggerValue 路由區段在中使用結果取代 ReplacementValue 路由值的篩選條件。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a><span data-ttu-id="ff67c-302">請參閱</span><span class="sxs-lookup"><span data-stu-id="ff67c-302">See also</span></span>

* [<span data-ttu-id="ff67c-303">Razor 頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="ff67c-303">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
