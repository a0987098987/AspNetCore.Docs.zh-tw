---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: 使用控制器和檢視來實作清單/詳細資料 UI |Microsoft Docs
author: microsoft
description: 步驟 4 說明如何將控制器新增至會充分利用我們的模型，為使用者提供的資料清單/詳細資料瀏覽體驗的應用程式...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 7a0a057efb52a869a72722b24d7283cb883db858
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838581"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a><span data-ttu-id="d5072-103">使用控制器和檢視來實作清單/詳細資料 UI</span><span class="sxs-lookup"><span data-stu-id="d5072-103">Use Controllers and Views to Implement a Listing/Details UI</span></span>
====================
<span data-ttu-id="d5072-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d5072-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d5072-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="d5072-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d5072-106">這是一套免費的步驟 4 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5072-106">This is step 4 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d5072-107">步驟 4 會示範如何將控制器新增至會充分利用我們的模型，為使用者提供的資料清單/詳細資料瀏覽體驗 dinners NerdDinner 網站上的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5072-107">Step 4 shows how to add a Controller to the application that takes advantage of our model to provide users with a data listing/details navigation experience for dinners on our NerdDinner site.</span></span>
> 
> <span data-ttu-id="d5072-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="d5072-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-4-controllers-and-views"></a><span data-ttu-id="d5072-109">NerdDinner 步驟 4： 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="d5072-109">NerdDinner Step 4: Controllers and Views</span></span>

<span data-ttu-id="d5072-110">有了傳統的 web 架構 （傳統 ASP、 PHP、 ASP.NET Web Form 等等），傳入的 Url 通常會對應至磁碟上的檔案。</span><span class="sxs-lookup"><span data-stu-id="d5072-110">With traditional web frameworks (classic ASP, PHP, ASP.NET Web Forms, etc), incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="d5072-111">例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會由 「 Products.aspx"或"Products.php 」 檔案的方式來處理。</span><span class="sxs-lookup"><span data-stu-id="d5072-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="d5072-112">Web 型 MVC 架構會稍有不同的方式，將 Url 對應至伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5072-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="d5072-113">而不是將傳入的 Url 對應至檔案中，它們改為將 Url 對應至類別的方法。</span><span class="sxs-lookup"><span data-stu-id="d5072-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="d5072-114">這些類別稱為 「 控制項 」，他們會負責處理傳入的 HTTP 要求，處理使用者輸入擷取和儲存資料，並判斷要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案、 重新導向至不同URL 等等）。</span><span class="sxs-lookup"><span data-stu-id="d5072-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc).</span></span>

<span data-ttu-id="d5072-115">既然我們已經建立基本模型 NerdDinner 應用程式，我們的下一個步驟是將控制器新增至會利用它為使用者提供的資料清單/詳細資料瀏覽體驗 Dinners 我們網站上的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5072-115">Now that we have built up a basic model for our NerdDinner application, our next step will be to add a Controller to the application that takes advantage of it to provide users with a data listing/details navigation experience for Dinners on our site.</span></span>

### <a name="adding-a-dinnerscontroller-controller"></a><span data-ttu-id="d5072-116">新增 DinnersController 控制器</span><span class="sxs-lookup"><span data-stu-id="d5072-116">Adding a DinnersController Controller</span></span>

<span data-ttu-id="d5072-117">我們將開始在我們的 web 專案中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選取**Add-&gt;控制器**功能表命令 （您也可以輸入 Ctrl-M、 Ctrl + C 來執行此命令）：</span><span class="sxs-lookup"><span data-stu-id="d5072-117">We'll begin by right-clicking on the "Controllers" folder within our web project, and then select the **Add-&gt;Controller** menu command (you can also execute this command by typing Ctrl-M, Ctrl-C):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

<span data-ttu-id="d5072-118">這會顯示 「 新增控制器 」 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="d5072-118">This will bring up the "Add Controller" dialog:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

<span data-ttu-id="d5072-119">我們將新的控制站 」 DinnersController"，然後按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d5072-119">We'll name the new controller "DinnersController" and click the "Add" button.</span></span> <span data-ttu-id="d5072-120">Visual Studio 將會再加入我們 \Controllers 目錄下的 DinnersController.cs 檔案：</span><span class="sxs-lookup"><span data-stu-id="d5072-120">Visual Studio will then add a DinnersController.cs file under our \Controllers directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

<span data-ttu-id="d5072-121">它也會開啟新 DinnersController 類別程式碼編輯器。</span><span class="sxs-lookup"><span data-stu-id="d5072-121">It will also open up the new DinnersController class within the code-editor.</span></span>

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a><span data-ttu-id="d5072-122">將 index （） 和 Details() 動作方法加入至 DinnersController 類別</span><span class="sxs-lookup"><span data-stu-id="d5072-122">Adding Index() and Details() Action Methods to the DinnersController Class</span></span>

<span data-ttu-id="d5072-123">我們想要讓訪客使用我們的應用程式來瀏覽即將推出的 dinners 的清單，並讓他們按一下清單中任何 Dinner，看到有關的特定詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d5072-123">We want to enable visitors using our application to browse a list of upcoming dinners, and allow them to click on any Dinner in the list to see specific details about it.</span></span> <span data-ttu-id="d5072-124">我們會發佈下列的 Url，從我們的應用程式來執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="d5072-124">We'll do this by publishing the following URLs from our application:</span></span>

| <span data-ttu-id="d5072-125">**URL**</span><span class="sxs-lookup"><span data-stu-id="d5072-125">**URL**</span></span> | <span data-ttu-id="d5072-126">**目的**</span><span class="sxs-lookup"><span data-stu-id="d5072-126">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="d5072-127">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="d5072-127">*/Dinners/*</span></span> | <span data-ttu-id="d5072-128">顯示即將推出的 dinners HTML 清單</span><span class="sxs-lookup"><span data-stu-id="d5072-128">Display an HTML list of upcoming dinners</span></span> |
| <span data-ttu-id="d5072-129">*/Dinners/Details/[id]*</span><span class="sxs-lookup"><span data-stu-id="d5072-129">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="d5072-130">顯示有關特定 dinner，內嵌於 URL-這會比對的資料庫中 dinner DinnerID 「 識別碼 」 參數所指示的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d5072-130">Display details about a specific dinner indicated by an "id" parameter embedded within the URL – which will match the DinnerID of the dinner in the database.</span></span> <span data-ttu-id="d5072-131">例如： /Dinners/Details/2 會顯示具有 Dinner DinnerID 值為 2 的詳細資料的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="d5072-131">For example: /Dinners/Details/2 would display an HTML page with details about the Dinner whose DinnerID value is 2.</span></span> |

<span data-ttu-id="d5072-132">我們將發佈這些 Url 的初始的實作加到 DinnersController 類別類似下面兩個公用 「 動作方法 」:</span><span class="sxs-lookup"><span data-stu-id="d5072-132">We will publish initial implementations of these URLs by adding two public "action methods" to our DinnersController class like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

