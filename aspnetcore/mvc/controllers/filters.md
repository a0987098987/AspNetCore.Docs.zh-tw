---
title: ASP.NET Core 中的篩選條件
author: ardalis
description: 了解篩選條件的運作方式，以及如何在 ASP.NET Core 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 6a83b8e85b68a9b8796aeed2fd39108dbeed3266
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190530"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="dcc50-103">ASP.NET Core 中的篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="dcc50-104">作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcc50-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="dcc50-105">ASP.NET Core 中的「篩選條件」可讓程式碼在要求處理管線中的特定階段之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="dcc50-106">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="dcc50-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="dcc50-107">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="dcc50-108">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="dcc50-109">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="dcc50-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="dcc50-110">跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。</span><span class="sxs-lookup"><span data-stu-id="dcc50-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="dcc50-111">篩選能避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="dcc50-112">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="dcc50-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="dcc50-113">本文件適用於 Razor Pages、API 控制器，以及具有檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="dcc50-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="dcc50-114">[檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="dcc50-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="dcc50-115">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="dcc50-115">How filters work</span></span>

<span data-ttu-id="dcc50-116">篩選條件會在「ASP.NET Core 動作引動過程管線」中執行，其有時也被稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="dcc50-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="dcc50-117">在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="dcc50-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 ASP.NET Core 動作引動過程管線。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="dcc50-120">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="dcc50-120">Filter types</span></span>

<span data-ttu-id="dcc50-121">每個篩選類型會在篩選條件管線中的不同階段執行：</span><span class="sxs-lookup"><span data-stu-id="dcc50-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="dcc50-122">[授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。</span><span class="sxs-lookup"><span data-stu-id="dcc50-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="dcc50-123">如果要求未經授權，授權篩選條件便會縮短管線。</span><span class="sxs-lookup"><span data-stu-id="dcc50-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="dcc50-124">[資源篩選條件](#resource-filters)：</span><span class="sxs-lookup"><span data-stu-id="dcc50-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="dcc50-125">會在授權之後執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-125">Run after authorization.</span></span>  
  * <span data-ttu-id="dcc50-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 可以在其餘的篩選條件管線之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="dcc50-127">例如，`OnResourceExecuting` 可以在模型繫結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="dcc50-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 可以在其餘的管線皆已完成之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="dcc50-129">[動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="dcc50-130">它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="dcc50-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="dcc50-131">Razor Pages 中**不**支援動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="dcc50-132">[例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="dcc50-133">[結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="dcc50-134">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="dcc50-135">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="dcc50-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="dcc50-136">下圖顯示篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="dcc50-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="dcc50-139">實作</span><span class="sxs-lookup"><span data-stu-id="dcc50-139">Implementation</span></span>

<span data-ttu-id="dcc50-140">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="dcc50-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="dcc50-141">同步篩選條件可以在其管線階段之前 (`On-Stage-Executing`) 和之後 (`On-Stage-Executed`) 執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="dcc50-142">例如，系統會在呼叫動作方法之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="dcc50-143">系統會在傳回動作方法之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="dcc50-144">非同步篩選條件會定義 `On-Stage-ExecutionAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="dcc50-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="dcc50-145">在上述程式碼中，`SampleAsyncActionFilter` 具有會執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="dcc50-146">每個 `On-Stage-ExecutionAsync` 都會接受能執行篩選條件之管線階段的 `FilterType-ExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="dcc50-147">多個篩選條件階段</span><span class="sxs-lookup"><span data-stu-id="dcc50-147">Multiple filter stages</span></span>

<span data-ttu-id="dcc50-148">可以在單一類別中實作多個篩選條件階段的介面。</span><span class="sxs-lookup"><span data-stu-id="dcc50-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="dcc50-149">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。</span><span class="sxs-lookup"><span data-stu-id="dcc50-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="dcc50-150">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="dcc50-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="dcc50-151">執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="dcc50-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="dcc50-152">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="dcc50-153">如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="dcc50-154">使用抽象類別 (例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>) 時，請僅覆寫每個篩選條件類型的同步方法或非同步方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="dcc50-155">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="dcc50-155">Built-in filter attributes</span></span>

<span data-ttu-id="dcc50-156">ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。</span><span class="sxs-lookup"><span data-stu-id="dcc50-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="dcc50-157">例如，下列結果篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="dcc50-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="dcc50-158">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="dcc50-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="dcc50-159">將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="dcc50-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="dcc50-160">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="dcc50-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="dcc50-161">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="dcc50-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="dcc50-162">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="dcc50-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="dcc50-163">篩選條件能以三個「範圍」之一新增至管線：</span><span class="sxs-lookup"><span data-stu-id="dcc50-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="dcc50-164">針對動作使用屬性。</span><span class="sxs-lookup"><span data-stu-id="dcc50-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="dcc50-165">針對控制器使用屬性。</span><span class="sxs-lookup"><span data-stu-id="dcc50-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="dcc50-166">全域地針對所有控制器和動作，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="dcc50-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="dcc50-167">上述程式碼會使用 [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) 集合來全域地新增三個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="dcc50-168">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="dcc50-168">Default order of execution</span></span>

<span data-ttu-id="dcc50-169">當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。</span><span class="sxs-lookup"><span data-stu-id="dcc50-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="dcc50-170">全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="dcc50-171">因為篩選條件巢狀結構的原因，篩選條件的「之後」程式碼的執行順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="dcc50-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="dcc50-172">篩選條件序列：</span><span class="sxs-lookup"><span data-stu-id="dcc50-172">The filter sequence:</span></span>

* <span data-ttu-id="dcc50-173">全域篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="dcc50-174">控制器篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="dcc50-175">動作方法篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="dcc50-176">動作方法篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="dcc50-177">控制器篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="dcc50-178">全域篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="dcc50-179">下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。</span><span class="sxs-lookup"><span data-stu-id="dcc50-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="dcc50-180">序列</span><span class="sxs-lookup"><span data-stu-id="dcc50-180">Sequence</span></span> | <span data-ttu-id="dcc50-181">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="dcc50-181">Filter scope</span></span> | <span data-ttu-id="dcc50-182">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="dcc50-183">1</span><span class="sxs-lookup"><span data-stu-id="dcc50-183">1</span></span> | <span data-ttu-id="dcc50-184">Global</span><span class="sxs-lookup"><span data-stu-id="dcc50-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="dcc50-185">2</span><span class="sxs-lookup"><span data-stu-id="dcc50-185">2</span></span> | <span data-ttu-id="dcc50-186">控制器</span><span class="sxs-lookup"><span data-stu-id="dcc50-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="dcc50-187">3</span><span class="sxs-lookup"><span data-stu-id="dcc50-187">3</span></span> | <span data-ttu-id="dcc50-188">方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="dcc50-189">4</span><span class="sxs-lookup"><span data-stu-id="dcc50-189">4</span></span> | <span data-ttu-id="dcc50-190">方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="dcc50-191">5</span><span class="sxs-lookup"><span data-stu-id="dcc50-191">5</span></span> | <span data-ttu-id="dcc50-192">控制器</span><span class="sxs-lookup"><span data-stu-id="dcc50-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="dcc50-193">6</span><span class="sxs-lookup"><span data-stu-id="dcc50-193">6</span></span> | <span data-ttu-id="dcc50-194">Global</span><span class="sxs-lookup"><span data-stu-id="dcc50-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="dcc50-195">此順序顯示：</span><span class="sxs-lookup"><span data-stu-id="dcc50-195">This sequence shows:</span></span>

* <span data-ttu-id="dcc50-196">方法篩選條件巢狀位於控制器篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="dcc50-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="dcc50-197">控制器篩選條件巢狀位於全域篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="dcc50-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="dcc50-198">控制器和 Razor 頁面層級篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="dcc50-199">繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="dcc50-200">這些方法會：</span><span class="sxs-lookup"><span data-stu-id="dcc50-200">These methods:</span></span>

* <span data-ttu-id="dcc50-201">包裝針對指定動作執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="dcc50-202">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="dcc50-203">系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="dcc50-204">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="dcc50-205">在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="dcc50-206">例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。</span><span class="sxs-lookup"><span data-stu-id="dcc50-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="dcc50-207">`TestController`：</span><span class="sxs-lookup"><span data-stu-id="dcc50-207">The `TestController`:</span></span>

* <span data-ttu-id="dcc50-208">將 `SampleActionFilterAttribute` （`[SampleActionFilter]`）套用至 `FilterTest2` 動作。</span><span class="sxs-lookup"><span data-stu-id="dcc50-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="dcc50-209">覆寫 `OnActionExecuting` 和 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="dcc50-210">瀏覽至 `https://localhost:5001/Test/FilterTest2` 執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="dcc50-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="dcc50-211">針對 Razor Pages，請參閱[覆寫篩選條件方法來實作 Razor 頁面篩選條件](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="dcc50-212">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="dcc50-212">Overriding the default order</span></span>

<span data-ttu-id="dcc50-213">可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="dcc50-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="dcc50-214">`IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="dcc50-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="dcc50-215">具有較低 `Order` 值的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="dcc50-216">在具有較高 `Order` 值的篩選條件之前執行「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="dcc50-217">在具有較高 `Order` 值的篩選條件之後執行「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="dcc50-218">`Order` 屬性可以搭配建構函式參數設定：</span><span class="sxs-lookup"><span data-stu-id="dcc50-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="dcc50-219">請考慮先前範例所示的同樣 3 個動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="dcc50-220">如果控制器和全域篩選條件的 `Order` 屬性被個別設定為 1 和 2，執行順序將會反轉。</span><span class="sxs-lookup"><span data-stu-id="dcc50-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="dcc50-221">序列</span><span class="sxs-lookup"><span data-stu-id="dcc50-221">Sequence</span></span> | <span data-ttu-id="dcc50-222">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="dcc50-222">Filter scope</span></span> | <span data-ttu-id="dcc50-223">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="dcc50-223">`Order` property</span></span> | <span data-ttu-id="dcc50-224">篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="dcc50-225">1</span><span class="sxs-lookup"><span data-stu-id="dcc50-225">1</span></span> | <span data-ttu-id="dcc50-226">方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-226">Method</span></span> | <span data-ttu-id="dcc50-227">0</span><span class="sxs-lookup"><span data-stu-id="dcc50-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="dcc50-228">2</span><span class="sxs-lookup"><span data-stu-id="dcc50-228">2</span></span> | <span data-ttu-id="dcc50-229">控制器</span><span class="sxs-lookup"><span data-stu-id="dcc50-229">Controller</span></span> | <span data-ttu-id="dcc50-230">1</span><span class="sxs-lookup"><span data-stu-id="dcc50-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="dcc50-231">3</span><span class="sxs-lookup"><span data-stu-id="dcc50-231">3</span></span> | <span data-ttu-id="dcc50-232">Global</span><span class="sxs-lookup"><span data-stu-id="dcc50-232">Global</span></span> | <span data-ttu-id="dcc50-233">2</span><span class="sxs-lookup"><span data-stu-id="dcc50-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="dcc50-234">4</span><span class="sxs-lookup"><span data-stu-id="dcc50-234">4</span></span> | <span data-ttu-id="dcc50-235">Global</span><span class="sxs-lookup"><span data-stu-id="dcc50-235">Global</span></span> | <span data-ttu-id="dcc50-236">2</span><span class="sxs-lookup"><span data-stu-id="dcc50-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="dcc50-237">5</span><span class="sxs-lookup"><span data-stu-id="dcc50-237">5</span></span> | <span data-ttu-id="dcc50-238">控制器</span><span class="sxs-lookup"><span data-stu-id="dcc50-238">Controller</span></span> | <span data-ttu-id="dcc50-239">1</span><span class="sxs-lookup"><span data-stu-id="dcc50-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="dcc50-240">6</span><span class="sxs-lookup"><span data-stu-id="dcc50-240">6</span></span> | <span data-ttu-id="dcc50-241">方法</span><span class="sxs-lookup"><span data-stu-id="dcc50-241">Method</span></span> | <span data-ttu-id="dcc50-242">0</span><span class="sxs-lookup"><span data-stu-id="dcc50-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="dcc50-243">`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。</span><span class="sxs-lookup"><span data-stu-id="dcc50-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="dcc50-244">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="dcc50-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="dcc50-245">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="dcc50-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="dcc50-246">針對內建篩選條件，除非將 `Order` 設定為非零值，否則範圍會決定順序。</span><span class="sxs-lookup"><span data-stu-id="dcc50-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="dcc50-247">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="dcc50-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="dcc50-248">可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 屬性，以縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="dcc50-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="dcc50-249">比方說，下列資源篩選條件可防止管線的其餘部分執行：</span><span class="sxs-lookup"><span data-stu-id="dcc50-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="dcc50-250">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="dcc50-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="dcc50-251">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="dcc50-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="dcc50-252">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="dcc50-253">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="dcc50-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="dcc50-254">因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="dcc50-255">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="dcc50-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="dcc50-256">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="dcc50-257">相依性插入</span><span class="sxs-lookup"><span data-stu-id="dcc50-257">Dependency injection</span></span>

<span data-ttu-id="dcc50-258">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="dcc50-259">如果新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="dcc50-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="dcc50-260">如果新增類型，其將會由類型啟動。</span><span class="sxs-lookup"><span data-stu-id="dcc50-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="dcc50-261">由類型啟動的篩選條件表示：</span><span class="sxs-lookup"><span data-stu-id="dcc50-261">A type-activated filter means:</span></span>

* <span data-ttu-id="dcc50-262">系統會針對每個要求建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-262">An instance is created for each request.</span></span>
* <span data-ttu-id="dcc50-263">任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。</span><span class="sxs-lookup"><span data-stu-id="dcc50-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="dcc50-264">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="dcc50-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="dcc50-265">DI 無法提供建構函式相依性，因為：</span><span class="sxs-lookup"><span data-stu-id="dcc50-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="dcc50-266">必須在提供屬性之建構函式參數的地方提供它們。</span><span class="sxs-lookup"><span data-stu-id="dcc50-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="dcc50-267">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="dcc50-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="dcc50-268">下列篩選條件支援由 DI 所提供的建構函式相依性：</span><span class="sxs-lookup"><span data-stu-id="dcc50-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="dcc50-269">在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="dcc50-270">上述篩選條件可以被套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="dcc50-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="dcc50-271">DI 會提供記錄器。</span><span class="sxs-lookup"><span data-stu-id="dcc50-271">Loggers are available from DI.</span></span> <span data-ttu-id="dcc50-272">不過，請避免僅針對記錄目的建立並使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="dcc50-273">[內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。</span><span class="sxs-lookup"><span data-stu-id="dcc50-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="dcc50-274">新增至篩選條件的記錄：</span><span class="sxs-lookup"><span data-stu-id="dcc50-274">Logging added to filters:</span></span>

* <span data-ttu-id="dcc50-275">應該專注在篩選條件特定的商務領域考量或行為。</span><span class="sxs-lookup"><span data-stu-id="dcc50-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="dcc50-276">**不**應該記錄動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="dcc50-277">內建篩選條件能記錄動作和架構事件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="dcc50-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="dcc50-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="dcc50-279">服務篩選條件實作類型會註冊在 `ConfigureServices` 中。</span><span class="sxs-lookup"><span data-stu-id="dcc50-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="dcc50-280"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="dcc50-281">下面程式碼會說明 `AddHeaderResultServiceFilter`：</span><span class="sxs-lookup"><span data-stu-id="dcc50-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="dcc50-282">在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="dcc50-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="dcc50-283">在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：</span><span class="sxs-lookup"><span data-stu-id="dcc50-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="dcc50-284">使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="dcc50-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="dcc50-285">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="dcc50-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="dcc50-286">ASP.NET Core 執行階段並不保證：</span><span class="sxs-lookup"><span data-stu-id="dcc50-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="dcc50-287">將會建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="dcc50-288">將不會於稍後的時間從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="dcc50-289">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="dcc50-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="dcc50-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="dcc50-291">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="dcc50-292">`CreateInstance` 會從 DI 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="dcc50-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="dcc50-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="dcc50-293">TypeFilterAttribute</span></span>

<span data-ttu-id="dcc50-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="dcc50-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="dcc50-295">它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="dcc50-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="dcc50-296">由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：</span><span class="sxs-lookup"><span data-stu-id="dcc50-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="dcc50-297">使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="dcc50-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="dcc50-298">不過它們的相依性會由 DI 容器滿足。</span><span class="sxs-lookup"><span data-stu-id="dcc50-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="dcc50-299">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="dcc50-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="dcc50-300">使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="dcc50-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="dcc50-301">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="dcc50-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="dcc50-302">ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="dcc50-303">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="dcc50-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="dcc50-304">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="dcc50-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="dcc50-305">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-305">Authorization filters</span></span>

<span data-ttu-id="dcc50-306">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-306">Authorization filters:</span></span>

* <span data-ttu-id="dcc50-307">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="dcc50-308">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="dcc50-308">Control access to action methods.</span></span>
* <span data-ttu-id="dcc50-309">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="dcc50-310">自訂授權篩選條件需要自訂的授權架構。</span><span class="sxs-lookup"><span data-stu-id="dcc50-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="dcc50-311">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="dcc50-312">內建的授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="dcc50-313">呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="dcc50-313">Calls the authorization system.</span></span>
* <span data-ttu-id="dcc50-314">不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="dcc50-314">Does not authorize requests.</span></span>

<span data-ttu-id="dcc50-315">**不會**在授權篩選條件內擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="dcc50-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="dcc50-316">該例外狀況將不會被處理。</span><span class="sxs-lookup"><span data-stu-id="dcc50-316">The exception will not be handled.</span></span>
* <span data-ttu-id="dcc50-317">例外狀況篩選條件將不會處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="dcc50-318">請考慮在例外狀況於授權篩選條件中發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="dcc50-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="dcc50-319">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="dcc50-320">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-320">Resource filters</span></span>

<span data-ttu-id="dcc50-321">資源篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-321">Resource filters:</span></span>

* <span data-ttu-id="dcc50-322">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="dcc50-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="dcc50-323">執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="dcc50-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="dcc50-324">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="dcc50-325">資源篩選條件很適合用來縮短大部分的管線。</span><span class="sxs-lookup"><span data-stu-id="dcc50-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="dcc50-326">比方說，快取篩選條件可以避免快取命中上的其餘管線。</span><span class="sxs-lookup"><span data-stu-id="dcc50-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="dcc50-327">資源篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="dcc50-327">Resource filter examples:</span></span>

* <span data-ttu-id="dcc50-328">先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="dcc50-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：</span><span class="sxs-lookup"><span data-stu-id="dcc50-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="dcc50-330">防止模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="dcc50-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="dcc50-331">用於大型檔案上傳，以避免將表單資料讀入記憶體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="dcc50-332">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcc50-333">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="dcc50-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="dcc50-334">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="dcc50-335">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="dcc50-336">動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-336">Action filters:</span></span>

* <span data-ttu-id="dcc50-337">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="dcc50-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="dcc50-338">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="dcc50-339">下列程式碼會顯示範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="dcc50-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="dcc50-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="dcc50-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - 讓針對某個動作方法的輸入可以被讀取。</span><span class="sxs-lookup"><span data-stu-id="dcc50-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="dcc50-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。</span><span class="sxs-lookup"><span data-stu-id="dcc50-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="dcc50-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="dcc50-344">在動作方法中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="dcc50-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="dcc50-345">防止執行後續篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="dcc50-346">和設定 `Result` 不同，會被視為失敗而非成功結果。</span><span class="sxs-lookup"><span data-stu-id="dcc50-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="dcc50-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="dcc50-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="dcc50-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="dcc50-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="dcc50-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="dcc50-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="dcc50-350">將此屬性設定為 Null：</span><span class="sxs-lookup"><span data-stu-id="dcc50-350">Setting this property to null:</span></span>

  * <span data-ttu-id="dcc50-351">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="dcc50-352">會執行 `Result`，有如它是從動作方法傳回一般。</span><span class="sxs-lookup"><span data-stu-id="dcc50-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="dcc50-353">對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：</span><span class="sxs-lookup"><span data-stu-id="dcc50-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="dcc50-354">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="dcc50-355">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="dcc50-356">若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="dcc50-357">架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="dcc50-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="dcc50-358">`OnActionExecuting` 動作篩選條件可以用來：</span><span class="sxs-lookup"><span data-stu-id="dcc50-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="dcc50-359">驗證模型狀態。</span><span class="sxs-lookup"><span data-stu-id="dcc50-359">Validate model state.</span></span>
* <span data-ttu-id="dcc50-360">如果狀態無效，則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="dcc50-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="dcc50-361">`OnActionExecuted` 方法會在動作方法之後執行：</span><span class="sxs-lookup"><span data-stu-id="dcc50-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="dcc50-362">並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。</span><span class="sxs-lookup"><span data-stu-id="dcc50-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="dcc50-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。</span><span class="sxs-lookup"><span data-stu-id="dcc50-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="dcc50-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="dcc50-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="dcc50-365">將 `Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="dcc50-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="dcc50-366">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="dcc50-367">執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="dcc50-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="dcc50-368">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-368">Exception filters</span></span>

<span data-ttu-id="dcc50-369">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-369">Exception filters:</span></span>

* <span data-ttu-id="dcc50-370">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="dcc50-371">可以用來實作常見的錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="dcc50-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="dcc50-372">下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="dcc50-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="dcc50-373">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-373">Exception filters:</span></span>

* <span data-ttu-id="dcc50-374">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-374">Don't have before and after events.</span></span>
* <span data-ttu-id="dcc50-375">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="dcc50-376">處理在 Razor 頁面或控制器建立、[模型繫結](xref:mvc/models/model-binding)、動作篩選條件或動作方法中發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="dcc50-377">**不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="dcc50-378">若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="dcc50-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="dcc50-379">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-379">This stops propagation of the exception.</span></span> <span data-ttu-id="dcc50-380">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="dcc50-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="dcc50-381">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="dcc50-381">Only an action filter can do that.</span></span>

<span data-ttu-id="dcc50-382">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-382">Exception filters:</span></span>

* <span data-ttu-id="dcc50-383">適合用於設陷在動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="dcc50-384">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="dcc50-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="dcc50-385">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="dcc50-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="dcc50-386">只有在錯誤處理會根據所呼叫的動作方法而「不同」時才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="dcc50-387">比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="dcc50-388">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="dcc50-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="dcc50-389">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="dcc50-389">Result filters</span></span>

<span data-ttu-id="dcc50-390">結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-390">Result filters:</span></span>

* <span data-ttu-id="dcc50-391">實作介面：</span><span class="sxs-lookup"><span data-stu-id="dcc50-391">Implement an interface:</span></span>
  * <span data-ttu-id="dcc50-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="dcc50-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="dcc50-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="dcc50-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="dcc50-394">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="dcc50-395">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="dcc50-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="dcc50-396">以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="dcc50-397">執行的結果類型會取決於動作。</span><span class="sxs-lookup"><span data-stu-id="dcc50-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="dcc50-398">傳回檢視的動作會在執行中的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 裡包含處理中的所有 Razor。</span><span class="sxs-lookup"><span data-stu-id="dcc50-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="dcc50-399">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="dcc50-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="dcc50-400">深入瞭解[動作結果](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-400">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="dcc50-401">只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。</span><span class="sxs-lookup"><span data-stu-id="dcc50-401">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="dcc50-402">當下列情況時，不會執行結果篩選：</span><span class="sxs-lookup"><span data-stu-id="dcc50-402">Result filters are not executed when:</span></span>

* <span data-ttu-id="dcc50-403">授權篩選或資源篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="dcc50-403">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="dcc50-404">例外狀況篩選條件會產生動作結果來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-404">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="dcc50-405">藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> 設為 `true`，<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-405">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="dcc50-406">在縮短時寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="dcc50-406">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="dcc50-407">在 `IResultFilter.OnResultExecuting` 中擲回例外狀況會：</span><span class="sxs-lookup"><span data-stu-id="dcc50-407">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="dcc50-408">導致無法執行動作結果和後續的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="dcc50-408">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="dcc50-409">視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="dcc50-409">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="dcc50-410">當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="dcc50-410">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="dcc50-411">如果回應已傳送給用戶端，則無法進一步變更。</span><span class="sxs-lookup"><span data-stu-id="dcc50-411">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="dcc50-412">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-412">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="dcc50-413">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="dcc50-413">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="dcc50-414">將 `Exception` 設為 Null 實際上會「處理」例外狀況，並且使 ASP.NET Core 稍後不會在管線中重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="dcc50-414">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="dcc50-415">並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。</span><span class="sxs-lookup"><span data-stu-id="dcc50-415">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="dcc50-416">當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="dcc50-416">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="dcc50-417">針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 上的 `await next` 的呼叫會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="dcc50-417">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="dcc50-418">若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：</span><span class="sxs-lookup"><span data-stu-id="dcc50-418">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="dcc50-419">架構提供抽象 `ResultFilterAttribute`，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="dcc50-419">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="dcc50-420">先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="dcc50-420">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="dcc50-421">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="dcc50-421">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="dcc50-422"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="dcc50-422">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="dcc50-423">這包括由產生的動作結果：</span><span class="sxs-lookup"><span data-stu-id="dcc50-423">This includes action results produced by:</span></span>

* <span data-ttu-id="dcc50-424">最少迴圈的授權篩選準則和資源篩選器。</span><span class="sxs-lookup"><span data-stu-id="dcc50-424">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="dcc50-425">例外狀況篩選準則。</span><span class="sxs-lookup"><span data-stu-id="dcc50-425">Exception filters.</span></span>

<span data-ttu-id="dcc50-426">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="dcc50-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="dcc50-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="dcc50-427">IFilterFactory</span></span>

<span data-ttu-id="dcc50-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="dcc50-429">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="dcc50-430">當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="dcc50-431">如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="dcc50-432">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="dcc50-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="dcc50-433">可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：</span><span class="sxs-lookup"><span data-stu-id="dcc50-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="dcc50-434">上述程式碼可以透過執行[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) 來進行測試：</span><span class="sxs-lookup"><span data-stu-id="dcc50-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="dcc50-435">叫用 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="dcc50-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="dcc50-436">巡覽至 `https://localhost:5001/Sample/HeaderWithFactory`。</span><span class="sxs-lookup"><span data-stu-id="dcc50-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="dcc50-437">F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：</span><span class="sxs-lookup"><span data-stu-id="dcc50-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="dcc50-438">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="dcc50-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="dcc50-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="dcc50-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="dcc50-440">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="dcc50-440">**internal:** `My header`</span></span>

<span data-ttu-id="dcc50-441">上述程式碼會建立 **internal:** `My header` 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="dcc50-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="dcc50-442">在屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="dcc50-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="dcc50-443">實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="dcc50-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="dcc50-444">不需要傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="dcc50-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="dcc50-445">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="dcc50-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="dcc50-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="dcc50-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="dcc50-447">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="dcc50-448">`CreateInstance` 會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="dcc50-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="dcc50-449">下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：</span><span class="sxs-lookup"><span data-stu-id="dcc50-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="dcc50-450">在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="dcc50-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="dcc50-451">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="dcc50-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="dcc50-452">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="dcc50-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="dcc50-453">但篩選條件與中介軟體不同之處在於它們是 ASP.NET Core 執行階段的一部分，這表示它們能存取 ASP.NET Core 內容和建構。</span><span class="sxs-lookup"><span data-stu-id="dcc50-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="dcc50-454">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="dcc50-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="dcc50-455">以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="dcc50-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="dcc50-456">使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="dcc50-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="dcc50-457">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="dcc50-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="dcc50-458">後續動作</span><span class="sxs-lookup"><span data-stu-id="dcc50-458">Next actions</span></span>

* <span data-ttu-id="dcc50-459">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-459">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="dcc50-460">若要嘗試使用篩選條件，請[下載、測試及修改 GitHub 範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="dcc50-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
