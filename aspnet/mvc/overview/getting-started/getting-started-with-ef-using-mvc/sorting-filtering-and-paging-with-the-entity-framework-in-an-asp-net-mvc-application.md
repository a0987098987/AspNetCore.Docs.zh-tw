---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d54c0e133bc2f6f2021821dc16cdf86cc23a5667
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="d12f6-103">排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="d12f6-103">Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="d12f6-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d12f6-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d12f6-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="d12f6-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="d12f6-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="d12f6-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="d12f6-108">在上一個教學課程中您會實作一組基本 CRUD 作業的 web 網頁`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="d12f6-108">In the previous tutorial you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="d12f6-109">在本教學課程，您要加入排序、 篩選和分頁功能**學生**索引頁面。</span><span class="sxs-lookup"><span data-stu-id="d12f6-109">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="d12f6-110">此外，還要建立會執行簡單群組頁面。</span><span class="sxs-lookup"><span data-stu-id="d12f6-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="d12f6-111">下圖顯示當您完成頁面的外觀。</span><span class="sxs-lookup"><span data-stu-id="d12f6-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="d12f6-112">資料行標題是使用者可以按一下以依據該資料行排序的連結。</span><span class="sxs-lookup"><span data-stu-id="d12f6-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="d12f6-113">按一下欄標題重複遞增和遞減順序之間切換。</span><span class="sxs-lookup"><span data-stu-id="d12f6-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="d12f6-115">將資料行排序連結加入至學生索引頁</span><span class="sxs-lookup"><span data-stu-id="d12f6-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="d12f6-116">若要加入排序學生索引頁，您要變更`Index`方法`Student`控制站，將程式碼加入`Student`索引檢視表。</span><span class="sxs-lookup"><span data-stu-id="d12f6-116">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="d12f6-117">加入排序功能 Index 方法</span><span class="sxs-lookup"><span data-stu-id="d12f6-117">Add Sorting Functionality to the Index Method</span></span>

<span data-ttu-id="d12f6-118">在*Controllers\StudentController.cs*，取代`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d12f6-118">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="d12f6-119">此程式碼會接收`sortOrder`從 URL 中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="d12f6-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="d12f6-120">查詢字串值是由 ASP.NET MVC 提供，做為參數至動作方法。</span><span class="sxs-lookup"><span data-stu-id="d12f6-120">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="d12f6-121">參數會是可選擇性地接著底線加上字串"desc"來指定遞減順序為"Name"Date"的字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-121">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="d12f6-122">預設的排序次序為遞增。</span><span class="sxs-lookup"><span data-stu-id="d12f6-122">The default sort order is ascending.</span></span>