<span data-ttu-id="d5072-133">然後，我們將執行 NerdDinner 應用程式，並使用瀏覽器叫用它們。</span><span class="sxs-lookup"><span data-stu-id="d5072-133">We'll then run the NerdDinner application and use our browser to invoke them.</span></span> <span data-ttu-id="d5072-134">在中輸入 *"Dinners / 」* URL 將會導致我們*index （)* 執行，以及它的方法會傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="d5072-134">Typing in the *"/Dinners/"* URL will cause our *Index()* method to run, and it will send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

<span data-ttu-id="d5072-135">在中輸入 *"/ Dinners/詳細資料/2 」* URL 將會導致我們*Details()* 方法來執行，並傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="d5072-135">Typing in the *"/Dinners/Details/2"* URL will cause our *Details()* method to run, and send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

<span data-ttu-id="d5072-136">您可能想知道-怎麼 ASP.NET MVC 知道建立我們 DinnersController 類別和叫用這些方法？</span><span class="sxs-lookup"><span data-stu-id="d5072-136">You might be wondering - how did ASP.NET MVC know to create our DinnersController class and invoke those methods?</span></span> <span data-ttu-id="d5072-137">若要了解，讓我們快速看一下路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="d5072-137">To understand that let's take a quick look at how routing works.</span></span>

### <a name="understanding-aspnet-mvc-routing"></a><span data-ttu-id="d5072-138">了解 ASP.NET MVC 路由</span><span class="sxs-lookup"><span data-stu-id="d5072-138">Understanding ASP.NET MVC Routing</span></span>

<span data-ttu-id="d5072-139">ASP.NET MVC 包含功能強大的 URL 路由引擎，提供很大的彈性，控制如何將 Url 對應至控制器類別。</span><span class="sxs-lookup"><span data-stu-id="d5072-139">ASP.NET MVC includes a powerful URL routing engine that provides a lot of flexibility in controlling how URLs are mapped to controller classes.</span></span> <span data-ttu-id="d5072-140">它可讓我們完全自訂 ASP.NET MVC 如何選擇要建立，要在其上叫用，以及設定不同的方式，變數可以是會自動從 URL/查詢字串剖析和傳遞給方法的參數的方法的控制器類別引數。</span><span class="sxs-lookup"><span data-stu-id="d5072-140">It allows us to completely customize how ASP.NET MVC chooses which controller class to create, which method to invoke on it, as well as configure different ways that variables can be automatically parsed from the URL/Querystring and passed to the method as parameter arguments.</span></span> <span data-ttu-id="d5072-141">它提供的彈性，完全最佳化 SEO （搜尋引擎最佳化） 的站台，以及發行任何我們想要從應用程式的 URL 結構。</span><span class="sxs-lookup"><span data-stu-id="d5072-141">It delivers the flexibility to totally optimize a site for SEO (search engine optimization) as well as publish any URL structure we want from an application.</span></span>

<span data-ttu-id="d5072-142">根據預設，新的 ASP.NET MVC 專案會隨附一組預先設定的已註冊的 URL 路由規則。</span><span class="sxs-lookup"><span data-stu-id="d5072-142">By default, new ASP.NET MVC projects come with a preconfigured set of URL routing rules already registered.</span></span> <span data-ttu-id="d5072-143">這可讓我們能夠輕鬆地開始使用應用程式而不需要明確設定任何項目。</span><span class="sxs-lookup"><span data-stu-id="d5072-143">This enables us to easily get started on an application without having to explicitly configure anything.</span></span> <span data-ttu-id="d5072-144">預設路由規則註冊可在我們的專案-我們可以按兩下以開啟 「 Global.asax 」 檔案的專案根目錄中的 「 應用程式 」 類別中：</span><span class="sxs-lookup"><span data-stu-id="d5072-144">The default routing rule registrations can be found within the "Application" class of our projects – which we can open by double-clicking the "Global.asax" file in the root of our project:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

<span data-ttu-id="d5072-145">這個類別之 「 RegisterRoutes"方法內註冊預設 ASP.NET MVC 路由規則：</span><span class="sxs-lookup"><span data-stu-id="d5072-145">The default ASP.NET MVC routing rules are registered within the "RegisterRoutes" method of this class:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

<span data-ttu-id="d5072-146">「 路由。MapRoute()"上述的方法呼叫註冊的預設路由規則將連入的 Url 對應至控制器類別使用的 URL 格式:"/ {controller} / {action} / {id}"，其中 「 控制器 」 是以具現化控制器類別的名稱"action"是的名稱公用方法來叫用，然後 「 識別碼 」 是可以做為引數傳遞給方法的 URL 中內嵌的選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="d5072-146">The "routes.MapRoute()" method call above registers a default routing rule that maps incoming URLs to controller classes using the URL format: "/{controller}/{action}/{id}" – where "controller" is the name of the controller class to instantiate, "action" is the name of a public method to invoke on it, and "id" is an optional parameter embedded within the URL that can be passed as an argument to the method.</span></span> <span data-ttu-id="d5072-147">傳遞給 「 MapRoute() 」 方法呼叫的第三個參數是一組要用於控制器/動作/識別碼值，它們不會出現在 URL 中的預設值 (控制器 ="Home"，動作 ="Index"，識別碼 ="")。</span><span class="sxs-lookup"><span data-stu-id="d5072-147">The third parameter passed to the "MapRoute()" method call is a set of default values to use for the controller/action/id values in the event that they are not present in the URL (Controller = "Home", Action="Index", Id="").</span></span>

<span data-ttu-id="d5072-148">以下是 使用預設對應的資料表，將示範如何在各種不同的 Url 」<em>/ {控制器} / {action} / {id}"</em>路由規則：</span><span class="sxs-lookup"><span data-stu-id="d5072-148">Below is a table that demonstrates how a variety of URLs are mapped using the default "<em>/{controllers}/{action}/{id}"</em>route rule:</span></span>

