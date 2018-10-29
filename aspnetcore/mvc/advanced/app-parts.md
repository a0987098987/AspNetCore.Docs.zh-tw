---
title: ASP.NET Core 的應用程式組件
author: ardalis
description: 了解如何使用應用程式組件 (也就是應用程式資源的抽象概念)，以探索或避免載入組件的功能。
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: c0d3ad6bcdf2e56df915b176b28759c59e76faf6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206559"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="aa641-103">ASP.NET Core 的應用程式組件</span><span class="sxs-lookup"><span data-stu-id="aa641-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="aa641-104">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa641-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="aa641-105">「應用程式組件」是應用程式資源的抽象概念，其中您可以探索到控制器、檢視元件或標籤協助程式等 MVC 功能。</span><span class="sxs-lookup"><span data-stu-id="aa641-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="aa641-106">應用程式組件的其中一個範例是 AssemblyPart，其會封裝組件參考，並將類型與編譯參考公開。</span><span class="sxs-lookup"><span data-stu-id="aa641-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="aa641-107">「功能提供者」會使用應用程式組件，來填入 ASP.NET Core MVC 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="aa641-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="aa641-108">應用程式組件的主要使用案例是讓您設定應用程式，以探索 (或避免載入) 組件中的 MVC 功能。</span><span class="sxs-lookup"><span data-stu-id="aa641-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="aa641-109">應用程式組件簡介</span><span class="sxs-lookup"><span data-stu-id="aa641-109">Introducing Application Parts</span></span>

<span data-ttu-id="aa641-110">MVC 應用程式會從[應用程式組件](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)載入其功能。</span><span class="sxs-lookup"><span data-stu-id="aa641-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="aa641-111">值得注意的是，[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) 類別代表組件所支援的應用程式組件。</span><span class="sxs-lookup"><span data-stu-id="aa641-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="aa641-112">您可以使用這些類別來探索及載入 MVC 功能，例如控制器、檢視元件、標籤協助程式和 Razor 編譯來源。</span><span class="sxs-lookup"><span data-stu-id="aa641-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="aa641-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) 負責追蹤適用於 MVC 應用程式的應用程式組件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="aa641-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="aa641-114">您可以在設定 MVC 時與 `Startup` 中的 `ApplicationPartManager` 互動：</span><span class="sxs-lookup"><span data-stu-id="aa641-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="aa641-115">MVC 預設會搜尋相依性樹狀結構，並尋找控制器 (即使位於其他組件中)。</span><span class="sxs-lookup"><span data-stu-id="aa641-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="aa641-116">您可以使用應用程式組件，來載入任意組件 (例如來自未於編譯時期參考的外掛程式)。</span><span class="sxs-lookup"><span data-stu-id="aa641-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="aa641-117">您可以使用應用程式組件，來「避免」尋找特定組件或位置中的控制器。</span><span class="sxs-lookup"><span data-stu-id="aa641-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="aa641-118">藉由修改 `ApplicationPartManager` 的 `ApplicationParts` 集合，您可以控制要提供給應用程式哪些組件。</span><span class="sxs-lookup"><span data-stu-id="aa641-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="aa641-119">`ApplicationParts` 集合中的項目順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="aa641-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="aa641-120">請務必完全設定 `ApplicationPartManager` 之後，再用它來設定容器中的服務。</span><span class="sxs-lookup"><span data-stu-id="aa641-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="aa641-121">例如，您應該完全設定 `ApplicationPartManager` 之後，再叫用 `AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="aa641-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="aa641-122">如果您沒有這麼做，於呼叫該方法之後才新增的應用程式組件控制器就不會受到影響 (亦無法註冊為服務)，而這可能會造成應用程式的行為異常。</span><span class="sxs-lookup"><span data-stu-id="aa641-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="aa641-123">如果組件包含您不想使用的控制器，請將它從 `ApplicationPartManager` 移除：</span><span class="sxs-lookup"><span data-stu-id="aa641-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="aa641-124">除了專案的組件和其相依組件，`ApplicationPartManager` 還預設包含 `Microsoft.AspNetCore.Mvc.TagHelpers` 和 `Microsoft.AspNetCore.Mvc.Razor` 的組件。</span><span class="sxs-lookup"><span data-stu-id="aa641-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="aa641-125">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="aa641-125">Application Feature Providers</span></span>

<span data-ttu-id="aa641-126">應用程式功能提供者會檢查應用程式組件，並對這些組件提供功能。</span><span class="sxs-lookup"><span data-stu-id="aa641-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="aa641-127">下列 MVC 功能均內建功能提供者：</span><span class="sxs-lookup"><span data-stu-id="aa641-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="aa641-128">控制器</span><span class="sxs-lookup"><span data-stu-id="aa641-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="aa641-129">中繼資料參考</span><span class="sxs-lookup"><span data-stu-id="aa641-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="aa641-130">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="aa641-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="aa641-131">檢視元件</span><span class="sxs-lookup"><span data-stu-id="aa641-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="aa641-132">繼承自 `IApplicationFeatureProvider<T>` 的功能提供者，其中 `T` 是功能的類型。</span><span class="sxs-lookup"><span data-stu-id="aa641-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="aa641-133">針對上述所列的任何 MVC 功能類型，您可以實作自己的功能提供者。</span><span class="sxs-lookup"><span data-stu-id="aa641-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="aa641-134">在 `ApplicationPartManager.FeatureProviders` 集合中，功能提供者順序可能相當重要，因為更新版本的提供者可以依據舊版提供者所採取的動作做出回應。</span><span class="sxs-lookup"><span data-stu-id="aa641-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="aa641-135">範例：泛型控制器功能</span><span class="sxs-lookup"><span data-stu-id="aa641-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="aa641-136">根據預設，ASP.NET Core MVC 會忽略泛型控制器 (例如 `SomeController<T>`)。</span><span class="sxs-lookup"><span data-stu-id="aa641-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="aa641-137">這個範例使用的控制器功能提供者，會在預設提供者之後執行，並新增指定類型清單 (定義於 `EntityTypes.Types`) 的泛型控制器執行個體：</span><span class="sxs-lookup"><span data-stu-id="aa641-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="aa641-138">實體類型：</span><span class="sxs-lookup"><span data-stu-id="aa641-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="aa641-139">系統會將功能提供者新增至 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="aa641-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="aa641-140">根據預設，用於路由的泛用控制器名稱格式為 *GenericController'1[Widget]* 而不是 *Widget*。</span><span class="sxs-lookup"><span data-stu-id="aa641-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="aa641-141">您可以使用下列屬性修改名稱，以對應至控制器所使用的泛型型別：</span><span class="sxs-lookup"><span data-stu-id="aa641-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="aa641-142">`GenericController` 類別：</span><span class="sxs-lookup"><span data-stu-id="aa641-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="aa641-143">要求相符路由時的結果為：</span><span class="sxs-lookup"><span data-stu-id="aa641-143">The result, when a matching route is requested:</span></span>

![來自範例應用程式的輸出範例會顯示 'Hello from a generic Sproket controller'。](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="aa641-145">範例：顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="aa641-145">Sample: Display available features</span></span>

<span data-ttu-id="aa641-146">您可以透過[相依性插入](../../fundamentals/dependency-injection.md)要求 `ApplicationPartManager`，並使用它來填入適當的功能執行個體，以逐一查看適用於應用程式的已填入功能：</span><span class="sxs-lookup"><span data-stu-id="aa641-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="aa641-147">範例輸出：</span><span class="sxs-lookup"><span data-stu-id="aa641-147">Example output:</span></span>

![來自範例應用程式的範例輸出](app-parts/_static/available-features.png)
