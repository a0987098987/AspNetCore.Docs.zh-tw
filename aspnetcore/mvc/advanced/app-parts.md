---
title: "在 ASP.NET Core 應用程式組件"
author: ardalis
description: "了解如何使用應用程式組件，也就是 abstrations 透過應用程式的資源，設定您的應用程式探索，或避免功能載入的組件。"
keywords: "ASP.NET Core 應用程式組件、 應用程式組件"
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a260675e7461105d4f6a0c61fd13971663c268f2
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="ccd72-104">在 ASP.NET Core 應用程式組件</span><span class="sxs-lookup"><span data-stu-id="ccd72-104">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="ccd72-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccd72-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ccd72-106">*應用程式部分*是的抽象概念的應用程式中，然後再從中 MVC 等功能的控制站，檢視元件的資源，或可能會發現標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ccd72-106">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="ccd72-107">應用程式一部分的其中一個範例是 AssemblyPart，封裝組件參考和公開型別和編譯的參考。</span><span class="sxs-lookup"><span data-stu-id="ccd72-107">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="ccd72-108">*功能提供者*使用應用程式組件，以填入 ASP.NET Core MVC 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="ccd72-108">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="ccd72-109">主要使用案例的應用程式組件可讓您設定您的應用程式探索 （或避免載入） MVC 組件中的功能。</span><span class="sxs-lookup"><span data-stu-id="ccd72-109">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="ccd72-110">介紹應用程式組件</span><span class="sxs-lookup"><span data-stu-id="ccd72-110">Introducing Application Parts</span></span>

<span data-ttu-id="ccd72-111">MVC 應用程式載入從其功能[應用程式組件](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)。</span><span class="sxs-lookup"><span data-stu-id="ccd72-111">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="ccd72-112">特別是， [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart)類別代表組件所支援的應用程式部分。</span><span class="sxs-lookup"><span data-stu-id="ccd72-112">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="ccd72-113">您可以使用這些類別來探索及載入 MVC 功能，例如控制器、 檢視元件、 標記協助程式和 razor 編譯來源。</span><span class="sxs-lookup"><span data-stu-id="ccd72-113">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="ccd72-114">[ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager)負責追蹤 MVC 應用程式的應用程式組件和功能提供者。</span><span class="sxs-lookup"><span data-stu-id="ccd72-114">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="ccd72-115">您可以互動`ApplicationPartManager`中`Startup`MVC 的設定時：</span><span class="sxs-lookup"><span data-stu-id="ccd72-115">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="ccd72-116">根據預設 MVC 會搜尋相依性樹狀結構，並尋找控制站 （即使在其他組件）。</span><span class="sxs-lookup"><span data-stu-id="ccd72-116">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="ccd72-117">若要載入的任意組件 （比方說，外掛程式不是參考在編譯時期），您可以使用應用程式部分。</span><span class="sxs-lookup"><span data-stu-id="ccd72-117">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="ccd72-118">您可以使用應用程式組件，以便*避免*找尋特定組件或位置中控制站。</span><span class="sxs-lookup"><span data-stu-id="ccd72-118">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="ccd72-119">您可以控制哪些組件 （或組件） 可用應用程式藉由修改`ApplicationParts`集合`ApplicationPartManager`。</span><span class="sxs-lookup"><span data-stu-id="ccd72-119">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="ccd72-120">中的項目順序`ApplicationParts`集合並不重要。</span><span class="sxs-lookup"><span data-stu-id="ccd72-120">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="ccd72-121">請務必完全設定`ApplicationPartManager`再用來設定服務容器中。</span><span class="sxs-lookup"><span data-stu-id="ccd72-121">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="ccd72-122">例如，您應該完全設定`ApplicationPartManager`叫用之前`AddControllersAsServices`。</span><span class="sxs-lookup"><span data-stu-id="ccd72-122">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="ccd72-123">這麼做，代表將會確認應用程式組件中的控制站新增之後將不會影響方法呼叫 （將未取得註冊為服務） 這可能會造成不正確的 bevavior 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ccd72-123">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="ccd72-124">如果您有包含控制站，您不想要使用的組件，將它從移除`ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="ccd72-124">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="ccd72-125">除了專案的組件和其相依的組件，`ApplicationPartManager`將包含組件，以便`Microsoft.AspNetCore.Mvc.TagHelpers`和`Microsoft.AspNetCore.Mvc.Razor`預設。</span><span class="sxs-lookup"><span data-stu-id="ccd72-125">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="ccd72-126">應用程式功能提供者</span><span class="sxs-lookup"><span data-stu-id="ccd72-126">Application Feature Providers</span></span>