| <span data-ttu-id="d5072-149">**URL**</span><span class="sxs-lookup"><span data-stu-id="d5072-149">**URL**</span></span> | <span data-ttu-id="d5072-150">**控制器類別**</span><span class="sxs-lookup"><span data-stu-id="d5072-150">**Controller Class**</span></span> | <span data-ttu-id="d5072-151">**動作方法**</span><span class="sxs-lookup"><span data-stu-id="d5072-151">**Action Method**</span></span> | <span data-ttu-id="d5072-152">**傳遞參數**</span><span class="sxs-lookup"><span data-stu-id="d5072-152">**Parameters Passed**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d5072-153">*/Dinners/Details/2*</span><span class="sxs-lookup"><span data-stu-id="d5072-153">*/Dinners/Details/2*</span></span> | <span data-ttu-id="d5072-154">DinnersController</span><span class="sxs-lookup"><span data-stu-id="d5072-154">DinnersController</span></span> | <span data-ttu-id="d5072-155">Details(id)</span><span class="sxs-lookup"><span data-stu-id="d5072-155">Details(id)</span></span> | <span data-ttu-id="d5072-156">id=2</span><span class="sxs-lookup"><span data-stu-id="d5072-156">id=2</span></span> |
| <span data-ttu-id="d5072-157">*/Dinners/Edit/5*</span><span class="sxs-lookup"><span data-stu-id="d5072-157">*/Dinners/Edit/5*</span></span> | <span data-ttu-id="d5072-158">DinnersController</span><span class="sxs-lookup"><span data-stu-id="d5072-158">DinnersController</span></span> | <span data-ttu-id="d5072-159">Edit(id)</span><span class="sxs-lookup"><span data-stu-id="d5072-159">Edit(id)</span></span> | <span data-ttu-id="d5072-160">id=5</span><span class="sxs-lookup"><span data-stu-id="d5072-160">id=5</span></span> |
| <span data-ttu-id="d5072-161">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="d5072-161">*/Dinners/Create*</span></span> | <span data-ttu-id="d5072-162">DinnersController</span><span class="sxs-lookup"><span data-stu-id="d5072-162">DinnersController</span></span> | <span data-ttu-id="d5072-163">Create （)</span><span class="sxs-lookup"><span data-stu-id="d5072-163">Create()</span></span> | <span data-ttu-id="d5072-164">N/A</span><span class="sxs-lookup"><span data-stu-id="d5072-164">N/A</span></span> |
| <span data-ttu-id="d5072-165">*/ Dinners*</span><span class="sxs-lookup"><span data-stu-id="d5072-165">*/Dinners*</span></span> | <span data-ttu-id="d5072-166">DinnersController</span><span class="sxs-lookup"><span data-stu-id="d5072-166">DinnersController</span></span> | <span data-ttu-id="d5072-167">Index （)</span><span class="sxs-lookup"><span data-stu-id="d5072-167">Index()</span></span> | <span data-ttu-id="d5072-168">N/A</span><span class="sxs-lookup"><span data-stu-id="d5072-168">N/A</span></span> |
| <span data-ttu-id="d5072-169">*/ 首頁*</span><span class="sxs-lookup"><span data-stu-id="d5072-169">*/Home*</span></span> | <span data-ttu-id="d5072-170">HomeController</span><span class="sxs-lookup"><span data-stu-id="d5072-170">HomeController</span></span> | <span data-ttu-id="d5072-171">Index （)</span><span class="sxs-lookup"><span data-stu-id="d5072-171">Index()</span></span> | <span data-ttu-id="d5072-172">N/A</span><span class="sxs-lookup"><span data-stu-id="d5072-172">N/A</span></span> |
| */* | <span data-ttu-id="d5072-173">HomeController</span><span class="sxs-lookup"><span data-stu-id="d5072-173">HomeController</span></span> | <span data-ttu-id="d5072-174">Index （)</span><span class="sxs-lookup"><span data-stu-id="d5072-174">Index()</span></span> | <span data-ttu-id="d5072-175">N/A</span><span class="sxs-lookup"><span data-stu-id="d5072-175">N/A</span></span> |

<span data-ttu-id="d5072-176">最後三個資料列會顯示預設值 (控制器 = 首頁，動作 = 索引，Id ="") 所使用。</span><span class="sxs-lookup"><span data-stu-id="d5072-176">The last three rows show the default values (Controller = Home, Action = Index, Id = "") being used.</span></span> <span data-ttu-id="d5072-177">因為如果未指定，"Index"方法註冊為預設的動作名稱"/ Dinners"和"/home"Url 原因，index （） 動作方法會叫用其控制器類別。</span><span class="sxs-lookup"><span data-stu-id="d5072-177">Because the "Index" method is registered as the default action name if one isn't specified, the "/Dinners" and "/Home" URLs cause the Index() action method to be invoked on their Controller classes.</span></span> <span data-ttu-id="d5072-178">因為"Home"控制器註冊為預設控制器中，如果未指定，"/"的 URL，則會導致要建立、 HomeController 和 index （） 動作方法以叫用。</span><span class="sxs-lookup"><span data-stu-id="d5072-178">Because the "Home" controller is registered as the default controller if one isn't specified, the "/" URL causes the HomeController to be created, and the Index() action method on it to be invoked.</span></span>

<span data-ttu-id="d5072-179">如果您不喜歡這些預設的 URL 路由規則，好消息是，可輕鬆地變更-只要將它們設為編輯上述的 RegisterRoutes 方法內。</span><span class="sxs-lookup"><span data-stu-id="d5072-179">If you don't like these default URL routing rules, the good news is that they are easy to change - just edit them within the RegisterRoutes method above.</span></span> <span data-ttu-id="d5072-180">NerdDinner 應用程式中，不過，我們不打算變更任何預設的 URL 路由規則 – 而我們只使用它們作為-是。</span><span class="sxs-lookup"><span data-stu-id="d5072-180">For our NerdDinner application, though, we aren't going to change any of the default URL routing rules – instead we'll just use them as-is.</span></span>

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a><span data-ttu-id="d5072-181">使用從我們 DinnersController DinnerRepository</span><span class="sxs-lookup"><span data-stu-id="d5072-181">Using the DinnerRepository from our DinnersController</span></span>

<span data-ttu-id="d5072-182">我們現在將我們的 DinnersController 目前的實作與使用我們的模型實作的 index （） 和 Details() 動作方法。</span><span class="sxs-lookup"><span data-stu-id="d5072-182">Let's now replace our current implementation of the DinnersController's Index() and Details() action methods with implementations that use our model.</span></span>

<span data-ttu-id="d5072-183">我們將使用我們稍早建置來實作行為的 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="d5072-183">We'll use the DinnerRepository class we built earlier to implement the behavior.</span></span> <span data-ttu-id="d5072-184">我們會藉由新增參照"NerdDinner.Models"命名空間中，"using"陳述式開始，然後將我們 DinnerRepository 的執行個體宣告做為欄位中，我們 DinnerController 類別上變更。</span><span class="sxs-lookup"><span data-stu-id="d5072-184">We'll begin by adding a "using" statement that references the "NerdDinner.Models" namespace, and then declare an instance of our DinnerRepository as a field on our DinnerController class.</span></span>

<span data-ttu-id="d5072-185">在本章稍後我們將介紹 「 相依性插入 」 的概念，並說明為了取得啟用更好的單元 DinnerRepository 參考控制器的另一種方式，測試 – 但權限現在我們將只建立我們 DinnerRepository 的執行個體下面類似內嵌。</span><span class="sxs-lookup"><span data-stu-id="d5072-185">Later in this chapter we'll introduce the concept of "Dependency Injection" and show another way for our Controllers to obtain a reference to a DinnerRepository that enables better unit testing – but for right now we'll just create an instance of our DinnerRepository inline like below.</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

<span data-ttu-id="d5072-186">現在我們已經準備好產生 HTML 回應使用擷取的資料模型物件。</span><span class="sxs-lookup"><span data-stu-id="d5072-186">Now we are ready to generate a HTML response back using our retrieved data model objects.</span></span>

### <a name="using-views-with-our-controller"></a><span data-ttu-id="d5072-187">使用我們的控制器的檢視</span><span class="sxs-lookup"><span data-stu-id="d5072-187">Using Views with our Controller</span></span>

<span data-ttu-id="d5072-188">雖然您可以撰寫程式碼來組合 HTML，然後使用我們的動作方法內*response.write （)* helper 方法，以將它傳送回用戶端，該方法會變得相當難以快速。</span><span class="sxs-lookup"><span data-stu-id="d5072-188">While it is possible to write code within our action methods to assemble HTML and then use the *Response.Write()* helper method to send it back to the client, that approach becomes fairly unwieldy quickly.</span></span> <span data-ttu-id="d5072-189">更好的方法是我們只能執行應用程式和我們的 DinnersController 動作方法內的資料邏輯，並將已轉譯 HTML 回應給負責輸出 HTML 表示個別的 「 檢視 」 範本所需的資料它。</span><span class="sxs-lookup"><span data-stu-id="d5072-189">A much better approach is for us to only perform application and data logic inside our DinnersController action methods, and to then pass the data needed to render a HTML response to a separate "view" template that is responsible for outputting the HTML representation of it.</span></span> <span data-ttu-id="d5072-190">如我們稍後所見，「 檢視 」 的範本是文字檔案，其中通常包含 HTML 標記和內嵌的轉譯程式碼的組合。</span><span class="sxs-lookup"><span data-stu-id="d5072-190">As we'll see in a moment, a "view" template is a text file that typically contains a combination of HTML markup and embedded rendering code.</span></span>

<span data-ttu-id="d5072-191">我們的控制器邏輯區分開來我們檢視轉譯提供數個優點。</span><span class="sxs-lookup"><span data-stu-id="d5072-191">Separating our controller logic from our view rendering brings several big benefits.</span></span> <span data-ttu-id="d5072-192">特別是它可協助應用程式程式碼與 UI 呈現格式化程式碼強制清除 「 關注點分離 」。</span><span class="sxs-lookup"><span data-stu-id="d5072-192">In particular it helps enforce a clear "separation of concerns" between the application code and UI formatting/rendering code.</span></span> <span data-ttu-id="d5072-193">這樣更容易在隔離的單元測試應用程式邏輯與 UI 呈現邏輯。</span><span class="sxs-lookup"><span data-stu-id="d5072-193">This makes it much easier to unit-test application logic in isolation from UI rendering logic.</span></span> <span data-ttu-id="d5072-194">它可讓您稍後修改 UI 轉譯範本，而不必變更應用程式程式碼的工作變得更容易。</span><span class="sxs-lookup"><span data-stu-id="d5072-194">It makes it easier to later modify the UI rendering templates without having to make application code changes.</span></span> <span data-ttu-id="d5072-195">它可以讓它更容易開發人員和設計人員共同合作專案。</span><span class="sxs-lookup"><span data-stu-id="d5072-195">And it can make it easier for developers and designers to collaborate together on projects.</span></span>

<span data-ttu-id="d5072-196">我們可以更新我們 DinnersController 類別，以指出我們想要藉由變更資訊，請參閱我們的兩個動作方法的方法簽章，不需要傳回型別"void"改為具有傳回型別"ActionResult 」 傳回的 HTML UI 回應中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d5072-196">We can update our DinnersController class to indicate that we want to use a view template to send back an HTML UI response by changing the method signatures of our two action methods from having a return type of "void" to instead have a return type of "ActionResult".</span></span> <span data-ttu-id="d5072-197">我們接著可以呼叫*View()* helper 方法，在控制器的基底類別傳回"ViewResult 」 物件如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5072-197">We can then call the *View()* helper method on the Controller base class to return back a "ViewResult" object like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

<span data-ttu-id="d5072-198">簽章*View()* 我們在上面使用的 helper 方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5072-198">The signature of the *View()* helper method we are using above looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

<span data-ttu-id="d5072-199">第一個參數*View()* helper 方法，是我們想要用來呈現 HTML 回應檢視範本檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="d5072-199">The first parameter to the *View()* helper method is the name of the view template file we want to use to render the HTML response.</span></span> <span data-ttu-id="d5072-200">第二個參數是模型物件，其中包含所檢視範本呈現 HTML 回應所需要的資料。</span><span class="sxs-lookup"><span data-stu-id="d5072-200">The second parameter is a model object that contains the data that the view template needs in order to render the HTML response.</span></span>

<span data-ttu-id="d5072-201">我們在我們的 index （） 動作方法內呼叫*View()* helper 方法，並指出我們想要呈現 dinners 使用 「 索引 」 檢視範本的 HTML 清單。</span><span class="sxs-lookup"><span data-stu-id="d5072-201">Within our Index() action method we are calling the *View()* helper method and indicating that we want to render an HTML listing of dinners using an "Index" view template.</span></span> <span data-ttu-id="d5072-202">我們要將檢視範本傳遞一連串 Dinner 物件產生的清單：</span><span class="sxs-lookup"><span data-stu-id="d5072-202">We are passing the view template a sequence of Dinner objects to generate the list from:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

<span data-ttu-id="d5072-203">在我們 Details() 動作方法內我們會嘗試擷取 Dinner 物件，使用提供的 URL 中的識別碼。</span><span class="sxs-lookup"><span data-stu-id="d5072-203">Within our Details() action method we attempt to retrieve a Dinner object using the id provided within the URL.</span></span> <span data-ttu-id="d5072-204">如果我們找到有效的 Dinner *View()* 協助程式方法，並指出我們想要使用 [詳細資料] 檢視範本來呈現擷取的 Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="d5072-204">If a valid Dinner is found we call the *View()* helper method, indicating we want to use a "Details" view template to render the retrieved Dinner object.</span></span> <span data-ttu-id="d5072-205">如果要求不正確的 dinner，我們會呈現表示 Dinner 不存在使用"NotFound"檢視範本的有用的錯誤訊息 (和多載的新版*View()* helper 方法，只會使用範本名稱):</span><span class="sxs-lookup"><span data-stu-id="d5072-205">If an invalid dinner is requested, we render a helpful error message that indicates that the Dinner doesn't exist using a "NotFound" view template (and an overloaded version of the *View()* helper method that just takes the template name):</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

<span data-ttu-id="d5072-206">讓我們現在實作"NotFound"、 [詳細資料] 和 [索引] 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d5072-206">Let's now implement the "NotFound", "Details", and "Index" view templates.</span></span>

### <a name="implementing-the-notfound-view-template"></a><span data-ttu-id="d5072-207">實作"NotFound"檢視範本</span><span class="sxs-lookup"><span data-stu-id="d5072-207">Implementing the "NotFound" View Template</span></span>

<span data-ttu-id="d5072-208">我們開始要先實作"NotFound"檢視範本 – 顯示易懂的錯誤訊息，指出找不到要求的 dinner。</span><span class="sxs-lookup"><span data-stu-id="d5072-208">We'll begin by implementing the "NotFound" view template – which displays a friendly error message indicating that the requested dinner can't be found.</span></span>

<span data-ttu-id="d5072-209">我們將定位文字資料指標內控制器動作方法，來建立新的檢視範本，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （我們也可以輸入 Ctrl-M、 Ctrl-V 來執行此命令）：</span><span class="sxs-lookup"><span data-stu-id="d5072-209">We'll create a new view template by positioning our text cursor within a controller action method, and then right click and choose the "Add View" menu command (we can also execute this command by typing Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

<span data-ttu-id="d5072-210">這會顯示類似下方的 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d5072-210">This will bring up an "Add View" dialog like below.</span></span> <span data-ttu-id="d5072-211">根據預設，對話方塊會預先填入的檢視，以建立名稱來比對動作方法的名稱資料指標時所在的對話方塊已啟動 （在此情況下 [詳細資料]）。</span><span class="sxs-lookup"><span data-stu-id="d5072-211">By default the dialog will pre-populate the name of the view to create to match the name of the action method the cursor was in when the dialog was launched (in this case "Details").</span></span> <span data-ttu-id="d5072-212">因為我們想要先實作"NotFound"範本，我們將會覆寫此檢視表名稱，然後將它改為"NotFound":</span><span class="sxs-lookup"><span data-stu-id="d5072-212">Because we want to first implement the "NotFound" template, we'll override this view name and set it to instead be "NotFound":</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

<span data-ttu-id="d5072-213">當我們按一下 [新增] 按鈕時，Visual Studio 會內的"\Views\Dinners"目錄 （如果目錄不存在時，它也會建立） 就讓我們建立新的 「 NotFound.aspx 」 檢視範本：</span><span class="sxs-lookup"><span data-stu-id="d5072-213">When we click the "Add" button, Visual Studio will create a new "NotFound.aspx" view template for us within the "\Views\Dinners" directory (which it will also create if the directory doesn't already exist):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

<span data-ttu-id="d5072-214">它也會開啟新 「 NotFound.aspx 」 檢視範本程式碼編輯器：</span><span class="sxs-lookup"><span data-stu-id="d5072-214">It will also open up our new "NotFound.aspx" view template within the code-editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

<span data-ttu-id="d5072-215">預設的檢視範本有兩個 「 內容地區 」，我們可以在其中新增內容及程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5072-215">View templates by default have two "content regions" where we can add content and code.</span></span> <span data-ttu-id="d5072-216">第一個可讓我們用來自訂"title"的 HTML 網頁傳回。</span><span class="sxs-lookup"><span data-stu-id="d5072-216">The first allows us to customize the "title" of the HTML page sent back.</span></span> <span data-ttu-id="d5072-217">第二個可讓我們用來自訂 「 主要內容 」 的 HTML 網頁傳回。</span><span class="sxs-lookup"><span data-stu-id="d5072-217">The second allows us to customize the "main content" of the HTML page sent back.</span></span>

<span data-ttu-id="d5072-218">若要實作"NotFound"檢視範本中，我們將新增一些基本的內容：</span><span class="sxs-lookup"><span data-stu-id="d5072-218">To implement our "NotFound" view template we'll add some basic content:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

<span data-ttu-id="d5072-219">我們可以再試試看在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d5072-219">We can then try it out within the browser.</span></span> <span data-ttu-id="d5072-220">若要這樣做讓我們來要求 *"/ Dinners/詳細資料/9999"* URL。</span><span class="sxs-lookup"><span data-stu-id="d5072-220">To do this let's request the *"/Dinners/Details/9999"* URL.</span></span> <span data-ttu-id="d5072-221">這會參考目前不存在於資料庫中，並會導致我們 DinnersController.Details() 動作方法，以呈現"NotFound"檢視範本 dinner:</span><span class="sxs-lookup"><span data-stu-id="d5072-221">This will refer to a dinner that doesn't currently exist in the database, and will cause our DinnersController.Details() action method to render our "NotFound" view template:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

<span data-ttu-id="d5072-222">您會注意到在上述螢幕擷取畫面中的一件事是 HTML 的基本的檢視範本繼承了一大堆圍繞在螢幕上的主要內容。</span><span class="sxs-lookup"><span data-stu-id="d5072-222">One thing you'll notice in the screen-shot above is that our basic view template has inherited a bunch of HTML that surrounds the main content on the screen.</span></span> <span data-ttu-id="d5072-223">這是因為我們檢視範本會使用 「 主版頁面 」 範本，可讓我們在網站上的所有檢視之間套用一致的版面配置。</span><span class="sxs-lookup"><span data-stu-id="d5072-223">This is because our view-template is using a "master page" template that enables us to apply a consistent layout across all views on the site.</span></span> <span data-ttu-id="d5072-224">我們將討論主版頁面詳細說明本教學課程的後半部分工作的方式。</span><span class="sxs-lookup"><span data-stu-id="d5072-224">We'll discuss how master pages work more in a later part of this tutorial.</span></span>

### <a name="implementing-the-details-view-template"></a><span data-ttu-id="d5072-225">實作 [詳細資料] 檢視範本</span><span class="sxs-lookup"><span data-stu-id="d5072-225">Implementing the "Details" View Template</span></span>

<span data-ttu-id="d5072-226">讓我們現在實作 [詳細資料] 檢視範本-將單一 Dinner 模型產生 HTML。</span><span class="sxs-lookup"><span data-stu-id="d5072-226">Let's now implement the "Details" view template – which will generate HTML for a single Dinner model.</span></span>

<span data-ttu-id="d5072-227">我們將可以定位文字資料指標在詳細資料動作方法中，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）：</span><span class="sxs-lookup"><span data-stu-id="d5072-227">We'll do this by positioning our text cursor within the Details action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

<span data-ttu-id="d5072-228">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d5072-228">This will bring up the "Add View" dialog.</span></span> <span data-ttu-id="d5072-229">我們將會保留預設的檢視名稱 （[詳細資料]）。</span><span class="sxs-lookup"><span data-stu-id="d5072-229">We'll keep the default view name ("Details").</span></span> <span data-ttu-id="d5072-230">我們也會在對話方塊中選取 [建立強型別檢視表] 核取方塊，然後選取 （使用下拉式方塊的下拉式清單中） 從控制器傳遞至檢視的模型型別名稱。</span><span class="sxs-lookup"><span data-stu-id="d5072-230">We'll also select the "Create a strongly-typed View" checkbox in the dialog and select (using the combobox dropdown) the name of the model type we are passing from the Controller to the View.</span></span> <span data-ttu-id="d5072-231">此檢視中，我們會傳遞 Dinner 物件 (此類型的完整的名稱是: 「 NerdDinner.Models.Dinner"):</span><span class="sxs-lookup"><span data-stu-id="d5072-231">For this view we are passing a Dinner object (the fully qualified name for this type is: "NerdDinner.Models.Dinner"):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

<span data-ttu-id="d5072-232">不同於先前的範本，在我們選擇要建立 「 空的檢視 」，這次我們會自動選擇"scaffold 」 檢視使用 [詳細資料] 範本。</span><span class="sxs-lookup"><span data-stu-id="d5072-232">Unlike the previous template, where we chose to create an "Empty View", this time we will choose to automatically "scaffold" the view using a "Details" template.</span></span> <span data-ttu-id="d5072-233">我們可以變更 [檢視內容] 下拉式清單中上述對話方塊來表示。</span><span class="sxs-lookup"><span data-stu-id="d5072-233">We can indicate this by changing the "View content" drop-down in the dialog above.</span></span>

<span data-ttu-id="d5072-234">「 Scaffolding"會產生我們根據 Dinner 物件傳遞給它的詳細資料檢視範本的初始實作。</span><span class="sxs-lookup"><span data-stu-id="d5072-234">"Scaffolding" will generate an initial implementation of our details view template based on the Dinner object we are passing to it.</span></span> <span data-ttu-id="d5072-235">這提供簡單的方法，讓我們快速地開始使用我們的檢視範本實作。</span><span class="sxs-lookup"><span data-stu-id="d5072-235">This provides an easy way for us to quickly get started on our view template implementation.</span></span>

<span data-ttu-id="d5072-236">當我們按一下 [新增] 按鈕時，Visual Studio 會在我們的 「 \Views\Dinners"目錄內就讓我們建立新的 「 Details.aspx 」 檢視範本檔案：</span><span class="sxs-lookup"><span data-stu-id="d5072-236">When we click the "Add" button, Visual Studio will create a new "Details.aspx" view template file for us within our "\Views\Dinners" directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

<span data-ttu-id="d5072-237">它也會開啟新 「 Details.aspx 」 檢視範本程式碼編輯器。</span><span class="sxs-lookup"><span data-stu-id="d5072-237">It will also open up our new "Details.aspx" view template within the code-editor.</span></span> <span data-ttu-id="d5072-238">它會包含 Dinner 模型為基礎的詳細資料檢視的初始 scaffold 實作。</span><span class="sxs-lookup"><span data-stu-id="d5072-238">It will contain an initial scaffold implementation of a details view based on a Dinner model.</span></span> <span data-ttu-id="d5072-239">Scaffolding 引擎會使用.NET 反映來查看傳遞給它，在類別上公開的公用屬性，並將適當的內容，根據每個找到的類型：</span><span class="sxs-lookup"><span data-stu-id="d5072-239">The scaffolding engine uses .NET reflection to look at the public properties exposed on the class passed it, and will add appropriate content based on each type it finds:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

<span data-ttu-id="d5072-240">我們可以要求 *"/ Dinners/詳細資料/1"* URL，以查看此"details"scaffold 實作在瀏覽器的外觀。</span><span class="sxs-lookup"><span data-stu-id="d5072-240">We can request the *"/Dinners/Details/1"* URL to see what this "details" scaffold implementation looks like in the browser.</span></span> <span data-ttu-id="d5072-241">使用此 URL 會顯示我們以手動方式加入至資料庫時我們第一次建立 dinners 的其中一個：</span><span class="sxs-lookup"><span data-stu-id="d5072-241">Using this URL will display one of the dinners we manually added to our database when we first created it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

<span data-ttu-id="d5072-242">這會啟動並執行快速，讓我們，並提供我們 Details.aspx 檢視的初始實作。</span><span class="sxs-lookup"><span data-stu-id="d5072-242">This gets us up and running quickly, and provides us with an initial implementation of our Details.aspx view.</span></span> <span data-ttu-id="d5072-243">然後，我們可以移，並調整它以自訂 UI 以我們的滿意度。</span><span class="sxs-lookup"><span data-stu-id="d5072-243">We can then go and tweak it to customize the UI to our satisfaction.</span></span>

<span data-ttu-id="d5072-244">當我們更仔細看看 Details.aspx 範本時，我們會發現，它包含靜態 HTML，以及內嵌轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5072-244">When we look at the Details.aspx template more closely, we'll find that it contains static HTML as well as embedded rendering code.</span></span> <span data-ttu-id="d5072-245">&lt;%%&gt;程式碼區塊執行的程式碼，當檢視範本呈現，並&lt;%= %&gt;程式碼區塊執行其中所包含的程式碼，並再呈現範本的輸出資料流的結果。</span><span class="sxs-lookup"><span data-stu-id="d5072-245">&lt;% %&gt; code nuggets execute code when the view template renders, and &lt;%= %&gt; code nuggets execute the code contained within them and then render the result to the output stream of the template.</span></span>

<span data-ttu-id="d5072-246">我們可以撰寫程式碼中我們存取傳遞從控制器使用強型別 「 模型 」 屬性 「 Dinner"模型物件的檢視。</span><span class="sxs-lookup"><span data-stu-id="d5072-246">We can write code within our View that accesses the "Dinner" model object that was passed from our controller using a strongly-typed "Model" property.</span></span> <span data-ttu-id="d5072-247">Visual Studio 提供我們具有完整的 intellisense 程式碼時存取這個編輯器中的 「 模型 」 屬性：</span><span class="sxs-lookup"><span data-stu-id="d5072-247">Visual Studio provides us with full code-intellisense when accessing this "Model" property within the editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

<span data-ttu-id="d5072-248">讓我們請少許調整，讓我們的最終詳細資料檢視範本的來源如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5072-248">Let's make some tweaks so that the source for our final Details view template looks like below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

<span data-ttu-id="d5072-249">當我們存取 *"/ Dinners/詳細資料/1"* URL 一次它將會立即呈現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5072-249">When we access the *"/Dinners/Details/1"* URL again it will now render like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a><span data-ttu-id="d5072-250">實作 「 索引 」 檢視範本</span><span class="sxs-lookup"><span data-stu-id="d5072-250">Implementing the "Index" View Template</span></span>

<span data-ttu-id="d5072-251">讓我們現在實作 「 索引 」 檢視範本 – 將會產生即將推出的 Dinners 的清單。</span><span class="sxs-lookup"><span data-stu-id="d5072-251">Let's now implement the "Index" view template – which will generate a listing of upcoming Dinners.</span></span> <span data-ttu-id="d5072-252">待辦事項這我們會在索引動作方法中，我們文字游標的位置，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）。</span><span class="sxs-lookup"><span data-stu-id="d5072-252">To-do this we'll position our text cursor within the Index action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V).</span></span>

<span data-ttu-id="d5072-253">在 [新增檢視] 對話方塊中，我們將保留名為"Index"的檢視範本，並選取 [建立強型別檢視] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d5072-253">Within the "Add View" dialog we'll keep the view template named "Index" and select the "Create a strongly-typed view" checkbox.</span></span> <span data-ttu-id="d5072-254">此時，我們將選擇自動產生 「 清單 」 檢視範本，並選取 「 NerdDinner.Models.Dinner"做為模型類型傳遞至檢視 (這因為我們已指出我們要建立 「 清單 」 scaffold 會假設我們的 [新增檢視] 對話方塊傳遞一連串 Dinner 物件從控制器到檢視）：</span><span class="sxs-lookup"><span data-stu-id="d5072-254">This time we will choose to automatically generate a "List" view template, and select "NerdDinner.Models.Dinner" as the model type passed to the view (which because we have indicated we are creating a "List" scaffold will cause the Add View dialog to assume we are passing a sequence of Dinner objects from our Controller to the View):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

<span data-ttu-id="d5072-255">當我們按一下 [新增] 按鈕時，Visual Studio 會在我們的 「 \Views\Dinners"目錄內就讓我們建立新的 「 Index.aspx 」 檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="d5072-255">When we click the "Add" button, Visual Studio will create a new "Index.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="d5072-256">它會 「 scaffold 」 內的初始實作提供 Dinners 我們傳遞至檢視的 HTML 資料表清單。</span><span class="sxs-lookup"><span data-stu-id="d5072-256">It will "scaffold" an initial implementation within it that provides an HTML table listing of the Dinners we pass to the view.</span></span>

<span data-ttu-id="d5072-257">當我們執行的應用程式和存取權 *"Dinners /"* 轉譯的 dinners 名單的 URL，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="d5072-257">When we run the application and access the *"/Dinners/"* URL it will render our list of dinners like so:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

<span data-ttu-id="d5072-258">上述表格解決方案讓我們吃晚餐資料 – 這並不是很希望我們為直接面對消費者 Dinner 清單的類似方格的配置。</span><span class="sxs-lookup"><span data-stu-id="d5072-258">The table solution above gives us a grid-like layout of our Dinner data – which isn't quite what we want for our consumer facing Dinner listing.</span></span> <span data-ttu-id="d5072-259">我們可以更新 Index.aspx 檢視範本，並修改它以列出較少的資料行的資料，並用&lt;ul&gt;来呈現它們而不是資料表，使用下列程式碼項目：</span><span class="sxs-lookup"><span data-stu-id="d5072-259">We can update the Index.aspx view template and modify it to list fewer columns of data, and use a &lt;ul&gt; element to render them instead of a table using the code below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

<span data-ttu-id="d5072-260">因為我們使用迴圈處理每個 dinner 在我們的模型，我們會使用上述的 foreach 陳述式內的 「 var 」 關鍵字。</span><span class="sxs-lookup"><span data-stu-id="d5072-260">We are using the "var" keyword within the above foreach statement as we loop over each dinner in our Model.</span></span> <span data-ttu-id="d5072-261">熟悉 C# 3.0 的那些可能會認為，使用 「 var 」 表示 dinner 物件是晚期繫結。</span><span class="sxs-lookup"><span data-stu-id="d5072-261">Those unfamiliar with C# 3.0 might think that using "var" means that the dinner object is late-bound.</span></span> <span data-ttu-id="d5072-262">它表示編譯器使用型別推斷對強型別 「 模型 」 的屬性 (類型"IEnumerable&lt;Dinner&gt;") 和編譯為 Dinner 類型 – 這表示我們取得完整的本機 「 dinner"變數intellisense 和編譯時期檢查程式碼區塊內：</span><span class="sxs-lookup"><span data-stu-id="d5072-262">It instead means that the compiler is using type-inference against the strongly typed "Model" property (which is of type "IEnumerable&lt;Dinner&gt;") and compiling the local "dinner" variable as a Dinner type – which means we get full intellisense and compile-time checking for it within code blocks:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

<span data-ttu-id="d5072-263">當我們叫用重新整理 */Dinners*在我們的更新的檢視現在看起來像下面我們瀏覽器中的 URL:</span><span class="sxs-lookup"><span data-stu-id="d5072-263">When we hit refresh on the */Dinners* URL in our browser our updated view now looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

