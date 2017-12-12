---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: "ASP.NET MVC Routing 概觀 (VB) |Microsoft 文件"
author: StephenWalther
description: "在本教學課程中，作者： Stephen Walther 會顯示 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作的方式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="9968e-103">ASP.NET MVC Routing 概觀 (VB)</span><span class="sxs-lookup"><span data-stu-id="9968e-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="9968e-104">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9968e-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9968e-105">在本教學課程中，作者： Stephen Walther 會顯示 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作的方式。</span><span class="sxs-lookup"><span data-stu-id="9968e-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="9968e-106">在本教學課程中，您可以引入呼叫每個 ASP.NET MVC 應用程式的重要功能*ASP.NET 路由*。</span><span class="sxs-lookup"><span data-stu-id="9968e-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="9968e-107">ASP.NET 路由模組負責將連入的瀏覽器要求對應至特定 MVC 控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="9968e-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="9968e-108">本教學課程結束時，您將了解標準的路由表如何將要求對應到控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="9968e-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="9968e-109">使用預設路由表</span><span class="sxs-lookup"><span data-stu-id="9968e-109">Using the Default Route Table</span></span>

<span data-ttu-id="9968e-110">當您建立新的 ASP.NET MVC 應用程式時，應用程式被設定為使用 ASP.NET 路由。</span><span class="sxs-lookup"><span data-stu-id="9968e-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="9968e-111">ASP.NET 路由是在兩個地方的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="9968e-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="9968e-112">首先，ASP.NET 路由中已啟用您的應用程式的 Web 組態檔 （Web.config 檔案）。</span><span class="sxs-lookup"><span data-stu-id="9968e-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="9968e-113">有四個區段的組態檔中與路由相關： system.web.httpModules 區段、 system.web.httpHandlers 區段、 system.webserver.modules 區段中和 system.webserver.handlers > 一節。</span><span class="sxs-lookup"><span data-stu-id="9968e-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="9968e-114">請小心不要刪除這些區段，因為這些區段不路由將無法再運作。</span><span class="sxs-lookup"><span data-stu-id="9968e-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="9968e-115">第二個，而更重要的是，應用程式的 Global.asax 檔案中建立路由表。</span><span class="sxs-lookup"><span data-stu-id="9968e-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="9968e-116">Global.asax 檔案是特殊的檔案，其中包含 ASP.NET 應用程式生命週期事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="9968e-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="9968e-117">應用程式的啟動事件期間建立的路由表。</span><span class="sxs-lookup"><span data-stu-id="9968e-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="9968e-118">程式碼範例 1 中的檔案包含 ASP.NET MVC 應用程式的預設 Global.asax 檔。</span><span class="sxs-lookup"><span data-stu-id="9968e-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="9968e-119">**列出 1-Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="9968e-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="9968e-120">MVC 應用程式第一次啟動時，應用程式\_呼叫 start （） 方法。</span><span class="sxs-lookup"><span data-stu-id="9968e-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="9968e-121">這個方法，又呼叫 RegisterRoutes() 方法。</span><span class="sxs-lookup"><span data-stu-id="9968e-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="9968e-122">RegisterRoutes() 方法建立路由表。</span><span class="sxs-lookup"><span data-stu-id="9968e-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="9968e-123">預設路由表包含單一的路由 （名為預設值）。</span><span class="sxs-lookup"><span data-stu-id="9968e-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="9968e-124">預設路由將 URL 的第一個區段對應至控制器名稱、 控制器的動作，URL 的第二個區段和名為參數的第三個區段**識別碼**。</span><span class="sxs-lookup"><span data-stu-id="9968e-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="9968e-125">假設您在網頁瀏覽器的網址列輸入下列 URL:</span><span class="sxs-lookup"><span data-stu-id="9968e-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="9968e-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="9968e-126">/Home/Index/3</span></span>

<span data-ttu-id="9968e-127">預設路由會將這個 URL 對應到下列參數：</span><span class="sxs-lookup"><span data-stu-id="9968e-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="9968e-128">控制器 = Home</span><span class="sxs-lookup"><span data-stu-id="9968e-128">controller = Home</span></span>

- <span data-ttu-id="9968e-129">動作 = 索引</span><span class="sxs-lookup"><span data-stu-id="9968e-129">action = Index</span></span>

- <span data-ttu-id="9968e-130">識別碼 = 3</span><span class="sxs-lookup"><span data-stu-id="9968e-130">id = 3</span></span>

<span data-ttu-id="9968e-131">當您要求 URL /Home/索引/3 時，會執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9968e-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="9968e-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="9968e-132">HomeController.Index(3)</span></span>

