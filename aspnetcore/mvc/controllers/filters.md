---
title: "篩選條件"
author: ardalis
description: "深入了解如何 * 篩選 * 工作，以及如何在 ASP.NET Core MVC 中使用它們。"
keywords: "ASP.NET Core 篩選、 mvc 篩選條件、 動作篩選條件，授權篩選條件、 資源篩選器、 結果篩選條件、 例外狀況篩選條件"
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: 34a5be6e77f8558c9b3c257575272e167ed95ea4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="filters"></a><span data-ttu-id="ce74c-104">篩選條件</span><span class="sxs-lookup"><span data-stu-id="ce74c-104">Filters</span></span>

<span data-ttu-id="ce74c-105">由[Tom Dykstra](https://github.com/tdykstra/)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ce74c-105">By [Tom Dykstra](https://github.com/tdykstra/) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ce74c-106">*篩選*中 ASP.NET Core MVC 可讓您執行程式碼之前或之後的要求處理管線中的特定階段。</span><span class="sxs-lookup"><span data-stu-id="ce74c-106">*Filters* in ASP.NET Core MVC allow you to run code before or after certain stages in the request processing pipeline.</span></span>

 <span data-ttu-id="ce74c-107">內建篩選器處理工作，例如授權 （防止未獲授權使用者的資源的存取），確保所有要求都使用 HTTPS 和快取 （最少運算的快取的回應傳回的要求管線） 的回應。</span><span class="sxs-lookup"><span data-stu-id="ce74c-107">Built-in filters handle tasks such as authorization (preventing access to resources a user isn't authorized for), ensuring that all requests use HTTPS, and response caching (short-circuiting the request pipeline to return a cached response).</span></span> 

<span data-ttu-id="ce74c-108">您可以建立自訂篩選來處理跨領域考量您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce74c-108">You can create custom filters to handle cross-cutting concerns for your application.</span></span> <span data-ttu-id="ce74c-109">每當您想要避免在動作之間複製程式碼，篩選是解決方案。</span><span class="sxs-lookup"><span data-stu-id="ce74c-109">Anytime you want to avoid duplicating code across actions, filters are the solution.</span></span> <span data-ttu-id="ce74c-110">例如，您可以合併錯誤處理程式碼例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-110">For example, you can consolidate error handling code in a exception filter.</span></span>

<span data-ttu-id="ce74c-111">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-111">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

## <a name="how-do-filters-work"></a><span data-ttu-id="ce74c-112">篩選如何運作？</span><span class="sxs-lookup"><span data-stu-id="ce74c-112">How do filters work?</span></span>

<span data-ttu-id="ce74c-113">篩選執行*MVC 動作引動過程管線*，有時又稱為*篩選管線*。</span><span class="sxs-lookup"><span data-stu-id="ce74c-113">Filters run within the *MVC action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="ce74c-114">篩選管線執行之後 MVC 選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-114">The filter pipeline runs after MVC selects the action to execute.</span></span>

![要求會透過其他中介軟體、 路由的中介軟體、 動作選取和 MVC 動作引動過程管線處理。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="ce74c-117">篩選類型</span><span class="sxs-lookup"><span data-stu-id="ce74c-117">Filter types</span></span>

<span data-ttu-id="ce74c-118">每個篩選類型是在篩選管線中的不同階段中執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-118">Each filter type is executed at a different stage in the filter pipeline.</span></span>

* <span data-ttu-id="ce74c-119">[授權篩選條件](#authorization-filters)會先執行，用來判斷目前使用者是否目前要求的授權。</span><span class="sxs-lookup"><span data-stu-id="ce74c-119">[Authorization filters](#authorization-filters) run first and are used to determine whether the current user is authorized for the current request.</span></span> <span data-ttu-id="ce74c-120">如果是未經授權的要求，它們就可以縮短管線。</span><span class="sxs-lookup"><span data-stu-id="ce74c-120">They can short-circuit the pipeline if a request is unauthorized.</span></span> 

* <span data-ttu-id="ce74c-121">[資源的篩選器](#resource-filters)是優先要授權之後處理要求。</span><span class="sxs-lookup"><span data-stu-id="ce74c-121">[Resource filters](#resource-filters) are the first to handle a request after authorization.</span></span>  <span data-ttu-id="ce74c-122">他們可以執行前篩選管線，並完成其餘的管線之後的其餘部分的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce74c-122">They can run code before the rest of the filter pipeline, and after the rest of the pipeline has completed.</span></span> <span data-ttu-id="ce74c-123">它們很有用來實作快取或否則最短路徑篩選管線，基於效能的考量。</span><span class="sxs-lookup"><span data-stu-id="ce74c-123">They're useful to implement caching or otherwise short-circuit the filter pipeline for performance reasons.</span></span> <span data-ttu-id="ce74c-124">因為它們執行模型繫結之前，它們會很有用來影響模型繫結所需的任何項目。</span><span class="sxs-lookup"><span data-stu-id="ce74c-124">Since they run before model binding, they're useful for anything that needs to influence model binding.</span></span>

* <span data-ttu-id="ce74c-125">[動作篩選條件](#action-filters)立即之前和之後執行個別的動作方法會呼叫可執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce74c-125">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="ce74c-126">它們可以用來處理引數傳遞至動作和動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="ce74c-126">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span>

* <span data-ttu-id="ce74c-127">[例外狀況篩選條件](#exception-filters)用來將通用的原則套用到任何項目已寫入回應主體之前，發生未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-127">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="ce74c-128">[產生篩選](#result-filters)立即之前和之後執行個別的動作結果的可執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce74c-128">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="ce74c-129">這些動作方法執行成功時，才執行，而且可用於必須住檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ce74c-129">They run only when the action method has executed successfully and are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="ce74c-130">下圖顯示這些篩選器型別篩選管線中的互動方式。</span><span class="sxs-lookup"><span data-stu-id="ce74c-130">The following diagram shows how these filter types interact in the filter pipeline.</span></span>

![要求會透過授權篩選條件、 資源篩選器、 模型繫結、 動作篩選條件、 動作執行和動作結果轉換、 例外狀況篩選條件、 結果篩選條件，以及結果執行處理。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="ce74c-133">實作</span><span class="sxs-lookup"><span data-stu-id="ce74c-133">Implementation</span></span>

<span data-ttu-id="ce74c-134">篩選器支援透過不同的介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-134">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span> <span data-ttu-id="ce74c-135">選擇您要執行的工作類型而定的同步或非同步 variant。</span><span class="sxs-lookup"><span data-stu-id="ce74c-135">Choose either the sync or async variant depending on the kind of task you need to perform.</span></span> 

<span data-ttu-id="ce74c-136">同步的篩選器可執行程式碼兩者上定義其管線階段前後*階段*執行以及*階段*執行方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-136">Synchronous filters that can run code both before and after their pipeline stage define On*Stage*Executing and On*Stage*Executed methods.</span></span> <span data-ttu-id="ce74c-137">例如，`OnActionExecuting`之前呼叫動作方法時，呼叫和`OnActionExecuted`動作方法傳回之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="ce74c-137">For example, `OnActionExecuting` is called before the action method is called, and `OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

<span data-ttu-id="ce74c-138">非同步的篩選會定義單一上*階段*ExecutionAsync 方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-138">Asynchronous filters define a single On*Stage*ExecutionAsync method.</span></span> <span data-ttu-id="ce74c-139">這個方法會採用*FilterType*ExecutionDelegate 委派，它會執行篩選條件的管線階段。</span><span class="sxs-lookup"><span data-stu-id="ce74c-139">This method takes a *FilterType*ExecutionDelegate delegate which executes the filter's pipeline stage.</span></span> <span data-ttu-id="ce74c-140">例如，`ActionExecutionDelegate`呼叫動作方法，而您可以執行程式碼之前和之後呼叫它。</span><span class="sxs-lookup"><span data-stu-id="ce74c-140">For example, `ActionExecutionDelegate` calls the action method, and you can execute code before and after you call it.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

<span data-ttu-id="ce74c-141">您可以實作介面的單一類別中的多個篩選器階段。</span><span class="sxs-lookup"><span data-stu-id="ce74c-141">You can implement interfaces for multiple filter stages in a single class.</span></span> <span data-ttu-id="ce74c-142">例如， [Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)抽象類別會同時實作`IActionFilter`和`IResultFilter`，以及其非同步對等用法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-142">For example, the [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) abstract class implements both `IActionFilter` and `IResultFilter`, as well as their async equivalents.</span></span>

> [!NOTE]
> <span data-ttu-id="ce74c-143">實作**任一**同步或非同步版本的篩選器的介面，不可同時為兩者。</span><span class="sxs-lookup"><span data-stu-id="ce74c-143">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="ce74c-144">架構會先檢查以查看如果篩選條件會實作非同步介面，而且如果是的話，它會呼叫的。</span><span class="sxs-lookup"><span data-stu-id="ce74c-144">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="ce74c-145">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-145">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="ce74c-146">如果您是在一個類別上實作這兩個介面，則會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-146">If you were to implement both interfaces on one class, only the async method would be called.</span></span> <span data-ttu-id="ce74c-147">當使用抽象類別，例如[Actionfilterattribut](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)同步的方法或其非同步方法的每個篩選類型，就會覆寫。</span><span class="sxs-lookup"><span data-stu-id="ce74c-147">When using abstract classes like [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) you would override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="ifilterfactory"></a><span data-ttu-id="ce74c-148">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="ce74c-148">IFilterFactory</span></span>

<span data-ttu-id="ce74c-149">`IFilterFactory` 會實作 `IFilter`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-149">`IFilterFactory` implements `IFilter`.</span></span> <span data-ttu-id="ce74c-150">因此，`IFilterFactory`執行個體可用來當做`IFilter`篩選管線中的任何位置執行個體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-150">Therefore, an `IFilterFactory` instance can be used as an `IFilter` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="ce74c-151">當架構準備要叫用的篩選時，會嘗試進行轉換， `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-151">When the framework prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="ce74c-152">如果該轉換成功，`CreateInstance`呼叫方法來建立`IFilter`會叫用的執行個體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-152">If that cast succeeds, the `CreateInstance` method is called to create the `IFilter` instance that will be invoked.</span></span> <span data-ttu-id="ce74c-153">這會提供相當有彈性的設計，因為不需要應用程式啟動時明確設定精確的篩選器管線。</span><span class="sxs-lookup"><span data-stu-id="ce74c-153">This provides a very flexible design, since the precise filter pipeline does not need to be set explicitly when the application starts.</span></span>

<span data-ttu-id="ce74c-154">您可以實作`IFilterFactory`對您自己做為另一種方法來建立篩選的屬性實作：</span><span class="sxs-lookup"><span data-stu-id="ce74c-154">You can implement `IFilterFactory` on your own attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a><span data-ttu-id="ce74c-155">內建的篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="ce74c-155">Built-in filter attributes</span></span>

<span data-ttu-id="ce74c-156">此架構包含內建屬性型篩選，您可以子類別和自訂。</span><span class="sxs-lookup"><span data-stu-id="ce74c-156">The framework includes built-in attribute-based filters that you can subclass and customize.</span></span> <span data-ttu-id="ce74c-157">例如，下列結果篩選條件會將標頭加入回應。</span><span class="sxs-lookup"><span data-stu-id="ce74c-157">For example, the following Result filter adds a header to the response.</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

<span data-ttu-id="ce74c-158">屬性可讓篩選器，以接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="ce74c-158">Attributes allow filters to accept arguments, as shown in the example above.</span></span> <span data-ttu-id="ce74c-159">您會將此屬性加入至控制器或動作方法，並指定名稱和 HTTP 標頭的值：</span><span class="sxs-lookup"><span data-stu-id="ce74c-159">You would add this attribute to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="ce74c-160">結果`Index`如下所示的動作-回應標頭會顯示在右下方。</span><span class="sxs-lookup"><span data-stu-id="ce74c-160">The result of the `Index` action is shown below - the response headers are displayed on the bottom right.</span></span>

![開發人員工具的 Microsoft Edge 顯示回應標頭，包括作者 Steve Smith@ardalis](filters/_static/add-header.png)

<span data-ttu-id="ce74c-162">有幾個篩選條件介面有對應的屬性，可用來當作基底類別的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-162">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="ce74c-163">篩選屬性：</span><span class="sxs-lookup"><span data-stu-id="ce74c-163">Filter attributes:</span></span>

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

<span data-ttu-id="ce74c-164">`TypeFilterAttribute`和`ServiceFilterAttribute`說明，請參閱[本文稍後](#dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-164">`TypeFilterAttribute` and `ServiceFilterAttribute` are explained [later in this article](#dependency-injection).</span></span>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="ce74c-165">篩選範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="ce74c-165">Filter scopes and order of execution</span></span>

<span data-ttu-id="ce74c-166">可以加入篩選器，在三個的其中一個管線*範圍*。</span><span class="sxs-lookup"><span data-stu-id="ce74c-166">A filter can be added to the pipeline at one of three *scopes*.</span></span> <span data-ttu-id="ce74c-167">您可以使用屬性特定的動作方法或控制器類別加入篩選。</span><span class="sxs-lookup"><span data-stu-id="ce74c-167">You can add a filter to a particular action method or to a controller class by using an attribute.</span></span> <span data-ttu-id="ce74c-168">您可以將它加入至註冊全域 （適用於所有控制器和動作） 篩選器或`MvcOptions.Filters`集合`ConfigureServices`方法中的`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="ce74c-168">Or you can register a filter globally (for all controllers and actions) by adding it to the `MvcOptions.Filters` collection in the `ConfigureServices` method in the `Startup` class:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a><span data-ttu-id="ce74c-169">預設的執行順序</span><span class="sxs-lookup"><span data-stu-id="ce74c-169">Default order of execution</span></span>

<span data-ttu-id="ce74c-170">當管線的特定階段如有多個篩選條件時，範圍會決定執行篩選條件的預設順序。</span><span class="sxs-lookup"><span data-stu-id="ce74c-170">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="ce74c-171">全域篩選括住類別篩選，又圍繞方法篩選器。</span><span class="sxs-lookup"><span data-stu-id="ce74c-171">Global filters surround class filters, which in turn surround method filters.</span></span> <span data-ttu-id="ce74c-172">這有時候稱為"俄文布偶 「 巢狀結構，因為每次加大範圍圍繞前一個範圍，例如[巢狀布偶](https://wikipedia.org/wiki/Matryoshka_doll)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-172">This is sometimes referred to as "Russian doll" nesting, as each increase in scope is wrapped around the previous scope, like a [nesting doll](https://wikipedia.org/wiki/Matryoshka_doll).</span></span> <span data-ttu-id="ce74c-173">您通常不必明確地判斷順序取得想要覆寫的行為。</span><span class="sxs-lookup"><span data-stu-id="ce74c-173">You generally get the desired overriding behavior without having to explicitly determine ordering.</span></span>

<span data-ttu-id="ce74c-174">因為這個巢狀結構、*之後*的篩選條件的程式碼會執行相反的順序*之前*程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce74c-174">As a result of this nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="ce74c-175">順序看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="ce74c-175">The sequence looks like this:</span></span>

* <span data-ttu-id="ce74c-176">*之前*全域套用的篩選器的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-176">The *before* code of filters applied globally</span></span>
  * <span data-ttu-id="ce74c-177">*之前*套用至控制器的篩選條件的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-177">The *before* code of filters applied to controllers</span></span>
    * <span data-ttu-id="ce74c-178">*之前*篩選器套用至動作方法的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-178">The *before* code of filters applied to action methods</span></span>
    * <span data-ttu-id="ce74c-179">*之後*篩選器套用至動作方法的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-179">The *after* code of filters applied to action methods</span></span>
  * <span data-ttu-id="ce74c-180">*之後*套用至控制器的篩選條件的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-180">The *after* code of filters applied to controllers</span></span>
* <span data-ttu-id="ce74c-181">*之後*全域套用的篩選器的程式碼</span><span class="sxs-lookup"><span data-stu-id="ce74c-181">The *after* code of filters applied globally</span></span>
  
<span data-ttu-id="ce74c-182">以下是範例，說明中的篩選方法呼叫同步動作篩選條件的順序。</span><span class="sxs-lookup"><span data-stu-id="ce74c-182">Here's an example that illustrates the order in which filter methods are called for synchronous Action filters.</span></span>

| <span data-ttu-id="ce74c-183">序列</span><span class="sxs-lookup"><span data-stu-id="ce74c-183">Sequence</span></span> | <span data-ttu-id="ce74c-184">篩選範圍</span><span class="sxs-lookup"><span data-stu-id="ce74c-184">Filter scope</span></span> | <span data-ttu-id="ce74c-185">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-185">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="ce74c-186">1</span><span class="sxs-lookup"><span data-stu-id="ce74c-186">1</span></span> | <span data-ttu-id="ce74c-187">Global</span><span class="sxs-lookup"><span data-stu-id="ce74c-187">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="ce74c-188">2</span><span class="sxs-lookup"><span data-stu-id="ce74c-188">2</span></span> | <span data-ttu-id="ce74c-189">控制器</span><span class="sxs-lookup"><span data-stu-id="ce74c-189">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="ce74c-190">3</span><span class="sxs-lookup"><span data-stu-id="ce74c-190">3</span></span> | <span data-ttu-id="ce74c-191">方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-191">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="ce74c-192">4</span><span class="sxs-lookup"><span data-stu-id="ce74c-192">4</span></span> | <span data-ttu-id="ce74c-193">方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-193">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="ce74c-194">5</span><span class="sxs-lookup"><span data-stu-id="ce74c-194">5</span></span> | <span data-ttu-id="ce74c-195">控制器</span><span class="sxs-lookup"><span data-stu-id="ce74c-195">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="ce74c-196">6</span><span class="sxs-lookup"><span data-stu-id="ce74c-196">6</span></span> | <span data-ttu-id="ce74c-197">Global</span><span class="sxs-lookup"><span data-stu-id="ce74c-197">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="ce74c-198">此順序顯示方法篩選巢狀在控制器的篩選器，並控制站篩選巢狀在全域篩選。</span><span class="sxs-lookup"><span data-stu-id="ce74c-198">This sequence shows that the method filter is nested within the controller filter, and the controller filter is nested within the global filter.</span></span> <span data-ttu-id="ce74c-199">若要將其放另一個方式，在非同步處理篩選條件的*階段*ExecutionAsync 方法的所有具有更緊密的範圍篩選器時執行您的程式碼在堆疊上。</span><span class="sxs-lookup"><span data-stu-id="ce74c-199">To put it another way, if you're inside an async filter's On*Stage*ExecutionAsync method, all of the filters with a tighter scope run while your code is on the stack.</span></span>

> [!NOTE]
> <span data-ttu-id="ce74c-200">繼承自每個控制站`Controller`基底類別包含`OnActionExecuting`和`OnActionExecuted`方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-200">Every controller that inherits from the `Controller` base class includes `OnActionExecuting` and `OnActionExecuted` methods.</span></span> <span data-ttu-id="ce74c-201">這些方法會包裝執行指定動作的篩選：`OnActionExecuting`之前，任何篩選器，呼叫和`OnActionExecuted`在所有的篩選後呼叫。</span><span class="sxs-lookup"><span data-stu-id="ce74c-201">These methods wrap the filters that run for a given action:  `OnActionExecuting` is called before any of the filters, and `OnActionExecuted` is called after all of the filters.</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="ce74c-202">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="ce74c-202">Overriding the default order</span></span>

<span data-ttu-id="ce74c-203">您可以藉由實作覆寫預設的序列執行`IOrderedFilter`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-203">You can override the default sequence of execution by implementing `IOrderedFilter`.</span></span> <span data-ttu-id="ce74c-204">此介面會公開`Order`優先順序高於範圍來決定執行順序的屬性。</span><span class="sxs-lookup"><span data-stu-id="ce74c-204">This interface exposes an `Order` property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="ce74c-205">較低的篩選器`Order`值將會有其*之前*，以較高值的篩選條件之前執行的程式碼`Order`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-205">A filter with a lower `Order` value will have its *before* code executed before that of a filter with a higher value of `Order`.</span></span> <span data-ttu-id="ce74c-206">較低的篩選器`Order`值將會有其*之後*，較高的篩選器之後執行的程式碼`Order`值。</span><span class="sxs-lookup"><span data-stu-id="ce74c-206">A filter with a lower `Order` value will have its *after* code executed after that of a filter with a higher `Order` value.</span></span> <span data-ttu-id="ce74c-207">您可以設定`Order`屬性使用建構函式參數：</span><span class="sxs-lookup"><span data-stu-id="ce74c-207">You can set the `Order` property by using a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="ce74c-208">如果您擁有相同 3 顯示在前面的範例，但集合中的動作篩選條件`Order`控制站和通用屬性分別篩選到 1 和 2 時，會反轉的執行順序。</span><span class="sxs-lookup"><span data-stu-id="ce74c-208">If you have the same 3 Action filters shown in the preceding example but set the `Order` property of the controller and global filters to 1 and 2 respectively, the order of execution would be reversed.</span></span>

| <span data-ttu-id="ce74c-209">序列</span><span class="sxs-lookup"><span data-stu-id="ce74c-209">Sequence</span></span> | <span data-ttu-id="ce74c-210">篩選範圍</span><span class="sxs-lookup"><span data-stu-id="ce74c-210">Filter scope</span></span> | <span data-ttu-id="ce74c-211">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="ce74c-211">`Order` property</span></span> | <span data-ttu-id="ce74c-212">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-212">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="ce74c-213">1</span><span class="sxs-lookup"><span data-stu-id="ce74c-213">1</span></span> | <span data-ttu-id="ce74c-214">方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-214">Method</span></span> | <span data-ttu-id="ce74c-215">0</span><span class="sxs-lookup"><span data-stu-id="ce74c-215">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="ce74c-216">2</span><span class="sxs-lookup"><span data-stu-id="ce74c-216">2</span></span> | <span data-ttu-id="ce74c-217">控制器</span><span class="sxs-lookup"><span data-stu-id="ce74c-217">Controller</span></span> | <span data-ttu-id="ce74c-218">1</span><span class="sxs-lookup"><span data-stu-id="ce74c-218">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="ce74c-219">3</span><span class="sxs-lookup"><span data-stu-id="ce74c-219">3</span></span> | <span data-ttu-id="ce74c-220">Global</span><span class="sxs-lookup"><span data-stu-id="ce74c-220">Global</span></span> | <span data-ttu-id="ce74c-221">2</span><span class="sxs-lookup"><span data-stu-id="ce74c-221">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="ce74c-222">4</span><span class="sxs-lookup"><span data-stu-id="ce74c-222">4</span></span> | <span data-ttu-id="ce74c-223">Global</span><span class="sxs-lookup"><span data-stu-id="ce74c-223">Global</span></span> | <span data-ttu-id="ce74c-224">2</span><span class="sxs-lookup"><span data-stu-id="ce74c-224">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="ce74c-225">5</span><span class="sxs-lookup"><span data-stu-id="ce74c-225">5</span></span> | <span data-ttu-id="ce74c-226">控制器</span><span class="sxs-lookup"><span data-stu-id="ce74c-226">Controller</span></span> | <span data-ttu-id="ce74c-227">1</span><span class="sxs-lookup"><span data-stu-id="ce74c-227">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="ce74c-228">6</span><span class="sxs-lookup"><span data-stu-id="ce74c-228">6</span></span> | <span data-ttu-id="ce74c-229">方法</span><span class="sxs-lookup"><span data-stu-id="ce74c-229">Method</span></span> | <span data-ttu-id="ce74c-230">0</span><span class="sxs-lookup"><span data-stu-id="ce74c-230">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="ce74c-231">`Order`屬性更勝於範圍時決定篩選條件會執行的順序。</span><span class="sxs-lookup"><span data-stu-id="ce74c-231">The `Order` property trumps scope when determining the order in which filters will run.</span></span> <span data-ttu-id="ce74c-232">篩選條件會依照先順序，則範圍用於切割繫結。</span><span class="sxs-lookup"><span data-stu-id="ce74c-232">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="ce74c-233">所有內建的篩選器實作`IOrderedFilter`和設定預設`Order`值設為 0，因此除非您將設定領域可決定順序`Order`為非零值。</span><span class="sxs-lookup"><span data-stu-id="ce74c-233">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0, so scope determines order unless you set `Order` to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="ce74c-234">取消作業和短最運算</span><span class="sxs-lookup"><span data-stu-id="ce74c-234">Cancellation and short circuiting</span></span>

<span data-ttu-id="ce74c-235">您可以縮短在任何時間點的篩選管線設定`Result`屬性`context`提供給篩選方法的參數。</span><span class="sxs-lookup"><span data-stu-id="ce74c-235">You can short-circuit the filter pipeline at any point by setting the `Result` property on the `context` parameter provided to the filter method.</span></span> <span data-ttu-id="ce74c-236">比方說，下列資源的篩選條件可防止其他管線執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-236">For instance, the following Resource filter prevents the rest of the pipeline from executing.</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

<span data-ttu-id="ce74c-237">下列程式碼，同時`ShortCircuitingResourceFilter`和`AddHeader`篩選目標`SomeResource`動作方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-237">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="ce74c-238">不過，因為`ShortCircuitingResourceFilter`會先執行 (因為它是資源的篩選器和`AddHeader`是動作篩選條件) 和 short-circuits 管線的其餘部分`AddHeader`永遠不會執行篩選`SomeResource`動作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-238">However, because the `ShortCircuitingResourceFilter` runs first (because it is a Resource Filter and `AddHeader` is an Action Filter) and short-circuits the rest of the pipeline, the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="ce74c-239">此行為也會相同，如果這兩個篩選套用在動作方法層級提供`ShortCircuitingResourceFilter`執行第一個 (因為其篩選器類型，或明確使用`Order`屬性，例如)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-239">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first (because of its filter type, or explicit use of `Order` property, for instance).</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="ce74c-240">相依性插入</span><span class="sxs-lookup"><span data-stu-id="ce74c-240">Dependency injection</span></span>

<span data-ttu-id="ce74c-241">依類型或執行個體，可以加入篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-241">Filters can be added by type or by instance.</span></span> <span data-ttu-id="ce74c-242">如果您新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="ce74c-242">If you add an instance, that instance will be used for every request.</span></span> <span data-ttu-id="ce74c-243">如果您加入類型，它將型別啟動，即會建立執行個體，每個要求，並將會填入任何建構函式相依性[相依性插入](../../fundamentals/dependency-injection.md)(DI)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-243">If you add a type, it will be type-activated, meaning an instance will be created for each request and any constructor dependencies will be populated by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="ce74c-244">加入篩選條件類型相當於`filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-244">Adding a filter by type is equivalent to `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.</span></span>

<span data-ttu-id="ce74c-245">篩選器，都實作為屬性直接加入至控制器類別或動作方法不能有建構函式所提供的相依性[相依性插入](../../fundamentals/dependency-injection.md)(DI)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-245">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="ce74c-246">這是因為屬性都必須將它們以套用所提供的建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="ce74c-246">This is because attributes must have their constructor parameters supplied where they are applied.</span></span> <span data-ttu-id="ce74c-247">這是屬性的運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="ce74c-247">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="ce74c-248">如果您的篩選沒有相依性，您必須從 DI 存取，有數種支援的方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-248">If your filters have dependencies that you need to access from DI, there are several supported approaches.</span></span> <span data-ttu-id="ce74c-249">您可以將篩選器套用至類別或動作方法，使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="ce74c-249">You can apply your filter to a class or action method using one of the following:</span></span>

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* <span data-ttu-id="ce74c-250">`IFilterFactory`在您的屬性上實作</span><span class="sxs-lookup"><span data-stu-id="ce74c-250">`IFilterFactory` implemented on your attribute</span></span>

> [!NOTE]
> <span data-ttu-id="ce74c-251">一種相依性，您可能想要取得從 DI 是記錄器。</span><span class="sxs-lookup"><span data-stu-id="ce74c-251">One dependency you might want to get from DI is a logger.</span></span> <span data-ttu-id="ce74c-252">不過，不需要建立及單純地進行記錄，使用篩選器，因為[內建架構記錄功能](../../fundamentals/logging.md)可能已提供您的需要。</span><span class="sxs-lookup"><span data-stu-id="ce74c-252">However, avoid creating and using filters purely for logging purposes, since the [built-in framework logging features](../../fundamentals/logging.md) may already provide what you need.</span></span> <span data-ttu-id="ce74c-253">如果您要將記錄加入至您的篩選，它應該專注於商務網域疑慮或篩選條件，而不是 MVC 動作或其他架構事件的特定行為。</span><span class="sxs-lookup"><span data-stu-id="ce74c-253">If you're going to add logging to your filters, it should focus on business domain concerns or behavior specific to your filter, rather than MVC actions or other framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="ce74c-254">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="ce74c-254">ServiceFilterAttribute</span></span>

<span data-ttu-id="ce74c-255">A `ServiceFilter` DI 從擷取執行個體的篩選器。</span><span class="sxs-lookup"><span data-stu-id="ce74c-255">A `ServiceFilter` retrieves an instance of the filter from DI.</span></span> <span data-ttu-id="ce74c-256">您將篩選加入至容器中`ConfigureServices`，並將它在參考`ServiceFilter`屬性</span><span class="sxs-lookup"><span data-stu-id="ce74c-256">You add the filter to the container in `ConfigureServices`, and reference it in a `ServiceFilter` attribute</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="ce74c-257">使用`ServiceFilter`而不註冊例外狀況的篩選器類型的結果：</span><span class="sxs-lookup"><span data-stu-id="ce74c-257">Using `ServiceFilter` without registering the filter type results in an exception:</span></span>

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

<span data-ttu-id="ce74c-258">`ServiceFilterAttribute`實作`IFilterFactory`，而這會公開單一方法來建立`IFilter`執行個體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-258">`ServiceFilterAttribute` implements `IFilterFactory`, which exposes a single method for creating an `IFilter` instance.</span></span> <span data-ttu-id="ce74c-259">如果是`ServiceFilterAttribute`、`IFilterFactory`介面的`CreateInstance`從服務容器 (DI) 載入指定的型別實作方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-259">In the case of `ServiceFilterAttribute`, the `IFilterFactory` interface's `CreateInstance` method is implemented to load the specified type from the services container (DI).</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="ce74c-260">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="ce74c-260">TypeFilterAttribute</span></span>

<span data-ttu-id="ce74c-261">`TypeFilterAttribute`非常類似於`ServiceFilterAttribute`(也會實作`IFilterFactory`)，但其類型未解析直接從 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="ce74c-261">`TypeFilterAttribute` is very similar to `ServiceFilterAttribute` (and also implements `IFilterFactory`), but its type is not resolved directly from the DI container.</span></span> <span data-ttu-id="ce74c-262">相反地，它會具現化型別使用`Microsoft.Extensions.DependencyInjection.ObjectFactory`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-262">Instead, it instantiates the type by using `Microsoft.Extensions.DependencyInjection.ObjectFactory`.</span></span>

<span data-ttu-id="ce74c-263">由於此差別，使用參考的型別`TypeFilterAttribute`不需要先註冊與容器 （但仍必須由容器及其相依性）。</span><span class="sxs-lookup"><span data-stu-id="ce74c-263">Because of this difference, types that are referenced using the `TypeFilterAttribute` do not need to be registered with the container first (but they will still have their dependencies fulfilled by the container).</span></span> <span data-ttu-id="ce74c-264">此外，`TypeFilterAttribute`可以選擇性地接受問題類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="ce74c-264">Also, `TypeFilterAttribute` can optionally accept constructor arguments for the type in question.</span></span> <span data-ttu-id="ce74c-265">下列範例示範如何將引數傳遞至型別，使用`TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="ce74c-265">The following example demonstrates how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<span data-ttu-id="ce74c-266">如果您有篩選，不需要任何引數，但其中有要填入的 DI 需要的建構函式相依性，您可以使用您自己的具名的屬性類別和方法，而非`[TypeFilter(typeof(FilterType))]`)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-266">If you have a filter that doesn't require any arguments, but which has constructor dependencies that need to be filled by DI, you can use your own named attribute on classes and methods instead of `[TypeFilter(typeof(FilterType))]`).</span></span> <span data-ttu-id="ce74c-267">下列的篩選條件會顯示如何實作：</span><span class="sxs-lookup"><span data-stu-id="ce74c-267">The following filter shows how this can be implemented:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="ce74c-268">此篩選條件可以套用至類別或方法使用`[SampleActionFilter]`語法，而不需使用`[TypeFilter]`或`[ServiceFilter]`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-268">This filter can be applied to classes or methods using the `[SampleActionFilter]` syntax, instead of having to use `[TypeFilter]` or `[ServiceFilter]`.</span></span>

## <a name="authorization-filters"></a><span data-ttu-id="ce74c-269">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="ce74c-269">Authorization filters</span></span>

<span data-ttu-id="ce74c-270">*授權篩選條件*控制存取動作方法，並且會篩選管線內執行的第一個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-270">*Authorization filters* control access to action methods and are the first filters to be executed within the filter pipeline.</span></span> <span data-ttu-id="ce74c-271">它們只有方法，不同於大部分的篩選器可支援在方法前後之前。</span><span class="sxs-lookup"><span data-stu-id="ce74c-271">They have only a before method, unlike most filters that support before and after methods.</span></span> <span data-ttu-id="ce74c-272">您只應該撰寫自訂的授權篩選條件如果您要撰寫您自己的授權架構。</span><span class="sxs-lookup"><span data-stu-id="ce74c-272">You should only write a custom authorization filter if you are writing your own authorization framework.</span></span> <span data-ttu-id="ce74c-273">偏好設定授權原則，或透過撰寫自訂的篩選條件中撰寫自訂授權原則。</span><span class="sxs-lookup"><span data-stu-id="ce74c-273">Prefer configuring your authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="ce74c-274">內建篩選器實作是只負責呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="ce74c-274">The built-in filter implementation is just responsible for calling the authorization system.</span></span>

<span data-ttu-id="ce74c-275">請注意，您不應該擲回例外狀況中授權篩選條件，因為不會處理此例外狀況 （例外狀況篩選條件將不會處理這類）。</span><span class="sxs-lookup"><span data-stu-id="ce74c-275">Note that you should not throw exceptions within authorization filters, since nothing will handle the exception (exception filters won't handle them).</span></span> <span data-ttu-id="ce74c-276">相反地，發出一項挑戰，或尋找另一種方式。</span><span class="sxs-lookup"><span data-stu-id="ce74c-276">Instead, issue a challenge or find another way.</span></span>

<span data-ttu-id="ce74c-277">深入了解[授權](../../security/authorization/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-277">Learn more about [Authorization](../../security/authorization/index.md).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="ce74c-278">資源篩選</span><span class="sxs-lookup"><span data-stu-id="ce74c-278">Resource filters</span></span>

<span data-ttu-id="ce74c-279">*資源的篩選器*實作`IResourceFilter`或`IAsyncResourceFilter`介面和其執行包裝大部分的篩選器管線。</span><span class="sxs-lookup"><span data-stu-id="ce74c-279">*Resource filters* implement either the `IResourceFilter` or `IAsyncResourceFilter` interface, and their execution wraps most of the filter pipeline.</span></span> <span data-ttu-id="ce74c-280">(只有[授權篩選條件](#authorization-filters)之前執行。)資源篩選就特別有用，如果您需要最少運算的大部分工作的要求正在進行的。</span><span class="sxs-lookup"><span data-stu-id="ce74c-280">(Only [Authorization filters](#authorization-filters) run before them.) Resource filters are especially useful if you need to short-circuit most of the work a request is doing.</span></span> <span data-ttu-id="ce74c-281">比方說，如果回應已快取中快取的篩選條件可以避免管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ce74c-281">For example, a caching filter can avoid the rest of the pipeline if the response is already in the cache.</span></span>

<span data-ttu-id="ce74c-282">[簡短短路的資源篩選](#short-circuiting-resource-filter)稍早所示為資源的篩選器的其中一個範例。</span><span class="sxs-lookup"><span data-stu-id="ce74c-282">The [short circuiting resource filter](#short-circuiting-resource-filter) shown earlier is one example of a resource filter.</span></span> <span data-ttu-id="ce74c-283">另一個例子是[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)，這樣就不會存取表單資料的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="ce74c-283">Another example is [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), which prevents model binding from accessing the form data.</span></span> <span data-ttu-id="ce74c-284">它可用於您知道，您會收到大型檔案上傳，並想要防止表單讀入記憶體的情況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-284">It's useful for cases where you know that you're going to receive large file uploads and want to prevent the form from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="ce74c-285">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="ce74c-285">Action filters</span></span>

<span data-ttu-id="ce74c-286">*動作篩選條件*實作`IActionFilter`或`IAsyncActionFilter`介面和其執行括住的動作方法執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-286">*Action filters* implement either the `IActionFilter` or `IAsyncActionFilter` interface, and their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="ce74c-287">以下是範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="ce74c-287">Here's a sample action filter:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="ce74c-288">[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext)提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ce74c-288">The [ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) provides the following properties:</span></span>

* <span data-ttu-id="ce74c-289">`ActionArguments`-可讓您管理動作的輸入。</span><span class="sxs-lookup"><span data-stu-id="ce74c-289">`ActionArguments` - lets you manipulate the inputs to the action.</span></span>
* <span data-ttu-id="ce74c-290">`Controller`-可讓您管理控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-290">`Controller` - lets you manipulate the controller instance.</span></span> 
* <span data-ttu-id="ce74c-291">`Result`-將此設定 short-circuits 執行的動作方法和後續的動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-291">`Result` - setting this short-circuits execution of the action method and subsequent action filters.</span></span> <span data-ttu-id="ce74c-292">擲回例外狀況也可避免執行的動作方法和後續的篩選條件，但會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="ce74c-292">Throwing an exception also prevents execution of the action method and subsequent filters, but is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="ce74c-293">[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext)提供`Controller`和`Result`再加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ce74c-293">The [ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="ce74c-294">`Canceled`-將如果動作執行的最少運算的另一個篩選，則為 true。</span><span class="sxs-lookup"><span data-stu-id="ce74c-294">`Canceled` - will be true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="ce74c-295">`Exception`-將會為非 null 的動作或後續的動作篩選條件項擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-295">`Exception` - will be non-null if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="ce74c-296">設定這個屬性為 null，有效地 'handles' 例外狀況，以及`Result`會執行，如同它已正常傳回從動作方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-296">Setting this property to null effectively 'handles' an exception, and `Result` will be executed as if it were returned from the action method normally.</span></span>

<span data-ttu-id="ce74c-297">如`IAsyncActionFilter`，呼叫`ActionExecutionDelegate`執行任何後續的動作篩選條件和動作方法，傳回`ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-297">For an `IAsyncActionFilter`, a call to the `ActionExecutionDelegate` executes any subsequent action filters and the action method, returning an `ActionExecutedContext`.</span></span> <span data-ttu-id="ce74c-298">最短路徑，請指派`ActionExecutingContext.Result`某些結果執行個體，並沒有呼叫`ActionExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-298">To short-circuit, assign `ActionExecutingContext.Result` to some result instance and do not call the `ActionExecutionDelegate`.</span></span>

<span data-ttu-id="ce74c-299">架構提供一個抽象`ActionFilterAttribute`可以進行子類別。</span><span class="sxs-lookup"><span data-stu-id="ce74c-299">The framework provides an abstract `ActionFilterAttribute` that you can subclass.</span></span> 

<span data-ttu-id="ce74c-300">您可以使用動作篩選條件會自動驗證模型狀態，並傳回任何錯誤，如果狀態不正確：</span><span class="sxs-lookup"><span data-stu-id="ce74c-300">You can use an action filter to automatically validate model state and return any errors if the state is invalid:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

<span data-ttu-id="ce74c-301">`OnActionExecuted`方法執行之後的動作方法和可以看到及管理透過動作的結果`ActionExecutedContext.Result`屬性。</span><span class="sxs-lookup"><span data-stu-id="ce74c-301">The `OnActionExecuted` method runs after the action method and can see and manipulate the results of the action through the `ActionExecutedContext.Result` property.</span></span> <span data-ttu-id="ce74c-302">`ActionExecutedContext.Canceled`將會設為 true 如果動作執行的最少運算的另一個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-302">`ActionExecutedContext.Canceled` will be set to true if the action execution was short-circuited by another filter.</span></span> <span data-ttu-id="ce74c-303">`ActionExecutedContext.Exception`將設定為非 null 值的動作或後續的動作篩選條件項擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-303">`ActionExecutedContext.Exception` will be set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="ce74c-304">設定`ActionExecutedContext.Exception`為 null，有效地 'handles' 例外狀況，以及`ActionExectedContext.Result`就如同它已正常傳回從動作方法再執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-304">Setting `ActionExecutedContext.Exception` to null effectively 'handles' an exception, and `ActionExectedContext.Result` will then be executed as if it were returned from the action method normally.</span></span>

## <a name="exception-filters"></a><span data-ttu-id="ce74c-305">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="ce74c-305">Exception filters</span></span>

<span data-ttu-id="ce74c-306">*例外狀況篩選條件*實作`IExceptionFilter`或`IAsyncExceptionFilter`介面。</span><span class="sxs-lookup"><span data-stu-id="ce74c-306">*Exception filters* implement either the `IExceptionFilter` or `IAsyncExceptionFilter` interface.</span></span> <span data-ttu-id="ce74c-307">它們可以用來實作常見的錯誤處理應用程式的原則。</span><span class="sxs-lookup"><span data-stu-id="ce74c-307">They can be used to implement common error handling policies for an app.</span></span> 

<span data-ttu-id="ce74c-308">下列範例例外狀況篩選條件會顯示有關開發應用程式時，會發生的例外狀況詳細資料使用自訂的開發人員檢視時發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="ce74c-308">The following sample exception filter uses a custom developer error view to display details about exceptions that occur when the application is in development:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

<span data-ttu-id="ce74c-309">例外狀況篩選條件不會有兩個事件 （如之前和之後）-只實作`OnException`(或`OnExceptionAsync`)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-309">Exception filters do not have two events (for before and after) - they only implement `OnException` (or `OnExceptionAsync`).</span></span> 

<span data-ttu-id="ce74c-310">例外狀況篩選條件處理未處理的例外狀況發生在控制站建立[模型繫結](../models/model-binding.md)，動作篩選條件或動作方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-310">Exception filters handle unhandled exceptions that occur in controller creation, [model binding](../models/model-binding.md), action filters, or action methods.</span></span> <span data-ttu-id="ce74c-311">它們不會攔截例外狀況是發生在資源篩選器、 結果篩選條件，還是 MVC 結果執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-311">They won't catch exceptions that occur in Resource filters, Result filters, or MVC Result execution.</span></span>

<span data-ttu-id="ce74c-312">若要處理的例外狀況，將`ExceptionContext.ExceptionHandled`屬性設為 true，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="ce74c-312">To handle an exception, set the `ExceptionContext.ExceptionHandled` property to true or write a response.</span></span> <span data-ttu-id="ce74c-313">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-313">This stops propagation of the exception.</span></span> <span data-ttu-id="ce74c-314">請注意例外狀況篩選條件無法開啟到 「 成功 」 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-314">Note that an Exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="ce74c-315">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="ce74c-315">Only an Action filter can do that.</span></span>

> [!NOTE]
> <span data-ttu-id="ce74c-316">在 ASP.NET 1.1 中，會傳送回應您設定如果`ExceptionHandled`為 true**和**撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="ce74c-316">In ASP.NET 1.1, the response is not sent if you set `ExceptionHandled` to true **and** write a response.</span></span> <span data-ttu-id="ce74c-317">在該案例中，ASP.NET Core 1.0 並傳送回應，且 ASP.NET Core 1.1.2 會傳回至 1.0 的行為。</span><span class="sxs-lookup"><span data-stu-id="ce74c-317">In that scenario, ASP.NET Core 1.0 does send the response, and ASP.NET Core 1.1.2 will return to the 1.0 behavior.</span></span> <span data-ttu-id="ce74c-318">如需詳細資訊，請參閱[發出 #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub 儲存機制中。</span><span class="sxs-lookup"><span data-stu-id="ce74c-318">For more information, see [issue #5594](https://github.com/aspnet/Mvc/issues/5594) in the GitHub repository.</span></span> 

<span data-ttu-id="ce74c-319">例外狀況篩選條件則適合用於設陷 MVC 動作中發生的例外狀況，但是它們並不具有彈性，錯誤處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-319">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="ce74c-320">偏好以一般的情況下中, 介軟體，並使用篩選器只需要執行錯誤處理*不同*根據選擇的 MVC 動作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-320">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span> <span data-ttu-id="ce74c-321">比方說，您的應用程式可能會有兩個應用程式開發介面端點及檢視/HTML，動作方法。</span><span class="sxs-lookup"><span data-stu-id="ce74c-321">For example, your app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="ce74c-322">雖然檢視為基礎的動作會傳回以 HTML 錯誤頁面 API 端點可能會傳回為 JSON，資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ce74c-322">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

<span data-ttu-id="ce74c-323">架構提供一個抽象`ExceptionFilterAttribute`可以進行子類別。</span><span class="sxs-lookup"><span data-stu-id="ce74c-323">The framework provides an abstract `ExceptionFilterAttribute` that you can subclass.</span></span> 

## <a name="result-filters"></a><span data-ttu-id="ce74c-324">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="ce74c-324">Result filters</span></span>

<span data-ttu-id="ce74c-325">*產生篩選*實作`IResultFilter`或`IAsyncResultFilter`介面和其執行括住的動作結果執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-325">*Result filters* implement either the `IResultFilter` or `IAsyncResultFilter` interface, and their execution surrounds the execution of action results.</span></span> 

<span data-ttu-id="ce74c-326">以下是結果篩選條件，將 HTTP 標頭的範例。</span><span class="sxs-lookup"><span data-stu-id="ce74c-326">Here's an example of a Result filter that adds an HTTP header.</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="ce74c-327">正在執行的結果類型取決於有問題的動作。</span><span class="sxs-lookup"><span data-stu-id="ce74c-327">The kind of result being executed depends on the action in question.</span></span> <span data-ttu-id="ce74c-328">傳回檢視的 MVC 動作會包含所有處理一部分的 razor`ViewResult`正在執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-328">An MVC action returning a view would include all razor processing as part of the `ViewResult` being executed.</span></span> <span data-ttu-id="ce74c-329">API 方法可能會執行一些序列化的執行結果的一部分。</span><span class="sxs-lookup"><span data-stu-id="ce74c-329">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="ce74c-330">深入了解[動作結果](actions.md)</span><span class="sxs-lookup"><span data-stu-id="ce74c-330">Learn more about [action results](actions.md)</span></span>

<span data-ttu-id="ce74c-331">動作篩選條件的動作產生的動作結果時，結果篩選條件才會執行成功的結果集。</span><span class="sxs-lookup"><span data-stu-id="ce74c-331">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="ce74c-332">當例外狀況篩選條件處理例外狀況時，不會執行結果篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-332">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="ce74c-333">`OnResultExecuting`方法可以最少運算執行的動作結果和後續的結果篩選條件設定`ResultExecutingContext.Cancel`為 true。</span><span class="sxs-lookup"><span data-stu-id="ce74c-333">The `OnResultExecuting` method can short-circuit execution of the action result and subsequent result filters by setting `ResultExecutingContext.Cancel` to true.</span></span> <span data-ttu-id="ce74c-334">最少運算，以避免產生空的回應時，您通常應該寫入至回應物件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-334">You should generally write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="ce74c-335">擲回例外狀況也會防止執行的動作結果和後續的篩選條件，但會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="ce74c-335">Throwing an exception will also prevent execution of the action result and subsequent filters, but will be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="ce74c-336">當`OnResultExecuted`方法執行時，回應可能傳送至用戶端，且 （除非發生例外狀況） 無法進一步變更。</span><span class="sxs-lookup"><span data-stu-id="ce74c-336">When the `OnResultExecuted` method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).</span></span> <span data-ttu-id="ce74c-337">`ResultExecutedContext.Canceled`將會設為 true 如果動作結果執行的最少運算的另一個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-337">`ResultExecutedContext.Canceled` will be set to true if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="ce74c-338">`ResultExecutedContext.Exception`如果動作結果或接下來的結果篩選器擲回例外狀況將會設定為非 null 值。</span><span class="sxs-lookup"><span data-stu-id="ce74c-338">`ResultExecutedContext.Exception` will be set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="ce74c-339">設定`Exception`到 null 有效地處理例外狀況，並可避免從正在由 MVC 管線中，稍後重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ce74c-339">Setting `Exception` to null effectively 'handles' an exception and prevents the exception from being rethrown by MVC later in the pipeline.</span></span> <span data-ttu-id="ce74c-340">當您處理結果篩選條件中的例外狀況時，您可能無法寫入至回應的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ce74c-340">When you're handling an exception in a result filter, you might not be able to write any data to the response.</span></span> <span data-ttu-id="ce74c-341">如果在動作結果會擲回推出透過其執行中，標頭已清除至用戶端，沒有任何可靠的機制，傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="ce74c-341">If the action result throws partway through its execution, and the headers have already been flushed to the client, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="ce74c-342">如`IAsyncResultFilter`呼叫`await next()`上`ResultExecutionDelegate`執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="ce74c-342">For an `IAsyncResultFilter` a call to `await next()` on the `ResultExecutionDelegate` executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="ce74c-343">最短路徑，請將`ResultExecutingContext.Cancel`至 true 並沒有呼叫`ResultExectionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="ce74c-343">To short-circuit, set `ResultExecutingContext.Cancel` to true and do not call the `ResultExectionDelegate`.</span></span>

<span data-ttu-id="ce74c-344">架構提供一個抽象`ResultFilterAttribute`可以進行子類別。</span><span class="sxs-lookup"><span data-stu-id="ce74c-344">The framework provides an abstract `ResultFilterAttribute` that you can subclass.</span></span> <span data-ttu-id="ce74c-345">[AddHeaderAttribute](#add-header-attribute)稍早所示的類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="ce74c-345">The [AddHeaderAttribute](#add-header-attribute) class shown earlier is an example of a result filter attribute.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="ce74c-346">篩選管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="ce74c-346">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="ce74c-347">資源的篩選器一樣[中介軟體](../../fundamentals/middleware.md)在於它們圍繞稍後在管線中出現的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="ce74c-347">Resource filters work like [middleware](../../fundamentals/middleware.md) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="ce74c-348">但與中介軟體，因為它們屬於的 MVC 中，這表示他們擁有存取權 MVC 內容和建構不同的篩選器。</span><span class="sxs-lookup"><span data-stu-id="ce74c-348">But filters differ from middleware in that they are part of MVC, which means that they have access to MVC context and constructs.</span></span>

<span data-ttu-id="ce74c-349">在 ASP.NET Core 1.1 中，您可以使用篩選器管線中的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-349">In ASP.NET Core 1.1, you can use middleware in the filter pipeline.</span></span> <span data-ttu-id="ce74c-350">您可能想要這樣做，您如有需要存取 MVC 路由資料，或其中一個控制器或動作應該僅針對特定執行中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="ce74c-350">You might want to do that if you have a middleware component that needs access to MVC route data, or one that should run only for certain controllers or actions.</span></span>

<span data-ttu-id="ce74c-351">若要使用做為篩選條件的中介軟體，建立一個具有型別`Configure`方法，可指定您想要將篩選管線插入的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ce74c-351">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware that you want to inject into the filter pipeline.</span></span> <span data-ttu-id="ce74c-352">以下是範例會使用來建立要求的目前文化特性的當地語系化的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ce74c-352">Here's an example that uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="ce74c-353">然後您可以使用`MiddlewareFilterAttribute`執行選取的控制器或動作中介軟體或全域：</span><span class="sxs-lookup"><span data-stu-id="ce74c-353">You can then use the `MiddlewareFilterAttribute` to run the middleware for a selected controller or action or globally:</span></span>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="ce74c-354">中介軟體篩選條件執行篩選管線做為資源的篩選，模型繫結之前和之後，管線的其餘部分的相同階段。</span><span class="sxs-lookup"><span data-stu-id="ce74c-354">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="ce74c-355">下一步動作</span><span class="sxs-lookup"><span data-stu-id="ce74c-355">Next actions</span></span>

<span data-ttu-id="ce74c-356">若要嘗試使用篩選器，[下載、 測試及修改範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="ce74c-356">To experiment with filters, [download, test and modify the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
