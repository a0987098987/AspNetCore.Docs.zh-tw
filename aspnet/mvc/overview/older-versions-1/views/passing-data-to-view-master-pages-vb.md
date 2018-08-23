---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: 將資料傳遞至檢視主版頁面 (VB) |Microsoft Docs
author: microsoft
description: 本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。 我們將探討兩種策略，將資料傳遞至檢視 m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b840e0a5cc325a043ae88c10f52cca418589119
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831208"
---
<a name="passing-data-to-view-master-pages-vb"></a><span data-ttu-id="73c75-104">將資料傳遞至檢視主版頁面 (VB)</span><span class="sxs-lookup"><span data-stu-id="73c75-104">Passing Data to View Master Pages (VB)</span></span>
====================
<span data-ttu-id="73c75-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="73c75-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="73c75-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="73c75-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> <span data-ttu-id="73c75-107">本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-107">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="73c75-108">我們將探討兩種策略，將資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-108">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="73c75-109">首先，我們會討論簡單的解決方案所產生的應用程式，很難維護。</span><span class="sxs-lookup"><span data-stu-id="73c75-109">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="73c75-110">接下來，我們會檢查較理想的方案需要稍微更多的初始工作，但更容易維護的應用程式中的結果。</span><span class="sxs-lookup"><span data-stu-id="73c75-110">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>


## <a name="passing-data-to-view-master-pages"></a><span data-ttu-id="73c75-111">將資料傳遞至檢視主版頁面</span><span class="sxs-lookup"><span data-stu-id="73c75-111">Passing Data to View Master Pages</span></span>

<span data-ttu-id="73c75-112">本教學課程的目標是要說明如何，您可以從控制器傳遞資料至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-112">The goal of this tutorial is to explain how you can pass data from a controller to a view master page.</span></span> <span data-ttu-id="73c75-113">我們將探討兩種策略，將資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-113">We examine two strategies for passing data to a view master page.</span></span> <span data-ttu-id="73c75-114">首先，我們會討論簡單的解決方案所產生的應用程式，很難維護。</span><span class="sxs-lookup"><span data-stu-id="73c75-114">First, we discuss an easy solution that results in an application that is difficult to maintain.</span></span> <span data-ttu-id="73c75-115">接下來，我們會檢查較理想的方案需要稍微更多的初始工作，但更容易維護的應用程式中的結果。</span><span class="sxs-lookup"><span data-stu-id="73c75-115">Next, we examine a much better solution that requires a little more initial work but results in a much more maintainable application.</span></span>

### <a name="the-problem"></a><span data-ttu-id="73c75-116">問題</span><span class="sxs-lookup"><span data-stu-id="73c75-116">The Problem</span></span>

<span data-ttu-id="73c75-117">假設您要建置影片資料庫應用程式，而您想要在您的應用程式中的每個頁面上顯示電影類別目錄的清單 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="73c75-117">Imagine that you are building a movie database application and you want to display the list of movie categories on every page in your application (see Figure 1).</span></span> <span data-ttu-id="73c75-118">此外，想像一下，電影類別目錄的清單會儲存在資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="73c75-118">Imagine, furthermore, that the list of movie categories is stored in a database table.</span></span> <span data-ttu-id="73c75-119">在此情況下，它將就從資料庫擷取類別，並呈現檢視主版頁面內的影片分類清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-119">In that case, it would make sense to retrieve the categories from the database and render the list of movie categories within a view master page.</span></span>


