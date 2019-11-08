---
title: ASP.NET Core 的應用程式組件
author: rick-anderson
description: 與中的應用程式元件共用控制器、視圖、Razor Pages 等等 ASP.NET Core
ms.author: riande
ms.date: 11/7/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ff6afa1852a3ee97fc4dbbae970dd746ec92f74c
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799470"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="f5670-103">在 ASP.NET Core 中與應用程式元件共用控制器、視圖、Razor Pages 和更多</span><span class="sxs-lookup"><span data-stu-id="f5670-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f5670-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f5670-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f5670-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5670-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f5670-106">*應用程式元件*是應用程式資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="f5670-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="f5670-107">應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助程式、Razor Pages、Razor 編譯來源等等。</span><span class="sxs-lookup"><span data-stu-id="f5670-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="f5670-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="f5670-109">`AssemblyPart` 封裝元件參考，並公開類型和編譯參考。</span><span class="sxs-lookup"><span data-stu-id="f5670-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="f5670-110">*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="f5670-111">應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="f5670-112">例如，您可能會想要在多個應用程式之間共用一般功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="f5670-113">使用應用程式元件時，您可以與多個應用程式共用包含控制器、視圖、Razor Pages、Razor 編譯來源、標記協助程式等元件（DLL）。</span><span class="sxs-lookup"><span data-stu-id="f5670-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="f5670-114">在多個專案中複製程式碼時，偏好共用元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="f5670-115">ASP.NET Core 應用程式會從 <xref:System.Web.WebPages.ApplicationPart>載入功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="f5670-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> 類別代表元件所支援的應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="f5670-117">載入 ASP.NET Core 功能</span><span class="sxs-lookup"><span data-stu-id="f5670-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="f5670-118">使用 `ApplicationPart` 和 `AssemblyPart` 類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等等）。</span><span class="sxs-lookup"><span data-stu-id="f5670-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="f5670-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> 會追蹤可用的應用程式元件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="f5670-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="f5670-120">`ApplicationPartManager` 是在 `Startup.ConfigureServices`中設定：</span><span class="sxs-lookup"><span data-stu-id="f5670-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="f5670-121">下列程式碼提供使用 `AssemblyPart`設定 `ApplicationPartManager` 的替代方法：</span><span class="sxs-lookup"><span data-stu-id="f5670-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="f5670-122">上述兩個程式碼範例會從元件載入 `SharedController`。</span><span class="sxs-lookup"><span data-stu-id="f5670-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="f5670-123">`SharedController` 不在應用程式的專案中。</span><span class="sxs-lookup"><span data-stu-id="f5670-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="f5670-124">請參閱[WebAppParts 解決方案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。</span><span class="sxs-lookup"><span data-stu-id="f5670-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="f5670-125">包含視圖</span><span class="sxs-lookup"><span data-stu-id="f5670-125">Include views</span></span>

<span data-ttu-id="f5670-126">若要在元件中包含 views：</span><span class="sxs-lookup"><span data-stu-id="f5670-126">To include views in the assembly:</span></span>

* <span data-ttu-id="f5670-127">將下列標記新增至共用專案檔：</span><span class="sxs-lookup"><span data-stu-id="f5670-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="f5670-128">將 <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> 新增至 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>：</span><span class="sxs-lookup"><span data-stu-id="f5670-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="f5670-129">防止載入資源</span><span class="sxs-lookup"><span data-stu-id="f5670-129">Prevent loading resources</span></span>

<span data-ttu-id="f5670-130">應用程式元件可以用來*避免*在特定元件或位置中載入資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="f5670-131">新增或移除 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 集合的成員，以隱藏或提供可用的資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="f5670-132">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="f5670-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="f5670-133">先設定 `ApplicationPartManager`，然後再使用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="f5670-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="f5670-134">例如，先設定 `ApplicationPartManager`，再叫用 `AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="f5670-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="f5670-135">呼叫 `ApplicationParts` 集合上的 `Remove` 以移除資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="f5670-136">下列程式碼會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 從應用程式移除 `MyDependentLibrary`： [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="f5670-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="f5670-137">`ApplicationPartManager` 包括下列各部分：</span><span class="sxs-lookup"><span data-stu-id="f5670-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="f5670-138">應用程式的元件和相依元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="f5670-139">`Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="f5670-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="f5670-140">`Microsoft.AspNetCore.Mvc.Razor`</span><span class="sxs-lookup"><span data-stu-id="f5670-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="f5670-141">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="f5670-141">Application feature providers</span></span>