<span data-ttu-id="ccd72-127">應用程式功能提供者會檢查應用程式組件，並對這些組件提供的功能。</span><span class="sxs-lookup"><span data-stu-id="ccd72-127">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="ccd72-128">有下列的 MVC 功能內建功能提供者：</span><span class="sxs-lookup"><span data-stu-id="ccd72-128">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="ccd72-129">控制器</span><span class="sxs-lookup"><span data-stu-id="ccd72-129">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="ccd72-130">中繼資料參考</span><span class="sxs-lookup"><span data-stu-id="ccd72-130">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="ccd72-131">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ccd72-131">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="ccd72-132">檢視元件</span><span class="sxs-lookup"><span data-stu-id="ccd72-132">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="ccd72-133">功能提供者繼承自`IApplicationFeatureProvider<T>`，其中`T`功能的類型。</span><span class="sxs-lookup"><span data-stu-id="ccd72-133">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="ccd72-134">您可以實作您自己上面所列的任何 MVC 的功能類型提供者的功能。</span><span class="sxs-lookup"><span data-stu-id="ccd72-134">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="ccd72-135">功能中的提供者順序`ApplicationPartManager.FeatureProviders`集合可能會很重要，因為更新的提供者可以做出回應之前的提供者所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="ccd72-135">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="ccd72-136">範例： 泛型控制器功能</span><span class="sxs-lookup"><span data-stu-id="ccd72-136">Sample: Generic controller feature</span></span>

<span data-ttu-id="ccd72-137">根據預設，ASP.NET Core MVC 會忽略一般控制站 (例如， `SomeController<T>`)。</span><span class="sxs-lookup"><span data-stu-id="ccd72-137">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="ccd72-138">這個範例會使用預設提供者之後執行，並將指定之清單的類型的泛型控制器執行個體的控制站功能提供者 (定義於`EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="ccd72-138">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="ccd72-139">實體類型：</span><span class="sxs-lookup"><span data-stu-id="ccd72-139">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="ccd72-140">功能提供者就會加入`Startup`:</span><span class="sxs-lookup"><span data-stu-id="ccd72-140">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="ccd72-141">根據預設，用於路由的泛用的控制器名稱可能會在表單的*GenericController'1 [Widget]*而不是*Widget*。</span><span class="sxs-lookup"><span data-stu-id="ccd72-141">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="ccd72-142">下列屬性用來修改要對應至控制器所使用的泛型類型的名稱：</span><span class="sxs-lookup"><span data-stu-id="ccd72-142">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="ccd72-143">`GenericController`類別：</span><span class="sxs-lookup"><span data-stu-id="ccd72-143">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="ccd72-144">結果中，要求相符的路由時：</span><span class="sxs-lookup"><span data-stu-id="ccd72-144">The result, when a matching route is requested:</span></span>

![輸出範例應用程式中的範例會讀取的 'Hello 從泛型 Sproket 控制器'。](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="ccd72-146">範例： 顯示可用的功能</span><span class="sxs-lookup"><span data-stu-id="ccd72-146">Sample: Display available features</span></span>

<span data-ttu-id="ccd72-147">您可以逐一填入可用的功能加入至應用程式要求`ApplicationPartManager`透過[相依性插入](../../fundamentals/dependency-injection.md)並使用它來填入適當的功能的執行個體：</span><span class="sxs-lookup"><span data-stu-id="ccd72-147">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="ccd72-148">範例輸出：</span><span class="sxs-lookup"><span data-stu-id="ccd72-148">Example output:</span></span>

![範例應用程式中的範例輸出](app-parts/_static/available-features.png)
