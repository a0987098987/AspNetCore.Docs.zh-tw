---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: "將資料傳遞至檢視主版頁面 (C#) |Microsoft 文件"
author: microsoft
description: "本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。 我們會檢查兩種策略將資料傳遞至檢視 m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc8ce0690d2e45877be75011d8883facbc74a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-c"></a><span data-ttu-id="cda27-104">將資料傳遞至檢視主版頁面 (C#)</span><span class="sxs-lookup"><span data-stu-id="cda27-104">Passing Data to View Master Pages (C#)</span></span>
====================
<span data-ttu-id="cda27-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cda27-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="cda27-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="cda27-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> <span data-ttu-id="cda27-107">本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-107">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="cda27-108">我們會檢查兩種策略將資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-108">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="cda27-109">首先，我們會討論簡單的解決方案所產生的應用程式，很難以維護。</span><span class="sxs-lookup"><span data-stu-id="cda27-109">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="cda27-110">接下來，我們會檢查比較好的解決方案有點需要更多的初始工作，但更容易維護的應用程式中的結果。</span><span class="sxs-lookup"><span data-stu-id="cda27-110">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>


## <a name="passing-data-to-view-master-pages"></a><span data-ttu-id="cda27-111">將資料傳遞至檢視主版頁面</span><span class="sxs-lookup"><span data-stu-id="cda27-111">Passing Data to View Master Pages</span></span>

<span data-ttu-id="cda27-112">本教學課程的目標是要說明如何將資料從控制器檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-112">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="cda27-113">我們會檢查兩種策略將資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-113">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="cda27-114">首先，我們會討論簡單的解決方案所產生的應用程式，很難以維護。</span><span class="sxs-lookup"><span data-stu-id="cda27-114">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="cda27-115">接下來，我們會檢查比較好的解決方案有點需要更多的初始工作，但更容易維護的應用程式中的結果。</span><span class="sxs-lookup"><span data-stu-id="cda27-115">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>

### <a name="the-problem"></a><span data-ttu-id="cda27-116">問題</span><span class="sxs-lookup"><span data-stu-id="cda27-116">The Problem</span></span>

<span data-ttu-id="cda27-117">假設您要建置影片資料庫應用程式，而您想要在應用程式中的每一頁上顯示電影類別目錄的清單 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="cda27-117">Imagine that you are building a movie database application and you want to display the list of movie categories on every page in your application (see Figure 1).</span></span> <span data-ttu-id="cda27-118">此外，假設影片類別目錄的清單，會儲存在資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="cda27-118">Imagine, furthermore, that the list of movie categories is stored in a database table.</span></span> <span data-ttu-id="cda27-119">在此情況下，它會使意義上，從資料庫擷取類別，並呈現檢視主版頁面中的影片分類清單。</span><span class="sxs-lookup"><span data-stu-id="cda27-119">In that case, it would make sense to retrieve the categories from the database and render the list of movie categories within a view master page.</span></span>


<span data-ttu-id="cda27-120">[![顯示在檢視主版頁面的電影類別](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cda27-120">[![Displaying movie categories in a view master page](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)</span></span>

<span data-ttu-id="cda27-121">**圖 01**： 電影類別顯示在檢視主版頁面 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cda27-121">**Figure 01**: Displaying movie categories in a view master page ([Click to view full-size image](passing-data-to-view-master-pages-cs/_static/image3.png))</span></span>


<span data-ttu-id="cda27-122">以下是此問題。</span><span class="sxs-lookup"><span data-stu-id="cda27-122">Here's the problem.</span></span> <span data-ttu-id="cda27-123">如何擷取主版頁面中的影片類別目錄的清單？</span><span class="sxs-lookup"><span data-stu-id="cda27-123">How do you retrieve the list of movie categories in the master page?</span></span> <span data-ttu-id="cda27-124">它會嘗試直接呼叫主版頁面中的模型類別的方法。</span><span class="sxs-lookup"><span data-stu-id="cda27-124">It is tempting to call methods of your model classes in the master page directly.</span></span> <span data-ttu-id="cda27-125">換句話說，很容易包含從您的主版頁面的資料庫右邊擷取資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cda27-125">In other words, it is tempting to include the code for retrieving the data from the database right in your master page.</span></span> <span data-ttu-id="cda27-126">不過，略過您存取資料庫的 MVC 控制器會違反全新的重要性分離所建立的 MVC 應用程式的主要優點之一。</span><span class="sxs-lookup"><span data-stu-id="cda27-126">However, bypassing your MVC controllers to access the database would violate the clean separation of concerns that is one of the primary benefits of building an MVC application.</span></span>

<span data-ttu-id="cda27-127">在 MVC 應用程式，您會想 MVC 檢視與 MVC 模型，由 MVC 控制器之間的所有互動。</span><span class="sxs-lookup"><span data-stu-id="cda27-127">In an MVC application, you want all interaction between your MVC views and your MVC model to be handled by your MVC controllers.</span></span> <span data-ttu-id="cda27-128">此分離問題會導致更容易維護、 可調適性，且可測試的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cda27-128">This separation of concerns results in a more maintainable, adaptable, and testable application.</span></span>

<span data-ttu-id="cda27-129">MVC 應用程式中，所有的資料傳遞至 – 包括檢視主版頁面 – 檢視應該傳遞至檢視的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="cda27-129">In an MVC application, all data passed to a view – including a view master page – should be passed to a view by a controller action.</span></span> <span data-ttu-id="cda27-130">此外，資料應該傳遞利用檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-130">Furthermore, the data should be passed by taking advantage of view data.</span></span> <span data-ttu-id="cda27-131">在本教學課程的其餘部分，我會檢查傳遞給檢視主版頁面的 檢視資料的兩種方法。</span><span class="sxs-lookup"><span data-stu-id="cda27-131">In the remainder of this tutorial, I examine two methods of passing view data to a view master page.</span></span>

### <a name="the-simple-solution"></a><span data-ttu-id="cda27-132">簡單的解決方案</span><span class="sxs-lookup"><span data-stu-id="cda27-132">The Simple Solution</span></span>

<span data-ttu-id="cda27-133">讓我們從開始檢視主版頁面，從控制器傳遞檢視資料最簡單的解決方案。</span><span class="sxs-lookup"><span data-stu-id="cda27-133">Let's start with the simplest solution to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="cda27-134">最簡單的解決方案是在每個控制器動作中傳遞的主版頁面的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-134">The simplest solution is to pass the view data for the master page in each and every controller action.</span></span>

<span data-ttu-id="cda27-135">請考慮列出 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="cda27-135">Consider the controller in Listing 1.</span></span> <span data-ttu-id="cda27-136">它會公開兩個動作，名為`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="cda27-136">It exposes two actions named `Index()` and `Details()`.</span></span> <span data-ttu-id="cda27-137">`Index()`動作方法會傳回每個影片電影資料庫資料表中。</span><span class="sxs-lookup"><span data-stu-id="cda27-137">The `Index()` action method returns every movie in the Movies database table.</span></span> <span data-ttu-id="cda27-138">`Details()`動作方法會傳回每個影片中特定的影片類別目錄。</span><span class="sxs-lookup"><span data-stu-id="cda27-138">The `Details()` action method returns every movie in a particular movie category.</span></span>

<span data-ttu-id="cda27-139">**列出 1 –`Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="cda27-139">**Listing 1 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

<span data-ttu-id="cda27-140">請注意，index （） 和 Details() 動作加入兩個項目來檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-140">Notice that both the Index() and the Details() actions add two items to view data.</span></span> <span data-ttu-id="cda27-141">Index 動作便會新增兩個索引鍵： 類別目錄與電影。</span><span class="sxs-lookup"><span data-stu-id="cda27-141">The Index() action adds two keys: categories and movies.</span></span> <span data-ttu-id="cda27-142">類別索引鍵代表檢視主版頁面所顯示的電影類別目錄的清單。</span><span class="sxs-lookup"><span data-stu-id="cda27-142">The categories key represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="cda27-143">電影索引鍵代表索引檢視頁面所顯示的電影的清單。</span><span class="sxs-lookup"><span data-stu-id="cda27-143">The movies key represents the list of movies displayed by the Index view page.</span></span>

<span data-ttu-id="cda27-144">Details() 動作也會加入名為類別目錄與電影的兩個索引鍵。</span><span class="sxs-lookup"><span data-stu-id="cda27-144">The Details() action also adds two keys named categories and movies.</span></span> <span data-ttu-id="cda27-145">索引鍵的類別，同樣地，表示檢視主版頁面所顯示的電影類別目錄的清單。</span><span class="sxs-lookup"><span data-stu-id="cda27-145">The categories key, once again, represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="cda27-146">電影索引鍵所代表的特定分類的詳細資料檢視頁面所顯示的電影清單 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="cda27-146">The movies key represents the list of movies in a particular category displayed by the Details view page (see Figure 2).</span></span>


<span data-ttu-id="cda27-147">[![詳細資料檢視](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cda27-147">[![The Details view](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)</span></span>

<span data-ttu-id="cda27-148">**圖 02**: 詳細資料檢視 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cda27-148">**Figure 02**: The Details view ([Click to view full-size image](passing-data-to-view-master-pages-cs/_static/image6.png))</span></span>


<span data-ttu-id="cda27-149">索引檢視包含在清單 2。</span><span class="sxs-lookup"><span data-stu-id="cda27-149">The Index view is contained in Listing 2.</span></span> <span data-ttu-id="cda27-150">它只是逐一查看的電影中的項目檢視資料所代表的電影清單。</span><span class="sxs-lookup"><span data-stu-id="cda27-150">It simply iterates through the list of movies represented by the movies item in view data.</span></span>

<span data-ttu-id="cda27-151">**列出 2 –`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="cda27-151">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

<span data-ttu-id="cda27-152">檢視主版頁面都包含在列出的 3。</span><span class="sxs-lookup"><span data-stu-id="cda27-152">The view master page is contained in Listing 3.</span></span> <span data-ttu-id="cda27-153">檢視主版頁面反覆運算，並呈現所有以類別目錄項目表示從檢視資料的電影類別。</span><span class="sxs-lookup"><span data-stu-id="cda27-153">The view master page iterates and renders all of the movie categories represented by the categories item from view data.</span></span>

<span data-ttu-id="cda27-154">**列出 3 –`Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="cda27-154">**Listing 3 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

<span data-ttu-id="cda27-155">所有的資料會透過檢視資料傳遞至檢視和檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-155">All data is passed to the view and the view master page through view data.</span></span> <span data-ttu-id="cda27-156">這是正確的方式將資料傳遞至主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-156">That is the correct way to pass data to the master page.</span></span>

<span data-ttu-id="cda27-157">因此，什麼是此解決方案有什麼？</span><span class="sxs-lookup"><span data-stu-id="cda27-157">So, what's wrong with this solution?</span></span> <span data-ttu-id="cda27-158">問題是這個解決方案違反乾 （不重複自行） 原則。</span><span class="sxs-lookup"><span data-stu-id="cda27-158">The problem is that this solution violates the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="cda27-159">每個控制器動作必須完全相同之新增電影的分類，以檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-159">Each and every controller action must add the very same list of movie categories to view data.</span></span> <span data-ttu-id="cda27-160">在您的應用程式中具有重複的程式碼，可讓您的應用程式更難維護、 調整，以及修改。</span><span class="sxs-lookup"><span data-stu-id="cda27-160">Having duplicate code in your application makes your application much more difficult to maintain, adapt, and modify.</span></span>

### <a name="the-good-solution"></a><span data-ttu-id="cda27-161">很好的解決方案</span><span class="sxs-lookup"><span data-stu-id="cda27-161">The Good Solution</span></span>

<span data-ttu-id="cda27-162">在本節中，我們會檢查替代，以及更好，解決方案將資料從控制器動作傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-162">In this section, we examine an alternative, and better, solution to passing data from a controller action to a view master page.</span></span> <span data-ttu-id="cda27-163">而非新增主版頁面的電影分類中每個控制器動作，我們加入電影類別檢視資料一次。</span><span class="sxs-lookup"><span data-stu-id="cda27-163">Instead of adding the movie categories for the master page in each and every controller action, we add the movie categories to the view data only once.</span></span> <span data-ttu-id="cda27-164">使用檢視主版頁面的所有檢視資料會都加入應用程式控制器。</span><span class="sxs-lookup"><span data-stu-id="cda27-164">All view data used by the view master page is added in an Application controller.</span></span>

<span data-ttu-id="cda27-165">ApplicationController 類別被包含在列出的 4。</span><span class="sxs-lookup"><span data-stu-id="cda27-165">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="cda27-166">**列出 4 –`Controllers\ApplicationController.cs`**</span><span class="sxs-lookup"><span data-stu-id="cda27-166">**Listing 4 – `Controllers\ApplicationController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

<span data-ttu-id="cda27-167">有三個的項目，您應該注意到相關的應用程式中的控制站清單 4。</span><span class="sxs-lookup"><span data-stu-id="cda27-167">There are three things that you should notice about the Application controller in Listing 4.</span></span> <span data-ttu-id="cda27-168">首先，請注意，類別可以繼承自基底 System.Web.Mvc.Controller 類別。</span><span class="sxs-lookup"><span data-stu-id="cda27-168">First, notice that the class inherits from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="cda27-169">應用程式的控制站是控制器的類別。</span><span class="sxs-lookup"><span data-stu-id="cda27-169">The Application controller is a controller class.</span></span>

<span data-ttu-id="cda27-170">第二，請注意應用程式控制器類別是抽象類別。</span><span class="sxs-lookup"><span data-stu-id="cda27-170">Second, notice that the Application controller class is an abstract class.</span></span> <span data-ttu-id="cda27-171">抽象類別是具象類別必須實作的類別。</span><span class="sxs-lookup"><span data-stu-id="cda27-171">An abstract class is a class that must be implemented by a concrete class.</span></span> <span data-ttu-id="cda27-172">因為應用程式的控制站的抽象類別，您無法叫用直接定義在類別中的任何方法。</span><span class="sxs-lookup"><span data-stu-id="cda27-172">Because the Application controller is an abstract class, you cannot not invoke any methods defined in the class directly.</span></span> <span data-ttu-id="cda27-173">如果您嘗試直接叫用應用程式類別，您會取得 「 找不到資源 」 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cda27-173">If you attempt to invoke the Application class directly then you'll get a Resource Cannot Be Found error message.</span></span>

<span data-ttu-id="cda27-174">第三個，請注意，應用程式控制器包含將加入電影來檢視資料的類別清單中的建構函式。</span><span class="sxs-lookup"><span data-stu-id="cda27-174">Third, notice that the Application controller contains a constructor that adds the list of movie categories to view data.</span></span> <span data-ttu-id="cda27-175">繼承自應用程式的控制站的每個控制器類別會自動呼叫應用程式控制器的建構函式。</span><span class="sxs-lookup"><span data-stu-id="cda27-175">Every controller class that inherits from the Application controller calls the Application controller's constructor automatically.</span></span> <span data-ttu-id="cda27-176">每當您呼叫任何繼承自應用程式控制器的控制站上的任何動作時，電影類別會自動包含在檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-176">Whenever you call any action on any controller that inherits from the Application controller, the movie categories is included in the view data automatically.</span></span>

<span data-ttu-id="cda27-177">電影中的控制站列出 5 繼承自應用程式的控制站。</span><span class="sxs-lookup"><span data-stu-id="cda27-177">The Movies controller in Listing 5 inherits from the Application controller.</span></span>

<span data-ttu-id="cda27-178">**列出 5 –`Controllers\MoviesController.cs`**</span><span class="sxs-lookup"><span data-stu-id="cda27-178">**Listing 5 – `Controllers\MoviesController.cs`**</span></span>

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

<span data-ttu-id="cda27-179">電影控制器，就像在先前的章節中討論的主控制器會公開兩個動作方法，名為`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="cda27-179">The Movies controller, just like the Home controller discussed in the previous section, exposes two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="cda27-180">請注意，不是檢視主版頁面所顯示的電影類別目錄的清單加入至檢視中的資料`Index()`或`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="cda27-180">Notice that the list of movie categories displayed by the view master page is not added to view data in either the `Index()` or `Details()` method.</span></span> <span data-ttu-id="cda27-181">電影控制器繼承自應用程式的控制站，所以影片類別清單中的加入自動檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-181">Because the Movies controller inherits from the Application controller, the list of movie categories is added to view data automatically.</span></span>

<span data-ttu-id="cda27-182">請注意此解決方案，以新增檢視主版頁面的檢視資料沒有違反乾 （不重複自行） 原則。</span><span class="sxs-lookup"><span data-stu-id="cda27-182">Notice that this solution to adding view data for a view master page does not violate the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="cda27-183">程式碼以新增電影的分類，以檢視資料的清單包含只能在一個位置： 應用程式的控制站的建構函式。</span><span class="sxs-lookup"><span data-stu-id="cda27-183">The code for adding the list of movie categories to view data is contained in only one location: the constructor for the Application controller.</span></span>

### <a name="summary"></a><span data-ttu-id="cda27-184">總結</span><span class="sxs-lookup"><span data-stu-id="cda27-184">Summary</span></span>

<span data-ttu-id="cda27-185">在本教學課程中，我們將討論兩種方法來將從控制器的檢視資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="cda27-185">In this tutorial, we discussed two approaches to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="cda27-186">首先，我們會檢查簡單，但難以維護的方法。</span><span class="sxs-lookup"><span data-stu-id="cda27-186">First, we examined a simple, but difficult to maintain approach.</span></span> <span data-ttu-id="cda27-187">在第一個區段中，我們將討論如何新增檢視主版頁面的檢視資料中每個每個控制器動作在應用程式中。</span><span class="sxs-lookup"><span data-stu-id="cda27-187">In the first section, we discussed how you can add view data for a view master page in each every controller action in your application.</span></span> <span data-ttu-id="cda27-188">我們可以得到這是不正確的方法，因為它違反乾 （不重複自行） 原則。</span><span class="sxs-lookup"><span data-stu-id="cda27-188">We concluded that this was a bad approach because it violates the DRY (Don't Repeat Yourself) principle.</span></span>

<span data-ttu-id="cda27-189">接下來，我們會檢查較佳策略來加入資料檢視主版頁面才能檢視資料。</span><span class="sxs-lookup"><span data-stu-id="cda27-189">Next, we examined a much better strategy for adding data required by a view master page to view data.</span></span> <span data-ttu-id="cda27-190">而不是新增的檢視資料中每個控制器動作，我們會加入一次檢視資料中的應用程式的控制站。</span><span class="sxs-lookup"><span data-stu-id="cda27-190">Instead of adding the view data in each and every controller action, we added the view data only once within an Application controller.</span></span> <span data-ttu-id="cda27-191">這樣一來，將資料傳遞到 ASP.NET MVC 應用程式中檢視主版頁面時，可以避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cda27-191">That way, you can avoid duplicate code when passing data to a view master page in an ASP.NET MVC application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="cda27-192">[上一頁](creating-page-layouts-with-view-master-pages-cs.md)
[下一頁](asp-net-mvc-views-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cda27-192">[Previous](creating-page-layouts-with-view-master-pages-cs.md)
[Next](asp-net-mvc-views-overview-vb.md)</span></span>
