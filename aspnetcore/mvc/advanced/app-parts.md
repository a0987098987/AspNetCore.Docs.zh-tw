---
title: ASP.NET Core 的應用程式組件
author: rick-anderson
description: 與中的應用程式元件共用控制器、視圖、Razor Pages 等等 ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ad0372f25377115e6fc7c8ea42db75de56b3e6d2
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187008"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="61d9e-103">在 ASP.NET Core 中與應用程式元件共用控制器、視圖、Razor Pages 和更多</span><span class="sxs-lookup"><span data-stu-id="61d9e-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>
=======

<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

<span data-ttu-id="61d9e-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="61d9e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="61d9e-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61d9e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="61d9e-106">*應用程式元件*是應用程式資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="61d9e-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="61d9e-107">應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助程式、Razor Pages、Razor 編譯來源等等。</span><span class="sxs-lookup"><span data-stu-id="61d9e-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="61d9e-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="61d9e-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="61d9e-109">`AssemblyPart`封裝元件參考，並公開類型和編譯參考。</span><span class="sxs-lookup"><span data-stu-id="61d9e-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="61d9e-110">*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="61d9e-111">應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="61d9e-112">例如，您可能會想要在多個應用程式之間共用一般功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="61d9e-113">使用應用程式元件時，您可以與多個應用程式共用包含控制器、視圖、Razor Pages、Razor 編譯來源、標記協助程式等元件（DLL）。</span><span class="sxs-lookup"><span data-stu-id="61d9e-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="61d9e-114">在多個專案中複製程式碼時，偏好共用元件。</span><span class="sxs-lookup"><span data-stu-id="61d9e-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="61d9e-115">ASP.NET Core 應用程式從<xref:System.Web.WebPages.ApplicationPart>載入功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="61d9e-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart>類別代表元件所支援的應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="61d9e-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="61d9e-117">載入 ASP.NET Core 功能</span><span class="sxs-lookup"><span data-stu-id="61d9e-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="61d9e-118">`ApplicationPart`使用和`AssemblyPart`類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等）。</span><span class="sxs-lookup"><span data-stu-id="61d9e-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="61d9e-119">會<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>追蹤可用的應用程式元件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="61d9e-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="61d9e-120">`ApplicationPartManager`設定于`Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="61d9e-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="61d9e-121">下列程式碼提供`ApplicationPartManager`使用`AssemblyPart`設定的替代方法：</span><span class="sxs-lookup"><span data-stu-id="61d9e-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="61d9e-122">上述兩個程式碼範例會`SharedController`從元件載入。</span><span class="sxs-lookup"><span data-stu-id="61d9e-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="61d9e-123">`SharedController`不在應用程式專案中。</span><span class="sxs-lookup"><span data-stu-id="61d9e-123">The `SharedController` is not in the applications project.</span></span> <span data-ttu-id="61d9e-124">請參閱[WebAppParts 解決方案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。</span><span class="sxs-lookup"><span data-stu-id="61d9e-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="61d9e-125">包含視圖</span><span class="sxs-lookup"><span data-stu-id="61d9e-125">Include views</span></span>

<span data-ttu-id="61d9e-126">若要在元件中包含 views：</span><span class="sxs-lookup"><span data-stu-id="61d9e-126">To include views in the assembly:</span></span>

* <span data-ttu-id="61d9e-127">將下列標記新增至共用專案檔：</span><span class="sxs-lookup"><span data-stu-id="61d9e-127">Add the following markup to the shared project file:</span></span>

  ```csproj
    <ItemGroup>
      <EmbeddedResource Include = "Views\**\*.cshtml" />
    </ ItemGroup >
  ```

* <span data-ttu-id="61d9e-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider>將新增<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>至：</span><span class="sxs-lookup"><span data-stu-id="61d9e-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="61d9e-129">防止載入資源</span><span class="sxs-lookup"><span data-stu-id="61d9e-129">Prevent loading resources</span></span>

<span data-ttu-id="61d9e-130">應用程式元件可以用來*避免*在特定元件或位置中載入資源。</span><span class="sxs-lookup"><span data-stu-id="61d9e-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="61d9e-131">新增或移除<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>集合的成員，以隱藏或提供可用的資源。</span><span class="sxs-lookup"><span data-stu-id="61d9e-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="61d9e-132">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="61d9e-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="61d9e-133">先設定`ApplicationPartManager` ，再使用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="61d9e-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="61d9e-134">例如，在叫用`ApplicationPartManager` `AddControllersAsServices`之前設定。</span><span class="sxs-lookup"><span data-stu-id="61d9e-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="61d9e-135">在集合上呼叫`Remove`以移除資源。`ApplicationParts`</span><span class="sxs-lookup"><span data-stu-id="61d9e-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="61d9e-136">下列程式碼會<xref:Microsoft.AspNetCore.Mvc.ApplicationParts>使用從`MyDependentLibrary`應用程式中移除：[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="61d9e-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="61d9e-137">`ApplicationPartManager`包含的部分：</span><span class="sxs-lookup"><span data-stu-id="61d9e-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="61d9e-138">應用程式元件和相依元件。</span><span class="sxs-lookup"><span data-stu-id="61d9e-138">The apps assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* <span data-ttu-id="61d9e-139">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="61d9e-139">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="61d9e-140">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="61d9e-140">Application feature providers</span></span>

<span data-ttu-id="61d9e-141">應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-141">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="61d9e-142">下列 ASP.NET Core 功能有內建的功能提供者：</span><span class="sxs-lookup"><span data-stu-id="61d9e-142">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="61d9e-143">控制器</span><span class="sxs-lookup"><span data-stu-id="61d9e-143">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="61d9e-144">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="61d9e-144">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="61d9e-145">檢視元件</span><span class="sxs-lookup"><span data-stu-id="61d9e-145">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="61d9e-146">繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="61d9e-146">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="61d9e-147">功能提供者可以針對任何先前列出的功能類型來執行。</span><span class="sxs-lookup"><span data-stu-id="61d9e-147">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="61d9e-148">中的功能提供者順序`ApplicationPartManager.FeatureProviders`可能會影響執行時間行為。</span><span class="sxs-lookup"><span data-stu-id="61d9e-148">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="61d9e-149">稍後新增的提供者可以回應先前新增的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="61d9e-149">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="61d9e-150">泛型控制器功能</span><span class="sxs-lookup"><span data-stu-id="61d9e-150">Generic controller feature</span></span>

<span data-ttu-id="61d9e-151">ASP.NET Core 會忽略[泛型控制器](/dotnet/csharp/programming-guide/generics/generic-classes)。</span><span class="sxs-lookup"><span data-stu-id="61d9e-151">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="61d9e-152">泛型控制器具有型別參數（ `MyController<T>`例如）。</span><span class="sxs-lookup"><span data-stu-id="61d9e-152">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="61d9e-153">下列範例會針對指定的類型清單加入泛型控制器實例。</span><span class="sxs-lookup"><span data-stu-id="61d9e-153">The following sample adds generic controller instances for a specified list of types.</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="61d9e-154">類型定義于`EntityTypes.Types`：</span><span class="sxs-lookup"><span data-stu-id="61d9e-154">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="61d9e-155">系統會將功能提供者新增至 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="61d9e-155">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="61d9e-156">用於路由的一般控制器名稱的格式為*GenericController ' 1 [widget]* ，而不是*Widget*。</span><span class="sxs-lookup"><span data-stu-id="61d9e-156">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="61d9e-157">下列屬性會修改名稱，以對應至控制器所使用的泛型型別：</span><span class="sxs-lookup"><span data-stu-id="61d9e-157">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="61d9e-158">`GenericController` 類別：</span><span class="sxs-lookup"><span data-stu-id="61d9e-158">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

### <a name="display-available-features"></a><span data-ttu-id="61d9e-159">顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="61d9e-159">Display available features</span></span>

<span data-ttu-id="61d9e-160">`ApplicationPartManager`透過相依性[插入](../../fundamentals/dependency-injection.md)要求，可以列舉應用程式可用的功能：</span><span class="sxs-lookup"><span data-stu-id="61d9e-160">The features available to an app can be enumerated by by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="61d9e-161">[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能。</span><span class="sxs-lookup"><span data-stu-id="61d9e-161">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features.</span></span>