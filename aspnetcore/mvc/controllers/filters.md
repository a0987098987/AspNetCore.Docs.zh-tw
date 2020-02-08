---
title: ASP.NET Core 中的篩選條件
author: Rick-Anderson
description: 了解篩選條件的運作方式，以及如何在 ASP.NET Core 中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
uid: mvc/controllers/filters
ms.openlocfilehash: c4bb9d5746e494106ead6ad5bbf972bbcc5a39f1
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034061"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="f8ff1-103">ASP.NET Core 中的篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8ff1-104">作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="f8ff1-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f8ff1-105">ASP.NET Core 中的「篩選條件」可讓程式碼在要求處理管線中的特定階段之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="f8ff1-106">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="f8ff1-107">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="f8ff1-108">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="f8ff1-109">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="f8ff1-110">跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="f8ff1-111">篩選能避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="f8ff1-112">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="f8ff1-113">本文件適用於 Razor Pages、API 控制器，以及具有檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span> <span data-ttu-id="f8ff1-114">篩選器無法直接與[Razor 元件](xref:blazor/components)搭配使用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-114">Filters don't work directly with [Razor components](xref:blazor/components).</span></span> <span data-ttu-id="f8ff1-115">篩選器只會在下列情況時間接影響元件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-115">A filter can only indirectly affect a component when:</span></span>

* <span data-ttu-id="f8ff1-116">此元件內嵌在頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-116">The component is embedded in a page or view.</span></span>
* <span data-ttu-id="f8ff1-117">頁面或控制器/視圖會使用篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-117">The page or controller/view uses the filter.</span></span>

