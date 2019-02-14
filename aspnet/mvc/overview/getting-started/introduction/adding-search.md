---
uid: mvc/overview/getting-started/introduction/adding-search
title: 搜尋 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: ada125c917656f3a83524ff39e53b4cfc041a497
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248377"
---
<a name="search"></a><span data-ttu-id="fb4b3-102">搜尋</span><span class="sxs-lookup"><span data-stu-id="fb4b3-102">Search</span></span>
====================

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="fb4b3-103">新增搜尋方法和搜尋檢視</span><span class="sxs-lookup"><span data-stu-id="fb4b3-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="fb4b3-104">在本節中，您會將搜尋功能加入`Index`動作方法，可讓您搜尋的內容類型或名稱的電影。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb4b3-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="fb4b3-105">Prerequisites</span></span>

<span data-ttu-id="fb4b3-106">若要符合此區段的螢幕擷取畫面，您需要執行應用程式 (F5)，並將下列的影片新增到資料庫。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="fb4b3-107">標題</span><span class="sxs-lookup"><span data-stu-id="fb4b3-107">Title</span></span> | <span data-ttu-id="fb4b3-108">發行日期</span><span class="sxs-lookup"><span data-stu-id="fb4b3-108">Release Date</span></span> | <span data-ttu-id="fb4b3-109">內容類型</span><span class="sxs-lookup"><span data-stu-id="fb4b3-109">Genre</span></span> | <span data-ttu-id="fb4b3-110">價格</span><span class="sxs-lookup"><span data-stu-id="fb4b3-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="fb4b3-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="fb4b3-111">Ghostbusters</span></span> | <span data-ttu-id="fb4b3-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="fb4b3-112">6/8/1984</span></span> | <span data-ttu-id="fb4b3-113">喜劇</span><span class="sxs-lookup"><span data-stu-id="fb4b3-113">Comedy</span></span> | <span data-ttu-id="fb4b3-114">6.99</span><span class="sxs-lookup"><span data-stu-id="fb4b3-114">6.99</span></span> |
| <span data-ttu-id="fb4b3-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="fb4b3-115">Ghostbusters II</span></span> | <span data-ttu-id="fb4b3-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="fb4b3-116">6/16/1989</span></span> | <span data-ttu-id="fb4b3-117">喜劇</span><span class="sxs-lookup"><span data-stu-id="fb4b3-117">Comedy</span></span> | <span data-ttu-id="fb4b3-118">6.99</span><span class="sxs-lookup"><span data-stu-id="fb4b3-118">6.99</span></span> |
| <span data-ttu-id="fb4b3-119">世界各地的 Apes</span><span class="sxs-lookup"><span data-stu-id="fb4b3-119">Planet of the Apes</span></span> | <span data-ttu-id="fb4b3-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="fb4b3-120">3/27/1986</span></span> | <span data-ttu-id="fb4b3-121">動作</span><span class="sxs-lookup"><span data-stu-id="fb4b3-121">Action</span></span> | <span data-ttu-id="fb4b3-122">5.99</span><span class="sxs-lookup"><span data-stu-id="fb4b3-122">5.99</span></span> |


## <a name="updating-the-index-form"></a><span data-ttu-id="fb4b3-123">更新索引表單</span><span class="sxs-lookup"><span data-stu-id="fb4b3-123">Updating the Index Form</span></span>

