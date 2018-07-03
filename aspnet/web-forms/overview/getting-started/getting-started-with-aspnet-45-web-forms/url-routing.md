---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL 路由 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 556ef01304d0b5a3cca3606d71ef055ce4b2dc5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389146"
---
<a name="url-routing"></a><span data-ttu-id="0dd24-103">URL 路由</span><span class="sxs-lookup"><span data-stu-id="0dd24-103">URL Routing</span></span>
====================
<span data-ttu-id="0dd24-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="0dd24-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="0dd24-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="0dd24-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="0dd24-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0dd24-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="0dd24-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="0dd24-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="0dd24-108">在本教學課程中，您將修改 「 Wingtip Toys 範例應用程式支援 URL 路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-108">In this tutorial, you will modify the Wingtip Toys sample application to support URL routing.</span></span> <span data-ttu-id="0dd24-109">路由可讓您的 web 應用程式，以使用好記，請記住，變得更容易而且更支援的搜尋引擎的 Url。</span><span class="sxs-lookup"><span data-stu-id="0dd24-109">Routing enables your web application to use URLs that are friendly, easier to remember, and better supported by search engines.</span></span> <span data-ttu-id="0dd24-110">此教學課程的上一個教學課程 [成員資格和系統管理]，並且是 Wingtip Toys 教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="0dd24-110">This tutorial builds on the previous tutorial "Membership and Administration" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="0dd24-111">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="0dd24-111">What you'll learn:</span></span>

- <span data-ttu-id="0dd24-112">如何註冊 ASP.NET Web Forms 應用程式的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-112">How to register routes for an ASP.NET Web Forms application.</span></span>
- <span data-ttu-id="0dd24-113">如何將路由新增至網頁。</span><span class="sxs-lookup"><span data-stu-id="0dd24-113">How to add routes to a web page.</span></span>
- <span data-ttu-id="0dd24-114">如何從某個資料庫支援路由選取資料。</span><span class="sxs-lookup"><span data-stu-id="0dd24-114">How to select data from a database to support routes.</span></span>

## <a name="aspnet-routing-overview"></a><span data-ttu-id="0dd24-115">ASP.NET 路由概觀</span><span class="sxs-lookup"><span data-stu-id="0dd24-115">ASP.NET Routing Overview</span></span>

<span data-ttu-id="0dd24-116">URL 路由可讓您設定應用程式以接受要求不會對應至實體檔案的 Url。</span><span class="sxs-lookup"><span data-stu-id="0dd24-116">URL routing allows you to configure an application to accept request URLs that do not map to physical files.</span></span> <span data-ttu-id="0dd24-117">要求 URL 會是只要將使用者輸入他們的瀏覽器，在您的網站上尋找頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="0dd24-117">A request URL is simply the URL a user enters into their browser to find a page on your web site.</span></span> <span data-ttu-id="0dd24-118">您使用路由來定義使用者具有語意意義，而且可以幫助搜尋引擎最佳化 (SEO) 的 Url。</span><span class="sxs-lookup"><span data-stu-id="0dd24-118">You use routing to define URLs that are semantically meaningful to users and that can help with search-engine optimization (SEO).</span></span>

<span data-ttu-id="0dd24-119">根據預設，Web Form 範本包含[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)。</span><span class="sxs-lookup"><span data-stu-id="0dd24-119">By default, the Web Forms template includes [ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/).</span></span> <span data-ttu-id="0dd24-120">大部分基本的路由工作將會透過實作*Friendly URLs*。</span><span class="sxs-lookup"><span data-stu-id="0dd24-120">Much of the basic routing work will be implemented by using *Friendly URLs*.</span></span> <span data-ttu-id="0dd24-121">不過，在本教學課程中，您將加入自訂的路由功能。</span><span class="sxs-lookup"><span data-stu-id="0dd24-121">However, in this tutorial you will add customized routing capabilities.</span></span>

<span data-ttu-id="0dd24-122">在自訂之前 URL 路由，Wingtip Toys 範例應用程式可以連結至產品，使用下列 URL:</span><span class="sxs-lookup"><span data-stu-id="0dd24-122">Before customizing URL routing, the Wingtip Toys sample application can link to a product using the following URL:</span></span>

