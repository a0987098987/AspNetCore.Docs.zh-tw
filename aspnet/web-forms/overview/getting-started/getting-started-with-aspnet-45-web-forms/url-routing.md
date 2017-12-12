---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: "URL 路由 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 279617e4ebb475d935c0d1e01e08a3a2def0f9e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="url-routing"></a><span data-ttu-id="39ca6-103">URL 路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-103">URL Routing</span></span>
====================
<span data-ttu-id="39ca6-104">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="39ca6-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="39ca6-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="39ca6-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="39ca6-106">此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="39ca6-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="39ca6-107">Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。</span><span class="sxs-lookup"><span data-stu-id="39ca6-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="39ca6-108">在本教學課程中，您將修改 Wingtip Toys 範例應用程式支援 URL 路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-108">In this tutorial, you will modify the Wingtip Toys sample application to support URL routing.</span></span> <span data-ttu-id="39ca6-109">路由可讓您的 web 應用程式使用好記，請記住，變得更容易而且更支援的搜尋引擎的 Url。</span><span class="sxs-lookup"><span data-stu-id="39ca6-109">Routing enables your web application to use URLs that are friendly, easier to remember, and better supported by search engines.</span></span> <span data-ttu-id="39ca6-110">本教學課程是根據上一個教學課程 [成員資格和系統管理]，並是 Wingtip Toys 教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="39ca6-110">This tutorial builds on the previous tutorial "Membership and Administration" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="39ca6-111">您將學習：</span><span class="sxs-lookup"><span data-stu-id="39ca6-111">What you'll learn:</span></span>

- <span data-ttu-id="39ca6-112">如何註冊 ASP.NET Web Form 應用程式的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-112">How to register routes for an ASP.NET Web Forms application.</span></span>
- <span data-ttu-id="39ca6-113">如何將路由新增至網頁。</span><span class="sxs-lookup"><span data-stu-id="39ca6-113">How to add routes to a web page.</span></span>
- <span data-ttu-id="39ca6-114">如何從某個資料庫支援路由選取資料。</span><span class="sxs-lookup"><span data-stu-id="39ca6-114">How to select data from a database to support routes.</span></span>

## <a name="aspnet-routing-overview"></a><span data-ttu-id="39ca6-115">ASP.NET 路由概觀</span><span class="sxs-lookup"><span data-stu-id="39ca6-115">ASP.NET Routing Overview</span></span>

<span data-ttu-id="39ca6-116">URL 路由可讓您設定應用程式以接受要求不會對應到實體檔案的 Url。</span><span class="sxs-lookup"><span data-stu-id="39ca6-116">URL routing allows you to configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="39ca6-117">要求 URL 是只要將使用者輸入他們的瀏覽器，在網站上尋找頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="39ca6-117">A request URL is simply the URL a user enters into their browser to find a page on your web site.</span></span> <span data-ttu-id="39ca6-118">若要定義的 Url，在語意上意義給使用者，且搜尋引擎最佳化 (SEO) 可協助使用路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-118">You use routing to define URLs that are semantically meaningful to users and that can help with search-engine optimization (SEO).</span></span>

<span data-ttu-id="39ca6-119">根據預設，Web Form 範本包括[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)。</span><span class="sxs-lookup"><span data-stu-id="39ca6-119">By default, the Web Forms template includes [ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/).</span></span> <span data-ttu-id="39ca6-120">大部分的基本的路由工作將會透過使用實作*Friendly URLs*。</span><span class="sxs-lookup"><span data-stu-id="39ca6-120">Much of the basic routing work will be implemented by using *Friendly URLs*.</span></span> <span data-ttu-id="39ca6-121">不過，在本教學課程，您會加入自訂路由的功能。</span><span class="sxs-lookup"><span data-stu-id="39ca6-121">However, in this tutorial you will add customized routing capabilities.</span></span>

<span data-ttu-id="39ca6-122">在自訂之前 URL 路由，Wingtip Toys 範例應用程式可以連結至產品，使用下列 URL:</span><span class="sxs-lookup"><span data-stu-id="39ca6-122">Before customizing URL routing, the Wingtip Toys sample application can link to a product using the following URL:</span></span>

