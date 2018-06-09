---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: 使用控制器和檢視來實作詳細資料清單/UI |Microsoft 文件
author: microsoft
description: 步驟 4 說明如何利用我們的模型中為使用者提供的資料清單/詳細資料瀏覽體驗的應用程式中新增的控制站...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30875730"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a><span data-ttu-id="2affa-103">使用控制器和檢視來實作詳細資料清單/UI</span><span class="sxs-lookup"><span data-stu-id="2affa-103">Use Controllers and Views to Implement a Listing/Details UI</span></span>
====================
<span data-ttu-id="2affa-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2affa-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="2affa-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="2affa-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="2affa-106">這是一套免費的步驟 4 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="2affa-106">This is step 4 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="2affa-107">步驟 4 說明如何利用我們的模型中為使用者提供的資料清單/詳細資料瀏覽體驗 dinners 我們 NerdDinner 站台上的應用程式中新增的控制站。</span><span class="sxs-lookup"><span data-stu-id="2affa-107">Step 4 shows how to add a Controller to the application that takes advantage of our model to provide users with a data listing/details navigation experience for dinners on our NerdDinner site.</span></span>
> 
> <span data-ttu-id="2affa-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="2affa-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-4-controllers-and-views"></a><span data-ttu-id="2affa-109">NerdDinner 步驟 4： 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="2affa-109">NerdDinner Step 4: Controllers and Views</span></span>

<span data-ttu-id="2affa-110">與傳統 web 架構 （傳統 ASP、 PHP、 ASP.NET Web Form 等等），傳入的 Url 通常會對應至磁碟上的檔案。</span><span class="sxs-lookup"><span data-stu-id="2affa-110">With traditional web frameworks (classic ASP, PHP, ASP.NET Web Forms, etc), incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="2affa-111">例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會處理由 「 Products.aspx"或"Products.php"檔案。</span><span class="sxs-lookup"><span data-stu-id="2affa-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="2affa-112">網頁型 MVC 架構會稍有不同的方式，將 Url 對應至伺服端程式碼。</span><span class="sxs-lookup"><span data-stu-id="2affa-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="2affa-113">而不是將傳入 Url 對應至檔案，它們改用對應 Url 上類別的方法。</span><span class="sxs-lookup"><span data-stu-id="2affa-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="2affa-114">這些類別稱為 「 控制項 」 而且負責處理傳入的 HTTP 要求處理使用者輸入，就會擷取和儲存資料，以及決定要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案，將重新導向至不同的URL 等）。</span><span class="sxs-lookup"><span data-stu-id="2affa-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc).</span></span>

<span data-ttu-id="2affa-115">既然我們已經建立基本模型 NerdDinner 應用程式下, 一步是新增會利用它為使用者提供的資料清單/詳細資料瀏覽體驗 Dinners 在網站上的應用程式的控制站。</span><span class="sxs-lookup"><span data-stu-id="2affa-115">Now that we have built up a basic model for our NerdDinner application, our next step will be to add a Controller to the application that takes advantage of it to provide users with a data listing/details navigation experience for Dinners on our site.</span></span>

### <a name="adding-a-dinnerscontroller-controller"></a><span data-ttu-id="2affa-116">加入 DinnersController 控制器</span><span class="sxs-lookup"><span data-stu-id="2affa-116">Adding a DinnersController Controller</span></span>

<span data-ttu-id="2affa-117">我們將開始在 web 專案中，「 控制項 」 資料夾上按一下滑鼠右鍵，然後選取**新增-&gt;控制器**（您也可以輸入 Ctrl-M、 Ctrl + C 來執行此命令） 的功能表命令：</span><span class="sxs-lookup"><span data-stu-id="2affa-117">We'll begin by right-clicking on the "Controllers" folder within our web project, and then select the **Add-&gt;Controller** menu command (you can also execute this command by typing Ctrl-M, Ctrl-C):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

<span data-ttu-id="2affa-118">這會顯示 「 新增控制器 」 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="2affa-118">This will bring up the "Add Controller" dialog:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

<span data-ttu-id="2affa-119">我們將新的控制器"DinnersController 」，然後按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2affa-119">We'll name the new controller "DinnersController" and click the "Add" button.</span></span> <span data-ttu-id="2affa-120">Visual Studio 會再加入 DinnersController.cs 檔案我們 \Controllers 目錄下：</span><span class="sxs-lookup"><span data-stu-id="2affa-120">Visual Studio will then add a DinnersController.cs file under our \Controllers directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

<span data-ttu-id="2affa-121">它也會開啟新 DinnersController 類別在程式碼編輯器中。</span><span class="sxs-lookup"><span data-stu-id="2affa-121">It will also open up the new DinnersController class within the code-editor.</span></span>

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a><span data-ttu-id="2affa-122">將 index （） 和 Details() 動作方法加入至 DinnersController 類別</span><span class="sxs-lookup"><span data-stu-id="2affa-122">Adding Index() and Details() Action Methods to the DinnersController Class</span></span>

<span data-ttu-id="2affa-123">我們想要讓訪客使用我們的應用程式瀏覽一份即將 dinners，並讓他們按一下清單中任何 Dinner，請參閱相關的特定詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2affa-123">We want to enable visitors using our application to browse a list of upcoming dinners, and allow them to click on any Dinner in the list to see specific details about it.</span></span> <span data-ttu-id="2affa-124">我們將會發行下列 Url 從我們的應用程式來執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="2affa-124">We'll do this by publishing the following URLs from our application:</span></span>

| <span data-ttu-id="2affa-125">**URL**</span><span class="sxs-lookup"><span data-stu-id="2affa-125">**URL**</span></span> | <span data-ttu-id="2affa-126">**目的**</span><span class="sxs-lookup"><span data-stu-id="2affa-126">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="2affa-127">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="2affa-127">*/Dinners/*</span></span> | <span data-ttu-id="2affa-128">顯示即將 dinners HTML 清單</span><span class="sxs-lookup"><span data-stu-id="2affa-128">Display an HTML list of upcoming dinners</span></span> |
| <span data-ttu-id="2affa-129">*/Dinners/Details/[id]*</span><span class="sxs-lookup"><span data-stu-id="2affa-129">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="2affa-130">顯示有關特定的 dinner 內嵌於 URL-會比對的資料庫中 dinner DinnerID 「 識別碼 」 參數所指示的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2affa-130">Display details about a specific dinner indicated by an "id" parameter embedded within the URL – which will match the DinnerID of the dinner in the database.</span></span> <span data-ttu-id="2affa-131">例如： /Dinners/Details/2 會顯示的 HTML 網頁 Dinner DinnerID 值為 2 的相關詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2affa-131">For example: /Dinners/Details/2 would display an HTML page with details about the Dinner whose DinnerID value is 2.</span></span> |

<span data-ttu-id="2affa-132">我們將發佈這些 Url 初始的實作兩個公用 「 執行方法 」 類別新增為我們 DinnersController 類似如下：</span><span class="sxs-lookup"><span data-stu-id="2affa-132">We will publish initial implementations of these URLs by adding two public "action methods" to our DinnersController class like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

