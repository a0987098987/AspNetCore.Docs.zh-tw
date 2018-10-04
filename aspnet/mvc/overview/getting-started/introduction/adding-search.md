---
uid: mvc/overview/getting-started/introduction/adding-search
title: 搜尋 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 2cf2274a5592e1f073e62c9b8a789fbb61e23a51
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576372"
---
<a name="search"></a><span data-ttu-id="e394e-102">搜尋</span><span class="sxs-lookup"><span data-stu-id="e394e-102">Search</span></span>
====================
<span data-ttu-id="e394e-103">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="e394e-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="e394e-104">新增搜尋方法和搜尋檢視</span><span class="sxs-lookup"><span data-stu-id="e394e-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="e394e-105">在本節中，您會將搜尋功能加入`Index`動作方法，可讓您搜尋的內容類型或名稱的電影。</span><span class="sxs-lookup"><span data-stu-id="e394e-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="e394e-106">更新索引表單</span><span class="sxs-lookup"><span data-stu-id="e394e-106">Updating the Index Form</span></span>

<span data-ttu-id="e394e-107">藉由更新開始`Index`至現有的動作方法`MoviesController`類別。</span><span class="sxs-lookup"><span data-stu-id="e394e-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="e394e-108">程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="e394e-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="e394e-109">第一行`Index`方法會建立下列[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="e394e-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="e394e-110">到目前為止，定義查詢，但還未對資料庫執行。</span><span class="sxs-lookup"><span data-stu-id="e394e-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="e394e-111">如果`searchString`參數包含字串，會修改電影查詢來篩選搜尋字串，使用下列程式碼的值：</span><span class="sxs-lookup"><span data-stu-id="e394e-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="e394e-112">上述 `s => s.Title` 程式碼是 [Lambda 運算式](https://msdn.microsoft.com/library/bb397687.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e394e-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="e394e-113">Lambda 會用來在以方法為基礎[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)這類查詢作為標準查詢運算子方法的引數[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上述的程式碼中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="e394e-114">當定義或修改這些呼叫的方法，例如，不會執行 LINQ 查詢`Where`或`OrderBy`。</span><span class="sxs-lookup"><span data-stu-id="e394e-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="e394e-115">相反地，延後查詢執行，這表示延遲評估的運算式，直到實際反覆運算其實現的值或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="e394e-116">在 `Search`範例中，在執行查詢*Index.cshtml*檢視。</span><span class="sxs-lookup"><span data-stu-id="e394e-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="e394e-117">如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://msdn.microsoft.com/library/bb738633.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e394e-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e394e-118">[Contains](https://msdn.microsoft.com/library/bb155125.aspx)方法是在資料庫上執行，不是 c# 程式碼上方。</span><span class="sxs-lookup"><span data-stu-id="e394e-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="e394e-119">在資料庫上， [Contains](https://msdn.microsoft.com/library/bb155125.aspx)對應至[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)，這是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e394e-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="e394e-120">現在您可以更新`Index`會向使用者顯示表單的檢視。</span><span class="sxs-lookup"><span data-stu-id="e394e-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="e394e-121">執行應用程式，並瀏覽至 */Movies/Index*。</span><span class="sxs-lookup"><span data-stu-id="e394e-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="e394e-122">將查詢字串 (例如 `?searchString=ghost`) 附加至 URL。</span><span class="sxs-lookup"><span data-stu-id="e394e-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="e394e-123">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="e394e-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="e394e-125">如果您變更的簽章`Index`方法具有一個名為參數`id`，則`id`參數會比對`{id}`預留位置的預設路由中的設定*應用程式\_才能執行RouteConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e394e-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="e394e-126">原始`Index`方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="e394e-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="e394e-127">已修改`Index`方法看起來如下：</span><span class="sxs-lookup"><span data-stu-id="e394e-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="e394e-128">您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="e394e-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="e394e-129">但是，您不能期望使用者在每次想要搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="e394e-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="e394e-130">所以現在您將 UI，可協助他們篩選電影。</span><span class="sxs-lookup"><span data-stu-id="e394e-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="e394e-131">如果您變更的簽章`Index`方法，以測試如何傳遞路由繫結的 ID 參數，將它變更以便您`Index`方法會採用字串參數，名為`searchString`:</span><span class="sxs-lookup"><span data-stu-id="e394e-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="e394e-132">開啟*Views\Movies\Index.cshtml*檔案，然後之後`@Html.ActionLink("Create New", "Create")`，新增下列醒目提示的表單標記：</span><span class="sxs-lookup"><span data-stu-id="e394e-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="e394e-133">`Html.BeginForm`協助程式會建立開頭`<form>`標記。</span><span class="sxs-lookup"><span data-stu-id="e394e-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="e394e-134">`Html.BeginForm`協助程式會導致表單張貼至本身，當使用者提交表單時，依序按一下**篩選** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e394e-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="e394e-135">Visual Studio 2013 都有一項不錯的改進時顯示和編輯檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="e394e-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="e394e-136">當您執行應用程式檢視檔案開啟時，Visual Studio 2013 會叫用正確的控制器動作方法，以顯示檢視。</span><span class="sxs-lookup"><span data-stu-id="e394e-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="e394e-137">（如圖所示），在 Visual Studio 中開啟 [索引] 檢視中，點選 Ctr f5 鍵或 F5 以執行應用程式然後再次嘗試搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="e394e-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="e394e-138">沒有任何`HttpPost`多載`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="e394e-139">您不需要它，因為方法不會變更狀態的應用程式，只會篩選資料。</span><span class="sxs-lookup"><span data-stu-id="e394e-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="e394e-140">您可以新增下列 `HttpPost Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="e394e-141">在此情況下，會比對動作啟動程式`HttpPost Index`方法，而`HttpPost Index`方法會執行下面的影像所示。</span><span class="sxs-lookup"><span data-stu-id="e394e-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="e394e-143">不過，即使您新增這個 `HttpPost` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。</span><span class="sxs-lookup"><span data-stu-id="e394e-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="e394e-144">假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。</span><span class="sxs-lookup"><span data-stu-id="e394e-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="e394e-145">請注意，HTTP POST 要求的 URL 是 GET 要求 URL （localhost: xxxxx/Movies/Index） 相同--在 URL 本身沒有搜尋資訊。</span><span class="sxs-lookup"><span data-stu-id="e394e-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="e394e-146">權限現在，搜尋字串資訊會傳送到伺服器做為表單欄位值。</span><span class="sxs-lookup"><span data-stu-id="e394e-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="e394e-147">這表示您無法擷取該搜尋資訊以加上書籤，或傳送給朋友在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="e394e-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="e394e-148">解決方法是使用的多載`BeginForm`可指定 POST 要求，應該將搜尋資訊新增至 URL，以及它應路由至`HttpGet`新版`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="e394e-149">取代現有無參數`BeginForm`方法，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="e394e-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="e394e-151">現在當您提交搜尋時，URL 會包含搜尋查詢字串。</span><span class="sxs-lookup"><span data-stu-id="e394e-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="e394e-152">即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="e394e-154">新增依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="e394e-154">Adding Search by Genre</span></span>

<span data-ttu-id="e394e-155">如果您已新增`HttpPost`新版`Index`方法，立即將其刪除。</span><span class="sxs-lookup"><span data-stu-id="e394e-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="e394e-156">接下來，您將新增特定功能，讓使用者進行搜尋的電影內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="e394e-157">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="e394e-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="e394e-158">本版`Index`方法會採用額外的參數，也就是`movieGenre`。</span><span class="sxs-lookup"><span data-stu-id="e394e-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="e394e-159">建立程式碼的前幾行`List`物件來保存從資料庫的電影內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="e394e-160">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="e394e-161">程式碼會使用`AddRange`方法的泛型`List`將清單中的所有不同的內容類型的集合。</span><span class="sxs-lookup"><span data-stu-id="e394e-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="e394e-162">(不含`Distinct`修飾詞，會加入重複的內容類型 — 比方說，喜劇會新增兩次在我們的範例)。</span><span class="sxs-lookup"><span data-stu-id="e394e-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="e394e-163">程式碼再將儲存在內容類型清單`ViewBag.MovieGenre`物件。</span><span class="sxs-lookup"><span data-stu-id="e394e-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="e394e-164">儲存類別目錄資料 （這類電影內容類型的） 當做[SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)物件中`ViewBag`，則存取下拉式清單方塊中的類別目錄資料的 MVC 應用程式的典型方法。</span><span class="sxs-lookup"><span data-stu-id="e394e-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="e394e-165">下列程式碼示範如何檢查`movieGenre`參數。</span><span class="sxs-lookup"><span data-stu-id="e394e-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="e394e-166">如果不是空的程式碼進一步限制電影查詢來限制所選的影片來指定內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="e394e-167">如先前所述，不執行查詢上的資料基底直到電影清單逐一 (這會在檢視中，在後`Index`動作方法傳回)。</span><span class="sxs-lookup"><span data-stu-id="e394e-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="e394e-168">將標記新增至 [索引] 檢視，以支援依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="e394e-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="e394e-169">新增`Html.DropDownList`協助專家*Views\Movies\Index.cshtml*檔案，之前`TextBox`協助程式。</span><span class="sxs-lookup"><span data-stu-id="e394e-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="e394e-170">完整的標記如下所示：</span><span class="sxs-lookup"><span data-stu-id="e394e-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="e394e-171">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e394e-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="e394e-172">參數"MovieGenre 」 提供的索引鍵`DropDownList`若要尋找的 helper`IEnumerable<SelectListItem>`在`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="e394e-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="e394e-173">`ViewBag`填入動作方法中：</span><span class="sxs-lookup"><span data-stu-id="e394e-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="e394e-174">「 全部 」 提供了選項的標籤參數。</span><span class="sxs-lookup"><span data-stu-id="e394e-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="e394e-175">如果您在瀏覽器中檢查該選擇，您會看到其 「 值 」 屬性為空白。</span><span class="sxs-lookup"><span data-stu-id="e394e-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="e394e-176">因為我們的控制器只會篩選`if`字串不是`null`或空的提交的值是空`movieGenre`顯示所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="e394e-177">您也可以設定預設會選取的選項。</span><span class="sxs-lookup"><span data-stu-id="e394e-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="e394e-178">如果您想"喜劇"做為預設選項，想要變更在控制器中的程式碼就像這樣：</span><span class="sxs-lookup"><span data-stu-id="e394e-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="e394e-179">執行應用程式，並瀏覽至 */Movies/Index*。</span><span class="sxs-lookup"><span data-stu-id="e394e-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="e394e-180">依內容類型、 電影名稱，以及這兩個準則，請嘗試搜尋。</span><span class="sxs-lookup"><span data-stu-id="e394e-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="e394e-181">在本節中，您建立的搜尋動作方法和檢視，可讓使用者進行搜尋電影標題和內容類型。</span><span class="sxs-lookup"><span data-stu-id="e394e-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="e394e-182">在下一步 區段中，您將探討如何將屬性新增至`Movie`模型，以及如何將會自動建立測試資料庫的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="e394e-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e394e-183">[上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="e394e-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
