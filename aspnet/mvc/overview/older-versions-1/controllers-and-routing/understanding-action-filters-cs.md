---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: 了解動作篩選條件 (C#) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是以說明動作篩選條件。 動作篩選條件是屬性，您可以套用至控制器動作--或整個控制器...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d68933297329370e227f524c4b96ed7e259ef833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870410"
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="d27db-104">了解動作篩選條件 (C#)</span><span class="sxs-lookup"><span data-stu-id="d27db-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="d27db-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d27db-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d27db-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="d27db-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="d27db-107">本教學課程的目標是以說明動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="d27db-108">動作篩選條件是您可以套用至控制器動作--或整個 controller--修改的方式執行動作的屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="d27db-109">了解動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="d27db-109">Understanding Action Filters</span></span>

<span data-ttu-id="d27db-110">本教學課程的目標是以說明動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="d27db-111">動作篩選條件是您可以套用至控制器動作--或整個 controller--修改的方式執行動作的屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="d27db-112">ASP.NET MVC 架構包括數個動作篩選條件：</span><span class="sxs-lookup"><span data-stu-id="d27db-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="d27db-113">OutputCache – 此動作篩選條件會快取指定的時間量的控制器動作的輸出。</span><span class="sxs-lookup"><span data-stu-id="d27db-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="d27db-114">HandleError – 此動作篩選條件來處理控制器動作執行時引發的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d27db-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="d27db-115">授權-本動作篩選條件可讓您限制存取特定使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="d27db-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="d27db-116">您也可以建立您自己的自訂動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="d27db-117">例如，您可以建立自訂動作篩選條件，以實作自訂驗證系統。</span><span class="sxs-lookup"><span data-stu-id="d27db-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="d27db-118">或者，您可能想要建立動作篩選條件，以修改控制器動作所傳回的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="d27db-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="d27db-119">在本教學課程中，您可以了解如何建置從頭動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="d27db-120">我們建立了記錄動作篩選條件的不同階段的處理動作記錄至 Visual Studio 輸出視窗。</span><span class="sxs-lookup"><span data-stu-id="d27db-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="d27db-121">使用動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="d27db-121">Using an Action Filter</span></span>

<span data-ttu-id="d27db-122">動作篩選條件是屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-122">An action filter is an attribute.</span></span> <span data-ttu-id="d27db-123">您可以將大部分的動作篩選條件套用至個別控制器動作或整個控制器。</span><span class="sxs-lookup"><span data-stu-id="d27db-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="d27db-124">例如，資料中的控制站清單 1 會公開名為動作`Index()`，傳回目前的時間。</span><span class="sxs-lookup"><span data-stu-id="d27db-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="d27db-125">這個動作以裝飾`OutputCache`動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="d27db-126">此篩選條件會導致快取為 10 秒的動作所傳回的值。</span><span class="sxs-lookup"><span data-stu-id="d27db-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="d27db-127">**列出 1 – `Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="d27db-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="d27db-128">如果您重複叫用`Index()`動作將您的瀏覽器的網址列輸入 URL/資料/索引，並按 [重新整理] 按鈕多次，您將會看到相同的時間為 10 秒。</span><span class="sxs-lookup"><span data-stu-id="d27db-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="d27db-129">輸出`Index()`動作會快取 （請參閱圖 1） 的 10 秒。</span><span class="sxs-lookup"><span data-stu-id="d27db-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="d27db-130">[![快取的時間](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d27db-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="d27db-131">**圖 01**： 快取時間 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d27db-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="d27db-132">在清單 1 中，單一動作篩選 –`OutputCache`動作篩選條件 – 套用至`Index()`方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="d27db-133">如果您需要您可以套用多個動作篩選條件至相同的動作。</span><span class="sxs-lookup"><span data-stu-id="d27db-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="d27db-134">例如，您可能想要套用這兩`OutputCache`和`HandleError`動作篩選條件至相同的動作。</span><span class="sxs-lookup"><span data-stu-id="d27db-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="d27db-135">列出第 1`OutputCache`動作篩選條件套用至`Index()`動作。</span><span class="sxs-lookup"><span data-stu-id="d27db-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="d27db-136">您也可以將套用此屬性為`DataController`類別本身。</span><span class="sxs-lookup"><span data-stu-id="d27db-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="d27db-137">在此情況下，任何由控制器的動作所傳回的結果會快取 10 秒。</span><span class="sxs-lookup"><span data-stu-id="d27db-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="d27db-138">不同類型的篩選器</span><span class="sxs-lookup"><span data-stu-id="d27db-138">The Different Types of Filters</span></span>

<span data-ttu-id="d27db-139">ASP.NET MVC 架構支援四種不同類型的篩選：</span><span class="sxs-lookup"><span data-stu-id="d27db-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="d27db-140">授權篩選 – 實作`IAuthorizationFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="d27db-141">動作篩選 – 實作`IActionFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="d27db-142">產生篩選 – 實作`IResultFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="d27db-143">例外狀況篩選條件 – 實作`IExceptionFilter`屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="d27db-144">篩選條件的執行以上所列的順序。</span><span class="sxs-lookup"><span data-stu-id="d27db-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="d27db-145">例如，授權篩選條件一定會執行動作篩選條件之前，並篩選器的型別之後一律執行例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="d27db-146">授權篩選條件可用於實作驗證和授權適用於控制器動作。</span><span class="sxs-lookup"><span data-stu-id="d27db-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="d27db-147">例如，授權篩選條件是授權篩選條件的範例。</span><span class="sxs-lookup"><span data-stu-id="d27db-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="d27db-148">動作篩選條件包含控制器動作執行前後執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d27db-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="d27db-149">您可以修改控制器動作傳回的檢視資料，比方說，使用動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="d27db-150">結果篩選條件會包含執行之前和之後執行檢視結果的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d27db-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="d27db-151">例如，您可能想要修改檢視結果 檢視呈現至瀏覽器之前，以滑鼠右鍵。</span><span class="sxs-lookup"><span data-stu-id="d27db-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="d27db-152">例外狀況篩選條件會篩選條件執行的最後一個類型。</span><span class="sxs-lookup"><span data-stu-id="d27db-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="d27db-153">您可以使用例外狀況篩選條件來處理您的控制器動作或控制器動作結果所引發的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d27db-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="d27db-154">您也可以使用例外狀況篩選條件來記錄錯誤。</span><span class="sxs-lookup"><span data-stu-id="d27db-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="d27db-155">以特定順序執行各種不同類型的篩選器。</span><span class="sxs-lookup"><span data-stu-id="d27db-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="d27db-156">如果您想要控制的執行相同類型的篩選條件的順序，您可以設定篩選器順序屬性。</span><span class="sxs-lookup"><span data-stu-id="d27db-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="d27db-157">所有動作篩選條件的基底類別是`System.Web.Mvc.FilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="d27db-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="d27db-158">如果您想要實作特定類型的篩選條件，則您必須建立繼承自基底的篩選條件類別並實作一或多個類別`IAuthorizationFilter`， `IActionFilter`， `IResultFilter`，或`IExceptionFilter`介面。</span><span class="sxs-lookup"><span data-stu-id="d27db-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="d27db-159">基底的 Actionfilterattribut 類別</span><span class="sxs-lookup"><span data-stu-id="d27db-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="d27db-160">為了讓您更輕鬆地實作自訂動作篩選條件，ASP.NET MVC 架構包括基底`ActionFilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="d27db-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="d27db-161">這個類別會同時實作`IActionFilter`和`IResultFilter`介面，並繼承自`Filter`類別。</span><span class="sxs-lookup"><span data-stu-id="d27db-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="d27db-162">無法完全一致的術語。</span><span class="sxs-lookup"><span data-stu-id="d27db-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="d27db-163">技術上來說，從 Actionfilterattribut 類別繼承的類別是動作篩選條件和結果篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="d27db-164">不過，鬆散的意義而言，文字的動作篩選條件用來參考任何一種 ASP.NET MVC 架構中的篩選器。</span><span class="sxs-lookup"><span data-stu-id="d27db-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="d27db-165">基底`ActionFilterAttribute`類別具有可以覆寫下列方法：</span><span class="sxs-lookup"><span data-stu-id="d27db-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="d27db-166">OnActionExecuting – 執行控制器動作之前，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="d27db-167">OnActionExecuted – 控制器動作執行之後，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="d27db-168">OnResultExecuting – 控制器動作結果執行之前，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="d27db-169">OnResultExecuted – 控制器動作結果執行之後，會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="d27db-170">在下一步 區段中，我們會看到實作每個不同方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="d27db-171">建立記錄動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="d27db-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="d27db-172">若要說明如何建置自訂動作篩選條件，我們將建立自訂動作篩選條件的記錄檔的階段處理控制器動作，以 Visual Studio 輸出視窗。</span><span class="sxs-lookup"><span data-stu-id="d27db-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="d27db-173">我們`LogActionFilter`包含清單 2 中。</span><span class="sxs-lookup"><span data-stu-id="d27db-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="d27db-174">**列出 2 – `ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="d27db-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="d27db-175">在列出 2 中， `OnActionExecuting()`， `OnActionExecuted()`， `OnResultExecuting()`，和`OnResultExecuted()`所有方法都呼叫`Log()`方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="d27db-176">方法的名稱和目前的路由資料傳遞至`Log()`方法。</span><span class="sxs-lookup"><span data-stu-id="d27db-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="d27db-177">`Log()`方法會將訊息寫入 Visual Studio 輸出 視窗 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="d27db-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="d27db-178">[![寫入至 Visual Studio [輸出] 視窗](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d27db-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="d27db-179">**圖 02**： 寫入至 Visual Studio [輸出] 視窗 ([按一下以檢視完整大小的影像](understanding-action-filters-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d27db-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="d27db-180">Home 控制器中列出的 3 說明如何將記錄動作篩選條件套用至整個控制器類別。</span><span class="sxs-lookup"><span data-stu-id="d27db-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="d27db-181">每當任何由主控制器的動作會叫用 – 請`Index()`方法或`About()`方法-動作會記錄至 Visual Studio [輸出] 視窗的處理的階段。</span><span class="sxs-lookup"><span data-stu-id="d27db-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="d27db-182">**列出 3 – `Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="d27db-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="d27db-183">總結</span><span class="sxs-lookup"><span data-stu-id="d27db-183">Summary</span></span>

<span data-ttu-id="d27db-184">在本教學課程中，您是導入 ASP.NET MVC 動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="d27db-185">您發現了四個不同類型的篩選： 授權篩選條件、 動作篩選條件、 結果篩選條件和例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="d27db-186">您也學到基底`ActionFilterAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="d27db-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="d27db-187">最後，您學會如何實作簡單動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="d27db-188">我們建立記錄檔的階段處理 Visual Studio 輸出 視窗的控制器動作記錄動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d27db-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d27db-189">[上一頁](asp-net-mvc-routing-overview-cs.md)
> [下一頁](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d27db-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