<span data-ttu-id="2affa-133">接著，我們會執行 NerdDinner 應用程式，並使用我們的瀏覽器叫用它們。</span><span class="sxs-lookup"><span data-stu-id="2affa-133">We'll then run the NerdDinner application and use our browser to invoke them.</span></span> <span data-ttu-id="2affa-134">在中輸入 *"/ Dinners /"* URL 會導致我們*index*執行，以及它的方法會傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="2affa-134">Typing in the *"/Dinners/"* URL will cause our *Index()* method to run, and it will send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

<span data-ttu-id="2affa-135">在中輸入 *"/ Dinners/詳細資料/2"* URL 會導致我們*Details()* 方法來執行，並傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="2affa-135">Typing in the *"/Dinners/Details/2"* URL will cause our *Details()* method to run, and send back the following response:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

<span data-ttu-id="2affa-136">您可能會懷疑-如何 ASP.NET MVC 知道建立 DinnersController 類別和叫用這些方法？</span><span class="sxs-lookup"><span data-stu-id="2affa-136">You might be wondering - how did ASP.NET MVC know to create our DinnersController class and invoke those methods?</span></span> <span data-ttu-id="2affa-137">若要了解，讓我們快速看看如何路由的運作。</span><span class="sxs-lookup"><span data-stu-id="2affa-137">To understand that let's take a quick look at how routing works.</span></span>

### <a name="understanding-aspnet-mvc-routing"></a><span data-ttu-id="2affa-138">了解 ASP.NET MVC 路由</span><span class="sxs-lookup"><span data-stu-id="2affa-138">Understanding ASP.NET MVC Routing</span></span>

<span data-ttu-id="2affa-139">ASP.NET MVC 包含功能強大的 URL 路由引擎提供很大的彈性來控制如何將 Url 對應至控制器類別。</span><span class="sxs-lookup"><span data-stu-id="2affa-139">ASP.NET MVC includes a powerful URL routing engine that provides a lot of flexibility in controlling how URLs are mapped to controller classes.</span></span> <span data-ttu-id="2affa-140">它可讓我們完全自訂 ASP.NET MVC 如何選擇控制器類別建立時，要在其上叫用，以及設定不同的方式，變數可自動從 URL/Querystring 剖析和傳遞給方法的參數的方法引數。</span><span class="sxs-lookup"><span data-stu-id="2affa-140">It allows us to completely customize how ASP.NET MVC chooses which controller class to create, which method to invoke on it, as well as configure different ways that variables can be automatically parsed from the URL/Querystring and passed to the method as parameter arguments.</span></span> <span data-ttu-id="2affa-141">它會傳遞完全最佳化 SEO （搜尋引擎最佳化） 的站台，以及發行任何我們想要從應用程式的 URL 結構的彈性。</span><span class="sxs-lookup"><span data-stu-id="2affa-141">It delivers the flexibility to totally optimize a site for SEO (search engine optimization) as well as publish any URL structure we want from an application.</span></span>

<span data-ttu-id="2affa-142">根據預設，新的 ASP.NET MVC 專案會隨附一組預先設定的已登錄的 URL 路由規則。</span><span class="sxs-lookup"><span data-stu-id="2affa-142">By default, new ASP.NET MVC projects come with a preconfigured set of URL routing rules already registered.</span></span> <span data-ttu-id="2affa-143">這可讓我們來輕鬆地開始使用的應用程式不需要明確設定的任何項目。</span><span class="sxs-lookup"><span data-stu-id="2affa-143">This enables us to easily get started on an application without having to explicitly configure anything.</span></span> <span data-ttu-id="2affa-144">預設路由規則註冊，請參閱我們的專案 – 我們可以按兩下以開啟 「 Global.asax"檔案受測專案的根目錄中的 「 應用程式 」 類別：</span><span class="sxs-lookup"><span data-stu-id="2affa-144">The default routing rule registrations can be found within the "Application" class of our projects – which we can open by double-clicking the "Global.asax" file in the root of our project:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

<span data-ttu-id="2affa-145">「 RegisterRoutes"方法，這個類別的內註冊預設的 ASP.NET MVC 路由規則：</span><span class="sxs-lookup"><span data-stu-id="2affa-145">The default ASP.NET MVC routing rules are registered within the "RegisterRoutes" method of this class:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

<span data-ttu-id="2affa-146">「 路由。MapRoute()"上述的方法呼叫註冊將傳入 Url 對應至控制器類別使用的 URL 格式的預設路由規則: 「 / {controller} / {action} / {id}"– 其中 「 控制器 」 是具現化，控制器類別的名稱 「 動作 」 是的名稱公用方法來叫用，「 識別碼 」 是可以做為引數傳遞給方法的 URL 中內嵌的選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2affa-146">The "routes.MapRoute()" method call above registers a default routing rule that maps incoming URLs to controller classes using the URL format: "/{controller}/{action}/{id}" – where "controller" is the name of the controller class to instantiate, "action" is the name of a public method to invoke on it, and "id" is an optional parameter embedded within the URL that can be passed as an argument to the method.</span></span> <span data-ttu-id="2affa-147">傳遞到 「 MapRoute()"方法呼叫的第三個參數是一組用於控制器/動作識別碼值，它們不會出現在 URL 中的預設值 (控制器 ="Home"，動作 ="Index"，識別碼 ="")。</span><span class="sxs-lookup"><span data-stu-id="2affa-147">The third parameter passed to the "MapRoute()" method call is a set of default values to use for the controller/action/id values in the event that they are not present in the URL (Controller = "Home", Action="Index", Id="").</span></span>

<span data-ttu-id="2affa-148">以下是使用預設對應的資料表，示範如何在各種不同的 Url"<em>/ {控制器} / {action} / {id}"</em>路由規則：</span><span class="sxs-lookup"><span data-stu-id="2affa-148">Below is a table that demonstrates how a variety of URLs are mapped using the default "<em>/{controllers}/{action}/{id}"</em>route rule:</span></span>

