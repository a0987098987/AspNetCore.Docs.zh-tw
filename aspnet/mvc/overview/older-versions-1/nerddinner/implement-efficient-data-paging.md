---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 實作有效率的資料分頁 |Microsoft Docs
author: microsoft
description: 步驟 8 示範如何將分頁支援新增至我們 /Dinners URL，讓不會顯示 1000 個 dinners 一次，我們只會顯示在 10 即將推出的 dinners...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842139"
---
<a name="implement-efficient-data-paging"></a><span data-ttu-id="49a15-103">實作有效率的資料分頁</span><span class="sxs-lookup"><span data-stu-id="49a15-103">Implement Efficient Data Paging</span></span>
====================
<span data-ttu-id="49a15-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="49a15-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="49a15-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="49a15-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="49a15-106">這是一套免費的步驟 8 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="49a15-106">This is step 8 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="49a15-107">步驟 8 示範如何將分頁支援加入至我們 /Dinners URL，以便不會顯示 1000 個 dinners 一次，我們將只顯示 10 即將推出的 dinners 一次-並讓使用者回到頁面和向前逐步執行整個清單在 SEO 易懂的方式。</span><span class="sxs-lookup"><span data-stu-id="49a15-107">Step 8 shows how to add paging support to our /Dinners URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>
> 
> <span data-ttu-id="49a15-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="49a15-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-8-paging-support"></a><span data-ttu-id="49a15-109">NerdDinner 步驟 8： 分頁支援</span><span class="sxs-lookup"><span data-stu-id="49a15-109">NerdDinner Step 8: Paging Support</span></span>

<span data-ttu-id="49a15-110">如果我們的網站成功時，會有數千個即將推出的 dinners。</span><span class="sxs-lookup"><span data-stu-id="49a15-110">If our site is successful, it will have thousands of upcoming dinners.</span></span> <span data-ttu-id="49a15-111">我們要確定我們的 UI 進行調整以處理所有這些 dinners，而且可讓使用者瀏覽它們。</span><span class="sxs-lookup"><span data-stu-id="49a15-111">We need to make sure that our UI scales to handle all of these dinners, and allows users to browse them.</span></span> <span data-ttu-id="49a15-112">若要這麼做，我們將新增的分頁支援我們 */Dinners* URL，因此在我們一次，顯示 1000 個 dinners 將只顯示 10 即將推出的 dinners 一次-並允許使用者頁面後和向前逐步執行中的整個清單SEO 易懂的方式。</span><span class="sxs-lookup"><span data-stu-id="49a15-112">To enable this, we'll add paging support to our */Dinners* URL so that instead of displaying 1000s of dinners at once, we'll only display 10 upcoming dinners at a time - and allow end-users to page back and forward through the entire list in an SEO friendly way.</span></span>

### <a name="index-action-method-recap"></a><span data-ttu-id="49a15-113">Index （） 動作方法的回顧</span><span class="sxs-lookup"><span data-stu-id="49a15-113">Index() Action Method Recap</span></span>

<span data-ttu-id="49a15-114">Index （） 動作方法，我們 DinnersController 類別內目前看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="49a15-114">The Index() action method within our DinnersController class currently looks like below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

<span data-ttu-id="49a15-115">若要提出要求時 */Dinners* URL，它會擷取一份所有即將推出的 dinners 和接下來會呈現出的所有清單：</span><span class="sxs-lookup"><span data-stu-id="49a15-115">When a request is made to the */Dinners* URL, it retrieves a list of all upcoming dinners and then renders a listing of all of them out:</span></span>

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a><span data-ttu-id="49a15-116">了解 IQuerable&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="49a15-116">Understanding IQuerable&lt;T&gt;</span></span>

<span data-ttu-id="49a15-117">*IQueryable&lt;T&gt;* 是引進了與 LINQ 為一部分的.NET 3.5 的介面。</span><span class="sxs-lookup"><span data-stu-id="49a15-117">*IQueryable&lt;T&gt;* is an interface that was introduced with LINQ as part of .NET 3.5.</span></span> <span data-ttu-id="49a15-118">它可讓我們可以利用實作分頁支援的強大 「 延後執行 」 案例。</span><span class="sxs-lookup"><span data-stu-id="49a15-118">It enables powerful "deferred execution" scenarios that we can take advantage of to implement paging support.</span></span>