`https://localhost:44300/ProductDetails.aspx?productID=2`

<span data-ttu-id="39ca6-123">藉由自訂 URL 路由，Wingtip Toys 範例應用程式將會連結至相同的產品使用方便讀取 URL:</span><span class="sxs-lookup"><span data-stu-id="39ca6-123">By customizing URL routing, the Wingtip Toys sample application will link to the same product using an easier to read URL:</span></span>

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a><span data-ttu-id="39ca6-124">路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-124">Routes</span></span>

<span data-ttu-id="39ca6-125">路由是對應至處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-125">A route is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="39ca6-126">此處理常式可以是實體檔案，例如 Web Form 應用程式中的.aspx 檔案。</span><span class="sxs-lookup"><span data-stu-id="39ca6-126">The handler can be a physical file, such as an .aspx file in a Web Forms application.</span></span> <span data-ttu-id="39ca6-127">處理常式也可以是處理要求的類別。</span><span class="sxs-lookup"><span data-stu-id="39ca6-127">A handler can also be a class that processes the request.</span></span> <span data-ttu-id="39ca6-128">若要定義路由，您可以建立路由類別的執行個體所指定的 URL 模式、 處理常式，以及選擇性地路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="39ca6-128">To define a route, you create an instance of the Route class by specifying the URL pattern, the handler, and optionally a name for the route.</span></span>

<span data-ttu-id="39ca6-129">將路由加入至應用程式的加`Route`靜態物件`Routes`屬性`RouteTable`類別。</span><span class="sxs-lookup"><span data-stu-id="39ca6-129">You add the route to the application by adding the `Route` object to the static `Routes` property of the `RouteTable` class.</span></span> <span data-ttu-id="39ca6-130">路由屬性是`RouteCollection`儲存應用程式的所有路由的物件。</span><span class="sxs-lookup"><span data-stu-id="39ca6-130">The Routes property is a `RouteCollection` object that stores all the routes for the application.</span></span>

### <a name="url-patterns"></a><span data-ttu-id="39ca6-131">URL 模式</span><span class="sxs-lookup"><span data-stu-id="39ca6-131">URL Patterns</span></span>

<span data-ttu-id="39ca6-132">URL 模式可以包含常值和變數的預留位置 （稱為 URL 參數）。</span><span class="sxs-lookup"><span data-stu-id="39ca6-132">A URL pattern can contain literal values and variable placeholders (referred to as URL parameters).</span></span> <span data-ttu-id="39ca6-133">常值和預留位置位於其以斜線分隔 URL 區段 (`/`) 字元。</span><span class="sxs-lookup"><span data-stu-id="39ca6-133">The literals and placeholders are located in segments of the URL which are delimited by the slash (`/`) character.</span></span>

<span data-ttu-id="39ca6-134">您 web 應用程式進行要求時，URL 會剖析為區段和預留位置，和變數的值提供給要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-134">When a request to your web application is made, the URL is parsed into segments and placeholders, and the variable values are provided to the request handler.</span></span> <span data-ttu-id="39ca6-135">此程序是在剖析查詢字串中的資料和傳遞給要求處理常式的方式類似。</span><span class="sxs-lookup"><span data-stu-id="39ca6-135">This process is similar to the way the data in a query string is parsed and passed to the request handler.</span></span> <span data-ttu-id="39ca6-136">在這兩種情況下，為 URL 中包含變數的資訊，並將其傳遞給索引鍵-值組表單中的處理常式中。</span><span class="sxs-lookup"><span data-stu-id="39ca6-136">In both cases, variable information is included in the URL and passed to the handler in the form of key-value pairs.</span></span> <span data-ttu-id="39ca6-137">查詢字串索引鍵和值是在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="39ca6-137">For query strings, both the keys and the values are in the URL.</span></span> <span data-ttu-id="39ca6-138">路由的索引鍵中的 URL 模式中，定義的預留位置名稱，而只有值在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="39ca6-138">For routes, the keys are the placeholder names defined in the URL pattern, and only the values are in the URL.</span></span>