`https://localhost:44300/ProductDetails.aspx?productID=2`

<span data-ttu-id="0dd24-123">藉由自訂 URL 路由，Wingtip Toys 範例應用程式將會連結至相同的產品使用容易閱讀的 URL:</span><span class="sxs-lookup"><span data-stu-id="0dd24-123">By customizing URL routing, the Wingtip Toys sample application will link to the same product using an easier to read URL:</span></span>

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a><span data-ttu-id="0dd24-124">路由</span><span class="sxs-lookup"><span data-stu-id="0dd24-124">Routes</span></span>

<span data-ttu-id="0dd24-125">路由是對應至處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-125">A route is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="0dd24-126">處理常式可以是實體的檔案，例如.aspx 檔案中的 Web Form 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-126">The handler can be a physical file, such as an .aspx file in a Web Forms application.</span></span> <span data-ttu-id="0dd24-127">處理常式也可以處理要求的類別。</span><span class="sxs-lookup"><span data-stu-id="0dd24-127">A handler can also be a class that processes the request.</span></span> <span data-ttu-id="0dd24-128">若要定義的路由，您可以建立路由類別的執行個體藉由指定的 URL 模式、 處理常式中，並選擇性地路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="0dd24-128">To define a route, you create an instance of the Route class by specifying the URL pattern, the handler, and optionally a name for the route.</span></span>

<span data-ttu-id="0dd24-129">將路由新增至應用程式的加`Route`靜態物件`Routes`屬性`RouteTable`類別。</span><span class="sxs-lookup"><span data-stu-id="0dd24-129">You add the route to the application by adding the `Route` object to the static `Routes` property of the `RouteTable` class.</span></span> <span data-ttu-id="0dd24-130">路由屬性是`RouteCollection`儲存應用程式的所有路由的物件。</span><span class="sxs-lookup"><span data-stu-id="0dd24-130">The Routes property is a `RouteCollection` object that stores all the routes for the application.</span></span>

### <a name="url-patterns"></a><span data-ttu-id="0dd24-131">URL 模式</span><span class="sxs-lookup"><span data-stu-id="0dd24-131">URL Patterns</span></span>

<span data-ttu-id="0dd24-132">URL 模式可以包含常值和變數預留位置 （稱為 URL 參數）。</span><span class="sxs-lookup"><span data-stu-id="0dd24-132">A URL pattern can contain literal values and variable placeholders (referred to as URL parameters).</span></span> <span data-ttu-id="0dd24-133">常值和預留位置位於的 URL，這以斜線分隔的區段 (`/`) 字元。</span><span class="sxs-lookup"><span data-stu-id="0dd24-133">The literals and placeholders are located in segments of the URL which are delimited by the slash (`/`) character.</span></span>

<span data-ttu-id="0dd24-134">您的 web 應用程式的要求進行時，URL 會剖析為區段和預留位置，和變數的值可供要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-134">When a request to your web application is made, the URL is parsed into segments and placeholders, and the variable values are provided to the request handler.</span></span> <span data-ttu-id="0dd24-135">此程序是類似的方式剖析查詢字串中的資料並將其傳遞至要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-135">This process is similar to the way the data in a query string is parsed and passed to the request handler.</span></span> <span data-ttu-id="0dd24-136">在這兩種情況下，會在 URL 中包含變數的資訊，並將其傳遞給索引鍵 / 值組的形式中的處理常式中。</span><span class="sxs-lookup"><span data-stu-id="0dd24-136">In both cases, variable information is included in the URL and passed to the handler in the form of key-value pairs.</span></span> <span data-ttu-id="0dd24-137">查詢字串的索引鍵和值是在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0dd24-137">For query strings, both the keys and the values are in the URL.</span></span> <span data-ttu-id="0dd24-138">對路由索引鍵的 URL 模式中定義的預留位置名稱而只會在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0dd24-138">For routes, the keys are the placeholder names defined in the URL pattern, and only the values are in the URL.</span></span>