<span data-ttu-id="73c75-120">[![在 檢視主版頁面中顯示影片類別](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="73c75-120">[![Displaying movie categories in a view master page](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)</span></span>

<span data-ttu-id="73c75-121">**圖 01**： 顯示在檢視主版頁面的電影類別 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="73c75-121">**Figure 01**: Displaying movie categories in a view master page ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image3.png))</span></span>


<span data-ttu-id="73c75-122">以下是問題。</span><span class="sxs-lookup"><span data-stu-id="73c75-122">Here's the problem.</span></span> <span data-ttu-id="73c75-123">您要如何擷取主版頁面中的電影類別目錄的清單呢？</span><span class="sxs-lookup"><span data-stu-id="73c75-123">How do you retrieve the list of movie categories in the master page?</span></span> <span data-ttu-id="73c75-124">它會嘗試直接呼叫主版頁面中的模型類別的方法。</span><span class="sxs-lookup"><span data-stu-id="73c75-124">It is tempting to call methods of your model classes in the master page directly.</span></span> <span data-ttu-id="73c75-125">換句話說，相當吸引人，包括從資料庫權限，您的主版頁面中擷取資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="73c75-125">In other words, it is tempting to include the code for retrieving the data from the database right in your master page.</span></span> <span data-ttu-id="73c75-126">不過，略過您的 MVC 控制站，來存取資料庫，會違反清楚分離關注點，是建置在 MVC 應用程式的主要優點之一。</span><span class="sxs-lookup"><span data-stu-id="73c75-126">However, bypassing your MVC controllers to access the database would violate the clean separation of concerns that is one of the primary benefits of building an MVC application.</span></span>

<span data-ttu-id="73c75-127">在 MVC 應用程式，您會想 MVC 檢視與您的 MVC 模型，由 MVC 控制器之間的所有互動。</span><span class="sxs-lookup"><span data-stu-id="73c75-127">In an MVC application, you want all interaction between your MVC views and your MVC model to be handled by your MVC controllers.</span></span> <span data-ttu-id="73c75-128">此區隔問題會導致更容易維護、 可調適性的且可測試的應用程式。</span><span class="sxs-lookup"><span data-stu-id="73c75-128">This separation of concerns results in a more maintainable, adaptable, and testable application.</span></span>

<span data-ttu-id="73c75-129">MVC 應用程式中，傳遞至 – 包括檢視主版頁面 – 檢視的所有資料應都傳遞至檢視的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="73c75-129">In an MVC application, all data passed to a view – including a view master page – should be passed to a view by a controller action.</span></span> <span data-ttu-id="73c75-130">此外，資料應該傳遞利用檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-130">Furthermore, the data should be passed by taking advantage of view data.</span></span> <span data-ttu-id="73c75-131">在本教學課程的其餘部分，我會探討兩種方法可以將檢視資料傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-131">In the remainder of this tutorial, I examine two methods of passing view data to a view master page.</span></span>

### <a name="the-simple-solution"></a><span data-ttu-id="73c75-132">簡單的解決方案</span><span class="sxs-lookup"><span data-stu-id="73c75-132">The Simple Solution</span></span>

<span data-ttu-id="73c75-133">讓我們開始將檢視資料從控制器傳遞至檢視主版頁面最簡單的解決方案。</span><span class="sxs-lookup"><span data-stu-id="73c75-133">Let's start with the simplest solution to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="73c75-134">最簡單的解決方法是傳入每個控制器動作中的主版頁面的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-134">The simplest solution is to pass the view data for the master page in each and every controller action.</span></span>

<span data-ttu-id="73c75-135">考量列表 1 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="73c75-135">Consider the controller in Listing 1.</span></span> <span data-ttu-id="73c75-136">它會公開名為兩個動作`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="73c75-136">It exposes two actions named `Index()` and `Details()`.</span></span> <span data-ttu-id="73c75-137">`Index()`動作方法會傳回每個影片的電影資料庫資料表中。</span><span class="sxs-lookup"><span data-stu-id="73c75-137">The `Index()` action method returns every movie in the Movies database table.</span></span> <span data-ttu-id="73c75-138">`Details()`動作方法會傳回特定影片類別中的每個影片。</span><span class="sxs-lookup"><span data-stu-id="73c75-138">The `Details()` action method returns every movie in a particular movie category.</span></span>

<span data-ttu-id="73c75-139">**列表 1 – `Controllers\HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="73c75-139">**Listing 1 – `Controllers\HomeController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

<span data-ttu-id="73c75-140">請注意，同時`Index()`而`Details()`動作新增至檢視資料的兩個項目。</span><span class="sxs-lookup"><span data-stu-id="73c75-140">Notice that both the `Index()` and the `Details()` actions add two items to view data.</span></span> <span data-ttu-id="73c75-141">`Index()`動作會將兩個索引鍵： 類別目錄與電影。</span><span class="sxs-lookup"><span data-stu-id="73c75-141">The `Index()` action adds two keys: categories and movies.</span></span> <span data-ttu-id="73c75-142">類別索引鍵所代表電影檢視主版頁面所顯示的類別的清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-142">The categories key represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="73c75-143">影片索引鍵所代表索引檢視頁面所顯示的電影的清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-143">The movies key represents the list of movies displayed by the Index view page.</span></span>

<span data-ttu-id="73c75-144">`Details()`動作也會將名為分類和電影的兩個索引鍵。</span><span class="sxs-lookup"><span data-stu-id="73c75-144">The `Details()` action also adds two keys named categories and movies.</span></span> <span data-ttu-id="73c75-145">類別索引鍵，同樣地，表示檢視主版頁面所顯示的電影類別目錄的清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-145">The categories key, once again, represents the list of movie categories displayed by the view master page.</span></span> <span data-ttu-id="73c75-146">影片索引鍵所代表的特定類別，顯示 詳細資料檢視 頁面中的電影清單 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="73c75-146">The movies key represents the list of movies in a particular category displayed by the Details view page (see Figure 2).</span></span>


<span data-ttu-id="73c75-147">[![詳細資料檢視](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="73c75-147">[![The Details view](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)</span></span>

<span data-ttu-id="73c75-148">**圖 02**: 詳細資料檢視 ([按一下以檢視完整大小的影像](passing-data-to-view-master-pages-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="73c75-148">**Figure 02**: The Details view ([Click to view full-size image](passing-data-to-view-master-pages-vb/_static/image6.png))</span></span>


<span data-ttu-id="73c75-149">索引 檢視會包含在 列表 2。</span><span class="sxs-lookup"><span data-stu-id="73c75-149">The Index view is contained in Listing 2.</span></span> <span data-ttu-id="73c75-150">它只會重複執行的電影中的項目檢視資料所代表的電影清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-150">It simply iterates through the list of movies represented by the movies item in view data.</span></span>

<span data-ttu-id="73c75-151">**列表 2 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="73c75-151">**Listing 2 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

<span data-ttu-id="73c75-152">列表 3 中包含的主版頁面的檢視。</span><span class="sxs-lookup"><span data-stu-id="73c75-152">The view master page is contained in Listing 3.</span></span> <span data-ttu-id="73c75-153">檢視主版頁面會逐一查看，並呈現所有電影類別由類別目錄項目從 檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-153">The view master page iterates and renders all of the movie categories represented by the categories item from view data.</span></span>

<span data-ttu-id="73c75-154">**列表 3 – `Views\Shared\Site.master`**</span><span class="sxs-lookup"><span data-stu-id="73c75-154">**Listing 3 – `Views\Shared\Site.master`**</span></span>

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

<span data-ttu-id="73c75-155">若要檢視和檢視主版頁面傳遞所有的資料透過檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-155">All data is passed to the view and the view master page through view data.</span></span> <span data-ttu-id="73c75-156">這是正確的方式將資料傳遞至主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-156">That is the correct way to pass data to the master page.</span></span>

<span data-ttu-id="73c75-157">因此，什麼是此解決方案有什麼？</span><span class="sxs-lookup"><span data-stu-id="73c75-157">So, what's wrong with this solution?</span></span> <span data-ttu-id="73c75-158">問題在於，此解決方案，違反了 (Don't Repeat Yourself) DRY 準則。</span><span class="sxs-lookup"><span data-stu-id="73c75-158">The problem is that this solution violates the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="73c75-159">每個控制器動作必須加入相同的檢視資料的電影類別清單。</span><span class="sxs-lookup"><span data-stu-id="73c75-159">Each and every controller action must add the very same list of movie categories to view data.</span></span> <span data-ttu-id="73c75-160">在您的應用程式中有重複的程式碼，可讓您的應用程式更難維護、 調整及修改。</span><span class="sxs-lookup"><span data-stu-id="73c75-160">Having duplicate code in your application makes your application much more difficult to maintain, adapt, and modify.</span></span>

### <a name="the-good-solution"></a><span data-ttu-id="73c75-161">很好的解決方案</span><span class="sxs-lookup"><span data-stu-id="73c75-161">The Good Solution</span></span>

<span data-ttu-id="73c75-162">在本節中，我們會檢驗替代，並比較好，解決方案將資料從控制器動作傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-162">In this section, we examine an alternative, and better, solution to passing data from a controller action to a view master page.</span></span> <span data-ttu-id="73c75-163">而不是在每個控制器動作中加入主版頁面的電影類別，我們影片將類別新增至檢視資料一次。</span><span class="sxs-lookup"><span data-stu-id="73c75-163">Instead of adding the movie categories for the master page in each and every controller action, we add the movie categories to the view data only once.</span></span> <span data-ttu-id="73c75-164">為應用程式控制器就會加入所有使用檢視主版頁面的檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-164">All view data used by the view master page is added in an Application controller.</span></span>

<span data-ttu-id="73c75-165">ApplicationController 類別包含在 列表 4 中。</span><span class="sxs-lookup"><span data-stu-id="73c75-165">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="73c75-166">ApplicationController 類別包含在 列表 4 中。</span><span class="sxs-lookup"><span data-stu-id="73c75-166">The ApplicationController class is contained in Listing 4.</span></span>

<span data-ttu-id="73c75-167">**列表 4 – `Controllers\ApplicationController.vb`**</span><span class="sxs-lookup"><span data-stu-id="73c75-167">**Listing 4 – `Controllers\ApplicationController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

<span data-ttu-id="73c75-168">有三個您應該注意到應用程式中的控制站列表 4 相關的項目。</span><span class="sxs-lookup"><span data-stu-id="73c75-168">There are three things that you should notice about the Application controller in Listing 4.</span></span> <span data-ttu-id="73c75-169">首先，請注意，類別可以繼承自基底 system.web.mvc.controller 衍生類別。</span><span class="sxs-lookup"><span data-stu-id="73c75-169">First, notice that the class inherits from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="73c75-170">應用程式的控制器是控制器類別。</span><span class="sxs-lookup"><span data-stu-id="73c75-170">The Application controller is a controller class.</span></span>

<span data-ttu-id="73c75-171">其次，請注意，應用程式控制器類別 MustInherit 類別。</span><span class="sxs-lookup"><span data-stu-id="73c75-171">Second, notice that the Application controller class is a MustInherit class.</span></span> <span data-ttu-id="73c75-172">MustInherit 類別是由具象類別必須實作的類別。</span><span class="sxs-lookup"><span data-stu-id="73c75-172">An MustInherit class is a class that must be implemented by a concrete class.</span></span> <span data-ttu-id="73c75-173">因為應用程式的控制器是 MustInherit 類別，您無法叫用直接在類別中定義的任何方法。</span><span class="sxs-lookup"><span data-stu-id="73c75-173">Because the Application controller is an MustInherit class, you cannot not invoke any methods defined in the class directly.</span></span> <span data-ttu-id="73c75-174">如果您嘗試直接叫用應用程式類別，您會取得 「 找不到資源 」 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="73c75-174">If you attempt to invoke the Application class directly then you'll get a Resource Cannot Be Found error message.</span></span>

<span data-ttu-id="73c75-175">第三，要請您注意的是應用程式的控制器包含將加入電影檢視資料的類別清單中的建構函式。</span><span class="sxs-lookup"><span data-stu-id="73c75-175">Third, notice that the Application controller contains a constructor that adds the list of movie categories to view data.</span></span> <span data-ttu-id="73c75-176">繼承自應用程式的控制站的每個控制器類別會自動呼叫應用程式控制器的建構函式。</span><span class="sxs-lookup"><span data-stu-id="73c75-176">Every controller class that inherits from the Application controller calls the Application controller's constructor automatically.</span></span> <span data-ttu-id="73c75-177">每當您呼叫任何繼承自應用程式的控制器的控制站上的任何動作，電影類別是自動包含在檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-177">Whenever you call any action on any controller that inherits from the Application controller, the movie categories is included in the view data automatically.</span></span>

<span data-ttu-id="73c75-178">表 5 中的電影控制器繼承自應用程式的控制器。</span><span class="sxs-lookup"><span data-stu-id="73c75-178">The Movies controller in Listing 5 inherits from the Application controller.</span></span>

<span data-ttu-id="73c75-179">**列表 5 – `Controllers\MoviesController.vb`**</span><span class="sxs-lookup"><span data-stu-id="73c75-179">**Listing 5 – `Controllers\MoviesController.vb`**</span></span>

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

<span data-ttu-id="73c75-180">電影控制器，就像在先前的章節中討論的主控制器會公開兩個動作方法，名為`Index()`和`Details()`。</span><span class="sxs-lookup"><span data-stu-id="73c75-180">The Movies controller, just like the Home controller discussed in the previous section, exposes two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="73c75-181">請注意，不是檢視主版頁面所顯示的電影類別目錄的清單加入至檢視中的資料`Index()`或`Details()`方法。</span><span class="sxs-lookup"><span data-stu-id="73c75-181">Notice that the list of movie categories displayed by the view master page is not added to view data in either the `Index()` or `Details()` method.</span></span> <span data-ttu-id="73c75-182">電影控制器是繼承自應用程式的控制器，因為電影類別清單中的加入自動檢視資料。</span><span class="sxs-lookup"><span data-stu-id="73c75-182">Because the Movies controller inherits from the Application controller, the list of movie categories is added to view data automatically.</span></span>

<span data-ttu-id="73c75-183">請注意，此解決方案新增檢視主版頁面的檢視資料不違反 (Don't Repeat Yourself) DRY 準則。</span><span class="sxs-lookup"><span data-stu-id="73c75-183">Notice that this solution to adding view data for a view master page does not violate the DRY (Don't Repeat Yourself) principle.</span></span> <span data-ttu-id="73c75-184">程式碼以新增電影的類別，以檢視資料的清單包含只能在一個位置： 應用程式的控制器的建構函式。</span><span class="sxs-lookup"><span data-stu-id="73c75-184">The code for adding the list of movie categories to view data is contained in only one location: the constructor for the Application controller.</span></span>

### <a name="summary"></a><span data-ttu-id="73c75-185">總結</span><span class="sxs-lookup"><span data-stu-id="73c75-185">Summary</span></span>

<span data-ttu-id="73c75-186">在本教學課程中，我們會討論兩種方法可以將檢視資料從控制器傳遞至檢視主版頁面。</span><span class="sxs-lookup"><span data-stu-id="73c75-186">In this tutorial, we discussed two approaches to passing view data from a controller to a view master page.</span></span> <span data-ttu-id="73c75-187">首先，我們會檢查簡單，但難以維護的方法。</span><span class="sxs-lookup"><span data-stu-id="73c75-187">First, we examined a simple, but difficult to maintain approach.</span></span> <span data-ttu-id="73c75-188">在第一個區段中，我們會討論如何，您可以在每個每個控制器動作中新增檢視主版頁面的檢視資料在您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="73c75-188">In the first section, we discussed how you can add view data for a view master page in each every controller action in your application.</span></span> <span data-ttu-id="73c75-189">我們的結論這是不正確的方法，因為它違反了 (Don't Repeat Yourself) DRY 準則。</span><span class="sxs-lookup"><span data-stu-id="73c75-189">We concluded that this was a bad approach because it violates the DRY (Don't Repeat Yourself) principle.</span></span>

<span data-ttu-id="73c75-190">接下來，我們會檢查用來加入檢視主版頁面檢視資料所需的資料更好的策略。</span><span class="sxs-lookup"><span data-stu-id="73c75-190">Next, we examined a much better strategy for adding data required by a view master page to view data.</span></span> <span data-ttu-id="73c75-191">而不是在每個控制器動作中新增的檢視資料，我們新增了一次只能檢視資料內的應用程式的控制站。</span><span class="sxs-lookup"><span data-stu-id="73c75-191">Instead of adding the view data in each and every controller action, we added the view data only once within an Application controller.</span></span> <span data-ttu-id="73c75-192">如此一來，將資料傳遞至檢視主版頁面中的 ASP.NET MVC 應用程式時，可以避免重複的程式碼。</span><span class="sxs-lookup"><span data-stu-id="73c75-192">That way, you can avoid duplicate code when passing data to a view master page in an ASP.NET MVC application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="73c75-193">上一步</span><span class="sxs-lookup"><span data-stu-id="73c75-193">Previous</span></span>](creating-page-layouts-with-view-master-pages-vb.md)