<span data-ttu-id="d5072-264">這會尋找比較好，但尚未完全有。</span><span class="sxs-lookup"><span data-stu-id="d5072-264">This is looking better – but isn't entirely there yet.</span></span> <span data-ttu-id="d5072-265">我們最後一個步驟是讓使用者按一下清單中的個別 Dinners，請參閱相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d5072-265">Our last step is to enable end-users to click individual Dinners in the list and see details about them.</span></span> <span data-ttu-id="d5072-266">我們將會轉譯 HTML 超連結項目連結至詳細資料動作方法，在我們 DinnersController 來實作。</span><span class="sxs-lookup"><span data-stu-id="d5072-266">We'll implement this by rendering HTML hyperlink elements that link to the Details action method on our DinnersController.</span></span>

<span data-ttu-id="d5072-267">在下列其中一種我們 [索引] 檢視中，我們可以產生這些超連結。</span><span class="sxs-lookup"><span data-stu-id="d5072-267">We can generate these hyperlinks within our Index view in one of two ways.</span></span> <span data-ttu-id="d5072-268">第一種是以手動方式建立 HTML &lt;&gt;項目類似下面的其中內嵌&lt;%%&gt;內封鎖&lt;&gt; HTML 項目：</span><span class="sxs-lookup"><span data-stu-id="d5072-268">The first is to manually create HTML &lt;a&gt; elements like below, where we embed &lt;% %&gt; blocks within the &lt;a&gt; HTML element:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