<span data-ttu-id="f5670-142">應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="f5670-143">下列 ASP.NET Core 功能有內建的功能提供者：</span><span class="sxs-lookup"><span data-stu-id="f5670-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="f5670-144">控制器</span><span class="sxs-lookup"><span data-stu-id="f5670-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="f5670-145">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="f5670-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="f5670-146">檢視元件</span><span class="sxs-lookup"><span data-stu-id="f5670-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="f5670-147">繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="f5670-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="f5670-148">功能提供者可以針對任何先前列出的功能類型來執行。</span><span class="sxs-lookup"><span data-stu-id="f5670-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="f5670-149">`ApplicationPartManager.FeatureProviders` 中的功能提供者順序可能會影響執行時間行為。</span><span class="sxs-lookup"><span data-stu-id="f5670-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="f5670-150">稍後新增的提供者可以回應先前新增的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="f5670-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="f5670-151">泛型控制器功能</span><span class="sxs-lookup"><span data-stu-id="f5670-151">Generic controller feature</span></span>

<span data-ttu-id="f5670-152">ASP.NET Core 會忽略[泛型控制器](/dotnet/csharp/programming-guide/generics/generic-classes)。</span><span class="sxs-lookup"><span data-stu-id="f5670-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="f5670-153">泛型控制器具有型別參數（例如，`MyController<T>`）。</span><span class="sxs-lookup"><span data-stu-id="f5670-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="f5670-154">下列範例會針對指定的類型清單加入泛型控制器實例：</span><span class="sxs-lookup"><span data-stu-id="f5670-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="f5670-155">類型定義于 `EntityTypes.Types`中：</span><span class="sxs-lookup"><span data-stu-id="f5670-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="f5670-156">系統會將功能提供者新增至 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="f5670-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="f5670-157">用於路由的一般控制器名稱的格式為*GenericController ' 1 [widget]* ，而不是*Widget*。</span><span class="sxs-lookup"><span data-stu-id="f5670-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="f5670-158">下列屬性會修改名稱，以對應至控制器所使用的泛型型別：</span><span class="sxs-lookup"><span data-stu-id="f5670-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="f5670-159">`GenericController` 類別：</span><span class="sxs-lookup"><span data-stu-id="f5670-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="f5670-160">例如，要求 `https://localhost:5001/Sprocket` 的 URL 會產生下列回應：</span><span class="sxs-lookup"><span data-stu-id="f5670-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="f5670-161">顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="f5670-161">Display available features</span></span>

<span data-ttu-id="f5670-162">透過相依性[插入](../../fundamentals/dependency-injection.md)來要求 `ApplicationPartManager`，即可列舉應用程式可用的功能：</span><span class="sxs-lookup"><span data-stu-id="f5670-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="f5670-163">[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：</span><span class="sxs-lookup"><span data-stu-id="f5670-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end

::: moniker range="<= aspnetcore-3.0"

<span data-ttu-id="f5670-164">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f5670-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f5670-165">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5670-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f5670-166">*應用程式元件*是應用程式資源的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="f5670-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="f5670-167">應用程式元件可讓 ASP.NET Core 探索控制器、視圖元件、標記協助程式、Razor Pages、Razor 編譯來源等等。</span><span class="sxs-lookup"><span data-stu-id="f5670-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="f5670-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)是一個應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="f5670-169">`AssemblyPart` 封裝元件參考，並公開類型和編譯參考。</span><span class="sxs-lookup"><span data-stu-id="f5670-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="f5670-170">*功能提供者*會使用應用程式元件來填入 ASP.NET Core 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="f5670-171">應用程式元件的主要使用案例是將應用程式設定為從元件中探索（或避免載入） ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="f5670-172">例如，您可能會想要在多個應用程式之間共用一般功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="f5670-173">使用應用程式元件時，您可以與多個應用程式共用包含控制器、視圖、Razor Pages、Razor 編譯來源、標記協助程式等元件（DLL）。</span><span class="sxs-lookup"><span data-stu-id="f5670-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="f5670-174">在多個專案中複製程式碼時，偏好共用元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="f5670-175">ASP.NET Core 應用程式會從 <xref:System.Web.WebPages.ApplicationPart>載入功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="f5670-176"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> 類別代表元件所支援的應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="f5670-177">載入 ASP.NET Core 功能</span><span class="sxs-lookup"><span data-stu-id="f5670-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="f5670-178">使用 `ApplicationPart` 和 `AssemblyPart` 類別來探索和載入 ASP.NET Core 功能（控制器、視圖元件等等）。</span><span class="sxs-lookup"><span data-stu-id="f5670-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="f5670-179"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> 會追蹤可用的應用程式元件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="f5670-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="f5670-180">`ApplicationPartManager` 是在 `Startup.ConfigureServices`中設定：</span><span class="sxs-lookup"><span data-stu-id="f5670-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="f5670-181">下列程式碼提供使用 `AssemblyPart`設定 `ApplicationPartManager` 的替代方法：</span><span class="sxs-lookup"><span data-stu-id="f5670-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="f5670-182">上述兩個程式碼範例會從元件載入 `SharedController`。</span><span class="sxs-lookup"><span data-stu-id="f5670-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="f5670-183">`SharedController` 不在應用程式的專案中。</span><span class="sxs-lookup"><span data-stu-id="f5670-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="f5670-184">請參閱[WebAppParts 解決方案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts)範例下載。</span><span class="sxs-lookup"><span data-stu-id="f5670-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="f5670-185">包含視圖</span><span class="sxs-lookup"><span data-stu-id="f5670-185">Include views</span></span>

