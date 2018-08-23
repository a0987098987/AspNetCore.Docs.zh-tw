---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: 了解動作篩選條件 (C#) |Microsoft Docs
author: microsoft
description: 本教學課程的目標在於說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作或整個控制器...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c7706d8252d5a0271f1b9243fa8eb282f722654
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832554"
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="33d66-104">了解動作篩選條件 (C#)</span><span class="sxs-lookup"><span data-stu-id="33d66-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="33d66-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="33d66-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="33d66-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="33d66-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="33d66-107">本教學課程的目標在於說明動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="33d66-108">動作篩選條件是屬性，您可以套用至控制器動作或整個 controller--修改 執行動作的方式。</span><span class="sxs-lookup"><span data-stu-id="33d66-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="33d66-109">了解動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="33d66-109">Understanding Action Filters</span></span>

<span data-ttu-id="33d66-110">本教學課程的目標在於說明動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="33d66-111">動作篩選條件是屬性，您可以套用至控制器動作或整個 controller--修改 執行動作的方式。</span><span class="sxs-lookup"><span data-stu-id="33d66-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="33d66-112">ASP.NET MVC 架構包括數個動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="33d66-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="33d66-113">OutputCache – 此動作篩選條件會快取一段指定時間的控制器動作的輸出。</span><span class="sxs-lookup"><span data-stu-id="33d66-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="33d66-114">HandleError – 此動作篩選條件處理控制器動作執行時引發的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d66-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="33d66-115">授權-此動作篩選條件可讓您限制存取特定使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="33d66-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="33d66-116">您也可以建立您自己的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="33d66-117">例如，您可能要建立自訂動作篩選條件，以實作自訂驗證系統。</span><span class="sxs-lookup"><span data-stu-id="33d66-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="33d66-118">或者，您可能想要建立動作篩選條件，以修改控制器動作所傳回的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="33d66-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="33d66-119">在本教學課程中，您會學習如何建立動作篩選條件所打造的。</span><span class="sxs-lookup"><span data-stu-id="33d66-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="33d66-120">我們會建立記錄至 Visual Studio [輸出] 視窗的不同階段的處理程序的動作記錄動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="33d66-121">使用動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="33d66-121">Using an Action Filter</span></span>

<span data-ttu-id="33d66-122">動作篩選條件是屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-122">An action filter is an attribute.</span></span> <span data-ttu-id="33d66-123">您可以將大部分的動作篩選條件套用至個別的控制器動作或整個控制器。</span><span class="sxs-lookup"><span data-stu-id="33d66-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="33d66-124">比方說，在 列表 1 中的資料控制者會公開名為動作`Index()`，傳回目前的時間。</span><span class="sxs-lookup"><span data-stu-id="33d66-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="33d66-125">此動作以裝飾`OutputCache`動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="33d66-126">此篩選條件會導致快取為 10 秒的動作所傳回的值。</span><span class="sxs-lookup"><span data-stu-id="33d66-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="33d66-127">**列表 1 – `Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="33d66-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="33d66-128">如果您重複叫用`Index()`由您的瀏覽器的網址列中輸入 URL/資料/索引，然後按 重新整理動作按鈕多次，，然後您會看到相同的時間為 10 秒。</span><span class="sxs-lookup"><span data-stu-id="33d66-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="33d66-129">輸出`Index()`動作的快取 （請參閱 圖 1） 的 10 秒。</span><span class="sxs-lookup"><span data-stu-id="33d66-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="33d66-130">[![快取的時間](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="33d66-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="33d66-131">**圖 01**： 快取時間 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="33d66-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="33d66-132">在列表 1 中，單一動作篩選條件 –`OutputCache`套用至動作篩選條件 –`Index()`方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="33d66-133">如果您需要您可以套用多個動作篩選條件至相同的動作。</span><span class="sxs-lookup"><span data-stu-id="33d66-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="33d66-134">例如，您可能想要套用這兩`OutputCache`和`HandleError`動作篩選條件至相同的動作。</span><span class="sxs-lookup"><span data-stu-id="33d66-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="33d66-135">在 [列表 1]`OutputCache`動作篩選條件會套用至`Index()`動作。</span><span class="sxs-lookup"><span data-stu-id="33d66-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="33d66-136">您也可以套用至這個屬性`DataController`類別本身。</span><span class="sxs-lookup"><span data-stu-id="33d66-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="33d66-137">在此情況下，任何由控制器的動作所傳回的結果會快取 10 秒的時間。</span><span class="sxs-lookup"><span data-stu-id="33d66-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="33d66-138">不同類型的篩選器</span><span class="sxs-lookup"><span data-stu-id="33d66-138">The Different Types of Filters</span></span>

<span data-ttu-id="33d66-139">ASP.NET MVC 架構支援四種不同類型的篩選：</span><span class="sxs-lookup"><span data-stu-id="33d66-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="33d66-140">授權篩選條件 – 實作`IAuthorizationFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="33d66-141">動作篩選條件 – 實作`IActionFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="33d66-142">結果篩選條件 – 實作`IResultFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="33d66-143">例外狀況篩選條件 – 實作`IExceptionFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="33d66-144">篩選條件的執行上面所列的順序。</span><span class="sxs-lookup"><span data-stu-id="33d66-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="33d66-145">例如，授權篩選條件永遠會在動作篩選條件之前執行，而例外狀況篩選條件一律會執行篩選器的型別之後。</span><span class="sxs-lookup"><span data-stu-id="33d66-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="33d66-146">授權篩選條件會用來實作驗證和授權適用於控制器動作。</span><span class="sxs-lookup"><span data-stu-id="33d66-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="33d66-147">比方說，授權篩選條件是為授權篩選條件的範例。</span><span class="sxs-lookup"><span data-stu-id="33d66-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="33d66-148">動作篩選條件包含之前和之後的控制器動作執行時，才會執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="33d66-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="33d66-149">您可以修改控制器動作傳回檢視資料，比方說，使用動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="33d66-150">結果篩選條件包含之前和之後執行檢視結果時，才會執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="33d66-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="33d66-151">例如，您可能想要修改檢視結果 檢視呈現至瀏覽器之前，以滑鼠右鍵。</span><span class="sxs-lookup"><span data-stu-id="33d66-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="33d66-152">例外狀況篩選條件會篩選條件執行的最後一個類型。</span><span class="sxs-lookup"><span data-stu-id="33d66-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="33d66-153">您可以使用例外狀況篩選條件來處理您的控制器動作或控制器動作結果所引發的錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d66-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="33d66-154">您也可以使用例外狀況篩選條件，來記錄錯誤。</span><span class="sxs-lookup"><span data-stu-id="33d66-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="33d66-155">各種不同類型的篩選器會以特定順序執行。</span><span class="sxs-lookup"><span data-stu-id="33d66-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="33d66-156">如果您想要控制在其中執行相同類型的篩選條件的順序，您可以設定篩選條件的順序屬性。</span><span class="sxs-lookup"><span data-stu-id="33d66-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="33d66-157">所有動作篩選條件的基底類別是`System.Web.Mvc.FilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="33d66-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="33d66-158">如果您想要實作特定類型的篩選條件，則您需要建立一個類別，繼承自基底的篩選條件類別並實作一或多個`IAuthorizationFilter`， `IActionFilter`， `IResultFilter`，或`IExceptionFilter`介面。</span><span class="sxs-lookup"><span data-stu-id="33d66-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="33d66-159">基底的 Actionfilterattribut 類別</span><span class="sxs-lookup"><span data-stu-id="33d66-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="33d66-160">為了讓您更輕鬆地實作自訂動作篩選條件，ASP.NET MVC 架構包括基底`ActionFilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="33d66-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="33d66-161">這個類別會實作`IActionFilter`並`IResultFilter`介面，並繼承自`Filter`類別。</span><span class="sxs-lookup"><span data-stu-id="33d66-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="33d66-162">無法完全一致的術語。</span><span class="sxs-lookup"><span data-stu-id="33d66-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="33d66-163">技術上來說，從 Actionfilterattribut 類別繼承的類別是動作篩選條件和結果篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="33d66-164">不過，鬆散的意義而言，文字的動作篩選條件用來參考任何類型的 ASP.NET MVC 架構中的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="33d66-165">基底`ActionFilterAttribute`類別具有可以覆寫下列方法：</span><span class="sxs-lookup"><span data-stu-id="33d66-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="33d66-166">OnActionExecuting – 執行控制器動作之前，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="33d66-167">OnActionExecuted – 控制器動作執行之後，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="33d66-168">OnResultExecuting – 控制器動作結果執行之前，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="33d66-169">OnResultExecuted – 控制器動作結果執行之後，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="33d66-170">在下一步 區段中，我們會看到如何實作每一種不同的方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="33d66-171">建立記錄的動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="33d66-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="33d66-172">若要說明如何建置自訂動作篩選條件，我們將建立自訂動作篩選條件的記錄處理 Visual Studio 輸出 視窗的控制器動作的階段。</span><span class="sxs-lookup"><span data-stu-id="33d66-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="33d66-173">我們`LogActionFilter`列表 2 中包含。</span><span class="sxs-lookup"><span data-stu-id="33d66-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="33d66-174">**列表 2 – `ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="33d66-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="33d66-175">在列表 2 中， `OnActionExecuting()`， `OnActionExecuted()`， `OnResultExecuting()`，以及`OnResultExecuted()`方法的所有呼叫`Log()`方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="33d66-176">方法的名稱和目前的路由資料傳遞至`Log()`方法。</span><span class="sxs-lookup"><span data-stu-id="33d66-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="33d66-177">`Log()`方法會將訊息寫入 Visual Studio 輸出 視窗 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="33d66-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="33d66-178">[![寫入至 Visual Studio [輸出] 視窗](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="33d66-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="33d66-179">**圖 02**： 寫入至 Visual Studio [輸出] 視窗 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="33d66-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="33d66-180">在 列表 3 中的主控制器會說明如何將記錄的動作篩選條件套用至整個控制器類別。</span><span class="sxs-lookup"><span data-stu-id="33d66-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="33d66-181">每當任何由主控制器的動作會叫用 – 請`Index()`方法或`About()`方法 – 動作都會記錄到 Visual Studio [輸出] 視窗的處理階段。</span><span class="sxs-lookup"><span data-stu-id="33d66-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="33d66-182">**列表 3 – `Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="33d66-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="33d66-183">總結</span><span class="sxs-lookup"><span data-stu-id="33d66-183">Summary</span></span>

<span data-ttu-id="33d66-184">在本教學課程中，已向您介紹 ASP.NET MVC 動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="33d66-185">您已了解四種不同類型的篩選： 授權篩選條件、 動作篩選條件、 結果篩選條件和例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="33d66-186">您也學會了基底`ActionFilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="33d66-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="33d66-187">最後，您已了解如何實作簡單動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="33d66-188">我們會建立記錄檔的處理 Visual Studio 輸出 視窗的控制器動作階段記錄動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="33d66-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="33d66-189">[上一頁](asp-net-mvc-routing-overview-cs.md)
> [下一頁](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="33d66-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
