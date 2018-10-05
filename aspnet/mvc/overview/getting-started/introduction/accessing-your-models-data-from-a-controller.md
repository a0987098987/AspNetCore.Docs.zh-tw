---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: 從控制器存取模型的資料 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 00731fbc0d578afa2df881b205170fb6a4f686e1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576270"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="8e257-102">從控制器存取模型資料</span><span class="sxs-lookup"><span data-stu-id="8e257-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="8e257-103">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="8e257-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="8e257-104">在本節中，您將建立新`MoviesController`類別，並撰寫程式碼會擷取電影資料，並顯示在瀏覽器中使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8e257-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="8e257-105">**建置應用程式**再繼續進行下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="8e257-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="8e257-106">如果您沒有建置應用程式，您會收到錯誤新增控制器。</span><span class="sxs-lookup"><span data-stu-id="8e257-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="8e257-107">在 [方案總管] 中，以滑鼠右鍵按一下*控制器*資料夾，然後按一下**新增**，然後**控制器**。</span><span class="sxs-lookup"><span data-stu-id="8e257-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="8e257-108">在 **新增 Scaffold**  對話方塊中，按一下**的 MVC 5 控制器與檢視，使用 Entity Framework**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="8e257-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="8e257-109">選取  **Movie (MvcMovie.Models)** 模型類別。</span><span class="sxs-lookup"><span data-stu-id="8e257-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="8e257-110">選取  **MovieDBContext (MvcMovie.Models)** 資料內容類別。</span><span class="sxs-lookup"><span data-stu-id="8e257-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="8e257-111">控制器名稱的輸入**MoviesController**。</span><span class="sxs-lookup"><span data-stu-id="8e257-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="8e257-112">下圖顯示 [已完成] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="8e257-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="8e257-113">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="8e257-113">Click **Add**.</span></span> <span data-ttu-id="8e257-114">（如果您收到錯誤，您可能未建置的應用程式，再啟動 新增控制器）。Visual Studio 會建立下列檔案和資料夾：</span><span class="sxs-lookup"><span data-stu-id="8e257-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="8e257-115">*MoviesController.cs*檔案中*控制器*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e257-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="8e257-116">A *Views\Movies*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e257-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="8e257-117">*Create.cshtml，Delete.cshtml，Details.cshtml，Edit.cshtml*，並*Index.cshtml*在新*Views\Movies*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e257-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="8e257-118">自動建立的 visual Studio [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) （建立、 讀取、 更新和刪除） 動作方法和您的檢視 （自動建立 CRUD 動作方法和檢視稱為 scaffolding）。</span><span class="sxs-lookup"><span data-stu-id="8e257-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="8e257-119">您現在有功能完整的 web 應用程式，可讓您建立、 列出、 編輯和刪除電影的項目。</span><span class="sxs-lookup"><span data-stu-id="8e257-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="8e257-120">執行應用程式，然後按一下**MVC Movie**連結 (或瀏覽至`Movies`控制站，藉由附加 */Movies*至您的瀏覽器的網址列中的 URL)。</span><span class="sxs-lookup"><span data-stu-id="8e257-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="8e257-121">因為應用程式依賴預設路由 (定義於*應用程式\_Start\RouteConfig.cs*檔案)，瀏覽器要求`http://localhost:xxxxx/Movies`會路由傳送至預設`Index`動作方法`Movies`控制器。</span><span class="sxs-lookup"><span data-stu-id="8e257-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="8e257-122">換句話說，瀏覽器要求`http://localhost:xxxxx/Movies`實際上是相同的瀏覽器要求`http://localhost:xxxxx/Movies/Index`。</span><span class="sxs-lookup"><span data-stu-id="8e257-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="8e257-123">因為您尚未加入任何尚未，結果會是空白的電影清單。</span><span class="sxs-lookup"><span data-stu-id="8e257-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="8e257-124">建立電影</span><span class="sxs-lookup"><span data-stu-id="8e257-124">Creating a Movie</span></span>

