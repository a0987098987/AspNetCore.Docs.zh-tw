---
title: 在 ASP.NET Core 中使用應用程式模型
author: ardalis
description: 了解如何讀取及操作應用程式模型，來修改 ASP.NET Core 中 MVC 項目的行為方式。
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/controllers/application-model
ms.openlocfilehash: 5e31d2e6611321bec7442534ce41350de10478e0
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768659"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a><span data-ttu-id="678b0-103">在 ASP.NET Core 中使用應用程式模型</span><span class="sxs-lookup"><span data-stu-id="678b0-103">Work with the application model in ASP.NET Core</span></span>

<span data-ttu-id="678b0-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="678b0-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="678b0-105">ASP.NET Core MVC 定義了一個「應用程式模型」\*\*，代表 MVC 應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="678b0-105">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="678b0-106">您可以讀取及操作此模型來修改 MVC 項目的行為。</span><span class="sxs-lookup"><span data-stu-id="678b0-106">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="678b0-107">根據預設，MVC 遵循特定慣例來判斷哪些類別會被視為控制器、對這些類別的哪些方法是動作，以及參數和路由的行為方式。</span><span class="sxs-lookup"><span data-stu-id="678b0-107">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="678b0-108">您可以自訂此行為，以符合您的應用程式需求，方法是建立自己的慣例，並將其全域套用或作為屬性來套用。</span><span class="sxs-lookup"><span data-stu-id="678b0-108">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="678b0-109">模型和提供者</span><span class="sxs-lookup"><span data-stu-id="678b0-109">Models and Providers</span></span>

