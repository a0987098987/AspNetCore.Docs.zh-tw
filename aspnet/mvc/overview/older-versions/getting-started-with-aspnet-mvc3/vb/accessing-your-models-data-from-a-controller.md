---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: 存取您的模型資料的控制站 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 030e7cc966078b76b5f96229d437c9990f17656a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller-vb"></a><span data-ttu-id="7de89-103">存取您的模型資料的控制站 (VB)</span><span class="sxs-lookup"><span data-stu-id="7de89-103">Accessing your Model's Data from a Controller (VB)</span></span>
====================
<span data-ttu-id="7de89-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7de89-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7de89-105">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="7de89-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="7de89-106">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="7de89-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="7de89-107">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="7de89-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="7de89-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="7de89-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="7de89-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="7de89-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="7de89-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="7de89-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="7de89-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="7de89-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="7de89-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="7de89-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="7de89-113">使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="7de89-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="7de89-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="7de89-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="7de89-115">如果您偏好 C#，切換至[C# 版本](../cs/accessing-your-models-data-from-a-controller.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="7de89-115">If you prefer C#, switch to the [C# version](../cs/accessing-your-models-data-from-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="7de89-116">在本節中，您將建立新`MoviesController`類別，並撰寫程式碼，擷取電影資料並將它顯示在瀏覽器中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7de89-116">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span> <span data-ttu-id="7de89-117">請務必在建置應用程式後再繼續。</span><span class="sxs-lookup"><span data-stu-id="7de89-117">Be sure to build your application before proceeding.</span></span>

<span data-ttu-id="7de89-118">以滑鼠右鍵按一下*控制器*資料夾並建立新`MoviesController`控制站。</span><span class="sxs-lookup"><span data-stu-id="7de89-118">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="7de89-119">選取下列選項：</span><span class="sxs-lookup"><span data-stu-id="7de89-119">Select the following options:</span></span>

- <span data-ttu-id="7de89-120">控制器名稱： **MoviesController**。</span><span class="sxs-lookup"><span data-stu-id="7de89-120">Controller name: **MoviesController**.</span></span> <span data-ttu-id="7de89-121">（這是預設值）。</span><span class="sxs-lookup"><span data-stu-id="7de89-121">(This is the default.)</span></span>
- <span data-ttu-id="7de89-122">範本：**使用 Entity Framework 的讀取/寫入動作和檢視、 控制器**。</span><span class="sxs-lookup"><span data-stu-id="7de89-122">Template: **Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="7de89-123">模型類別：**影片 (MvcMovie.Models)**。</span><span class="sxs-lookup"><span data-stu-id="7de89-123">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7de89-124">資料內容類別： **MovieDBContext (MvcMovie.Models)**。</span><span class="sxs-lookup"><span data-stu-id="7de89-124">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="7de89-125">檢視： **Razor (CSHTML)**。</span><span class="sxs-lookup"><span data-stu-id="7de89-125">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="7de89-126">（預設值。）</span><span class="sxs-lookup"><span data-stu-id="7de89-126">(The default.)</span></span>

<span data-ttu-id="7de89-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="7de89-128">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="7de89-128">Click **Add**.</span></span> <span data-ttu-id="7de89-129">Visual Web Developer 會建立下列檔案和資料夾：</span><span class="sxs-lookup"><span data-stu-id="7de89-129">Visual Web Developer creates the following files and folders:</span></span>

- <span data-ttu-id="7de89-130">*MoviesController.vb*專案檔案中的*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7de89-130">*A MoviesController.vb* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="7de89-131">A*電影*資料夾置於專案的*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7de89-131">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="7de89-132">*Create.vbhtml，Delete.vbhtml，Details.vbhtml，Edit.vbhtml*，和*Index.vbhtml*在新*Views\Movies*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7de89-132">*Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, and *Index.vbhtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="7de89-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="7de89-134">ASP.NET MVC 3 scaffolding 機制會自動建立的 CRUD （建立、 讀取、 更新和刪除） 的動作方法和您的檢視。</span><span class="sxs-lookup"><span data-stu-id="7de89-134">The ASP.NET MVC 3 scaffolding mechanism automatically created the CRUD (create, read, update, and delete) action methods and views for you.</span></span> <span data-ttu-id="7de89-135">您現在有一個功能完整的 web 應用程式，可讓您建立、 列出、 編輯和刪除影片項目。</span><span class="sxs-lookup"><span data-stu-id="7de89-135">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="7de89-136">執行應用程式，並瀏覽至`Movies`控制器藉由附加*/Movies*至您的瀏覽器的網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="7de89-136">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="7de89-137">因為應用程式依賴預設路由 (定義於*Global.asax*檔案)，瀏覽器要求`http://localhost:xxxxx/Movies`路由傳送至預設`Index`動作方法的`Movies`控制站。</span><span class="sxs-lookup"><span data-stu-id="7de89-137">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="7de89-138">換句話說，瀏覽器要求`http://localhost:xxxxx/Movies`實際上是相同的瀏覽器要求`http://localhost:xxxxx/Movies/Index`。</span><span class="sxs-lookup"><span data-stu-id="7de89-138">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="7de89-139">因為您尚未加入任何尚未，結果會是空的電影清單。</span><span class="sxs-lookup"><span data-stu-id="7de89-139">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a><span data-ttu-id="7de89-140">建立電影</span><span class="sxs-lookup"><span data-stu-id="7de89-140">Creating a Movie</span></span>