<span data-ttu-id="39ca6-139">在 URL 模式中，您必須定義預留位置放在大括號 (`{`和`}`)。</span><span class="sxs-lookup"><span data-stu-id="39ca6-139">In a URL pattern, you define placeholders by enclosing them in braces ( `{` and `}` ).</span></span> <span data-ttu-id="39ca6-140">您可以定義一個以上的預留位置的區段中，但必須以常值分隔預留位置。</span><span class="sxs-lookup"><span data-stu-id="39ca6-140">You can define more than one placeholder in a segment, but the placeholders must be separated by a literal value.</span></span> <span data-ttu-id="39ca6-141">例如，`{language}-{country}/{action}`是有效的路由模式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-141">For example, `{language}-{country}/{action}` is a valid route pattern.</span></span> <span data-ttu-id="39ca6-142">不過，`{language}{country}/{action}`不是有效的模式，因為沒有任何常值或預留位置之間的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="39ca6-142">However, `{language}{country}/{action}` is not a valid pattern, because there is no literal value or delimiter between the placeholders.</span></span> <span data-ttu-id="39ca6-143">因此，路由無法判斷分開值語言預留位置值的國家/地區預留位置的位置。</span><span class="sxs-lookup"><span data-stu-id="39ca6-143">Therefore, routing cannot determine where to separate the value for the language placeholder from the value for the country placeholder.</span></span>

### <a name="mapping-and-registering-routes"></a><span data-ttu-id="39ca6-144">對應和註冊路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-144">Mapping and Registering Routes</span></span>

<span data-ttu-id="39ca6-145">您可以包含路由，以便 Wingtip Toys 範例應用程式的頁面之前，您必須在應用程式啟動時註冊路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-145">Before you can include routes to pages of the Wingtip Toys sample application, you must register the routes when the application starts.</span></span> <span data-ttu-id="39ca6-146">若要註冊的路由，您將修改`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-146">To register the routes, you will modify the `Application_Start` event handler.</span></span>

1. <span data-ttu-id="39ca6-147">在**方案總管 中**的 Visual Studio 中，尋找並開啟*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="39ca6-147">In **Solution Explorer**of Visual Studio, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="39ca6-148">加入程式碼中以黃色反白顯示*Global.asax.cs*檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39ca6-148">Add the code highlighted in yellow to the *Global.asax.cs* file as follows:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