<span data-ttu-id="9968e-133">預設路由包含所有的三個參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="9968e-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="9968e-134">如果您並未提供控制器、 控制器參數的預設值的值**首頁**。</span><span class="sxs-lookup"><span data-stu-id="9968e-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="9968e-135">如果您並未提供動作，action 參數預設為值**索引**。</span><span class="sxs-lookup"><span data-stu-id="9968e-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="9968e-136">最後，如果您未提供的識別碼，id 參數的預設值為空字串。</span><span class="sxs-lookup"><span data-stu-id="9968e-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="9968e-137">讓我們看看如何預設路由將 Url 對應至控制器動作的一些範例。</span><span class="sxs-lookup"><span data-stu-id="9968e-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="9968e-138">假設您的瀏覽器網址列輸入下列 URL:</span><span class="sxs-lookup"><span data-stu-id="9968e-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="9968e-139">/ 首頁</span><span class="sxs-lookup"><span data-stu-id="9968e-139">/Home</span></span>

<span data-ttu-id="9968e-140">由於預設路由參數預設值，輸入此 URL 會導致 HomeController 類別的 index （） 方法的清單 2 來呼叫。</span><span class="sxs-lookup"><span data-stu-id="9968e-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="9968e-141">**列出 2-HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="9968e-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="9968e-142">在列出 2 中，HomeController 類別包含名為 index （） 可接受名為識別碼的單一參數的方法URL /Home 會導致 index （） 方法呼叫的值做為 Id 參數的值執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="9968e-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="9968e-143">MVC 架構會叫用非同步控制器動作的方式，因為 URL /Home 也會比對中列出的 3 HomeController 類別的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="9968e-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="9968e-144">**列出 3-HomeController.vb （沒有參數的索引動作）**</span><span class="sxs-lookup"><span data-stu-id="9968e-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="9968e-145">範例 3 中的 index （） 方法不接受任何參數。</span><span class="sxs-lookup"><span data-stu-id="9968e-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="9968e-146">URL /Home 會導致這個 index （） 方法來呼叫。</span><span class="sxs-lookup"><span data-stu-id="9968e-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="9968e-147">URL /Home/索引/3 也會叫用這個方法 （識別碼會被忽略）。</span><span class="sxs-lookup"><span data-stu-id="9968e-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="9968e-148">URL /Home 也會比對中列出的 4 HomeController 類別的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="9968e-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="9968e-149">**列出 4-HomeController.vb （具有可為 null 的參數索引動作）**</span><span class="sxs-lookup"><span data-stu-id="9968e-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="9968e-150">在列出 4 中，index （） 方法會有一個整數參數。</span><span class="sxs-lookup"><span data-stu-id="9968e-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="9968e-151">因為參數是可為 null 的參數 （可以具有值執行任何動作），index （） 可以呼叫而不會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="9968e-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="9968e-152">最後，叫用 5 與 URL /Home 中的 index （） 方法會發生例外狀況，因為 Id 參數*不*可為 null 的參數。</span><span class="sxs-lookup"><span data-stu-id="9968e-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="9968e-153">如果您嘗試叫用 index （） 方法時您會取得在圖 1 顯示的錯誤。</span><span class="sxs-lookup"><span data-stu-id="9968e-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="9968e-154">**列出 5-HomeController.vb （使用 Id 參數的索引動作）**</span><span class="sxs-lookup"><span data-stu-id="9968e-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="9968e-155">[![叫用控制器動作，預期的參數值](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9968e-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="9968e-156">**圖 01**： 叫用控制器動作，預期的參數值 ([按一下以檢視完整大小的影像](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9968e-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="9968e-157">URL /Home/索引/3，相反地，具有索引控制器動作，列出 5 中正常運作。</span><span class="sxs-lookup"><span data-stu-id="9968e-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="9968e-158">要求 /Home/Index/3 使 index （） 方法用值 3 Id 參數來呼叫。</span><span class="sxs-lookup"><span data-stu-id="9968e-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="9968e-159">總結</span><span class="sxs-lookup"><span data-stu-id="9968e-159">Summary</span></span>

<span data-ttu-id="9968e-160">本教學課程的目標是為您提供 ASP.NET 路由的簡介。</span><span class="sxs-lookup"><span data-stu-id="9968e-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="9968e-161">我們會檢查您取得新的 ASP.NET MVC 應用程式的預設路由表。</span><span class="sxs-lookup"><span data-stu-id="9968e-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="9968e-162">您已學習如何預設路由將 Url 對應至控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="9968e-162">You learned how the default route maps URLs to controller actions.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9968e-163">[上一頁](creating-an-action-cs.md)
[下一頁](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9968e-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
