---
title: ASP.NET Core 中的篩選條件
author: Rick-Anderson
description: 了解篩選條件的運作方式，以及如何在 ASP.NET Core 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 2300b14a6a89191d3d8c673311880fc144183da9
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608117"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="9c94e-103">ASP.NET Core 中的篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9c94e-104">作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="9c94e-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9c94e-105">ASP.NET Core 中的「篩選條件」可讓程式碼在要求處理管線中的特定階段之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="9c94e-106">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="9c94e-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="9c94e-107">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="9c94e-108">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="9c94e-109">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="9c94e-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="9c94e-110">跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。</span><span class="sxs-lookup"><span data-stu-id="9c94e-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="9c94e-111">篩選能避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="9c94e-112">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="9c94e-113">本文件適用於 Razor Pages、API 控制器，以及具有檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="9c94e-114">[檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="9c94e-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="9c94e-115">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="9c94e-115">How filters work</span></span>

<span data-ttu-id="9c94e-116">篩選條件會在「ASP.NET Core 動作引動過程管線」中執行，其有時也被稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="9c94e-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="9c94e-117">在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![此要求會透過其他中介軟體、路由中介軟體、動作選取和動作調用管線來處理。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="9c94e-120">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="9c94e-120">Filter types</span></span>

<span data-ttu-id="9c94e-121">每個篩選類型會在篩選條件管線中的不同階段執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="9c94e-122">[授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。</span><span class="sxs-lookup"><span data-stu-id="9c94e-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="9c94e-123">如果要求未獲授權，授權篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="9c94e-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="9c94e-124">[資源篩選條件](#resource-filters)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="9c94e-125">會在授權之後執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-125">Run after authorization.</span></span>  
  * <span data-ttu-id="9c94e-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 在篩選器管線的其餘部分之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="9c94e-127">例如，`OnResourceExecuting` 會在模型系結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="9c94e-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 在完成管線的其餘部分之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="9c94e-129">[動作篩選條件](#action-filters)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="9c94e-130">在呼叫動作方法之前和之後，立即執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="9c94e-131">可以變更傳遞至動作的引數。</span><span class="sxs-lookup"><span data-stu-id="9c94e-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="9c94e-132">可以變更從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="9c94e-133">Razor Pages 中**不**支援。</span><span class="sxs-lookup"><span data-stu-id="9c94e-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="9c94e-134">[例外狀況篩選準則](#exception-filters)會將全域原則套用至在寫入回應主體之前發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="9c94e-135">[結果篩選準則](#result-filters)會在動作結果執行前後立即執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="9c94e-136">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="9c94e-137">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9c94e-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="9c94e-138">下圖顯示篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="9c94e-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="9c94e-141">實作</span><span class="sxs-lookup"><span data-stu-id="9c94e-141">Implementation</span></span>

<span data-ttu-id="9c94e-142">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="9c94e-143">同步篩選會在其管線階段之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="9c94e-144">例如，系統會在呼叫動作方法之前呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="9c94e-145">系統會在傳回動作方法之後呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="9c94e-146">非同步篩選會定義 `On-Stage-ExecutionAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="9c94e-147">例如，<xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>：</span><span class="sxs-lookup"><span data-stu-id="9c94e-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="9c94e-148">在上述程式碼中，`SampleAsyncActionFilter` 具有執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> （`next`）。</span><span class="sxs-lookup"><span data-stu-id="9c94e-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="9c94e-149">多個篩選條件階段</span><span class="sxs-lookup"><span data-stu-id="9c94e-149">Multiple filter stages</span></span>

<span data-ttu-id="9c94e-150">可以在單一類別中實作多個篩選條件階段的介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="9c94e-151">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="9c94e-152">同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="9c94e-153">非同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="9c94e-154">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="9c94e-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="9c94e-155">執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="9c94e-156">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="9c94e-157">如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="9c94e-158">使用抽象類別（例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>）時，只會覆寫同步方法或每個篩選準則類型的非同步方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="9c94e-159">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="9c94e-159">Built-in filter attributes</span></span>

<span data-ttu-id="9c94e-160">ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。</span><span class="sxs-lookup"><span data-stu-id="9c94e-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="9c94e-161">例如，下列結果篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="9c94e-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-162">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="9c94e-163">將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="9c94e-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="9c94e-164">使用[瀏覽器開發人員工具](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools)之類的工具來檢查標頭。</span><span class="sxs-lookup"><span data-stu-id="9c94e-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="9c94e-165">在 [**回應標頭**] 底下，會顯示 `author: Rick Anderson`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="9c94e-166">下列程式碼會實 `ActionFilterAttribute`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="9c94e-167">讀取設定系統中的標題和名稱。</span><span class="sxs-lookup"><span data-stu-id="9c94e-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="9c94e-168">與先前的範例不同的是，下列程式碼不需要將篩選參數新增至程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="9c94e-169">將標題和名稱新增至回應標頭。</span><span class="sxs-lookup"><span data-stu-id="9c94e-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-170">設定選項是使用[選項模式](xref:fundamentals/configuration/options)從設定[系統](xref:fundamentals/configuration/index)提供。</span><span class="sxs-lookup"><span data-stu-id="9c94e-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="9c94e-171">例如，從*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c94e-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="9c94e-172">在 `StartUp.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="9c94e-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="9c94e-173">`PositionOptions` 類別會使用 `"Position"` 設定區域新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="9c94e-174">`MyActionFilterAttribute` 會新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="9c94e-175">下列程式碼顯示 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="9c94e-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="9c94e-176">下列程式碼會將 `MyActionFilterAttribute` 套用至 `Index2` 方法：</span><span class="sxs-lookup"><span data-stu-id="9c94e-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="9c94e-177">當呼叫 `Sample/Index2` 端點時，會顯示 [**回應標頭**]、[`author: Rick Anderson`] 和 [`Editor: Joe Smith`]。</span><span class="sxs-lookup"><span data-stu-id="9c94e-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="9c94e-178">下列程式碼會將 `MyActionFilterAttribute` 和 `AddHeaderAttribute` 套用至 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="9c94e-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="9c94e-179">無法將篩選套用至 Razor 頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="9c94e-180">它們可以套用至 Razor 頁面模型或全域套用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="9c94e-181">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="9c94e-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="9c94e-182">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="9c94e-183">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="9c94e-184">篩選條件能以三個「範圍」之一新增至管線：</span><span class="sxs-lookup"><span data-stu-id="9c94e-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="9c94e-185">在控制器動作上使用屬性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="9c94e-186">無法將篩選屬性套用至 Razor Pages 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="9c94e-187">使用控制器或 Razor 頁面上的屬性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="9c94e-188">全域適用于所有的控制器、動作和 Razor Pages，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="9c94e-189">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-189">Default order of execution</span></span>

<span data-ttu-id="9c94e-190">當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="9c94e-191">全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="9c94e-192">因為篩選條件巢狀結構的原因，篩選條件的「之後」程式碼的執行順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="9c94e-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="9c94e-193">篩選條件序列：</span><span class="sxs-lookup"><span data-stu-id="9c94e-193">The filter sequence:</span></span>

* <span data-ttu-id="9c94e-194">全域篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="9c94e-195">控制站的*程式碼和*Razor 頁面篩選準則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="9c94e-196">動作方法篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="9c94e-197">動作方法篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="9c94e-198">控制器和 Razor 頁面篩選*後*的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="9c94e-199">全域篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="9c94e-200">下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="9c94e-201">序列</span><span class="sxs-lookup"><span data-stu-id="9c94e-201">Sequence</span></span> | <span data-ttu-id="9c94e-202">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="9c94e-202">Filter scope</span></span> | <span data-ttu-id="9c94e-203">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="9c94e-204">1</span><span class="sxs-lookup"><span data-stu-id="9c94e-204">1</span></span> | <span data-ttu-id="9c94e-205">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-206">2</span><span class="sxs-lookup"><span data-stu-id="9c94e-206">2</span></span> | <span data-ttu-id="9c94e-207">控制器或 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="9c94e-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="9c94e-208">3</span><span class="sxs-lookup"><span data-stu-id="9c94e-208">3</span></span> | <span data-ttu-id="9c94e-209">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-210">4</span><span class="sxs-lookup"><span data-stu-id="9c94e-210">4</span></span> | <span data-ttu-id="9c94e-211">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="9c94e-212">5</span><span class="sxs-lookup"><span data-stu-id="9c94e-212">5</span></span> | <span data-ttu-id="9c94e-213">控制器或 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="9c94e-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="9c94e-214">6</span><span class="sxs-lookup"><span data-stu-id="9c94e-214">6</span></span> | <span data-ttu-id="9c94e-215">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="9c94e-216">控制器層級篩選</span><span class="sxs-lookup"><span data-stu-id="9c94e-216">Controller level filters</span></span>

<span data-ttu-id="9c94e-217">繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="9c94e-218">這些方法會：</span><span class="sxs-lookup"><span data-stu-id="9c94e-218">These methods:</span></span>

* <span data-ttu-id="9c94e-219">包裝針對指定動作執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="9c94e-220">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="9c94e-221">系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="9c94e-222">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="9c94e-223">在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="9c94e-224">例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="9c94e-225">`TestController`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-225">The `TestController`:</span></span>

* <span data-ttu-id="9c94e-226">將 `SampleActionFilterAttribute` （`[SampleActionFilter]`）套用至 `FilterTest2` 動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="9c94e-227">覆寫 `OnActionExecuting` 和 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="9c94e-228">瀏覽至 `https://localhost:5001/Test2/FilterTest2` 執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="9c94e-229">控制器層級篩選會將[Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-229">Controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="9c94e-230">在套用至方法的篩選器之後，**無法**將控制器層級篩選設定為執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="9c94e-231">下一節將說明順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-231">Order is explained in the next section.</span></span>

<span data-ttu-id="9c94e-232">針對 Razor Pages，請參閱[覆寫篩選條件方法來實作 Razor 頁面篩選條件](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="9c94e-233">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-233">Overriding the default order</span></span>

<span data-ttu-id="9c94e-234">可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="9c94e-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="9c94e-235">`IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="9c94e-236">具有較低 `Order` 值的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="9c94e-237">在具有較高 `Order` 值的篩選條件之前執行「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="9c94e-238">在具有較高 `Order` 值的篩選條件之後執行「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="9c94e-239">`Order` 屬性是以「函式參數設定：</span><span class="sxs-lookup"><span data-stu-id="9c94e-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="9c94e-240">請考慮下列控制器中的兩個動作篩選準則：</span><span class="sxs-lookup"><span data-stu-id="9c94e-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="9c94e-241">全域篩選準則會在 `StartUp.ConfigureServices`中新增：</span><span class="sxs-lookup"><span data-stu-id="9c94e-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="9c94e-242">3個篩選準則會依照下列循序執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="9c94e-243">`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。</span><span class="sxs-lookup"><span data-stu-id="9c94e-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="9c94e-244">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="9c94e-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="9c94e-245">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="9c94e-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="9c94e-246">如先前所述，控制器層級篩選會針對內建篩選準則將[order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue`，除非 `Order` 設定為非零值，否則範圍會決定訂單。</span><span class="sxs-lookup"><span data-stu-id="9c94e-246">As mentioned previously, controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="9c94e-247">在上述程式碼中，`MySampleActionFilter` 具有全域範圍，因此它會在具有控制器範圍的 `MyAction2FilterAttribute`之前執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="9c94e-248">若要讓 `MyAction2FilterAttribute` 先執行，請將順序設定為 `int.MinValue`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="9c94e-249">若要讓全域篩選 `MySampleActionFilter` 先執行，請將 `Order` 設定為 `int.MinValue`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="9c94e-250">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="9c94e-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="9c94e-251">可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 屬性，以縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="9c94e-252">比方說，下列資源篩選條件可防止管線的其餘部分執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-253">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="9c94e-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="9c94e-254">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="9c94e-255">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="9c94e-256">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="9c94e-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="9c94e-257">因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="9c94e-258">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="9c94e-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="9c94e-259">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="9c94e-260">相依性插入</span><span class="sxs-lookup"><span data-stu-id="9c94e-260">Dependency injection</span></span>

<span data-ttu-id="9c94e-261">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="9c94e-262">如果新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="9c94e-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="9c94e-263">如果新增類型，其將會由類型啟動。</span><span class="sxs-lookup"><span data-stu-id="9c94e-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="9c94e-264">由類型啟動的篩選條件表示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-264">A type-activated filter means:</span></span>

* <span data-ttu-id="9c94e-265">系統會針對每個要求建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-265">An instance is created for each request.</span></span>
* <span data-ttu-id="9c94e-266">任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。</span><span class="sxs-lookup"><span data-stu-id="9c94e-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="9c94e-267">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="9c94e-268">DI 無法提供建構函式相依性，因為：</span><span class="sxs-lookup"><span data-stu-id="9c94e-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="9c94e-269">必須在提供屬性之建構函式參數的地方提供它們。</span><span class="sxs-lookup"><span data-stu-id="9c94e-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="9c94e-270">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="9c94e-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="9c94e-271">下列篩選條件支援由 DI 所提供的建構函式相依性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="9c94e-272">在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="9c94e-273">上述篩選條件可以被套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="9c94e-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="9c94e-274">DI 會提供記錄器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-274">Loggers are available from DI.</span></span> <span data-ttu-id="9c94e-275">不過，請避免僅針對記錄目的建立並使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="9c94e-276">[內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。</span><span class="sxs-lookup"><span data-stu-id="9c94e-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="9c94e-277">新增至篩選條件的記錄：</span><span class="sxs-lookup"><span data-stu-id="9c94e-277">Logging added to filters:</span></span>

* <span data-ttu-id="9c94e-278">應該專注在篩選條件特定的商務領域考量或行為。</span><span class="sxs-lookup"><span data-stu-id="9c94e-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="9c94e-279">**不**應該記錄動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="9c94e-280">內建篩選記錄動作和架構事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="9c94e-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="9c94e-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="9c94e-282">服務篩選條件實作類型會註冊在 `ConfigureServices` 中。</span><span class="sxs-lookup"><span data-stu-id="9c94e-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="9c94e-283"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="9c94e-284">下面程式碼會說明 `AddHeaderResultServiceFilter`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="9c94e-285">在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="9c94e-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="9c94e-286">在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：</span><span class="sxs-lookup"><span data-stu-id="9c94e-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="9c94e-287">使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="9c94e-288">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="9c94e-289">ASP.NET Core 執行階段並不保證：</span><span class="sxs-lookup"><span data-stu-id="9c94e-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="9c94e-290">將會建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="9c94e-291">將不會於稍後的時間從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="9c94e-292">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="9c94e-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="9c94e-294">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="9c94e-295">`CreateInstance` 會從 DI 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="9c94e-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="9c94e-296">TypeFilterAttribute</span></span>

<span data-ttu-id="9c94e-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="9c94e-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="9c94e-298">它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="9c94e-299">由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：</span><span class="sxs-lookup"><span data-stu-id="9c94e-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="9c94e-300">使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9c94e-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="9c94e-301">不過它們的相依性會由 DI 容器滿足。</span><span class="sxs-lookup"><span data-stu-id="9c94e-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="9c94e-302">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="9c94e-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="9c94e-303">使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="9c94e-304">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="9c94e-305">ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="9c94e-306">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="9c94e-307">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="9c94e-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="9c94e-308">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-308">Authorization filters</span></span>

<span data-ttu-id="9c94e-309">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-309">Authorization filters:</span></span>

* <span data-ttu-id="9c94e-310">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="9c94e-311">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="9c94e-311">Control access to action methods.</span></span>
* <span data-ttu-id="9c94e-312">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="9c94e-313">自訂授權篩選條件需要自訂的授權架構。</span><span class="sxs-lookup"><span data-stu-id="9c94e-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="9c94e-314">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="9c94e-315">內建的授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="9c94e-316">呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="9c94e-316">Calls the authorization system.</span></span>
* <span data-ttu-id="9c94e-317">不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="9c94e-317">Does not authorize requests.</span></span>

<span data-ttu-id="9c94e-318">**不會**在授權篩選條件內擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="9c94e-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="9c94e-319">該例外狀況將不會被處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-319">The exception will not be handled.</span></span>
* <span data-ttu-id="9c94e-320">例外狀況篩選條件將不會處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="9c94e-321">請考慮在例外狀況於授權篩選條件中發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="9c94e-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="9c94e-322">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="9c94e-323">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-323">Resource filters</span></span>

<span data-ttu-id="9c94e-324">資源篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-324">Resource filters:</span></span>

* <span data-ttu-id="9c94e-325">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="9c94e-326">執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="9c94e-327">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="9c94e-328">資源篩選條件很適合用來縮短大部分的管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="9c94e-329">比方說，快取篩選條件可以避免快取命中上的其餘管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="9c94e-330">資源篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="9c94e-330">Resource filter examples:</span></span>

* <span data-ttu-id="9c94e-331">先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="9c94e-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="9c94e-333">防止模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="9c94e-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="9c94e-334">用於大型檔案上傳，以避免將表單資料讀入記憶體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="9c94e-335">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-335">Action filters</span></span>

<span data-ttu-id="9c94e-336">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="9c94e-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="9c94e-337">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="9c94e-338">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="9c94e-339">動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-339">Action filters:</span></span>

* <span data-ttu-id="9c94e-340">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="9c94e-341">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="9c94e-342">下列程式碼會顯示範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="9c94e-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="9c94e-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-啟用讀取動作方法的輸入。</span><span class="sxs-lookup"><span data-stu-id="9c94e-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="9c94e-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="9c94e-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="9c94e-347">在動作方法中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="9c94e-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="9c94e-348">防止執行後續篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="9c94e-349">和設定 `Result` 不同，會被視為失敗而非成功結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="9c94e-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="9c94e-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="9c94e-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="9c94e-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="9c94e-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="9c94e-353">將此屬性設定為 Null：</span><span class="sxs-lookup"><span data-stu-id="9c94e-353">Setting this property to null:</span></span>

  * <span data-ttu-id="9c94e-354">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="9c94e-355">會執行 `Result`，有如它是從動作方法傳回一般。</span><span class="sxs-lookup"><span data-stu-id="9c94e-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="9c94e-356">對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：</span><span class="sxs-lookup"><span data-stu-id="9c94e-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="9c94e-357">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="9c94e-358">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="9c94e-359">若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="9c94e-360">架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="9c94e-361">`OnActionExecuting` 動作篩選條件可以用來：</span><span class="sxs-lookup"><span data-stu-id="9c94e-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="9c94e-362">驗證模型狀態。</span><span class="sxs-lookup"><span data-stu-id="9c94e-362">Validate model state.</span></span>
* <span data-ttu-id="9c94e-363">如果狀態無效，則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="9c94e-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-364">`OnActionExecuted` 方法會在動作方法之後執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="9c94e-365">並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="9c94e-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。</span><span class="sxs-lookup"><span data-stu-id="9c94e-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="9c94e-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="9c94e-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="9c94e-368">將 `Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="9c94e-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="9c94e-369">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="9c94e-370">執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="9c94e-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="9c94e-371">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-371">Exception filters</span></span>

<span data-ttu-id="9c94e-372">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-372">Exception filters:</span></span>

* <span data-ttu-id="9c94e-373">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="9c94e-374">可以用來實作常見的錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="9c94e-375">下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="9c94e-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="9c94e-376">下列程式碼會測試例外狀況篩選準則：</span><span class="sxs-lookup"><span data-stu-id="9c94e-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="9c94e-377">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-377">Exception filters:</span></span>

* <span data-ttu-id="9c94e-378">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-378">Don't have before and after events.</span></span>
* <span data-ttu-id="9c94e-379">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="9c94e-380">處理在 Razor 頁面或控制器建立、[模型繫結](xref:mvc/models/model-binding)、動作篩選條件或動作方法中發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="9c94e-381">**不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="9c94e-382">若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="9c94e-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="9c94e-383">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-383">This stops propagation of the exception.</span></span> <span data-ttu-id="9c94e-384">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="9c94e-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="9c94e-385">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="9c94e-385">Only an action filter can do that.</span></span>

<span data-ttu-id="9c94e-386">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-386">Exception filters:</span></span>

* <span data-ttu-id="9c94e-387">適合用於設陷在動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="9c94e-388">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="9c94e-389">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="9c94e-390">只有在錯誤處理會根據所呼叫的動作方法而「不同」時才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="9c94e-391">比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="9c94e-392">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="9c94e-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="9c94e-393">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-393">Result filters</span></span>

<span data-ttu-id="9c94e-394">結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-394">Result filters:</span></span>

* <span data-ttu-id="9c94e-395">實作介面：</span><span class="sxs-lookup"><span data-stu-id="9c94e-395">Implement an interface:</span></span>
  * <span data-ttu-id="9c94e-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="9c94e-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="9c94e-398">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="9c94e-399">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="9c94e-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="9c94e-400">以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="9c94e-401">執行的結果類型會取決於動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="9c94e-402">傳回視圖的動作包含所有 razor 處理，做為執行 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 的一部分。</span><span class="sxs-lookup"><span data-stu-id="9c94e-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="9c94e-403">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="9c94e-404">深入瞭解[動作結果](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="9c94e-405">只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="9c94e-406">當下列情況時，不會執行結果篩選：</span><span class="sxs-lookup"><span data-stu-id="9c94e-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="9c94e-407">授權篩選或資源篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="9c94e-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="9c94e-408">例外狀況篩選條件會產生動作結果來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="9c94e-409">藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> 設為 `true`，<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="9c94e-410">在縮短時寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="9c94e-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="9c94e-411">在 `IResultFilter.OnResultExecuting`中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="9c94e-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="9c94e-412">防止執行動作結果和後續的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="9c94e-413">會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="9c94e-414">當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="9c94e-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="9c94e-415">如果回應已經傳送到用戶端，就無法變更。</span><span class="sxs-lookup"><span data-stu-id="9c94e-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="9c94e-416">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="9c94e-417">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="9c94e-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="9c94e-418">將 `Exception` 設定為 null 會有效率地處理例外狀況，並防止稍後在管線中擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="9c94e-419">並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。</span><span class="sxs-lookup"><span data-stu-id="9c94e-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="9c94e-420">當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="9c94e-421">針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 上的 `await next` 的呼叫會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="9c94e-422">若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="9c94e-423">架構提供抽象 `ResultFilterAttribute`，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="9c94e-424">先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="9c94e-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="9c94e-425">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="9c94e-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="9c94e-426"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="9c94e-427">這包括由產生的動作結果：</span><span class="sxs-lookup"><span data-stu-id="9c94e-427">This includes action results produced by:</span></span>

* <span data-ttu-id="9c94e-428">最少迴圈的授權篩選準則和資源篩選器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="9c94e-429">例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-429">Exception filters.</span></span>

<span data-ttu-id="9c94e-430">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="9c94e-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="9c94e-431">IFilterFactory</span></span>

<span data-ttu-id="9c94e-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="9c94e-433">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="9c94e-434">當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="9c94e-435">如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="9c94e-436">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="9c94e-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="9c94e-437">可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="9c94e-438">此篩選準則會套用至下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="9c94e-439">執行[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)來測試上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="9c94e-440">叫用 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="9c94e-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="9c94e-441">巡覽至 `https://localhost:5001/Sample/HeaderWithFactory`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="9c94e-442">F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：</span><span class="sxs-lookup"><span data-stu-id="9c94e-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="9c94e-443">**作者：** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="9c94e-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="9c94e-444">**globaladdheader：** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="9c94e-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="9c94e-445">**內部：** `My header`</span><span class="sxs-lookup"><span data-stu-id="9c94e-445">**internal:** `My header`</span></span>

<span data-ttu-id="9c94e-446">上述程式碼會建立**internal：** `My header` 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="9c94e-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="9c94e-447">在屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="9c94e-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="9c94e-448">實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="9c94e-449">不需要傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="9c94e-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="9c94e-450">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="9c94e-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="9c94e-452">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="9c94e-453">`CreateInstance` 會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="9c94e-454">下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：</span><span class="sxs-lookup"><span data-stu-id="9c94e-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="9c94e-455">在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="9c94e-456">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="9c94e-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="9c94e-457">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="9c94e-458">但是篩選器與中介軟體不同之處在于它們是執行時間的一部分，這表示它們可以存取內容和結構。</span><span class="sxs-lookup"><span data-stu-id="9c94e-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="9c94e-459">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="9c94e-460">以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="9c94e-461">使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="9c94e-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="9c94e-462">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="9c94e-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="9c94e-463">後續動作</span><span class="sxs-lookup"><span data-stu-id="9c94e-463">Next actions</span></span>

* <span data-ttu-id="9c94e-464">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="9c94e-465">若要嘗試使用篩選條件，請[下載、測試及修改 GitHub 範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9c94e-466">作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="9c94e-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9c94e-467">ASP.NET Core 中的「篩選條件」可讓程式碼在要求處理管線中的特定階段之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="9c94e-468">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="9c94e-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="9c94e-469">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="9c94e-470">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="9c94e-471">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="9c94e-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="9c94e-472">跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。</span><span class="sxs-lookup"><span data-stu-id="9c94e-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="9c94e-473">篩選能避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="9c94e-474">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="9c94e-475">本文件適用於 Razor Pages、API 控制器，以及具有檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="9c94e-476">[檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="9c94e-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="9c94e-477">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="9c94e-477">How filters work</span></span>

<span data-ttu-id="9c94e-478">篩選條件會在「ASP.NET Core 動作引動過程管線」中執行，其有時也被稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="9c94e-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="9c94e-479">在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 ASP.NET Core 動作引動過程管線。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="9c94e-482">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="9c94e-482">Filter types</span></span>

<span data-ttu-id="9c94e-483">每個篩選類型會在篩選條件管線中的不同階段執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="9c94e-484">[授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。</span><span class="sxs-lookup"><span data-stu-id="9c94e-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="9c94e-485">如果要求未經授權，授權篩選條件便會縮短管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="9c94e-486">[資源篩選條件](#resource-filters)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="9c94e-487">會在授權之後執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-487">Run after authorization.</span></span>  
  * <span data-ttu-id="9c94e-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 可以在其餘的篩選條件管線之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="9c94e-489">例如，`OnResourceExecuting` 可以在模型繫結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="9c94e-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 可以在其餘的管線皆已完成之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="9c94e-491">[動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="9c94e-492">它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="9c94e-493">Razor Pages 中**不**支援動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="9c94e-494">[例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="9c94e-495">[結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="9c94e-496">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="9c94e-497">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9c94e-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="9c94e-498">下圖顯示篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="9c94e-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="9c94e-501">實作</span><span class="sxs-lookup"><span data-stu-id="9c94e-501">Implementation</span></span>

<span data-ttu-id="9c94e-502">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="9c94e-503">同步篩選條件可以在其管線階段之前 (`On-Stage-Executing`) 和之後 (`On-Stage-Executed`) 執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="9c94e-504">例如，系統會在呼叫動作方法之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="9c94e-505">系統會在傳回動作方法之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="9c94e-506">非同步篩選條件會定義 `On-Stage-ExecutionAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="9c94e-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="9c94e-507">在上述程式碼中，`SampleAsyncActionFilter` 具有會執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="9c94e-508">每個 `On-Stage-ExecutionAsync` 都會接受能執行篩選條件之管線階段的 `FilterType-ExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="9c94e-509">多個篩選條件階段</span><span class="sxs-lookup"><span data-stu-id="9c94e-509">Multiple filter stages</span></span>

<span data-ttu-id="9c94e-510">可以在單一類別中實作多個篩選條件階段的介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="9c94e-511">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。</span><span class="sxs-lookup"><span data-stu-id="9c94e-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="9c94e-512">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="9c94e-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="9c94e-513">執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="9c94e-514">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="9c94e-515">如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="9c94e-516">使用抽象類別 (例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>) 時，請僅覆寫每個篩選條件類型的同步方法或非同步方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="9c94e-517">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="9c94e-517">Built-in filter attributes</span></span>

<span data-ttu-id="9c94e-518">ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。</span><span class="sxs-lookup"><span data-stu-id="9c94e-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="9c94e-519">例如，下列結果篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="9c94e-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-520">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="9c94e-521">將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="9c94e-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="9c94e-522">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="9c94e-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="9c94e-523">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="9c94e-524">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="9c94e-525">篩選條件能以三個「範圍」之一新增至管線：</span><span class="sxs-lookup"><span data-stu-id="9c94e-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="9c94e-526">針對動作使用屬性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="9c94e-527">針對控制器使用屬性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="9c94e-528">全域地針對所有控制器和動作，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9c94e-529">上述程式碼會使用 [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) 集合來全域地新增三個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="9c94e-530">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-530">Default order of execution</span></span>

<span data-ttu-id="9c94e-531">當有多個*相同類型*的篩選時，範圍會決定篩選執行的預設順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="9c94e-532">全域篩選準則會括住類別篩選。</span><span class="sxs-lookup"><span data-stu-id="9c94e-532">Global filters surround class filters.</span></span> <span data-ttu-id="9c94e-533">類別篩選準則環繞方法篩選準則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-533">Class filters surround method filters.</span></span>

<span data-ttu-id="9c94e-534">因為篩選條件巢狀結構的原因，篩選條件的「之後」程式碼的執行順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="9c94e-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="9c94e-535">篩選條件序列：</span><span class="sxs-lookup"><span data-stu-id="9c94e-535">The filter sequence:</span></span>

* <span data-ttu-id="9c94e-536">全域篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="9c94e-537">控制器篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="9c94e-538">動作方法篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="9c94e-539">動作方法篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="9c94e-540">控制器篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="9c94e-541">全域篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="9c94e-542">下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="9c94e-543">序列</span><span class="sxs-lookup"><span data-stu-id="9c94e-543">Sequence</span></span> | <span data-ttu-id="9c94e-544">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="9c94e-544">Filter scope</span></span> | <span data-ttu-id="9c94e-545">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="9c94e-546">1</span><span class="sxs-lookup"><span data-stu-id="9c94e-546">1</span></span> | <span data-ttu-id="9c94e-547">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-548">2</span><span class="sxs-lookup"><span data-stu-id="9c94e-548">2</span></span> | <span data-ttu-id="9c94e-549">控制器</span><span class="sxs-lookup"><span data-stu-id="9c94e-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-550">3</span><span class="sxs-lookup"><span data-stu-id="9c94e-550">3</span></span> | <span data-ttu-id="9c94e-551">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-552">4</span><span class="sxs-lookup"><span data-stu-id="9c94e-552">4</span></span> | <span data-ttu-id="9c94e-553">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="9c94e-554">5</span><span class="sxs-lookup"><span data-stu-id="9c94e-554">5</span></span> | <span data-ttu-id="9c94e-555">控制器</span><span class="sxs-lookup"><span data-stu-id="9c94e-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="9c94e-556">6</span><span class="sxs-lookup"><span data-stu-id="9c94e-556">6</span></span> | <span data-ttu-id="9c94e-557">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="9c94e-558">此順序顯示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-558">This sequence shows:</span></span>

* <span data-ttu-id="9c94e-559">方法篩選條件巢狀位於控制器篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="9c94e-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="9c94e-560">控制器篩選條件巢狀位於全域篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="9c94e-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="9c94e-561">控制器和 Razor 頁面層級篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="9c94e-562">繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="9c94e-563">這些方法會：</span><span class="sxs-lookup"><span data-stu-id="9c94e-563">These methods:</span></span>

* <span data-ttu-id="9c94e-564">包裝針對指定動作執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="9c94e-565">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="9c94e-566">系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="9c94e-567">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="9c94e-568">在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="9c94e-569">例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="9c94e-570">`TestController`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-570">The `TestController`:</span></span>

* <span data-ttu-id="9c94e-571">將 `SampleActionFilterAttribute` （`[SampleActionFilter]`）套用至 `FilterTest2` 動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="9c94e-572">覆寫 `OnActionExecuting` 和 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="9c94e-573">瀏覽至 `https://localhost:5001/Test/FilterTest2` 執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="9c94e-574">針對 Razor Pages，請參閱[覆寫篩選條件方法來實作 Razor 頁面篩選條件](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="9c94e-575">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="9c94e-575">Overriding the default order</span></span>

<span data-ttu-id="9c94e-576">可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="9c94e-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="9c94e-577">`IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="9c94e-578">具有較低 `Order` 值的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="9c94e-579">在具有較高 `Order` 值的篩選條件之前執行「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="9c94e-580">在具有較高 `Order` 值的篩選條件之後執行「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="9c94e-581">`Order` 屬性可以搭配建構函式參數設定：</span><span class="sxs-lookup"><span data-stu-id="9c94e-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="9c94e-582">請考慮先前範例所示的同樣 3 個動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="9c94e-583">如果控制器和全域篩選條件的 `Order` 屬性被個別設定為 1 和 2，執行順序將會反轉。</span><span class="sxs-lookup"><span data-stu-id="9c94e-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="9c94e-584">序列</span><span class="sxs-lookup"><span data-stu-id="9c94e-584">Sequence</span></span> | <span data-ttu-id="9c94e-585">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="9c94e-585">Filter scope</span></span> | <span data-ttu-id="9c94e-586">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="9c94e-586">`Order` property</span></span> | <span data-ttu-id="9c94e-587">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="9c94e-588">1</span><span class="sxs-lookup"><span data-stu-id="9c94e-588">1</span></span> | <span data-ttu-id="9c94e-589">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-589">Method</span></span> | <span data-ttu-id="9c94e-590">0</span><span class="sxs-lookup"><span data-stu-id="9c94e-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="9c94e-591">2</span><span class="sxs-lookup"><span data-stu-id="9c94e-591">2</span></span> | <span data-ttu-id="9c94e-592">控制器</span><span class="sxs-lookup"><span data-stu-id="9c94e-592">Controller</span></span> | <span data-ttu-id="9c94e-593">1</span><span class="sxs-lookup"><span data-stu-id="9c94e-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="9c94e-594">3</span><span class="sxs-lookup"><span data-stu-id="9c94e-594">3</span></span> | <span data-ttu-id="9c94e-595">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-595">Global</span></span> | <span data-ttu-id="9c94e-596">2</span><span class="sxs-lookup"><span data-stu-id="9c94e-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="9c94e-597">4</span><span class="sxs-lookup"><span data-stu-id="9c94e-597">4</span></span> | <span data-ttu-id="9c94e-598">Global</span><span class="sxs-lookup"><span data-stu-id="9c94e-598">Global</span></span> | <span data-ttu-id="9c94e-599">2</span><span class="sxs-lookup"><span data-stu-id="9c94e-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="9c94e-600">5</span><span class="sxs-lookup"><span data-stu-id="9c94e-600">5</span></span> | <span data-ttu-id="9c94e-601">控制器</span><span class="sxs-lookup"><span data-stu-id="9c94e-601">Controller</span></span> | <span data-ttu-id="9c94e-602">1</span><span class="sxs-lookup"><span data-stu-id="9c94e-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="9c94e-603">6</span><span class="sxs-lookup"><span data-stu-id="9c94e-603">6</span></span> | <span data-ttu-id="9c94e-604">方法</span><span class="sxs-lookup"><span data-stu-id="9c94e-604">Method</span></span> | <span data-ttu-id="9c94e-605">0</span><span class="sxs-lookup"><span data-stu-id="9c94e-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="9c94e-606">`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。</span><span class="sxs-lookup"><span data-stu-id="9c94e-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="9c94e-607">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="9c94e-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="9c94e-608">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="9c94e-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="9c94e-609">針對內建篩選條件，除非將 `Order` 設定為非零值，否則範圍會決定順序。</span><span class="sxs-lookup"><span data-stu-id="9c94e-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="9c94e-610">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="9c94e-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="9c94e-611">可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 屬性，以縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="9c94e-612">比方說，下列資源篩選條件可防止管線的其餘部分執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-613">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="9c94e-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="9c94e-614">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="9c94e-615">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="9c94e-616">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="9c94e-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="9c94e-617">因此，`SomeResource` 動作的 `AddHeader` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="9c94e-618">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="9c94e-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="9c94e-619">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="9c94e-620">相依性插入</span><span class="sxs-lookup"><span data-stu-id="9c94e-620">Dependency injection</span></span>

<span data-ttu-id="9c94e-621">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="9c94e-622">如果新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="9c94e-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="9c94e-623">如果新增類型，其將會由類型啟動。</span><span class="sxs-lookup"><span data-stu-id="9c94e-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="9c94e-624">由類型啟動的篩選條件表示：</span><span class="sxs-lookup"><span data-stu-id="9c94e-624">A type-activated filter means:</span></span>

* <span data-ttu-id="9c94e-625">系統會針對每個要求建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-625">An instance is created for each request.</span></span>
* <span data-ttu-id="9c94e-626">任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。</span><span class="sxs-lookup"><span data-stu-id="9c94e-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="9c94e-627">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="9c94e-628">DI 無法提供建構函式相依性，因為：</span><span class="sxs-lookup"><span data-stu-id="9c94e-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="9c94e-629">必須在提供屬性之建構函式參數的地方提供它們。</span><span class="sxs-lookup"><span data-stu-id="9c94e-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="9c94e-630">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="9c94e-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="9c94e-631">下列篩選條件支援由 DI 所提供的建構函式相依性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="9c94e-632">在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="9c94e-633">上述篩選條件可以被套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="9c94e-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="9c94e-634">DI 會提供記錄器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-634">Loggers are available from DI.</span></span> <span data-ttu-id="9c94e-635">不過，請避免僅針對記錄目的建立並使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="9c94e-636">[內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。</span><span class="sxs-lookup"><span data-stu-id="9c94e-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="9c94e-637">新增至篩選條件的記錄：</span><span class="sxs-lookup"><span data-stu-id="9c94e-637">Logging added to filters:</span></span>

* <span data-ttu-id="9c94e-638">應該專注在篩選條件特定的商務領域考量或行為。</span><span class="sxs-lookup"><span data-stu-id="9c94e-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="9c94e-639">**不**應該記錄動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="9c94e-640">內建篩選條件能記錄動作和架構事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="9c94e-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="9c94e-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="9c94e-642">服務篩選條件實作類型會註冊在 `ConfigureServices` 中。</span><span class="sxs-lookup"><span data-stu-id="9c94e-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="9c94e-643"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="9c94e-644">下面程式碼會說明 `AddHeaderResultServiceFilter`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="9c94e-645">在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="9c94e-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="9c94e-646">在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：</span><span class="sxs-lookup"><span data-stu-id="9c94e-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="9c94e-647">使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="9c94e-648">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="9c94e-649">ASP.NET Core 執行階段並不保證：</span><span class="sxs-lookup"><span data-stu-id="9c94e-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="9c94e-650">將會建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="9c94e-651">將不會於稍後的時間從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="9c94e-652">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="9c94e-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="9c94e-654">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="9c94e-655">`CreateInstance` 會從 DI 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="9c94e-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="9c94e-656">TypeFilterAttribute</span></span>

<span data-ttu-id="9c94e-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="9c94e-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="9c94e-658">它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="9c94e-659">由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：</span><span class="sxs-lookup"><span data-stu-id="9c94e-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="9c94e-660">使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9c94e-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="9c94e-661">不過它們的相依性會由 DI 容器滿足。</span><span class="sxs-lookup"><span data-stu-id="9c94e-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="9c94e-662">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="9c94e-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="9c94e-663">使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="9c94e-664">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="9c94e-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="9c94e-665">ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="9c94e-666">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="9c94e-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="9c94e-667">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="9c94e-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="9c94e-668">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-668">Authorization filters</span></span>

<span data-ttu-id="9c94e-669">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-669">Authorization filters:</span></span>

* <span data-ttu-id="9c94e-670">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="9c94e-671">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="9c94e-671">Control access to action methods.</span></span>
* <span data-ttu-id="9c94e-672">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="9c94e-673">自訂授權篩選條件需要自訂的授權架構。</span><span class="sxs-lookup"><span data-stu-id="9c94e-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="9c94e-674">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="9c94e-675">內建的授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="9c94e-676">呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="9c94e-676">Calls the authorization system.</span></span>
* <span data-ttu-id="9c94e-677">不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="9c94e-677">Does not authorize requests.</span></span>

<span data-ttu-id="9c94e-678">**不會**在授權篩選條件內擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="9c94e-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="9c94e-679">該例外狀況將不會被處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-679">The exception will not be handled.</span></span>
* <span data-ttu-id="9c94e-680">例外狀況篩選條件將不會處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="9c94e-681">請考慮在例外狀況於授權篩選條件中發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="9c94e-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="9c94e-682">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="9c94e-683">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-683">Resource filters</span></span>

<span data-ttu-id="9c94e-684">資源篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-684">Resource filters:</span></span>

* <span data-ttu-id="9c94e-685">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="9c94e-686">執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="9c94e-687">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="9c94e-688">資源篩選條件很適合用來縮短大部分的管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="9c94e-689">比方說，快取篩選條件可以避免快取命中上的其餘管線。</span><span class="sxs-lookup"><span data-stu-id="9c94e-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="9c94e-690">資源篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="9c94e-690">Resource filter examples:</span></span>

* <span data-ttu-id="9c94e-691">先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="9c94e-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：</span><span class="sxs-lookup"><span data-stu-id="9c94e-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="9c94e-693">防止模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="9c94e-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="9c94e-694">用於大型檔案上傳，以避免將表單資料讀入記憶體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="9c94e-695">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c94e-696">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="9c94e-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="9c94e-697">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="9c94e-698">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="9c94e-699">動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-699">Action filters:</span></span>

* <span data-ttu-id="9c94e-700">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="9c94e-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="9c94e-701">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="9c94e-702">下列程式碼會顯示範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="9c94e-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="9c94e-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - 讓針對某個動作方法的輸入可以被讀取。</span><span class="sxs-lookup"><span data-stu-id="9c94e-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="9c94e-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="9c94e-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="9c94e-707">在動作方法中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="9c94e-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="9c94e-708">防止執行後續篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="9c94e-709">和設定 `Result` 不同，會被視為失敗而非成功結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="9c94e-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="9c94e-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="9c94e-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="9c94e-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="9c94e-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="9c94e-713">將此屬性設定為 Null：</span><span class="sxs-lookup"><span data-stu-id="9c94e-713">Setting this property to null:</span></span>

  * <span data-ttu-id="9c94e-714">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="9c94e-715">會執行 `Result`，有如它是從動作方法傳回一般。</span><span class="sxs-lookup"><span data-stu-id="9c94e-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="9c94e-716">對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：</span><span class="sxs-lookup"><span data-stu-id="9c94e-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="9c94e-717">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="9c94e-718">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="9c94e-719">若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="9c94e-720">架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="9c94e-721">`OnActionExecuting` 動作篩選條件可以用來：</span><span class="sxs-lookup"><span data-stu-id="9c94e-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="9c94e-722">驗證模型狀態。</span><span class="sxs-lookup"><span data-stu-id="9c94e-722">Validate model state.</span></span>
* <span data-ttu-id="9c94e-723">如果狀態無效，則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="9c94e-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="9c94e-724">`OnActionExecuted` 方法會在動作方法之後執行：</span><span class="sxs-lookup"><span data-stu-id="9c94e-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="9c94e-725">並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="9c94e-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。</span><span class="sxs-lookup"><span data-stu-id="9c94e-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="9c94e-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="9c94e-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="9c94e-728">將 `Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="9c94e-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="9c94e-729">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="9c94e-730">執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="9c94e-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="9c94e-731">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-731">Exception filters</span></span>

<span data-ttu-id="9c94e-732">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-732">Exception filters:</span></span>

* <span data-ttu-id="9c94e-733">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="9c94e-734">可以用來實作常見的錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="9c94e-735">下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="9c94e-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="9c94e-736">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-736">Exception filters:</span></span>

* <span data-ttu-id="9c94e-737">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-737">Don't have before and after events.</span></span>
* <span data-ttu-id="9c94e-738">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="9c94e-739">處理在 Razor 頁面或控制器建立、[模型繫結](xref:mvc/models/model-binding)、動作篩選條件或動作方法中發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="9c94e-740">**不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="9c94e-741">若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="9c94e-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="9c94e-742">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-742">This stops propagation of the exception.</span></span> <span data-ttu-id="9c94e-743">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="9c94e-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="9c94e-744">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="9c94e-744">Only an action filter can do that.</span></span>

<span data-ttu-id="9c94e-745">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-745">Exception filters:</span></span>

* <span data-ttu-id="9c94e-746">適合用於設陷在動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="9c94e-747">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="9c94e-748">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="9c94e-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="9c94e-749">只有在錯誤處理會根據所呼叫的動作方法而「不同」時才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="9c94e-750">比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="9c94e-751">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="9c94e-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="9c94e-752">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="9c94e-752">Result filters</span></span>

<span data-ttu-id="9c94e-753">結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-753">Result filters:</span></span>

* <span data-ttu-id="9c94e-754">實作介面：</span><span class="sxs-lookup"><span data-stu-id="9c94e-754">Implement an interface:</span></span>
  * <span data-ttu-id="9c94e-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="9c94e-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="9c94e-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="9c94e-757">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="9c94e-758">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="9c94e-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="9c94e-759">以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="9c94e-760">執行的結果類型會取決於動作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="9c94e-761">傳回檢視的動作會在執行中的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 裡包含處理中的所有 Razor。</span><span class="sxs-lookup"><span data-stu-id="9c94e-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="9c94e-762">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="9c94e-763">深入瞭解[動作結果](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="9c94e-764">只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。</span><span class="sxs-lookup"><span data-stu-id="9c94e-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="9c94e-765">當下列情況時，不會執行結果篩選：</span><span class="sxs-lookup"><span data-stu-id="9c94e-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="9c94e-766">授權篩選或資源篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="9c94e-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="9c94e-767">例外狀況篩選條件會產生動作結果來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="9c94e-768">藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> 設為 `true`，<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="9c94e-769">在縮短時寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="9c94e-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="9c94e-770">在 `IResultFilter.OnResultExecuting` 中擲回例外狀況會：</span><span class="sxs-lookup"><span data-stu-id="9c94e-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="9c94e-771">導致無法執行動作結果和後續的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="9c94e-772">視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="9c94e-773">當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="9c94e-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="9c94e-774">如果回應已傳送給用戶端，則無法進一步變更。</span><span class="sxs-lookup"><span data-stu-id="9c94e-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="9c94e-775">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="9c94e-776">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="9c94e-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="9c94e-777">將 `Exception` 設為 Null 實際上會「處理」例外狀況，並且使 ASP.NET Core 稍後不會在管線中重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c94e-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="9c94e-778">並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。</span><span class="sxs-lookup"><span data-stu-id="9c94e-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="9c94e-779">當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="9c94e-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="9c94e-780">針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 上的 `await next` 的呼叫會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="9c94e-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="9c94e-781">若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="9c94e-782">架構提供抽象 `ResultFilterAttribute`，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="9c94e-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="9c94e-783">先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="9c94e-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="9c94e-784">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="9c94e-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="9c94e-785"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="9c94e-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="9c94e-786">這包括由產生的動作結果：</span><span class="sxs-lookup"><span data-stu-id="9c94e-786">This includes action results produced by:</span></span>

* <span data-ttu-id="9c94e-787">最少迴圈的授權篩選準則和資源篩選器。</span><span class="sxs-lookup"><span data-stu-id="9c94e-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="9c94e-788">例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="9c94e-788">Exception filters.</span></span>

<span data-ttu-id="9c94e-789">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="9c94e-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="9c94e-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="9c94e-790">IFilterFactory</span></span>

<span data-ttu-id="9c94e-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="9c94e-792">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="9c94e-793">當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="9c94e-794">如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="9c94e-795">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="9c94e-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="9c94e-796">可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：</span><span class="sxs-lookup"><span data-stu-id="9c94e-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="9c94e-797">上述程式碼可以透過執行[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) 來進行測試：</span><span class="sxs-lookup"><span data-stu-id="9c94e-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="9c94e-798">叫用 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="9c94e-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="9c94e-799">巡覽至 `https://localhost:5001/Sample/HeaderWithFactory`。</span><span class="sxs-lookup"><span data-stu-id="9c94e-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="9c94e-800">F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：</span><span class="sxs-lookup"><span data-stu-id="9c94e-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="9c94e-801">**作者：** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="9c94e-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="9c94e-802">**globaladdheader：** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="9c94e-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="9c94e-803">**內部：** `My header`</span><span class="sxs-lookup"><span data-stu-id="9c94e-803">**internal:** `My header`</span></span>

<span data-ttu-id="9c94e-804">上述程式碼會建立**internal：** `My header` 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="9c94e-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="9c94e-805">在屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="9c94e-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="9c94e-806">實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="9c94e-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="9c94e-807">不需要傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="9c94e-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="9c94e-808">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="9c94e-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="9c94e-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="9c94e-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="9c94e-810">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="9c94e-811">`CreateInstance` 會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="9c94e-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="9c94e-812">下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：</span><span class="sxs-lookup"><span data-stu-id="9c94e-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="9c94e-813">在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="9c94e-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="9c94e-814">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="9c94e-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="9c94e-815">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="9c94e-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="9c94e-816">但篩選條件與中介軟體不同之處在於它們是 ASP.NET Core 執行階段的一部分，這表示它們能存取 ASP.NET Core 內容和建構。</span><span class="sxs-lookup"><span data-stu-id="9c94e-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="9c94e-817">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9c94e-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="9c94e-818">以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="9c94e-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="9c94e-819">使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="9c94e-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="9c94e-820">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="9c94e-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="9c94e-821">後續動作</span><span class="sxs-lookup"><span data-stu-id="9c94e-821">Next actions</span></span>

* <span data-ttu-id="9c94e-822">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="9c94e-823">若要嘗試使用篩選條件，請[下載、測試及修改 GitHub 範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="9c94e-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
