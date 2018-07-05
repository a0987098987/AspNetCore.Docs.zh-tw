---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: 建立 ASP.NET MVC 應用程式 (VB) 的單元測試 |Microsoft Docs
author: StephenWalther
description: 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，Stephen Walther 會示範如何測試控制器動作傳回 parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edd134f6534b2a53be7f475cf0cb35ca93d3067
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384079"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="2c53f-104">建立單元測試的 ASP.NET MVC 應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="2c53f-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="2c53f-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2c53f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="2c53f-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="2c53f-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="2c53f-107">了解如何建立適用於控制器動作的單元測試。</span><span class="sxs-lookup"><span data-stu-id="2c53f-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="2c53f-108">在本教學課程中，Stephen Walther 會示範如何將測試控制器動作傳回的特定檢視，傳回一組特定的資料，或傳回不同類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="2c53f-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="2c53f-109">本教學課程的目的在於示範如何撰寫單元測試的控制站在您的 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c53f-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="2c53f-110">我們會討論如何建置三種不同的單元測試。</span><span class="sxs-lookup"><span data-stu-id="2c53f-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="2c53f-111">您將了解如何在測試控制器動作所傳回之檢視、 如何在測試控制器動作，所傳回的檢視資料以及如何測試或有一個控制器動作會將您導向第二個控制器動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="2c53f-112">建立測試控制器</span><span class="sxs-lookup"><span data-stu-id="2c53f-112">Creating the Controller under Test</span></span>