<span data-ttu-id="7de89-141">選取 **Create New** 連結。</span><span class="sxs-lookup"><span data-stu-id="7de89-141">Select the **Create New** link.</span></span> <span data-ttu-id="7de89-142">輸入電影的相關詳細資料，然後按一下**建立** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7de89-142">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="7de89-143">按一下**建立**按鈕會導致表單張貼至伺服器，電影資訊儲存在資料庫中的位置。</span><span class="sxs-lookup"><span data-stu-id="7de89-143">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="7de89-144">您重新導向至*/Movies* URL，您可以在其中看到新建立的電影清單中。</span><span class="sxs-lookup"><span data-stu-id="7de89-144">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="7de89-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span></span>

<span data-ttu-id="7de89-146">建立幾個電影項目。</span><span class="sxs-lookup"><span data-stu-id="7de89-146">Create a couple more movie entries.</span></span> <span data-ttu-id="7de89-147">嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="7de89-147">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="7de89-148">檢查產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="7de89-148">Examining the Generated Code</span></span>

<span data-ttu-id="7de89-149">開啟*Controllers\MoviesController.vb*檔案，並檢查產生`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="7de89-149">Open the *Controllers\MoviesController.vb* file and examine the generated `Index` method.</span></span> <span data-ttu-id="7de89-150">部分影片控制器`Index`方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="7de89-150">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

<span data-ttu-id="7de89-151">下列程式行會從`MoviesController`類別執行個體化影片資料庫內容，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="7de89-151">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="7de89-152">您可以使用電影資料庫內容查詢、 編輯和刪除電影。</span><span class="sxs-lookup"><span data-stu-id="7de89-152">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

<span data-ttu-id="7de89-153">要求`Movies`控制站會傳回中的所有項目`Movies`影片資料庫資料表然後將結果傳送至`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="7de89-153">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="7de89-154">強類型模型和@model關鍵字</span><span class="sxs-lookup"><span data-stu-id="7de89-154">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="7de89-155">您已看到稍早在本教學課程中，如何控制站可以傳遞資料或物件給檢視範本使用`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="7de89-155">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="7de89-156">`ViewBag`是動態的物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="7de89-156">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="7de89-157">ASP.NET MVC 也提供將傳遞強的功能類型的資料或檢視表範本的物件。</span><span class="sxs-lookup"><span data-stu-id="7de89-157">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="7de89-158">強型別方法啟用更佳編譯時期檢查您的程式碼和更豐富的 IntelliSense，在 Visual Web Developer 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="7de89-158">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Web Developer editor.</span></span> <span data-ttu-id="7de89-159">我們會使用這個方法與`MoviesController`類別和*Index.vbhtml*檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7de89-159">We're using this approach with the `MoviesController` class and *Index.vbhtml* view template.</span></span>

<span data-ttu-id="7de89-160">請注意程式碼如何建立[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)物件呼叫時，如果`View`中的協助程式方法`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="7de89-160">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="7de89-161">在程式碼再通過這`Movies`從控制器到檢視的清單：</span><span class="sxs-lookup"><span data-stu-id="7de89-161">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

<span data-ttu-id="7de89-162">藉由`@ModelType`頂端的 檢視範本檔案的陳述式，您可以指定的檢視必須要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="7de89-162">By including a `@ModelType` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="7de89-163">當您建立的電影控制站時，Visual Web Developer 會自動包含下列`@model`上方的陳述式*Index.vbhtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="7de89-163">When you created the movie controller, Visual Web Developer automatically included the following `@model` statement at the top of the *Index.vbhtml* file:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

<span data-ttu-id="7de89-164">這`@ModelType`指示詞可讓您存取的控制器檢視的方式來使用傳遞的電影清單`Model`強類型的物件。</span><span class="sxs-lookup"><span data-stu-id="7de89-164">This `@ModelType` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="7de89-165">例如，在*Index.vbhtml*範本，此程式碼迴圈電影透過操作`foreach`陳述式，透過強型別`Model`物件：</span><span class="sxs-lookup"><span data-stu-id="7de89-165">For example, in the *Index.vbhtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