<span data-ttu-id="0dd24-139">在 URL 模式中，您必須定義的預留位置中括號括住 (`{`和`}`)。</span><span class="sxs-lookup"><span data-stu-id="0dd24-139">In a URL pattern, you define placeholders by enclosing them in braces ( `{` and `}` ).</span></span> <span data-ttu-id="0dd24-140">您可以定義一個以上的預留位置，在區段中，但必須以常值分隔的預留位置。</span><span class="sxs-lookup"><span data-stu-id="0dd24-140">You can define more than one placeholder in a segment, but the placeholders must be separated by a literal value.</span></span> <span data-ttu-id="0dd24-141">比方說，`{language}-{country}/{action}`是有效的路由的模式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-141">For example, `{language}-{country}/{action}` is a valid route pattern.</span></span> <span data-ttu-id="0dd24-142">不過，`{language}{country}/{action}`不是有效的模式，因為沒有任何常值或預留位置之間的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="0dd24-142">However, `{language}{country}/{action}` is not a valid pattern, because there is no literal value or delimiter between the placeholders.</span></span> <span data-ttu-id="0dd24-143">因此，路由不能判斷如何區隔值語言預留位置值的國家/地區的預留位置。</span><span class="sxs-lookup"><span data-stu-id="0dd24-143">Therefore, routing cannot determine where to separate the value for the language placeholder from the value for the country placeholder.</span></span>

### <a name="mapping-and-registering-routes"></a><span data-ttu-id="0dd24-144">對應和註冊的路由</span><span class="sxs-lookup"><span data-stu-id="0dd24-144">Mapping and Registering Routes</span></span>

<span data-ttu-id="0dd24-145">您可以包含在 Wingtip Toys 範例應用程式頁面的路由之前，您必須在應用程式啟動時註冊的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-145">Before you can include routes to pages of the Wingtip Toys sample application, you must register the routes when the application starts.</span></span> <span data-ttu-id="0dd24-146">若要註冊的路由，您將修改`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-146">To register the routes, you will modify the `Application_Start` event handler.</span></span>

1. <span data-ttu-id="0dd24-147">在 **方案總管**的 Visual Studio 中，尋找並開啟*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="0dd24-147">In **Solution Explorer**of Visual Studio, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="0dd24-148">加入程式碼，以黃色反白顯示*Global.asax.cs*檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0dd24-148">Add the code highlighted in yellow to the *Global.asax.cs* file as follows:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