<span data-ttu-id="d5072-269">我們可以使用替代方法是才能內建的 「 Html.ActionLink()"helper 方法，支援以程式設計方式建立 HTML 的 ASP.NET MVC 中充分&lt;&gt;連結至另一個動作方法的項目控制站：</span><span class="sxs-lookup"><span data-stu-id="d5072-269">An alternative approach we can use is to take advantage of the built-in "Html.ActionLink()" helper method within ASP.NET MVC that supports programmatically creating an HTML &lt;a&gt; element that links to another action method on a Controller:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

<span data-ttu-id="d5072-270">Html.ActionLink() helper 方法的第一個參數是要顯示的連結文字 （在此案例中標題的 dinner），第二個參數是我們想要產生的連結 （在此案例的詳細資料的方法，） 的控制器動作名稱和第三個參數要傳送到其中的動作 （實作為具有屬性名稱/值的匿名型別） 的參數集。</span><span class="sxs-lookup"><span data-stu-id="d5072-270">The first parameter to the Html.ActionLink() helper method is the link-text to display (in this case the title of the dinner), the second parameter is the Controller action name we want to generate the link to (in this case the Details method), and the third parameter is a set of parameters to send to the action (implemented as an anonymous type with property name/values).</span></span> <span data-ttu-id="d5072-271">在此情況下，我們會指定我們想要連結至，而且因為 ASP.NET MVC 中的預設 URL 路由規則 dinner 的 「 識別碼 」 參數"{Controller} / {Action} / {id}"Html.ActionLink() helper 方法會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="d5072-271">In this case we are specifying the "id" parameter of the dinner we want to link to, and because the default URL routing rule in ASP.NET MVC is "{Controller}/{Action}/{id}" the Html.ActionLink() helper method will generate the following output:</span></span>

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