<span data-ttu-id="678b0-110">ASP.NET Core MVC 應用程式模型包含抽象介面和描述 MVC 應用程式的具象實作類別。</span><span class="sxs-lookup"><span data-stu-id="678b0-110">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="678b0-111">此模型是 MVC 根據預設慣例來探索應用程式控制器、動作、動作參數、路由和篩選條件的結果。</span><span class="sxs-lookup"><span data-stu-id="678b0-111">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="678b0-112">藉由使用應用程式模型，您可以修改應用程式以遵循與預設 MVC 行為不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="678b0-112">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="678b0-113">參數、名稱、路由和篩選條件全都用作動作與控制器的組態資料。</span><span class="sxs-lookup"><span data-stu-id="678b0-113">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="678b0-114">ASP.NET Core MVC 應用程式模型具有下列結構：</span><span class="sxs-lookup"><span data-stu-id="678b0-114">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="678b0-115">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="678b0-115">ApplicationModel</span></span>
  * <span data-ttu-id="678b0-116">控制器 (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="678b0-116">Controllers (ControllerModel)</span></span>
    * <span data-ttu-id="678b0-117">動作 (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="678b0-117">Actions (ActionModel)</span></span>
      * <span data-ttu-id="678b0-118">參數 (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="678b0-118">Parameters (ParameterModel)</span></span>

<span data-ttu-id="678b0-119">模型的每個層級都可存取共同的 `Properties` 集合，而較低層級可以存取並覆寫階層架構中較高層級所設定的屬性值。</span><span class="sxs-lookup"><span data-stu-id="678b0-119">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="678b0-120">屬性會在建立動作時保存到 `ActionDescriptor.Properties`。</span><span class="sxs-lookup"><span data-stu-id="678b0-120">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="678b0-121">然後當處理要求時，可以透過 `ActionContext.ActionDescriptor.Properties` 來存取慣例所新增或修改的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="678b0-121">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="678b0-122">使用屬性是針對每個動作設定篩選條件及模型繫結器等的好方法。</span><span class="sxs-lookup"><span data-stu-id="678b0-122">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="678b0-123">一旦完成應用程式啟動之後，`ActionDescriptor.Properties` 集合不具備執行緒安全 (對於寫入)。</span><span class="sxs-lookup"><span data-stu-id="678b0-123">The `ActionDescriptor.Properties` collection isn't thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="678b0-124">慣例是安全地將資料新增至此集合的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="678b0-124">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="678b0-125">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="678b0-125">IApplicationModelProvider</span></span>

<span data-ttu-id="678b0-126">ASP.NET Core MVC 使用 [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) 介面定義的提供者模式來載入應用程式模型。</span><span class="sxs-lookup"><span data-stu-id="678b0-126">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="678b0-127">本節涵蓋此提供者運作方式的一些內部實作詳細資料。</span><span class="sxs-lookup"><span data-stu-id="678b0-127">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="678b0-128">這是進階的主題 - 大部分使用應用程式模型的應用程式都應該遵照慣例這樣做。</span><span class="sxs-lookup"><span data-stu-id="678b0-128">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="678b0-129">`IApplicationModelProvider` 介面的實作會彼此「包裝」，每個實作根據其 `Order` 屬性以遞增順序呼叫 `OnProvidersExecuting`。</span><span class="sxs-lookup"><span data-stu-id="678b0-129">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="678b0-130">然後以相反順序呼叫 `OnProvidersExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="678b0-130">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="678b0-131">架構會定義數個提供者：</span><span class="sxs-lookup"><span data-stu-id="678b0-131">The framework defines several providers:</span></span>

<span data-ttu-id="678b0-132">先是 (`Order=-1000`)：</span><span class="sxs-lookup"><span data-stu-id="678b0-132">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="678b0-133">然後 (`Order=-990`)：</span><span class="sxs-lookup"><span data-stu-id="678b0-133">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="678b0-134">具有相同 `Order` 值的兩個提供者的呼叫順序未定義，因此不應依賴它。</span><span class="sxs-lookup"><span data-stu-id="678b0-134">The order in which two providers with the same value for `Order` are called is undefined, and therefore shouldn't be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="678b0-135">`IApplicationModelProvider` 是讓架構作者擴充的進階概念。</span><span class="sxs-lookup"><span data-stu-id="678b0-135">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="678b0-136">一般情況下，應用程式應該使用慣例，而架構應該使用提供者。</span><span class="sxs-lookup"><span data-stu-id="678b0-136">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="678b0-137">主要的差別是提供者一律在慣例之前先執行。</span><span class="sxs-lookup"><span data-stu-id="678b0-137">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="678b0-138">`DefaultApplicationModelProvider` 建立了 ASP.NET Core MVC 使用的許多預設行為。</span><span class="sxs-lookup"><span data-stu-id="678b0-138">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="678b0-139">其負責的部分包括：</span><span class="sxs-lookup"><span data-stu-id="678b0-139">Its responsibilities include:</span></span>

* <span data-ttu-id="678b0-140">將全域篩選條件新增至內容</span><span class="sxs-lookup"><span data-stu-id="678b0-140">Adding global filters to the context</span></span>
* <span data-ttu-id="678b0-141">將控制器新增至內容</span><span class="sxs-lookup"><span data-stu-id="678b0-141">Adding controllers to the context</span></span>
* <span data-ttu-id="678b0-142">將公用控制器方法新增作為動作</span><span class="sxs-lookup"><span data-stu-id="678b0-142">Adding public controller methods as actions</span></span>
* <span data-ttu-id="678b0-143">將動作方法參數新增至內容</span><span class="sxs-lookup"><span data-stu-id="678b0-143">Adding action method parameters to the context</span></span>
* <span data-ttu-id="678b0-144">套用路由和其他屬性</span><span class="sxs-lookup"><span data-stu-id="678b0-144">Applying route and other attributes</span></span>

<span data-ttu-id="678b0-145">某些內建行為由 `DefaultApplicationModelProvider` 實作。</span><span class="sxs-lookup"><span data-stu-id="678b0-145">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="678b0-146">這個提供者[`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)會負責建立，而後者又會參考[`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel)、 [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)和[`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel)實例。</span><span class="sxs-lookup"><span data-stu-id="678b0-146">This provider is responsible for constructing the [`ControllerModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel), [`PropertyModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel) instances.</span></span> <span data-ttu-id="678b0-147">`DefaultApplicationModelProvider` 類別是內部架構實作詳細資料，未來將會變更。</span><span class="sxs-lookup"><span data-stu-id="678b0-147">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="678b0-148">`AuthorizationApplicationModelProvider` 負責套用與 `AuthorizeFilter` 和 `AllowAnonymousFilter` 屬性建立關聯的行為。</span><span class="sxs-lookup"><span data-stu-id="678b0-148">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="678b0-149">[進一步了解這些屬性](xref:security/authorization/simple)。</span><span class="sxs-lookup"><span data-stu-id="678b0-149">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="678b0-150">`CorsApplicationModelProvider` 實作與 `IEnableCorsAttribute` 和 `IDisableCorsAttribute` 建立關聯的行為，以及 `DisableCorsAuthorizationFilter`。</span><span class="sxs-lookup"><span data-stu-id="678b0-150">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="678b0-151">[進一步了解 CORS](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="678b0-151">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="678b0-152">慣例</span><span class="sxs-lookup"><span data-stu-id="678b0-152">Conventions</span></span>

<span data-ttu-id="678b0-153">應用程式模型定義了慣例抽象化，提供比覆寫整個模型或提供者更簡單的方式，來自訂模型的行為。</span><span class="sxs-lookup"><span data-stu-id="678b0-153">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="678b0-154">這些抽象化是用來修改您應用程式行為的建議方式。</span><span class="sxs-lookup"><span data-stu-id="678b0-154">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="678b0-155">慣例會提供方式讓您撰寫程式碼，動態地套用自訂。</span><span class="sxs-lookup"><span data-stu-id="678b0-155">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="678b0-156">雖然[篩選準則](xref:mvc/controllers/filters)提供修改架構行為的方法，但自訂可讓您控制整個應用程式如何一起運作。</span><span class="sxs-lookup"><span data-stu-id="678b0-156">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app works together.</span></span>

<span data-ttu-id="678b0-157">可用的慣例如下：</span><span class="sxs-lookup"><span data-stu-id="678b0-157">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="678b0-158">藉由將慣例新增至 MVC 選項或執行`Attribute`，並將其套用至控制器、動作或動作參數（類似于[`Filters`](xref:mvc/controllers/filters)）來套用。</span><span class="sxs-lookup"><span data-stu-id="678b0-158">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="678b0-159">與篩選條件不同的是，只有在應用程式啟動時才會執行慣例，而不會在每個要求當中執行。</span><span class="sxs-lookup"><span data-stu-id="678b0-159">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="678b0-160">範例：修改 ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="678b0-160">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="678b0-161">下列慣例用來將屬性新增至應用程式模型。</span><span class="sxs-lookup"><span data-stu-id="678b0-161">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="678b0-162">在 `Startup` 中的 `ConfigureServices` 中新增 MVC 時，應用程式模型慣例會套用為選項。</span><span class="sxs-lookup"><span data-stu-id="678b0-162">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="678b0-163">屬性可從控制器動作中的 `ActionDescriptor` 屬性集合存取：</span><span class="sxs-lookup"><span data-stu-id="678b0-163">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="678b0-164">範例：修改 ControllerModel 描述</span><span class="sxs-lookup"><span data-stu-id="678b0-164">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="678b0-165">如同先前的範例，控制器模型也可以修改為包含自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="678b0-165">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="678b0-166">這些會使用應用程式模型中指定的相同名稱，來覆寫現有的屬性。</span><span class="sxs-lookup"><span data-stu-id="678b0-166">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="678b0-167">下列慣例屬性會在控制器層級新增描述：</span><span class="sxs-lookup"><span data-stu-id="678b0-167">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="678b0-168">這個慣例會套用為控制器上的屬性。</span><span class="sxs-lookup"><span data-stu-id="678b0-168">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="678b0-169">"description" 屬性的存取方式與先前範例相同。</span><span class="sxs-lookup"><span data-stu-id="678b0-169">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="678b0-170">範例：修改 ActionModel 描述</span><span class="sxs-lookup"><span data-stu-id="678b0-170">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="678b0-171">個別的屬性規格可以套用至個別的動作，且已在應用程式或控制器層級套用覆寫行為。</span><span class="sxs-lookup"><span data-stu-id="678b0-171">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="678b0-172">將此套用到先前範例控制器內的動作，將示範如何覆寫控制器層級慣例：</span><span class="sxs-lookup"><span data-stu-id="678b0-172">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="678b0-173">範例：修改 ParameterModel</span><span class="sxs-lookup"><span data-stu-id="678b0-173">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="678b0-174">下列的慣例可以套用至動作參數，以修改其 `BindingInfo`。</span><span class="sxs-lookup"><span data-stu-id="678b0-174">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="678b0-175">下列慣例需要參數是路由參數；其他可能的繫結來源 (例如查詢字串值) 都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="678b0-175">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="678b0-176">屬性可套用至任何動作參數：</span><span class="sxs-lookup"><span data-stu-id="678b0-176">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="678b0-177">範例：修改 ActionModel 名稱</span><span class="sxs-lookup"><span data-stu-id="678b0-177">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="678b0-178">下列慣例會修改 `ActionModel` 以更新其所套用之動作的「名稱」\*\*。</span><span class="sxs-lookup"><span data-stu-id="678b0-178">The following convention modifies the `ActionModel` to update the *name* of the action to which it's applied.</span></span> <span data-ttu-id="678b0-179">新的名稱會當作傳給屬性的參數。</span><span class="sxs-lookup"><span data-stu-id="678b0-179">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="678b0-180">這個新名稱由路由使用，因此它會影響用來連線到此動作方法的路由。</span><span class="sxs-lookup"><span data-stu-id="678b0-180">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="678b0-181">這個屬性會套用至 `HomeController` 中的動作方法：</span><span class="sxs-lookup"><span data-stu-id="678b0-181">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="678b0-182">即使方法名稱是 `SomeName`，屬性仍會覆寫使用方法名稱的 MVC 慣例，並將動作名稱取代為 `MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="678b0-182">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="678b0-183">因此，用來連線到此動作的路由是 `/Home/MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="678b0-183">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="678b0-184">這個範例基本上與使用內建 [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) 屬性相同。</span><span class="sxs-lookup"><span data-stu-id="678b0-184">This example is essentially the same as using the built-in [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="678b0-185">範例：自訂路由慣例</span><span class="sxs-lookup"><span data-stu-id="678b0-185">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="678b0-186">您可以使用 `IApplicationModelConvention` 來自訂路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="678b0-186">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="678b0-187">例如，下列慣例會在路由中併入控制器的命名空間，並將命名空間中的 `.` 在路由中取代為 `/`：</span><span class="sxs-lookup"><span data-stu-id="678b0-187">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="678b0-188">慣例會新增為 Startup 中的選項。</span><span class="sxs-lookup"><span data-stu-id="678b0-188">The convention is added as an option in Startup.</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="678b0-189">您可以藉由使用 `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));` 存取 `MvcOptions`，將慣例新增到您的[中介軟體](xref:fundamentals/middleware/index)</span><span class="sxs-lookup"><span data-stu-id="678b0-189">You can add conventions to your [middleware](xref:fundamentals/middleware/index) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="678b0-190">這個範例將此慣例套用到不使用控制器名稱中包含 "Namespace" 之屬性路由的路由。</span><span class="sxs-lookup"><span data-stu-id="678b0-190">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="678b0-191">下列控制器示範此慣例：</span><span class="sxs-lookup"><span data-stu-id="678b0-191">The following controller demonstrates this convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="678b0-192">WebApiCompatShim 中的應用程式模型使用方式</span><span class="sxs-lookup"><span data-stu-id="678b0-192">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="678b0-193">ASP.NET Core MVC 會使用與 ASP.NET Web API 2 不同的一組慣例。</span><span class="sxs-lookup"><span data-stu-id="678b0-193">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="678b0-194">您可以使用自訂慣例，來修改 ASP.NET Core MVC 應用程式的行為，以便與 Web API 應用程式一致。</span><span class="sxs-lookup"><span data-stu-id="678b0-194">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="678b0-195">Microsoft 特別針對這個用途而隨附 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)。</span><span class="sxs-lookup"><span data-stu-id="678b0-195">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="678b0-196">深入了解[從 ASP.NET Web API 移轉](xref:migration/webapi)。</span><span class="sxs-lookup"><span data-stu-id="678b0-196">Learn more about [migration from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="678b0-197">若要使用 Web API 相容性填充碼，您必須將套件新增到您的專案，然後藉由在 `Startup` 中呼叫 `AddWebApiConventions` 來新增慣例：</span><span class="sxs-lookup"><span data-stu-id="678b0-197">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```csharp
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="678b0-198">填充碼提供的慣例僅會套用到已套用特定屬性的應用程式組件。</span><span class="sxs-lookup"><span data-stu-id="678b0-198">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="678b0-199">下列四個屬性用來控制哪些控制器應該讓其慣例由填充碼修改：</span><span class="sxs-lookup"><span data-stu-id="678b0-199">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="678b0-200">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="678b0-200">UseWebApiActionConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="678b0-201">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="678b0-201">UseWebApiOverloadingAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="678b0-202">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="678b0-202">UseWebApiParameterConventionsAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="678b0-203">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="678b0-203">UseWebApiRoutesAttribute</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="678b0-204">動作慣例</span><span class="sxs-lookup"><span data-stu-id="678b0-204">Action Conventions</span></span>

<span data-ttu-id="678b0-205">`UseWebApiActionConventionsAttribute` 用來根據名稱將 HTTP 方法對應到動作 (例如，`Get` 會對應至 `HttpGet`)。</span><span class="sxs-lookup"><span data-stu-id="678b0-205">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="678b0-206">它只適用於不使用屬性路由的動作。</span><span class="sxs-lookup"><span data-stu-id="678b0-206">It only applies to actions that don't use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="678b0-207">多載化</span><span class="sxs-lookup"><span data-stu-id="678b0-207">Overloading</span></span>

<span data-ttu-id="678b0-208">`UseWebApiOverloadingAttribute` 用於套用 `WebApiOverloadingApplicationModelConvention` 慣例。</span><span class="sxs-lookup"><span data-stu-id="678b0-208">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="678b0-209">這個慣例會將 `OverloadActionConstraint` 新增至動作選取程序，這將候選項目動作限制為要求符合其所有非選擇性參數的動作。</span><span class="sxs-lookup"><span data-stu-id="678b0-209">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="678b0-210">參數慣例</span><span class="sxs-lookup"><span data-stu-id="678b0-210">Parameter Conventions</span></span>

<span data-ttu-id="678b0-211">`UseWebApiParameterConventionsAttribute` 用於套用 `WebApiParameterConventionsApplicationModelConvention` 動作慣例。</span><span class="sxs-lookup"><span data-stu-id="678b0-211">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="678b0-212">這個慣例指定用來作為動作參數的簡單類型預設會從 URL 繫結，而複雜類型則會從要求主體繫結。</span><span class="sxs-lookup"><span data-stu-id="678b0-212">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="678b0-213">路由</span><span class="sxs-lookup"><span data-stu-id="678b0-213">Routes</span></span>

<span data-ttu-id="678b0-214">`UseWebApiRoutesAttribute` 控制是否套用 `WebApiApplicationModelConvention` 控制器慣例。</span><span class="sxs-lookup"><span data-stu-id="678b0-214">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="678b0-215">啟用時，這個慣例會用來將[區域](xref:mvc/controllers/areas)的支援新增到路由。</span><span class="sxs-lookup"><span data-stu-id="678b0-215">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="678b0-216">除了一組慣例之外，相容性套件還包括 `System.Web.Http.ApiController` 基底類別，以取代 Web API 提供的類別。</span><span class="sxs-lookup"><span data-stu-id="678b0-216">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="678b0-217">這可讓您針對 Web API 撰寫且繼承自其 `ApiController` 的控制器如同設計般地運作，同時在 ASP.NET Core MVC 上執行。</span><span class="sxs-lookup"><span data-stu-id="678b0-217">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="678b0-218">先前列出的`UseWebApi*`所有屬性都會套用至基底控制器類別。</span><span class="sxs-lookup"><span data-stu-id="678b0-218">All of the `UseWebApi*` attributes listed earlier are applied to the base controller class.</span></span> <span data-ttu-id="678b0-219">`ApiController` 會公開屬性、方法和與 Web API 中之結果類型相容的結果類型。</span><span class="sxs-lookup"><span data-stu-id="678b0-219">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="678b0-220">使用 ApiExplorer 記載您的應用程式</span><span class="sxs-lookup"><span data-stu-id="678b0-220">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="678b0-221">應用程式模型會公開[`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)每個層級上的屬性，以用來流覽應用程式的結構。</span><span class="sxs-lookup"><span data-stu-id="678b0-221">The application model exposes an [`ApiExplorer`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="678b0-222">這可以用來[使用 Swagger 等工具為您的 Web API 產生說明頁面](xref:tutorials/web-api-help-pages-using-swagger)。</span><span class="sxs-lookup"><span data-stu-id="678b0-222">This can be used to [generate help pages for your Web APIs using tools like Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="678b0-223">`ApiExplorer` 屬性會公開 `IsVisible` 屬性，它可以設定來指定應該公開您應用程式模型中的哪些部分。</span><span class="sxs-lookup"><span data-stu-id="678b0-223">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="678b0-224">您可以使用慣例來設定這項設定：</span><span class="sxs-lookup"><span data-stu-id="678b0-224">You can configure this setting using a convention:</span></span>

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="678b0-225">使用此方法 (和其他必要的慣例)，您可以啟用或停用應用程式內任何層級的 API 可見度。</span><span class="sxs-lookup"><span data-stu-id="678b0-225">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