| <span data-ttu-id="2affa-149">**URL**</span><span class="sxs-lookup"><span data-stu-id="2affa-149">**URL**</span></span> | <span data-ttu-id="2affa-150">**控制器類別**</span><span class="sxs-lookup"><span data-stu-id="2affa-150">**Controller Class**</span></span> | <span data-ttu-id="2affa-151">**動作方法**</span><span class="sxs-lookup"><span data-stu-id="2affa-151">**Action Method**</span></span> | <span data-ttu-id="2affa-152">**傳遞參數**</span><span class="sxs-lookup"><span data-stu-id="2affa-152">**Parameters Passed**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2affa-153">*/Dinners/Details/2*</span><span class="sxs-lookup"><span data-stu-id="2affa-153">*/Dinners/Details/2*</span></span> | <span data-ttu-id="2affa-154">DinnersController</span><span class="sxs-lookup"><span data-stu-id="2affa-154">DinnersController</span></span> | <span data-ttu-id="2affa-155">Details(id)</span><span class="sxs-lookup"><span data-stu-id="2affa-155">Details(id)</span></span> | <span data-ttu-id="2affa-156">id=2</span><span class="sxs-lookup"><span data-stu-id="2affa-156">id=2</span></span> |
| <span data-ttu-id="2affa-157">*/Dinners/Edit/5*</span><span class="sxs-lookup"><span data-stu-id="2affa-157">*/Dinners/Edit/5*</span></span> | <span data-ttu-id="2affa-158">DinnersController</span><span class="sxs-lookup"><span data-stu-id="2affa-158">DinnersController</span></span> | <span data-ttu-id="2affa-159">Edit(id)</span><span class="sxs-lookup"><span data-stu-id="2affa-159">Edit(id)</span></span> | <span data-ttu-id="2affa-160">id=5</span><span class="sxs-lookup"><span data-stu-id="2affa-160">id=5</span></span> |
| <span data-ttu-id="2affa-161">*/Dinners/Create*</span><span class="sxs-lookup"><span data-stu-id="2affa-161">*/Dinners/Create*</span></span> | <span data-ttu-id="2affa-162">DinnersController</span><span class="sxs-lookup"><span data-stu-id="2affa-162">DinnersController</span></span> | <span data-ttu-id="2affa-163">Create （)</span><span class="sxs-lookup"><span data-stu-id="2affa-163">Create()</span></span> | <span data-ttu-id="2affa-164">N/A</span><span class="sxs-lookup"><span data-stu-id="2affa-164">N/A</span></span> |
| <span data-ttu-id="2affa-165">*/ Dinners*</span><span class="sxs-lookup"><span data-stu-id="2affa-165">*/Dinners*</span></span> | <span data-ttu-id="2affa-166">DinnersController</span><span class="sxs-lookup"><span data-stu-id="2affa-166">DinnersController</span></span> | <span data-ttu-id="2affa-167">Index （)</span><span class="sxs-lookup"><span data-stu-id="2affa-167">Index()</span></span> | <span data-ttu-id="2affa-168">N/A</span><span class="sxs-lookup"><span data-stu-id="2affa-168">N/A</span></span> |
| <span data-ttu-id="2affa-169">*/ 首頁*</span><span class="sxs-lookup"><span data-stu-id="2affa-169">*/Home*</span></span> | <span data-ttu-id="2affa-170">HomeController</span><span class="sxs-lookup"><span data-stu-id="2affa-170">HomeController</span></span> | <span data-ttu-id="2affa-171">Index （)</span><span class="sxs-lookup"><span data-stu-id="2affa-171">Index()</span></span> | <span data-ttu-id="2affa-172">N/A</span><span class="sxs-lookup"><span data-stu-id="2affa-172">N/A</span></span> |
| */* | <span data-ttu-id="2affa-173">HomeController</span><span class="sxs-lookup"><span data-stu-id="2affa-173">HomeController</span></span> | <span data-ttu-id="2affa-174">Index （)</span><span class="sxs-lookup"><span data-stu-id="2affa-174">Index()</span></span> | <span data-ttu-id="2affa-175">N/A</span><span class="sxs-lookup"><span data-stu-id="2affa-175">N/A</span></span> |

<span data-ttu-id="2affa-176">最後三個資料列會顯示預設值 (控制器 = 首頁，動作 = 索引，Id ="") 正在使用。</span><span class="sxs-lookup"><span data-stu-id="2affa-176">The last three rows show the default values (Controller = Home, Action = Index, Id = "") being used.</span></span> <span data-ttu-id="2affa-177">因為 「 索引 」 方法註冊為預設動作名稱，如果沒有指定，"/ Dinners"和"/home"Url 原因 index 動作方法的控制器類別上叫用。</span><span class="sxs-lookup"><span data-stu-id="2affa-177">Because the "Index" method is registered as the default action name if one isn't specified, the "/Dinners" and "/Home" URLs cause the Index() action method to be invoked on their Controller classes.</span></span> <span data-ttu-id="2affa-178">「 組織 」 控制站登錄為預設的控制站中，如果未指定，因為"/"的 URL，則會導致要建立、 HomeController 和 index 動作方法上叫用。</span><span class="sxs-lookup"><span data-stu-id="2affa-178">Because the "Home" controller is registered as the default controller if one isn't specified, the "/" URL causes the HomeController to be created, and the Index() action method on it to be invoked.</span></span>

<span data-ttu-id="2affa-179">如果您不喜歡這些預設 URL 的路由規則，好消息是可輕鬆地變更-只內 RegisterRoutes 上述方法，編輯它們。</span><span class="sxs-lookup"><span data-stu-id="2affa-179">If you don't like these default URL routing rules, the good news is that they are easy to change - just edit them within the RegisterRoutes method above.</span></span> <span data-ttu-id="2affa-180">NerdDinner 應用程式，不過，我們不打算變更任何預設 URL 的路由規則 – 改為我們只是使用以-為。</span><span class="sxs-lookup"><span data-stu-id="2affa-180">For our NerdDinner application, though, we aren't going to change any of the default URL routing rules – instead we'll just use them as-is.</span></span>

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a><span data-ttu-id="2affa-181">使用從我們 DinnersController DinnerRepository</span><span class="sxs-lookup"><span data-stu-id="2affa-181">Using the DinnerRepository from our DinnersController</span></span>

<span data-ttu-id="2affa-182">現在讓我們來取代 DinnersController 我們目前實作與使用我們的模型中實作，index （） 和 Details() 動作方法。</span><span class="sxs-lookup"><span data-stu-id="2affa-182">Let's now replace our current implementation of the DinnersController's Index() and Details() action methods with implementations that use our model.</span></span>

<span data-ttu-id="2affa-183">我們會使用我們稍早建立實作該行為 DinnerRepository 類別。</span><span class="sxs-lookup"><span data-stu-id="2affa-183">We'll use the DinnerRepository class we built earlier to implement the behavior.</span></span> <span data-ttu-id="2affa-184">我們會藉由新增參考的 「 NerdDinner.Models"命名空間中，"using"陳述式開始，然後將我們 DinnerRepository 的執行個體宣告做為欄位中，我們 DinnerController 類別上變更。</span><span class="sxs-lookup"><span data-stu-id="2affa-184">We'll begin by adding a "using" statement that references the "NerdDinner.Models" namespace, and then declare an instance of our DinnerRepository as a field on our DinnerController class.</span></span>

<span data-ttu-id="2affa-185">在本章稍後我們將介紹概念 「 相依性插入 」，並顯示我們的控制站，以取得更好的單元可讓 DinnerRepository 參考另一種方法，測試 –，但權限現在我們將只建立我們 DinnerRepository 的執行個體下面類似內嵌。</span><span class="sxs-lookup"><span data-stu-id="2affa-185">Later in this chapter we'll introduce the concept of "Dependency Injection" and show another way for our Controllers to obtain a reference to a DinnerRepository that enables better unit testing – but for right now we'll just create an instance of our DinnerRepository inline like below.</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

<span data-ttu-id="2affa-186">現在我們已經準備好產生使用我們擷取的資料模型物件的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="2affa-186">Now we are ready to generate a HTML response back using our retrieved data model objects.</span></span>

### <a name="using-views-with-our-controller"></a><span data-ttu-id="2affa-187">使用我們的控制器與檢視</span><span class="sxs-lookup"><span data-stu-id="2affa-187">Using Views with our Controller</span></span>

<span data-ttu-id="2affa-188">雖然您可以撰寫程式碼來組合 HTML，然後使用我們的動作方法內*Response.write* helper 方法，可將它傳送回用戶端，該方法就會成為變得相當快速。</span><span class="sxs-lookup"><span data-stu-id="2affa-188">While it is possible to write code within our action methods to assemble HTML and then use the *Response.Write()* helper method to send it back to the client, that approach becomes fairly unwieldy quickly.</span></span> <span data-ttu-id="2affa-189">更好的方法是讓我們只能執行應用程式和資料邏輯內我們 DinnersController 動作方法，並將再傳遞來呈現 HTML 回應給負責輸出 HTML 表示個別的 「 檢視 」 範本所需的資料它。</span><span class="sxs-lookup"><span data-stu-id="2affa-189">A much better approach is for us to only perform application and data logic inside our DinnersController action methods, and to then pass the data needed to render a HTML response to a separate "view" template that is responsible for outputting the HTML representation of it.</span></span> <span data-ttu-id="2affa-190">我們會看到在不久後，「 檢視 」 範本將會是文字檔案，其中通常包含 HTML 標記和內嵌的轉譯程式碼的組合。</span><span class="sxs-lookup"><span data-stu-id="2affa-190">As we'll see in a moment, a "view" template is a text file that typically contains a combination of HTML markup and embedded rendering code.</span></span>

<span data-ttu-id="2affa-191">我們控制器邏輯分離我們檢視轉譯會帶來幾個好處。</span><span class="sxs-lookup"><span data-stu-id="2affa-191">Separating our controller logic from our view rendering brings several big benefits.</span></span> <span data-ttu-id="2affa-192">尤其是它會協助強化清楚 」 的重要性分離 」 應用程式程式碼與 UI 呈現格式化程式碼之間。</span><span class="sxs-lookup"><span data-stu-id="2affa-192">In particular it helps enforce a clear "separation of concerns" between the application code and UI formatting/rendering code.</span></span> <span data-ttu-id="2affa-193">這使得更容易隔離單元測試應用程式邏輯與 UI 呈現邏輯。</span><span class="sxs-lookup"><span data-stu-id="2affa-193">This makes it much easier to unit-test application logic in isolation from UI rendering logic.</span></span> <span data-ttu-id="2affa-194">它可讓您稍後修改 UI 轉譯範本，而不需要變更應用程式程式碼更容易。</span><span class="sxs-lookup"><span data-stu-id="2affa-194">It makes it easier to later modify the UI rendering templates without having to make application code changes.</span></span> <span data-ttu-id="2affa-195">它可以讓它更容易開發人員和設計師一起共同作業專案。</span><span class="sxs-lookup"><span data-stu-id="2affa-195">And it can make it easier for developers and designers to collaborate together on projects.</span></span>

<span data-ttu-id="2affa-196">我們可以更新 DinnersController 類別來表示我們想要透過檢視表範本傳送回 HTML UI 回應變更我們的兩個動作方法的方法簽章的傳回類型為"void"改為使用"ActionResult"傳回型別。</span><span class="sxs-lookup"><span data-stu-id="2affa-196">We can update our DinnersController class to indicate that we want to use a view template to send back an HTML UI response by changing the method signatures of our two action methods from having a return type of "void" to instead have a return type of "ActionResult".</span></span> <span data-ttu-id="2affa-197">我們可以呼叫*View()* 傳回到控制器基底類別的 helper 方法類似下面的 「 ViewResult"物件：</span><span class="sxs-lookup"><span data-stu-id="2affa-197">We can then call the *View()* helper method on the Controller base class to return back a "ViewResult" object like below:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

<span data-ttu-id="2affa-198">簽章*View()* 我們使用上述的 helper 方法看起來如下：</span><span class="sxs-lookup"><span data-stu-id="2affa-198">The signature of the *View()* helper method we are using above looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

<span data-ttu-id="2affa-199">第一個參數*View()* helper 方法是我們想要用來呈現 HTML 回應的檢視範本檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="2affa-199">The first parameter to the *View()* helper method is the name of the view template file we want to use to render the HTML response.</span></span> <span data-ttu-id="2affa-200">第二個參數是包含資料中呈現 HTML 回應所需要的檢視表範本的模型物件。</span><span class="sxs-lookup"><span data-stu-id="2affa-200">The second parameter is a model object that contains the data that the view template needs in order to render the HTML response.</span></span>

<span data-ttu-id="2affa-201">在我們的 index 動作方法內，我們正在撥打*View()* helper 方法，並指出我們想要呈現 dinners 使用 「 索引 」 檢視範本的 HTML 清單。</span><span class="sxs-lookup"><span data-stu-id="2affa-201">Within our Index() action method we are calling the *View()* helper method and indicating that we want to render an HTML listing of dinners using an "Index" view template.</span></span> <span data-ttu-id="2affa-202">我們在傳遞檢視範本的 Dinner 物件產生的清單，從序列：</span><span class="sxs-lookup"><span data-stu-id="2affa-202">We are passing the view template a sequence of Dinner objects to generate the list from:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

<span data-ttu-id="2affa-203">我們 Details() 動作方法內我們嘗試擷取 Dinner 物件使用的 URL 中提供的識別碼。</span><span class="sxs-lookup"><span data-stu-id="2affa-203">Within our Details() action method we attempt to retrieve a Dinner object using the id provided within the URL.</span></span> <span data-ttu-id="2affa-204">如果找到有效的 Dinner 我們呼叫*View()* helper 方法，表示我們想要使用 [詳細資料] 檢視範本來擷取的 Dinner 物件呈現。</span><span class="sxs-lookup"><span data-stu-id="2affa-204">If a valid Dinner is found we call the *View()* helper method, indicating we want to use a "Details" view template to render the retrieved Dinner object.</span></span> <span data-ttu-id="2affa-205">如果要求不正確的 dinner，我們會呈現表示 Dinner 不存在使用"NotFound"檢視範本的有用的錯誤訊息 (與多載的版本*View()* helper 方法，只使用範本名稱):</span><span class="sxs-lookup"><span data-stu-id="2affa-205">If an invalid dinner is requested, we render a helpful error message that indicates that the Dinner doesn't exist using a "NotFound" view template (and an overloaded version of the *View()* helper method that just takes the template name):</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

<span data-ttu-id="2affa-206">讓我們現在實作的"NotFound"、 「 詳細資料 」 和 「 索引 」 的 「 檢視 」 範本。</span><span class="sxs-lookup"><span data-stu-id="2affa-206">Let's now implement the "NotFound", "Details", and "Index" view templates.</span></span>

### <a name="implementing-the-notfound-view-template"></a><span data-ttu-id="2affa-207">實作"NotFound"檢視範本</span><span class="sxs-lookup"><span data-stu-id="2affa-207">Implementing the "NotFound" View Template</span></span>

<span data-ttu-id="2affa-208">我們一開始會藉由實作"NotFound"檢視範本 – 顯示易懂的錯誤訊息，指出找不到要求的 dinner。</span><span class="sxs-lookup"><span data-stu-id="2affa-208">We'll begin by implementing the "NotFound" view template – which displays a friendly error message indicating that the requested dinner can't be found.</span></span>

<span data-ttu-id="2affa-209">我們將建立新的檢視表範本放在控制器動作方法中，我們文字游標然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （我們也可以輸入 Ctrl-M、 Ctrl-V 來執行此命令）：</span><span class="sxs-lookup"><span data-stu-id="2affa-209">We'll create a new view template by positioning our text cursor within a controller action method, and then right click and choose the "Add View" menu command (we can also execute this command by typing Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

<span data-ttu-id="2affa-210">這會顯示類似的 [新增檢視] 對話方塊下方。</span><span class="sxs-lookup"><span data-stu-id="2affa-210">This will bring up an "Add View" dialog like below.</span></span> <span data-ttu-id="2affa-211">根據預設，對話方塊會預先填入要建立之檢視的名稱要比對動作方法的名稱資料指標時所在的對話方塊已啟動 （在此情況下 [詳細資料]）。</span><span class="sxs-lookup"><span data-stu-id="2affa-211">By default the dialog will pre-populate the name of the view to create to match the name of the action method the cursor was in when the dialog was launched (in this case "Details").</span></span> <span data-ttu-id="2affa-212">由於我們想要先實作"NotFound"範本，我們將會覆寫這個檢視表名稱，並設定它改為"NotFound":</span><span class="sxs-lookup"><span data-stu-id="2affa-212">Because we want to first implement the "NotFound" template, we'll override this view name and set it to instead be "NotFound":</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

<span data-ttu-id="2affa-213">當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄 （若目錄不存在時，它也會建立） 內就讓我們建立新的"NotFound.aspx 」 檢視範本：</span><span class="sxs-lookup"><span data-stu-id="2affa-213">When we click the "Add" button, Visual Studio will create a new "NotFound.aspx" view template for us within the "\Views\Dinners" directory (which it will also create if the directory doesn't already exist):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

<span data-ttu-id="2affa-214">它也會開啟我們新"NotFound.aspx 」 檢視的範本程式碼編輯器中：</span><span class="sxs-lookup"><span data-stu-id="2affa-214">It will also open up our new "NotFound.aspx" view template within the code-editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

<span data-ttu-id="2affa-215">檢視預設的範本有兩個 「 內容區域 」，我們可以在其中新增內容及程式碼。</span><span class="sxs-lookup"><span data-stu-id="2affa-215">View templates by default have two "content regions" where we can add content and code.</span></span> <span data-ttu-id="2affa-216">第一個可讓我們自訂"title"的 HTML 網頁送回。</span><span class="sxs-lookup"><span data-stu-id="2affa-216">The first allows us to customize the "title" of the HTML page sent back.</span></span> <span data-ttu-id="2affa-217">第二個可讓我們用來自訂 「 主要內容 」 的 HTML 網頁送回。</span><span class="sxs-lookup"><span data-stu-id="2affa-217">The second allows us to customize the "main content" of the HTML page sent back.</span></span>

<span data-ttu-id="2affa-218">若要實作我們"NotFound"檢視範本中，我們會將新增一些基本的內容：</span><span class="sxs-lookup"><span data-stu-id="2affa-218">To implement our "NotFound" view template we'll add some basic content:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

<span data-ttu-id="2affa-219">我們可以再現在就試試看瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="2affa-219">We can then try it out within the browser.</span></span> <span data-ttu-id="2affa-220">若要這樣做讓我們要求 *"/ Dinners/詳細資料/9999"* URL。</span><span class="sxs-lookup"><span data-stu-id="2affa-220">To do this let's request the *"/Dinners/Details/9999"* URL.</span></span> <span data-ttu-id="2affa-221">這會指 dinner，目前不存在於資料庫中，且會導致我們 DinnersController.Details() 動作方法，以呈現我們"NotFound"檢視的範本：</span><span class="sxs-lookup"><span data-stu-id="2affa-221">This will refer to a dinner that doesn't currently exist in the database, and will cause our DinnersController.Details() action method to render our "NotFound" view template:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

<span data-ttu-id="2affa-222">您會注意到上述螢幕擷取畫面中的一件事是 HTML 的我們基本的檢視的範本已繼承一堆圍繞在螢幕上的主要內容。</span><span class="sxs-lookup"><span data-stu-id="2affa-222">One thing you'll notice in the screen-shot above is that our basic view template has inherited a bunch of HTML that surrounds the main content on the screen.</span></span> <span data-ttu-id="2affa-223">這是因為我們檢視範本使用的"master page"範本，讓我們在站台上的所有檢視之間套用一致的版面配置。</span><span class="sxs-lookup"><span data-stu-id="2affa-223">This is because our view-template is using a "master page" template that enables us to apply a consistent layout across all views on the site.</span></span> <span data-ttu-id="2affa-224">我們將討論主版頁面在本教學課程的後續部分中的多個工作的方式。</span><span class="sxs-lookup"><span data-stu-id="2affa-224">We'll discuss how master pages work more in a later part of this tutorial.</span></span>

### <a name="implementing-the-details-view-template"></a><span data-ttu-id="2affa-225">實作 [詳細資料] 檢視範本</span><span class="sxs-lookup"><span data-stu-id="2affa-225">Implementing the "Details" View Template</span></span>

<span data-ttu-id="2affa-226">讓我們現在實作的 [詳細資料] 檢視範本 – 將單一 Dinner 模型產生 HTML。</span><span class="sxs-lookup"><span data-stu-id="2affa-226">Let's now implement the "Details" view template – which will generate HTML for a single Dinner model.</span></span>

<span data-ttu-id="2affa-227">我們會這麼做定位在詳細資料動作方法中，我們文字游標然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）：</span><span class="sxs-lookup"><span data-stu-id="2affa-227">We'll do this by positioning our text cursor within the Details action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

<span data-ttu-id="2affa-228">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="2affa-228">This will bring up the "Add View" dialog.</span></span> <span data-ttu-id="2affa-229">我們會保留預設檢視表名稱 ("Details")。</span><span class="sxs-lookup"><span data-stu-id="2affa-229">We'll keep the default view name ("Details").</span></span> <span data-ttu-id="2affa-230">我們也會在對話方塊中選取 [建立強型別檢視表] 核取方塊，然後選取 （使用下拉式方塊的下拉式清單中） 我們傳遞從控制器到檢視的模型型別名稱。</span><span class="sxs-lookup"><span data-stu-id="2affa-230">We'll also select the "Create a strongly-typed View" checkbox in the dialog and select (using the combobox dropdown) the name of the model type we are passing from the Controller to the View.</span></span> <span data-ttu-id="2affa-231">此檢視中，我們會傳遞 Dinner 物件 (此類型的完整的名稱是: 「 NerdDinner.Models.Dinner"):</span><span class="sxs-lookup"><span data-stu-id="2affa-231">For this view we are passing a Dinner object (the fully qualified name for this type is: "NerdDinner.Models.Dinner"):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

<span data-ttu-id="2affa-232">不同於先前的範本，其中我們選擇建立的 「 空的檢視 」，的此時，我們會自動選擇"scaffold 」 檢視使用 [詳細資料] 範本。</span><span class="sxs-lookup"><span data-stu-id="2affa-232">Unlike the previous template, where we chose to create an "Empty View", this time we will choose to automatically "scaffold" the view using a "Details" template.</span></span> <span data-ttu-id="2affa-233">我們可以變更 [檢視內容] 下拉式清單中上述對話方塊來表示。</span><span class="sxs-lookup"><span data-stu-id="2affa-233">We can indicate this by changing the "View content" drop-down in the dialog above.</span></span>

<span data-ttu-id="2affa-234">「 Scaffolding"會產生初始我們根據我們要傳遞給它的 Dinner 物件的詳細資料檢視範本的實作。</span><span class="sxs-lookup"><span data-stu-id="2affa-234">"Scaffolding" will generate an initial implementation of our details view template based on the Dinner object we are passing to it.</span></span> <span data-ttu-id="2affa-235">這提供簡單的方法，讓我們快速地開始在我們檢視範本的實作。</span><span class="sxs-lookup"><span data-stu-id="2affa-235">This provides an easy way for us to quickly get started on our view template implementation.</span></span>

<span data-ttu-id="2affa-236">當我們按一下 [新增] 按鈕時，Visual Studio 會我們"\Views\Dinners"目錄中就我們建立新的 「 Details.aspx 」 檢視的範本檔：</span><span class="sxs-lookup"><span data-stu-id="2affa-236">When we click the "Add" button, Visual Studio will create a new "Details.aspx" view template file for us within our "\Views\Dinners" directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

<span data-ttu-id="2affa-237">它也會開啟程式碼編輯器中我們新"Details.aspx 」 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="2affa-237">It will also open up our new "Details.aspx" view template within the code-editor.</span></span> <span data-ttu-id="2affa-238">它會包含詳細資料檢視 Dinner 模型為基礎的初始 scaffold 實作。</span><span class="sxs-lookup"><span data-stu-id="2affa-238">It will contain an initial scaffold implementation of a details view based on a Dinner model.</span></span> <span data-ttu-id="2affa-239">Scaffolding 引擎會使用.NET 反映來查看傳遞它，在類別上公開的公用屬性，並會加入適當的內容根據每個找到的類型：</span><span class="sxs-lookup"><span data-stu-id="2affa-239">The scaffolding engine uses .NET reflection to look at the public properties exposed on the class passed it, and will add appropriate content based on each type it finds:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

<span data-ttu-id="2affa-240">我們可以要求 *"/ 1/Dinners/詳細資料 」* URL，請參閱此 [詳細資料] scaffold 實作在瀏覽器中的外觀。</span><span class="sxs-lookup"><span data-stu-id="2affa-240">We can request the *"/Dinners/Details/1"* URL to see what this "details" scaffold implementation looks like in the browser.</span></span> <span data-ttu-id="2affa-241">使用此 URL 將會顯示我們手動新增至資料庫時，我們建立 dinners 的其中一個：</span><span class="sxs-lookup"><span data-stu-id="2affa-241">Using this URL will display one of the dinners we manually added to our database when we first created it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

<span data-ttu-id="2affa-242">這會啟動並執行快速，讓我們，並提供我們的我們 Details.aspx 檢視的初始實作。</span><span class="sxs-lookup"><span data-stu-id="2affa-242">This gets us up and running quickly, and provides us with an initial implementation of our Details.aspx view.</span></span> <span data-ttu-id="2affa-243">我們可以前往並調整其自訂的 UI，讓我們滿意。</span><span class="sxs-lookup"><span data-stu-id="2affa-243">We can then go and tweak it to customize the UI to our satisfaction.</span></span>

<span data-ttu-id="2affa-244">當我們探討 Details.aspx 範本更緊密時，我們會發現它包含靜態的 HTML 以及內嵌轉譯程式碼。</span><span class="sxs-lookup"><span data-stu-id="2affa-244">When we look at the Details.aspx template more closely, we'll find that it contains static HTML as well as embedded rendering code.</span></span> <span data-ttu-id="2affa-245">&lt;%%&gt;程式碼區塊會執行程式碼時檢視範本轉譯時，和&lt;%= %&gt;程式碼區塊執行其中所包含的程式碼，然後轉譯輸出資料流的範本結果。</span><span class="sxs-lookup"><span data-stu-id="2affa-245">&lt;% %&gt; code nuggets execute code when the view template renders, and &lt;%= %&gt; code nuggets execute the code contained within them and then render the result to the output stream of the template.</span></span>

<span data-ttu-id="2affa-246">我們會存取 「 Dinner 」 模型物件傳遞從 controller 使用強型別 「 模型 」 屬性的檢視中，我們可以撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="2affa-246">We can write code within our View that accesses the "Dinner" model object that was passed from our controller using a strongly-typed "Model" property.</span></span> <span data-ttu-id="2affa-247">Visual Studio 提供完整的 intellisense 程式碼時存取這個 「 模型 」 的屬性，在編輯器中：</span><span class="sxs-lookup"><span data-stu-id="2affa-247">Visual Studio provides us with full code-intellisense when accessing this "Model" property within the editor:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

<span data-ttu-id="2affa-248">讓我們進行某些做調整，讓我們最後一個詳細資料檢視範本的來源看起來如下：</span><span class="sxs-lookup"><span data-stu-id="2affa-248">Let's make some tweaks so that the source for our final Details view template looks like below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

<span data-ttu-id="2affa-249">當存取 *"/ 1/Dinners/詳細資料 」* URL 一次它即將轉譯類似下面的：</span><span class="sxs-lookup"><span data-stu-id="2affa-249">When we access the *"/Dinners/Details/1"* URL again it will now render like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a><span data-ttu-id="2affa-250">實作 「 索引 」 檢視範本</span><span class="sxs-lookup"><span data-stu-id="2affa-250">Implementing the "Index" View Template</span></span>

<span data-ttu-id="2affa-251">讓我們現在實作的 「 索引 」 檢視範本 – 將會產生未來 Dinners 的清單。</span><span class="sxs-lookup"><span data-stu-id="2affa-251">Let's now implement the "Index" view template – which will generate a listing of upcoming Dinners.</span></span> <span data-ttu-id="2affa-252">待辦事項此我們會在索引動作方法中，我們文字游標的位置，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）。</span><span class="sxs-lookup"><span data-stu-id="2affa-252">To-do this we'll position our text cursor within the Index action method, and then right click and choose the "Add View" menu command (or press Ctrl-M, Ctrl-V).</span></span>

<span data-ttu-id="2affa-253">在 [新增檢視] 對話方塊中我們將保留檢視範本名為"Index"，並選取 [建立強型別檢視表] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2affa-253">Within the "Add View" dialog we'll keep the view template named "Index" and select the "Create a strongly-typed view" checkbox.</span></span> <span data-ttu-id="2affa-254">此時，我們將選擇要自動產生的 「 清單 」 檢視範本，並選取"NerdDinner.Models.Dinner"做為模型類型傳遞至檢視 (這因為我們已指出，我們會建立 scaffold 會導致 [新增檢視] 對話方塊，假設我們是一個 「 清單 」傳遞 Dinner 物件序列從 Controller 檢視）：</span><span class="sxs-lookup"><span data-stu-id="2affa-254">This time we will choose to automatically generate a "List" view template, and select "NerdDinner.Models.Dinner" as the model type passed to the view (which because we have indicated we are creating a "List" scaffold will cause the Add View dialog to assume we are passing a sequence of Dinner objects from our Controller to the View):</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

<span data-ttu-id="2affa-255">當我們按一下 [新增] 按鈕時，Visual Studio 將我們"\Views\Dinners"目錄中就讓我們建立新的"Index.aspx 」 檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="2affa-255">When we click the "Add" button, Visual Studio will create a new "Index.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="2affa-256">它會 「 scaffold"初始實作中提供 Dinners 我們傳遞至檢視的 HTML 資料表清單。</span><span class="sxs-lookup"><span data-stu-id="2affa-256">It will "scaffold" an initial implementation within it that provides an HTML table listing of the Dinners we pass to the view.</span></span>

<span data-ttu-id="2affa-257">當我們執行應用程式及存取 *"/ Dinners /"* URL 將會呈現 dinners 的清單，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="2affa-257">When we run the application and access the *"/Dinners/"* URL it will render our list of dinners like so:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

<span data-ttu-id="2affa-258">上述表格方案提供我們 Dinner 資料 – 不相當是我們所要面對 Dinner 清單我們消費者類似格線版面配置。</span><span class="sxs-lookup"><span data-stu-id="2affa-258">The table solution above gives us a grid-like layout of our Dinner data – which isn't quite what we want for our consumer facing Dinner listing.</span></span> <span data-ttu-id="2affa-259">我們可以更新 Index.aspx 檢視範本，然後修改它來列出較少的資料行的資料，並使用&lt;ul&gt;轉譯它們，而不是資料表，使用下列程式碼的項目：</span><span class="sxs-lookup"><span data-stu-id="2affa-259">We can update the Index.aspx view template and modify it to list fewer columns of data, and use a &lt;ul&gt; element to render them instead of a table using the code below:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

<span data-ttu-id="2affa-260">因為我們會在我們的模型執行迴圈每個晚餐，我們使用上述的 foreach 陳述式內的"var"關鍵字。</span><span class="sxs-lookup"><span data-stu-id="2affa-260">We are using the "var" keyword within the above foreach statement as we loop over each dinner in our Model.</span></span> <span data-ttu-id="2affa-261">這些熟悉 C# 3.0 可能會考慮，使用"var"表示 dinner 物件是晚期繫結。</span><span class="sxs-lookup"><span data-stu-id="2affa-261">Those unfamiliar with C# 3.0 might think that using "var" means that the dinner object is late-bound.</span></span> <span data-ttu-id="2affa-262">而是表示編譯器使用型別-推斷針對強型別"Model"屬性 (類型"IEnumerable&lt;Dinner&gt;") 和編譯為 Dinner 類型 – 這表示我們取得完整的本機 「 dinner"變數intellisense 和編譯時間檢查它在程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="2affa-262">It instead means that the compiler is using type-inference against the strongly typed "Model" property (which is of type "IEnumerable&lt;Dinner&gt;") and compiling the local "dinner" variable as a Dinner type – which means we get full intellisense and compile-time checking for it within code blocks:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

<span data-ttu-id="2affa-263">當我們點擊 重新整理 */Dinners*我們更新的檢視現在看起來像下面我們瀏覽器中的 URL:</span><span class="sxs-lookup"><span data-stu-id="2affa-263">When we hit refresh on the */Dinners* URL in our browser our updated view now looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