<span data-ttu-id="d5072-272">對 Index.aspx 檢視，我們將使用 Html.ActionLink() 協助程式方法的方法，並有清單連結至適當的詳細資料的 URL 中的每個 dinner:</span><span class="sxs-lookup"><span data-stu-id="d5072-272">For our Index.aspx view we'll use the Html.ActionLink() helper method approach and have each dinner in the list link to the appropriate details URL:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

<span data-ttu-id="d5072-273">而且現在當按下 */Dinners* dinner 清單如下所示的 URL:</span><span class="sxs-lookup"><span data-stu-id="d5072-273">And now when we hit the */Dinners* URL our dinner list looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

<span data-ttu-id="d5072-274">當我們按一下任一清單中 Dinners 我們會瀏覽以查看其相關的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="d5072-274">When we click any of the Dinners in the list we'll navigate to see details about it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a><span data-ttu-id="d5072-275">以慣例為基礎的命名和 \Views 目錄結構</span><span class="sxs-lookup"><span data-stu-id="d5072-275">Convention-based naming and the \Views directory structure</span></span>

<span data-ttu-id="d5072-276">預設的 ASP.NET MVC 應用程式會使用以慣例為基礎的目錄，解析檢視範本時，命名結構。</span><span class="sxs-lookup"><span data-stu-id="d5072-276">ASP.NET MVC applications by default use a convention-based directory naming structure when resolving view templates.</span></span> <span data-ttu-id="d5072-277">這可讓開發人員不必完整限定的位置路徑參考從控制器類別內的檢視時。</span><span class="sxs-lookup"><span data-stu-id="d5072-277">This allows developers to avoid having to fully-qualify a location path when referencing views from within a Controller class.</span></span> <span data-ttu-id="d5072-278">根據預設 ASP.NET MVC 會尋找檢視範本檔案中的 * \Views\[ControllerName]\*目錄應用程式下方。</span><span class="sxs-lookup"><span data-stu-id="d5072-278">By default ASP.NET MVC will look for the view template file within the *\Views\[ControllerName]\* directory underneath the application.</span></span>