<span data-ttu-id="8e257-125">選取 **Create New** 連結。</span><span class="sxs-lookup"><span data-stu-id="8e257-125">Select the **Create New** link.</span></span> <span data-ttu-id="8e257-126">輸入電影相關的一些詳細資料，然後按一下**建立** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8e257-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="8e257-127">您可能無法在 [價格] 欄位中輸入小數點或逗號。</span><span class="sxs-lookup"><span data-stu-id="8e257-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="8e257-128">以 jQuery 驗證支援非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點和非英文日期格式，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用的 JavaScript `Globalize.parseFloat`。</span><span class="sxs-lookup"><span data-stu-id="8e257-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="8e257-129">我將示範如何在下一個教學課程中這麼做。</span><span class="sxs-lookup"><span data-stu-id="8e257-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="8e257-130">現在，只要輸入如 10 之類的整數。</span><span class="sxs-lookup"><span data-stu-id="8e257-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="8e257-131">按一下 **建立**按鈕會使表單張貼至伺服器時，電影資訊儲存在資料庫中的位置。</span><span class="sxs-lookup"><span data-stu-id="8e257-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="8e257-132">若要將您再重新導向 */Movies* URL，您可以在其中看到新建立的電影清單中。</span><span class="sxs-lookup"><span data-stu-id="8e257-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="8e257-133">建立幾個電影項目。</span><span class="sxs-lookup"><span data-stu-id="8e257-133">Create a couple more movie entries.</span></span> <span data-ttu-id="8e257-134">嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="8e257-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="8e257-135">檢查產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="8e257-135">Examining the Generated Code</span></span>