<span data-ttu-id="0dd24-149">Wingtip Toys 範例應用程式會啟動，它會呼叫`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-149">When the Wingtip Toys sample application starts, it calls the `Application_Start` event handler.</span></span> <span data-ttu-id="0dd24-150">這個事件處理常式中，結尾`RegisterCustomRoutes`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="0dd24-150">At the end of this event handler, the `RegisterCustomRoutes` method is called.</span></span> <span data-ttu-id="0dd24-151">`RegisterCustomRoutes`方法會將每個路由，藉由呼叫`MapPageRoute`方法`RouteCollection`物件。</span><span class="sxs-lookup"><span data-stu-id="0dd24-151">The `RegisterCustomRoutes` method adds each route by calling the `MapPageRoute` method of the `RouteCollection` object.</span></span> <span data-ttu-id="0dd24-152">使用路由名稱的路由 URL 和實體的 URL 進行定義的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-152">Routes are defined using a route name, a route URL and a physical URL.</span></span>

<span data-ttu-id="0dd24-153">第一個參數 ("`ProductsByCategoryRoute`」) 是路由名稱。</span><span class="sxs-lookup"><span data-stu-id="0dd24-153">The first parameter ("`ProductsByCategoryRoute`") is the route name.</span></span> <span data-ttu-id="0dd24-154">它用來在需要時呼叫的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-154">It is used to call the route when it is needed.</span></span> <span data-ttu-id="0dd24-155">第二個參數 ("`Category/{categoryName}`」) 會定義好記的取代程式碼為基礎的可以是動態的 URL。</span><span class="sxs-lookup"><span data-stu-id="0dd24-155">The second parameter ("`Category/{categoryName}`") defines the friendly replacement URL that can be dynamic based on code.</span></span> <span data-ttu-id="0dd24-156">您會填入資料控制項產生資料為基礎的連結時，您可以使用此路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-156">You use this route when you are populating a data control with links that are generated based on data.</span></span> <span data-ttu-id="0dd24-157">路由會顯示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0dd24-157">A route is shown as follows:</span></span>

[!code-csharp[Main](url-routing/samples/sample2.cs)]

<span data-ttu-id="0dd24-158">路由的第二個參數中包含大括號中指定的動態值 (`{ }`)。</span><span class="sxs-lookup"><span data-stu-id="0dd24-158">The second parameter of the route includes a dynamic value specified by braces (`{ }`).</span></span> <span data-ttu-id="0dd24-159">在此情況下，`categoryName`是一個變數，將用來判斷適當的路由路徑。</span><span class="sxs-lookup"><span data-stu-id="0dd24-159">In this case, the `categoryName` is a variable that will be used to determine the proper routing path.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0dd24-160">**Optional**</span><span class="sxs-lookup"><span data-stu-id="0dd24-160">**Optional**</span></span>
> 
> <span data-ttu-id="0dd24-161">您可能會發現它更輕鬆地管理您的程式碼，藉由移動`RegisterCustomRoutes`的不同類別的方法。</span><span class="sxs-lookup"><span data-stu-id="0dd24-161">You might find it easier to manage your code by moving the `RegisterCustomRoutes` method to a separate class.</span></span> <span data-ttu-id="0dd24-162">在 *邏輯*資料夾中，建立個別`RouteActions`類別。</span><span class="sxs-lookup"><span data-stu-id="0dd24-162">In the *Logic* folder, create a separate `RouteActions` class.</span></span> <span data-ttu-id="0dd24-163">移動上述`RegisterCustomRoutes`方法，從*Global.asax.cs*到新的檔案`RoutesActions`類別。</span><span class="sxs-lookup"><span data-stu-id="0dd24-163">Move the above `RegisterCustomRoutes` method from the *Global.asax.cs* file into the new `RoutesActions` class.</span></span> <span data-ttu-id="0dd24-164">使用`RoleActions`類別和`createAdmin`為了舉例說明如何呼叫的方法`RegisterCustomRoutes`方法，從*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="0dd24-164">Use the `RoleActions` class and the `createAdmin` method as an example of how to call the `RegisterCustomRoutes` method from the *Global.asax.cs* file.</span></span>


<span data-ttu-id="0dd24-165">您也可能會發現`RegisterRoutes`方法呼叫使用`RouteConfig`開頭的物件`Application_Start`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-165">You may also have noticed the `RegisterRoutes` method call using the `RouteConfig` object at the beginning of the `Application_Start` event handler.</span></span> <span data-ttu-id="0dd24-166">進行此呼叫，以實作預設路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-166">This call is made to implement default routing.</span></span> <span data-ttu-id="0dd24-167">當您建立使用 Visual Studio 的 Web Form 範本的應用程式時，它是隨附預設的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0dd24-167">It was included as default code when you created the application using Visual Studio's Web Forms template.</span></span>

## <a name="retrieving-and-using-route-data"></a><span data-ttu-id="0dd24-168">擷取並使用路線資料</span><span class="sxs-lookup"><span data-stu-id="0dd24-168">Retrieving and Using Route Data</span></span>

<span data-ttu-id="0dd24-169">如先前所述，您可以定義的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-169">As mentioned above, routes can be defined.</span></span> <span data-ttu-id="0dd24-170">您加入的程式碼`Application_Start`中的事件處理常式*Global.asax.cs*檔案載入的可定義的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-170">The code that you added to the `Application_Start` event handler in the *Global.asax.cs* file loads the definable routes.</span></span>

### <a name="setting-routes"></a><span data-ttu-id="0dd24-171">設定路由</span><span class="sxs-lookup"><span data-stu-id="0dd24-171">Setting Routes</span></span>

<span data-ttu-id="0dd24-172">路由需要您將新增額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0dd24-172">Routes require you to add additional code.</span></span> <span data-ttu-id="0dd24-173">在本教學課程中，您將使用模型繫結來擷取`RouteValueDictionary`產生使用的資料控制項資料的路由時，會使用的物件。</span><span class="sxs-lookup"><span data-stu-id="0dd24-173">In this tutorial, you will use model binding to retrieve a `RouteValueDictionary` object that is used when generating the routes using data from a data control.</span></span> <span data-ttu-id="0dd24-174">`RouteValueDictionary`物件會包含屬於特定類別的產品的產品名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="0dd24-174">The `RouteValueDictionary` object will contain a list of product names that belong to a specific category of products.</span></span> <span data-ttu-id="0dd24-175">根據資料和路由每項產品建立連結。</span><span class="sxs-lookup"><span data-stu-id="0dd24-175">A link is created for each product based on the data and route.</span></span>

#### <a name="enable-routes-for-categories-and-products"></a><span data-ttu-id="0dd24-176">為分類和產品啟用的路由</span><span class="sxs-lookup"><span data-stu-id="0dd24-176">Enable Routes for Categories and Products</span></span>

<span data-ttu-id="0dd24-177">接下來，您將在其中更新應用程式使用`ProductsByCategoryRoute`來判斷正確的路由，以包含每個產品類別目錄連結。</span><span class="sxs-lookup"><span data-stu-id="0dd24-177">Next, you'll update the application to use the `ProductsByCategoryRoute` to determine the correct route to include for each product category link.</span></span> <span data-ttu-id="0dd24-178">您也會更新*ProductList.aspx*加入每個產品的路由的連結的頁面。</span><span class="sxs-lookup"><span data-stu-id="0dd24-178">You'll also update the *ProductList.aspx* page to include a routed link for each product.</span></span> <span data-ttu-id="0dd24-179">連結會顯示之前變更，但連結現在會使用 URL 路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-179">The links will be displayed as they were before the change, however the links will now use URL routing.</span></span>

1. <span data-ttu-id="0dd24-180">在 **方案總管**，開啟*Site.Master*頁面上，如果它尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="0dd24-180">In **Solution Explorer**, open the *Site.Master* page if it is not already open.</span></span>
2. <span data-ttu-id="0dd24-181">更新**ListView**控制項，名為"`categoryList`」 以黃色反白顯示變更，因此標記看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0dd24-181">Update the **ListView** control named "`categoryList`" with the changes highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. <span data-ttu-id="0dd24-182">在 **方案總管**，開啟*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="0dd24-182">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
4. <span data-ttu-id="0dd24-183">更新`ItemTemplate`項目*ProductList.aspx*頁面更新黃色反白顯示，讓標記看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0dd24-183">Update the `ItemTemplate` element of the *ProductList.aspx* page with the updates highlighted in yellow, so the markup appears as follows:</span></span>   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. <span data-ttu-id="0dd24-184">開啟的程式碼後置*ProductList.aspx.cs*並新增下列命名空間為中反白顯示黃色：</span><span class="sxs-lookup"><span data-stu-id="0dd24-184">Open the code-behind of *ProductList.aspx.cs* and add the following namespace as highlighted in yellow:</span></span>  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. <span data-ttu-id="0dd24-185">取代`GetProducts`方法的程式碼後置 (*ProductList.aspx.cs*) 為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0dd24-185">Replace the `GetProducts` method of the code-behind (*ProductList.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a><span data-ttu-id="0dd24-186">將程式碼新增的產品詳細資料</span><span class="sxs-lookup"><span data-stu-id="0dd24-186">Add Code for Product Details</span></span>

<span data-ttu-id="0dd24-187">現在，更新程式碼後置 (*ProductDetails.aspx.cs*) 的*ProductDetails.aspx*頁面，即可使用路由資料。</span><span class="sxs-lookup"><span data-stu-id="0dd24-187">Now, update the code-behind (*ProductDetails.aspx.cs*) for the *ProductDetails.aspx* page to use route data.</span></span> <span data-ttu-id="0dd24-188">請注意，新`GetProduct`方法也可接受的情況下使用者具有設為書籤的連結，會使用較舊的非友善、 非路由 URL 的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="0dd24-188">Notice that the new `GetProduct` method also accepts a query string value for the case where the user has a link bookmarked that uses the older non-friendly, non-routed URL.</span></span>

1. <span data-ttu-id="0dd24-189">取代`GetProduct`方法的程式碼後置 (*ProductDetails.aspx.cs*) 為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0dd24-189">Replace the `GetProduct` method of the code-behind (*ProductDetails.aspx.cs*) with the following code:</span></span>   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a><span data-ttu-id="0dd24-190">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="0dd24-190">Running the Application</span></span>

<span data-ttu-id="0dd24-191">您可以執行應用程式現在若要查看更新的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-191">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="0dd24-192">按下**F5**執行 Wingtip Toys 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="0dd24-192">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="0dd24-193">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="0dd24-193">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="0dd24-194">按一下 **產品**在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="0dd24-194">Click the **Products** link at the top of the page.</span></span>  
 <span data-ttu-id="0dd24-195">顯示所有產品*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="0dd24-195">All products are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="0dd24-196">針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="0dd24-196">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/ProductList`
