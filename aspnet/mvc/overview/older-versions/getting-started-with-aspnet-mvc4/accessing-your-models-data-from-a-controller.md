---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: 從控制器存取模型的資料 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804933"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="6d2b4-104">從控制器存取模型資料</span><span class="sxs-lookup"><span data-stu-id="6d2b4-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="6d2b4-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6d2b4-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="6d2b4-106">本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="6d2b4-107">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="6d2b4-108">在本節中，您將建立新`MoviesController`類別，並撰寫程式碼會擷取電影資料，並顯示在瀏覽器中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="6d2b4-109">**建置應用程式**再繼續進行下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="6d2b4-110">以滑鼠右鍵按一下*控制器*資料夾，並建立新`MoviesController`控制站。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="6d2b4-111">必須先建置您的應用程式，不會出現下列選項。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="6d2b4-112">選取下列選項：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-112">Select the following options:</span></span>

- <span data-ttu-id="6d2b4-113">控制器名稱： **MoviesController**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="6d2b4-114">（這是預設值。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-114">(This is the default.</span></span> <span data-ttu-id="6d2b4-115">)</span><span class="sxs-lookup"><span data-stu-id="6d2b4-115">)</span></span>
- <span data-ttu-id="6d2b4-116">範本：**使用 Entity Framework 的 MVC 控制器具有讀取/寫入動作和檢視，**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="6d2b4-117">模型類別： **Movie (MvcMovie.Models)**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="6d2b4-118">資料內容類別： **MovieDBContext (MvcMovie.Models)**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="6d2b4-119">檢視： **Razor (CSHTML)**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="6d2b4-120">（預設值。）</span><span class="sxs-lookup"><span data-stu-id="6d2b4-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="6d2b4-122">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-122">Click **Add**.</span></span> <span data-ttu-id="6d2b4-123">Visual Studio Express，會建立下列檔案和資料夾：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="6d2b4-124">*MoviesController.cs*專案中的檔案*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="6d2b4-125">A*電影*在專案的資料夾*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="6d2b4-126">*Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，並*Index.cshtml*在新*Views\Movies*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="6d2b4-127">ASP.NET MVC 4 自動建立 CRUD （建立、 讀取、 更新和刪除） 動作方法和您的檢視 （自動建立 CRUD 動作方法和檢視稱為 scaffolding）。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="6d2b4-128">您現在有功能完整的 web 應用程式，可讓您建立、 列出、 編輯和刪除電影的項目。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="6d2b4-129">執行應用程式，並瀏覽至`Movies`藉由附加的控制站 */Movies*至您的瀏覽器的網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="6d2b4-130">因為應用程式依賴預設路由 (定義於*Global.asax*檔案)，瀏覽器要求`http://localhost:xxxxx/Movies`會路由傳送至預設`Index`動作方法的`Movies`控制站。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="6d2b4-131">換句話說，瀏覽器要求`http://localhost:xxxxx/Movies`實際上是相同的瀏覽器要求`http://localhost:xxxxx/Movies/Index`。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="6d2b4-132">因為您尚未加入任何尚未，結果會是空白的電影清單。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="6d2b4-133">建立電影</span><span class="sxs-lookup"><span data-stu-id="6d2b4-133">Creating a Movie</span></span>

<span data-ttu-id="6d2b4-134">選取 **Create New** 連結。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-134">Select the **Create New** link.</span></span> <span data-ttu-id="6d2b4-135">輸入電影相關的一些詳細資料，然後按一下**建立** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="6d2b4-136">按一下 **建立**按鈕會使表單張貼至伺服器時，電影資訊儲存在資料庫中的位置。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="6d2b4-137">若要將您再重新導向 */Movies* URL，您可以在其中看到新建立的電影清單中。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="6d2b4-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="6d2b4-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="6d2b4-139">建立幾個電影項目。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-139">Create a couple more movie entries.</span></span> <span data-ttu-id="6d2b4-140">嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="6d2b4-141">檢查產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="6d2b4-141">Examining the Generated Code</span></span>