<span data-ttu-id="39ca6-149">當 Wingtip Toys 範例應用程式啟動時，它會呼叫`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-149">When the Wingtip Toys sample application starts, it calls the `Application_Start` event handler.</span></span> <span data-ttu-id="39ca6-150">這個事件處理常式中，結尾`RegisterCustomRoutes`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="39ca6-150">At the end of this event handler, the `RegisterCustomRoutes` method is called.</span></span> <span data-ttu-id="39ca6-151">`RegisterCustomRoutes`方法呼叫會將每個路由`MapPageRoute`方法`RouteCollection`物件。</span><span class="sxs-lookup"><span data-stu-id="39ca6-151">The `RegisterCustomRoutes` method adds each route by calling the `MapPageRoute` method of the `RouteCollection` object.</span></span> <span data-ttu-id="39ca6-152">路由會定義使用路由名稱，路由 URL 及實體的 URL。</span><span class="sxs-lookup"><span data-stu-id="39ca6-152">Routes are defined using a route name, a route URL and a physical URL.</span></span>

<span data-ttu-id="39ca6-153">第一個參數 ("`ProductsByCategoryRoute`") 的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="39ca6-153">The first parameter ("`ProductsByCategoryRoute`") is the route name.</span></span> <span data-ttu-id="39ca6-154">它用來在需要時呼叫的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-154">It is used to call the route when it is needed.</span></span> <span data-ttu-id="39ca6-155">第二個參數 ("`Category/{categoryName}`」) 會定義可以是動態的 URL 為基礎程式碼的好記取代。</span><span class="sxs-lookup"><span data-stu-id="39ca6-155">The second parameter ("`Category/{categoryName}`") defines the friendly replacement URL that can be dynamic based on code.</span></span> <span data-ttu-id="39ca6-156">會擴展資料控制項產生根據資料的連結時，您可以使用此路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-156">You use this route when you are populating a data control with links that are generated based on data.</span></span> <span data-ttu-id="39ca6-157">路由會顯示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39ca6-157">A route is shown as follows:</span></span>

[!code-csharp[Main](url-routing/samples/sample2.cs)]

<span data-ttu-id="39ca6-158">第二個參數的路由包含大括號所指定的動態值 (`{ }`)。</span><span class="sxs-lookup"><span data-stu-id="39ca6-158">The second parameter of the route includes a dynamic value specified by braces (`{ }`).</span></span> <span data-ttu-id="39ca6-159">在此情況下，`categoryName`是將用來判斷適當的路由路徑的變數。</span><span class="sxs-lookup"><span data-stu-id="39ca6-159">In this case, the `categoryName` is a variable that will be used to determine the proper routing path.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="39ca6-160">**Optional**</span><span class="sxs-lookup"><span data-stu-id="39ca6-160">**Optional**</span></span>
> 
> <span data-ttu-id="39ca6-161">您可能會發現它更輕鬆地管理您的程式碼移動`RegisterCustomRoutes`的不同類別的方法。</span><span class="sxs-lookup"><span data-stu-id="39ca6-161">You might find it easier to manage your code by moving the `RegisterCustomRoutes` method to a separate class.</span></span> <span data-ttu-id="39ca6-162">在*邏輯*資料夾中，建立個別`RouteActions`類別。</span><span class="sxs-lookup"><span data-stu-id="39ca6-162">In the *Logic* folder, create a separate `RouteActions` class.</span></span> <span data-ttu-id="39ca6-163">移動上述`RegisterCustomRoutes`方法從*Global.asax.cs*檔案到新`RoutesActions`類別。</span><span class="sxs-lookup"><span data-stu-id="39ca6-163">Move the above `RegisterCustomRoutes` method from the *Global.asax.cs* file into the new `RoutesActions` class.</span></span> <span data-ttu-id="39ca6-164">使用`RoleActions`類別和`createAdmin`方法做為範例如何呼叫`RegisterCustomRoutes`方法從*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="39ca6-164">Use the `RoleActions` class and the `createAdmin` method as an example of how to call the `RegisterCustomRoutes` method from the *Global.asax.cs* file.</span></span>


<span data-ttu-id="39ca6-165">您也可能已經發現`RegisterRoutes`方法呼叫使用`RouteConfig`物件的開頭`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-165">You may also have noticed the `RegisterRoutes` method call using the `RouteConfig` object at the beginning of the `Application_Start` event handler.</span></span> <span data-ttu-id="39ca6-166">進行此呼叫，以實作預設路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-166">This call is made to implement default routing.</span></span> <span data-ttu-id="39ca6-167">當您建立使用 Visual Studio Web 表單範本的應用程式時，它是作為預設程式碼。</span><span class="sxs-lookup"><span data-stu-id="39ca6-167">It was included as default code when you created the application using Visual Studio's Web Forms template.</span></span>

## <a name="retrieving-and-using-route-data"></a><span data-ttu-id="39ca6-168">擷取和使用資料路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-168">Retrieving and Using Route Data</span></span>

<span data-ttu-id="39ca6-169">如上面所述，您可以定義路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-169">As mentioned above, routes can be defined.</span></span> <span data-ttu-id="39ca6-170">您加入的程式碼`Application_Start`中的事件處理常式*Global.asax.cs*檔案載入可定義的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-170">The code that you added to the `Application_Start` event handler in the *Global.asax.cs* file loads the definable routes.</span></span>

### <a name="setting-routes"></a><span data-ttu-id="39ca6-171">設定路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-171">Setting Routes</span></span>