<span data-ttu-id="7de89-166">因為`Model`物件已強類型 (為`IEnumerable<Movie>`物件)，每個`item`迴圈中的物件型別為`Movie`。</span><span class="sxs-lookup"><span data-stu-id="7de89-166">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="7de89-167">撇開其他優點，這表示您會取得程式碼的編譯時期檢查和完整的程式碼編輯器中的 IntelliSense 支援：</span><span class="sxs-lookup"><span data-stu-id="7de89-167">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

<span data-ttu-id="7de89-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span></span>

## <a name="working-with-sql-server-compact"></a><span data-ttu-id="7de89-169">使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="7de89-169">Working with SQL Server Compact</span></span>

<span data-ttu-id="7de89-170">Entity Framework Code First 偵測到提供的資料庫連接字串指向`Movies`不存在，所以 Code First 資料庫自動建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7de89-170">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="7de89-171">您可以確認它已在查看已建立*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7de89-171">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="7de89-172">如果您沒有看到*Movies.sdf*檔案，請按一下**顯示所有檔案**按鈕**方案總管] 中**工具列上，按一下 [**重新整理**按鈕，然後再展開*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7de89-172">If you don't see the *Movies.sdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

<span data-ttu-id="7de89-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span></span>

<span data-ttu-id="7de89-174">按兩下*Movies.sdf*開啟**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="7de89-174">Double-click *Movies.sdf* to open **Server Explorer**.</span></span> <span data-ttu-id="7de89-175">然後展開**資料表**資料夾，以查看資料庫中已建立的資料表。</span><span class="sxs-lookup"><span data-stu-id="7de89-175">Then expand the **Tables** folder to see the tables that have been created in the database.</span></span>

> [!NOTE]
> <span data-ttu-id="7de89-176">如果您收到錯誤，當您按兩下*Movies.sdf*，請確定您已安裝**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**。</span><span class="sxs-lookup"><span data-stu-id="7de89-176">If you get an error when you double-click *Movies.sdf*, make sure you've installed **Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**.</span></span> <span data-ttu-id="7de89-177">（如需軟體的連結，請參閱第 1 部分，此教學課程系列的必要條件清單）。如果您現在安裝的版本，您必須關閉再重新開啟 Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="7de89-177">(For links to the software, see the list of prerequisites in part 1 of this tutorial series.) If you install the release now, you'll have to close and re-open Visual Web Developer.</span></span>


<span data-ttu-id="7de89-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span></span>

<span data-ttu-id="7de89-179">有兩個資料表，一個用於`Movie`實體集，然後`EdmMetadata`資料表。</span><span class="sxs-lookup"><span data-stu-id="7de89-179">There are two tables, one for the `Movie` entity set and then the `EdmMetadata` table.</span></span> <span data-ttu-id="7de89-180">`EdmMetadata`資料表由 Entity Framework 來判斷當模型和資料庫未同步。</span><span class="sxs-lookup"><span data-stu-id="7de89-180">The `EdmMetadata` table is used by the Entity Framework to determine when the model and the database are out of sync.</span></span>

<span data-ttu-id="7de89-181">以滑鼠右鍵按一下`Movies`資料表，然後選取**顯示資料表資料**以查看您所建立的資料。</span><span class="sxs-lookup"><span data-stu-id="7de89-181">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

<span data-ttu-id="7de89-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span></span>

<span data-ttu-id="7de89-183">以滑鼠右鍵按一下`Movies`資料表，然後選取**編輯資料表結構描述**。</span><span class="sxs-lookup"><span data-stu-id="7de89-183">Right-click the `Movies` table and select **Edit Table Schema**.</span></span>

<span data-ttu-id="7de89-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span></span>

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

<span data-ttu-id="7de89-186">請注意如何的結構描述`Movies`資料表對應至`Movie`您稍早建立的類別。</span><span class="sxs-lookup"><span data-stu-id="7de89-186">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="7de89-187">Entity Framework Code First 自動建立此結構描述根據您`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="7de89-187">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="7de89-188">當您完成時，關閉連接。</span><span class="sxs-lookup"><span data-stu-id="7de89-188">When you're finished, close the connection.</span></span> <span data-ttu-id="7de89-189">（如果您未關閉連接，您可能會發生錯誤的下次您執行專案時）。</span><span class="sxs-lookup"><span data-stu-id="7de89-189">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="7de89-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="7de89-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span></span>

<span data-ttu-id="7de89-191">現在您有資料庫的簡單清單頁面，即可顯示來自它的內容。</span><span class="sxs-lookup"><span data-stu-id="7de89-191">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="7de89-192">在下一個教學課程中，我們會檢查 scaffold 的程式碼的其餘部分，並新增`SearchIndex`方法和`SearchIndex`可讓您搜尋的電影，此資料庫中的檢視。</span><span class="sxs-lookup"><span data-stu-id="7de89-192">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7de89-193">[上一頁](adding-a-model.md)
> [下一頁](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="7de89-193">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
