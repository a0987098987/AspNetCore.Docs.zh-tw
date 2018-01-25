---
title: "ASP.NET Core MVC EF 核心-排序、 篩選、 分頁-10-3"
author: tdykstra
description: "在本教學課程中，您要加入排序、 篩選和分頁至網頁的 ASP.NET 核心和實體架構的核心功能。"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 60ac1844e7747002d72aa892a47490cb7a416359
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="4f526-103">排序、 篩選、 分頁和群組-EF Core 與 ASP.NET Core MVC 教學課程 (10-3)</span><span class="sxs-lookup"><span data-stu-id="4f526-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="4f526-104">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f526-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f526-105">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="4f526-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="4f526-106">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="4f526-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="4f526-107">在上一個教學課程中，您可以實作網頁學生實體的基本 CRUD 作業的一組。</span><span class="sxs-lookup"><span data-stu-id="4f526-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="4f526-108">在此教學課程中您要加入排序、 篩選和分頁功能學生索引頁。</span><span class="sxs-lookup"><span data-stu-id="4f526-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="4f526-109">此外，還要建立會執行簡單群組頁面。</span><span class="sxs-lookup"><span data-stu-id="4f526-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="4f526-110">下圖顯示當您完成頁面的外觀。</span><span class="sxs-lookup"><span data-stu-id="4f526-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="4f526-111">資料行標題是使用者可以按一下以依據該資料行排序的連結。</span><span class="sxs-lookup"><span data-stu-id="4f526-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="4f526-112">按一下欄標題重複遞增和遞減順序之間切換。</span><span class="sxs-lookup"><span data-stu-id="4f526-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![學生索引頁](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="4f526-114">將資料行排序連結加入至學生索引頁</span><span class="sxs-lookup"><span data-stu-id="4f526-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="4f526-115">若要加入排序學生索引頁，您要變更`Index`學生控制器方法並將程式碼加入至學生索引檢視。</span><span class="sxs-lookup"><span data-stu-id="4f526-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="4f526-116">加入排序功能索引方法</span><span class="sxs-lookup"><span data-stu-id="4f526-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="4f526-117">在*StudentsController.cs*，取代`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4f526-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="4f526-118">此程式碼會接收`sortOrder`從 URL 中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="4f526-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="4f526-119">查詢字串值是由 ASP.NET Core MVC 提供，做為參數至動作方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="4f526-120">參數會是可選擇性地接著底線加上字串"desc"來指定遞減順序為"Name"Date"的字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="4f526-121">預設的排序次序為遞增。</span><span class="sxs-lookup"><span data-stu-id="4f526-121">The default sort order is ascending.</span></span>

<span data-ttu-id="4f526-122">第一次要求的索引頁面時，沒有任何查詢字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="4f526-123">學生姓氏，而且是在中斷的情況中所建立的預設值會顯示以遞增順序`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="4f526-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="4f526-124">當使用者按一下資料行標題超連結，適當`sortOrder`查詢字串中提供值。</span><span class="sxs-lookup"><span data-stu-id="4f526-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="4f526-125">這兩個`ViewData`（NameSortParm 和 DateSortParm） 的項目可用的檢視來設定適當的查詢字串值的資料行標題超連結。</span><span class="sxs-lookup"><span data-stu-id="4f526-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="4f526-126">這些是三元陳述式。</span><span class="sxs-lookup"><span data-stu-id="4f526-126">These are ternary statements.</span></span> <span data-ttu-id="4f526-127">第一個指定，如果`sortOrder`參數為 null 或空的 NameSortParm 應該設定為"name_desc 」; 否則它應該設定為空字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="4f526-128">這些兩個陳述式會啟用檢視設定的資料行標題的超連結，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4f526-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="4f526-129">目前的排序順序</span><span class="sxs-lookup"><span data-stu-id="4f526-129">Current sort order</span></span>  | <span data-ttu-id="4f526-130">最後一個名稱的超連結</span><span class="sxs-lookup"><span data-stu-id="4f526-130">Last Name Hyperlink</span></span> | <span data-ttu-id="4f526-131">日期的超連結</span><span class="sxs-lookup"><span data-stu-id="4f526-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="4f526-132">最後一個名稱升冪</span><span class="sxs-lookup"><span data-stu-id="4f526-132">Last Name ascending</span></span>  | <span data-ttu-id="4f526-133">descending</span><span class="sxs-lookup"><span data-stu-id="4f526-133">descending</span></span>          | <span data-ttu-id="4f526-134">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-134">ascending</span></span>      |
| <span data-ttu-id="4f526-135">最後一個遞減的名稱</span><span class="sxs-lookup"><span data-stu-id="4f526-135">Last Name descending</span></span> | <span data-ttu-id="4f526-136">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-136">ascending</span></span>           | <span data-ttu-id="4f526-137">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-137">ascending</span></span>      |
| <span data-ttu-id="4f526-138">遞增的日期</span><span class="sxs-lookup"><span data-stu-id="4f526-138">Date ascending</span></span>       | <span data-ttu-id="4f526-139">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-139">ascending</span></span>           | <span data-ttu-id="4f526-140">descending</span><span class="sxs-lookup"><span data-stu-id="4f526-140">descending</span></span>     |
| <span data-ttu-id="4f526-141">日期遞減</span><span class="sxs-lookup"><span data-stu-id="4f526-141">Date descending</span></span>      | <span data-ttu-id="4f526-142">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-142">ascending</span></span>           | <span data-ttu-id="4f526-143">ascending</span><span class="sxs-lookup"><span data-stu-id="4f526-143">ascending</span></span>      |

<span data-ttu-id="4f526-144">方法會使用 LINQ to Entities，來指定排序所依據的資料行。</span><span class="sxs-lookup"><span data-stu-id="4f526-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="4f526-145">程式碼會建立`IQueryable`變數 switch 陳述式前的加以修改在 switch 陳述式，並呼叫`ToListAsync`方法之後`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="4f526-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="4f526-146">當您建立和修改`IQueryable`變數沒有查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="4f526-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="4f526-147">不執行查詢，直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToListAsync`。</span><span class="sxs-lookup"><span data-stu-id="4f526-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="4f526-148">因此，此程式碼會產生單一查詢，將會等到執行`return View`陳述式。</span><span class="sxs-lookup"><span data-stu-id="4f526-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="4f526-149">此程式碼可以取得詳細資訊，使用大量的資料行。</span><span class="sxs-lookup"><span data-stu-id="4f526-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="4f526-150">[此系列中的最後一個教學課程](advanced.md#dynamic-linq)示範如何撰寫程式碼，可讓您傳遞的名稱`OrderBy`字串變數中的資料行。</span><span class="sxs-lookup"><span data-stu-id="4f526-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="4f526-151">將資料行標題的超連結加入到學生索引檢視</span><span class="sxs-lookup"><span data-stu-id="4f526-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="4f526-152">中的程式碼取代*Views/Students/Index.cshtml*，若要將資料行標題的超連結加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="4f526-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="4f526-153">反白顯示已變更的行。</span><span class="sxs-lookup"><span data-stu-id="4f526-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="4f526-154">此程式碼使用中的資訊`ViewData`屬性，以設定適當的查詢與超連結的字串值。</span><span class="sxs-lookup"><span data-stu-id="4f526-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="4f526-155">執行應用程式中，選取**學生**索引標籤，然後按一下**姓氏**和**註冊日期**以確認該排序的資料行標題的運作方式。</span><span class="sxs-lookup"><span data-stu-id="4f526-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![學生名稱順序的索引頁](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="4f526-157">新增搜尋方塊學生索引頁</span><span class="sxs-lookup"><span data-stu-id="4f526-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="4f526-158">若要加入篩選學生索引頁，您會將文字方塊及提交按鈕加入至檢視，並進行中的對應變更`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="4f526-159">在文字方塊中，可讓您輸入要搜尋的名字和最後一個名稱欄位中的字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="4f526-160">索引方法中加入篩選功能</span><span class="sxs-lookup"><span data-stu-id="4f526-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="4f526-161">在*StudentsController.cs*，取代`Index`（所做的變更會反白顯示） 為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="4f526-162">您已新增`searchString`參數`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="4f526-163">從文字方塊中，您會加入至索引檢視收到的搜尋字串值。</span><span class="sxs-lookup"><span data-stu-id="4f526-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="4f526-164">您已也加入 LINQ 陳述式 where 子句，會選取其名字或姓氏包含搜尋字串的學生。</span><span class="sxs-lookup"><span data-stu-id="4f526-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="4f526-165">陳述式加入 where 子句在沒有要搜尋的值時，才會執行。</span><span class="sxs-lookup"><span data-stu-id="4f526-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="4f526-166">這裡您將會呼叫`Where`方法`IQueryable`物件和篩選器會在伺服器上處理。</span><span class="sxs-lookup"><span data-stu-id="4f526-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="4f526-167">在某些情況下您可能會呼叫`Where`當做擴充方法上的記憶體中集合的方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="4f526-168">(例如，假設您變更參考`_context.Students`因此，而是的 EF`DbSet`它所參考的儲存機制的方法會傳回`IEnumerable`集合。)結果通常都是一樣的但在某些情況下可能會不同。</span><span class="sxs-lookup"><span data-stu-id="4f526-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="4f526-169">例如，.NET Framework 實作的`Contains`方法會根據預設，執行區分大小寫的比較，但 SQL Server 中這取決於 SQL Server 執行個體的定序設定。</span><span class="sxs-lookup"><span data-stu-id="4f526-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="4f526-170">該設定就會預設為不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="4f526-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="4f526-171">您可以呼叫`ToUpper`來進行測試更明確地不區分大小寫的方法：*其中 (s = > s.LastName.ToUpper()。Contains(searchString.ToUpper())*。</span><span class="sxs-lookup"><span data-stu-id="4f526-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="4f526-172">會確保結果保持相同如果您變更程式碼，稍後要使用的儲存機制傳回`IEnumerable`集合，而不是`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="4f526-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="4f526-173">(當您呼叫`Contains`方法`IEnumerable`集合，取得.NET Framework 實作，則當您呼叫它在`IQueryable`物件，則會收到資料庫提供者實作。)不過，沒有針對此解決方案對效能帶來負面影響。</span><span class="sxs-lookup"><span data-stu-id="4f526-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="4f526-174">`ToUpper`程式碼會置於 TSQL SELECT 陳述式的 WHERE 子句的函式。</span><span class="sxs-lookup"><span data-stu-id="4f526-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="4f526-175">會防止最佳化工具使用索引。</span><span class="sxs-lookup"><span data-stu-id="4f526-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="4f526-176">假設 SQL 大部分安裝為不區分大小寫，就最能夠避免`ToUpper`程式碼，直到您將移轉至區分大小寫的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="4f526-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="4f526-177">學生索引檢視中新增搜尋方塊</span><span class="sxs-lookup"><span data-stu-id="4f526-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="4f526-178">在*Views/Student/Index.cshtml*，開頭才能建立 標題 文字方塊中，表格標記之前加入反白顯示程式碼和**搜尋** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4f526-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="4f526-179">此程式碼使用`<form>`[標記協助程式](xref:mvc/views/tag-helpers/intro)加入搜尋文字方塊和按鈕。</span><span class="sxs-lookup"><span data-stu-id="4f526-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="4f526-180">根據預設，`<form>`標記協助程式送出表單資料的文章時，這表示，參數傳遞的 HTTP 訊息本文，而不是在 URL 查詢字串的形式。</span><span class="sxs-lookup"><span data-stu-id="4f526-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="4f526-181">當您指定 HTTP GET 時，表單資料會在 URL 中當做傳遞查詢字串，可讓使用者 URL 加入書籤。</span><span class="sxs-lookup"><span data-stu-id="4f526-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="4f526-182">更新並不會造成此動作時，就會收到 W3C 指導方針建議，您應該使用。</span><span class="sxs-lookup"><span data-stu-id="4f526-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="4f526-183">執行應用程式中，選取**學生**索引標籤，輸入搜尋字串，然後按一下 [搜尋] 以確認篩選可以運作。</span><span class="sxs-lookup"><span data-stu-id="4f526-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![學生與篩選的索引頁](sort-filter-page/_static/filtering.png)

<span data-ttu-id="4f526-185">請注意 「 URL 」 包含搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="4f526-186">如果您加入書籤此頁面，您會得到的篩選的清單，當您使用書籤。</span><span class="sxs-lookup"><span data-stu-id="4f526-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="4f526-187">加入`method="get"`至`form`標記是什麼造成要產生的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="4f526-188">在這個階段，如果您按一下資料行標題排序連結將會遺失您在中輸入篩選值**搜尋**方塊。</span><span class="sxs-lookup"><span data-stu-id="4f526-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="4f526-189">您可以修正，下一節。</span><span class="sxs-lookup"><span data-stu-id="4f526-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="4f526-190">將分頁功能加入至學生索引頁</span><span class="sxs-lookup"><span data-stu-id="4f526-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="4f526-191">若要加入分頁學生索引頁，您將建立`PaginatedList`使用類別`Skip`和`Take`陳述式來篩選資料，而不是永遠擷取所有資料列之資料表的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="4f526-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="4f526-192">然後您要進行其他變更的`Index`方法並加入分頁按鈕`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="4f526-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="4f526-193">下圖顯示 [分頁] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4f526-193">The following illustration shows the paging buttons.</span></span>

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

<span data-ttu-id="4f526-195">在專案資料夾中，建立`PaginatedList.cs`，然後將範本程式碼取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="4f526-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="4f526-196">`CreateAsync`這個程式碼中的方法會採用頁面大小和頁面數目，並套用適當`Skip`和`Take`陳述式，以`IQueryable`。</span><span class="sxs-lookup"><span data-stu-id="4f526-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="4f526-197">當`ToListAsync`上呼叫`IQueryable`，它會傳回包含要求的頁面的清單。</span><span class="sxs-lookup"><span data-stu-id="4f526-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="4f526-198">屬性`HasPreviousPage`和`HasNextPage`可用來啟用或停用**上一步**和**下一步**分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="4f526-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="4f526-199">A`CreateAsync`方法用來建立而不是建構函式`PaginatedList<T>`物件，因為建構函式無法執行非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="4f526-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="4f526-200">將分頁功能加入至索引方法</span><span class="sxs-lookup"><span data-stu-id="4f526-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="4f526-201">在*StudentsController.cs*，取代`Index`方法取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="4f526-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="4f526-202">這個程式碼會加入頁面的數字參數、 目前的排序順序參數和目前的篩選參數的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="4f526-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="4f526-203">第一次顯示網頁，或如果使用者尚未按一下分頁或排序連結，所有參數將會都是 null。</span><span class="sxs-lookup"><span data-stu-id="4f526-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="4f526-204">按一下分頁連結時，如果頁面變數將包含要顯示的頁面數。</span><span class="sxs-lookup"><span data-stu-id="4f526-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="4f526-205">`ViewData` CurrentSort 名為的項目會提供與目前的排序次序中，檢視，因為這必須以保持在分頁時相同的排序順序包含在分頁連結。</span><span class="sxs-lookup"><span data-stu-id="4f526-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="4f526-206">`ViewData` CurrentFilter 名為的項目提供具有目前篩選條件字串檢視。</span><span class="sxs-lookup"><span data-stu-id="4f526-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="4f526-207">此值必須包含在分頁連結，以篩選設定維護期間分頁，而且它必須還原到文字方塊中時的頁面會重新顯示。</span><span class="sxs-lookup"><span data-stu-id="4f526-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="4f526-208">如果搜尋字串變更在分頁時，此頁面具有要重設為 1，因為新的篩選器可能會導致不同的資料，以顯示。</span><span class="sxs-lookup"><span data-stu-id="4f526-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="4f526-209">在文字方塊中輸入的值，並按下 [提交] 按鈕時，會變更搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="4f526-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="4f526-210">在此情況下，`searchString`參數不是 null。</span><span class="sxs-lookup"><span data-stu-id="4f526-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="4f526-211">在結尾`Index`方法，`PaginatedList.CreateAsync`方法將學生查詢轉換成單一頁面的學生支援分頁的集合型別。</span><span class="sxs-lookup"><span data-stu-id="4f526-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="4f526-212">學生的單一頁面再傳遞到檢視中。</span><span class="sxs-lookup"><span data-stu-id="4f526-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="4f526-213">`PaginatedList.CreateAsync`方法會採用頁面數目。</span><span class="sxs-lookup"><span data-stu-id="4f526-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="4f526-214">兩個問號表示 null 聯合運算子。</span><span class="sxs-lookup"><span data-stu-id="4f526-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="4f526-215">Null 聯合運算子定義預設值為 null 的型別。運算式`(page ?? 1)`方法傳回的值`page`它具有的值，或傳回 1，如果`page`為 null。</span><span class="sxs-lookup"><span data-stu-id="4f526-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="4f526-216">將 Student 索引檢視中的分頁連結</span><span class="sxs-lookup"><span data-stu-id="4f526-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="4f526-217">在*Views/Students/Index.cshtml*，以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4f526-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="4f526-218">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="4f526-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="4f526-219">`@model`在頁面頂端的陳述式指定檢視現在取得`PaginatedList<T>`物件而非`List<T>`物件。</span><span class="sxs-lookup"><span data-stu-id="4f526-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="4f526-220">資料行頁首連結使用查詢字串，將目前的搜尋字串傳遞至控制器，以便使用者可以排序內篩選結果：</span><span class="sxs-lookup"><span data-stu-id="4f526-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="4f526-221">分頁按鈕會顯示標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="4f526-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="4f526-222">執行應用程式，並移至學生頁面。</span><span class="sxs-lookup"><span data-stu-id="4f526-222">Run the app and go to the Students page.</span></span>

![學生索引分頁連結的頁面](sort-filter-page/_static/paging.png)

<span data-ttu-id="4f526-224">按一下分頁中的連結可確定分頁運作的不同排序順序。</span><span class="sxs-lookup"><span data-stu-id="4f526-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="4f526-225">然後輸入搜尋字串，然後再試一次以確認，分頁也會搭配正確排序和篩選的分頁。</span><span class="sxs-lookup"><span data-stu-id="4f526-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="4f526-226">建立學生統計資料會顯示關於頁面</span><span class="sxs-lookup"><span data-stu-id="4f526-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="4f526-227">Contoso 大學網站**有關** 頁面上，您要顯示多少學生已註冊的每個註冊日期。</span><span class="sxs-lookup"><span data-stu-id="4f526-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="4f526-228">這需要在群組的群組和簡單計算。</span><span class="sxs-lookup"><span data-stu-id="4f526-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="4f526-229">若要這麼做，您會執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="4f526-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="4f526-230">建立您要傳遞至檢視資料的檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="4f526-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="4f526-231">修改主控制器中的關於方法。</span><span class="sxs-lookup"><span data-stu-id="4f526-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="4f526-232">修改 [關於] 檢視。</span><span class="sxs-lookup"><span data-stu-id="4f526-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="4f526-233">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="4f526-233">Create the view model</span></span>

<span data-ttu-id="4f526-234">建立*SchoolViewModels*資料夾中的*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="4f526-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="4f526-235">在新的資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*和範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4f526-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="4f526-236">修改主控制器</span><span class="sxs-lookup"><span data-stu-id="4f526-236">Modify the Home Controller</span></span>

<span data-ttu-id="4f526-237">在*HomeController.cs*，加入下列 using 陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="4f526-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="4f526-238">將資料庫內容的類別變數加入之後左大括號類別，並從 ASP.NET Core DI 取得內容的執行個體：</span><span class="sxs-lookup"><span data-stu-id="4f526-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="4f526-239">以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="4f526-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="4f526-240">LINQ 陳述式註冊日期分組的學生實體、 計算每個群組中的實體數目，並將結果儲存在集合中`EnrollmentDateGroup`檢視模型物件。</span><span class="sxs-lookup"><span data-stu-id="4f526-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="4f526-241">在 Entity Framework Core 1.0 版中，整個結果集傳回至用戶端，且群組進行用戶端上。</span><span class="sxs-lookup"><span data-stu-id="4f526-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="4f526-242">在某些情況下，這可以建立效能問題。</span><span class="sxs-lookup"><span data-stu-id="4f526-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="4f526-243">請務必測試效能與實際磁碟區的資料，並如有必要使用原始的 SQL 執行伺服器上的群組。</span><span class="sxs-lookup"><span data-stu-id="4f526-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="4f526-244">如需如何使用原始的 SQL，請參閱[這一系列中的最後一個教學課程](advanced.md)。</span><span class="sxs-lookup"><span data-stu-id="4f526-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="4f526-245">修改檢視</span><span class="sxs-lookup"><span data-stu-id="4f526-245">Modify the About View</span></span>

<span data-ttu-id="4f526-246">中的程式碼取代*Views/Home/About.cshtml*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="4f526-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="4f526-247">執行應用程式，並移至 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="4f526-247">Run the app and go to the About page.</span></span> <span data-ttu-id="4f526-248">針對每個註冊日期的學生人數會顯示在資料表中。</span><span class="sxs-lookup"><span data-stu-id="4f526-248">The count of students for each enrollment date is displayed in a table.</span></span>

![有關頁面](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="4f526-250">總結</span><span class="sxs-lookup"><span data-stu-id="4f526-250">Summary</span></span>

<span data-ttu-id="4f526-251">在本教學課程中，您已經看到如何執行排序、 篩選、 分頁、 與群組。</span><span class="sxs-lookup"><span data-stu-id="4f526-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="4f526-252">在下一個教學課程中，您將學習如何使用移轉作業來處理資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="4f526-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4f526-253">[上一頁](crud.md)
[下一頁](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="4f526-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