<span data-ttu-id="f5670-186">若要在元件中包含 views：</span><span class="sxs-lookup"><span data-stu-id="f5670-186">To include views in the assembly:</span></span>

* <span data-ttu-id="f5670-187">將下列標記新增至共用專案檔：</span><span class="sxs-lookup"><span data-stu-id="f5670-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="f5670-188">將 <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> 新增至 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>：</span><span class="sxs-lookup"><span data-stu-id="f5670-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="f5670-189">防止載入資源</span><span class="sxs-lookup"><span data-stu-id="f5670-189">Prevent loading resources</span></span>

<span data-ttu-id="f5670-190">應用程式元件可以用來*避免*在特定元件或位置中載入資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="f5670-191">新增或移除 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 集合的成員，以隱藏或提供可用的資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="f5670-192">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="f5670-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="f5670-193">先設定 `ApplicationPartManager`，然後再使用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="f5670-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="f5670-194">例如，先設定 `ApplicationPartManager`，再叫用 `AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="f5670-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="f5670-195">呼叫 `ApplicationParts` 集合上的 `Remove` 以移除資源。</span><span class="sxs-lookup"><span data-stu-id="f5670-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="f5670-196">下列程式碼會使用 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> 從應用程式移除 `MyDependentLibrary`： [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="f5670-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="f5670-197">`ApplicationPartManager` 包括下列各部分：</span><span class="sxs-lookup"><span data-stu-id="f5670-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="f5670-198">應用程式的元件和相依元件。</span><span class="sxs-lookup"><span data-stu-id="f5670-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="f5670-199">`Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="f5670-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="f5670-200">`Microsoft.AspNetCore.Mvc.Razor`</span><span class="sxs-lookup"><span data-stu-id="f5670-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="f5670-201">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="f5670-201">Application feature providers</span></span>

<span data-ttu-id="f5670-202">應用程式功能提供者會檢查應用程式元件，並為這些元件提供功能。</span><span class="sxs-lookup"><span data-stu-id="f5670-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="f5670-203">下列 ASP.NET Core 功能有內建的功能提供者：</span><span class="sxs-lookup"><span data-stu-id="f5670-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="f5670-204">控制器</span><span class="sxs-lookup"><span data-stu-id="f5670-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="f5670-205">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="f5670-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="f5670-206">檢視元件</span><span class="sxs-lookup"><span data-stu-id="f5670-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="f5670-207">繼承自 <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1> 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="f5670-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="f5670-208">功能提供者可以針對任何先前列出的功能類型來執行。</span><span class="sxs-lookup"><span data-stu-id="f5670-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="f5670-209">`ApplicationPartManager.FeatureProviders` 中的功能提供者順序可能會影響執行時間行為。</span><span class="sxs-lookup"><span data-stu-id="f5670-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="f5670-210">稍後新增的提供者可以回應先前新增的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="f5670-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="f5670-211">泛型控制器功能</span><span class="sxs-lookup"><span data-stu-id="f5670-211">Generic controller feature</span></span>

<span data-ttu-id="f5670-212">ASP.NET Core 會忽略[泛型控制器](/dotnet/csharp/programming-guide/generics/generic-classes)。</span><span class="sxs-lookup"><span data-stu-id="f5670-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="f5670-213">泛型控制器具有型別參數（例如，`MyController<T>`）。</span><span class="sxs-lookup"><span data-stu-id="f5670-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="f5670-214">下列範例會針對指定的類型清單加入泛型控制器實例：</span><span class="sxs-lookup"><span data-stu-id="f5670-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="f5670-215">類型定義于 `EntityTypes.Types`中：</span><span class="sxs-lookup"><span data-stu-id="f5670-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="f5670-216">系統會將功能提供者新增至 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="f5670-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="f5670-217">用於路由的一般控制器名稱的格式為*GenericController ' 1 [widget]* ，而不是*Widget*。</span><span class="sxs-lookup"><span data-stu-id="f5670-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="f5670-218">下列屬性會修改名稱，以對應至控制器所使用的泛型型別：</span><span class="sxs-lookup"><span data-stu-id="f5670-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="f5670-219">`GenericController` 類別：</span><span class="sxs-lookup"><span data-stu-id="f5670-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="f5670-220">例如，要求 `https://localhost:5001/Sprocket` 的 URL 會產生下列回應：</span><span class="sxs-lookup"><span data-stu-id="f5670-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="f5670-221">顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="f5670-221">Display available features</span></span>

<span data-ttu-id="f5670-222">透過相依性[插入](../../fundamentals/dependency-injection.md)來要求 `ApplicationPartManager`，即可列舉應用程式可用的功能：</span><span class="sxs-lookup"><span data-stu-id="f5670-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="f5670-223">[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2)會使用上述程式碼來顯示應用程式功能：</span><span class="sxs-lookup"><span data-stu-id="f5670-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end