<span data-ttu-id="2affa-264">這會尋找比較好，但尚未完全有。</span><span class="sxs-lookup"><span data-stu-id="2affa-264">This is looking better – but isn't entirely there yet.</span></span> <span data-ttu-id="2affa-265">我們最後一個步驟是讓使用者可以按一下清單中的個別 Dinners 和其相關詳細資料請參閱。</span><span class="sxs-lookup"><span data-stu-id="2affa-265">Our last step is to enable end-users to click individual Dinners in the list and see details about them.</span></span> <span data-ttu-id="2affa-266">我們將會轉譯 HTML 超連結項目連結至詳細資料動作方法，在我們 DinnersController 來實作。</span><span class="sxs-lookup"><span data-stu-id="2affa-266">We'll implement this by rendering HTML hyperlink elements that link to the Details action method on our DinnersController.</span></span>

<span data-ttu-id="2affa-267">我們可以在其中一種我們索引檢視表中產生這些超連結。</span><span class="sxs-lookup"><span data-stu-id="2affa-267">We can generate these hyperlinks within our Index view in one of two ways.</span></span> <span data-ttu-id="2affa-268">第一個方法是以手動方式建立 HTML &lt;&gt;項目類似下面的其中我們內嵌&lt;%%&gt;中封鎖了&lt;&gt; HTML 項目：</span><span class="sxs-lookup"><span data-stu-id="2affa-268">The first is to manually create HTML &lt;a&gt; elements like below, where we embed &lt;% %&gt; blocks within the &lt;a&gt; HTML element:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