<span data-ttu-id="f8ff1-118">[檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-118">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="f8ff1-119">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-119">How filters work</span></span>

<span data-ttu-id="f8ff1-120">篩選條件會在「ASP.NET Core 動作引動過程管線」中執行，其有時也被稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-120">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span> <span data-ttu-id="f8ff1-121">在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-121">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![此要求會透過其他中介軟體、路由中介軟體、動作選取和動作調用管線來處理。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="f8ff1-124">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="f8ff1-124">Filter types</span></span>

<span data-ttu-id="f8ff1-125">每個篩選類型會在篩選條件管線中的不同階段執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-125">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="f8ff1-126">[授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-126">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="f8ff1-127">如果要求未獲授權，授權篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-127">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="f8ff1-128">[資源篩選條件](#resource-filters)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-128">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="f8ff1-129">會在授權之後執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-129">Run after authorization.</span></span>  
  * <span data-ttu-id="f8ff1-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 在篩選器管線的其餘部分之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="f8ff1-131">例如，`OnResourceExecuting` 會在模型系結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-131">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="f8ff1-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 在完成管線的其餘部分之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="f8ff1-133">[動作篩選條件](#action-filters)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-133">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="f8ff1-134">在呼叫動作方法之前和之後，立即執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-134">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="f8ff1-135">可以變更傳遞至動作的引數。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-135">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="f8ff1-136">可以變更從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-136">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="f8ff1-137">Razor Pages 中**不**支援。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-137">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="f8ff1-138">[例外狀況篩選準則](#exception-filters)會將全域原則套用至在寫入回應主體之前發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-138">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="f8ff1-139">[結果篩選準則](#result-filters)會在動作結果執行前後立即執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-139">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="f8ff1-140">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-140">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="f8ff1-141">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-141">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="f8ff1-142">下圖顯示篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-142">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="f8ff1-145">實作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-145">Implementation</span></span>

<span data-ttu-id="f8ff1-146">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-146">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="f8ff1-147">同步篩選會在其管線階段之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-147">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="f8ff1-148">例如，系統會在呼叫動作方法之前呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-148">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="f8ff1-149">系統會在傳回動作方法之後呼叫 <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f8ff1-150">非同步篩選會定義 `On-Stage-ExecutionAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-150">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="f8ff1-151">例如，<xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-151">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="f8ff1-152">在上述程式碼中，`SampleAsyncActionFilter` 具有執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> （`next`）。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-152">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="f8ff1-153">多個篩選條件階段</span><span class="sxs-lookup"><span data-stu-id="f8ff1-153">Multiple filter stages</span></span>

<span data-ttu-id="f8ff1-154">可以在單一類別中實作多個篩選條件階段的介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-154">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="f8ff1-155">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-155">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="f8ff1-156">同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-156">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="f8ff1-157">非同步： <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-157">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="f8ff1-158">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-158">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="f8ff1-159">執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-159">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="f8ff1-160">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-160">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="f8ff1-161">如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-161">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="f8ff1-162">使用抽象類別（例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>）時，只會覆寫同步方法或每個篩選準則類型的非同步方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-162">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="f8ff1-163">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="f8ff1-163">Built-in filter attributes</span></span>

<span data-ttu-id="f8ff1-164">ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-164">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="f8ff1-165">例如，下列結果篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-165">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-166">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-166">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="f8ff1-167">將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-167">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="f8ff1-168">使用[瀏覽器開發人員工具](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools)之類的工具來檢查標頭。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-168">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="f8ff1-169">在 [**回應標頭**] 底下，會顯示 `author: Rick Anderson`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-169">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="f8ff1-170">下列程式碼會實 `ActionFilterAttribute`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-170">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="f8ff1-171">讀取設定系統中的標題和名稱。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-171">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="f8ff1-172">與先前的範例不同的是，下列程式碼不需要將篩選參數新增至程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-172">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="f8ff1-173">將標題和名稱新增至回應標頭。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-173">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-174">設定選項是使用[選項模式](xref:fundamentals/configuration/options)從設定[系統](xref:fundamentals/configuration/index)提供。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-174">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="f8ff1-175">例如，從*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-175">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="f8ff1-176">在 `StartUp.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-176">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="f8ff1-177">`PositionOptions` 類別會使用 `"Position"` 設定區域新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-177">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="f8ff1-178">`MyActionFilterAttribute` 會新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-178">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="f8ff1-179">下列程式碼顯示 `PositionOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-179">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="f8ff1-180">下列程式碼會將 `MyActionFilterAttribute` 套用至 `Index2` 方法：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-180">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="f8ff1-181">當呼叫 `Sample/Index2` 端點時，會顯示 [**回應標頭**]、[`author: Rick Anderson`] 和 [`Editor: Joe Smith`]。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-181">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="f8ff1-182">下列程式碼會將 `MyActionFilterAttribute` 和 `AddHeaderAttribute` 套用至 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-182">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="f8ff1-183">無法將篩選套用至 Razor 頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-183">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="f8ff1-184">它們可以套用至 Razor 頁面模型或全域套用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-184">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="f8ff1-185">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-185">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="f8ff1-186">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-186">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="f8ff1-187">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-187">Filter scopes and order of execution</span></span>

<span data-ttu-id="f8ff1-188">篩選條件能以三個「範圍」之一新增至管線：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-188">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="f8ff1-189">在控制器動作上使用屬性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-189">Using an attribute on a controller action.</span></span> <span data-ttu-id="f8ff1-190">無法將篩選屬性套用至 Razor Pages 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-190">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="f8ff1-191">使用控制器或 Razor 頁面上的屬性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-191">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="f8ff1-192">全域適用于所有的控制器、動作和 Razor Pages，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-192">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="f8ff1-193">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-193">Default order of execution</span></span>

<span data-ttu-id="f8ff1-194">當管線的特定階段有多個篩選條件時，範圍會決定篩選條件的預設執行順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-194">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="f8ff1-195">全域篩選條件會圍繞類別篩選條件，後者又圍繞方法篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-195">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="f8ff1-196">因為篩選條件巢狀結構的原因，篩選條件的「之後」程式碼的執行順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-196">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="f8ff1-197">篩選條件序列：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-197">The filter sequence:</span></span>

* <span data-ttu-id="f8ff1-198">全域篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-198">The *before* code of global filters.</span></span>
  * <span data-ttu-id="f8ff1-199">控制站的*程式碼和*Razor 頁面篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-199">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="f8ff1-200">動作方法篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-200">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="f8ff1-201">動作方法篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-201">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="f8ff1-202">控制器和 Razor 頁面篩選*後*的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-202">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="f8ff1-203">全域篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-203">The *after* code of global filters.</span></span>
  
<span data-ttu-id="f8ff1-204">下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-204">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="f8ff1-205">順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-205">Sequence</span></span> | <span data-ttu-id="f8ff1-206">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="f8ff1-206">Filter scope</span></span> | <span data-ttu-id="f8ff1-207">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-207">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="f8ff1-208">1</span><span class="sxs-lookup"><span data-stu-id="f8ff1-208">1</span></span> | <span data-ttu-id="f8ff1-209">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-209">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-210">2</span><span class="sxs-lookup"><span data-stu-id="f8ff1-210">2</span></span> | <span data-ttu-id="f8ff1-211">控制器或 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f8ff1-211">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="f8ff1-212">3</span><span class="sxs-lookup"><span data-stu-id="f8ff1-212">3</span></span> | <span data-ttu-id="f8ff1-213">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-213">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-214">4</span><span class="sxs-lookup"><span data-stu-id="f8ff1-214">4</span></span> | <span data-ttu-id="f8ff1-215">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-215">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-216">5</span><span class="sxs-lookup"><span data-stu-id="f8ff1-216">5</span></span> | <span data-ttu-id="f8ff1-217">控制器或 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f8ff1-217">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-218">6</span><span class="sxs-lookup"><span data-stu-id="f8ff1-218">6</span></span> | <span data-ttu-id="f8ff1-219">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-219">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="f8ff1-220">控制器層級篩選</span><span class="sxs-lookup"><span data-stu-id="f8ff1-220">Controller level filters</span></span>

<span data-ttu-id="f8ff1-221">繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-221">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="f8ff1-222">這些方法會：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-222">These methods:</span></span>

* <span data-ttu-id="f8ff1-223">包裝針對指定動作執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-223">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="f8ff1-224">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-224">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="f8ff1-225">系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-225">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="f8ff1-226">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-226">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="f8ff1-227">在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-227">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="f8ff1-228">例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-228">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="f8ff1-229">`TestController`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-229">The `TestController`:</span></span>

* <span data-ttu-id="f8ff1-230">將 `SampleActionFilterAttribute` （`[SampleActionFilter]`）套用至 `FilterTest2` 動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-230">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="f8ff1-231">覆寫 `OnActionExecuting` 和 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-231">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="f8ff1-232">瀏覽至 `https://localhost:5001/Test2/FilterTest2` 執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-232">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="f8ff1-233">控制器層級篩選會將[Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-233">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="f8ff1-234">在套用至方法的篩選器之後，**無法**將控制器層級篩選設定為執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-234">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="f8ff1-235">下一節將說明順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-235">Order is explained in the next section.</span></span>

<span data-ttu-id="f8ff1-236">針對 Razor Pages，請參閱[覆寫篩選條件方法來實作 Razor 頁面篩選條件](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-236">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="f8ff1-237">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-237">Overriding the default order</span></span>

<span data-ttu-id="f8ff1-238">可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-238">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="f8ff1-239">`IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-239">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="f8ff1-240">具有較低 `Order` 值的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-240">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="f8ff1-241">在具有較高  *值的篩選條件之前執行「之前」* `Order`程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-241">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="f8ff1-242">在具有較高  *值的篩選條件之後執行「之後」* `Order`程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-242">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="f8ff1-243">`Order` 屬性是以「函式參數設定：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-243">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="f8ff1-244">請考慮下列控制器中的兩個動作篩選準則：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-244">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="f8ff1-245">全域篩選準則會在 `StartUp.ConfigureServices`中新增：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-245">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="f8ff1-246">3個篩選準則會依照下列循序執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-246">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="f8ff1-247">`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-247">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="f8ff1-248">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-248">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="f8ff1-249">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-249">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="f8ff1-250">如先前所述，控制器層級篩選會針對內建篩選準則將[order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17)屬性設定為 `int.MinValue`，除非 `Order` 設定為非零值，否則範圍會決定訂單。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-250">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="f8ff1-251">在上述程式碼中，`MySampleActionFilter` 具有全域範圍，因此它會在具有控制器範圍的 `MyAction2FilterAttribute`之前執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-251">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="f8ff1-252">若要讓 `MyAction2FilterAttribute` 先執行，請將順序設定為 `int.MinValue`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-252">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="f8ff1-253">若要讓全域篩選 `MySampleActionFilter` 先執行，請將 `Order` 設定為 `int.MinValue`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-253">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="f8ff1-254">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="f8ff1-254">Cancellation and short-circuiting</span></span>

<span data-ttu-id="f8ff1-255">可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 屬性，以縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-255">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="f8ff1-256">比方說，下列資源篩選條件可防止管線的其餘部分執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-256">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-257">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-257">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="f8ff1-258">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-258">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="f8ff1-259">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-259">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="f8ff1-260">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-260">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="f8ff1-261">因此，`AddHeader` 動作的 `SomeResource` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-261">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="f8ff1-262">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-262">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="f8ff1-263">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-263">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="f8ff1-264">相依性插入</span><span class="sxs-lookup"><span data-stu-id="f8ff1-264">Dependency injection</span></span>

<span data-ttu-id="f8ff1-265">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-265">Filters can be added by type or by instance.</span></span> <span data-ttu-id="f8ff1-266">如果新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-266">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="f8ff1-267">如果新增類型，其將會由類型啟動。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-267">If a type is added, it's type-activated.</span></span> <span data-ttu-id="f8ff1-268">由類型啟動的篩選條件表示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-268">A type-activated filter means:</span></span>

* <span data-ttu-id="f8ff1-269">系統會針對每個要求建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-269">An instance is created for each request.</span></span>
* <span data-ttu-id="f8ff1-270">任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-270">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="f8ff1-271">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-271">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="f8ff1-272">DI 無法提供建構函式相依性，因為：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-272">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="f8ff1-273">必須在提供屬性之建構函式參數的地方提供它們。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-273">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="f8ff1-274">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-274">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="f8ff1-275">下列篩選條件支援由 DI 所提供的建構函式相依性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-275">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="f8ff1-276">在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="f8ff1-277">上述篩選條件可以被套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-277">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="f8ff1-278">DI 會提供記錄器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-278">Loggers are available from DI.</span></span> <span data-ttu-id="f8ff1-279">不過，請避免僅針對記錄目的建立並使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-279">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="f8ff1-280">[內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-280">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="f8ff1-281">新增至篩選條件的記錄：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-281">Logging added to filters:</span></span>

* <span data-ttu-id="f8ff1-282">應該專注在篩選條件特定的商務領域考量或行為。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-282">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="f8ff1-283">**不**應該記錄動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-283">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="f8ff1-284">內建篩選記錄動作和架構事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-284">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="f8ff1-285">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f8ff1-285">ServiceFilterAttribute</span></span>

<span data-ttu-id="f8ff1-286">服務篩選條件實作類型會註冊在 `ConfigureServices` 中。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-286">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="f8ff1-287"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-287">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="f8ff1-288">下面程式碼會說明 `AddHeaderResultServiceFilter`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-288">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f8ff1-289">在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-289">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="f8ff1-290">在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-290">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="f8ff1-291">使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-291">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="f8ff1-292">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-292">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f8ff1-293">ASP.NET Core 執行階段並不保證：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-293">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="f8ff1-294">將會建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-294">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="f8ff1-295">將不會於稍後的時間從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-295">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="f8ff1-296">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-296">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="f8ff1-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f8ff1-298">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-298">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f8ff1-299">`CreateInstance` 會從 DI 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-299">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="f8ff1-300">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f8ff1-300">TypeFilterAttribute</span></span>

<span data-ttu-id="f8ff1-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="f8ff1-302">它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-302">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="f8ff1-303">由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-303">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="f8ff1-304">使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-304">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="f8ff1-305">不過它們的相依性會由 DI 容器滿足。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-305">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="f8ff1-306">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-306">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="f8ff1-307">使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-307">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="f8ff1-308">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-308">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f8ff1-309">ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-309">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="f8ff1-310">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-310">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="f8ff1-311">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-311">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="f8ff1-312">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-312">Authorization filters</span></span>

<span data-ttu-id="f8ff1-313">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-313">Authorization filters:</span></span>

* <span data-ttu-id="f8ff1-314">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-314">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="f8ff1-315">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-315">Control access to action methods.</span></span>
* <span data-ttu-id="f8ff1-316">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-316">Have a before method, but no after method.</span></span>

<span data-ttu-id="f8ff1-317">自訂授權篩選條件需要自訂的授權架構。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-317">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="f8ff1-318">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-318">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="f8ff1-319">內建的授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-319">The built-in authorization filter:</span></span>

* <span data-ttu-id="f8ff1-320">呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-320">Calls the authorization system.</span></span>
* <span data-ttu-id="f8ff1-321">不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-321">Does not authorize requests.</span></span>

<span data-ttu-id="f8ff1-322">**不會**在授權篩選條件內擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-322">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="f8ff1-323">該例外狀況將不會被處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-323">The exception will not be handled.</span></span>
* <span data-ttu-id="f8ff1-324">例外狀況篩選條件將不會處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-324">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="f8ff1-325">請考慮在例外狀況於授權篩選條件中發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-325">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="f8ff1-326">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-326">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="f8ff1-327">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-327">Resource filters</span></span>

<span data-ttu-id="f8ff1-328">資源篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-328">Resource filters:</span></span>

* <span data-ttu-id="f8ff1-329">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-329">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="f8ff1-330">執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-330">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="f8ff1-331">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-331">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="f8ff1-332">資源篩選條件很適合用來縮短大部分的管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-332">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="f8ff1-333">比方說，快取篩選條件可以避免快取命中上的其餘管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-333">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="f8ff1-334">資源篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-334">Resource filter examples:</span></span>

* <span data-ttu-id="f8ff1-335">先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-335">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="f8ff1-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="f8ff1-337">防止模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-337">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="f8ff1-338">用於大型檔案上傳，以避免將表單資料讀入記憶體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-338">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="f8ff1-339">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-339">Action filters</span></span>

<span data-ttu-id="f8ff1-340">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-340">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="f8ff1-341">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-341">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="f8ff1-342">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-342">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="f8ff1-343">動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-343">Action filters:</span></span>

* <span data-ttu-id="f8ff1-344">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-344">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="f8ff1-345">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-345">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="f8ff1-346">下列程式碼會顯示範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-346">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f8ff1-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="f8ff1-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-啟用讀取動作方法的輸入。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="f8ff1-349"><xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-349"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="f8ff1-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="f8ff1-351">在動作方法中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-351">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="f8ff1-352">防止執行後續篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-352">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="f8ff1-353">和設定 `Result` 不同，會被視為失敗而非成功結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-353">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f8ff1-354"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-354">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="f8ff1-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f8ff1-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="f8ff1-357">將此屬性設定為 Null：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-357">Setting this property to null:</span></span>

  * <span data-ttu-id="f8ff1-358">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-358">Effectively handles the exception.</span></span>
  * <span data-ttu-id="f8ff1-359">會執行 `Result`，有如它是從動作方法傳回一般。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-359">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="f8ff1-360">對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-360">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="f8ff1-361">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-361">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="f8ff1-362">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-362">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="f8ff1-363">若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-363">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="f8ff1-364">架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-364">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="f8ff1-365">`OnActionExecuting` 動作篩選條件可以用來：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-365">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="f8ff1-366">驗證模型狀態。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-366">Validate model state.</span></span>
* <span data-ttu-id="f8ff1-367">如果狀態無效，則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-367">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-368">`OnActionExecuted` 方法會在動作方法之後執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-368">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="f8ff1-369">並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-369">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="f8ff1-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f8ff1-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="f8ff1-372">將 `Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-372">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="f8ff1-373">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-373">Effectively handles an exception.</span></span>
  * <span data-ttu-id="f8ff1-374">執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-374">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="f8ff1-375">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-375">Exception filters</span></span>

<span data-ttu-id="f8ff1-376">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-376">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-377">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-377">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="f8ff1-378">可以用來實作常見的錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-378">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="f8ff1-379">下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-379">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="f8ff1-380">下列程式碼會測試例外狀況篩選準則：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-380">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="f8ff1-381">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-381">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-382">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-382">Don't have before and after events.</span></span>
* <span data-ttu-id="f8ff1-383">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-383">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="f8ff1-384">處理在 Razor 頁面或控制器建立、[模型繫結](xref:mvc/models/model-binding)、動作篩選條件或動作方法中發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-384">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="f8ff1-385">**不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-385">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="f8ff1-386">若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-386">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="f8ff1-387">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-387">This stops propagation of the exception.</span></span> <span data-ttu-id="f8ff1-388">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-388">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="f8ff1-389">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-389">Only an action filter can do that.</span></span>

<span data-ttu-id="f8ff1-390">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-390">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-391">適合用於設陷在動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-391">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="f8ff1-392">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-392">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="f8ff1-393">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-393">Prefer middleware for exception handling.</span></span> <span data-ttu-id="f8ff1-394">只有在錯誤處理會根據所呼叫的動作方法而「不同」時才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-394">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="f8ff1-395">比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-395">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="f8ff1-396">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-396">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="f8ff1-397">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-397">Result filters</span></span>

<span data-ttu-id="f8ff1-398">結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-398">Result filters:</span></span>

* <span data-ttu-id="f8ff1-399">實作介面：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-399">Implement an interface:</span></span>
  * <span data-ttu-id="f8ff1-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="f8ff1-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="f8ff1-402">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-402">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="f8ff1-403">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="f8ff1-403">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="f8ff1-404">以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-404">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f8ff1-405">執行的結果類型會取決於動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-405">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="f8ff1-406">傳回視圖的動作包含所有 razor 處理，做為執行 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 的一部分。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-406">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="f8ff1-407">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-407">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="f8ff1-408">深入瞭解[動作結果](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-408">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="f8ff1-409">只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-409">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="f8ff1-410">當下列情況時，不會執行結果篩選：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-410">Result filters are not executed when:</span></span>

* <span data-ttu-id="f8ff1-411">授權篩選或資源篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-411">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="f8ff1-412">例外狀況篩選條件會產生動作結果來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-412">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="f8ff1-413">藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 設為 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName>，`true` 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-413">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="f8ff1-414">在縮短時寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-414">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="f8ff1-415">在 `IResultFilter.OnResultExecuting`中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-415">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="f8ff1-416">防止執行動作結果和後續的篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-416">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="f8ff1-417">會被視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-417">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f8ff1-418">當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-418">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="f8ff1-419">如果回應已經傳送到用戶端，就無法變更。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-419">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="f8ff1-420">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-420">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="f8ff1-421">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-421">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="f8ff1-422">將 `Exception` 設定為 null 會有效率地處理例外狀況，並防止稍後在管線中擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-422">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="f8ff1-423">並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-423">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="f8ff1-424">當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-424">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="f8ff1-425">針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 `await next` 上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 的呼叫會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-425">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="f8ff1-426">若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-426">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="f8ff1-427">架構提供抽象 `ResultFilterAttribute`，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-427">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="f8ff1-428">先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-428">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="f8ff1-429">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="f8ff1-429">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="f8ff1-430"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-430">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="f8ff1-431">這包括由產生的動作結果：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-431">This includes action results produced by:</span></span>

* <span data-ttu-id="f8ff1-432">最少迴圈的授權篩選準則和資源篩選器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-432">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="f8ff1-433">例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-433">Exception filters.</span></span>

<span data-ttu-id="f8ff1-434">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-434">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="f8ff1-435">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="f8ff1-435">IFilterFactory</span></span>

<span data-ttu-id="f8ff1-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="f8ff1-437">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-437">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="f8ff1-438">當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-438">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="f8ff1-439">如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-439">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="f8ff1-440">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-440">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="f8ff1-441">可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-441">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="f8ff1-442">此篩選準則會套用至下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-442">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="f8ff1-443">執行[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)來測試上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-443">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="f8ff1-444">叫用 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-444">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="f8ff1-445">瀏覽至 `https://localhost:5001/Sample/HeaderWithFactory`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-445">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="f8ff1-446">F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-446">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="f8ff1-447">**作者：** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-447">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="f8ff1-448">**globaladdheader：** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="f8ff1-449">**內部：** `My header`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-449">**internal:** `My header`</span></span>

<span data-ttu-id="f8ff1-450">上述程式碼會建立**internal：** `My header` 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-450">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="f8ff1-451">在屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="f8ff1-451">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="f8ff1-452">實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-452">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="f8ff1-453">不需要傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-453">Don't require passing parameters.</span></span>
* <span data-ttu-id="f8ff1-454">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-454">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="f8ff1-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f8ff1-456">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-456">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f8ff1-457">`CreateInstance` 會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-457">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="f8ff1-458">下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-458">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="f8ff1-459">在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-459">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="f8ff1-460">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="f8ff1-460">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="f8ff1-461">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-461">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="f8ff1-462">但是篩選器與中介軟體不同之處在于它們是執行時間的一部分，這表示它們可以存取內容和結構。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-462">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="f8ff1-463">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-463">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="f8ff1-464">以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-464">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="f8ff1-465">使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-465">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="f8ff1-466">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-466">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="f8ff1-467">後續動作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-467">Next actions</span></span>

* <span data-ttu-id="f8ff1-468">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-468">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="f8ff1-469">若要嘗試使用篩選條件，請[下載、測試及修改 GitHub 範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-469">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8ff1-470">作者：[Kirk Larkin](https://github.com/serpent5) \(英文\)、[Rick Anderson](https://twitter.com/RickAndMSFT) \(英文\)、[Tom Dykstra](https://github.com/tdykstra/) \(英文\) 及 [Steve Smith](https://ardalis.com/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="f8ff1-470">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f8ff1-471">ASP.NET Core 中的「篩選條件」可讓程式碼在要求處理管線中的特定階段之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-471">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="f8ff1-472">內建篩選條件會處理工作，例如：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-472">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="f8ff1-473">授權 (避免存取使用者未獲授權的資源)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-473">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="f8ff1-474">回應快取 (縮短要求管線，傳回快取的回應)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-474">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="f8ff1-475">可以建立自訂篩選條件來處理跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-475">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="f8ff1-476">跨領域關注的範例包括錯誤處理、快取、設定、授權及記錄。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-476">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="f8ff1-477">篩選能避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-477">Filters avoid duplicating code.</span></span> <span data-ttu-id="f8ff1-478">例如，錯誤處理例外狀況篩選條件中可以合併錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-478">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="f8ff1-479">本文件適用於 Razor Pages、API 控制器，以及具有檢視的控制器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-479">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="f8ff1-480">[檢視或下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-480">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="f8ff1-481">篩選條件如何運作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-481">How filters work</span></span>

<span data-ttu-id="f8ff1-482">篩選條件會在「ASP.NET Core 動作引動過程管線」中執行，其有時也被稱為「篩選條件管線」。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-482">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="f8ff1-483">在 ASP.NET Core 之後執行的篩選條件管線會選取要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-483">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![要求處理會歷經其他中介軟體、路由中介軟體、動作選取和 ASP.NET Core 動作引動過程管線。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="f8ff1-486">篩選條件類型</span><span class="sxs-lookup"><span data-stu-id="f8ff1-486">Filter types</span></span>

<span data-ttu-id="f8ff1-487">每個篩選類型會在篩選條件管線中的不同階段執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-487">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="f8ff1-488">[授權篩選條件](#authorization-filters)會先執行，用來判斷使用者是否已針對要求取得授權。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-488">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="f8ff1-489">如果要求未經授權，授權篩選條件便會縮短管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-489">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="f8ff1-490">[資源篩選條件](#resource-filters)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-490">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="f8ff1-491">會在授權之後執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-491">Run after authorization.</span></span>  
  * <span data-ttu-id="f8ff1-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> 可以在其餘的篩選條件管線之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="f8ff1-493">例如，`OnResourceExecuting` 可以在模型繫結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-493">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="f8ff1-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> 可以在其餘的管線皆已完成之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="f8ff1-495">[動作篩選條件](#action-filters)可以緊接在呼叫個別動作方法之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-495">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="f8ff1-496">它們可以用來處理傳遞至動作的引數，和從動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-496">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="f8ff1-497">Razor Pages 中**不**支援動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-497">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="f8ff1-498">[例外狀況篩選條件](#exception-filters)用來將通用原則套用到在任何項目寫入回應主體之前，發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-498">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="f8ff1-499">[結果篩選條件](#result-filters)可以緊接在執行個別動作結果之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-499">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="f8ff1-500">它們只有在動作方法執行成功時才執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-500">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="f8ff1-501">它們適用於必須包圍檢視或格式器執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-501">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="f8ff1-502">下圖顯示篩選條件類型如何在篩選條件管線中互動。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-502">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![要求處理會歷經授權篩選條件、資源篩選條件、模型繫結、動作篩選條件、動作執行和動作結果轉換、例外狀況篩選條件、結果篩選條件，以及結果執行。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="f8ff1-505">實作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-505">Implementation</span></span>

<span data-ttu-id="f8ff1-506">篩選條件同時支援透過不同介面定義的同步和非同步實作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-506">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="f8ff1-507">同步篩選條件可以在其管線階段之前 (`On-Stage-Executing`) 和之後 (`On-Stage-Executed`) 執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-507">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="f8ff1-508">例如，系統會在呼叫動作方法之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-508">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="f8ff1-509">系統會在傳回動作方法之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-509">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f8ff1-510">非同步篩選條件會定義 `On-Stage-ExecutionAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-510">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="f8ff1-511">在上述程式碼中，`SampleAsyncActionFilter` 具有會執行動作方法的 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-511">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="f8ff1-512">每個 `On-Stage-ExecutionAsync` 都會接受能執行篩選條件之管線階段的 `FilterType-ExecutionDelegate`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-512">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="f8ff1-513">多個篩選條件階段</span><span class="sxs-lookup"><span data-stu-id="f8ff1-513">Multiple filter stages</span></span>

<span data-ttu-id="f8ff1-514">可以在單一類別中實作多個篩選條件階段的介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-514">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="f8ff1-515">例如，<xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> 類別會實作 `IActionFilter`、`IResultFilter` 與其非同步對等項目。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-515">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="f8ff1-516">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-516">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="f8ff1-517">執行階段會先檢查以查看篩選條件是否會實作非同步介面，如果是，便會呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-517">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="f8ff1-518">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-518">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="f8ff1-519">如果同時在單一類別中實作非同步和同步介面，系統只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-519">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="f8ff1-520">使用抽象類別 (例如 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>) 時，請僅覆寫每個篩選條件類型的同步方法或非同步方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-520">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="f8ff1-521">內建篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="f8ff1-521">Built-in filter attributes</span></span>

<span data-ttu-id="f8ff1-522">ASP.NET Core 包含內建的屬性型篩選條件，可對其進行子類別化和自訂。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-522">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="f8ff1-523">例如，下列結果篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-523">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-524">屬性可讓篩選條件接受引數，如上述範例所示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-524">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="f8ff1-525">將 `AddHeaderAttribute` 新增至控制器或動作方法，並指定 HTTP 標頭的名稱和值：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-525">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="f8ff1-526">有幾個篩選條件介面有對應的屬性，可用來作為自訂實作的基底類別。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-526">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="f8ff1-527">篩選條件屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-527">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="f8ff1-528">篩選條件範圍和執行的順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-528">Filter scopes and order of execution</span></span>

<span data-ttu-id="f8ff1-529">篩選條件能以三個「範圍」之一新增至管線：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-529">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="f8ff1-530">針對動作使用屬性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-530">Using an attribute on an action.</span></span>
* <span data-ttu-id="f8ff1-531">針對控制器使用屬性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-531">Using an attribute on a controller.</span></span>
* <span data-ttu-id="f8ff1-532">全域地針對所有控制器和動作，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-532">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="f8ff1-533">上述程式碼會使用 [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) 集合來全域地新增三個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-533">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="f8ff1-534">預設執行順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-534">Default order of execution</span></span>

<span data-ttu-id="f8ff1-535">當有多個*相同類型*的篩選時，範圍會決定篩選執行的預設順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-535">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="f8ff1-536">全域篩選準則會括住類別篩選。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-536">Global filters surround class filters.</span></span> <span data-ttu-id="f8ff1-537">類別篩選準則環繞方法篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-537">Class filters surround method filters.</span></span>

<span data-ttu-id="f8ff1-538">因為篩選條件巢狀結構的原因，篩選條件的「之後」程式碼的執行順序會與「之前」程式碼相反。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-538">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="f8ff1-539">篩選條件序列：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-539">The filter sequence:</span></span>

* <span data-ttu-id="f8ff1-540">全域篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-540">The *before* code of global filters.</span></span>
  * <span data-ttu-id="f8ff1-541">控制器篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-541">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="f8ff1-542">動作方法篩選條件的「之前」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-542">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="f8ff1-543">動作方法篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-543">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="f8ff1-544">控制器篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-544">The *after* code of controller filters.</span></span>
* <span data-ttu-id="f8ff1-545">全域篩選條件的「之後」程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-545">The *after* code of global filters.</span></span>
  
<span data-ttu-id="f8ff1-546">下列範例說明針對同步動作篩選條件呼叫篩選條件方法的順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-546">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="f8ff1-547">順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-547">Sequence</span></span> | <span data-ttu-id="f8ff1-548">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="f8ff1-548">Filter scope</span></span> | <span data-ttu-id="f8ff1-549">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-549">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="f8ff1-550">1</span><span class="sxs-lookup"><span data-stu-id="f8ff1-550">1</span></span> | <span data-ttu-id="f8ff1-551">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-551">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-552">2</span><span class="sxs-lookup"><span data-stu-id="f8ff1-552">2</span></span> | <span data-ttu-id="f8ff1-553">控制器</span><span class="sxs-lookup"><span data-stu-id="f8ff1-553">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-554">3</span><span class="sxs-lookup"><span data-stu-id="f8ff1-554">3</span></span> | <span data-ttu-id="f8ff1-555">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-555">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-556">4</span><span class="sxs-lookup"><span data-stu-id="f8ff1-556">4</span></span> | <span data-ttu-id="f8ff1-557">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-557">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-558">5</span><span class="sxs-lookup"><span data-stu-id="f8ff1-558">5</span></span> | <span data-ttu-id="f8ff1-559">控制器</span><span class="sxs-lookup"><span data-stu-id="f8ff1-559">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-560">6</span><span class="sxs-lookup"><span data-stu-id="f8ff1-560">6</span></span> | <span data-ttu-id="f8ff1-561">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-561">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="f8ff1-562">此順序顯示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-562">This sequence shows:</span></span>

* <span data-ttu-id="f8ff1-563">方法篩選條件巢狀位於控制器篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-563">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="f8ff1-564">控制器篩選條件巢狀位於全域篩選條件內。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-564">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="f8ff1-565">控制器和 Razor 頁面層級篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-565">Controller and Razor Page level filters</span></span>

<span data-ttu-id="f8ff1-566">繼承自 <xref:Microsoft.AspNetCore.Mvc.Controller> 基底類別的每個控制器都會包含 [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*)、[Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)，以及 [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` 方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-566">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="f8ff1-567">這些方法會：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-567">These methods:</span></span>

* <span data-ttu-id="f8ff1-568">包裝針對指定動作執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-568">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="f8ff1-569">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecuting`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-569">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="f8ff1-570">系統會在呼叫所有動作篩選條件之後呼叫 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-570">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="f8ff1-571">系統會在呼叫任何動作的篩選條件之前呼叫 `OnActionExecutionAsync`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-571">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="f8ff1-572">在動作方法之後執行 `next` 後，位於篩選條件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-572">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="f8ff1-573">例如，在下載範例中，`MySampleActionFilter` 會在啟動時全域套用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-573">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="f8ff1-574">`TestController`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-574">The `TestController`:</span></span>

* <span data-ttu-id="f8ff1-575">將 `SampleActionFilterAttribute` （`[SampleActionFilter]`）套用至 `FilterTest2` 動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-575">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="f8ff1-576">覆寫 `OnActionExecuting` 和 `OnActionExecuted`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-576">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="f8ff1-577">瀏覽至 `https://localhost:5001/Test/FilterTest2` 執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-577">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="f8ff1-578">針對 Razor Pages，請參閱[覆寫篩選條件方法來實作 Razor 頁面篩選條件](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-578">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="f8ff1-579">覆寫預設順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-579">Overriding the default order</span></span>

<span data-ttu-id="f8ff1-580">可以藉由實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> 來覆寫預設執行序列。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-580">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="f8ff1-581">`IOrderedFilter` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> 屬性，其優先順序會高於範圍以決定執行順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-581">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="f8ff1-582">具有較低 `Order` 值的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-582">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="f8ff1-583">在具有較高  *值的篩選條件之前執行「之前」* `Order`程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-583">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="f8ff1-584">在具有較高  *值的篩選條件之後執行「之後」* `Order`程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-584">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="f8ff1-585">`Order` 屬性可以搭配建構函式參數設定：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-585">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="f8ff1-586">請考慮先前範例所示的同樣 3 個動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-586">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="f8ff1-587">如果控制器和全域篩選條件的 `Order` 屬性被個別設定為 1 和 2，執行順序將會反轉。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-587">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="f8ff1-588">順序</span><span class="sxs-lookup"><span data-stu-id="f8ff1-588">Sequence</span></span> | <span data-ttu-id="f8ff1-589">篩選條件範圍</span><span class="sxs-lookup"><span data-stu-id="f8ff1-589">Filter scope</span></span> | <span data-ttu-id="f8ff1-590">`Order` 屬性</span><span class="sxs-lookup"><span data-stu-id="f8ff1-590">`Order` property</span></span> | <span data-ttu-id="f8ff1-591">Filter 方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-591">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="f8ff1-592">1</span><span class="sxs-lookup"><span data-stu-id="f8ff1-592">1</span></span> | <span data-ttu-id="f8ff1-593">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-593">Method</span></span> | <span data-ttu-id="f8ff1-594">0</span><span class="sxs-lookup"><span data-stu-id="f8ff1-594">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-595">2</span><span class="sxs-lookup"><span data-stu-id="f8ff1-595">2</span></span> | <span data-ttu-id="f8ff1-596">控制器</span><span class="sxs-lookup"><span data-stu-id="f8ff1-596">Controller</span></span> | <span data-ttu-id="f8ff1-597">1</span><span class="sxs-lookup"><span data-stu-id="f8ff1-597">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-598">3</span><span class="sxs-lookup"><span data-stu-id="f8ff1-598">3</span></span> | <span data-ttu-id="f8ff1-599">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-599">Global</span></span> | <span data-ttu-id="f8ff1-600">2</span><span class="sxs-lookup"><span data-stu-id="f8ff1-600">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="f8ff1-601">4</span><span class="sxs-lookup"><span data-stu-id="f8ff1-601">4</span></span> | <span data-ttu-id="f8ff1-602">全域</span><span class="sxs-lookup"><span data-stu-id="f8ff1-602">Global</span></span> | <span data-ttu-id="f8ff1-603">2</span><span class="sxs-lookup"><span data-stu-id="f8ff1-603">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-604">5</span><span class="sxs-lookup"><span data-stu-id="f8ff1-604">5</span></span> | <span data-ttu-id="f8ff1-605">控制器</span><span class="sxs-lookup"><span data-stu-id="f8ff1-605">Controller</span></span> | <span data-ttu-id="f8ff1-606">1</span><span class="sxs-lookup"><span data-stu-id="f8ff1-606">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="f8ff1-607">6</span><span class="sxs-lookup"><span data-stu-id="f8ff1-607">6</span></span> | <span data-ttu-id="f8ff1-608">方法</span><span class="sxs-lookup"><span data-stu-id="f8ff1-608">Method</span></span> | <span data-ttu-id="f8ff1-609">0</span><span class="sxs-lookup"><span data-stu-id="f8ff1-609">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="f8ff1-610">`Order` 屬性在決定篩選條件執行的順序時會覆寫範圍。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-610">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="f8ff1-611">篩選條件會先依照順序排序，然後使用範圍來打破僵局。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-611">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="f8ff1-612">所有內建的篩選條件都會實作 `IOrderedFilter` 並將預設 `Order` 值設為 0。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-612">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="f8ff1-613">針對內建篩選條件，除非將 `Order` 設定為非零值，否則範圍會決定順序。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-613">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="f8ff1-614">取消和縮短</span><span class="sxs-lookup"><span data-stu-id="f8ff1-614">Cancellation and short-circuiting</span></span>

<span data-ttu-id="f8ff1-615">可以設定提供給篩選條件方法之 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> 參數上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> 屬性，以縮短篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-615">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="f8ff1-616">比方說，下列資源篩選條件可防止管線的其餘部分執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-616">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-617">在下列程式碼中，`ShortCircuitingResourceFilter` 和 `AddHeader` 篩選條件都以 `SomeResource` 動作方法為目標。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-617">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="f8ff1-618">`ShortCircuitingResourceFilter`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-618">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="f8ff1-619">最先執行，因為它是資源篩選條件，且 `AddHeader` 是動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-619">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="f8ff1-620">縮短管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-620">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="f8ff1-621">因此，`AddHeader` 動作的 `SomeResource` 篩選條件永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-621">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="f8ff1-622">如果這兩個篩選條件都套用在動作方法層級，此行為也會相同，假設 `ShortCircuitingResourceFilter` 先執行的話。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-622">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="f8ff1-623">`ShortCircuitingResourceFilter` 會因為其篩選器類型而先執行，或藉由明確使用 `Order` 屬性而先執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-623">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="f8ff1-624">相依性插入</span><span class="sxs-lookup"><span data-stu-id="f8ff1-624">Dependency injection</span></span>

<span data-ttu-id="f8ff1-625">可以依類型或執行個體新增篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-625">Filters can be added by type or by instance.</span></span> <span data-ttu-id="f8ff1-626">如果新增執行個體，該執行個體將用於每個要求。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-626">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="f8ff1-627">如果新增類型，其將會由類型啟動。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-627">If a type is added, it's type-activated.</span></span> <span data-ttu-id="f8ff1-628">由類型啟動的篩選條件表示：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-628">A type-activated filter means:</span></span>

* <span data-ttu-id="f8ff1-629">系統會針對每個要求建立執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-629">An instance is created for each request.</span></span>
* <span data-ttu-id="f8ff1-630">任何建構函式相依性都會由[相依性插入](xref:fundamentals/dependency-injection) (DI) 所填入。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-630">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="f8ff1-631">實作為屬性並直接新增至控制器類別或動作方法的篩選條件，不能由[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-631">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="f8ff1-632">DI 無法提供建構函式相依性，因為：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-632">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="f8ff1-633">必須在提供屬性之建構函式參數的地方提供它們。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-633">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="f8ff1-634">這是屬性運作方式的限制。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-634">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="f8ff1-635">下列篩選條件支援由 DI 所提供的建構函式相依性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-635">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="f8ff1-636">在屬性上實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="f8ff1-637">上述篩選條件可以被套用至控制器或動作方法：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-637">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="f8ff1-638">DI 會提供記錄器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-638">Loggers are available from DI.</span></span> <span data-ttu-id="f8ff1-639">不過，請避免僅針對記錄目的建立並使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-639">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="f8ff1-640">[內建架構記錄](xref:fundamentals/logging/index)通常便可以提供記錄所需的項目。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-640">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="f8ff1-641">新增至篩選條件的記錄：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-641">Logging added to filters:</span></span>

* <span data-ttu-id="f8ff1-642">應該專注在篩選條件特定的商務領域考量或行為。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-642">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="f8ff1-643">**不**應該記錄動作或其他架構事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-643">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="f8ff1-644">內建篩選條件能記錄動作和架構事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-644">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="f8ff1-645">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f8ff1-645">ServiceFilterAttribute</span></span>

<span data-ttu-id="f8ff1-646">服務篩選條件實作類型會註冊在 `ConfigureServices` 中。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-646">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="f8ff1-647"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會從 DI 擷取篩選條件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-647">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="f8ff1-648">下面程式碼會說明 `AddHeaderResultServiceFilter`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-648">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f8ff1-649">在下列程式碼中，會將 `AddHeaderResultServiceFilter` 新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-649">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="f8ff1-650">在下列程式碼中，`ServiceFilter` 屬性會從 DI 擷取 `AddHeaderResultServiceFilter` 篩選條件的執行個體：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-650">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="f8ff1-651">使用 `ServiceFilterAttribute` 時，設定 [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-651">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="f8ff1-652">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-652">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f8ff1-653">ASP.NET Core 執行階段並不保證：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-653">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="f8ff1-654">將會建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-654">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="f8ff1-655">將不會於稍後的時間從 DI 容器重新要求篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-655">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="f8ff1-656">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-656">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="f8ff1-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f8ff1-658">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-658">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f8ff1-659">`CreateInstance` 會從 DI 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-659">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="f8ff1-660">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="f8ff1-660">TypeFilterAttribute</span></span>

<span data-ttu-id="f8ff1-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 類似於 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>，但其類型不會直接從 DI 容器解析。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="f8ff1-662">它會使用 <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName> 來具現化類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-662">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="f8ff1-663">由於 `TypeFilterAttribute` 類型不會直接從 DI 容器解析：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-663">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="f8ff1-664">使用 `TypeFilterAttribute` 參考的類型不需要向 DI 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-664">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="f8ff1-665">不過它們的相依性會由 DI 容器滿足。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-665">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="f8ff1-666">`TypeFilterAttribute` 可以選擇性地接受類型的建構函式引數。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-666">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="f8ff1-667">使用 `TypeFilterAttribute` 時，設定 [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-667">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="f8ff1-668">能提供篩選條件執行個體「可能」可以在其建立的要求範圍之外重複使用的提示。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-668">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="f8ff1-669">ASP.NET Core 執行階段不保證將建立篩選條件的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-669">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="f8ff1-670">不應該搭配仰賴具有非單一資料庫存留期之服務的篩選條件使用。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-670">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="f8ff1-671">下列範例示範如何使用 `TypeFilterAttribute` 將引數傳遞至類型：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-671">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="f8ff1-672">授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-672">Authorization filters</span></span>

<span data-ttu-id="f8ff1-673">授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-673">Authorization filters:</span></span>

* <span data-ttu-id="f8ff1-674">是篩選條件管線內最先執行的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-674">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="f8ff1-675">控制動作方法的存取。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-675">Control access to action methods.</span></span>
* <span data-ttu-id="f8ff1-676">有之前的方法，但沒有之後的方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-676">Have a before method, but no after method.</span></span>

<span data-ttu-id="f8ff1-677">自訂授權篩選條件需要自訂的授權架構。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-677">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="f8ff1-678">最好是設定授權原則或撰寫自訂授權原則，而不要撰寫自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-678">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="f8ff1-679">內建的授權篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-679">The built-in authorization filter:</span></span>

* <span data-ttu-id="f8ff1-680">呼叫授權系統。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-680">Calls the authorization system.</span></span>
* <span data-ttu-id="f8ff1-681">不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-681">Does not authorize requests.</span></span>

<span data-ttu-id="f8ff1-682">**不會**在授權篩選條件內擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-682">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="f8ff1-683">該例外狀況將不會被處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-683">The exception will not be handled.</span></span>
* <span data-ttu-id="f8ff1-684">例外狀況篩選條件將不會處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-684">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="f8ff1-685">請考慮在例外狀況於授權篩選條件中發生時發出挑戰。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-685">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="f8ff1-686">深入了解[授權](xref:security/authorization/introduction)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-686">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="f8ff1-687">資源篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-687">Resource filters</span></span>

<span data-ttu-id="f8ff1-688">資源篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-688">Resource filters:</span></span>

* <span data-ttu-id="f8ff1-689">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-689">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="f8ff1-690">執行會包裝大部分的篩選條件管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-690">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="f8ff1-691">只有[授權篩選條件](#authorization-filters)會在資源篩選條件之前執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-691">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="f8ff1-692">資源篩選條件很適合用來縮短大部分的管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-692">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="f8ff1-693">比方說，快取篩選條件可以避免快取命中上的其餘管線。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-693">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="f8ff1-694">資源篩選條件範例：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-694">Resource filter examples:</span></span>

* <span data-ttu-id="f8ff1-695">先前所示的[縮短資源篩選條件](#short-circuiting-resource-filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-695">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="f8ff1-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) \(英文\)：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="f8ff1-697">防止模型繫結存取表單資料。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-697">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="f8ff1-698">用於大型檔案上傳，以避免將表單資料讀入記憶體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-698">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="f8ff1-699">動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-699">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8ff1-700">動作篩選條件**不會**套用到 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-700">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="f8ff1-701">Razor Pages 支援 <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> 與 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-701">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="f8ff1-702">如需詳細資訊，請參閱 [Razor 頁面的篩選條件方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-702">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="f8ff1-703">動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-703">Action filters:</span></span>

* <span data-ttu-id="f8ff1-704">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> 介面。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-704">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="f8ff1-705">它們執行會包圍動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-705">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="f8ff1-706">下列程式碼會顯示範例動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-706">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="f8ff1-707"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> 提供下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-707">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="f8ff1-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - 讓針對某個動作方法的輸入可以被讀取。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="f8ff1-709"><xref:Microsoft.AspNetCore.Mvc.Controller> - 提供對控制器執行個體的管理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-709"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="f8ff1-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> - 設定 `Result` 會縮短動作方法和後續動作篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="f8ff1-711">在動作方法中擲回例外狀況：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-711">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="f8ff1-712">防止執行後續篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-712">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="f8ff1-713">和設定 `Result` 不同，會被視為失敗而非成功結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-713">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f8ff1-714"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> 提供 `Controller` 與 `Result`，加上下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-714">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="f8ff1-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - 如果動作執行被另一個篩選條件縮短，則為 true。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f8ff1-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - 如果動作或先前所執行的動作篩選條件擲回例外狀況，則為非 Null。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="f8ff1-717">將此屬性設定為 Null：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-717">Setting this property to null:</span></span>

  * <span data-ttu-id="f8ff1-718">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-718">Effectively handles the exception.</span></span>
  * <span data-ttu-id="f8ff1-719">會執行 `Result`，有如它是從動作方法傳回一般。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-719">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="f8ff1-720">對於 `IAsyncActionFilter`，呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> 會：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-720">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="f8ff1-721">執行任何後續的動作篩選條件和動作方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-721">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="f8ff1-722">傳回 `ActionExecutedContext`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-722">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="f8ff1-723">若要縮短，請指派 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> 給某個結果執行個體，並且不要呼叫 `next` (`ActionExecutionDelegate`)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-723">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="f8ff1-724">架構提供抽象 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-724">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="f8ff1-725">`OnActionExecuting` 動作篩選條件可以用來：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-725">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="f8ff1-726">驗證模型狀態。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-726">Validate model state.</span></span>
* <span data-ttu-id="f8ff1-727">如果狀態無效，則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-727">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="f8ff1-728">`OnActionExecuted` 方法會在動作方法之後執行：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-728">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="f8ff1-729">並可以透過 <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> 屬性查看及操作動作的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-729">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="f8ff1-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> 會在動作執行被另一個篩選條件縮短時被設為 true。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="f8ff1-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> 會在動作或後續的動作篩選條件擲回例外狀況時被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="f8ff1-732">將 `Exception` 設定為 null：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-732">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="f8ff1-733">實際上會「處理」例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-733">Effectively handles an exception.</span></span>
  * <span data-ttu-id="f8ff1-734">執行 `ActionExecutedContext.Result`，彷彿它已正常地從動作方法傳回。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-734">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="f8ff1-735">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-735">Exception filters</span></span>

<span data-ttu-id="f8ff1-736">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-736">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-737">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-737">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="f8ff1-738">可以用來實作常見的錯誤處理原則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-738">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="f8ff1-739">下列範例例外狀況篩選條件會使用自訂的錯誤檢視，以顯示在開發應用程式時發生的例外狀況詳細資料：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-739">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="f8ff1-740">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-740">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-741">沒有之前和之後的事件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-741">Don't have before and after events.</span></span>
* <span data-ttu-id="f8ff1-742">實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-742">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="f8ff1-743">處理在 Razor 頁面或控制器建立、[模型繫結](xref:mvc/models/model-binding)、動作篩選條件或動作方法中發生的未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-743">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="f8ff1-744">**不會**攔截在資源篩選條件、結果篩選條件或 MVC 結果執行中發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-744">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="f8ff1-745">若要處理例外狀況，請將 <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> 屬性設為 `true`，或撰寫回應。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-745">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="f8ff1-746">這樣會阻止傳播例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-746">This stops propagation of the exception.</span></span> <span data-ttu-id="f8ff1-747">例外狀況篩選條件無法將例外狀況變成「成功」。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-747">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="f8ff1-748">只有動作篩選條件可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-748">Only an action filter can do that.</span></span>

<span data-ttu-id="f8ff1-749">例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-749">Exception filters:</span></span>

* <span data-ttu-id="f8ff1-750">適合用於設陷在動作內發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-750">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="f8ff1-751">不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-751">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="f8ff1-752">偏好使用中介軟體進行例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-752">Prefer middleware for exception handling.</span></span> <span data-ttu-id="f8ff1-753">只有在錯誤處理會根據所呼叫的動作方法而「不同」時才使用例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-753">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="f8ff1-754">比方說，應用程式可能會有適用於 API 端點及檢視/HTML 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-754">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="f8ff1-755">API 端點可能將錯誤資訊傳回為 JSON，而以檢視為基礎的動作則可能將錯誤頁面傳回為 HTML。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-755">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="f8ff1-756">結果篩選條件</span><span class="sxs-lookup"><span data-stu-id="f8ff1-756">Result filters</span></span>

<span data-ttu-id="f8ff1-757">結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-757">Result filters:</span></span>

* <span data-ttu-id="f8ff1-758">實作介面：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-758">Implement an interface:</span></span>
  * <span data-ttu-id="f8ff1-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="f8ff1-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 或 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="f8ff1-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="f8ff1-761">它們執行會包圍動作結果的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-761">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="f8ff1-762">IResultFilter 和 IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="f8ff1-762">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="f8ff1-763">以下程式碼會顯示能新增 HTTP 標頭的結果篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-763">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="f8ff1-764">執行的結果類型會取決於動作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-764">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="f8ff1-765">傳回檢視的動作會在執行中的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 裡包含處理中的所有 Razor。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-765">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="f8ff1-766">API 方法可能在結果執行當中執行某種序列化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-766">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="f8ff1-767">深入瞭解[動作結果](xref:mvc/controllers/actions)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-767">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="f8ff1-768">只有當動作或動作篩選準則產生動作結果時，才會執行結果篩選準則。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-768">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="f8ff1-769">當下列情況時，不會執行結果篩選：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-769">Result filters are not executed when:</span></span>

* <span data-ttu-id="f8ff1-770">授權篩選或資源篩選器會將管線短路。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-770">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="f8ff1-771">例外狀況篩選條件會產生動作結果來處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-771">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="f8ff1-772">藉由將 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> 設為 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName>，`true` 方法可以縮短動作結果和後續結果篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-772">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="f8ff1-773">在縮短時寫入至回應物件，以避免產生空的回應。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-773">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="f8ff1-774">在 `IResultFilter.OnResultExecuting` 中擲回例外狀況會：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-774">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="f8ff1-775">導致無法執行動作結果和後續的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-775">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="f8ff1-776">視為失敗，而不是成功的結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-776">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="f8ff1-777">當 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> 方法執行時，回應可能已經傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-777">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="f8ff1-778">如果回應已傳送給用戶端，則無法進一步變更。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-778">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="f8ff1-779">如果動作結果執行被另一個篩選條件縮短，則 `ResultExecutedContext.Canceled` 會被設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-779">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="f8ff1-780">如果動作結果或後續的結果篩選條件擲回例外狀況，則 `ResultExecutedContext.Exception` 會被設為非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-780">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="f8ff1-781">將 `Exception` 設為 Null 實際上會「處理」例外狀況，並且使 ASP.NET Core 稍後不會在管線中重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-781">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="f8ff1-782">並沒有可以在處理結果篩選條件中的例外狀況時，將資料寫入回應的可靠方式。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-782">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="f8ff1-783">當動作結果擲回例外狀況時，如果標頭已清除至用戶端，則沒有任何可靠的機制能傳送失敗碼。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-783">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="f8ff1-784">針對 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> 而言，對 `await next` 上的 <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> 的呼叫會執行任何後續的結果篩選條件和動作結果。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-784">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="f8ff1-785">若要縮短，請將 [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) 設定為 `true`，並且不要呼叫 `ResultExecutionDelegate`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-785">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="f8ff1-786">架構提供抽象 `ResultFilterAttribute`，其可被子類別化。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-786">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="f8ff1-787">先前所示的 [AddHeaderAttribute](#add-header-attribute) 類別是結果篩選條件屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-787">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="f8ff1-788">IAlwaysRunResultFilter 和 IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="f8ff1-788">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="f8ff1-789"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> 介面會宣告針對所有動作結果執行的 <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> 實作。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-789">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="f8ff1-790">這包括由產生的動作結果：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-790">This includes action results produced by:</span></span>

* <span data-ttu-id="f8ff1-791">最少迴圈的授權篩選準則和資源篩選器。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-791">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="f8ff1-792">例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-792">Exception filters.</span></span>

<span data-ttu-id="f8ff1-793">例如，下列篩選一律會執行，並會在內容交涉失敗時，為動作結果 (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) 設定「422 無法處理的實體」狀態碼：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-793">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="f8ff1-794">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="f8ff1-794">IFilterFactory</span></span>

<span data-ttu-id="f8ff1-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="f8ff1-796">因此，`IFilterFactory` 執行個體可用來在篩選條件管線中任何位置作為 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-796">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="f8ff1-797">當執行階段準備要叫用篩選條件時，它會嘗試將它轉換成 `IFilterFactory`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-797">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="f8ff1-798">如果該轉換成功，系統會呼叫 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立被叫用的 `IFilterMetadata` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-798">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="f8ff1-799">因為在應用程式啟動時不需要明確設定精確的篩選條件管線，所以這提供了具有彈性的設計。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-799">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="f8ff1-800">可以使用自訂屬性實作作為另一種建立篩選條件的方法，來實作 `IFilterFactory`：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-800">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="f8ff1-801">上述程式碼可以透過執行[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\) 來進行測試：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-801">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="f8ff1-802">叫用 F12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-802">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="f8ff1-803">瀏覽至 `https://localhost:5001/Sample/HeaderWithFactory`。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-803">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="f8ff1-804">F12 開發人員工具會顯示由範例程式碼所加入的下列回應標頭：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-804">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="f8ff1-805">**作者：** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-805">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="f8ff1-806">**globaladdheader：** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="f8ff1-807">**內部：** `My header`</span><span class="sxs-lookup"><span data-stu-id="f8ff1-807">**internal:** `My header`</span></span>

<span data-ttu-id="f8ff1-808">上述程式碼會建立**internal：** `My header` 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-808">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="f8ff1-809">在屬性上實作的 IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="f8ff1-809">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="f8ff1-810">實作 `IFilterFactory` 的篩選條件很適合用於下列類型的篩選條件：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-810">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="f8ff1-811">不需要傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-811">Don't require passing parameters.</span></span>
* <span data-ttu-id="f8ff1-812">有需要由 DI 滿足的建構函式相依性。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-812">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="f8ff1-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> 會實作 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="f8ff1-814">`IFilterFactory` 會公開 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> 方法來建立 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-814">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="f8ff1-815">`CreateInstance` 會從服務容器 (DI) 載入指定的類型。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-815">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="f8ff1-816">下列程式碼會顯示套用 `[SampleActionFilter]` 的三種方式：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-816">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="f8ff1-817">在上述程式碼中，使用 `[SampleActionFilter]` 來裝飾方法是套用 `SampleActionFilter` 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-817">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="f8ff1-818">在篩選條件管線中使用中介軟體</span><span class="sxs-lookup"><span data-stu-id="f8ff1-818">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="f8ff1-819">資源篩選條件的運作與[中介軟體](xref:fundamentals/middleware/index)相似之處在於，它們會圍繞管線中稍後的所有項目執行。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-819">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="f8ff1-820">但篩選條件與中介軟體不同之處在於它們是 ASP.NET Core 執行階段的一部分，這表示它們能存取 ASP.NET Core 內容和建構。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-820">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="f8ff1-821">若要使用中介軟體作為篩選條件，請建立一個具有 `Configure` 方法的類型，其能指定要插入到篩選條件管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-821">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="f8ff1-822">以下範例會使用當地語系化中介軟體來建立某個要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-822">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="f8ff1-823">使用 <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> 來執行中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8ff1-823">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="f8ff1-824">中介軟體篩選條件與資源篩選條件在相同的篩選條件管線階段執行：模型繫結之前和管線的其餘部分之後。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-824">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="f8ff1-825">後續動作</span><span class="sxs-lookup"><span data-stu-id="f8ff1-825">Next actions</span></span>

* <span data-ttu-id="f8ff1-826">請參閱[Razor Pages 的篩選方法](xref:razor-pages/filter)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-826">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="f8ff1-827">若要嘗試使用篩選條件，請[下載、測試及修改 GitHub 範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="f8ff1-827">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