<span data-ttu-id="49a15-119">在我們 DinnerRepository 中，我們會傳回 IQueryable&lt;Dinner&gt;從我們 FindUpcomingDinners() 方法的順序：</span><span class="sxs-lookup"><span data-stu-id="49a15-119">In our DinnerRepository we are returning an IQueryable&lt;Dinner&gt; sequence from our FindUpcomingDinners() method:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

<span data-ttu-id="49a15-120">IQueryable&lt;Dinner&gt;我們 FindUpcomingDinners() 方法所傳回的物件會封裝 Dinner 物件擷取資料庫使用 LINQ to SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="49a15-120">The IQueryable&lt;Dinner&gt; object returned by our FindUpcomingDinners() method encapsulates a query to retrieve Dinner objects from our database using LINQ to SQL.</span></span> <span data-ttu-id="49a15-121">重要的是，它將不會執行對資料庫的查詢，直到我們嘗試存取/逐一查看資料在查詢中，或在其上呼叫 Tolist 方法。</span><span class="sxs-lookup"><span data-stu-id="49a15-121">Importantly, it won't execute the query against the database until we attempt to access/iterate over the data in the query, or until we call the ToList() method on it.</span></span> <span data-ttu-id="49a15-122">呼叫我們 FindUpcomingDinners() 方法的程式碼可以選擇性地選擇將額外的 「 鏈結 」 作業/篩選新增至 IQueryable&lt;Dinner&gt;之前執行的查詢物件。</span><span class="sxs-lookup"><span data-stu-id="49a15-122">The code calling our FindUpcomingDinners() method can optionally choose to add additional "chained" operations/filters to the IQueryable&lt;Dinner&gt; object before executing the query.</span></span> <span data-ttu-id="49a15-123">LINQ to SQL 就夠聰明，無法合併對資料庫執行查詢時要求的資料。</span><span class="sxs-lookup"><span data-stu-id="49a15-123">LINQ to SQL is then smart enough to execute the combined query against the database when the data is requested.</span></span>

<span data-ttu-id="49a15-124">若要實作分頁邏輯我們可以更新我們 DinnersController index （） 動作方法，使它套用額外的"Skip"和"Take"運算子來傳回 IQueryable&lt;Dinner&gt;之前在其上呼叫 Tolist 的序列：</span><span class="sxs-lookup"><span data-stu-id="49a15-124">To implement paging logic we can update our DinnersController's Index() action method so that it applies additional "Skip" and "Take" operators to the returned IQueryable&lt;Dinner&gt; sequence before calling ToList() on it:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

<span data-ttu-id="49a15-125">上述程式碼會略過資料庫中的前 10 個即將推出 dinners，並傳回上一步 20 dinners。</span><span class="sxs-lookup"><span data-stu-id="49a15-125">The above code skips over the first 10 upcoming dinners in the database, and then returns back 20 dinners.</span></span> <span data-ttu-id="49a15-126">LINQ to SQL 是聰明，足以建構執行此略過邏輯中的 SQL database-而不是在 web 伺服器的最佳化的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="49a15-126">LINQ to SQL is smart enough to construct an optimized SQL query that performs this skipping logic in the SQL database – and not in the web-server.</span></span> <span data-ttu-id="49a15-127">這表示，即使我們即將推出的 Dinners 的數百萬個資料庫中，只有 10 的個我們想要將會擷取 （讓它成為有效率且可調整） 此要求的一部分。</span><span class="sxs-lookup"><span data-stu-id="49a15-127">This means that even if we have millions of upcoming Dinners in the database, only the 10 we want will be retrieved as part of this request (making it efficient and scalable).</span></span>

### <a name="adding-a-page-value-to-the-url"></a><span data-ttu-id="49a15-128">URL 中加入"page"值</span><span class="sxs-lookup"><span data-stu-id="49a15-128">Adding a "page" value to the URL</span></span>

<span data-ttu-id="49a15-129">而不是硬式編碼的特定頁面範圍，我們會想我們的 Url 以包括表示使用者正在要求哪一個 Dinner 範圍的 「 頁面 」 參數。</span><span class="sxs-lookup"><span data-stu-id="49a15-129">Instead of hard-coding a specific page range, we'll want our URLs to include a "page" parameter that indicates which Dinner range a user is requesting.</span></span>

#### <a name="using-a-querystring-value"></a><span data-ttu-id="49a15-130">使用查詢字串值</span><span class="sxs-lookup"><span data-stu-id="49a15-130">Using a Querystring value</span></span>