<span data-ttu-id="d12f6-123">第一次要求的索引頁面時，沒有任何查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="d12f6-124">以遞增順序顯示學生`LastName`，在中斷的情況中所建立，這是預設`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-124">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="d12f6-125">當使用者按一下資料行標題超連結，適當`sortOrder`查詢字串中提供值。</span><span class="sxs-lookup"><span data-stu-id="d12f6-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="d12f6-126">這兩個`ViewBag`使用變數時，讓檢視可以設定適當的查詢字串值的資料行標題超連結：</span><span class="sxs-lookup"><span data-stu-id="d12f6-126">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="d12f6-127">這些是三元陳述式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-127">These are ternary statements.</span></span> <span data-ttu-id="d12f6-128">第一個指定，如果`sortOrder`參數為 null 或空的`ViewBag.NameSortParm`應該設定為 「 名稱\_desc"，否則應設為空字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-128">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="d12f6-129">這些兩個陳述式會啟用檢視設定的資料行標題的超連結，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d12f6-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="d12f6-130">目前的排序順序</span><span class="sxs-lookup"><span data-stu-id="d12f6-130">Current sort order</span></span> | <span data-ttu-id="d12f6-131">最後一個名稱的超連結</span><span class="sxs-lookup"><span data-stu-id="d12f6-131">Last Name Hyperlink</span></span> | <span data-ttu-id="d12f6-132">日期的超連結</span><span class="sxs-lookup"><span data-stu-id="d12f6-132">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d12f6-133">最後一個名稱升冪</span><span class="sxs-lookup"><span data-stu-id="d12f6-133">Last Name ascending</span></span> | <span data-ttu-id="d12f6-134">descending</span><span class="sxs-lookup"><span data-stu-id="d12f6-134">descending</span></span> | <span data-ttu-id="d12f6-135">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-135">ascending</span></span> |
| <span data-ttu-id="d12f6-136">最後一個遞減的名稱</span><span class="sxs-lookup"><span data-stu-id="d12f6-136">Last Name descending</span></span> | <span data-ttu-id="d12f6-137">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-137">ascending</span></span> | <span data-ttu-id="d12f6-138">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-138">ascending</span></span> |
| <span data-ttu-id="d12f6-139">遞增的日期</span><span class="sxs-lookup"><span data-stu-id="d12f6-139">Date ascending</span></span> | <span data-ttu-id="d12f6-140">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-140">ascending</span></span> | <span data-ttu-id="d12f6-141">descending</span><span class="sxs-lookup"><span data-stu-id="d12f6-141">descending</span></span> |
| <span data-ttu-id="d12f6-142">日期遞減</span><span class="sxs-lookup"><span data-stu-id="d12f6-142">Date descending</span></span> | <span data-ttu-id="d12f6-143">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-143">ascending</span></span> | <span data-ttu-id="d12f6-144">ascending</span><span class="sxs-lookup"><span data-stu-id="d12f6-144">ascending</span></span> |

<span data-ttu-id="d12f6-145">此方法會使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定排序所依據的資料行。</span><span class="sxs-lookup"><span data-stu-id="d12f6-145">The method uses [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) to specify the column to sort by.</span></span> <span data-ttu-id="d12f6-146">程式碼會建立[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)變數之前`switch`陳述式，修改在`switch`陳述式，並呼叫`ToList`方法之後`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-146">The code creates an [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="d12f6-147">當您建立和修改`IQueryable`變數沒有查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="d12f6-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="d12f6-148">查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。</span><span class="sxs-lookup"><span data-stu-id="d12f6-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="d12f6-149">因此，此程式碼會產生單一查詢，將會等到執行`return View`陳述式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="d12f6-150">替代方法來撰寫每個排序順序的不同 LINQ 陳述式中，您可以動態建立 LINQ 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-150">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="d12f6-151">如需動態 LINQ 資訊，請參閱[動態 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-151">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="d12f6-152">加入資料行標題學生索引檢視的超連結</span><span class="sxs-lookup"><span data-stu-id="d12f6-152">Add Column Heading Hyperlinks to the Student Index View</span></span>

<span data-ttu-id="d12f6-153">在*Views\Student\Index.cshtml*，取代`<tr>`和`<th>`標題資料列，以反白顯示的程式碼項目：</span><span class="sxs-lookup"><span data-stu-id="d12f6-153">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

<span data-ttu-id="d12f6-154">此程式碼使用中的資訊`ViewBag`屬性，以設定適當的查詢與超連結的字串值。</span><span class="sxs-lookup"><span data-stu-id="d12f6-154">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="d12f6-155">執行網頁，然後按一下**姓氏**和**註冊日期**以確認該排序的資料行標題的運作方式。</span><span class="sxs-lookup"><span data-stu-id="d12f6-155">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="d12f6-157">按一下 之後**姓氏**標題之下，學生顯示遞減最後一個的名稱。</span><span class="sxs-lookup"><span data-stu-id="d12f6-157">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="d12f6-158">學生索引頁面中新增搜尋方塊</span><span class="sxs-lookup"><span data-stu-id="d12f6-158">Add a Search Box to the Students Index Page</span></span>

<span data-ttu-id="d12f6-159">若要加入篩選學生索引頁，您會將文字方塊及提交按鈕加入至檢視，並進行中的對應變更`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="d12f6-159">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="d12f6-160">在文字方塊中，可讓您輸入要搜尋的名字和最後一個名稱欄位中的字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-160">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="d12f6-161">索引方法中加入篩選功能</span><span class="sxs-lookup"><span data-stu-id="d12f6-161">Add Filtering Functionality to the Index Method</span></span>

<span data-ttu-id="d12f6-162">在*Controllers\StudentController.cs*，取代`Index`（所做的變更會反白顯示） 為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="d12f6-162">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="d12f6-163">您已新增`searchString`參數`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="d12f6-163">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="d12f6-164">從文字方塊中，您會加入至索引檢視收到的搜尋字串值。</span><span class="sxs-lookup"><span data-stu-id="d12f6-164">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="d12f6-165">您也已經加入至 LINQ 陳述式`where`選取其名字或姓氏包含搜尋字串的學生的子句。</span><span class="sxs-lookup"><span data-stu-id="d12f6-165">You've also added to the LINQ statement a `where` clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="d12f6-166">加入陳述式[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句在沒有要搜尋的值時，才會執行。</span><span class="sxs-lookup"><span data-stu-id="d12f6-166">The statement that adds the [where](https://msdn.microsoft.com/library/bb535040.aspx) clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="d12f6-167">在許多情況下，Entity Framework 實體集上，或是當做擴充方法上的記憶體中集合，可以呼叫相同的方法。</span><span class="sxs-lookup"><span data-stu-id="d12f6-167">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="d12f6-168">結果通常都相同，但在某些情況下可能會不同。</span><span class="sxs-lookup"><span data-stu-id="d12f6-168">The results are normally the same but in some cases may be different.</span></span>
> 
> <span data-ttu-id="d12f6-169">例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回零個資料列空白字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-169">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="d12f6-170">因此在範例程式碼 (放`Where`陳述式內`if`陳述式) 可確保您取得所有版本的 SQL Server 相同的結果。</span><span class="sxs-lookup"><span data-stu-id="d12f6-170">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="d12f6-171">此外，.NET Framework 實作的`Contains`方法會根據預設，執行區分大小寫的比較，但預設實體架構的 SQL Server 提供者執行不區分大小寫的比較。</span><span class="sxs-lookup"><span data-stu-id="d12f6-171">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="d12f6-172">因此，呼叫`ToUpper`來進行測試更明確地不區分大小寫的方法可確保當您變更程式碼更新版本使用的儲存機制將會傳回結果不會變更`IEnumerable`集合，而不是`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="d12f6-172">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="d12f6-173">(當您呼叫`Contains`方法`IEnumerable`集合，取得.NET Framework 實作，則當您呼叫它在`IQueryable`物件，則會收到資料庫提供者實作。)</span><span class="sxs-lookup"><span data-stu-id="d12f6-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
> 
> <span data-ttu-id="d12f6-174">Null 處理也可能不同的資料庫提供者，或當您使用的`IQueryable`物件相較於，當您使用`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="d12f6-174">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="d12f6-175">例如，在某些情況下`Where`這類條件`table.Column != 0`可能不會傳回具有資料行`null`做為值。</span><span class="sxs-lookup"><span data-stu-id="d12f6-175">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="d12f6-176">如需詳細資訊，請參閱['where' 子句中的 null 變數不正確地處理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-176">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>


### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="d12f6-177">學生索引檢視中新增搜尋方塊</span><span class="sxs-lookup"><span data-stu-id="d12f6-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="d12f6-178">在*Views\Student\Index.cshtml*，將之前開啟反白顯示的程式碼加入`table`才能建立 標題 文字方塊中，標記和**搜尋** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d12f6-178">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

<span data-ttu-id="d12f6-179">執行頁面，輸入搜尋字串，然後按一下**搜尋**驗證篩選是否正在運作。</span><span class="sxs-lookup"><span data-stu-id="d12f6-179">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="d12f6-181">請注意 URL 未包含"an"搜尋字串，這表示，如果您加入書籤此頁面，您不會取得篩選的清單，當您使用書籤。</span><span class="sxs-lookup"><span data-stu-id="d12f6-181">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="d12f6-182">這也適用於資料行排序連結，因為它們會排序整個清單。</span><span class="sxs-lookup"><span data-stu-id="d12f6-182">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="d12f6-183">您要變更**搜尋** 按鈕，稍後在本教學課程中使用的篩選準則的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-183">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="d12f6-184">加入分頁學生索引頁</span><span class="sxs-lookup"><span data-stu-id="d12f6-184">Add Paging to the Students Index Page</span></span>

<span data-ttu-id="d12f6-185">若要加入分頁學生索引頁，您會先安裝**PagedList.Mvc** NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="d12f6-185">To add paging to the Students Index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="d12f6-186">然後您要進行其他變更的`Index`方法並新增到分頁連結`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="d12f6-186">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="d12f6-187">**PagedList.Mvc**許多良好分頁和排序封裝 ASP.NET mvc 的其中一個，而且它的使用只為例，不做為它的建議，透過其他選項。</span><span class="sxs-lookup"><span data-stu-id="d12f6-187">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="d12f6-188">下圖顯示分頁連結。</span><span class="sxs-lookup"><span data-stu-id="d12f6-188">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="d12f6-190">安裝 PagedList.MVC NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="d12f6-190">Install the PagedList.MVC NuGet Package</span></span>

<span data-ttu-id="d12f6-191">NuGet **PagedList.Mvc**套件會自動安裝**PagedList**封裝做為相依性。</span><span class="sxs-lookup"><span data-stu-id="d12f6-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="d12f6-192">**PagedList**封裝安裝`PagedList`集合型別和延伸模組方法`IQueryable`和`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="d12f6-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="d12f6-193">擴充方法建立單一頁面中的資料`PagedList`集合超出您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法，以便分頁。</span><span class="sxs-lookup"><span data-stu-id="d12f6-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="d12f6-194">**PagedList.Mvc**套件會安裝分頁的 helper 顯示分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="d12f6-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

<span data-ttu-id="d12f6-195">從**工具**功能表上，選取**程式庫套件管理員**然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="d12f6-195">From the **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>

<span data-ttu-id="d12f6-196">在**Package Manager Console**視窗中，請確定**套件來源**是**nuget.org**和**預設專案**是**ContosoUniversity**，然後輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="d12f6-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

`Install-Package PagedList.Mvc`

![Install PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="d12f6-198">建置專案。</span><span class="sxs-lookup"><span data-stu-id="d12f6-198">Build the project.</span></span> 

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="d12f6-199">將分頁功能加入至索引方法</span><span class="sxs-lookup"><span data-stu-id="d12f6-199">Add Paging Functionality to the Index Method</span></span>

<span data-ttu-id="d12f6-200">在*Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：</span><span class="sxs-lookup"><span data-stu-id="d12f6-200">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="d12f6-201">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="d12f6-201">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

<span data-ttu-id="d12f6-202">這個程式碼加入`page`參數、 目前的排序順序參數，以及目前的篩選參數的方法簽章：</span><span class="sxs-lookup"><span data-stu-id="d12f6-202">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="d12f6-203">第一次顯示網頁，或如果使用者尚未按一下分頁或排序連結，所有參數將會都是 null。</span><span class="sxs-lookup"><span data-stu-id="d12f6-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span> <span data-ttu-id="d12f6-204">按一下分頁連結時，如果`page`變數將包含要顯示的頁面數。</span><span class="sxs-lookup"><span data-stu-id="d12f6-204">If a paging link is clicked, the `page` variable will contain the page number to display.</span></span>

<span data-ttu-id="d12f6-205">A`ViewBag`屬性提供的檢視，且目前的排序次序中，因為這必須以保持在分頁時相同的排序順序包含在分頁連結：</span><span class="sxs-lookup"><span data-stu-id="d12f6-205">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="d12f6-206">另一個屬性`ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。</span><span class="sxs-lookup"><span data-stu-id="d12f6-206">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="d12f6-207">此值必須包含在分頁連結，以篩選設定維護期間分頁，而且它必須還原到文字方塊中時的頁面會重新顯示。</span><span class="sxs-lookup"><span data-stu-id="d12f6-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="d12f6-208">如果搜尋字串變更在分頁時，此頁面具有要重設為 1，因為新的篩選器可能會導致不同的資料，以顯示。</span><span class="sxs-lookup"><span data-stu-id="d12f6-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="d12f6-209">在文字方塊中輸入的值，並按下 [提交] 按鈕時，會變更搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-209">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="d12f6-210">在此情況下，`searchString`參數不是 null。</span><span class="sxs-lookup"><span data-stu-id="d12f6-210">In that case, the `searchString` parameter is not null.</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="d12f6-211">方法的結尾`ToPagedList`學生上的擴充方法`IQueryable`物件會將學生查詢轉換成單一頁面的學生支援分頁的集合型別。</span><span class="sxs-lookup"><span data-stu-id="d12f6-211">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="d12f6-212">學生的單一頁面再傳遞到檢視中：</span><span class="sxs-lookup"><span data-stu-id="d12f6-212">That single page of students is then passed to the view:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="d12f6-213">`ToPagedList`方法會採用頁面數目。</span><span class="sxs-lookup"><span data-stu-id="d12f6-213">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="d12f6-214">兩個問號代表[null 聯合運算子](https://msdn.microsoft.com/library/ms173224.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-214">The two question marks represent the [null-coalescing operator](https://msdn.microsoft.com/library/ms173224.aspx).</span></span> <span data-ttu-id="d12f6-215">Null 聯合運算子定義預設值為 null 的型別。運算式`(page ?? 1)`方法傳回的值`page`它具有的值，或傳回 1，如果`page`為 null。</span><span class="sxs-lookup"><span data-stu-id="d12f6-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="d12f6-216">將 Student 索引檢視中的分頁連結</span><span class="sxs-lookup"><span data-stu-id="d12f6-216">Add Paging Links to the Student Index View</span></span>

<span data-ttu-id="d12f6-217">在*Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d12f6-217">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="d12f6-218">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="d12f6-218">The changes are highlighted.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

<span data-ttu-id="d12f6-219">`@model`在頁面頂端的陳述式指定檢視現在取得`PagedList`物件而非`List`物件。</span><span class="sxs-lookup"><span data-stu-id="d12f6-219">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

<span data-ttu-id="d12f6-220">`using`陳述式`PagedList.Mvc`可讓 MVC 協助程式的存取 [分頁] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d12f6-220">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

<span data-ttu-id="d12f6-221">程式碼使用的多載[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，可讓它指定[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-221">The code uses an overload of [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) that allows it to specify [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

<span data-ttu-id="d12f6-222">預設值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表單與文章時，這表示，參數傳遞的 HTTP 訊息本文，而不是在 URL 查詢字串的形式的資料。</span><span class="sxs-lookup"><span data-stu-id="d12f6-222">The default [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="d12f6-223">當您指定 HTTP GET 時，表單資料會在 URL 中當做傳遞查詢字串，可讓使用者 URL 加入書籤。</span><span class="sxs-lookup"><span data-stu-id="d12f6-223">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="d12f6-224">[W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建議動作並不會導致更新時，您應該使用 GET。</span><span class="sxs-lookup"><span data-stu-id="d12f6-224">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="d12f6-225">在文字方塊中會使用目前的搜尋字串初始化，因此當您按一下新的頁面時，您就可以看到目前的搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="d12f6-225">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

<span data-ttu-id="d12f6-226">資料行頁首連結使用查詢字串，將目前的搜尋字串傳遞至控制器，以便使用者可以排序內篩選結果：</span><span class="sxs-lookup"><span data-stu-id="d12f6-226">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

<span data-ttu-id="d12f6-227">會顯示目前頁面和總頁數。</span><span class="sxs-lookup"><span data-stu-id="d12f6-227">The current page and total number of pages are displayed.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

<span data-ttu-id="d12f6-228">如果沒有顯示網頁，則會顯示頁面 0 的 0 」。</span><span class="sxs-lookup"><span data-stu-id="d12f6-228">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="d12f6-229">(在此情況下頁面數目大於頁面計數因為`Model.PageNumber`為 1，和`Model.PageCount`為 0。)</span><span class="sxs-lookup"><span data-stu-id="d12f6-229">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

<span data-ttu-id="d12f6-230">分頁按鈕會顯示`PagedListPager`helper:</span><span class="sxs-lookup"><span data-stu-id="d12f6-230">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="d12f6-231">`PagedListPager` Helper 提供數個選項，您可以自訂，包括 Url 和樣式設定。</span><span class="sxs-lookup"><span data-stu-id="d12f6-231">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="d12f6-232">如需詳細資訊，請參閱[TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 網站上。</span><span class="sxs-lookup"><span data-stu-id="d12f6-232">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

<span data-ttu-id="d12f6-233">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="d12f6-233">Run the page.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="d12f6-235">按一下分頁中的連結可確定分頁運作的不同排序順序。</span><span class="sxs-lookup"><span data-stu-id="d12f6-235">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="d12f6-236">然後輸入搜尋字串，然後再試一次以確認，分頁也會搭配正確排序和篩選的分頁。</span><span class="sxs-lookup"><span data-stu-id="d12f6-236">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="d12f6-237">建立相關頁面，其中顯示學生統計資料</span><span class="sxs-lookup"><span data-stu-id="d12f6-237">Create an About Page That Shows Student Statistics</span></span>

<span data-ttu-id="d12f6-238">Contoso 大學網站的相關頁面，您要顯示多少學生已註冊的每個註冊日期。</span><span class="sxs-lookup"><span data-stu-id="d12f6-238">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="d12f6-239">這需要在群組的群組和簡單計算。</span><span class="sxs-lookup"><span data-stu-id="d12f6-239">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="d12f6-240">若要這麼做，您會執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="d12f6-240">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="d12f6-241">建立您要傳遞至檢視資料的檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="d12f6-241">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="d12f6-242">修改`About`方法中的`Home`控制站。</span><span class="sxs-lookup"><span data-stu-id="d12f6-242">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="d12f6-243">修改`About`檢視。</span><span class="sxs-lookup"><span data-stu-id="d12f6-243">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="d12f6-244">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="d12f6-244">Create the View Model</span></span>

<span data-ttu-id="d12f6-245">建立*ViewModels*專案資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d12f6-245">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="d12f6-246">在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*和範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d12f6-246">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="d12f6-247">修改主控制器</span><span class="sxs-lookup"><span data-stu-id="d12f6-247">Modify the Home Controller</span></span>

<span data-ttu-id="d12f6-248">在*HomeController.cs*，加入下列`using`在檔案最上方的陳述式：</span><span class="sxs-lookup"><span data-stu-id="d12f6-248">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="d12f6-249">將資料庫內容的類別變數加入之後左大括號類別：</span><span class="sxs-lookup"><span data-stu-id="d12f6-249">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

<span data-ttu-id="d12f6-250">以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="d12f6-250">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="d12f6-251">LINQ 陳述式註冊日期分組的學生實體、 計算每個群組中的實體數目，並將結果儲存在集合中`EnrollmentDateGroup`檢視模型物件。</span><span class="sxs-lookup"><span data-stu-id="d12f6-251">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

<span data-ttu-id="d12f6-252">新增`Dispose`方法：</span><span class="sxs-lookup"><span data-stu-id="d12f6-252">Add a `Dispose` method:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="d12f6-253">修改檢視</span><span class="sxs-lookup"><span data-stu-id="d12f6-253">Modify the About View</span></span>

<span data-ttu-id="d12f6-254">中的程式碼取代*Views\Home\About.cshtml*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="d12f6-254">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

<span data-ttu-id="d12f6-255">執行應用程式，然後按一下**有關**連結。</span><span class="sxs-lookup"><span data-stu-id="d12f6-255">Run the app and click the **About** link.</span></span> <span data-ttu-id="d12f6-256">針對每個註冊日期的學生人數會顯示在資料表中。</span><span class="sxs-lookup"><span data-stu-id="d12f6-256">The count of students for each enrollment date is displayed in a table.</span></span>

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="d12f6-258">總結</span><span class="sxs-lookup"><span data-stu-id="d12f6-258">Summary</span></span>

<span data-ttu-id="d12f6-259">在本教學課程中，您已經看到如何建立資料模型及實作基本 CRUD 排序、 篩選、 分頁和分組功能。</span><span class="sxs-lookup"><span data-stu-id="d12f6-259">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="d12f6-260">在下一個教學課程中開始，您將查看更進階的主題，方法是展開的資料模型。</span><span class="sxs-lookup"><span data-stu-id="d12f6-260">In the next tutorial you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="d12f6-261">請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="d12f6-261">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="d12f6-262">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-262">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="d12f6-263">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="d12f6-263">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d12f6-264">[上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[下一頁](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="d12f6-264">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