<span data-ttu-id="8e257-136">開啟*Controllers\MoviesController.cs*檔案，並檢查產生`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="8e257-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="8e257-137">電影控制器的一部分`Index`方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="8e257-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="8e257-138">若要要求`Movies`控制器傳回中的所有項目`Movies`資料表，並接著會將結果傳遞`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="8e257-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="8e257-139">下面這行從`MoviesController`類別執行個體化的電影資料庫內容，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="8e257-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="8e257-140">您可用於查詢、 編輯和刪除影片的電影資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="8e257-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="8e257-141">強型別模型和@model關鍵字</span><span class="sxs-lookup"><span data-stu-id="8e257-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="8e257-142">稍早在本教學課程中，您看到如何控制器傳遞資料或物件給檢視範本中使用`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="8e257-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="8e257-143">`ViewBag`是動態的物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="8e257-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="8e257-144">MVC 也讓您能夠傳遞*強*型別檢視範本的物件。</span><span class="sxs-lookup"><span data-stu-id="8e257-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="8e257-145">這個強類型的方法可讓更佳編譯時間檢查您的程式碼以及更豐富[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio 編輯器中。</span><span class="sxs-lookup"><span data-stu-id="8e257-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="8e257-146">Scaffolding 機制，在 Visual Studio 中的使用這種方法 (也就傳遞*強烈*型別的模型) 與`MoviesController`類別並檢視範本，建立方法和檢視時。</span><span class="sxs-lookup"><span data-stu-id="8e257-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="8e257-147">在  *Controllers\MoviesController.cs*檢查所產生的檔案`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="8e257-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="8e257-148">`Details`方法如下所示。</span><span class="sxs-lookup"><span data-stu-id="8e257-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="8e257-149">`id`參數通常會傳遞為路由資料，例如`http://localhost:1234/movies/details/1`會將控制器設到電影控制器時，所要的動作`details`而`id`為 1。</span><span class="sxs-lookup"><span data-stu-id="8e257-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="8e257-150">您也可以傳入查詢字串的 id，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e257-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="8e257-151">如果`Movie`找到，則執行個體`Movie`的模型傳遞至`Details`檢視：</span><span class="sxs-lookup"><span data-stu-id="8e257-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="8e257-152">檢查的內容*Views\Movies\Details.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="8e257-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="8e257-153">藉由包括`@model`陳述式在檢視範本檔案的頂端，您可以指定檢視預期要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="8e257-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="8e257-154">當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="8e257-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="8e257-155">這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。</span><span class="sxs-lookup"><span data-stu-id="8e257-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="8e257-156">例如，在*Details.cshtml*範本，程式碼會傳遞至每個電影欄位`DisplayNameFor`並[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML 協助程式的強型別`Model`物件。</span><span class="sxs-lookup"><span data-stu-id="8e257-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="8e257-157">`Create`和`Edit`方法和檢視範本也會傳遞影片模型物件。</span><span class="sxs-lookup"><span data-stu-id="8e257-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="8e257-158">檢查*Index.cshtml*檢視範本並`Index`中的方法*MoviesController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="8e257-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="8e257-159">請注意程式碼的建立方式[ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)物件時它會呼叫`View`中的協助程式方法`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="8e257-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="8e257-160">程式碼接著會傳遞這`Movies`從清單`Index`至檢視的動作方法：</span><span class="sxs-lookup"><span data-stu-id="8e257-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="8e257-161">當您建立電影控制器時，Visual Studio 會自動包含下列`@model`陳述式，在頂端*Index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="8e257-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="8e257-162">這`@model`指示詞可讓您存取控制器傳遞至檢視使用的電影清單`Model`強類型的物件。</span><span class="sxs-lookup"><span data-stu-id="8e257-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="8e257-163">例如，在*Index.cshtml*範本，此程式碼迴圈電影透過操作`foreach`陳述式，透過強型別`Model`物件：</span><span class="sxs-lookup"><span data-stu-id="8e257-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="8e257-164">因為`Model`物件已強類型 (為`IEnumerable<Movie>`物件)，每個`item`迴圈中的物件型別為`Movie`。</span><span class="sxs-lookup"><span data-stu-id="8e257-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="8e257-165">撇開其他優點，這表示您取得的程式碼的編譯時間檢查，並完整的程式碼編輯器中的 IntelliSense 支援：</span><span class="sxs-lookup"><span data-stu-id="8e257-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="8e257-167">使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="8e257-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="8e257-168">Entity Framework Code First 偵測到所提供的資料庫連接字串指向`Movies`不存在，所以 Code First 資料庫自動建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8e257-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="8e257-169">您可以確認它在查看已建立*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e257-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="8e257-170">如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**按鈕**方案總管] 中**工具列上，按一下 [**重新整理**按鈕，然後再展開*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e257-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="8e257-171">按兩下*Movies.mdf*來開啟**伺服器總管**，然後展開**資料表**資料夾，以查看電影資料表。</span><span class="sxs-lookup"><span data-stu-id="8e257-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="8e257-172">請注意索引鍵的圖示旁邊識別碼。</span><span class="sxs-lookup"><span data-stu-id="8e257-172">Note the key icon next to ID.</span></span> <span data-ttu-id="8e257-173">根據預設，EF 將名為 ID 的主索引鍵的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e257-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="8e257-174">如需有關 EF 和 MVC 的詳細資訊，請參閱 Tom Dykstra 絕佳的教學課程[MVC 和 EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="8e257-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="8e257-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="8e257-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="8e257-176">以滑鼠右鍵按一下`Movies`資料表，然後選取**顯示資料表資料**以查看您所建立的資料。</span><span class="sxs-lookup"><span data-stu-id="8e257-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="8e257-177">以滑鼠右鍵按一下`Movies`資料表，然後選取**開啟資料表定義**查看資料表結構，Entity Framework Code First 會為您建立。</span><span class="sxs-lookup"><span data-stu-id="8e257-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="8e257-178">請注意如何的結構描述`Movies`資料表會對應至`Movie`您稍早建立的類別。</span><span class="sxs-lookup"><span data-stu-id="8e257-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="8e257-179">Entity Framework Code First 自動建立此結構描述根據您`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="8e257-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="8e257-180">當您完成時，關閉連線，以滑鼠右鍵按一下*MovieDBContext* ，然後選取**關閉連線**。</span><span class="sxs-lookup"><span data-stu-id="8e257-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="8e257-181">（如果您未關閉連接，您可能會發生錯誤的下次您執行專案）。</span><span class="sxs-lookup"><span data-stu-id="8e257-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="8e257-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="8e257-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="8e257-183">您現在擁有一個資料庫和多個頁面，可用來顯示、編輯、更新和刪除資料。</span><span class="sxs-lookup"><span data-stu-id="8e257-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="8e257-184">在下一個教學課程中，我們將檢查的 scaffold 程式碼的其餘部分，並新增`SearchIndex`方法和`SearchIndex`可讓您搜尋此資料庫中的電影的檢視。</span><span class="sxs-lookup"><span data-stu-id="8e257-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="8e257-185">如需有關使用使用 MVC 的 Entity Framework 的詳細資訊，請參閱 <<c0> [ 建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="8e257-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e257-186">[上一頁](creating-a-connection-string.md)
> [下一頁](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="8e257-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
