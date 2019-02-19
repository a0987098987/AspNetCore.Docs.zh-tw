---
title: ASP.NET Core 中的篩選條件
author: ardalis
description: 深入了解「篩選條件」的運作方式，以及如何在 ASP.NET Core MVC 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 3cd576b389a2a4384c0ba90b5740ac42140533cc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159310"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="851ac-103">ASP.NET Core 中的篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="851ac-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra/) 以及 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="851ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="851ac-105">ASP.NET Core MVC 中的「篩選條件」可讓您在要求處理管線的特定階段之前或之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-105">*Filters* in ASP.NET Core MVC allow you to run code before or after specific stages in the request processing pipeline.</span></span>

 <span data-ttu-id="851ac-106">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="851ac-106">Built-in filters handle tasks such as:</span></span>

 * <span data-ttu-id="851ac-107">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="851ac-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
 * <span data-ttu-id="851ac-108">確定所有要求都使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="851ac-108">Ensuring that all requests use HTTPS.</span></span>
 * <span data-ttu-id="851ac-109">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="851ac-109">Response caching (short-circuiting the request pipeline to return a cached response).</span></span> 

<span data-ttu-id="851ac-110">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="851ac-110">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="851ac-111">篩選條件可以避免在動作之間複製程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-111">Filters can avoid duplicating code across actions.</span></span> <span data-ttu-id="851ac-112">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="851ac-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="851ac-113">[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="851ac-113">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="851ac-114">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="851ac-114">How filters work</span></span>

<span data-ttu-id="851ac-115">篩選條件在「MVC 動作引動過程管線」中執行，這有時又稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="851ac-115">Filters run within the *MVC action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="851ac-116">MVC 之後的篩選條件管線執行會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="851ac-116">The filter pipeline runs after MVC selects the action to execute.</span></span>

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 MVC 動作引動過程管線。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="851ac-119">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="851ac-119">Filter types</span></span>

<span data-ttu-id="851ac-120">每個篩選類型會在篩選條件管線中的不同階段執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-120">Each filter type is executed at a different stage in the filter pipeline.</span></span>

* <span data-ttu-id="851ac-121">[授權篩選條件](#authorization-filters)會先執行，用來判斷目前使用者是否被授權提出目前的要求。</span><span class="sxs-lookup"><span data-stu-id="851ac-121">[Authorization filters](#authorization-filters) run first and are used to determine whether the current user is authorized for the current request.</span></span> <span data-ttu-id="851ac-122">如果要求未經授權，它們可以縮短管線。</span><span class="sxs-lookup"><span data-stu-id="851ac-122">They can short-circuit the pipeline if a request is unauthorized.</span></span> 

* <span data-ttu-id="851ac-123">[資源篩選條件](#resource-filters)是授權之後最先處理要求的。</span><span class="sxs-lookup"><span data-stu-id="851ac-123">[Resource filters](#resource-filters) are the first to handle a request after authorization.</span></span>  <span data-ttu-id="851ac-124">它們可以在篩選條件管線的其餘部分之前執行程式碼，以及在管線的其餘部分完成之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-124">They can run code before the rest of the filter pipeline, and after the rest of the pipeline has completed.</span></span> <span data-ttu-id="851ac-125">基於效能的考量，它們適合用來實作快取或以其他方式縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="851ac-125">They're useful to implement caching or otherwise short-circuit the filter pipeline for performance reasons.</span></span> <span data-ttu-id="851ac-126">它們在模型繫結之前執行，因此可能會影響模型繫結。</span><span class="sxs-lookup"><span data-stu-id="851ac-126">They run before model binding, so they can influence model binding.</span></span>

* <span data-ttu-id="851ac-127">[動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-127">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="851ac-128">它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-128">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="851ac-129">Razor Pages 中不支援動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-129">Action filters are not supported in Razor Pages.</span></span>

* <span data-ttu-id="851ac-130">[例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-130">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="851ac-131">[結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-131">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="851ac-132">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-132">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="851ac-133">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="851ac-133">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="851ac-134">下圖顯示這些篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="851ac-134">The following diagram shows how these filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="851ac-137">實作</span><span class="sxs-lookup"><span data-stu-id="851ac-137">Implementation</span></span>

<span data-ttu-id="851ac-138">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="851ac-138">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span> 

<span data-ttu-id="851ac-139">同步篩選條件可以在管線階段之前和之後執行程式碼，它們定義 On*Stage*Executing 以及 On*Stage*Executed 方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-139">Synchronous filters that can run code both before and after their pipeline stage define On*Stage*Executing and On*Stage*Executed methods.</span></span> <span data-ttu-id="851ac-140">例如，`OnActionExecuting` 會在呼叫動作方法之前呼叫，而 `OnActionExecuted` 則在動作方法傳回之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="851ac-140">For example, `OnActionExecuting` is called before the action method is called, and `OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

<span data-ttu-id="851ac-141">非同步篩選條件會定義單一的 On*Stage*ExecutionAsync 方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-141">Asynchronous filters define a single On*Stage*ExecutionAsync method.</span></span> <span data-ttu-id="851ac-142">這個方法會接受 *FilterType*ExecutionDelegate 委派，它會執行篩選條件的管線階段。</span><span class="sxs-lookup"><span data-stu-id="851ac-142">This method takes a *FilterType*ExecutionDelegate delegate which executes the filter's pipeline stage.</span></span> <span data-ttu-id="851ac-143">例如，`ActionExecutionDelegate` 會呼叫動作方法或下一個動作篩選條件，而且您可以在呼叫之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-143">For example, `ActionExecutionDelegate` calls the action method or next action filter, and you can execute code before and after you call it.</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

<span data-ttu-id="851ac-144">您可以為單一類別中的多個篩選條件階段實作介面。</span><span class="sxs-lookup"><span data-stu-id="851ac-144">You can implement interfaces for multiple filter stages in a single class.</span></span> <span data-ttu-id="851ac-145">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。</span><span class="sxs-lookup"><span data-stu-id="851ac-145">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

> [!NOTE]
> <span data-ttu-id="851ac-146">請實作同步**或**非同步版本的篩選條件介面，但不要兩者同時實作。</span><span class="sxs-lookup"><span data-stu-id="851ac-146">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="851ac-147">架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="851ac-147">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="851ac-148">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-148">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="851ac-149">如果您要在一個類別上同時實作兩種介面，則只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-149">If you were to implement both interfaces on one class, only the async method would be called.</span></span> <span data-ttu-id="851ac-150">使用抽象類別時，例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，您只會覆寫同步方法或每個篩選條件類型的非同步方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-150">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> you would override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="ifilterfactory"></a><span data-ttu-id="851ac-151">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="851ac-151">IFilterFactory</span></span>

<span data-ttu-id="851ac-152">[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="851ac-152">[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="851ac-153">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-153">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="851ac-154">當架構準備要叫用篩選條件時，會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="851ac-154">When the framework prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="851ac-155">如果該轉換成功，則會呼叫 [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) 方法來建立將被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-155">If that cast succeeds, the [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) method is called to create the `IFilterMetadata` instance that will be invoked.</span></span> <span data-ttu-id="851ac-156">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="851ac-156">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="851ac-157">您可以對您自己的屬性實作，實作 `IFilterFactory` 以作為另一種建立篩選條件的方法：</span><span class="sxs-lookup"><span data-stu-id="851ac-157">You can implement `IFilterFactory` on your own attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a><span data-ttu-id="851ac-158">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="851ac-158">Built-in filter attributes</span></span>

<span data-ttu-id="851ac-159">架構包含內建的屬性型篩選條件，您可以進行子分類和自訂。</span><span class="sxs-lookup"><span data-stu-id="851ac-159">The framework includes built-in attribute-based filters that you can subclass and customize.</span></span> <span data-ttu-id="851ac-160">例如，下列結果篩選條件會將標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="851ac-160">For example, the following Result filter adds a header to the response.</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

<span data-ttu-id="851ac-161">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="851ac-161">Attributes allow filters to accept arguments, as shown in the example above.</span></span> <span data-ttu-id="851ac-162">您會將此屬性新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="851ac-162">You would add this attribute to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="851ac-163">`Index` 動作的結果如下所示 - 回應標頭顯示在右下方。</span><span class="sxs-lookup"><span data-stu-id="851ac-163">The result of the `Index` action is shown below - the response headers are displayed on the bottom right.</span></span>

![顯示回應標頭的 Microsoft Edge 開發人員工具，包括作者 Steve Smith @ardalis](filters/_static/add-header.png)

<span data-ttu-id="851ac-165">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="851ac-165">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="851ac-166">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="851ac-166">Filter attributes:</span></span>

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

<span data-ttu-id="851ac-167">[本文稍後](#dependency-injection)將說明 `TypeFilterAttribute` 和 `ServiceFilterAttribute`。</span><span class="sxs-lookup"><span data-stu-id="851ac-167">`TypeFilterAttribute` and `ServiceFilterAttribute` are explained [later in this article](#dependency-injection).</span></span>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="851ac-168">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="851ac-168">Filter scopes and order of execution</span></span>

<span data-ttu-id="851ac-169">篩選條件可以新增至管線的三個「範圍」之一。</span><span class="sxs-lookup"><span data-stu-id="851ac-169">A filter can be added to the pipeline at one of three *scopes*.</span></span> <span data-ttu-id="851ac-170">您可以使用屬性將篩選條件新增至特定的動作方法或控制器類別。</span><span class="sxs-lookup"><span data-stu-id="851ac-170">You can add a filter to a particular action method or to a controller class by using an attribute.</span></span> <span data-ttu-id="851ac-171">或者您可以為所有控制器和動作全域地註冊篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-171">Or you can register a filter globally for all controllers and actions.</span></span> <span data-ttu-id="851ac-172">藉由將篩選條件新增至 `ConfigureServices` 中的 `MvcOptions.Filters` 集合，即可全域新增篩選條件：</span><span class="sxs-lookup"><span data-stu-id="851ac-172">Filters are added globally by adding it to the `MvcOptions.Filters` collection in `ConfigureServices`:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a><span data-ttu-id="851ac-173">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="851ac-173">Default order of execution</span></span>

<span data-ttu-id="851ac-174">當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。</span><span class="sxs-lookup"><span data-stu-id="851ac-174">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="851ac-175">全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-175">Global filters surround class filters, which in turn surround method filters.</span></span> <span data-ttu-id="851ac-176">這有時候稱為「俄羅斯娃娃」巢狀結構，因為範圍的每次增加都圍繞著前一個範圍，就像[俄羅斯娃娃](https://wikipedia.org/wiki/Matryoshka_doll)一樣。</span><span class="sxs-lookup"><span data-stu-id="851ac-176">This is sometimes referred to as "Russian doll" nesting, as each increase in scope is wrapped around the previous scope, like a [nesting doll](https://wikipedia.org/wiki/Matryoshka_doll).</span></span> <span data-ttu-id="851ac-177">您通常不必明確決定順序，即可得到想要的覆寫行為。</span><span class="sxs-lookup"><span data-stu-id="851ac-177">You generally get the desired overriding behavior without having to explicitly determine ordering.</span></span>

<span data-ttu-id="851ac-178">因為這個巢狀結構，篩選條件的「之後」程式碼，執行的順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="851ac-178">As a result of this nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="851ac-179">順序看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="851ac-179">The sequence looks like this:</span></span>

* <span data-ttu-id="851ac-180">全域套用的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-180">The *before* code of filters applied globally</span></span>
  * <span data-ttu-id="851ac-181">套用至控制器的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-181">The *before* code of filters applied to controllers</span></span>
    * <span data-ttu-id="851ac-182">套用至動作方法的篩選條件「之前」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-182">The *before* code of filters applied to action methods</span></span>
    * <span data-ttu-id="851ac-183">套用至動作方法的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-183">The *after* code of filters applied to action methods</span></span>
  * <span data-ttu-id="851ac-184">套用至控制器的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-184">The *after* code of filters applied to controllers</span></span>
* <span data-ttu-id="851ac-185">全域套用的篩選條件「之後」程式碼</span><span class="sxs-lookup"><span data-stu-id="851ac-185">The *after* code of filters applied globally</span></span>
  
<span data-ttu-id="851ac-186">下列範例說明同步動作篩選條件的篩選條件方法呼叫順序。</span><span class="sxs-lookup"><span data-stu-id="851ac-186">Here's an example that illustrates the order in which filter methods are called for synchronous Action filters.</span></span>

| <span data-ttu-id="851ac-187">序列</span><span class="sxs-lookup"><span data-stu-id="851ac-187">Sequence</span></span> | <span data-ttu-id="851ac-188">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="851ac-188">Filter scope</span></span> | <span data-ttu-id="851ac-189">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="851ac-189">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="851ac-190">1</span><span class="sxs-lookup"><span data-stu-id="851ac-190">1</span></span> | <span data-ttu-id="851ac-191">Global</span><span class="sxs-lookup"><span data-stu-id="851ac-191">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="851ac-192">2</span><span class="sxs-lookup"><span data-stu-id="851ac-192">2</span></span> | <span data-ttu-id="851ac-193">控制器</span><span class="sxs-lookup"><span data-stu-id="851ac-193">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="851ac-194">3</span><span class="sxs-lookup"><span data-stu-id="851ac-194">3</span></span> | <span data-ttu-id="851ac-195">方法</span><span class="sxs-lookup"><span data-stu-id="851ac-195">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="851ac-196">4</span><span class="sxs-lookup"><span data-stu-id="851ac-196">4</span></span> | <span data-ttu-id="851ac-197">方法</span><span class="sxs-lookup"><span data-stu-id="851ac-197">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="851ac-198">5</span><span class="sxs-lookup"><span data-stu-id="851ac-198">5</span></span> | <span data-ttu-id="851ac-199">控制器</span><span class="sxs-lookup"><span data-stu-id="851ac-199">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="851ac-200">6</span><span class="sxs-lookup"><span data-stu-id="851ac-200">6</span></span> | <span data-ttu-id="851ac-201">Global</span><span class="sxs-lookup"><span data-stu-id="851ac-201">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="851ac-202">此順序顯示：</span><span class="sxs-lookup"><span data-stu-id="851ac-202">This sequence shows:</span></span>

* <span data-ttu-id="851ac-203">方法篩選條件巢狀位於控制器篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="851ac-203">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="851ac-204">控制器篩選條件巢狀位於全域篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="851ac-204">The controller filter is nested within the global filter.</span></span> 

<span data-ttu-id="851ac-205">換句話說，如果您在非同步篩選條件的 On*Stage*ExecutionAsync 方法內，具有較緊密範圍的所有篩選條件會在您的程式碼在堆疊上時執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-205">To put it another way, if you're inside an async filter's On*Stage*ExecutionAsync method, all of the filters with a tighter scope run while your code is on the stack.</span></span>

> [!NOTE]
> <span data-ttu-id="851ac-206">繼承自 `Controller` 基底類別的每個控制器，都包含 `OnActionExecuting` 和 `OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-206">Every controller that inherits from the `Controller` base class includes `OnActionExecuting` and `OnActionExecuted` methods.</span></span> <span data-ttu-id="851ac-207">這些方法會包裝針對指定動作執行的篩選條件：`OnActionExecuting` 在任何篩選條件之前呼叫，而 `OnActionExecuted` 則在所有篩選條件之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="851ac-207">These methods wrap the filters that run for a given action:  `OnActionExecuting` is called before any of the filters, and `OnActionExecuted` is called after all of the filters.</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="851ac-208">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="851ac-208">Overriding the default order</span></span>

<span data-ttu-id="851ac-209">您可以藉由實作 `IOrderedFilter` 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="851ac-209">You can override the default sequence of execution by implementing `IOrderedFilter`.</span></span> <span data-ttu-id="851ac-210">此介面會公開 `Order` 屬性，其優先順序高於範圍，可決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="851ac-210">This interface exposes an `Order` property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="851ac-211">`Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之前執行其「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-211">A filter with a lower `Order` value will have its *before* code executed before that of a filter with a higher value of `Order`.</span></span> <span data-ttu-id="851ac-212">`Order` 值較低的篩選條件，會在 `Order` 值較高的篩選條件之後執行其「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-212">A filter with a lower `Order` value will have its *after* code executed after that of a filter with a higher `Order` value.</span></span> <span data-ttu-id="851ac-213">您可以使用建構函式參數來設定 `Order` 屬性：</span><span class="sxs-lookup"><span data-stu-id="851ac-213">You can set the `Order` property by using a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="851ac-214">如果您擁有相同 3 個顯示在前面範例中的動作篩選條件，但將控制器的 `Order` 屬性和全域篩選條件分別設到 1 和 2，則執行順序會反轉。</span><span class="sxs-lookup"><span data-stu-id="851ac-214">If you have the same 3 Action filters shown in the preceding example but set the `Order` property of the controller and global filters to 1 and 2 respectively, the order of execution would be reversed.</span></span>

| <span data-ttu-id="851ac-215">序列</span><span class="sxs-lookup"><span data-stu-id="851ac-215">Sequence</span></span> | <span data-ttu-id="851ac-216">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="851ac-216">Filter scope</span></span> | <span data-ttu-id="851ac-217">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="851ac-217">`Order` property</span></span> | <span data-ttu-id="851ac-218">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="851ac-218">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="851ac-219">1</span><span class="sxs-lookup"><span data-stu-id="851ac-219">1</span></span> | <span data-ttu-id="851ac-220">方法</span><span class="sxs-lookup"><span data-stu-id="851ac-220">Method</span></span> | <span data-ttu-id="851ac-221">0</span><span class="sxs-lookup"><span data-stu-id="851ac-221">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="851ac-222">2</span><span class="sxs-lookup"><span data-stu-id="851ac-222">2</span></span> | <span data-ttu-id="851ac-223">控制器</span><span class="sxs-lookup"><span data-stu-id="851ac-223">Controller</span></span> | <span data-ttu-id="851ac-224">1</span><span class="sxs-lookup"><span data-stu-id="851ac-224">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="851ac-225">3</span><span class="sxs-lookup"><span data-stu-id="851ac-225">3</span></span> | <span data-ttu-id="851ac-226">Global</span><span class="sxs-lookup"><span data-stu-id="851ac-226">Global</span></span> | <span data-ttu-id="851ac-227">2</span><span class="sxs-lookup"><span data-stu-id="851ac-227">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="851ac-228">4</span><span class="sxs-lookup"><span data-stu-id="851ac-228">4</span></span> | <span data-ttu-id="851ac-229">Global</span><span class="sxs-lookup"><span data-stu-id="851ac-229">Global</span></span> | <span data-ttu-id="851ac-230">2</span><span class="sxs-lookup"><span data-stu-id="851ac-230">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="851ac-231">5</span><span class="sxs-lookup"><span data-stu-id="851ac-231">5</span></span> | <span data-ttu-id="851ac-232">控制器</span><span class="sxs-lookup"><span data-stu-id="851ac-232">Controller</span></span> | <span data-ttu-id="851ac-233">1</span><span class="sxs-lookup"><span data-stu-id="851ac-233">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="851ac-234">6</span><span class="sxs-lookup"><span data-stu-id="851ac-234">6</span></span> | <span data-ttu-id="851ac-235">方法</span><span class="sxs-lookup"><span data-stu-id="851ac-235">Method</span></span> | <span data-ttu-id="851ac-236">0</span><span class="sxs-lookup"><span data-stu-id="851ac-236">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="851ac-237">`Order` 屬性在決定篩選條件執行的順序時以範圍取勝。</span><span class="sxs-lookup"><span data-stu-id="851ac-237">The `Order` property trumps scope when determining the order in which filters will run.</span></span> <span data-ttu-id="851ac-238">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="851ac-238">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="851ac-239">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="851ac-239">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="851ac-240">對於內建篩選條件，除非您將 `Order` 設定為非零值，否則範圍決定了順序。</span><span class="sxs-lookup"><span data-stu-id="851ac-240">For built-in filters, scope determines order unless you set `Order` to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="851ac-241">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="851ac-241">Cancellation and short circuiting</span></span>

<span data-ttu-id="851ac-242">您可以設定提供給篩選條件方法之 `context` 參數上的 `Result` 屬性，以在任何時間點縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="851ac-242">You can short-circuit the filter pipeline at any point by setting the `Result` property on the `context` parameter provided to the filter method.</span></span> <span data-ttu-id="851ac-243">比方說，下列資源篩選條件可防止管線的其餘部分執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-243">For instance, the following Resource filter prevents the rest of the pipeline from executing.</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

<span data-ttu-id="851ac-244">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="851ac-244">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="851ac-245">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="851ac-245">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="851ac-246">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-246">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="851ac-247">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="851ac-247">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="851ac-248">因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-248">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="851ac-249">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="851ac-249">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="851ac-250">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-250">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="851ac-251">相依性插入</span><span class="sxs-lookup"><span data-stu-id="851ac-251">Dependency injection</span></span>

<span data-ttu-id="851ac-252">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-252">Filters can be added by type or by instance.</span></span> <span data-ttu-id="851ac-253">如果您新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="851ac-253">If you add an instance, that instance will be used for every request.</span></span> <span data-ttu-id="851ac-254">如果您新增類型，它將會由類型啟動，意思是會為每個要求建立執行個體，並且將會藉由[相依性插入](../../fundamentals/dependency-injection.md)(DI) 填入任何建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="851ac-254">If you add a type, it will be type-activated, meaning an instance will be created for each request and any constructor dependencies will be populated by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="851ac-255">依類型新增篩選條件相當於 `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`。</span><span class="sxs-lookup"><span data-stu-id="851ac-255">Adding a filter by type is equivalent to `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.</span></span>

<span data-ttu-id="851ac-256">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](../../fundamentals/dependency-injection.md) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="851ac-256">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](../../fundamentals/dependency-injection.md) (DI).</span></span> <span data-ttu-id="851ac-257">這是因為屬性都必須在套用它們的地方提供其建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="851ac-257">This is because attributes must have their constructor parameters supplied where they're applied.</span></span> <span data-ttu-id="851ac-258">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="851ac-258">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="851ac-259">如果您的篩選條件有必須從 DI 存取的相依性，會有數種支援的方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-259">If your filters have dependencies that you need to access from DI, there are several supported approaches.</span></span> <span data-ttu-id="851ac-260">您可以使用下列其中一項將篩選條件套用至類別或動作方法：</span><span class="sxs-lookup"><span data-stu-id="851ac-260">You can apply your filter to a class or action method using one of the following:</span></span>

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* <span data-ttu-id="851ac-261">在您的屬性上實作 `IFilterFactory`</span><span class="sxs-lookup"><span data-stu-id="851ac-261">`IFilterFactory` implemented on your attribute</span></span>

> [!NOTE]
> <span data-ttu-id="851ac-262">您可能想要從 DI 取得的一種相依性是記錄器。</span><span class="sxs-lookup"><span data-stu-id="851ac-262">One dependency you might want to get from DI is a logger.</span></span> <span data-ttu-id="851ac-263">不過，不需要單純為了記錄便建立和使用篩選條件，因為[內建的架構記錄功能](xref:fundamentals/logging/index)可能已提供您需要的功能。</span><span class="sxs-lookup"><span data-stu-id="851ac-263">However, avoid creating and using filters purely for logging purposes, since the [built-in framework logging features](xref:fundamentals/logging/index) may already provide what you need.</span></span> <span data-ttu-id="851ac-264">如果您要將記錄新增至您的篩選條件，它應該專注於商務網域考量或您篩選條件的特定行為，而不是 MVC 動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="851ac-264">If you're going to add logging to your filters, it should focus on business domain concerns or behavior specific to your filter, rather than MVC actions or other framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="851ac-265">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="851ac-265">ServiceFilterAttribute</span></span>

<span data-ttu-id="851ac-266">在 DI 中註冊服務篩選條件實作類型。</span><span class="sxs-lookup"><span data-stu-id="851ac-266">Service filter implementation types are registered in DI.</span></span> <span data-ttu-id="851ac-267">`ServiceFilterAttribute` 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-267">A `ServiceFilterAttribute` retrieves an instance of the filter from DI.</span></span> <span data-ttu-id="851ac-268">將 `ServiceFilterAttribute` 新增至 `Startup.ConfigureServices` 的容器中，並在 `[ServiceFilter]` 屬性參考它：</span><span class="sxs-lookup"><span data-stu-id="851ac-268">Add the `ServiceFilterAttribute` to the container in `Startup.ConfigureServices`, and reference it in a `[ServiceFilter]` attribute:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="851ac-269">使用 `ServiceFilterAttribute` 時，設定 `IsReusable` 是一個提示，篩選條件執行個體「可能」會在其建立的要求範圍之外重複使用。</span><span class="sxs-lookup"><span data-stu-id="851ac-269">When using `ServiceFilterAttribute`, setting `IsReusable` is a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="851ac-270">該架構不保證將在稍後的某個時間點建立篩選條件的單一執行個體，或者不會從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-270">The framework provides no guarantees that a single instance of the filter will be created or the filter will not be re-requested from the DI container at some later point.</span></span> <span data-ttu-id="851ac-271">在使用仰賴具有非單一存留期之服務的篩選條件時，請避免使用 `IsReusable`。</span><span class="sxs-lookup"><span data-stu-id="851ac-271">Avoid using `IsReusable` when using a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="851ac-272">使用 `ServiceFilterAttribute` 而不註冊篩選條件類型會導致例外狀況：</span><span class="sxs-lookup"><span data-stu-id="851ac-272">Using `ServiceFilterAttribute` without registering the filter type results in an exception:</span></span>

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

<span data-ttu-id="851ac-273">`ServiceFilterAttribute` 會實作 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="851ac-273">`ServiceFilterAttribute` implements `IFilterFactory`.</span></span> <span data-ttu-id="851ac-274">`IFilterFactory` 會公開 `CreateInstance` 方法來建立 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-274">`IFilterFactory` exposes the `CreateInstance` method for creating an `IFilterMetadata` instance.</span></span> <span data-ttu-id="851ac-275">`CreateInstance` 方法會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="851ac-275">The `CreateInstance` method loads the specified type from the services container (DI).</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="851ac-276">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="851ac-276">TypeFilterAttribute</span></span>

<span data-ttu-id="851ac-277">`TypeFilterAttribute` 類似於 `ServiceFilterAttribute`，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="851ac-277">`TypeFilterAttribute` is similar to `ServiceFilterAttribute`, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="851ac-278">它會使用 `Microsoft.Extensions.DependencyInjection.ObjectFactory` 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="851ac-278">It instantiates the type by using `Microsoft.Extensions.DependencyInjection.ObjectFactory`.</span></span>

<span data-ttu-id="851ac-279">由於此差異：</span><span class="sxs-lookup"><span data-stu-id="851ac-279">Because of this difference:</span></span>

* <span data-ttu-id="851ac-280">使用 `TypeFilterAttribute` 參考的類型不需要先向容器註冊。</span><span class="sxs-lookup"><span data-stu-id="851ac-280">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the container first.</span></span>  <span data-ttu-id="851ac-281">它們的相依性由容器滿足。</span><span class="sxs-lookup"><span data-stu-id="851ac-281">They do have their dependencies fulfilled by the container.</span></span> 
* <span data-ttu-id="851ac-282">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="851ac-282">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="851ac-283">使用 `TypeFilterAttribute` 時，設定 `IsReusable` 是一個提示，篩選條件執行個體「可能」會在其建立的要求範圍之外重複使用。</span><span class="sxs-lookup"><span data-stu-id="851ac-283">When using `TypeFilterAttribute`, setting `IsReusable` is a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="851ac-284">該架構不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-284">The framework provides no guarantees that a single instance of the filter will be created.</span></span> <span data-ttu-id="851ac-285">在使用仰賴具有非單一存留期之服務的篩選條件時，請避免使用 `IsReusable`。</span><span class="sxs-lookup"><span data-stu-id="851ac-285">Avoid using `IsReusable` when using a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="851ac-286">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="851ac-286">The following example demonstrates how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a><span data-ttu-id="851ac-287">在您屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="851ac-287">IFilterFactory implemented on your attribute</span></span>

<span data-ttu-id="851ac-288">如果您的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="851ac-288">If you have a filter that:</span></span>

* <span data-ttu-id="851ac-289">不需要任何引數。</span><span class="sxs-lookup"><span data-stu-id="851ac-289">Doesn't require any arguments.</span></span>
* <span data-ttu-id="851ac-290">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="851ac-290">Has constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="851ac-291">您可以在類別和方法上使用自己的具名屬性，而非 `[TypeFilter(typeof(FilterType))]`)。</span><span class="sxs-lookup"><span data-stu-id="851ac-291">You can use your own named attribute on classes and methods instead of `[TypeFilter(typeof(FilterType))]`).</span></span> <span data-ttu-id="851ac-292">下列篩選條件顯示如何實作這點：</span><span class="sxs-lookup"><span data-stu-id="851ac-292">The following filter shows how this can be implemented:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="851ac-293">此篩選條件可以使用 `[SampleActionFilter]` 語法套用至類別或方法，而不需使用 `[TypeFilter]` 或 `[ServiceFilter]`。</span><span class="sxs-lookup"><span data-stu-id="851ac-293">This filter can be applied to classes or methods using the `[SampleActionFilter]` syntax, instead of having to use `[TypeFilter]` or `[ServiceFilter]`.</span></span>

## <a name="authorization-filters"></a><span data-ttu-id="851ac-294">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-294">Authorization filters</span></span>

<span data-ttu-id="851ac-295">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="851ac-295">*Authorization filters*:</span></span>

* <span data-ttu-id="851ac-296">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="851ac-296">Control access to action methods.</span></span>
* <span data-ttu-id="851ac-297">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-297">Are the first filters to be executed within the filter pipeline.</span></span> 
* <span data-ttu-id="851ac-298">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-298">Have a before method, but no after method.</span></span> 

<span data-ttu-id="851ac-299">您應該唯有在撰寫自己的授權架構時才撰寫自訂授權篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-299">You should only write a custom authorization filter if you are writing your own authorization framework.</span></span> <span data-ttu-id="851ac-300">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-300">Prefer configuring your authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="851ac-301">內建篩選條件實作只負責呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="851ac-301">The built-in filter implementation is just responsible for calling the authorization system.</span></span>

<span data-ttu-id="851ac-302">您不應該在授權篩選條件內擲回例外狀況，因為不會處理例外狀況 (例外狀況篩選條件將不會處理它們)。</span><span class="sxs-lookup"><span data-stu-id="851ac-302">You shouldn't throw exceptions within authorization filters, since nothing will handle the exception (exception filters won't handle them).</span></span> <span data-ttu-id="851ac-303">請考慮在例外狀況發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="851ac-303">Consider issuing a challenge when an exception occurs.</span></span>

<span data-ttu-id="851ac-304">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="851ac-304">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="851ac-305">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-305">Resource filters</span></span>

* <span data-ttu-id="851ac-306">實作 `IResourceFilter` 或 `IAsyncResourceFilter` 介面，</span><span class="sxs-lookup"><span data-stu-id="851ac-306">Implement either the `IResourceFilter` or `IAsyncResourceFilter` interface,</span></span>
* <span data-ttu-id="851ac-307">它們的執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="851ac-307">Their execution wraps most of the filter pipeline.</span></span> 
* <span data-ttu-id="851ac-308">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-308">Only [Authorization filters](#authorization-filters) run before Resource filters.</span></span>

<span data-ttu-id="851ac-309">資源篩選條件適用於縮短要求正在進行的大部分工作。</span><span class="sxs-lookup"><span data-stu-id="851ac-309">Resource filters are useful to short-circuit most of the work a request is doing.</span></span> <span data-ttu-id="851ac-310">比方說，如果回應在快取中，快取篩選條件可以避免管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="851ac-310">For example, a caching filter can avoid the rest of the pipeline if the response is in the cache.</span></span>

<span data-ttu-id="851ac-311">稍早所示的[縮短資源篩選條件](#short-circuiting-resource-filter)是資源篩選條件的一個範例。</span><span class="sxs-lookup"><span data-stu-id="851ac-311">The [short circuiting resource filter](#short-circuiting-resource-filter) shown earlier is one example of a resource filter.</span></span> <span data-ttu-id="851ac-312">另一個例子是 [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)：</span><span class="sxs-lookup"><span data-stu-id="851ac-312">Another example is [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

* <span data-ttu-id="851ac-313">它會導致模型繫結無法存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="851ac-313">It prevents model binding from accessing the form data.</span></span> 
* <span data-ttu-id="851ac-314">它適用於大型檔案上傳，並且想要避免表單被讀入到記憶體。</span><span class="sxs-lookup"><span data-stu-id="851ac-314">It's useful for large file uploads and want to prevent the form from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="851ac-315">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-315">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="851ac-316">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="851ac-316">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="851ac-317">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="851ac-317">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="851ac-318">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="851ac-318">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="851ac-319">*動作篩選條件*：</span><span class="sxs-lookup"><span data-stu-id="851ac-319">*Action filters*:</span></span>

* <span data-ttu-id="851ac-320">實作 `IActionFilter` 或 `IAsyncActionFilter` 介面。</span><span class="sxs-lookup"><span data-stu-id="851ac-320">Implement either the `IActionFilter` or `IAsyncActionFilter` interface.</span></span>
* <span data-ttu-id="851ac-321">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-321">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="851ac-322">以下是動作篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="851ac-322">Here's a sample action filter:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="851ac-323"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="851ac-323">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="851ac-324">`ActionArguments` - 可讓您管理動作的輸入。</span><span class="sxs-lookup"><span data-stu-id="851ac-324">`ActionArguments` - lets you manipulate the inputs to the action.</span></span>
* <span data-ttu-id="851ac-325">`Controller` - 可讓您管理控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="851ac-325">`Controller` - lets you manipulate the controller instance.</span></span> 
* <span data-ttu-id="851ac-326">`Result` - 設定動作方法和後續動作篩選條件的縮短執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-326">`Result` - setting this short-circuits execution of the action method and subsequent action filters.</span></span> <span data-ttu-id="851ac-327">擲回例外狀況也會導致無法執行動作方法和後續的篩選條件，但會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-327">Throwing an exception also prevents execution of the action method and subsequent filters, but is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="851ac-328"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="851ac-328">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="851ac-329">`Canceled` - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="851ac-329">`Canceled` - will be true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="851ac-330">`Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="851ac-330">`Exception` - will be non-null if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="851ac-331">將這個屬性設為 Null，實際上會「處理」例外狀況，並且會執行 `Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="851ac-331">Setting this property to null effectively 'handles' an exception, and `Result` will be executed as if it were returned from the action method normally.</span></span>

<span data-ttu-id="851ac-332">對於 `IAsyncActionFilter`，呼叫 `ActionExecutionDelegate` 會：</span><span class="sxs-lookup"><span data-stu-id="851ac-332">For an `IAsyncActionFilter`, a call to the `ActionExecutionDelegate`:</span></span>

* <span data-ttu-id="851ac-333">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-333">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="851ac-334">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="851ac-334">returns `ActionExecutedContext`.</span></span> 

<span data-ttu-id="851ac-335">若要縮短，請指派 `ActionExecutingContext.Result` 給某個結果執行個體，並且不要呼叫 `ActionExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="851ac-335">To short-circuit, assign `ActionExecutingContext.Result` to some result instance and don't call the `ActionExecutionDelegate`.</span></span>

<span data-ttu-id="851ac-336">架構提供一個抽象 `ActionFilterAttribute`，您可以進行子分類。</span><span class="sxs-lookup"><span data-stu-id="851ac-336">The framework provides an abstract `ActionFilterAttribute` that you can subclass.</span></span> 

<span data-ttu-id="851ac-337">您可以使用動作篩選條件來驗證模型狀態，並在狀態無效時傳回任何錯誤：</span><span class="sxs-lookup"><span data-stu-id="851ac-337">You can use an action filter to validate model state and return any errors if the state is invalid:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

<span data-ttu-id="851ac-338">`OnActionExecuted` 方法在動作方法之後執行，且可以透過 `ActionExecutedContext.Result` 屬性看到及管理動作的結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-338">The `OnActionExecuted` method runs after the action method and can see and manipulate the results of the action through the `ActionExecutedContext.Result` property.</span></span> <span data-ttu-id="851ac-339">`ActionExecutedContext.Canceled` - 如果動作執行被另一個篩選條件縮短，則將設為 true。</span><span class="sxs-lookup"><span data-stu-id="851ac-339">`ActionExecutedContext.Canceled` will be set to true if the action execution was short-circuited by another filter.</span></span> <span data-ttu-id="851ac-340">`ActionExecutedContext.Exception` - 如果動作或後續的動作篩選條件擲回例外狀況，則將設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="851ac-340">`ActionExecutedContext.Exception` will be set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="851ac-341">將 `ActionExecutedContext.Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="851ac-341">Setting `ActionExecutedContext.Exception` to null:</span></span>

* <span data-ttu-id="851ac-342">實際「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-342">Effectively 'handles' an exception.</span></span>
* <span data-ttu-id="851ac-343">執行 `ActionExectedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="851ac-343">`ActionExectedContext.Result` is executed as if it were returned normally from the action method.</span></span>

## <a name="exception-filters"></a><span data-ttu-id="851ac-344">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-344">Exception filters</span></span>

<span data-ttu-id="851ac-345">「例外狀況篩選條件」會實作 `IExceptionFilter` 或 `IAsyncExceptionFilter` 介面。</span><span class="sxs-lookup"><span data-stu-id="851ac-345">*Exception filters* implement either the `IExceptionFilter` or `IAsyncExceptionFilter` interface.</span></span> <span data-ttu-id="851ac-346">它們可以用來實作常見的應用程式錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="851ac-346">They can be used to implement common error handling policies for an app.</span></span> 

<span data-ttu-id="851ac-347">下列範例例外狀況篩選條件會使用自訂的開發人員檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="851ac-347">The following sample exception filter uses a custom developer error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

<span data-ttu-id="851ac-348">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="851ac-348">Exception filters:</span></span>

* <span data-ttu-id="851ac-349">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="851ac-349">Don't have before and after events.</span></span> 
* <span data-ttu-id="851ac-350">實作 `OnException` 或 `OnExceptionAsync`。</span><span class="sxs-lookup"><span data-stu-id="851ac-350">Implement `OnException` or `OnExceptionAsync`.</span></span> 
* <span data-ttu-id="851ac-351">處理在控制器建立、[模型繫結](../models/model-binding.md)、動作篩選條件或動作方法發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-351">Handle unhandled exceptions that occur in controller creation, [model binding](../models/model-binding.md), action filters, or action methods.</span></span> 
* <span data-ttu-id="851ac-352">不會攔截在資源篩選條件、結果篩選條件或 MVC 結果執行發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-352">Do not catch exceptions that occur in Resource filters, Result filters, or MVC Result execution.</span></span>

<span data-ttu-id="851ac-353">若要處理例外狀況，請將 `ExceptionContext.ExceptionHandled` 屬性設為 true，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="851ac-353">To handle an exception, set the `ExceptionContext.ExceptionHandled` property to true or write a response.</span></span> <span data-ttu-id="851ac-354">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-354">This stops propagation of the exception.</span></span> <span data-ttu-id="851ac-355">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="851ac-355">An Exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="851ac-356">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="851ac-356">Only an Action filter can do that.</span></span>

> [!NOTE]
> <span data-ttu-id="851ac-357">在 ASP.NET Core 1.1 中，如果您將 `ExceptionHandled` 設為 true，**並且**撰寫回應，則不會傳送回應。</span><span class="sxs-lookup"><span data-stu-id="851ac-357">In ASP.NET Core 1.1, the response isn't sent if you set `ExceptionHandled` to true **and** write a response.</span></span> <span data-ttu-id="851ac-358">在該情況中，ASP.NET Core 1.0 會傳送回應，而 ASP.NET Core 1.1.2 則會回到 1.0 的行為。</span><span class="sxs-lookup"><span data-stu-id="851ac-358">In that scenario, ASP.NET Core 1.0 does send the response, and ASP.NET Core 1.1.2 will return to the 1.0 behavior.</span></span> <span data-ttu-id="851ac-359">如需詳細資訊，請參閱 GitHub 儲存機制中的[問題 #5594](https://github.com/aspnet/Mvc/issues/5594)。</span><span class="sxs-lookup"><span data-stu-id="851ac-359">For more information, see [issue #5594](https://github.com/aspnet/Mvc/issues/5594) in the GitHub repository.</span></span> 

<span data-ttu-id="851ac-360">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="851ac-360">Exception filters:</span></span>

* <span data-ttu-id="851ac-361">適合用於設陷 MVC 動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-361">Are good for trapping exceptions that occur within MVC actions.</span></span>
* <span data-ttu-id="851ac-362">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="851ac-362">Are not as flexible as error handling middleware.</span></span> 

<span data-ttu-id="851ac-363">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="851ac-363">Prefer middleware for exception handling.</span></span> <span data-ttu-id="851ac-364">只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-364">Use exception filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span> <span data-ttu-id="851ac-365">比方說，您的應用程式可能會有 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="851ac-365">For example, your app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="851ac-366">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="851ac-366">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

<span data-ttu-id="851ac-367">`ExceptionFilterAttribute` 可以子類別化。</span><span class="sxs-lookup"><span data-stu-id="851ac-367">The `ExceptionFilterAttribute` can be subclassed.</span></span> 

## <a name="result-filters"></a><span data-ttu-id="851ac-368">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="851ac-368">Result filters</span></span>

* <span data-ttu-id="851ac-369">實作介面：</span><span class="sxs-lookup"><span data-stu-id="851ac-369">Implement an interface:</span></span>
  * <span data-ttu-id="851ac-370">`IResultFilter` 或 `IAsyncResultFilter`。</span><span class="sxs-lookup"><span data-stu-id="851ac-370">`IResultFilter` or `IAsyncResultFilter`.</span></span>
  * <span data-ttu-id="851ac-371">`IAlwaysRunResultFilter` 或 `IAsyncAlwaysRunResultFilter`</span><span class="sxs-lookup"><span data-stu-id="851ac-371">`IAlwaysRunResultFilter` or `IAsyncAlwaysRunResultFilter`</span></span>
* <span data-ttu-id="851ac-372">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-372">Their execution surrounds the execution of action results.</span></span> 

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="851ac-373">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="851ac-373">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="851ac-374">以下是結果篩選條件的範例，它會新增 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="851ac-374">Here's an example of a Result filter that adds an HTTP header.</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="851ac-375">正在執行的結果類型取決於討論中的動作。</span><span class="sxs-lookup"><span data-stu-id="851ac-375">The kind of result being executed depends on the action in question.</span></span> <span data-ttu-id="851ac-376">傳回檢視的 MVC 動作會在執行中的 `ViewResult` 裡包含處理中的所有 Razor。</span><span class="sxs-lookup"><span data-stu-id="851ac-376">An MVC action returning a view would include all razor processing as part of the `ViewResult` being executed.</span></span> <span data-ttu-id="851ac-377">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="851ac-377">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="851ac-378">深入了解[動作結果](actions.md)</span><span class="sxs-lookup"><span data-stu-id="851ac-378">Learn more about [action results](actions.md)</span></span>

<span data-ttu-id="851ac-379">動作篩選條件只會針對成功的結果執行 - 動作或動作篩選條件會執行動作結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-379">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="851ac-380">當例外狀況篩選條件處理例外狀況時，不會執行結果篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-380">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="851ac-381">藉由將 `ResultExecutingContext.Cancel` 設為 true，`OnResultExecuting` 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-381">The `OnResultExecuting` method can short-circuit execution of the action result and subsequent result filters by setting `ResultExecutingContext.Cancel` to true.</span></span> <span data-ttu-id="851ac-382">在縮短時您通常應該寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="851ac-382">You should generally write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="851ac-383">擲回例外狀況會：</span><span class="sxs-lookup"><span data-stu-id="851ac-383">Throwing an exception will:</span></span>

* <span data-ttu-id="851ac-384">導致無法執行動作結果和後續的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="851ac-384">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="851ac-385">視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-385">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="851ac-386">當 `OnResultExecuted` 方法執行時，回應可能傳送至用戶端，且無法進一步變更 (除非擲回例外狀況)。</span><span class="sxs-lookup"><span data-stu-id="851ac-386">When the `OnResultExecuted` method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).</span></span> <span data-ttu-id="851ac-387">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 將設為 true。</span><span class="sxs-lookup"><span data-stu-id="851ac-387">`ResultExecutedContext.Canceled` will be set to true if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="851ac-388">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 將設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="851ac-388">`ResultExecutedContext.Exception` will be set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="851ac-389">將 `Exception` 設為 Null，實際上會「處理」例外狀況，並且使管線中稍後的 MVC 不會重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="851ac-389">Setting `Exception` to null effectively 'handles' an exception and prevents the exception from being rethrown by MVC later in the pipeline.</span></span> <span data-ttu-id="851ac-390">當您處理結果篩選條件的例外狀況時，您可能無法將任何資料寫入至回應。</span><span class="sxs-lookup"><span data-stu-id="851ac-390">When you're handling an exception in a result filter, you might not be able to write any data to the response.</span></span> <span data-ttu-id="851ac-391">如果動作結果在執行中途擲回，且標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="851ac-391">If the action result throws partway through its execution, and the headers have already been flushed to the client, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="851ac-392">針對 `IAsyncResultFilter`，對 `ResultExecutionDelegate` 呼叫 `await next` 會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="851ac-392">For an `IAsyncResultFilter` a call to `await next` on the `ResultExecutionDelegate` executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="851ac-393">若要縮短，請將 `ResultExecutingContext.Cancel` 設為 true，且不要呼叫 `ResultExectionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="851ac-393">To short-circuit, set `ResultExecutingContext.Cancel` to true and don't call the `ResultExectionDelegate`.</span></span>

<span data-ttu-id="851ac-394">架構提供一個抽象 `ResultFilterAttribute`，您可以進行子分類。</span><span class="sxs-lookup"><span data-stu-id="851ac-394">The framework provides an abstract `ResultFilterAttribute` that you can subclass.</span></span> <span data-ttu-id="851ac-395">稍早所示的 [AddHeaderAttribute](#add-header-attribute)類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="851ac-395">The [AddHeaderAttribute](#add-header-attribute) class shown earlier is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="851ac-396">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="851ac-396">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="851ac-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="851ac-397">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for action results.</span></span> <span data-ttu-id="851ac-398">篩選會套用至動作結果，除非 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> 發生作用而使回應發生短路。</span><span class="sxs-lookup"><span data-stu-id="851ac-398">The filter is applied to an action result unless an <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>

<span data-ttu-id="851ac-399">換句話說，這些「一律執行」的篩選一律會執行，除非例外狀況或授權篩選使它們發生短路。</span><span class="sxs-lookup"><span data-stu-id="851ac-399">In other words, these "always run" filters, always run, except when an exception or authorization filter short-circuits them.</span></span> <span data-ttu-id="851ac-400">`IExceptionFilter` 和 `IAuthorizationFilter` 以外的篩選不會使它們發生短路。</span><span class="sxs-lookup"><span data-stu-id="851ac-400">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short circuit them.</span></span>

<span data-ttu-id="851ac-401">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="851ac-401">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

```csharp
public class UnprocessableResultFilter : Attribute, IAlwaysRunResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
    {
        if (context.Result is StatusCodeResult statusCodeResult &&
            statusCodeResult.StatusCode == 415)
        {
            context.Result = new ObjectResult("Can't process this!")
            {
                StatusCode = 422,
            };
        }
    }

    public void OnResultExecuted(ResultExecutedContext context)
    {
    }
}
```

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="851ac-402">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="851ac-402">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="851ac-403">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="851ac-403">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="851ac-404">但篩選條件與中介軟體不同之處在於，它們是 MVC 的一部分，這表示它們能存取 MVC 內容和建構。</span><span class="sxs-lookup"><span data-stu-id="851ac-404">But filters differ from middleware in that they're part of MVC, which means that they have access to MVC context and constructs.</span></span>

<span data-ttu-id="851ac-405">在 ASP.NET Core 1.1 中，您可以在篩選條件管線中使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="851ac-405">In ASP.NET Core 1.1, you can use middleware in the filter pipeline.</span></span> <span data-ttu-id="851ac-406">如果您有需要存取 MVC 路由資料的中介軟體元件，或只應該針對特定控制器或動作執行的中介軟體元件，則可能會想要這樣做。</span><span class="sxs-lookup"><span data-stu-id="851ac-406">You might want to do that if you have a middleware component that needs access to MVC route data, or one that should run only for certain controllers or actions.</span></span>

<span data-ttu-id="851ac-407">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，指定您要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="851ac-407">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware that you want to inject into the filter pipeline.</span></span> <span data-ttu-id="851ac-408">以下是使用當地語系化中介軟體，建立要求之目前文化特性的範例：</span><span class="sxs-lookup"><span data-stu-id="851ac-408">Here's an example that uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="851ac-409">然後您可以使用 `MiddlewareFilterAttribute` 針對選取的控制器或動作執行中介軟體，或是全域地執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="851ac-409">You can then use the `MiddlewareFilterAttribute` to run the middleware for a selected controller or action or globally:</span></span>

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="851ac-410">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="851ac-410">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="851ac-411">後續動作</span><span class="sxs-lookup"><span data-stu-id="851ac-411">Next actions</span></span>

* <span data-ttu-id="851ac-412">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="851ac-412">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="851ac-413">若要嘗試使用篩選條件，請[下載、測試及修改 Github 範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。</span><span class="sxs-lookup"><span data-stu-id="851ac-413">To experiment with filters, [download, test and modify the Github sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
