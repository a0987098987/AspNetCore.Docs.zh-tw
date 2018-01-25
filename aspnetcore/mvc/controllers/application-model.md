---
title: "使用應用程式模型"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: a0913edaab723656c9be484332e02c551a5c88e1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="26402-102">使用應用程式模型</span><span class="sxs-lookup"><span data-stu-id="26402-102">Working with the Application Model</span></span>

<span data-ttu-id="26402-103">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="26402-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="26402-104">定義 ASP.NET Core MVC*應用程式模型*代表 MVC 應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="26402-104">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="26402-105">您可以讀取及操作此模型來修改 MVC 元件的行為方式。</span><span class="sxs-lookup"><span data-stu-id="26402-105">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="26402-106">根據預設，MVC 會遵循特定慣例，來判斷哪些類別會被視為控制站，這些類別的方法是動作，以及參數和路由的行為方式。</span><span class="sxs-lookup"><span data-stu-id="26402-106">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="26402-107">您可以自訂此行為，以符合您的應用程式需求建立您自己的慣例，並將其套用全域方式或屬性為根據。</span><span class="sxs-lookup"><span data-stu-id="26402-107">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="26402-108">模型和提供者</span><span class="sxs-lookup"><span data-stu-id="26402-108">Models and Providers</span></span>

