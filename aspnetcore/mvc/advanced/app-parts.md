---
title: 在 ASP.NET Core 中，與Razor應用程式元件共用控制器、視圖、頁面和更多
author: rick-anderson
description: 在 ASP.NET Core 中，與Razor應用程式元件共用控制器、視圖、頁面和更多
ms.author: riande
ms.date: 11/11/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 68991a3df5e09b63dc52bdadae55f055a721ad3c
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774401"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts"></a><span data-ttu-id="a886e-103">與應用程式元件共用Razor控制器、視圖、頁面和更多</span><span class="sxs-lookup"><span data-stu-id="a886e-103">Share controllers, views, Razor Pages and more with Application Parts</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a886e-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a886e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a886e-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a886e-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a886e-106">*應用程式元件*是應用程式資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a886e-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="a886e-107">應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助Razor程式、頁面、razor 編譯來源等等。</span><span class="sxs-lookup"><span data-stu-id="a886e-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="a886e-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart>是應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> is an Application part.</span></span> <span data-ttu-id="a886e-109">`AssemblyPart`封裝元件參考，並公開類型和編譯參考。</span><span class="sxs-lookup"><span data-stu-id="a886e-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="a886e-110">[功能提供者](#fp)會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-110">[Feature providers](#fp) work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="a886e-111">應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="a886e-112">例如，您可能會想要在多個應用程式之間共用一般功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="a886e-113">使用應用程式元件時，您可以與多個應用程式共用包含控制器、 Razor視圖、頁面、razor 編譯來源、標記協助程式等的元件（DLL）。</span><span class="sxs-lookup"><span data-stu-id="a886e-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="a886e-114">在多個專案中複製程式碼時，偏好共用元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="a886e-115">ASP.NET Core 應用程式從<xref:System.Web.WebPages.ApplicationPart>載入功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="a886e-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart>類別代表元件所支援的應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="a886e-117">載入 ASP.NET Core 功能</span><span class="sxs-lookup"><span data-stu-id="a886e-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="a886e-118">使用<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>和<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart>類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等）。</span><span class="sxs-lookup"><span data-stu-id="a886e-118">Use the <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> and <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="a886e-119">會<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>追蹤可用的應用程式元件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="a886e-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="a886e-120">`ApplicationPartManager`設定于`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="a886e-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="a886e-121">下列程式碼提供`ApplicationPartManager`使用`AssemblyPart`設定的替代方法：</span><span class="sxs-lookup"><span data-stu-id="a886e-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="a886e-122">上述兩個程式碼範例會`SharedController`從元件載入。</span><span class="sxs-lookup"><span data-stu-id="a886e-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="a886e-123">`SharedController`不在應用程式的專案中。</span><span class="sxs-lookup"><span data-stu-id="a886e-123">The `SharedController` is not in the app's project.</span></span> <span data-ttu-id="a886e-124">請參閱[WebAppParts 解決方案](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts)範例下載。</span><span class="sxs-lookup"><span data-stu-id="a886e-124">See the [WebAppParts solution](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="a886e-125">包含視圖</span><span class="sxs-lookup"><span data-stu-id="a886e-125">Include views</span></span>

<span data-ttu-id="a886e-126">使用[ Razor類別庫](xref:razor-pages/ui-class)將 views 包含在元件中。</span><span class="sxs-lookup"><span data-stu-id="a886e-126">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="a886e-127">防止載入資源</span><span class="sxs-lookup"><span data-stu-id="a886e-127">Prevent loading resources</span></span>

<span data-ttu-id="a886e-128">應用程式元件可以用來*避免*在特定元件或位置中載入資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-128">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="a886e-129">新增或移除<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>集合的成員，以隱藏或提供可用的資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-129">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="a886e-130">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="a886e-130">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="a886e-131">先設定`ApplicationPartManager` ，再使用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="a886e-131">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="a886e-132">例如，在叫用`ApplicationPartManager`之前設定`AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="a886e-132">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="a886e-133">在`Remove` `ApplicationParts`集合上呼叫以移除資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-133">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="a886e-134">`ApplicationPartManager`包含的部分：</span><span class="sxs-lookup"><span data-stu-id="a886e-134">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="a886e-135">應用程式的元件和相依元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-135">The app's assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.ApplicationParts.CompiledRazorAssemblyPart`
* `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`
* <span data-ttu-id="a886e-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="a886e-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="a886e-137">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="a886e-137">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

<a name="fp"></a>

## <a name="feature-providers"></a><span data-ttu-id="a886e-138">功能提供者</span><span class="sxs-lookup"><span data-stu-id="a886e-138">Feature providers</span></span>

<span data-ttu-id="a886e-139">應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-139">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="a886e-140">下列 ASP.NET Core 功能有內建的功能提供者：</span><span class="sxs-lookup"><span data-stu-id="a886e-140">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Controllers.ControllerFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.MetadataReferenceFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.ViewsFeatureProvider>
* <span data-ttu-id="a886e-141">`internal class`[RazorCompiledItemFeatureProvider](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span><span class="sxs-lookup"><span data-stu-id="a886e-141">`internal class` [RazorCompiledItemFeatureProvider](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span></span>

<span data-ttu-id="a886e-142">繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="a886e-142">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="a886e-143">功能提供者可以針對任何先前列出的功能類型來執行。</span><span class="sxs-lookup"><span data-stu-id="a886e-143">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="a886e-144">中的功能提供者順序`ApplicationPartManager.FeatureProviders`可能會影響執行時間行為。</span><span class="sxs-lookup"><span data-stu-id="a886e-144">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="a886e-145">稍後新增的提供者可以回應先前新增的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="a886e-145">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="a886e-146">顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="a886e-146">Display available features</span></span>

<span data-ttu-id="a886e-147">`ApplicationPartManager`透過相依性[插入](../../fundamentals/dependency-injection.md)要求，可以列舉應用程式可用的功能：</span><span class="sxs-lookup"><span data-stu-id="a886e-147">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="a886e-148">[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：</span><span class="sxs-lookup"><span data-stu-id="a886e-148">The [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="a886e-149">應用程式元件中的探索</span><span class="sxs-lookup"><span data-stu-id="a886e-149">Discovery in application parts</span></span>

<span data-ttu-id="a886e-150">使用應用程式元件進行開發時，HTTP 404 錯誤並不常見。</span><span class="sxs-lookup"><span data-stu-id="a886e-150">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="a886e-151">這些錯誤通常是因為遺漏應用程式元件探索方式的基本需求所造成。</span><span class="sxs-lookup"><span data-stu-id="a886e-151">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="a886e-152">如果您的應用程式傳回 HTTP 404 錯誤，請確認是否符合下列需求：</span><span class="sxs-lookup"><span data-stu-id="a886e-152">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="a886e-153">`applicationName`設定必須設定為用於探索的根元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-153">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="a886e-154">用於探索的根元件通常是進入點元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-154">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="a886e-155">根元件必須參考用於探索的元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-155">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="a886e-156">參考可以是直接或可轉移的。</span><span class="sxs-lookup"><span data-stu-id="a886e-156">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="a886e-157">根元件需要參考 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="a886e-157">The root assembly needs to reference the Web SDK.</span></span> <span data-ttu-id="a886e-158">此架構的邏輯會將屬性戳記成用於探索的根元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-158">The framework has logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a886e-159">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a886e-159">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a886e-160">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a886e-160">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a886e-161">*應用程式元件*是應用程式資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a886e-161">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="a886e-162">應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助Razor程式、頁面、razor 編譯來源等等。</span><span class="sxs-lookup"><span data-stu-id="a886e-162">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="a886e-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="a886e-164">`AssemblyPart`封裝元件參考，並公開類型和編譯參考。</span><span class="sxs-lookup"><span data-stu-id="a886e-164">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="a886e-165">*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-165">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="a886e-166">應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-166">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="a886e-167">例如，您可能會想要在多個應用程式之間共用一般功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-167">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="a886e-168">使用應用程式元件時，您可以與多個應用程式共用包含控制器、 Razor視圖、頁面、razor 編譯來源、標記協助程式等的元件（DLL）。</span><span class="sxs-lookup"><span data-stu-id="a886e-168">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="a886e-169">在多個專案中複製程式碼時，偏好共用元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-169">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="a886e-170">ASP.NET Core 應用程式從<xref:System.Web.WebPages.ApplicationPart>載入功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-170">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="a886e-171"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart>類別代表元件所支援的應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-171">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="a886e-172">載入 ASP.NET Core 功能</span><span class="sxs-lookup"><span data-stu-id="a886e-172">Load ASP.NET Core features</span></span>

<span data-ttu-id="a886e-173">使用`ApplicationPart`和`AssemblyPart`類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等）。</span><span class="sxs-lookup"><span data-stu-id="a886e-173">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="a886e-174">會<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>追蹤可用的應用程式元件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="a886e-174">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="a886e-175">`ApplicationPartManager`設定于`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="a886e-175">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="a886e-176">下列程式碼提供`ApplicationPartManager`使用`AssemblyPart`設定的替代方法：</span><span class="sxs-lookup"><span data-stu-id="a886e-176">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="a886e-177">上述兩個程式碼範例會`SharedController`從元件載入。</span><span class="sxs-lookup"><span data-stu-id="a886e-177">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="a886e-178">`SharedController`不在應用程式的專案中。</span><span class="sxs-lookup"><span data-stu-id="a886e-178">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="a886e-179">請參閱[WebAppParts 解決方案](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。</span><span class="sxs-lookup"><span data-stu-id="a886e-179">See the [WebAppParts solution](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="a886e-180">包含視圖</span><span class="sxs-lookup"><span data-stu-id="a886e-180">Include views</span></span>

<span data-ttu-id="a886e-181">使用[ Razor類別庫](xref:razor-pages/ui-class)將 views 包含在元件中。</span><span class="sxs-lookup"><span data-stu-id="a886e-181">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="a886e-182">防止載入資源</span><span class="sxs-lookup"><span data-stu-id="a886e-182">Prevent loading resources</span></span>

<span data-ttu-id="a886e-183">應用程式元件可以用來*避免*在特定元件或位置中載入資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-183">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="a886e-184">新增或移除<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>集合的成員，以隱藏或提供可用的資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-184">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="a886e-185">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="a886e-185">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="a886e-186">先設定`ApplicationPartManager` ，再使用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="a886e-186">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="a886e-187">例如，在叫用`ApplicationPartManager`之前設定`AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="a886e-187">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="a886e-188">在`Remove` `ApplicationParts`集合上呼叫以移除資源。</span><span class="sxs-lookup"><span data-stu-id="a886e-188">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="a886e-189">下列程式碼會<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>使用從`MyDependentLibrary`應用程式中移除：[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="a886e-189">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="a886e-190">`ApplicationPartManager`包含的部分：</span><span class="sxs-lookup"><span data-stu-id="a886e-190">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="a886e-191">應用程式的元件和相依元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-191">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="a886e-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="a886e-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="a886e-193">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="a886e-193">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="a886e-194">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="a886e-194">Application feature providers</span></span>

<span data-ttu-id="a886e-195">應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。</span><span class="sxs-lookup"><span data-stu-id="a886e-195">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="a886e-196">下列 ASP.NET Core 功能有內建的功能提供者：</span><span class="sxs-lookup"><span data-stu-id="a886e-196">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="a886e-197">Controllers</span><span class="sxs-lookup"><span data-stu-id="a886e-197">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="a886e-198">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="a886e-198">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="a886e-199">視圖元件</span><span class="sxs-lookup"><span data-stu-id="a886e-199">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="a886e-200">繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="a886e-200">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="a886e-201">功能提供者可以針對任何先前列出的功能類型來執行。</span><span class="sxs-lookup"><span data-stu-id="a886e-201">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="a886e-202">中的功能提供者順序`ApplicationPartManager.FeatureProviders`可能會影響執行時間行為。</span><span class="sxs-lookup"><span data-stu-id="a886e-202">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="a886e-203">稍後新增的提供者可以回應先前新增的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="a886e-203">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="a886e-204">顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="a886e-204">Display available features</span></span>

<span data-ttu-id="a886e-205">`ApplicationPartManager`透過相依性[插入](../../fundamentals/dependency-injection.md)要求，可以列舉應用程式可用的功能：</span><span class="sxs-lookup"><span data-stu-id="a886e-205">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="a886e-206">[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：</span><span class="sxs-lookup"><span data-stu-id="a886e-206">The [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="a886e-207">應用程式元件中的探索</span><span class="sxs-lookup"><span data-stu-id="a886e-207">Discovery in application parts</span></span>

<span data-ttu-id="a886e-208">使用應用程式元件進行開發時，HTTP 404 錯誤並不常見。</span><span class="sxs-lookup"><span data-stu-id="a886e-208">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="a886e-209">這些錯誤通常是因為遺漏應用程式元件探索方式的基本需求所造成。</span><span class="sxs-lookup"><span data-stu-id="a886e-209">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="a886e-210">如果您的應用程式傳回 HTTP 404 錯誤，請確認是否符合下列需求：</span><span class="sxs-lookup"><span data-stu-id="a886e-210">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="a886e-211">`applicationName`設定必須設定為用於探索的根元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-211">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="a886e-212">用於探索的根元件通常是進入點元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-212">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="a886e-213">根元件必須參考用於探索的元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-213">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="a886e-214">參考可以是直接或可轉移的。</span><span class="sxs-lookup"><span data-stu-id="a886e-214">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="a886e-215">根元件需要參考 Web SDK。</span><span class="sxs-lookup"><span data-stu-id="a886e-215">The root assembly needs to reference the Web SDK.</span></span>
  * <span data-ttu-id="a886e-216">ASP.NET Core 架構有自訂的組建邏輯，可將屬性戳記成用於探索的根元件。</span><span class="sxs-lookup"><span data-stu-id="a886e-216">The ASP.NET Core framework has custom build logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end