3. <span data-ttu-id="0dd24-197">接下來，按一下**汽車**靠近頁面頂端的 類別目錄連結。</span><span class="sxs-lookup"><span data-stu-id="0dd24-197">Next, click the **Cars** category link near the top of the page.</span></span>  
 <span data-ttu-id="0dd24-198">只有汽車會顯示在*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="0dd24-198">Only cars are displayed on the *ProductList.aspx* page.</span></span> <span data-ttu-id="0dd24-199">針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="0dd24-199">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Category/Cars`
4. <span data-ttu-id="0dd24-200">按一下頁面列出包含名稱的第一輛車的連結 ("**可轉換汽車**") 來顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0dd24-200">Click the link containing the name of the first car listed on the page ("**Convertible Car**") to display the product details.</span></span>  
 <span data-ttu-id="0dd24-201">針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="0dd24-201">The following URL (using your port number) is displayed for the browser:</span></span>  
    `https://localhost:44300/Product/Convertible%20Car`
5. <span data-ttu-id="0dd24-202">接下來，瀏覽器中輸入下列非路由 URL （使用您的連接埠號碼）：</span><span class="sxs-lookup"><span data-stu-id="0dd24-202">Next, enter the following non-routed URL (using your port number) into the browser:</span></span>  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 <span data-ttu-id="0dd24-203">程式碼仍會辨識包含查詢字串，使用者有設為書籤的連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="0dd24-203">The code still recognizes a URL that includes a query string, for the case where a user has a link bookmarked.</span></span>