<span data-ttu-id="d5072-279">比方說，我們一直在處理明確參考三個檢視範本的 DinnersController 類別 –:"Index"、"Details"和"NotFound"。</span><span class="sxs-lookup"><span data-stu-id="d5072-279">For example, we've been working on the DinnersController class – which explicitly references three view templates: "Index", "Details" and "NotFound".</span></span> <span data-ttu-id="d5072-280">ASP.NET MVC 會預設情況下尋找這些檢視內*\Views\Dinners*目錄底下的應用程式根目錄：</span><span class="sxs-lookup"><span data-stu-id="d5072-280">ASP.NET MVC will by default look for these views within the *\Views\Dinners* directory underneath our application root directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

<span data-ttu-id="d5072-281">請注意上述方式有目前專案中的三個控制器類別 (DinnersController HomeController，AccountController – 我們在建立專案時預設便會加入這兩個)，而且有三個子目錄 （一個用於每個控制站） \Views 目錄內。</span><span class="sxs-lookup"><span data-stu-id="d5072-281">Notice above how there are currently three controller classes within the project (DinnersController, HomeController and AccountController – the last two were added by default when we created the project), and there are three sub-directories (one for each controller) within the \Views directory.</span></span>

<span data-ttu-id="d5072-282">從首頁和帳戶控制器所參考的檢視將會自動解除其檢視範本，從個別*\Views\Home*並*\Views\Account*目錄。</span><span class="sxs-lookup"><span data-stu-id="d5072-282">Views referenced from the Home and Accounts controllers will automatically resolve their view templates from the respective *\Views\Home* and *\Views\Account* directories.</span></span> <span data-ttu-id="d5072-283">*\Views\Shared*子目錄提供儲存在應用程式內的多個控制站都是重複使用的檢視範本的方式。</span><span class="sxs-lookup"><span data-stu-id="d5072-283">The *\Views\Shared* sub-directory provides a way to store view templates that are re-used across multiple controllers within the application.</span></span> <span data-ttu-id="d5072-284">當 ASP.NET MVC 會嘗試解析檢視範本時，它首先會檢查內*\Views\[控制器]* 特定目錄中，如果找不到檢視範本那里它看起來會內*\Views\共用*目錄。</span><span class="sxs-lookup"><span data-stu-id="d5072-284">When ASP.NET MVC attempts to resolve a view template, it will first check within the *\Views\[Controller]* specific directory, and if it can't find the view template there it will look within the *\Views\Shared* directory.</span></span>