<span data-ttu-id="6d2b4-142">開啟*Controllers\MoviesController.cs*檔案，並檢查產生`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="6d2b4-143">電影控制器的一部分`Index`方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="6d2b4-144">下面這行從`MoviesController`類別執行個體化的電影資料庫內容，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="6d2b4-145">您可用於查詢、 編輯和刪除影片的電影資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="6d2b4-146">要求`Movies`控制器中的所有項目會傳回`Movies`電影資料庫資料表然後將結果傳送至`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="6d2b4-147">強型別模型和@model關鍵字</span><span class="sxs-lookup"><span data-stu-id="6d2b4-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="6d2b4-148">稍早在本教學課程中，您看到如何控制器傳遞資料或物件給檢視範本中使用`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="6d2b4-149">`ViewBag`是動態的物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="6d2b4-150">ASP.NET MVC 也會提供能夠傳遞強型別資料或檢視範本的物件。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="6d2b4-151">強型別方法可讓更佳編譯時間檢查您的程式碼和更豐富的 IntelliSense，Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="6d2b4-152">在 Visual Studio 中的 scaffolding 機制使用這個方法和`MoviesController`類別並檢視範本，建立方法和檢視時。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="6d2b4-153">在  *Controllers\MoviesController.cs*檢查所產生的檔案`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="6d2b4-154">電影控制器的一部分`Details`方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="6d2b4-155">如果`Movie`找到，則執行個體`Movie`模型傳遞至詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="6d2b4-156">檢查的內容*Views\Movies\Details.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="6d2b4-157">藉由包括`@model`陳述式在檢視範本檔案的頂端，您可以指定檢視預期要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="6d2b4-158">當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="6d2b4-159">這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6d2b4-160">例如，在*Details.cshtml*範本，程式碼會傳遞至每個電影欄位`DisplayNameFor`並[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML 協助程式的強型別`Model`物件。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="6d2b4-161">Create 及 Edit 方法和檢視範本也會傳遞影片模型物件。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="6d2b4-162">檢查*Index.cshtml*檢視範本並`Index`中的方法*MoviesController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="6d2b4-163">請注意程式碼的建立方式[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)物件時它會呼叫`View`中的協助程式方法`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="6d2b4-164">程式碼接著會傳遞這`Movies`從控制器到檢視的清單：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="6d2b4-165">當您建立電影控制器時，Visual Studio Express 會自動包含下列`@model`陳述式，在頂端*Index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="6d2b4-166">這`@model`指示詞可讓您存取控制器傳遞至檢視使用的電影清單`Model`強類型的物件。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="6d2b4-167">例如，在*Index.cshtml*範本，此程式碼迴圈電影透過操作`foreach`陳述式，透過強型別`Model`物件：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="6d2b4-168">因為`Model`物件已強類型 (為`IEnumerable<Movie>`物件)，每個`item`迴圈中的物件型別為`Movie`。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="6d2b4-169">撇開其他優點，這表示您取得的程式碼的編譯時間檢查，並完整的程式碼編輯器中的 IntelliSense 支援：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="6d2b4-171">使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="6d2b4-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="6d2b4-172">Entity Framework Code First 偵測到所提供的資料庫連接字串指向`Movies`不存在，所以 Code First 資料庫自動建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="6d2b4-173">您可以確認它在查看已建立*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="6d2b4-174">如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**按鈕**方案總管] 中**工具列上，按一下 [**重新整理**按鈕，然後再展開*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="6d2b4-175">按兩下*Movies.mdf*來開啟**資料庫總管**，然後展開**資料表**資料夾，以查看電影資料表。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="6d2b4-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="6d2b4-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="6d2b4-177">如果未顯示 [資料庫總管] 中，從**工具**功能表上，選取**連接到資料庫**，然後取消**選擇資料來源**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="6d2b4-178">這會強制開啟 [資料庫總管] 中。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="6d2b4-179">如果您使用的 VWD 或 Visual Studio 2010，並出現類似下列的下列任何一個錯誤：</span><span class="sxs-lookup"><span data-stu-id="6d2b4-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="6d2b4-180">資料庫 ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES。MDF' 無法開啟，因為它是版本 706。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="6d2b4-181">這個伺服器支援有 655 和更早版本。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="6d2b4-182">不支援降級路徑。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="6d2b4-183">&quot;InvalidOperation 例外狀況未處理使用者程式碼&quot;提供的 SqlConnection 未指定初始目錄。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="6d2b4-184">您必須安裝[SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)並[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="6d2b4-185">確認`MovieDBContext`前一頁上指定的連接字串。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="6d2b4-186">以滑鼠右鍵按一下`Movies`資料表，然後選取**顯示資料表資料**以查看您所建立的資料。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="6d2b4-187">以滑鼠右鍵按一下`Movies`資料表，然後選取**開啟資料表定義**查看資料表結構，Entity Framework Code First 會為您建立。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="6d2b4-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="6d2b4-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="6d2b4-189">請注意如何的結構描述`Movies`資料表會對應至`Movie`您稍早建立的類別。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="6d2b4-190">Entity Framework Code First 自動建立此結構描述根據您`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="6d2b4-191">當您完成時，關閉連線，以滑鼠右鍵按一下*MovieDBContext* ，然後選取**關閉連線**。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="6d2b4-192">（如果您未關閉連接，您可能會發生錯誤的下次您執行專案）。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="6d2b4-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="6d2b4-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="6d2b4-194">您現在有資料庫和簡單的清單頁面，以顯示來自它的內容。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="6d2b4-195">在下一個教學課程中，我們將檢查的 scaffold 程式碼的其餘部分，並新增`SearchIndex`方法和`SearchIndex`可讓您搜尋此資料庫中的電影的檢視。</span><span class="sxs-lookup"><span data-stu-id="6d2b4-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6d2b4-196">[上一頁](adding-a-model.md)
> [下一頁](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="6d2b4-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