<span data-ttu-id="2affa-269">我們可以使用的替代方式是利用內建的 「 Html.ActionLink()"helper 方法支援以程式設計方式建立 HTML 的 ASP.NET MVC 中&lt;&gt;連結至其他動作方法的項目控制站：</span><span class="sxs-lookup"><span data-stu-id="2affa-269">An alternative approach we can use is to take advantage of the built-in "Html.ActionLink()" helper method within ASP.NET MVC that supports programmatically creating an HTML &lt;a&gt; element that links to another action method on a Controller:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

<span data-ttu-id="2affa-270">Html.ActionLink() helper 方法的第一個參數是要顯示的連結文字 （在此情況下標題的 dinner），第二個參數是我們想要產生的連結 （在此情況下的詳細資料的方法），控制器動作名稱和第三個參數一組要傳送 （實作為具有屬性名稱/值的匿名類型） 之動作的參數。</span><span class="sxs-lookup"><span data-stu-id="2affa-270">The first parameter to the Html.ActionLink() helper method is the link-text to display (in this case the title of the dinner), the second parameter is the Controller action name we want to generate the link to (in this case the Details method), and the third parameter is a set of parameters to send to the action (implemented as an anonymous type with property name/values).</span></span> <span data-ttu-id="2affa-271">我們將在此情況下指定的 dinner 我們想要連結至，而且因為預設的 URL 路由規則在 ASP.NET MVC 中的 「 識別碼 」 參數"{Controller} / {Action} / {id}"Html.ActionLink() helper 方法會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2affa-271">In this case we are specifying the "id" parameter of the dinner we want to link to, and because the default URL routing rule in ASP.NET MVC is "{Controller}/{Action}/{id}" the Html.ActionLink() helper method will generate the following output:</span></span>

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

