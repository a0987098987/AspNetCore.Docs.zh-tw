---
title: "篩選條件"
author: ardalis
description: "深入了解「篩選條件」的運作方式，以及如何在 ASP.NET Core MVC 中使用它們。"
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 2ba3c226cc57f8a3fb26b4119ae9e575eff522f9
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="filters"></a><span data-ttu-id="b8b6c-103">篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-103">Filters</span></span>

<span data-ttu-id="b8b6c-104">作者：[Tom Dykstra](https://github.com/tdykstra/) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b8b6c-104">By [Tom Dykstra](https://github.com/tdykstra/) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b8b6c-105">ASP.NET Core MVC 中的「篩選條件」可讓您在要求處理管線的特定階段之前或之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-105">*Filters* in ASP.NET Core MVC allow you to run code before or after certain stages in the request processing pipeline.</span></span>

 <span data-ttu-id="b8b6c-106">內建篩選條件處理的工作像是例如授權 (防止存取使用者未獲授權的資源)、確保所有要求都使用 HTTPS，以及回應快取 (縮短要求管線，以傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-106">Built-in filters handle tasks such as authorization (preventing access to resources a user isn't authorized for), ensuring that all requests use HTTPS, and response caching (short-circuiting the request pipeline to return a cached response).</span></span> 

<span data-ttu-id="b8b6c-107">您可以建立自訂篩選條件來處理應用程式的跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-107">You can create custom filters to handle cross-cutting concerns for your application.</span></span> <span data-ttu-id="b8b6c-108">每當您想要避免在動作之間重複程式碼，篩選條件便是解決方案。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-108">Anytime you want to avoid duplicating code across actions, filters are the solution.</span></span> <span data-ttu-id="b8b6c-109">例如，您可以將錯誤處理程式碼合併在例外狀況篩選條件中。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-109">For example, you can consolidate error handling code in a exception filter.</span></span>

<span data-ttu-id="b8b6c-110">[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-110">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

## <a name="how-do-filters-work"></a><span data-ttu-id="b8b6c-111">篩選條件如何運作？</span><span class="sxs-lookup"><span data-stu-id="b8b6c-111">How do filters work?</span></span>

<span data-ttu-id="b8b6c-112">篩選條件在「MVC 動作引動過程管線」中執行，這有時又稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-112">Filters run within the *MVC action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="b8b6c-113">MVC 之後的篩選條件管線執行會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-113">The filter pipeline runs after MVC selects the action to execute.</span></span>

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 MVC 動作引動過程管線。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="b8b6c-116">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="b8b6c-116">Filter types</span></span>

<span data-ttu-id="b8b6c-117">每個篩選類型會在篩選條件管線中的不同階段執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-117">Each filter type is executed at a different stage in the filter pipeline.</span></span>

* <span data-ttu-id="b8b6c-118">[授權篩選條件](#authorization-filters)會先執行，用來判斷目前使用者是否被授權提出目前的要求。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-118">[Authorization filters](#authorization-filters) run first and are used to determine whether the current user is authorized for the current request.</span></span> <span data-ttu-id="b8b6c-119">如果要求未經授權，它們可以縮短管線。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-119">They can short-circuit the pipeline if a request is unauthorized.</span></span> 

* <span data-ttu-id="b8b6c-120">[資源篩選條件](#resource-filters)是授權之後最先處理要求的。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-120">[Resource filters](#resource-filters) are the first to handle a request after authorization.</span></span>  <span data-ttu-id="b8b6c-121">它們可以在篩選條件管線的其餘部分之前執行程式碼，以及在管線的其餘部分完成之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-121">They can run code before the rest of the filter pipeline, and after the rest of the pipeline has completed.</span></span> <span data-ttu-id="b8b6c-122">基於效能的考量，它們適合用來實作快取或以其他方式縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-122">They're useful to implement caching or otherwise short-circuit the filter pipeline for performance reasons.</span></span> <span data-ttu-id="b8b6c-123">因為它們在模型繫結之前執行，所以適合需要影響模型繫結的任何項目。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-123">Since they run before model binding, they're useful for anything that needs to influence model binding.</span></span>

* <span data-ttu-id="b8b6c-124">[動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-124">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="b8b6c-125">它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-125">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span>

* <span data-ttu-id="b8b6c-126">[例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-126">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="b8b6c-127">[結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-127">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="b8b6c-128">只有當動作方法執行成功，而且可用於必須包圍檢視或格式器執行的邏輯時，它們才會執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-128">They run only when the action method has executed successfully and are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="b8b6c-129">下圖顯示這些篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-129">The following diagram shows how these filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="b8b6c-132">實作</span><span class="sxs-lookup"><span data-stu-id="b8b6c-132">Implementation</span></span>

<span data-ttu-id="b8b6c-133">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-133">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span> <span data-ttu-id="b8b6c-134">請根據您需要執行的工作類型，選擇同步或非同步的版本。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-134">Choose either the sync or async variant depending on the kind of task you need to perform.</span></span> 

<span data-ttu-id="b8b6c-135">同步篩選條件可以在管線階段之前和之後執行程式碼，它們定義 On*Stage*Executing 以及 On*Stage*Executed 方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-135">Synchronous filters that can run code both before and after their pipeline stage define On*Stage*Executing and On*Stage*Executed methods.</span></span> <span data-ttu-id="b8b6c-136">例如，`OnActionExecuting` 會在呼叫動作方法之前呼叫，而 `OnActionExecuted` 則在動作方法傳回之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-136">For example, `OnActionExecuting` is called before the action method is called, and `OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

<span data-ttu-id="b8b6c-137">非同步篩選條件會定義單一的 On*Stage*ExecutionAsync 方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-137">Asynchronous filters define a single On*Stage*ExecutionAsync method.</span></span> <span data-ttu-id="b8b6c-138">這個方法會接受 *FilterType*ExecutionDelegate 委派，它會執行篩選條件的管線階段。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-138">This method takes a *FilterType*ExecutionDelegate delegate which executes the filter's pipeline stage.</span></span> <span data-ttu-id="b8b6c-139">例如，`ActionExecutionDelegate` 呼叫動作方法，而您可以在呼叫它之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-139">For example, `ActionExecutionDelegate` calls the action method, and you can execute code before and after you call it.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

<span data-ttu-id="b8b6c-140">您可以為單一類別中的多個篩選條件階段實作介面。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-140">You can implement interfaces for multiple filter stages in a single class.</span></span> <span data-ttu-id="b8b6c-141">例如，[Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) 抽象類別會同時實作 `IActionFilter` 和 `IResultFilter`，以及它們的非同步對等項目。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-141">For example, the [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) abstract class implements both `IActionFilter` and `IResultFilter`, as well as their async equivalents.</span></span>

> [!NOTE]
> <span data-ttu-id="b8b6c-142">請實作同步**或**非同步版本的篩選條件介面，但不要兩者同時實作。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-142">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="b8b6c-143">架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-143">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="b8b6c-144">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-144">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="b8b6c-145">如果您要在一個類別上同時實作兩種介面，則只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-145">If you were to implement both interfaces on one class, only the async method would be called.</span></span> <span data-ttu-id="b8b6c-146">使用抽象類別時，例如 [Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)，您只會覆寫同步方法或每個篩選條件類型的非同步方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-146">When using abstract classes like [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) you would override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="ifilterfactory"></a><span data-ttu-id="b8b6c-147">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="b8b6c-147">IFilterFactory</span></span>

<span data-ttu-id="b8b6c-148">`IFilterFactory` 會實作 `IFilter`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-148">`IFilterFactory` implements `IFilter`.</span></span> <span data-ttu-id="b8b6c-149">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilter` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-149">Therefore, an `IFilterFactory` instance can be used as an `IFilter` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="b8b6c-150">當架構準備要叫用篩選條件時，會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-150">When the framework prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="b8b6c-151">如果該轉換成功，會呼叫 `CreateInstance` 方法來建立將被叫用的 `IFilter`執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-151">If that cast succeeds, the `CreateInstance` method is called to create the `IFilter` instance that will be invoked.</span></span> <span data-ttu-id="b8b6c-152">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有相當彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-152">This provides a very flexible design, since the precise filter pipeline doesn't need to be set explicitly when the application starts.</span></span>

<span data-ttu-id="b8b6c-153">您可以對您自己的屬性實作，實作 `IFilterFactory` 以作為另一種建立篩選條件的方法：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-153">You can implement `IFilterFactory` on your own attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a><span data-ttu-id="b8b6c-154">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="b8b6c-154">Built-in filter attributes</span></span>

<span data-ttu-id="b8b6c-155">架構包含內建的屬性型篩選條件，您可以進行子分類和自訂。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-155">The framework includes built-in attribute-based filters that you can subclass and customize.</span></span> <span data-ttu-id="b8b6c-156">例如，下列結果篩選條件會將標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-156">For example, the following Result filter adds a header to the response.</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

<span data-ttu-id="b8b6c-157">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-157">Attributes allow filters to accept arguments, as shown in the example above.</span></span> <span data-ttu-id="b8b6c-158">您會將此屬性新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-158">You would add this attribute to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="b8b6c-159">`Index` 動作的結果如下所示 - 回應標頭顯示在右下方。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-159">The result of the `Index` action is shown below - the response headers are displayed on the bottom right.</span></span>

![顯示回應標頭的 Microsoft Edge 開發人員工具，包括作者 Steve Smith @ardalis](filters/_static/add-header.png)

<span data-ttu-id="b8b6c-161">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-161">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="b8b6c-162">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-162">Filter attributes:</span></span>

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

<span data-ttu-id="b8b6c-163">[本文稍後](#dependency-injection)將說明 `TypeFilterAttribute` 和 `ServiceFilterAttribute`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-163">`TypeFilterAttribute` and `ServiceFilterAttribute` are explained [later in this article](#dependency-injection).</span></span>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="b8b6c-164">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="b8b6c-164">Filter scopes and order of execution</span></span>

<span data-ttu-id="b8b6c-165">篩選條件可以新增至管線的三個「範圍」之一。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-165">A filter can be added to the pipeline at one of three *scopes*.</span></span> <span data-ttu-id="b8b6c-166">您可以使用屬性將篩選條件新增至特定的動作方法或控制器類別。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-166">You can add a filter to a particular action method or to a controller class by using an attribute.</span></span> <span data-ttu-id="b8b6c-167">您可以將篩選條件新增至 `Startup` 類別之 `ConfigureServices` 方法中的 `MvcOptions.Filters` 集合，以便全域地註冊篩選條件 (適用於所有控制器和動作)：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-167">Or you can register a filter globally (for all controllers and actions) by adding it to the `MvcOptions.Filters` collection in the `ConfigureServices` method in the `Startup` class:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a><span data-ttu-id="b8b6c-168">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="b8b6c-168">Default order of execution</span></span>

<span data-ttu-id="b8b6c-169">當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="b8b6c-170">全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-170">Global filters surround class filters, which in turn surround method filters.</span></span> <span data-ttu-id="b8b6c-171">這有時候稱為「俄羅斯娃娃」巢狀結構，因為範圍的每次增加都圍繞著前一個範圍，就像[俄羅斯娃娃](https://wikipedia.org/wiki/Matryoshka_doll)一樣。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-171">This is sometimes referred to as "Russian doll" nesting, as each increase in scope is wrapped around the previous scope, like a [nesting doll](https://wikipedia.org/wiki/Matryoshka_doll).</span></span> <span data-ttu-id="b8b6c-172">您通常不必明確決定順序，即可得到想要的覆寫行為。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-172">You generally get the desired overriding behavior without having to explicitly determine ordering.</span></span>

<span data-ttu-id="b8b6c-173">因為這個巢狀結構，篩選條件的「之後」程式碼，執行的順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-173">As a result of this nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="b8b6c-174">順序看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-174">The sequence looks like this:</span></span>

* <span data-ttu-id="b8b6c-175">全域套用的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-175">The *before* code of filters applied globally</span></span>
  * <span data-ttu-id="b8b6c-176">套用至控制器的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-176">The *before* code of filters applied to controllers</span></span>
    * <span data-ttu-id="b8b6c-177">套用至動作方法的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-177">The *before* code of filters applied to action methods</span></span>
    * <span data-ttu-id="b8b6c-178">套用至動作方法的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-178">The *after* code of filters applied to action methods</span></span>
  * <span data-ttu-id="b8b6c-179">套用至控制器的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-179">The *after* code of filters applied to controllers</span></span>
* <span data-ttu-id="b8b6c-180">全域套用的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="b8b6c-180">The *after* code of filters applied globally</span></span>
  
<span data-ttu-id="b8b6c-181">下列範例說明同步動作篩選條件的篩選條件方法呼叫順序。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-181">Here's an example that illustrates the order in which filter methods are called for synchronous Action filters.</span></span>

| <span data-ttu-id="b8b6c-182">序列</span><span class="sxs-lookup"><span data-stu-id="b8b6c-182">Sequence</span></span> | <span data-ttu-id="b8b6c-183">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="b8b6c-183">Filter scope</span></span> | <span data-ttu-id="b8b6c-184">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-184">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="b8b6c-185">1</span><span class="sxs-lookup"><span data-stu-id="b8b6c-185">1</span></span> | <span data-ttu-id="b8b6c-186">Global</span><span class="sxs-lookup"><span data-stu-id="b8b6c-186">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-187">2</span><span class="sxs-lookup"><span data-stu-id="b8b6c-187">2</span></span> | <span data-ttu-id="b8b6c-188">控制器</span><span class="sxs-lookup"><span data-stu-id="b8b6c-188">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-189">3</span><span class="sxs-lookup"><span data-stu-id="b8b6c-189">3</span></span> | <span data-ttu-id="b8b6c-190">方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-190">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-191">4</span><span class="sxs-lookup"><span data-stu-id="b8b6c-191">4</span></span> | <span data-ttu-id="b8b6c-192">方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-192">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b8b6c-193">5</span><span class="sxs-lookup"><span data-stu-id="b8b6c-193">5</span></span> | <span data-ttu-id="b8b6c-194">控制器</span><span class="sxs-lookup"><span data-stu-id="b8b6c-194">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="b8b6c-195">6</span><span class="sxs-lookup"><span data-stu-id="b8b6c-195">6</span></span> | <span data-ttu-id="b8b6c-196">Global</span><span class="sxs-lookup"><span data-stu-id="b8b6c-196">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="b8b6c-197">此順序顯示方法篩選條件巢狀位於控制器篩選條件內，而控制器篩選條件巢狀位於全域篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-197">This sequence shows that the method filter is nested within the controller filter, and the controller filter is nested within the global filter.</span></span> <span data-ttu-id="b8b6c-198">換句話說，如果您在非同步篩選條件的 On*Stage*ExecutionAsync 方法內，具有較緊密範圍的所有篩選條件會在您的程式碼在堆疊上時執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-198">To put it another way, if you're inside an async filter's On*Stage*ExecutionAsync method, all of the filters with a tighter scope run while your code is on the stack.</span></span>

> [!NOTE]
> <span data-ttu-id="b8b6c-199">繼承自 `Controller` 基底類別的每個控制器，都包含 `OnActionExecuting` 和 `OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-199">Every controller that inherits from the `Controller` base class includes `OnActionExecuting` and `OnActionExecuted` methods.</span></span> <span data-ttu-id="b8b6c-200">這些方法會包裝針對指定動作執行的篩選條件：`OnActionExecuting` 在任何篩選條件之前呼叫，而 `OnActionExecuted` 則在所有篩選條件之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-200">These methods wrap the filters that run for a given action:  `OnActionExecuting` is called before any of the filters, and `OnActionExecuted` is called after all of the filters.</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="b8b6c-201">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="b8b6c-201">Overriding the default order</span></span>

<span data-ttu-id="b8b6c-202">您可以藉由實作 `IOrderedFilter` 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-202">You can override the default sequence of execution by implementing `IOrderedFilter`.</span></span> <span data-ttu-id="b8b6c-203">此介面會公開 `Order` 屬性，其優先順序高於範圍，可決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-203">This interface exposes an `Order` property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="b8b6c-204">`Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之前執行其「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-204">A filter with a lower `Order` value will have its *before* code executed before that of a filter with a higher value of `Order`.</span></span> <span data-ttu-id="b8b6c-205">`Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之後執行其「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-205">A filter with a lower `Order` value will have its *after* code executed after that of a filter with a higher `Order` value.</span></span> <span data-ttu-id="b8b6c-206">您可以使用建構函式參數來設定 `Order` 屬性：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-206">You can set the `Order` property by using a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="b8b6c-207">如果您擁有相同 3 個顯示在前面範例中的動作篩選條件，但將控制器的 `Order` 屬性和全域篩選條件分別設到 1 和 2，則執行順序會反轉。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-207">If you have the same 3 Action filters shown in the preceding example but set the `Order` property of the controller and global filters to 1 and 2 respectively, the order of execution would be reversed.</span></span>

| <span data-ttu-id="b8b6c-208">序列</span><span class="sxs-lookup"><span data-stu-id="b8b6c-208">Sequence</span></span> | <span data-ttu-id="b8b6c-209">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="b8b6c-209">Filter scope</span></span> | <span data-ttu-id="b8b6c-210">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="b8b6c-210">`Order` property</span></span> | <span data-ttu-id="b8b6c-211">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-211">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="b8b6c-212">1</span><span class="sxs-lookup"><span data-stu-id="b8b6c-212">1</span></span> | <span data-ttu-id="b8b6c-213">方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-213">Method</span></span> | <span data-ttu-id="b8b6c-214">0</span><span class="sxs-lookup"><span data-stu-id="b8b6c-214">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-215">2</span><span class="sxs-lookup"><span data-stu-id="b8b6c-215">2</span></span> | <span data-ttu-id="b8b6c-216">控制器</span><span class="sxs-lookup"><span data-stu-id="b8b6c-216">Controller</span></span> | <span data-ttu-id="b8b6c-217">1</span><span class="sxs-lookup"><span data-stu-id="b8b6c-217">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-218">3</span><span class="sxs-lookup"><span data-stu-id="b8b6c-218">3</span></span> | <span data-ttu-id="b8b6c-219">Global</span><span class="sxs-lookup"><span data-stu-id="b8b6c-219">Global</span></span> | <span data-ttu-id="b8b6c-220">2</span><span class="sxs-lookup"><span data-stu-id="b8b6c-220">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="b8b6c-221">4</span><span class="sxs-lookup"><span data-stu-id="b8b6c-221">4</span></span> | <span data-ttu-id="b8b6c-222">Global</span><span class="sxs-lookup"><span data-stu-id="b8b6c-222">Global</span></span> | <span data-ttu-id="b8b6c-223">2</span><span class="sxs-lookup"><span data-stu-id="b8b6c-223">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="b8b6c-224">5</span><span class="sxs-lookup"><span data-stu-id="b8b6c-224">5</span></span> | <span data-ttu-id="b8b6c-225">控制器</span><span class="sxs-lookup"><span data-stu-id="b8b6c-225">Controller</span></span> | <span data-ttu-id="b8b6c-226">1</span><span class="sxs-lookup"><span data-stu-id="b8b6c-226">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="b8b6c-227">6</span><span class="sxs-lookup"><span data-stu-id="b8b6c-227">6</span></span> | <span data-ttu-id="b8b6c-228">方法</span><span class="sxs-lookup"><span data-stu-id="b8b6c-228">Method</span></span> | <span data-ttu-id="b8b6c-229">0</span><span class="sxs-lookup"><span data-stu-id="b8b6c-229">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="b8b6c-230">`Order` 屬性在決定篩選條件執行的順序時以範圍取勝。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-230">The `Order` property trumps scope when determining the order in which filters will run.</span></span> <span data-ttu-id="b8b6c-231">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-231">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="b8b6c-232">所有內建的篩選條件會實作 `IOrderedFilter` 並將預設的 `Order` 值設為 0，因此除非您將 `Order` 設為非零值，否則範圍會決定順序。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-232">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0, so scope determines order unless you set `Order` to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="b8b6c-233">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="b8b6c-233">Cancellation and short circuiting</span></span>

<span data-ttu-id="b8b6c-234">您可以設定提供給篩選條件方法之 `context` 參數上的 `Result` 屬性，以在任何時間點縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-234">You can short-circuit the filter pipeline at any point by setting the `Result` property on the `context` parameter provided to the filter method.</span></span> <span data-ttu-id="b8b6c-235">比方說，下列資源篩選條件可防止管線的其餘部分執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-235">For instance, the following Resource filter prevents the rest of the pipeline from executing.</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

<span data-ttu-id="b8b6c-236">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-236">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="b8b6c-237">不過，因為 `ShortCircuitingResourceFilter` 會先執行 (因為它是資源篩選條件而 `AddHeader` 是動作篩選條件) 並縮短管線的其餘部分，所以 `SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-237">However, because the `ShortCircuitingResourceFilter` runs first (because it's a Resource Filter and `AddHeader` is an Action Filter) and short-circuits the rest of the pipeline, the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="b8b6c-238">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話 (例如因為其篩選條件類型，或明確使用了 `Order` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-238">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first (because of its filter type, or explicit use of `Order` property, for instance).</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="b8b6c-239">相依性插入</span><span class="sxs-lookup"><span data-stu-id="b8b6c-239">Dependency injection</span></span>

<span data-ttu-id="b8b6c-240">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-240">Filters can be added by type or by instance.</span></span> <span data-ttu-id="b8b6c-241">如果您新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-241">If you add an instance, that instance will be used for every request.</span></span> <span data-ttu-id="b8b6c-242">如果您新增類型，它將會由類型啟動，意思是會為每個要求建立執行個體，並且將會藉由[相依性插入](../../fundamentals/dependency-injection.md)(DI) 填入任何建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-242">If you add a type, it will be type-activated, meaning an instance will be created for each request and any constructor dependencies will be populated by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="b8b6c-243">依類型新增篩選條件相當於 `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-243">Adding a filter by type is equivalent to `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.</span></span>

<span data-ttu-id="b8b6c-244">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](../../fundamentals/dependency-injection.md) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-244">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="b8b6c-245">這是因為屬性都必須在套用它們的地方提供其建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-245">This is because attributes must have their constructor parameters supplied where they're applied.</span></span> <span data-ttu-id="b8b6c-246">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-246">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="b8b6c-247">如果您的篩選條件有必須從 DI 存取的相依性，會有數種支援的方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-247">If your filters have dependencies that you need to access from DI, there are several supported approaches.</span></span> <span data-ttu-id="b8b6c-248">您可以使用下列其中一項將篩選條件套用至類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-248">You can apply your filter to a class or action method using one of the following:</span></span>

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* <span data-ttu-id="b8b6c-249">在您的屬性上實作 `IFilterFactory`</span><span class="sxs-lookup"><span data-stu-id="b8b6c-249">`IFilterFactory` implemented on your attribute</span></span>

> [!NOTE]
> <span data-ttu-id="b8b6c-250">您可能想要從 DI 取得的一種相依性是記錄器。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-250">One dependency you might want to get from DI is a logger.</span></span> <span data-ttu-id="b8b6c-251">不過，不需要單純為了記錄便建立和使用篩選條件，因為[內建的架構記錄功能](xref:fundamentals/logging/index)可能已提供您需要的功能。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-251">However, avoid creating and using filters purely for logging purposes, since the [built-in framework logging features](xref:fundamentals/logging/index) may already provide what you need.</span></span> <span data-ttu-id="b8b6c-252">如果您要將記錄新增至您的篩選條件，它應該專注於商務網域考量或您篩選條件的特定行為，而不是 MVC 動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-252">If you're going to add logging to your filters, it should focus on business domain concerns or behavior specific to your filter, rather than MVC actions or other framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="b8b6c-253">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b8b6c-253">ServiceFilterAttribute</span></span>

<span data-ttu-id="b8b6c-254">`ServiceFilter` 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-254">A `ServiceFilter` retrieves an instance of the filter from DI.</span></span> <span data-ttu-id="b8b6c-255">您將篩選條件新增至 `ConfigureServices` 的容器中，並在 `ServiceFilter` 屬性參考它</span><span class="sxs-lookup"><span data-stu-id="b8b6c-255">You add the filter to the container in `ConfigureServices`, and reference it in a `ServiceFilter` attribute</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="b8b6c-256">使用 `ServiceFilter` 而不註冊篩選條件類型會導致例外狀況：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-256">Using `ServiceFilter` without registering the filter type results in an exception:</span></span>

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

<span data-ttu-id="b8b6c-257">`ServiceFilterAttribute` 會實作 `IFilterFactory`，而這會公開單一方法來建立 `IFilter` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-257">`ServiceFilterAttribute` implements `IFilterFactory`, which exposes a single method for creating an `IFilter` instance.</span></span> <span data-ttu-id="b8b6c-258">如果是 `ServiceFilterAttribute`，會實作 `IFilterFactory` 介面的 `CreateInstance` 方法以從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-258">In the case of `ServiceFilterAttribute`, the `IFilterFactory` interface's `CreateInstance` method is implemented to load the specified type from the services container (DI).</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="b8b6c-259">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="b8b6c-259">TypeFilterAttribute</span></span>

<span data-ttu-id="b8b6c-260">`TypeFilterAttribute` 非常類似於 `ServiceFilterAttribute` (也會實作 `IFilterFactory`)，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-260">`TypeFilterAttribute` is very similar to `ServiceFilterAttribute` (and also implements `IFilterFactory`), but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="b8b6c-261">相反地，它會使用 `Microsoft.Extensions.DependencyInjection.ObjectFactory` 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-261">Instead, it instantiates the type by using `Microsoft.Extensions.DependencyInjection.ObjectFactory`.</span></span>

<span data-ttu-id="b8b6c-262">由於此差別，使用 `TypeFilterAttribute` 參考的類型不必先向容器註冊 (但它們仍然會由容器滿足其相依性)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-262">Because of this difference, types that are referenced using the `TypeFilterAttribute` don't need to be registered with the container first (but they will still have their dependencies fulfilled by the container).</span></span> <span data-ttu-id="b8b6c-263">此外，`TypeFilterAttribute` 可以選擇性地接受討論中之類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-263">Also, `TypeFilterAttribute` can optionally accept constructor arguments for the type in question.</span></span> <span data-ttu-id="b8b6c-264">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-264">The following example demonstrates how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<span data-ttu-id="b8b6c-265">如果您的篩選條件不需要任何引數，但有需要 DI 填入的建構函式相依性，您可以在類別和方法上使用自己的具名屬性來取代 `[TypeFilter(typeof(FilterType))]`)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-265">If you have a filter that doesn't require any arguments, but which has constructor dependencies that need to be filled by DI, you can use your own named attribute on classes and methods instead of `[TypeFilter(typeof(FilterType))]`).</span></span> <span data-ttu-id="b8b6c-266">下列篩選條件顯示如何實作這點：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-266">The following filter shows how this can be implemented:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="b8b6c-267">此篩選條件可以使用 `[SampleActionFilter]` 語法套用至類別或方法，而不需使用 `[TypeFilter]` 或 `[ServiceFilter]`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-267">This filter can be applied to classes or methods using the `[SampleActionFilter]` syntax, instead of having to use `[TypeFilter]` or `[ServiceFilter]`.</span></span>

## <a name="authorization-filters"></a><span data-ttu-id="b8b6c-268">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-268">Authorization filters</span></span>

<span data-ttu-id="b8b6c-269">「授權篩選條件」控制動作方法的存取，且是在篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-269">*Authorization filters* control access to action methods and are the first filters to be executed within the filter pipeline.</span></span> <span data-ttu-id="b8b6c-270">它們只有之前的方法，不像支援之前和之後方法的大部分篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-270">They have only a before method, unlike most filters that support before and after methods.</span></span> <span data-ttu-id="b8b6c-271">您應該唯有在撰寫自己的授權架構時才撰寫自訂授權篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-271">You should only write a custom authorization filter if you are writing your own authorization framework.</span></span> <span data-ttu-id="b8b6c-272">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-272">Prefer configuring your authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="b8b6c-273">內建篩選條件實作只負責呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-273">The built-in filter implementation is just responsible for calling the authorization system.</span></span>

<span data-ttu-id="b8b6c-274">請注意，您不應該在授權篩選條件內擲回例外狀況，因為不會處理例外狀況 (例外狀況篩選條件將不會處理它們)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-274">Note that you shouldn't throw exceptions within authorization filters, since nothing will handle the exception (exception filters won't handle them).</span></span> <span data-ttu-id="b8b6c-275">相反地，請發出挑戰，或尋找另一種方式。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-275">Instead, issue a challenge or find another way.</span></span>

<span data-ttu-id="b8b6c-276">深入了解[授權](../../security/authorization/index.md)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-276">Learn more about [Authorization](../../security/authorization/index.md).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="b8b6c-277">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-277">Resource filters</span></span>

<span data-ttu-id="b8b6c-278">「資源篩選條件」會實作 `IResourceFilter` 或 `IAsyncResourceFilter` 介面，而它們的執行會圍繞大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-278">*Resource filters* implement either the `IResourceFilter` or `IAsyncResourceFilter` interface, and their execution wraps most of the filter pipeline.</span></span> <span data-ttu-id="b8b6c-279">(只有[授權篩選條件](#authorization-filters)會在它們之前執行。)資源篩選條件特別適用於您需要縮短要求正在進行的大部分工作時。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-279">(Only [Authorization filters](#authorization-filters) run before them.) Resource filters are especially useful if you need to short-circuit most of the work a request is doing.</span></span> <span data-ttu-id="b8b6c-280">比方說，如果回應已在快取中，快取篩選條件可以避免管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-280">For example, a caching filter can avoid the rest of the pipeline if the response is already in the cache.</span></span>

<span data-ttu-id="b8b6c-281">稍早所示的[縮短資源篩選條件](#short-circuiting-resource-filter)是資源篩選條件的一個範例。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-281">The [short circuiting resource filter](#short-circuiting-resource-filter) shown earlier is one example of a resource filter.</span></span> <span data-ttu-id="b8b6c-282">另一個例子是 [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)，它可避免模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-282">Another example is [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), which prevents model binding from accessing the form data.</span></span> <span data-ttu-id="b8b6c-283">它適用於您知道您會收到大型檔案上傳，並且想要避免表單被讀入到記憶體的情況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-283">It's useful for cases where you know that you're going to receive large file uploads and want to prevent the form from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="b8b6c-284">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-284">Action filters</span></span>

<span data-ttu-id="b8b6c-285">「動作篩選條件」會實作 `IActionFilter` 或 `IAsyncActionFilter` 介面，而它們的執行會圍繞動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-285">*Action filters* implement either the `IActionFilter` or `IAsyncActionFilter` interface, and their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="b8b6c-286">以下是動作篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-286">Here's a sample action filter:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="b8b6c-287">[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-287">The [ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) provides the following properties:</span></span>

* <span data-ttu-id="b8b6c-288">`ActionArguments` - 可讓您管理動作的輸入。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-288">`ActionArguments` - lets you manipulate the inputs to the action.</span></span>
* <span data-ttu-id="b8b6c-289">`Controller` - 可讓您管理控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-289">`Controller` - lets you manipulate the controller instance.</span></span> 
* <span data-ttu-id="b8b6c-290">`Result` - 設定動作方法和後續動作篩選條件的縮短執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-290">`Result` - setting this short-circuits execution of the action method and subsequent action filters.</span></span> <span data-ttu-id="b8b6c-291">擲回例外狀況也會導致無法執行動作方法和後續的篩選條件，但會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-291">Throwing an exception also prevents execution of the action method and subsequent filters, but is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b8b6c-292">[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) 提供 `Controller` 和 `Result`，再加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-292">The [ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="b8b6c-293">`Canceled` - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-293">`Canceled` - will be true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="b8b6c-294">`Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-294">`Exception` - will be non-null if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="b8b6c-295">將這個屬性設為 Null，實際上會「處理」例外狀況，並且會執行 `Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-295">Setting this property to null effectively 'handles' an exception, and `Result` will be executed as if it were returned from the action method normally.</span></span>

<span data-ttu-id="b8b6c-296">對於 `IAsyncActionFilter`，呼叫 `ActionExecutionDelegate` 會執行任何後續的動作篩選條件和動作方法，並傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-296">For an `IAsyncActionFilter`, a call to the `ActionExecutionDelegate` executes any subsequent action filters and the action method, returning an `ActionExecutedContext`.</span></span> <span data-ttu-id="b8b6c-297">若要縮短，請指派 `ActionExecutingContext.Result` 給某個結果執行個體，並且不要呼叫 `ActionExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-297">To short-circuit, assign `ActionExecutingContext.Result` to some result instance and don't call the `ActionExecutionDelegate`.</span></span>

<span data-ttu-id="b8b6c-298">架構提供一個抽象 `ActionFilterAttribute`，您可以進行子分類。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-298">The framework provides an abstract `ActionFilterAttribute` that you can subclass.</span></span> 

<span data-ttu-id="b8b6c-299">您可以使用動作篩選條件來自動驗證模型狀態，並在狀態無效時傳回任何錯誤：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-299">You can use an action filter to automatically validate model state and return any errors if the state is invalid:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

<span data-ttu-id="b8b6c-300">`OnActionExecuted` 方法在動作方法之後執行，且可以透過 `ActionExecutedContext.Result` 屬性看到及管理動作的結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-300">The `OnActionExecuted` method runs after the action method and can see and manipulate the results of the action through the `ActionExecutedContext.Result` property.</span></span> <span data-ttu-id="b8b6c-301">`ActionExecutedContext.Canceled` - 如果動作執行被另一個篩選條件縮短，則將設為 true。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-301">`ActionExecutedContext.Canceled` will be set to true if the action execution was short-circuited by another filter.</span></span> <span data-ttu-id="b8b6c-302">`ActionExecutedContext.Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則將設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-302">`ActionExecutedContext.Exception` will be set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="b8b6c-303">將 `ActionExecutedContext.Exception` 設為 Null，實際上會「處理」例外狀況，並且會執行 `ActionExectedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-303">Setting `ActionExecutedContext.Exception` to null effectively 'handles' an exception, and `ActionExectedContext.Result` will then be executed as if it were returned from the action method normally.</span></span>

## <a name="exception-filters"></a><span data-ttu-id="b8b6c-304">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-304">Exception filters</span></span>

<span data-ttu-id="b8b6c-305">「例外狀況篩選條件」會實作 `IExceptionFilter` 或 `IAsyncExceptionFilter` 介面。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-305">*Exception filters* implement either the `IExceptionFilter` or `IAsyncExceptionFilter` interface.</span></span> <span data-ttu-id="b8b6c-306">它們可以用來實作常見的應用程式錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-306">They can be used to implement common error handling policies for an app.</span></span> 

<span data-ttu-id="b8b6c-307">下列範例例外狀況篩選條件會使用自訂的開發人員檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-307">The following sample exception filter uses a custom developer error view to display details about exceptions that occur when the application is in development:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

<span data-ttu-id="b8b6c-308">例外狀況篩選條件沒有兩個事件 (針對之前和之後) - 它們只實作 `OnException` (或 `OnExceptionAsync`)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-308">Exception filters don't have two events (for before and after) - they only implement `OnException` (or `OnExceptionAsync`).</span></span> 

<span data-ttu-id="b8b6c-309">例外狀況篩選條件會處理在控制器建立、[模型繫結](../models/model-binding.md)、動作篩選條件或動作方法發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-309">Exception filters handle unhandled exceptions that occur in controller creation, [model binding](../models/model-binding.md), action filters, or action methods.</span></span> <span data-ttu-id="b8b6c-310">它們不會攔截在資源篩選條件、結果篩選條件或 MVC 結果執行發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-310">They won't catch exceptions that occur in Resource filters, Result filters, or MVC Result execution.</span></span>

<span data-ttu-id="b8b6c-311">若要處理例外狀況，請將 `ExceptionContext.ExceptionHandled` 屬性設為 true，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-311">To handle an exception, set the `ExceptionContext.ExceptionHandled` property to true or write a response.</span></span> <span data-ttu-id="b8b6c-312">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-312">This stops propagation of the exception.</span></span> <span data-ttu-id="b8b6c-313">請注意，例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-313">Note that an Exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="b8b6c-314">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-314">Only an Action filter can do that.</span></span>

> [!NOTE]
> <span data-ttu-id="b8b6c-315">在 ASP.NET 1.1 中，如果您將 `ExceptionHandled` 設為 true，**並且**撰寫回應，則不會傳送回應。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-315">In ASP.NET 1.1, the response isn't sent if you set `ExceptionHandled` to true **and** write a response.</span></span> <span data-ttu-id="b8b6c-316">在該情況中，ASP.NET Core 1.0 會傳送回應，而 ASP.NET Core 1.1.2 則會回到 1.0 的行為。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-316">In that scenario, ASP.NET Core 1.0 does send the response, and ASP.NET Core 1.1.2 will return to the 1.0 behavior.</span></span> <span data-ttu-id="b8b6c-317">如需詳細資訊，請參閱 GitHub 儲存機制中的[問題 #5594](https://github.com/aspnet/Mvc/issues/5594)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-317">For more information, see [issue #5594](https://github.com/aspnet/Mvc/issues/5594) in the GitHub repository.</span></span> 

<span data-ttu-id="b8b6c-318">例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-318">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="b8b6c-319">一般情況下通常使用中介軟體；只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-319">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span> <span data-ttu-id="b8b6c-320">比方說，您的應用程式可能會有 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-320">For example, your app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="b8b6c-321">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-321">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

<span data-ttu-id="b8b6c-322">架構提供一個抽象 `ExceptionFilterAttribute`，您可以進行子分類。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-322">The framework provides an abstract `ExceptionFilterAttribute` that you can subclass.</span></span> 

## <a name="result-filters"></a><span data-ttu-id="b8b6c-323">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="b8b6c-323">Result filters</span></span>

<span data-ttu-id="b8b6c-324">「結果篩選條件」會實作 `IResultFilter` 或 `IAsyncResultFilter` 介面，而它們的執行會圍繞動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-324">*Result filters* implement either the `IResultFilter` or `IAsyncResultFilter` interface, and their execution surrounds the execution of action results.</span></span> 

<span data-ttu-id="b8b6c-325">以下是結果篩選條件的範例，它會新增 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-325">Here's an example of a Result filter that adds an HTTP header.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="b8b6c-326">正在執行的結果類型取決於討論中的動作。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-326">The kind of result being executed depends on the action in question.</span></span> <span data-ttu-id="b8b6c-327">傳回檢視的 MVC 動作會在執行中的 `ViewResult` 裡包含處理中的所有 Razor。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-327">An MVC action returning a view would include all razor processing as part of the `ViewResult` being executed.</span></span> <span data-ttu-id="b8b6c-328">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-328">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="b8b6c-329">深入了解[動作結果](actions.md)</span><span class="sxs-lookup"><span data-stu-id="b8b6c-329">Learn more about [action results](actions.md)</span></span>

<span data-ttu-id="b8b6c-330">動作篩選條件只會針對成功的結果執行 - 動作或動作篩選條件會執行動作結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-330">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="b8b6c-331">當例外狀況篩選條件處理例外狀況時，不會執行結果篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-331">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="b8b6c-332">藉由將 `ResultExecutingContext.Cancel` 設為 true，`OnResultExecuting` 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-332">The `OnResultExecuting` method can short-circuit execution of the action result and subsequent result filters by setting `ResultExecutingContext.Cancel` to true.</span></span> <span data-ttu-id="b8b6c-333">在縮短時您通常應該寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-333">You should generally write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="b8b6c-334">擲回例外狀況也會導致無法執行動作結果和後續的篩選條件，但會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-334">Throwing an exception will also prevent execution of the action result and subsequent filters, but will be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="b8b6c-335">當 `OnResultExecuted` 方法執行時，回應可能傳送至用戶端，且無法進一步變更 (除非擲回例外狀況)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-335">When the `OnResultExecuted` method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).</span></span> <span data-ttu-id="b8b6c-336">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 將設為 true。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-336">`ResultExecutedContext.Canceled` will be set to true if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="b8b6c-337">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 將設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-337">`ResultExecutedContext.Exception` will be set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="b8b6c-338">將 `Exception` 設為 Null，實際上會「處理」例外狀況，並且使管線中稍後的 MVC 不會重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-338">Setting `Exception` to null effectively 'handles' an exception and prevents the exception from being rethrown by MVC later in the pipeline.</span></span> <span data-ttu-id="b8b6c-339">當您處理結果篩選條件的例外狀況時，您可能無法將任何資料寫入至回應。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-339">When you're handling an exception in a result filter, you might not be able to write any data to the response.</span></span> <span data-ttu-id="b8b6c-340">如果動作結果在執行中途擲回，且標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-340">If the action result throws partway through its execution, and the headers have already been flushed to the client, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="b8b6c-341">針對 `IAsyncResultFilter`，對 `ResultExecutionDelegate` 呼叫 `await next()` 會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-341">For an `IAsyncResultFilter` a call to `await next()` on the `ResultExecutionDelegate` executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="b8b6c-342">若要縮短，請將 `ResultExecutingContext.Cancel` 設為 true，且不要呼叫 `ResultExectionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-342">To short-circuit, set `ResultExecutingContext.Cancel` to true and don't call the `ResultExectionDelegate`.</span></span>

<span data-ttu-id="b8b6c-343">架構提供一個抽象 `ResultFilterAttribute`，您可以進行子分類。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-343">The framework provides an abstract `ResultFilterAttribute` that you can subclass.</span></span> <span data-ttu-id="b8b6c-344">稍早所示的 [AddHeaderAttribute](#add-header-attribute)類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-344">The [AddHeaderAttribute](#add-header-attribute) class shown earlier is an example of a result filter attribute.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="b8b6c-345">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="b8b6c-345">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="b8b6c-346">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-346">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="b8b6c-347">但篩選條件與中介軟體不同之處在於，它們是 MVC 的一部分，這表示它們能存取 MVC 內容和建構。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-347">But filters differ from middleware in that they're part of MVC, which means that they have access to MVC context and constructs.</span></span>

<span data-ttu-id="b8b6c-348">在 ASP.NET Core 1.1 中，您可以在篩選條件管線中使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-348">In ASP.NET Core 1.1, you can use middleware in the filter pipeline.</span></span> <span data-ttu-id="b8b6c-349">如果您有需要存取 MVC 路由資料的中介軟體元件，或只應該針對特定控制器或動作執行的中介軟體元件，則可能會想要這樣做。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-349">You might want to do that if you have a middleware component that needs access to MVC route data, or one that should run only for certain controllers or actions.</span></span>

<span data-ttu-id="b8b6c-350">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，指定您要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-350">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware that you want to inject into the filter pipeline.</span></span> <span data-ttu-id="b8b6c-351">以下是使用當地語系化中介軟體，建立要求之目前文化特性的範例：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-351">Here's an example that uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="b8b6c-352">然後您可以使用 `MiddlewareFilterAttribute` 針對選取的控制器或動作執行中介軟體，或是全域地執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b8b6c-352">You can then use the `MiddlewareFilterAttribute` to run the middleware for a selected controller or action or globally:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="b8b6c-353">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-353">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="b8b6c-354">後續動作</span><span class="sxs-lookup"><span data-stu-id="b8b6c-354">Next actions</span></span>

<span data-ttu-id="b8b6c-355">若要嘗試使用篩選條件，請[下載、測試及修改範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="b8b6c-355">To experiment with filters, [download, test and modify the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