<span data-ttu-id="49a15-131">下列程式碼示範我們如何更新我們的 index （） 動作方法，以支援的查詢字串參數，並啟用 Url，例如 */Dinners？ 頁面 = 2*:</span><span class="sxs-lookup"><span data-stu-id="49a15-131">The code below demonstrates how we can update our Index() action method to support a querystring parameter and enable URLs like */Dinners?page=2*:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

<span data-ttu-id="49a15-132">上述的 index （） 動作方法具有名為 「 頁面 」 的參數。</span><span class="sxs-lookup"><span data-stu-id="49a15-132">The Index() action method above has a parameter named "page".</span></span> <span data-ttu-id="49a15-133">參數宣告為可為 null 的整數 (這是什麼 int？ 表示)。</span><span class="sxs-lookup"><span data-stu-id="49a15-133">The parameter is declared as a nullable integer (that is what int? indicates).</span></span> <span data-ttu-id="49a15-134">這表示 */Dinners？ 頁面 = 2* URL 將會導致不可傳遞為參數值的"2"的值。</span><span class="sxs-lookup"><span data-stu-id="49a15-134">This means that the */Dinners?page=2* URL will cause a value of "2" to be passed as the parameter value.</span></span> <span data-ttu-id="49a15-135">*/Dinners* URL （不含查詢字串值） 會造成要傳遞 null 值。</span><span class="sxs-lookup"><span data-stu-id="49a15-135">The */Dinners* URL (without a querystring value) will cause a null value to be passed.</span></span>