<span data-ttu-id="2affa-272">我們 Index.aspx 檢視我們將使用 Html.ActionLink() helper 方法的方法以及每個 dinner 清單連結至適當的詳細資料的 URL 中：</span><span class="sxs-lookup"><span data-stu-id="2affa-272">For our Index.aspx view we'll use the Html.ActionLink() helper method approach and have each dinner in the list link to the appropriate details URL:</span></span>

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

<span data-ttu-id="2affa-273">現在我們叫用時和 */Dinners* dinner 清單看起來像下列的 URL:</span><span class="sxs-lookup"><span data-stu-id="2affa-273">And now when we hit the */Dinners* URL our dinner list looks like below:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

<span data-ttu-id="2affa-274">當我們按一下任一清單中 Dinners 我們會瀏覽以查看其相關詳細資料：</span><span class="sxs-lookup"><span data-stu-id="2affa-274">When we click any of the Dinners in the list we'll navigate to see details about it:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a><span data-ttu-id="2affa-275">命名慣例為基礎和 \Views 目錄結構</span><span class="sxs-lookup"><span data-stu-id="2affa-275">Convention-based naming and the \Views directory structure</span></span>

<span data-ttu-id="2affa-276">預設的 ASP.NET MVC 應用程式使用以慣例為基礎的目錄解析檢視範本時，命名結構。</span><span class="sxs-lookup"><span data-stu-id="2affa-276">ASP.NET MVC applications by default use a convention-based directory naming structure when resolving view templates.</span></span> <span data-ttu-id="2affa-277">這可讓開發人員不必完整限定的位置路徑參考從控制器類別中的檢視時。</span><span class="sxs-lookup"><span data-stu-id="2affa-277">This allows developers to avoid having to fully-qualify a location path when referencing views from within a Controller class.</span></span> <span data-ttu-id="2affa-278">根據預設，ASP.NET MVC 會尋找檢視的範本檔案內 * \Views\[ControllerName]\*目錄底下的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2affa-278">By default ASP.NET MVC will look for the view template file within the *\Views\[ControllerName]\* directory underneath the application.</span></span>