<span data-ttu-id="2c53f-113">現在就開始建立我們想要測試的控制器。</span><span class="sxs-lookup"><span data-stu-id="2c53f-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="2c53f-114">控制器中，名為`ProductController`，包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="2c53f-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="2c53f-115">**列表 1 – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="2c53f-116">`ProductController`包含兩個動作方法，名為`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="2c53f-117">這兩個動作方法傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="2c53f-117">Both action methods return a view.</span></span> <span data-ttu-id="2c53f-118">請注意，`Details()`動作接受參數，名為識別碼。</span><span class="sxs-lookup"><span data-stu-id="2c53f-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="2c53f-119">測試檢視傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="2c53f-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="2c53f-120">假設我們想要測試是否`ProductController`傳回正確的檢視。</span><span class="sxs-lookup"><span data-stu-id="2c53f-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="2c53f-121">我們想要確定，當`ProductController.Details()`叫用動作時，會傳回詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="2c53f-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="2c53f-122">在 列表 2 中的測試類別包含測試所傳回之檢視的單元測試`ProductController.Details()`動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="2c53f-123">**列表 2 – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="2c53f-124">列表 2 中的類別包含名為測試方法`TestDetailsView()`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="2c53f-125">這個方法包含三行程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c53f-125">This method contains three lines of code.</span></span> <span data-ttu-id="2c53f-126">第一行程式碼建立的新執行個體`ProductController`類別。</span><span class="sxs-lookup"><span data-stu-id="2c53f-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="2c53f-127">第二行程式碼會叫用控制器的`Details()`動作方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="2c53f-128">最後，程式碼檢查檢視是否傳回的最後一行`Details()`動作是 詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="2c53f-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="2c53f-129">`ViewResult.ViewName`屬性代表傳回的控制站之檢視的名稱。</span><span class="sxs-lookup"><span data-stu-id="2c53f-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="2c53f-130">測試此屬性相關的一個重要警告。</span><span class="sxs-lookup"><span data-stu-id="2c53f-130">One big warning about testing this property.</span></span> <span data-ttu-id="2c53f-131">有兩種方式，控制站可以傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="2c53f-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="2c53f-132">控制器可以明確地傳回的檢視，像這樣：</span><span class="sxs-lookup"><span data-stu-id="2c53f-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="2c53f-133">或者，從控制器動作，就像這樣的名稱，可以推斷檢視的名稱：</span><span class="sxs-lookup"><span data-stu-id="2c53f-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="2c53f-134">此控制器動作也會傳回名為檢視`Details`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="2c53f-135">不過，檢視的名稱是從動作名稱來推斷。</span><span class="sxs-lookup"><span data-stu-id="2c53f-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="2c53f-136">如果您想要測試的檢視名稱，然後您必須明確地傳回檢視表名稱從控制器動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="2c53f-137">您也可以輸入鍵盤組合在列表 2 中執行單元測試**CTRL-R、 A**或按一下**執行方案中的所有測試**按鈕 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="2c53f-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="2c53f-138">如果測試成功，您會看到 [圖 2] 中的 [測試結果] 視窗。</span><span class="sxs-lookup"><span data-stu-id="2c53f-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="2c53f-139">[![在方案中執行所有測試](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2c53f-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="2c53f-140">**圖 01**： 在方案中執行所有測試 ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2c53f-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="2c53f-141">[![成功 ！](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2c53f-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="2c53f-142">**圖 02**： 成功 ！</span><span class="sxs-lookup"><span data-stu-id="2c53f-142">**Figure 02**: Success!</span></span> <span data-ttu-id="2c53f-143">([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2c53f-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="2c53f-144">測試檢視資料傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="2c53f-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="2c53f-145">此 MVC 控制器會將資料傳遞至檢視使用所謂*`View Data`*。</span><span class="sxs-lookup"><span data-stu-id="2c53f-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="2c53f-146">例如，假設您想要顯示特定產品的詳細資料，當您叫用`ProductController Details()`動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="2c53f-147">在此情況下，您可以在其中建立的執行個體`Product`（在模型中定義） 的類別，並傳遞要的執行個體`Details`利用檢視`View Data`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="2c53f-148">已修改`ProductController`包含 列表 3 中的 更新`Details()`傳回產品的動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="2c53f-149">**列表 3 – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="2c53f-150">首先，`Details()`動作會建立的新執行個體`Product`類別，表示在膝上型電腦。</span><span class="sxs-lookup"><span data-stu-id="2c53f-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="2c53f-151">接下來，執行個體`Product`做為第二個參數來傳遞類別`View()`方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="2c53f-152">您可以撰寫單元測試來測試是否為預期的資料包含在檢視中的資料。</span><span class="sxs-lookup"><span data-stu-id="2c53f-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="2c53f-153">單元測試列表 4 測試中，當您呼叫時，會傳回代表膝上型電腦的產品是否`ProductController Details()`動作方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="2c53f-154">**列表 4 – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="2c53f-155">在 [列表 4]`TestDetailsView()`方法會測試所叫用傳回的檢視資料`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="2c53f-156">`ViewData`上公開為屬性`ViewResult`藉由叫用傳回`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="2c53f-157">`ViewData.Model`屬性包含傳遞至檢視的產品。</span><span class="sxs-lookup"><span data-stu-id="2c53f-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="2c53f-158">測試只會確認產品檢視資料中包含的膝上型電腦的名稱。</span><span class="sxs-lookup"><span data-stu-id="2c53f-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="2c53f-159">測試動作結果傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="2c53f-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="2c53f-160">更複雜的控制器動作可能會傳回不同類型的動作取決於值中的結果傳遞至控制器動作的參數。</span><span class="sxs-lookup"><span data-stu-id="2c53f-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="2c53f-161">控制器動作可以傳回各種類型的動作結果，包括`ViewResult`， `RedirectToRouteResult`，或`JsonResult`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="2c53f-162">例如，已修改`Details()`表 5 中的動作傳回`Details`檢視當您將有效的產品識別碼傳遞至動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="2c53f-163">如果您傳遞無效的產品識別碼，識別碼的值小於 1，則您會重新導向至`Index()`動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="2c53f-164">**列表 5 – `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="2c53f-165">您可以測試的行為`Details()`與單元測試列表 6 中的動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="2c53f-166">在 列表 6 中的單元測試驗證，將您重新導向`Index`檢視識別碼為-1 的值傳遞至`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="2c53f-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="2c53f-167">**列表 6 – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="2c53f-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="2c53f-168">當您呼叫`RedirectToAction()`方法的控制器動作中，控制器動作傳回`RedirectToRouteResult`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="2c53f-169">測試檢查是否`RedirectToRouteResult`至名為控制器動作會將使用者重新導向`Index`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="2c53f-170">總結</span><span class="sxs-lookup"><span data-stu-id="2c53f-170">Summary</span></span>

<span data-ttu-id="2c53f-171">在本教學課程中，您已了解如何建置適用於 MVC 控制器動作的單元測試。</span><span class="sxs-lookup"><span data-stu-id="2c53f-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="2c53f-172">首先，您已了解如何確認正確的檢視由控制器動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="2c53f-173">您已了解如何使用`ViewResult.ViewName`屬性，確認檢視的名稱。</span><span class="sxs-lookup"><span data-stu-id="2c53f-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="2c53f-174">接下來，我們檢查如何測試的內容`View Data`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="2c53f-175">您已了解如何檢查是否已傳回正確的產品`View Data`之後呼叫控制器動作。</span><span class="sxs-lookup"><span data-stu-id="2c53f-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="2c53f-176">最後，我們會討論如何測試是否從控制器動作傳回不同類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="2c53f-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="2c53f-177">您已了解如何測試是否會傳回一個控制站`ViewResult`或`RedirectToRouteResult`。</span><span class="sxs-lookup"><span data-stu-id="2c53f-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2c53f-178">上一步</span><span class="sxs-lookup"><span data-stu-id="2c53f-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
