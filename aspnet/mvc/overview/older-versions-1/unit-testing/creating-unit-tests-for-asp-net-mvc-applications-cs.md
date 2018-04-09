---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: 建立單元測試的 ASP.NET MVC 應用程式 (C#) |Microsoft 文件
author: StephenWalther
description: 了解如何建立適用於控制器動作的單元測試。 在本教學課程中，作者： Stephen Walther 會示範如何測試是否控制器動作傳回 parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: ccd9a1b3aee8379c23c01c5eb7f756a786f6359d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="fe515-104">建立單元測試的 ASP.NET MVC 應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="fe515-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="fe515-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="fe515-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="fe515-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="fe515-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="fe515-107">了解如何建立適用於控制器動作的單元測試。</span><span class="sxs-lookup"><span data-stu-id="fe515-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="fe515-108">在本教學課程中，作者： Stephen Walther 會示範如何測試控制器動作傳回的特定檢視，傳回一組特定的資料，或傳回不同類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="fe515-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="fe515-109">本教學課程的目標是為了示範如何撰寫單元測試的控制站在 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fe515-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="fe515-110">我們會討論如何建立單元測試的三種不同的類型。</span><span class="sxs-lookup"><span data-stu-id="fe515-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="fe515-111">您了解如何測試控制器動作傳回的檢視、 測試控制器動作，所傳回的檢視資料以及如何測試一個控制器動作將您重新導向至第二個控制器動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="fe515-112">建立測試控制器</span><span class="sxs-lookup"><span data-stu-id="fe515-112">Creating the Controller under Test</span></span>

<span data-ttu-id="fe515-113">讓我們開始建立我們想要測試的控制器。</span><span class="sxs-lookup"><span data-stu-id="fe515-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="fe515-114">具名的控制站`ProductController`，包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="fe515-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="fe515-115">**列出 1 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="fe515-116">`ProductController`包含兩個動作方法，名為`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="fe515-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="fe515-117">這兩個動作方法傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-117">Both action methods return a view.</span></span> <span data-ttu-id="fe515-118">請注意，`Details()`動作接受參數，名為識別碼。</span><span class="sxs-lookup"><span data-stu-id="fe515-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="fe515-119">測試檢視傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="fe515-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="fe515-120">假設我們想要測試是否`ProductController`傳回右方檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="fe515-121">我們想要確定當`ProductController.Details()`叫用動作時，會傳回詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="fe515-122">列出 2 中的測試類別包含單元測試來測試所傳回的檢視`ProductController.Details()`動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="fe515-123">**列出 2 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="fe515-124">列出 2 中的類別包含名為的測試方法`TestDetailsView()`。</span><span class="sxs-lookup"><span data-stu-id="fe515-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="fe515-125">這個方法包含三行程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe515-125">This method contains three lines of code.</span></span> <span data-ttu-id="fe515-126">第一行程式碼建立的新執行個體`ProductController`類別。</span><span class="sxs-lookup"><span data-stu-id="fe515-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="fe515-127">程式碼的第二行叫用控制器的`Details()`動作方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="fe515-128">最後，程式碼會檢查檢視是否傳回的最後一行`Details()`動作是詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="fe515-129">`ViewResult.ViewName`屬性表示檢視傳回的控制站的名稱。</span><span class="sxs-lookup"><span data-stu-id="fe515-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="fe515-130">關於測試這個屬性的其中一個大警告。</span><span class="sxs-lookup"><span data-stu-id="fe515-130">One big warning about testing this property.</span></span> <span data-ttu-id="fe515-131">有兩種方式，控制站可以傳回的檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="fe515-132">控制器可以明確傳回檢視就像這樣：</span><span class="sxs-lookup"><span data-stu-id="fe515-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="fe515-133">或者，可以推斷檢視的名稱，從控制器動作，就像這樣的名稱：</span><span class="sxs-lookup"><span data-stu-id="fe515-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="fe515-134">此控制器動作也會傳回名為檢視`Details`。</span><span class="sxs-lookup"><span data-stu-id="fe515-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="fe515-135">不過，檢視的名稱被推斷的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="fe515-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="fe515-136">如果您想要測試的檢視名稱，然後您必須明確地傳回檢視表名稱從控制器動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="fe515-137">您也可以輸入鍵盤組合清單 2 中執行單元測試**Ctrl-R、 A**或按一下**方案中執行所有測試**按鈕 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="fe515-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="fe515-138">如果測試成功，您會看到在圖 2 中的測試結果 視窗。</span><span class="sxs-lookup"><span data-stu-id="fe515-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="fe515-139">[![在方案中執行所有測試](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fe515-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="fe515-140">**圖 01**： 方案中執行所有測試 ([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fe515-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="fe515-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fe515-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="fe515-142">**圖 02**： 成功 ！</span><span class="sxs-lookup"><span data-stu-id="fe515-142">**Figure 02**: Success!</span></span> <span data-ttu-id="fe515-143">([按一下以檢視完整大小的影像](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fe515-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="fe515-144">測試檢視資料傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="fe515-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="fe515-145">此 MVC 控制器會將資料傳遞至檢視使用所謂*`View Data`*。</span><span class="sxs-lookup"><span data-stu-id="fe515-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="fe515-146">例如，假設您想要顯示特定產品的詳細資料，當您叫用`ProductController Details()`動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="fe515-147">在此情況下，您可以建立的執行個體`Product`類別 （定義於您的模型），並傳遞要的執行個體`Details`檢視藉由運用`View Data`。</span><span class="sxs-lookup"><span data-stu-id="fe515-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="fe515-148">修改`ProductController`中列出的 3 包含更新`Details()`傳回產品的動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="fe515-149">**列出 3 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="fe515-150">首先，`Details()`動作建立的新執行個體`Product`表示膝上型電腦類別。</span><span class="sxs-lookup"><span data-stu-id="fe515-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="fe515-151">下一步，執行個體`Product`類別會當做第二個參數傳遞`View()`方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="fe515-152">您可以撰寫單元測試來測試是否為預期的資料包含在檢視中的資料。</span><span class="sxs-lookup"><span data-stu-id="fe515-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="fe515-153">單元測試中列出的 4 測試是否會傳回代表膝上型電腦的產品，當您呼叫`ProductController Details()`動作方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="fe515-154">**列出 4 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="fe515-155">列出第 4`TestDetailsView()`方法會測試所叫用傳回的檢視資料`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="fe515-156">`ViewData`上公開為屬性`ViewResult`所叫用傳回`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="fe515-157">`ViewData.Model`屬性包含傳遞至檢視的產品。</span><span class="sxs-lookup"><span data-stu-id="fe515-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="fe515-158">測試只會驗證檢視資料中包含產品具有膝上型電腦的名稱。</span><span class="sxs-lookup"><span data-stu-id="fe515-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="fe515-159">測試動作結果傳回的控制站</span><span class="sxs-lookup"><span data-stu-id="fe515-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="fe515-160">更複雜的控制器動作可能會傳回不同類型的動作結果取決於值傳遞至控制器動作的參數。</span><span class="sxs-lookup"><span data-stu-id="fe515-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="fe515-161">控制器動作，可以傳回各種類型的動作結果，包括`ViewResult`， `RedirectToRouteResult`，或`JsonResult`。</span><span class="sxs-lookup"><span data-stu-id="fe515-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="fe515-162">例如，已修改`Details()`列出 5 中的動作傳回`Details`檢視當您將有效的產品識別碼傳遞至動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="fe515-163">如果您傳遞無效的產品識別碼-識別碼的值小於 1 部--就會重新導向至`Index()`動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="fe515-164">**列出 5 – `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="fe515-165">您可以測試的行為`Details()`與單元測試程式碼範例 6 中的動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="fe515-166">列出第 6 單元測試會驗證您會重新導向至`Index`檢視時的識別碼值為-1 會傳遞至`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="fe515-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="fe515-167">**列出 6 – `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="fe515-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="fe515-168">當您呼叫`RedirectToAction()`方法控制器動作中的，控制器動作傳回`RedirectToRouteResult`。</span><span class="sxs-lookup"><span data-stu-id="fe515-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="fe515-169">測試檢查是否`RedirectToRouteResult`至名為控制器動作會將使用者重新導向`Index`。</span><span class="sxs-lookup"><span data-stu-id="fe515-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="fe515-170">總結</span><span class="sxs-lookup"><span data-stu-id="fe515-170">Summary</span></span>

<span data-ttu-id="fe515-171">在本教學課程中，您將學會如何建立單元測試的 MVC 控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="fe515-172">首先，您學會了如何以確認控制器動作是否傳回右方檢視。</span><span class="sxs-lookup"><span data-stu-id="fe515-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="fe515-173">您已學習如何使用`ViewResult.ViewName`屬性，確認為檢視的名稱。</span><span class="sxs-lookup"><span data-stu-id="fe515-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="fe515-174">接下來，我們檢驗如何測試的內容`View Data`。</span><span class="sxs-lookup"><span data-stu-id="fe515-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="fe515-175">您已學會如何以檢查是否已傳回正確的產品`View Data`之後呼叫控制器動作。</span><span class="sxs-lookup"><span data-stu-id="fe515-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="fe515-176">最後，我們將討論如何測試是否從控制器動作傳回不同類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="fe515-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="fe515-177">您已學習如何測試是否為控制站傳回`ViewResult`或`RedirectToRouteResult`。</span><span class="sxs-lookup"><span data-stu-id="fe515-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fe515-178">下一步</span><span class="sxs-lookup"><span data-stu-id="fe515-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