<span data-ttu-id="26402-109">ASP.NET Core MVC 應用程式模型包含抽象介面和說明 MVC 應用程式的具象實作類別。</span><span class="sxs-lookup"><span data-stu-id="26402-109">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="26402-110">此模型是 MVC 應用程式的控制器、 動作、 動作參數、 路由和篩選器，根據預設慣例探索結果。</span><span class="sxs-lookup"><span data-stu-id="26402-110">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="26402-111">藉由使用應用程式模型，您可以修改您的應用程式遵循預設 MVC 行為的不同慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-111">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="26402-112">參數、 名稱、 路由和篩選所有可用做為組態資料的動作與控制器。</span><span class="sxs-lookup"><span data-stu-id="26402-112">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="26402-113">ASP.NET Core MVC 應用程式模型具有下列結構：</span><span class="sxs-lookup"><span data-stu-id="26402-113">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="26402-114">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="26402-114">ApplicationModel</span></span>
    * <span data-ttu-id="26402-115">控制站 (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="26402-115">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="26402-116">動作 (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="26402-116">Actions (ActionModel)</span></span>
            * <span data-ttu-id="26402-117">參數 (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="26402-117">Parameters (ParameterModel)</span></span>

<span data-ttu-id="26402-118">模型的每個層級對一般存取`Properties`集合中，而較低層級可以存取，並覆寫階層架構中較高的層級所設定的屬性值。</span><span class="sxs-lookup"><span data-stu-id="26402-118">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="26402-119">屬性會保存到`ActionDescriptor.Properties`建立動作時。</span><span class="sxs-lookup"><span data-stu-id="26402-119">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="26402-120">然後時在處理要求時，任何屬性的加入或修改的慣例可以透過存取`ActionContext.ActionDescriptor.Properties`。</span><span class="sxs-lookup"><span data-stu-id="26402-120">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="26402-121">使用屬性會設定您的篩選器、 模型繫結器等每個動作為基礎的好方法。</span><span class="sxs-lookup"><span data-stu-id="26402-121">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="26402-122">`ActionDescriptor.Properties`收集不具備執行緒安全 （適用於寫入），一旦完成應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="26402-122">The `ActionDescriptor.Properties` collection isn't thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="26402-123">慣例會安全地將資料加入至這個集合的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="26402-123">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="26402-124">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="26402-124">IApplicationModelProvider</span></span>

<span data-ttu-id="26402-125">ASP.NET Core MVC 載入應用程式模型使用定義的提供者模式[IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider)介面。</span><span class="sxs-lookup"><span data-stu-id="26402-125">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="26402-126">本節涵蓋一些內部實作詳細資料的方式這個提供者的功能。</span><span class="sxs-lookup"><span data-stu-id="26402-126">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="26402-127">這是進階的主題-大部分應用程式使用之應用程式模型都應該這樣做所使用的慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-127">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="26402-128">實作`IApplicationModelProvider`介面 「 包裝 」，與每個實作呼叫`OnProvidersExecuting`以遞增順序根據其`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-128">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="26402-129">`OnProvidersExecuted`然後以相反順序呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="26402-129">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="26402-130">架構會定義數個提供者：</span><span class="sxs-lookup"><span data-stu-id="26402-130">The framework defines several providers:</span></span>

<span data-ttu-id="26402-131">第一個 (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="26402-131">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="26402-132">然後 (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="26402-132">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="26402-133">具有相同值的兩個提供者中的順序`Order`稱為未定義，並因此不應憑藉此方式。</span><span class="sxs-lookup"><span data-stu-id="26402-133">The order in which two providers with the same value for `Order` are called is undefined, and therefore shouldn't be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="26402-134">`IApplicationModelProvider`是擴充的架構作者進階的概念。</span><span class="sxs-lookup"><span data-stu-id="26402-134">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="26402-135">一般情況下，應用程式應該使用的慣例，因此架構應該使用提供者。</span><span class="sxs-lookup"><span data-stu-id="26402-135">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="26402-136">主要的差別是提供者，一律執行之前慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-136">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="26402-137">`DefaultApplicationModelProvider`建立許多使用 ASP.NET Core MVC 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="26402-137">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="26402-138">其負責的部分包括：</span><span class="sxs-lookup"><span data-stu-id="26402-138">Its responsibilities include:</span></span>

* <span data-ttu-id="26402-139">將全域篩選條件加入至內容</span><span class="sxs-lookup"><span data-stu-id="26402-139">Adding global filters to the context</span></span>
* <span data-ttu-id="26402-140">將控制站新增至內容</span><span class="sxs-lookup"><span data-stu-id="26402-140">Adding controllers to the context</span></span>
* <span data-ttu-id="26402-141">新增公用控制器方法當做動作</span><span class="sxs-lookup"><span data-stu-id="26402-141">Adding public controller methods as actions</span></span>
* <span data-ttu-id="26402-142">將動作方法參數加入至內容</span><span class="sxs-lookup"><span data-stu-id="26402-142">Adding action method parameters to the context</span></span>
* <span data-ttu-id="26402-143">套用路由和其他屬性</span><span class="sxs-lookup"><span data-stu-id="26402-143">Applying route and other attributes</span></span>

<span data-ttu-id="26402-144">某些內建行為由實作`DefaultApplicationModelProvider`。</span><span class="sxs-lookup"><span data-stu-id="26402-144">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="26402-145">此提供者會負責建構[ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)，會接著參考[ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)， [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)，和[`ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel)執行個體。</span><span class="sxs-lookup"><span data-stu-id="26402-145">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="26402-146">`DefaultApplicationModelProvider`類別是可以的會在未來變更的內部架構實作詳細資料。</span><span class="sxs-lookup"><span data-stu-id="26402-146">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="26402-147">`AuthorizationApplicationModelProvider`會負責套用與相關聯的行為`AuthorizeFilter`和`AllowAnonymousFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-147">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="26402-148">[深入了解這些屬性](xref:security/authorization/simple)。</span><span class="sxs-lookup"><span data-stu-id="26402-148">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="26402-149">`CorsApplicationModelProvider`實作相關聯的行為`IEnableCorsAttribute`和`IDisableCorsAttribute`，而`DisableCorsAuthorizationFilter`。</span><span class="sxs-lookup"><span data-stu-id="26402-149">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="26402-150">[深入了解 CORS](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="26402-150">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="26402-151">慣例</span><span class="sxs-lookup"><span data-stu-id="26402-151">Conventions</span></span>

<span data-ttu-id="26402-152">應用程式模型定義慣例的抽象概念，提供更簡單的方式，自訂的模型，而不覆寫整個模型或提供者的行為。</span><span class="sxs-lookup"><span data-stu-id="26402-152">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="26402-153">這些抽象化是建議用來修改您的應用程式行為。</span><span class="sxs-lookup"><span data-stu-id="26402-153">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="26402-154">慣例會提供您撰寫程式碼，將會動態地套用自訂的方式。</span><span class="sxs-lookup"><span data-stu-id="26402-154">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="26402-155">雖然[篩選](xref:mvc/controllers/filters)提供方法來修改架構的行為，自訂項目可讓您控制如何整個應用程式會連接在一起。</span><span class="sxs-lookup"><span data-stu-id="26402-155">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="26402-156">可使用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="26402-156">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="26402-157">將它們加入至 MVC 選項，或實作，會套用慣例`Attribute`s，並將其套用至控制器、 動作或動作的參數 (類似於[ `Filters` ](xref:mvc/controllers/filters))。</span><span class="sxs-lookup"><span data-stu-id="26402-157">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="26402-158">與篩選不同的是應用程式啟動時，不會為每個要求的一部分時，才會執行慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-158">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="26402-159">範例： 修改 ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="26402-159">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="26402-160">慣例用來將屬性加入至應用程式模型。</span><span class="sxs-lookup"><span data-stu-id="26402-160">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="26402-161">MVC 加入時，將會套用做為選項的應用程式模型慣例`ConfigureServices`中`Startup`。</span><span class="sxs-lookup"><span data-stu-id="26402-161">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="26402-162">屬性是從存取`ActionDescriptor`控制器動作中的屬性集合：</span><span class="sxs-lookup"><span data-stu-id="26402-162">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="26402-163">範例： 修改 ControllerModel 描述</span><span class="sxs-lookup"><span data-stu-id="26402-163">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="26402-164">如同先前的範例中，控制器模型也可以修改為包含自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-164">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="26402-165">這些會與應用程式模型中指定的相同名稱，覆寫現有的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-165">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="26402-166">下列慣例屬性新增了描述，在控制器層級：</span><span class="sxs-lookup"><span data-stu-id="26402-166">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="26402-167">這個慣例會套用做為控制站上的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-167">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="26402-168">如先前範例所示的相同方式存取 [描述] 屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-168">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="26402-169">範例： 修改 ActionModel 描述</span><span class="sxs-lookup"><span data-stu-id="26402-169">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="26402-170">個別的屬性規格可以套用至個別的動作，覆寫已套用到應用程式或控制器層級的行為。</span><span class="sxs-lookup"><span data-stu-id="26402-170">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="26402-171">將此套用到先前的範例控制器內的動作將示範如何覆寫的控制器層級慣例：</span><span class="sxs-lookup"><span data-stu-id="26402-171">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="26402-172">範例： 修改 ParameterModel</span><span class="sxs-lookup"><span data-stu-id="26402-172">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="26402-173">下列的慣例可以套用至動作參數，若要修改其`BindingInfo`。</span><span class="sxs-lookup"><span data-stu-id="26402-173">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="26402-174">下列慣例需要的參數是路由參數;其他可能的繫結來源 （例如查詢字串值） 都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="26402-174">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="26402-175">屬性會套用至任何動作參數：</span><span class="sxs-lookup"><span data-stu-id="26402-175">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="26402-176">範例： 修改 ActionModel 名稱</span><span class="sxs-lookup"><span data-stu-id="26402-176">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="26402-177">修改下列慣例`ActionModel`更新*名稱*来套用的動作。</span><span class="sxs-lookup"><span data-stu-id="26402-177">The following convention modifies the `ActionModel` to update the *name* of the action to which it's applied.</span></span> <span data-ttu-id="26402-178">提供新的名稱做為參數的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-178">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="26402-179">這個新名稱會使用路由，因此它會影響用來連線到此動作方法的路由。</span><span class="sxs-lookup"><span data-stu-id="26402-179">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="26402-180">這個屬性會套用至動作方法中`HomeController`:</span><span class="sxs-lookup"><span data-stu-id="26402-180">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="26402-181">即使方法名稱是`SomeName`，屬性會使用方法名稱的 MVC 慣例覆寫並取代的動作名稱與`MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="26402-181">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="26402-182">因此，用來連線到此動作的路由是`/Home/MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="26402-182">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="26402-183">這個範例基本上就是使用內建[ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-183">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="26402-184">範例： 自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="26402-184">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="26402-185">您可以使用`IApplicationModelConvention`自訂如何路由的運作。</span><span class="sxs-lookup"><span data-stu-id="26402-185">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="26402-186">例如，下列慣例會併入控制站的命名空間的路由，取代`.`以命名空間中`/`路由中：</span><span class="sxs-lookup"><span data-stu-id="26402-186">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="26402-187">慣例是在啟動的選項加入。</span><span class="sxs-lookup"><span data-stu-id="26402-187">The convention is added as an option in Startup.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="26402-188">您可以加入慣例，以便您[中介軟體](xref:fundamentals/middleware)藉由存取`MvcOptions`使用`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="26402-188">You can add conventions to your [middleware](xref:fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="26402-189">這個範例適用於這個慣例不使用屬性路由控制器之 「 命名空間 」 在其名稱的路由。</span><span class="sxs-lookup"><span data-stu-id="26402-189">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="26402-190">下列的控制器會示範這個慣例：</span><span class="sxs-lookup"><span data-stu-id="26402-190">The following controller demonstrates this convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="26402-191">WebApiCompatShim 中的應用程式模型使用方式</span><span class="sxs-lookup"><span data-stu-id="26402-191">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="26402-192">ASP.NET Core MVC 會使用一組不同的慣例從 ASP.NET Web API 2。</span><span class="sxs-lookup"><span data-stu-id="26402-192">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="26402-193">您可以使用自訂的慣例，來修改 ASP.NET Core MVC 應用程式的行為與 Web API 應用程式的一致。</span><span class="sxs-lookup"><span data-stu-id="26402-193">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="26402-194">Microsoft 隨附[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)專用於此用途。</span><span class="sxs-lookup"><span data-stu-id="26402-194">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="26402-195">深入了解[從 ASP.NET Web API 移轉](xref:migration/webapi)。</span><span class="sxs-lookup"><span data-stu-id="26402-195">Learn more about [migrating from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="26402-196">若要使用 Web 應用程式開發介面相容性填充碼，您必須將封裝加入您的專案，然後新增藉由呼叫 MVC 慣例`AddWebApiConventions`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="26402-196">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="26402-197">提供的填充碼的慣例僅適用於應用程式都已經套用至其特定屬性的組件。</span><span class="sxs-lookup"><span data-stu-id="26402-197">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="26402-198">下列四個屬性可用來控制哪些控制站應該有其填充碼的慣例所修改的慣例：</span><span class="sxs-lookup"><span data-stu-id="26402-198">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="26402-199">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="26402-199">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="26402-200">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="26402-200">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="26402-201">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="26402-201">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="26402-202">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="26402-202">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="26402-203">動作慣例</span><span class="sxs-lookup"><span data-stu-id="26402-203">Action Conventions</span></span>

<span data-ttu-id="26402-204">`UseWebApiActionConventionsAttribute`用來根據其名稱的動作對應的 HTTP 方法 (例如，`Get`會將對應至`HttpGet`)。</span><span class="sxs-lookup"><span data-stu-id="26402-204">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="26402-205">它只適用於不使用屬性路由的動作。</span><span class="sxs-lookup"><span data-stu-id="26402-205">It only applies to actions that don't use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="26402-206">多載化</span><span class="sxs-lookup"><span data-stu-id="26402-206">Overloading</span></span>

<span data-ttu-id="26402-207">`UseWebApiOverloadingAttribute`用於套用`WebApiOverloadingApplicationModelConvention`慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-207">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="26402-208">這個慣例新增`OverloadActionConstraint`動作選取程序，以其限制為其要求符合所有的非選擇性參數的候選項目動作。</span><span class="sxs-lookup"><span data-stu-id="26402-208">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="26402-209">參數慣例</span><span class="sxs-lookup"><span data-stu-id="26402-209">Parameter Conventions</span></span>

<span data-ttu-id="26402-210">`UseWebApiParameterConventionsAttribute`用於套用`WebApiParameterConventionsApplicationModelConvention`動作慣例。</span><span class="sxs-lookup"><span data-stu-id="26402-210">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="26402-211">這個慣例指定簡單型別做為動作參數會繫結從 URI 根據預設，而複雜型別會從要求主體繫結。</span><span class="sxs-lookup"><span data-stu-id="26402-211">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="26402-212">路由</span><span class="sxs-lookup"><span data-stu-id="26402-212">Routes</span></span>

<span data-ttu-id="26402-213">`UseWebApiRoutesAttribute`控制項是否`WebApiApplicationModelConvention`控制器慣例會套用。</span><span class="sxs-lookup"><span data-stu-id="26402-213">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="26402-214">啟用時，這個慣例可用來將支援[區域](xref:mvc/controllers/areas)路由。</span><span class="sxs-lookup"><span data-stu-id="26402-214">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="26402-215">相容性封裝包括一組慣例，除了`System.Web.Http.ApiController`基底類別，以取代提供的 Web API。</span><span class="sxs-lookup"><span data-stu-id="26402-215">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="26402-216">這可讓您撰寫的 Web API 和繼承的控制站從其`ApiController`運作，如同它們設計，ASP.NET Core MVC 上執行時運作。</span><span class="sxs-lookup"><span data-stu-id="26402-216">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="26402-217">這個基底控制器類別使用的所有裝飾`UseWebApi*`上面所列的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-217">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="26402-218">`ApiController`公開屬性、 方法和相容的 Web API 中的結果型別。</span><span class="sxs-lookup"><span data-stu-id="26402-218">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="26402-219">使用 ApiExplorer 記載您的應用程式</span><span class="sxs-lookup"><span data-stu-id="26402-219">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="26402-220">應用程式模型會公開[ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)可用來周遊應用程式的結構，每個層級的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-220">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="26402-221">這可以用來[您 Web 應用程式開發介面使用 Swagger 等工具產生的說明頁面](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger)。</span><span class="sxs-lookup"><span data-stu-id="26402-221">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="26402-222">`ApiExplorer`屬性會公開`IsVisible`可以指定應該公開您的應用程式模型中哪些部分設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="26402-222">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="26402-223">您可以設定這項設定使用的慣例：</span><span class="sxs-lookup"><span data-stu-id="26402-223">You can configure this setting using a convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="26402-224">使用此方法 （和其他必要的慣例），您可以啟用或停用應用程式內的任何層級的應用程式開發介面可見性。</span><span class="sxs-lookup"><span data-stu-id="26402-224">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
