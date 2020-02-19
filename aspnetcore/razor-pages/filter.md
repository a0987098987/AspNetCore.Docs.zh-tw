---
title: ASP.NET Core 中 Razor 頁面的篩選條件方法
author: Rick-Anderson
description: 了解如何在 ASP.NET Core 中建立 Razor 頁面的篩選條件方法。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: a60b17685c6f836de7c0afcc5b89a9894fb8b28f
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447227"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="02207-103">ASP.NET Core 中 Razor 頁面的篩選條件方法</span><span class="sxs-lookup"><span data-stu-id="02207-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="02207-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="02207-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="02207-105">Razor 頁面篩選條件 [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) 和 [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) 可讓 Razor 頁面在 Razor 頁面處理常式執行之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="02207-106">Razor 頁面篩選條件類似於 [ASP.NET Core MVC 動作篩選條件](xref:mvc/controllers/filters#action-filters)，但它們無法套用至個別的頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="02207-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="02207-107">Razor 頁面篩選條件：</span><span class="sxs-lookup"><span data-stu-id="02207-107">Razor Page filters:</span></span>

* <span data-ttu-id="02207-108">在選取處理常式方法之後，但在進行模型繫結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="02207-109">在執行處理常式方法之前，並在完成模型繫結之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="02207-110">在執行處理常式方法之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="02207-111">可以在某個頁面或全域實作。</span><span class="sxs-lookup"><span data-stu-id="02207-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="02207-112">無法套用至特定頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="02207-112">Cannot be applied to specific page handler methods.</span></span>
* <span data-ttu-id="02207-113">可以有相依性[插入](xref:fundamentals/dependency-injection)（DI）填入的函式相依性。</span><span class="sxs-lookup"><span data-stu-id="02207-113">Can have constructor dependencies populated by [Dependency Injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="02207-114">如需詳細資訊，請參閱[ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute)和[TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute)。</span><span class="sxs-lookup"><span data-stu-id="02207-114">For more information, see [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) and [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span></span>

<span data-ttu-id="02207-115">雖然頁面的程式碼和中介軟體能夠在處理常式方法執行之前執行自訂程式碼，但只有 Razor 頁面篩選器才能夠存取 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> 和頁面。</span><span class="sxs-lookup"><span data-stu-id="02207-115">While page constructors and middleware enable executing custom code before a handler method executes, only Razor Page filters enable access to <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> and the page.</span></span> <span data-ttu-id="02207-116">中介軟體可以存取 `HttpContext`，而不是「頁面內容」。</span><span class="sxs-lookup"><span data-stu-id="02207-116">Middleware has access to the `HttpContext`, but not to the "page context".</span></span> <span data-ttu-id="02207-117">篩選具有 <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> 的衍生參數，可提供 `HttpContext`的存取權。</span><span class="sxs-lookup"><span data-stu-id="02207-117">Filters have a <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="02207-118">例如，[實作篩選條件屬性](#ifa)範例會將標頭新增至回應，這是無法使用建構函式或中介軟體完成的作業。</span><span class="sxs-lookup"><span data-stu-id="02207-118">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="02207-119">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02207-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="02207-120">Razor 頁面篩選條件提供下列方法，可在全域或頁面層級套用：</span><span class="sxs-lookup"><span data-stu-id="02207-120">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="02207-121">同步方法：</span><span class="sxs-lookup"><span data-stu-id="02207-121">Synchronous methods:</span></span>

  * <span data-ttu-id="02207-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="02207-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0)：在執行處理常式方法之前，並在完成模型繫結之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="02207-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0)：在執行處理常式方法之後，並在動作結果之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="02207-125">非同步方法：</span><span class="sxs-lookup"><span data-stu-id="02207-125">Asynchronous methods:</span></span>

  * <span data-ttu-id="02207-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前以非同步方式呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="02207-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0)：在叫用處理常式方法之前，並在完成模型繫結之後以非同步方式呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

<span data-ttu-id="02207-128">請實作同步**或**非同步版本的篩選條件介面，而**不要**同時實作這兩者。</span><span class="sxs-lookup"><span data-stu-id="02207-128">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="02207-129">架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="02207-129">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="02207-130">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="02207-130">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="02207-131">如果同時實作為這兩個介面，則只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="02207-131">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="02207-132">相同的規則會套用至頁面中的覆寫，實作覆寫的同步或非同步版本，但不能同時實作。</span><span class="sxs-lookup"><span data-stu-id="02207-132">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="02207-133">全域實作 Razor 頁面篩選條件</span><span class="sxs-lookup"><span data-stu-id="02207-133">Implement Razor Page filters globally</span></span>

<span data-ttu-id="02207-134">下列程式碼會實作 `IAsyncPageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-134">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="02207-135">在上述程式碼中，`ProcessUserAgent.Write` 是使用者提供的程式碼，可與使用者代理字串搭配使用。</span><span class="sxs-lookup"><span data-stu-id="02207-135">In the preceding code, `ProcessUserAgent.Write` is user supplied code that works with the user agent string.</span></span>

<span data-ttu-id="02207-136">下列程式碼會啟用 `SampleAsyncPageFilter` 類別中的 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="02207-136">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

<span data-ttu-id="02207-137">下列程式碼會呼叫 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>，只將 `SampleAsyncPageFilter` 套用至 */Movies*中的頁面：</span><span class="sxs-lookup"><span data-stu-id="02207-137">The following code calls <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to apply the `SampleAsyncPageFilter` to only pages in */Movies*:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="02207-138">下例程式碼會實作同步的 `IPageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-138">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="02207-139">下列程式碼會啟用 `SamplePageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-139">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="02207-140">覆寫篩選條件方法來實作 Razor 頁面篩選條件</span><span class="sxs-lookup"><span data-stu-id="02207-140">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="02207-141">下列程式碼會覆寫非同步 Razor 頁面篩選：</span><span class="sxs-lookup"><span data-stu-id="02207-141">The following code overrides the asynchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="02207-142">實作篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="02207-142">Implement a filter attribute</span></span>

<span data-ttu-id="02207-143">內建的以屬性為基礎的篩選器 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> 篩選器可以子類別化。</span><span class="sxs-lookup"><span data-stu-id="02207-143">The built-in attribute-based filter <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filter can be subclassed.</span></span> <span data-ttu-id="02207-144">下列篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="02207-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="02207-145">下列程式碼會套用 `AddHeader` 屬性：</span><span class="sxs-lookup"><span data-stu-id="02207-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

<span data-ttu-id="02207-146">使用瀏覽器開發人員工具之類的工具來檢查標頭。</span><span class="sxs-lookup"><span data-stu-id="02207-146">Use a tool such as the browser developer tools to examine the headers.</span></span> <span data-ttu-id="02207-147">在 [**回應標頭**] 底下，會顯示 `author: Rick`。</span><span class="sxs-lookup"><span data-stu-id="02207-147">Under **Response Headers**, `author: Rick` is displayed.</span></span>

<span data-ttu-id="02207-148">如需覆寫順序的指示，請參閱[覆寫預設順序](xref:mvc/controllers/filters#overriding-the-default-order)。</span><span class="sxs-lookup"><span data-stu-id="02207-148">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="02207-149">如需從篩選條件縮短篩選管線的指示，請參閱[取消和縮短](xref:mvc/controllers/filters#cancellation-and-short-circuiting)。</span><span class="sxs-lookup"><span data-stu-id="02207-149">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span>

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="02207-150">授權篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="02207-150">Authorize filter attribute</span></span>

<span data-ttu-id="02207-151">[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) 屬性可以套用至 `PageModel`：</span><span class="sxs-lookup"><span data-stu-id="02207-151">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="02207-152">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="02207-152">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="02207-153">Razor 頁面篩選條件 [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) 和 [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) 可讓 Razor 頁面在 Razor 頁面處理常式執行之前和之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-153">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="02207-154">Razor 頁面篩選條件類似於 [ASP.NET Core MVC 動作篩選條件](xref:mvc/controllers/filters#action-filters)，但它們無法套用至個別的頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="02207-154">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="02207-155">Razor 頁面篩選條件：</span><span class="sxs-lookup"><span data-stu-id="02207-155">Razor Page filters:</span></span>

* <span data-ttu-id="02207-156">在選取處理常式方法之後，但在進行模型繫結之前執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-156">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="02207-157">在執行處理常式方法之前，並在完成模型繫結之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-157">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="02207-158">在執行處理常式方法之後執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="02207-158">Run code after the handler method executes.</span></span>
* <span data-ttu-id="02207-159">可以在某個頁面或全域實作。</span><span class="sxs-lookup"><span data-stu-id="02207-159">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="02207-160">無法套用至特定頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="02207-160">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="02207-161">程式碼可以使用頁面的建構函式或中介軟體在執行處理常式方法之前執行，但只有 Razor 頁面篩選條件可以存取 [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext)。</span><span class="sxs-lookup"><span data-stu-id="02207-161">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="02207-162">篩選條件具有 [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) 衍生參數，可提供對 `HttpContext` 的存取。</span><span class="sxs-lookup"><span data-stu-id="02207-162">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="02207-163">例如，[實作篩選條件屬性](#ifa)範例會將標頭新增至回應，這是無法使用建構函式或中介軟體完成的作業。</span><span class="sxs-lookup"><span data-stu-id="02207-163">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="02207-164">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02207-164">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="02207-165">Razor 頁面篩選條件提供下列方法，可在全域或頁面層級套用：</span><span class="sxs-lookup"><span data-stu-id="02207-165">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="02207-166">同步方法：</span><span class="sxs-lookup"><span data-stu-id="02207-166">Synchronous methods:</span></span>

  * <span data-ttu-id="02207-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="02207-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0)：在執行處理常式方法之前，並在完成模型繫結之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="02207-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0)：在執行處理常式方法之後，並在動作結果之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="02207-170">非同步方法：</span><span class="sxs-lookup"><span data-stu-id="02207-170">Asynchronous methods:</span></span>

  * <span data-ttu-id="02207-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0)：在選取處理常式方法之後，但在進行模型繫結之前以非同步方式呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="02207-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0)：在叫用處理常式方法之前，並在完成模型繫結之後以非同步方式呼叫。</span><span class="sxs-lookup"><span data-stu-id="02207-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="02207-173">請實作同步**或**非同步版本的篩選條件介面，但不要兩者同時實作。</span><span class="sxs-lookup"><span data-stu-id="02207-173">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="02207-174">架構會先檢查以查看篩選條件是否實作非同步介面，如果是的話，便呼叫該介面。</span><span class="sxs-lookup"><span data-stu-id="02207-174">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="02207-175">如果沒有，它會呼叫同步介面的方法。</span><span class="sxs-lookup"><span data-stu-id="02207-175">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="02207-176">如果同時實作為這兩個介面，則只會呼叫非同步方法。</span><span class="sxs-lookup"><span data-stu-id="02207-176">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="02207-177">相同的規則會套用至頁面中的覆寫，實作覆寫的同步或非同步版本，但不能同時實作。</span><span class="sxs-lookup"><span data-stu-id="02207-177">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="02207-178">全域實作 Razor 頁面篩選條件</span><span class="sxs-lookup"><span data-stu-id="02207-178">Implement Razor Page filters globally</span></span>

<span data-ttu-id="02207-179">下列程式碼會實作 `IAsyncPageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-179">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="02207-180">在上述程式碼中，[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) 並非必要。</span><span class="sxs-lookup"><span data-stu-id="02207-180">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="02207-181">它在範例中用來提供應用程式的追蹤資訊。</span><span class="sxs-lookup"><span data-stu-id="02207-181">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="02207-182">下列程式碼會啟用 `SampleAsyncPageFilter` 類別中的 `Startup`：</span><span class="sxs-lookup"><span data-stu-id="02207-182">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="02207-183">下列程式碼顯示完整的 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="02207-183">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="02207-184">下列程式碼會呼叫 `AddFolderApplicationModelConvention`，只將 `SampleAsyncPageFilter` 套用至 */subFolder* 中的頁面：</span><span class="sxs-lookup"><span data-stu-id="02207-184">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="02207-185">下例程式碼會實作同步的 `IPageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-185">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="02207-186">下列程式碼會啟用 `SamplePageFilter`：</span><span class="sxs-lookup"><span data-stu-id="02207-186">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="02207-187">覆寫篩選條件方法來實作 Razor 頁面篩選條件</span><span class="sxs-lookup"><span data-stu-id="02207-187">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="02207-188">下列程式碼會覆寫同步的 Razor 頁面篩選條件：</span><span class="sxs-lookup"><span data-stu-id="02207-188">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="02207-189">實作篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="02207-189">Implement a filter attribute</span></span>

<span data-ttu-id="02207-190">以內建屬性為基礎的篩選條件 [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) 可以進行子類別化。</span><span class="sxs-lookup"><span data-stu-id="02207-190">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="02207-191">下列篩選條件會將標頭新增至回應：</span><span class="sxs-lookup"><span data-stu-id="02207-191">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="02207-192">下列程式碼會套用 `AddHeader` 屬性：</span><span class="sxs-lookup"><span data-stu-id="02207-192">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="02207-193">如需覆寫順序的指示，請參閱[覆寫預設順序](xref:mvc/controllers/filters#overriding-the-default-order)。</span><span class="sxs-lookup"><span data-stu-id="02207-193">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="02207-194">如需從篩選條件縮短篩選管線的指示，請參閱[取消和縮短](xref:mvc/controllers/filters#cancellation-and-short-circuiting)。</span><span class="sxs-lookup"><span data-stu-id="02207-194">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="02207-195">授權篩選條件屬性</span><span class="sxs-lookup"><span data-stu-id="02207-195">Authorize filter attribute</span></span>

<span data-ttu-id="02207-196">[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) 屬性可以套用至 `PageModel`：</span><span class="sxs-lookup"><span data-stu-id="02207-196">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