<span data-ttu-id="2affa-279">比方說，我們已使用的 DinnersController 類別 – 明確參考三個檢視的範本: 「 索引 」、 「 詳細資料 」 和"NotFound"。</span><span class="sxs-lookup"><span data-stu-id="2affa-279">For example, we've been working on the DinnersController class – which explicitly references three view templates: "Index", "Details" and "NotFound".</span></span> <span data-ttu-id="2affa-280">ASP.NET MVC 會依預設尋找這些檢視內*\Views\Dinners*我們的應用程式根目錄底下的目錄：</span><span class="sxs-lookup"><span data-stu-id="2affa-280">ASP.NET MVC will by default look for these views within the *\Views\Dinners* directory underneath our application root directory:</span></span>

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

<span data-ttu-id="2affa-281">請注意上述方式沒有目前專案中的三個控制器類別 (DinnersController、 HomeController 和 AccountController – 最後兩個已加入依預設，當我們建立專案時)，而且有三個子目錄 （一個用於每個控制站） \Views 目錄內。</span><span class="sxs-lookup"><span data-stu-id="2affa-281">Notice above how there are currently three controller classes within the project (DinnersController, HomeController and AccountController – the last two were added by default when we created the project), and there are three sub-directories (one for each controller) within the \Views directory.</span></span>