<span data-ttu-id="39ca6-172">路由需要您將新增額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="39ca6-172">Routes require you to add additional code.</span></span> <span data-ttu-id="39ca6-173">在此教學課程中，您將使用模型繫結來擷取`RouteValueDictionary`產生的路由使用資料控制項中的資料時所使用的物件。</span><span class="sxs-lookup"><span data-stu-id="39ca6-173">In this tutorial, you will use model binding to retrieve a `RouteValueDictionary` object that is used when generating the routes using data from a data control.</span></span> <span data-ttu-id="39ca6-174">`RouteValueDictionary`物件會包含屬於特定類別的產品的產品名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="39ca6-174">The `RouteValueDictionary` object will contain a list of product names that belong to a specific category of products.</span></span> <span data-ttu-id="39ca6-175">根據資料和路由每項產品會建立連結。</span><span class="sxs-lookup"><span data-stu-id="39ca6-175">A link is created for each product based on the data and route.</span></span>

#### <a name="enable-routes-for-categories-and-products"></a><span data-ttu-id="39ca6-176">分類和產品啟用的路由</span><span class="sxs-lookup"><span data-stu-id="39ca6-176">Enable Routes for Categories and Products</span></span>

<span data-ttu-id="39ca6-177">接下來，您要更新應用程式使用`ProductsByCategoryRoute`來判斷要為每個產品類別目錄連結包含正確的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-177">Next, you'll update the application to use the `ProductsByCategoryRoute` to determine the correct route to include for each product category link.</span></span> <span data-ttu-id="39ca6-178">您還會更新*ProductList.aspx*加入每個產品的路由的連結的頁面。</span><span class="sxs-lookup"><span data-stu-id="39ca6-178">You'll also update the *ProductList.aspx* page to include a routed link for each product.</span></span> <span data-ttu-id="39ca6-179">連結將會顯示之前變更，但是連結現在會使用 URL 路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-179">The links will be displayed as they were before the change, however the links will now use URL routing.</span></span>

1. <span data-ttu-id="39ca6-180">在**方案總管 中**，開啟*Site.Master*頁面上，如果它尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="39ca6-180">In **Solution Explorer**, open the *Site.Master* page if it is not already open.</span></span>
2. <span data-ttu-id="39ca6-181">更新**ListView**控制項，名為"`categoryList`」 以黃色反白顯示的變更，所以標記會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39ca6-181">Update the **ListView** control named "`categoryList`" with the changes highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. <span data-ttu-id="39ca6-182">在**方案總管 中**，開啟*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="39ca6-182">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
4. <span data-ttu-id="39ca6-183">更新`ItemTemplate`元素*ProductList.aspx*頁面更新中反白顯示黃色，讓標記出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39ca6-183">Update the `ItemTemplate` element of the *ProductList.aspx* page with the updates highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. <span data-ttu-id="39ca6-184">開啟 程式碼後置的*ProductList.aspx.cs*並加入下列命名空間為中反白黃色：</span><span class="sxs-lookup"><span data-stu-id="39ca6-184">Open the code-behind of *ProductList.aspx.cs* and add the following namespace as highlighted in yellow:</span></span>  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. <span data-ttu-id="39ca6-185">取代`GetProducts`方法的程式碼後置 (*ProductList.aspx.cs*) 取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="39ca6-185">Replace the `GetProducts` method of the code-behind (*ProductList.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a><span data-ttu-id="39ca6-186">加入程式碼的產品詳細資料</span><span class="sxs-lookup"><span data-stu-id="39ca6-186">Add Code for Product Details</span></span>

<span data-ttu-id="39ca6-187">現在，更新程式碼後置 (*ProductDetails.aspx.cs*) 的*ProductDetails.aspx*頁面，即可使用路由資料。</span><span class="sxs-lookup"><span data-stu-id="39ca6-187">Now, update the code-behind (*ProductDetails.aspx.cs*) for the *ProductDetails.aspx* page to use route data.</span></span> <span data-ttu-id="39ca6-188">請注意，新`GetProduct`方法也可接受的情況在使用者具有設為書籤的連結，會使用較舊的非易記、 非路由 URL 的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="39ca6-188">Notice that the new `GetProduct` method also accepts a query string value for the case where the user has a link bookmarked that uses the older non-friendly, non-routed URL.</span></span>