## <a name="summary"></a><span data-ttu-id="0dd24-204">總結</span><span class="sxs-lookup"><span data-stu-id="0dd24-204">Summary</span></span>

<span data-ttu-id="0dd24-205">在本教學課程中，您已新增為分類和產品的路由。</span><span class="sxs-lookup"><span data-stu-id="0dd24-205">In this tutorial, you have added routes for categories and products.</span></span> <span data-ttu-id="0dd24-206">您已了解如何整合的路由，以使用模型繫結的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="0dd24-206">You have learned how routes can be integrated with data controls that use model binding.</span></span> <span data-ttu-id="0dd24-207">在下一個教學課程中，您將實作全域錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="0dd24-207">In the next tutorial, you will implement global error handling.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0dd24-208">其他資源</span><span class="sxs-lookup"><span data-stu-id="0dd24-208">Additional Resources</span></span>

[<span data-ttu-id="0dd24-209">ASP.NET 易記 Url</span><span class="sxs-lookup"><span data-stu-id="0dd24-209">ASP.NET Friendly URLs</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[<span data-ttu-id="0dd24-210">使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0dd24-210">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="0dd24-211">Microsoft Azure-免費試用版</span><span class="sxs-lookup"><span data-stu-id="0dd24-211">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> <span data-ttu-id="0dd24-212">[上一頁](membership-and-administration.md)
> [下一頁](aspnet-error-handling.md)</span><span class="sxs-lookup"><span data-stu-id="0dd24-212">[Previous](membership-and-administration.md)
[Next](aspnet-error-handling.md)</span></span>