<span data-ttu-id="49a15-136">我們會乘以分頁值的頁面大小 （在此情況下 10 個資料列） 來判斷多少 dinners 略過。</span><span class="sxs-lookup"><span data-stu-id="49a15-136">We are multiplying the page value by the page size (in this case 10 rows) to determine how many dinners to skip over.</span></span> <span data-ttu-id="49a15-137">我們會使用[C# null"聯合"運算子 （？）](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)處理可為 null 的類型時，這很有用。</span><span class="sxs-lookup"><span data-stu-id="49a15-137">We are using the [C# null "coalescing" operator (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) which is useful when dealing with nullable types.</span></span> <span data-ttu-id="49a15-138">如果頁面參數為 null，上述程式碼將指派頁面 0 的值。</span><span class="sxs-lookup"><span data-stu-id="49a15-138">The code above assigns page the value of 0 if the page parameter is null.</span></span>

#### <a name="using-embedded-url-values"></a><span data-ttu-id="49a15-139">使用內嵌 URL 值</span><span class="sxs-lookup"><span data-stu-id="49a15-139">Using Embedded URL values</span></span>

<span data-ttu-id="49a15-140">使用查詢字串值的替代方案是內嵌實際的 URL 本身內的頁面參數。</span><span class="sxs-lookup"><span data-stu-id="49a15-140">An alternative to using a querystring value would be to embed the page parameter within the actual URL itself.</span></span> <span data-ttu-id="49a15-141">例如： */Dinners/Page/2*或是*Dinners/2*。</span><span class="sxs-lookup"><span data-stu-id="49a15-141">For example: */Dinners/Page/2* or */Dinners/2*.</span></span> <span data-ttu-id="49a15-142">ASP.NET MVC 包含功能強大的 URL 路由引擎，可讓您輕鬆地支援這種情況。</span><span class="sxs-lookup"><span data-stu-id="49a15-142">ASP.NET MVC includes a powerful URL routing engine that makes it easy to support scenarios like this.</span></span>

<span data-ttu-id="49a15-143">我們可以的對應至我們想要任何控制器類別或動作方法的任何連入的 URL 或 URL 格式來註冊自訂的路由規則。</span><span class="sxs-lookup"><span data-stu-id="49a15-143">We can register custom routing rules that map any incoming URL or URL format to any controller class or action method we want.</span></span> <span data-ttu-id="49a15-144">我們只需要待辦事項是開啟我們的專案中的 Global.asax 檔案：</span><span class="sxs-lookup"><span data-stu-id="49a15-144">All we need to-do is to open the Global.asax file within our project:</span></span>

![](implement-efficient-data-paging/_static/image2.png)

<span data-ttu-id="49a15-145">然後註冊新的對應規則，使用 MapRoute() helper 方法，例如路由的第一個呼叫。MapRoute() 下方：</span><span class="sxs-lookup"><span data-stu-id="49a15-145">And then register a new mapping rule using the MapRoute() helper method like the first call to routes.MapRoute() below:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

<span data-ttu-id="49a15-146">以上所述，我們要註冊新的路由規則，名為"UpcomingDinners 」。</span><span class="sxs-lookup"><span data-stu-id="49a15-146">Above we are registering a new routing rule named "UpcomingDinners".</span></span> <span data-ttu-id="49a15-147">我們會指出其 URL 格式"Dinners/網頁 / {}"– 其中 {頁面} 是內嵌在 URL 內的參數值。</span><span class="sxs-lookup"><span data-stu-id="49a15-147">We are indicating it has the URL format "Dinners/Page/{page}" – where {page} is a parameter value embedded within the URL.</span></span> <span data-ttu-id="49a15-148">MapRoute() 方法的第三個參數指出我們應該將對應符合這種格式來 DinnersController 類別上的 index （） 動作方法的 Url。</span><span class="sxs-lookup"><span data-stu-id="49a15-148">The third parameter to the MapRoute() method indicates that we should map URLs that match this format to the Index() action method on the DinnersController class.</span></span>

<span data-ttu-id="49a15-149">我們可以使用完全相同我們先前使用我們的 Querystring 案例 – 除了現在我們 「 頁面 」 的參數會從 URL，而且不是查詢字串的 index （） 程式碼：</span><span class="sxs-lookup"><span data-stu-id="49a15-149">We can use the exact same Index() code we had before with our Querystring scenario – except now our "page" parameter will come from the URL and not the querystring:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

<span data-ttu-id="49a15-150">現在當我們執行應用程式，並輸入並 */Dinners*我們會看到前 10 個即將推出 dinners:</span><span class="sxs-lookup"><span data-stu-id="49a15-150">And now when we run the application and type in */Dinners* we'll see the first 10 upcoming dinners:</span></span>

![](implement-efficient-data-paging/_static/image3.png)

<span data-ttu-id="49a15-151">當我們輸入 */Dinners/Page/1*我們會看到 dinners 的下一個頁面：</span><span class="sxs-lookup"><span data-stu-id="49a15-151">And when we type in */Dinners/Page/1* we'll see the next page of dinners:</span></span>

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a><span data-ttu-id="49a15-152">新增頁面導覽 UI</span><span class="sxs-lookup"><span data-stu-id="49a15-152">Adding page navigation UI</span></span>

<span data-ttu-id="49a15-153">若要完成我們的分頁案例的最後一個步驟是實作 下一步 」 和 「 之前 」 導覽 UI 中檢視範本，讓使用者可以輕鬆地略過 Dinner 資料。</span><span class="sxs-lookup"><span data-stu-id="49a15-153">The last step to complete our paging scenario will be to implement "next" and "previous" navigation UI within our view template to enable users to easily skip over the Dinner data.</span></span>

<span data-ttu-id="49a15-154">若要正確實作此，我們必須在資料庫中，知道 Dinners 總數以及如何多頁資料這會轉譯為。</span><span class="sxs-lookup"><span data-stu-id="49a15-154">To implement this correctly, we'll need to know the total number of Dinners in the database, as well as how many pages of data this translates to.</span></span> <span data-ttu-id="49a15-155">我們接著要計算目前所要求的 「 頁面 」 值的開頭或結尾的資料，以及顯示或隱藏 「 之前 」 和 「 下一步 UI 據此。</span><span class="sxs-lookup"><span data-stu-id="49a15-155">We'll then need to calculate whether the currently requested "page" value is at the beginning or end of the data, and show or hide the "previous" and "next" UI accordingly.</span></span> <span data-ttu-id="49a15-156">我們可以在我們的 index （） 動作方法內實作此邏輯。</span><span class="sxs-lookup"><span data-stu-id="49a15-156">We could implement this logic within our Index() action method.</span></span> <span data-ttu-id="49a15-157">或者我們可以將協助程式類別，加入我們以多重複使用的方式會封裝這個邏輯的專案。</span><span class="sxs-lookup"><span data-stu-id="49a15-157">Alternatively we can add a helper class to our project that encapsulates this logic in a more re-usable way.</span></span>

<span data-ttu-id="49a15-158">以下是一個簡單的"PaginatedList"helper 類別，衍生自清單&lt;T&gt;集合類別，內建於.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="49a15-158">Below is a simple "PaginatedList" helper class that derives from the List&lt;T&gt; collection class built-into the .NET Framework.</span></span> <span data-ttu-id="49a15-159">它會實作可用來重新編頁的 IQueryable 資料的任何序列的重複使用的集合類別。</span><span class="sxs-lookup"><span data-stu-id="49a15-159">It implements a re-usable collection class that can be used to paginate any sequence of IQueryable data.</span></span> <span data-ttu-id="49a15-160">NerdDinner 應用程式中我們要讓它運作 IQueryable&lt;Dinner&gt;結果，但它可以輕易地使用針對 IQueryable&lt;產品&gt;或 IQueryable&lt;客戶&gt;導致其他應用程式案例：</span><span class="sxs-lookup"><span data-stu-id="49a15-160">In our NerdDinner application we'll have it work over IQueryable&lt;Dinner&gt; results, but it could just as easily be used against IQueryable&lt;Product&gt; or IQueryable&lt;Customer&gt; results in other application scenarios:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

<span data-ttu-id="49a15-161">請注意，上例計算的方式，然後公開屬性，例如 「 PageIndex"、"PageSize"、"TotalCount"和 「 TotalPages"。</span><span class="sxs-lookup"><span data-stu-id="49a15-161">Notice above how it calculates and then exposes properties like "PageIndex", "PageSize", "TotalCount", and "TotalPages".</span></span> <span data-ttu-id="49a15-162">它也會公開兩個協助程式屬性"HasPreviousPage 」 和 「 HasNextPage"，表示集合中的資料頁的開頭或結尾的原始順序。</span><span class="sxs-lookup"><span data-stu-id="49a15-162">It also then exposes two helper properties "HasPreviousPage" and "HasNextPage" that indicate whether the page of data in the collection is at the beginning or end of the original sequence.</span></span> <span data-ttu-id="49a15-163">上述程式碼會導致兩個 SQL 查詢執行-第一個擷取 Dinner 物件總數的計數 （這不會傳回物件，而不是它會執行"SELECT COUNT 」 陳述式會傳回整數），若要擷取的列，第二個我們需要從目前的頁面資料的資料庫的資料。</span><span class="sxs-lookup"><span data-stu-id="49a15-163">The above code will cause two SQL queries to be run - the first to retrieve the count of the total number of Dinner objects (this doesn't return the objects – rather it performs a "SELECT COUNT" statement that returns an integer), and the second to retrieve just the rows of data we need from our database for the current page of data.</span></span>

<span data-ttu-id="49a15-164">然後，我們可以更新我們 DinnersController.Index() 協助程式方法，以建立 PaginatedList&lt;Dinner&gt;從我們 DinnerRepository.FindUpcomingDinners() 產生，並將它傳遞至我們的檢視範本：</span><span class="sxs-lookup"><span data-stu-id="49a15-164">We can then update our DinnersController.Index() helper method to create a PaginatedList&lt;Dinner&gt; from our DinnerRepository.FindUpcomingDinners() result, and pass it to our view template:</span></span>

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

<span data-ttu-id="49a15-165">然後，我們可以更新 \Views\Dinners\Index.aspx 檢視範本繼承自 ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt;而不是 ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;，然後將下列程式碼新增至我們檢視範本來顯示或隱藏 下一步 和 上一個巡覽 UI 底部：</span><span class="sxs-lookup"><span data-stu-id="49a15-165">We can then update the \Views\Dinners\Index.aspx view template to inherit from ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt;&gt; instead of ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;, and then add the following code to the bottom of our view-template to show or hide next and previous navigation UI:</span></span>

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

<span data-ttu-id="49a15-166">請注意，上例我們如何使用 Html.RouteLink() helper 方法來產生我們的超連結。</span><span class="sxs-lookup"><span data-stu-id="49a15-166">Notice above how we are using the Html.RouteLink() helper method to generate our hyperlinks.</span></span> <span data-ttu-id="49a15-167">這個方法很類似於我們先前使用過 Html.ActionLink() 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="49a15-167">This method is similar to the Html.ActionLink() helper method we've used previously.</span></span> <span data-ttu-id="49a15-168">差別在於，我們會產生使用 「 UpcomingDinners"路由規則設定我們的 Global.asax 檔案中的 URL。</span><span class="sxs-lookup"><span data-stu-id="49a15-168">The difference is that we are generating the URL using the "UpcomingDinners" routing rule we setup within our Global.asax file.</span></span> <span data-ttu-id="49a15-169">這可確保我們將具有格式我們 index （） 動作方法產生 Url: */Dinners/網頁 / {page}* – 其中 {page} 值是的變數，我們會提供上述根據目前 PageIndex。</span><span class="sxs-lookup"><span data-stu-id="49a15-169">This ensures that we'll generate URLs to our Index() action method that have the format: */Dinners/Page/{page}* – where the {page} value is a variable we are providing above based on the current PageIndex.</span></span>

<span data-ttu-id="49a15-170">看現在當我們執行我們應用程式一次一次的 10 個 dinners 在我們的瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="49a15-170">And now when we run our application again we'll see 10 dinners at a time in our browser:</span></span>

![](implement-efficient-data-paging/_static/image5.png)

<span data-ttu-id="49a15-171">我們也有&lt; &lt; &lt;並&gt; &gt; &gt;巡覽 UI 底部的頁面，好讓我們略過向前和向後透過我們的資料會使用搜尋引擎存取的 Url:</span><span class="sxs-lookup"><span data-stu-id="49a15-171">We also have &lt;&lt;&lt; and &gt;&gt;&gt; navigation UI at the bottom of the page that allows us to skip forwards and backwards over our data using search engine accessible URLs:</span></span>

![](implement-efficient-data-paging/_static/image6.png)

| <span data-ttu-id="49a15-172">**側邊的主題內容： 了解 IQueryable 的含意&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="49a15-172">**Side Topic: Understanding the implications of IQueryable&lt;T&gt;**</span></span> |
| --- |
| <span data-ttu-id="49a15-173">IQueryable&lt;T&gt;是一項非常強大的功能，可讓各種有趣的延後的執行案例 （例如分頁和轉譯緩衝處理架構的查詢）。</span><span class="sxs-lookup"><span data-stu-id="49a15-173">IQueryable&lt;T&gt; is a very powerful feature that enables a variety of interesting deferred execution scenarios (like paging and composition based queries).</span></span> <span data-ttu-id="49a15-174">為所有的強大功能，您想要謹慎使用方式，並確定不被濫用。</span><span class="sxs-lookup"><span data-stu-id="49a15-174">As with all powerful features, you want to be careful with how you use it and make sure it is not abused.</span></span> <span data-ttu-id="49a15-175">請務必辨識該傳回 IQueryable&lt;T&gt;結果從儲存機制可讓呼叫的程式碼，以附加於鏈結的運算子方法，並因此參與 ultimate 的查詢執行。</span><span class="sxs-lookup"><span data-stu-id="49a15-175">It is important to recognize that returning an IQueryable&lt;T&gt; result from your repository enables calling code to append on chained operator methods to it, and so participate in the ultimate query execution.</span></span> <span data-ttu-id="49a15-176">如果不想提供呼叫端程式碼這項功能，則您應該會傳回傳回 IList&lt;T&gt;或 IEnumerable&lt;T&gt;結果-其中包含已執行查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="49a15-176">If you do not want to provide calling code this ability, then you should return back IList&lt;T&gt; or IEnumerable&lt;T&gt; results - which contain the results of a query that has already executed.</span></span> <span data-ttu-id="49a15-177">針對分頁案例這會要求您將實際的資料分頁邏輯推送至儲存機制方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="49a15-177">For pagination scenarios this would require you to push the actual data pagination logic into the repository method being called.</span></span> <span data-ttu-id="49a15-178">在此案例中，我們可能會更新我們 FindUpcomingDinners() 搜尋工具方法中，使其中一個傳回 PaginatedList 的簽章： PaginatedList&lt; Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize） {} 或傳回 IList&lt;Dinner&gt;，使用"totalCount"out 參數傳回 Dinners 總數： IList&lt;Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize，放大 int totalCount） {}</span><span class="sxs-lookup"><span data-stu-id="49a15-178">In this scenario we might update our FindUpcomingDinners() finder method to have a signature that either returned a PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize) { } Or return back an IList&lt;Dinner&gt;, and use a "totalCount" out param to return the total count of Dinners: IList&lt;Dinner&gt; FindUpcomingDinners(int pageIndex, int pageSize, out int totalCount) { }</span></span> |

### <a name="next-step"></a><span data-ttu-id="49a15-179">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="49a15-179">Next Step</span></span>

<span data-ttu-id="49a15-180">現在來看看我們如何新增至應用程式支援的驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="49a15-180">Let's now look at how we can add authentication and authorization support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49a15-181">[上一頁](re-use-ui-using-master-pages-and-partials.md)
> [下一頁](secure-applications-using-authentication-and-authorization.md)</span><span class="sxs-lookup"><span data-stu-id="49a15-181">[Previous](re-use-ui-using-master-pages-and-partials.md)
[Next](secure-applications-using-authentication-and-authorization.md)</span></span>