<span data-ttu-id="2affa-282">從首頁和帳戶控制器所參考的檢視將會自動解除其檢視範本，從個別*\Views\Home*和*\Views\Account*目錄。</span><span class="sxs-lookup"><span data-stu-id="2affa-282">Views referenced from the Home and Accounts controllers will automatically resolve their view templates from the respective *\Views\Home* and *\Views\Account* directories.</span></span> <span data-ttu-id="2affa-283">*\Views\Shared*子目錄提供方法來儲存跨多個應用程式中的控制器會重複使用的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="2affa-283">The *\Views\Shared* sub-directory provides a way to store view templates that are re-used across multiple controllers within the application.</span></span> <span data-ttu-id="2affa-284">當 ASP.NET MVC 會嘗試解析檢視範本時，它會先檢查，內*\Views\[控制站]* 特定目錄中，如果找不到檢視表範本那里它將會尋找內*\Views\共用*目錄。</span><span class="sxs-lookup"><span data-stu-id="2affa-284">When ASP.NET MVC attempts to resolve a view template, it will first check within the *\Views\[Controller]* specific directory, and if it can't find the view template there it will look within the *\Views\Shared* directory.</span></span>

<span data-ttu-id="2affa-285">命名個別檢視範本時，若要將共用相同的名稱做為動作方法，導致呈現檢視範本是建議的指引。</span><span class="sxs-lookup"><span data-stu-id="2affa-285">When it comes to naming individual view templates, the recommended guidance is to have the view template share the same name as the action method that caused it to render.</span></span> <span data-ttu-id="2affa-286">例如，上面我們 「 索引 」 動作的方法呈現檢視結果，使用 「 索引 」 檢視和 [詳細資料] 動作方法使用 [詳細資料] 檢視來呈現其結果。</span><span class="sxs-lookup"><span data-stu-id="2affa-286">For example, above our "Index" action method is using the "Index" view to render the view result, and the "Details" action method is using the "Details" view to render its results.</span></span> <span data-ttu-id="2affa-287">這可讓您輕鬆快速地查看哪一個範本是與每個動作相關聯。</span><span class="sxs-lookup"><span data-stu-id="2affa-287">This makes it easy to quickly see which template is associated with each action.</span></span>

<span data-ttu-id="2affa-288">若要檢視範本有相同名稱做為控制器上叫用動作方法時明確指定檢視表範本名稱不需要開發人員。</span><span class="sxs-lookup"><span data-stu-id="2affa-288">Developers do not need to explicitly specify the view template name when the view template has the same name as the action method being invoked on the controller.</span></span> <span data-ttu-id="2affa-289">我們可以改為只傳遞模型物件 」 View()"helper 方法 （而不指定檢視表名稱），和 ASP.NET MVC 會自動推斷我們想要使用*\Views\[ControllerName]\[ActionName]* 檢視範本來對應它的磁碟上。</span><span class="sxs-lookup"><span data-stu-id="2affa-289">We can instead just pass the model object to the "View()" helper method (without specifying the view name), and ASP.NET MVC will automatically infer that we want to use the *\Views\[ControllerName]\[ActionName]* view template on disk to render it.</span></span>

<span data-ttu-id="2affa-290">這可讓我們稍有清除控制器的程式碼，並避免重複兩次中的程式碼的名稱：</span><span class="sxs-lookup"><span data-stu-id="2affa-290">This allows us to clean up our controller code a little, and avoid duplicating the name twice in our code:</span></span>

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

<span data-ttu-id="2affa-291">上述程式碼會實作 nice Dinner 清單/詳細資料所需的所有站台的經驗。</span><span class="sxs-lookup"><span data-stu-id="2affa-291">The above code is all that is needed to implement a nice Dinner listing/details experience for the site.</span></span>

#### <a name="next-step"></a><span data-ttu-id="2affa-292">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="2affa-292">Next Step</span></span>

<span data-ttu-id="2affa-293">我們現在有 nice Dinner，瀏覽建置體驗。</span><span class="sxs-lookup"><span data-stu-id="2affa-293">We now have a nice Dinner browsing experience built.</span></span>

<span data-ttu-id="2affa-294">我們現在啟用編輯支援的 CRUD （建立、 讀取、 更新、 刪除） 資料格式。</span><span class="sxs-lookup"><span data-stu-id="2affa-294">Let's now enable CRUD (Create, Read, Update, Delete) data form editing support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2affa-295">[上一頁](build-a-model-with-business-rule-validations.md)
> [下一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)</span><span class="sxs-lookup"><span data-stu-id="2affa-295">[Previous](build-a-model-with-business-rule-validations.md)
[Next](provide-crud-create-read-update-delete-data-form-entry-support.md)</span></span>