<span data-ttu-id="fb4b3-124">藉由更新開始`Index`至現有的動作方法`MoviesController`類別。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="fb4b3-125">程式碼如下：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="fb4b3-126">第一行`Index`方法會建立下列[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="fb4b3-127">到目前為止，定義查詢，但還未對資料庫執行。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="fb4b3-128">如果`searchString`參數包含字串，會修改電影查詢來篩選搜尋字串，使用下列程式碼的值：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="fb4b3-129">上述 `s => s.Title` 程式碼是 [Lambda 運算式](https://msdn.microsoft.com/library/bb397687.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="fb4b3-130">Lambda 會用來在以方法為基礎[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)這類查詢作為標準查詢運算子方法的引數[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上述的程式碼中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="fb4b3-131">當定義或修改這些呼叫的方法，例如，不會執行 LINQ 查詢`Where`或`OrderBy`。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="fb4b3-132">相反地，延後查詢執行，這表示延遲評估的運算式，直到實際反覆運算其實現的值或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="fb4b3-133">在 `Search`範例中，在執行查詢*Index.cshtml*檢視。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="fb4b3-134">如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://msdn.microsoft.com/library/bb738633.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="fb4b3-135">[Contains](https://msdn.microsoft.com/library/bb155125.aspx)方法是在資料庫上執行，不是 c# 程式碼上方。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="fb4b3-136">在資料庫上， [Contains](https://msdn.microsoft.com/library/bb155125.aspx)對應至[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)，這是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="fb4b3-137">現在您可以更新`Index`會向使用者顯示表單的檢視。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="fb4b3-138">執行應用程式，並瀏覽至 */Movies/Index*。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="fb4b3-139">將查詢字串 (例如 `?searchString=ghost`) 附加至 URL。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="fb4b3-140">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="fb4b3-142">如果您變更的簽章`Index`方法具有一個名為參數`id`，則`id`參數會比對`{id}`預留位置的預設路由中的設定*應用程式\_才能執行RouteConfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="fb4b3-143">原始`Index`方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="fb4b3-144">已修改`Index`方法看起來如下：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="fb4b3-145">您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="fb4b3-146">但是，您不能期望使用者在每次想要搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="fb4b3-147">因此，現在您將新增可協助他們篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="fb4b3-148">如果您變更的簽章`Index`方法，以測試如何傳遞路由繫結的 ID 參數，將它變更以便您`Index`方法會採用字串參數，名為`searchString`:</span><span class="sxs-lookup"><span data-stu-id="fb4b3-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="fb4b3-149">開啟*Views\Movies\Index.cshtml*檔案，然後之後`@Html.ActionLink("Create New", "Create")`，新增下列醒目提示的表單標記：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="fb4b3-150">`Html.BeginForm`協助程式會建立開頭`<form>`標記。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="fb4b3-151">`Html.BeginForm`協助程式會導致表單張貼至本身，當使用者提交表單時，依序按一下**篩選** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="fb4b3-152">Visual Studio 2013 都有一項不錯的改進時顯示和編輯檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="fb4b3-153">當您執行應用程式檢視檔案開啟時，Visual Studio 2013 會叫用正確的控制器動作方法，以顯示檢視。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="fb4b3-154">（如圖所示），在 Visual Studio 中開啟 [索引] 檢視中，點選 Ctr f5 鍵或 F5 以執行應用程式然後再次嘗試搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="fb4b3-155">沒有任何`HttpPost`多載`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="fb4b3-156">您不需要它，因為方法不會變更狀態的應用程式，只會篩選資料。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="fb4b3-157">您可以新增下列 `HttpPost Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="fb4b3-158">在此情況下，會比對動作啟動程式`HttpPost Index`方法，而`HttpPost Index`方法會執行下面的影像所示。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="fb4b3-160">不過，即使您新增這個 `HttpPost` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="fb4b3-161">假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="fb4b3-162">請注意，HTTP POST 要求的 URL 是 GET 要求 URL （localhost: xxxxx/Movies/Index） 相同--在 URL 本身沒有搜尋資訊。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="fb4b3-163">權限現在，搜尋字串資訊會傳送到伺服器做為表單欄位值。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="fb4b3-164">這表示您無法擷取該搜尋資訊以加上書籤，或傳送給朋友在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="fb4b3-165">解決方法是使用的多載`BeginForm`可指定 POST 要求，應該將搜尋資訊新增至 URL，以及它應路由至`HttpGet`新版`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="fb4b3-166">取代現有無參數`BeginForm`方法，以下列標記：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="fb4b3-168">現在當您提交搜尋時，URL 會包含搜尋查詢字串。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="fb4b3-169">即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="fb4b3-171">新增依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="fb4b3-171">Adding Search by Genre</span></span>

<span data-ttu-id="fb4b3-172">如果您已新增`HttpPost`新版`Index`方法，立即將其刪除。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="fb4b3-173">接下來，您將新增特定功能，讓使用者進行搜尋的電影內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="fb4b3-174">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="fb4b3-175">本版`Index`方法會採用額外的參數，也就是`movieGenre`。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="fb4b3-176">建立程式碼的前幾行`List`物件來保存從資料庫的電影內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="fb4b3-177">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="fb4b3-178">程式碼會使用`AddRange`方法的泛型`List`將清單中的所有不同的內容類型的集合。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="fb4b3-179">(不含`Distinct`修飾詞，會加入重複的內容類型 — 比方說，喜劇會新增兩次在我們的範例)。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="fb4b3-180">程式碼再將儲存在內容類型清單`ViewBag.MovieGenre`物件。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="fb4b3-181">將類別目錄資料 （這類電影內容類型） 儲存為[SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)物件中`ViewBag`，則存取下拉式清單方塊中的類別目錄資料的 MVC 應用程式的典型方法。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="fb4b3-182">下列程式碼示範如何檢查`movieGenre`參數。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="fb4b3-183">如果不是空的程式碼進一步限制電影查詢來限制所選的影片來指定內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="fb4b3-184">如先前所述，查詢是不在資料庫上執行之前的電影清單逐一 (這會在檢視中，在後`Index`動作方法傳回)。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="fb4b3-185">將標記新增至 [索引] 檢視，以支援依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="fb4b3-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="fb4b3-186">新增`Html.DropDownList`協助專家*Views\Movies\Index.cshtml*檔案，之前`TextBox`協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="fb4b3-187">完整的標記如下所示：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="fb4b3-188">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="fb4b3-189">參數"MovieGenre 」 提供的索引鍵`DropDownList`若要尋找的 helper`IEnumerable<SelectListItem>`在`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="fb4b3-190">`ViewBag`填入動作方法中：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="fb4b3-191">「 全部 」 提供了選項的標籤參數。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="fb4b3-192">如果您在瀏覽器中檢查該選擇，您會看到其 「 值 」 屬性為空白。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="fb4b3-193">因為我們的控制器只會篩選`if`字串不是`null`或空的提交的值是空`movieGenre`顯示所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="fb4b3-194">您也可以設定預設會選取的選項。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="fb4b3-195">如果您想"喜劇"做為預設選項，想要變更在控制器中的程式碼就像這樣：</span><span class="sxs-lookup"><span data-stu-id="fb4b3-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="fb4b3-196">執行應用程式，並瀏覽至 */Movies/Index*。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="fb4b3-197">依內容類型、 電影名稱，以及這兩個準則，請嘗試搜尋。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="fb4b3-198">在本節中，您建立的搜尋動作方法和檢視，可讓使用者進行搜尋電影標題和內容類型。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="fb4b3-199">在下一步 區段中，您將探討如何將屬性新增至`Movie`模型，以及如何將會自動建立測試資料庫的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="fb4b3-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb4b3-200">[上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="fb4b3-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