<span data-ttu-id="d5072-285">談到命名個別的檢視範本的範例，建議的指引，就是讓共用相同的名稱，做為動作方法，導致呈現的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d5072-285">When it comes to naming individual view templates, the recommended guidance is to have the view template share the same name as the action method that caused it to render.</span></span> <span data-ttu-id="d5072-286">例如，上面我們"Index"動作方法使用 [索引] 檢視來呈現檢視結果，而 [詳細資料] 動作的方法使用 [詳細資料] 檢視來呈現其結果。</span><span class="sxs-lookup"><span data-stu-id="d5072-286">For example, above our "Index" action method is using the "Index" view to render the view result, and the "Details" action method is using the "Details" view to render its results.</span></span> <span data-ttu-id="d5072-287">這可讓您方便您快速查看哪一個範本是與每個動作建立關聯。</span><span class="sxs-lookup"><span data-stu-id="d5072-287">This makes it easy to quickly see which template is associated with each action.</span></span>

<span data-ttu-id="d5072-288">開發人員不需要時檢視範本具有相同的名稱，作為在控制器上叫用動作方法，明確指定檢視範本名稱。</span><span class="sxs-lookup"><span data-stu-id="d5072-288">Developers do not need to explicitly specify the view template name when the view template has the same name as the action method being invoked on the controller.</span></span> <span data-ttu-id="d5072-289">我們可以改為只是模型物件傳遞給 「 View()"helper 方法 （如果沒有指定的檢視表名稱），和 ASP.NET MVC 會自動推斷我們想要使用*\Views\[ControllerName]\[ActionName]* 檢視範本呈現的磁碟上。</span><span class="sxs-lookup"><span data-stu-id="d5072-289">We can instead just pass the model object to the "View()" helper method (without specifying the view name), and ASP.NET MVC will automatically infer that we want to use the *\Views\[ControllerName]\[ActionName]* view template on disk to render it.</span></span>

<span data-ttu-id="d5072-290">這可讓我們有點，清除我們的控制器程式碼，並避免重複兩次相同的程式碼名稱：</span><span class="sxs-lookup"><span data-stu-id="d5072-290">This allows us to clean up our controller code a little, and avoid duplicating the name twice in our code:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

<span data-ttu-id="d5072-291">上述程式碼是實作不錯 Dinner 清單/詳細資料所需的所有站台的體驗。</span><span class="sxs-lookup"><span data-stu-id="d5072-291">The above code is all that is needed to implement a nice Dinner listing/details experience for the site.</span></span>

#### <a name="next-step"></a><span data-ttu-id="d5072-292">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="d5072-292">Next Step</span></span>

<span data-ttu-id="d5072-293">我們現在有不錯的 Dinner 瀏覽體驗。</span><span class="sxs-lookup"><span data-stu-id="d5072-293">We now have a nice Dinner browsing experience built.</span></span>

<span data-ttu-id="d5072-294">我們現在就讓編輯支援的 CRUD （建立、 讀取、 更新、 刪除） 資料格式。</span><span class="sxs-lookup"><span data-stu-id="d5072-294">Let's now enable CRUD (Create, Read, Update, Delete) data form editing support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5072-295">[上一頁](build-a-model-with-business-rule-validations.md)
> [下一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)</span><span class="sxs-lookup"><span data-stu-id="d5072-295">[Previous](build-a-model-with-business-rule-validations.md)
[Next](provide-crud-create-read-update-delete-data-form-entry-support.md)</span></span>