1. <span data-ttu-id="39ca6-189">取代`GetProduct`方法的程式碼後置 (*ProductDetails.aspx.cs*) 取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="39ca6-189">Replace the `GetProduct` method of the code-behind (*ProductDetails.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a><span data-ttu-id="39ca6-190">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="39ca6-190">Running the Application</span></span>

<span data-ttu-id="39ca6-191">您可以執行應用程式現在以查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-191">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="39ca6-192">按**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="39ca6-192">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="39ca6-193">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="39ca6-193">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="39ca6-194">按一下**產品**在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="39ca6-194">Click the **Products** link at the top of the page.</span></span>  
 <span data-ttu-id="39ca6-195">所有產品會都顯示在*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="39ca6-195">All products are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="39ca6-196">瀏覽器會顯示下列 URL （使用連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="39ca6-196">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/ProductList`
3. <span data-ttu-id="39ca6-197">接下來，按一下**汽車**靠近頁面頂端的類別目錄連結。</span><span class="sxs-lookup"><span data-stu-id="39ca6-197">Next, click the **Cars** category link near the top of the page.</span></span>  
 <span data-ttu-id="39ca6-198">只有汽車會顯示在*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="39ca6-198">Only cars are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="39ca6-199">瀏覽器會顯示下列 URL （使用連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="39ca6-199">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Category/Cars`
4. <span data-ttu-id="39ca6-200">按一下頁面列出的連結，包含名稱的第一個汽車 ("**可轉換汽車**") 以顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="39ca6-200">Click the link containing the name of the first car listed on the page ("**Convertible Car**") to display the product details.</span></span>  
 <span data-ttu-id="39ca6-201">瀏覽器會顯示下列 URL （使用連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="39ca6-201">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Product/Convertible%20Car`
5. <span data-ttu-id="39ca6-202">接下來，瀏覽器中輸入下列非路由 URL （使用連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="39ca6-202">Next, enter the following non-routed URL (using your port number) into the browser:</span></span>  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 <span data-ttu-id="39ca6-203">程式碼仍會辨識包含查詢字串，使用者有標示為書籤的連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="39ca6-203">The code still recognizes a URL that includes a query string, for the case where a user has a link bookmarked.</span></span>

## <a name="summary"></a><span data-ttu-id="39ca6-204">總結</span><span class="sxs-lookup"><span data-stu-id="39ca6-204">Summary</span></span>

<span data-ttu-id="39ca6-205">在本教學課程中，您已加入為分類和產品的路由。</span><span class="sxs-lookup"><span data-stu-id="39ca6-205">In this tutorial, you have added routes for categories and products.</span></span> <span data-ttu-id="39ca6-206">您已經學會如何整合路由，與使用模型繫結的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="39ca6-206">You have learned how routes can be integrated with data controls that use model binding.</span></span> <span data-ttu-id="39ca6-207">在下一個教學課程中，您將實作通用的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="39ca6-207">In the next tutorial, you will implement global error handling.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39ca6-208">其他資源</span><span class="sxs-lookup"><span data-stu-id="39ca6-208">Additional Resources</span></span>

[<span data-ttu-id="39ca6-209">ASP.NET 易記 Url</span><span class="sxs-lookup"><span data-stu-id="39ca6-209">ASP.NET Friendly URLs</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[<span data-ttu-id="39ca6-210">將具有成員資格、 OAuth、 和 SQL Database 的安全的 ASP.NET Web Form 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="39ca6-210">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="39ca6-211">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="39ca6-211">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
<span data-ttu-id="39ca6-212">[上一頁](membership-and-administration.md)
[下一頁](aspnet-error-handling.md)</span><span class="sxs-lookup"><span data-stu-id="39ca6-212">[Previous](membership-and-administration.md)
[Next](aspnet-error-handling.md)</span></span>
