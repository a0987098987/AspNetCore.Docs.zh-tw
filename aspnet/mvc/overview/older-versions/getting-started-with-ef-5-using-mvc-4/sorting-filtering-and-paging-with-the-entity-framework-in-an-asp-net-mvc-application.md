---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (第 3 部 10) |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f5ce5f926021a53a5dd96e578b26b8c186c9a83c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841030"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a><span data-ttu-id="1c3e4-103">排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (第 3 部 10)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-103">Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application (3 of 10)</span></span>
====================
<span data-ttu-id="1c3e4-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1c3e4-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="1c3e4-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="1c3e4-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="1c3e4-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="1c3e4-108">您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="1c3e4-109">如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="1c3e4-110">您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="1c3e4-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="1c3e4-112">在上一個教學課程中，您實作的基本 CRUD 作業的 web 網頁的一組`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-112">In the previous tutorial you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="1c3e4-113">在本教學課程中，您將新增排序、 篩選和分頁功能**學生**索引頁面。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-113">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="1c3e4-114">此外，還要建立將執行簡易群組的頁面。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-114">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="1c3e4-115">下圖顯示當您完成時的頁面外觀。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-115">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="1c3e4-116">資料行標題是使用者可以按一下以依據該資料行排序的連結。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-116">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="1c3e4-117">重覆按一下資料行標題，可切換遞增和遞減排序次序。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-117">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="1c3e4-119">將資料行排序連結新增至 Students 的 [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="1c3e4-119">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="1c3e4-120">若要將排序新增至學生的 [索引] 頁面，您將會變更`Index`方法`Student`控制器並將程式碼加入`Student`索引檢視。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-120">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="1c3e4-121">加入排序功能至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="1c3e4-121">Add Sorting Functionality to the Index Method</span></span>

<span data-ttu-id="1c3e4-122">在  *Controllers\StudentController.cs*，取代`Index`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-122">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="1c3e4-123">此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-123">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="1c3e4-124">查詢字串值是由 ASP.NET MVC 提供，做為動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-124">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="1c3e4-125">該參數是 "Name" 或 "Date" 的字串，後面可選擇性地接著底線和字串 "desc" 來指定遞減順序。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-125">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="1c3e4-126">預設的排序次序為遞增。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-126">The default sort order is ascending.</span></span>

<span data-ttu-id="1c3e4-127">第一次要求 [索引] 頁面時，沒有任何查詢字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-127">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="1c3e4-128">以遞增順序顯示學生`LastName`，這是預設值中的 fallthrough 案例所建立`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-128">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="1c3e4-129">使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-129">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="1c3e4-130">這兩個`ViewBag`使用變數時，讓檢視可以使用適當的查詢字串值來設定資料行標題超連結：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-130">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="1c3e4-131">這些是三元陳述式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-131">These are ternary statements.</span></span> <span data-ttu-id="1c3e4-132">第一個指定，如果`sortOrder`參數為 null 或空的`ViewBag.NameSortParm`應設為 「 名稱\_desc"，否則它應該設定為空字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-132">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="1c3e4-133">這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-133">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="1c3e4-134">目前排序次序</span><span class="sxs-lookup"><span data-stu-id="1c3e4-134">Current sort order</span></span> | <span data-ttu-id="1c3e4-135">姓氏超連結</span><span class="sxs-lookup"><span data-stu-id="1c3e4-135">Last Name Hyperlink</span></span> | <span data-ttu-id="1c3e4-136">日期超連結</span><span class="sxs-lookup"><span data-stu-id="1c3e4-136">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c3e4-137">姓氏遞增</span><span class="sxs-lookup"><span data-stu-id="1c3e4-137">Last Name ascending</span></span> | <span data-ttu-id="1c3e4-138">descending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-138">descending</span></span> | <span data-ttu-id="1c3e4-139">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-139">ascending</span></span> |
| <span data-ttu-id="1c3e4-140">姓氏遞減</span><span class="sxs-lookup"><span data-stu-id="1c3e4-140">Last Name descending</span></span> | <span data-ttu-id="1c3e4-141">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-141">ascending</span></span> | <span data-ttu-id="1c3e4-142">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-142">ascending</span></span> |
| <span data-ttu-id="1c3e4-143">日期遞增</span><span class="sxs-lookup"><span data-stu-id="1c3e4-143">Date ascending</span></span> | <span data-ttu-id="1c3e4-144">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-144">ascending</span></span> | <span data-ttu-id="1c3e4-145">descending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-145">descending</span></span> |
| <span data-ttu-id="1c3e4-146">日期遞減</span><span class="sxs-lookup"><span data-stu-id="1c3e4-146">Date descending</span></span> | <span data-ttu-id="1c3e4-147">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-147">ascending</span></span> | <span data-ttu-id="1c3e4-148">ascending</span><span class="sxs-lookup"><span data-stu-id="1c3e4-148">ascending</span></span> |

<span data-ttu-id="1c3e4-149">此方法會使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定排序所依據的資料行。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-149">The method uses [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) to specify the column to sort by.</span></span> <span data-ttu-id="1c3e4-150">程式碼會建立[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)變數之前`switch`陳述式，會修改它`switch`陳述式以及呼叫`ToList`方法之後`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-150">The code creates an [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="1c3e4-151">當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-151">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="1c3e4-152">查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-152">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="1c3e4-153">因此，此程式碼會產生之前不會執行單一查詢`return View`陳述式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-153">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="1c3e4-154">新增資料行標題超連結至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="1c3e4-154">Add Column Heading Hyperlinks to the Student Index View</span></span>

<span data-ttu-id="1c3e4-155">在  *Views\Student\Index.cshtml*，取代`<tr>`和`<th>`醒目提示的程式碼的標題資料列的項目：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-155">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

<span data-ttu-id="1c3e4-156">此程式碼使用中的資訊`ViewBag`屬性來設定超連結，以適當的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-156">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="1c3e4-157">執行頁面，然後按一下**姓氏**並**註冊日期**資料行標題，以確認該排序運作。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-157">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="1c3e4-159">按一下 之後**姓氏**標題之下，學生以遞減顯示姓氏來排序。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-159">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="1c3e4-160">在搜尋方塊新增至 Students [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="1c3e4-160">Add a Search Box to the Students Index Page</span></span>

<span data-ttu-id="1c3e4-161">若要將篩選新增至 Students 的 [索引] 頁面，您要將文字方塊和提交按鈕新增至檢視，並在 `Index` 方法中進行對應的變更。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-161">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="1c3e4-162">文字方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-162">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="1c3e4-163">將篩選功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="1c3e4-163">Add Filtering Functionality to the Index Method</span></span>

<span data-ttu-id="1c3e4-164">在  *Controllers\StudentController.cs*，取代`Index`（變更已醒目提示） 為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-164">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="1c3e4-165">您已將 `searchString` 參數新增至 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-165">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="1c3e4-166">您也已加入 LINQ 陳述式`where`clausethat 選取其名字或姓氏包含搜尋字串的學生。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-166">You've also added to the LINQ statement a `where` clausethat selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="1c3e4-167">從文字方塊中，您會加入至 [索引] 檢視接收搜尋字串值。加入陳述式[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句在沒有要搜尋的值時，才會執行。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-167">The search string value is received from a text box that you'll add to the Index view.The statement that adds the [where](https://msdn.microsoft.com/library/bb535040.aspx) clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="1c3e4-168">在許多情況下，Entity Framework 實體集上，或是當做擴充方法在記憶體中的集合，可以呼叫相同的方法。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-168">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="1c3e4-169">結果通常都相同，但在某些情況下可能會不同。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-169">The results are normally the same but in some cases may be different.</span></span> <span data-ttu-id="1c3e4-170">例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回空字串的零個資料列。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-170">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="1c3e4-171">因此在範例程式碼 (將放`Where`內的陳述式`if`陳述式) 可確保您取得所有的 SQL Server 版本相同的結果。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-171">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="1c3e4-172">此外，.NET Framework 實作的`Contains`方法依預設，執行區分大小寫比較，但 Entity Framework 的 SQL Server 提供者預設情況下執行不區分大小寫的比較。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-172">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="1c3e4-173">因此，呼叫`ToUpper`若要使測試明確不區分大小寫的方法可確保當您變更程式碼稍後使用的儲存機制，這會傳回結果不會變更`IEnumerable`而不是集合`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-173">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="1c3e4-174">(當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>


### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="1c3e4-175">將搜尋方塊新增至學生的 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="1c3e4-175">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="1c3e4-176">在*Views\Student\Index.cshtml*，新增醒目提示的程式碼開頭之前，立即`table`標記，以建立標題，文字方塊中，和**搜尋** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-176">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

<span data-ttu-id="1c3e4-177">執行頁面，輸入搜尋字串，然後按一下**搜尋**以確認篩選可以運作。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-177">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="1c3e4-179">請注意 URL 未包含"an"搜尋字串，這表示，如果您將本頁加入標籤，您無法取得篩選的清單，當您使用書籤。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-179">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="1c3e4-180">您將會變更**搜尋**来用於篩選準則中的查詢字串，稍後在本教學課程中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-180">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="1c3e4-181">將分頁新增至 Students [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="1c3e4-181">Add Paging to the Students Index Page</span></span>

<span data-ttu-id="1c3e4-182">若要將分頁新增至 Students [索引] 頁面中，您會先安裝**PagedList.Mvc** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-182">To add paging to the Students Index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="1c3e4-183">然後您將進行中的其他變更`Index`方法，並新增到分頁連結`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-183">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="1c3e4-184">**PagedList.Mvc**許多很好的分頁和排序封裝的 ASP.NET MVC 的其中一個，而且它的使用僅為例，不做為它的建議，透過其他選項。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-184">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="1c3e4-185">下圖顯示分頁連結。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-185">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="1c3e4-187">安裝 PagedList.MVC NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="1c3e4-187">Install the PagedList.MVC NuGet Package</span></span>

<span data-ttu-id="1c3e4-188">NuGet **PagedList.Mvc**會自動安裝套件**PagedList**套件作為相依性。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-188">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="1c3e4-189">**PagedList**封裝會安裝`PagedList`集合型別和擴充功能方法`IQueryable`和`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-189">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="1c3e4-190">擴充方法建立單一頁面中的資料`PagedList`共收集您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法可簡化分頁。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-190">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="1c3e4-191">**PagedList.Mvc**套件會安裝的分頁的協助程式會顯示分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-191">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

<span data-ttu-id="1c3e4-192">從**工具**功能表上，選取**程式庫套件管理員**，然後**管理方案的 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-192">From the **Tools** menu, select **Library Package Manager** and then **Manage NuGet Packages for Solution**.</span></span>

<span data-ttu-id="1c3e4-193">在 **管理 NuGet 套件** 對話方塊中，按一下**線上**左側索引標籤，然後在 搜尋 方塊中輸入 「 分頁 」。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-193">In the **Manage NuGet Packages** dialog box, click the **Online** tab on the left and then enter "paged" in the search box.</span></span> <span data-ttu-id="1c3e4-194">當您看到**PagedList.Mvc**套件，按一下 **安裝**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-194">When you see the **PagedList.Mvc** package, click **Install**.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="1c3e4-195">在 **選取專案**方塊中，按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-195">In the **Select Projects** box, click **OK**.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="1c3e4-196">將分頁功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="1c3e4-196">Add Paging Functionality to the Index Method</span></span>

<span data-ttu-id="1c3e4-197">在  *Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-197">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="1c3e4-198">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-198">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="1c3e4-199">這個程式碼加入`page`參數、 目前的排序次序參數和目前的篩選參數至方法簽章，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-199">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature, as shown here:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="1c3e4-200">第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-200">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span> <span data-ttu-id="1c3e4-201">如果按一下分頁連結，`page`變數將包含要顯示的頁碼。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-201">If a paging link is clicked, the `page` variable will contain the page number to display.</span></span>

<span data-ttu-id="1c3e4-202">`A ViewBag` 屬性會提供目前的排序次序中，使用的檢視，因為這必須包含在分頁連結，才能保留分頁時相同的排序次序：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-202">`A ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="1c3e4-203">另一個屬性， `ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-203">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="1c3e4-204">此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-204">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="1c3e4-205">如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-205">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="1c3e4-206">在文字方塊中輸入值，並按下 [提交] 按鈕會變更搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-206">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="1c3e4-207">在此情況下，`searchString`參數不是 null。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-207">In that case, the `searchString` parameter is not null.</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="1c3e4-208">方法的結尾`ToPagedList`擴充方法給學生們`IQueryable`物件將學生查詢轉換成支援分頁的集合類型的單一頁面。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-208">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="1c3e4-209">該學生單一頁面接著會傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-209">That single page of students is then passed to the view:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="1c3e4-210">`ToPagedList` 方法會採用頁面數。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-210">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="1c3e4-211">兩個問號代表[null 聯合運算子](https://msdn.microsoft.com/library/ms173224.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-211">The two question marks represent the [null-coalescing operator](https://msdn.microsoft.com/library/ms173224.aspx).</span></span> <span data-ttu-id="1c3e4-212">Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-212">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="1c3e4-213">將分頁連結新增至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="1c3e4-213">Add Paging Links to the Student Index View</span></span>

<span data-ttu-id="1c3e4-214">在  *Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-214">In *Views\Student\Index.cshtml*, replace the existing code with the following code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

<span data-ttu-id="1c3e4-215">頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-215">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

<span data-ttu-id="1c3e4-216">`using`陳述式`PagedList.Mvc`分頁按鈕讓 MVC 協助程式的存取。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-216">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

<span data-ttu-id="1c3e4-217">此程式碼使用的多載[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)可指定[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-217">The code uses an overload of [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) that allows it to specify [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

<span data-ttu-id="1c3e4-218">預設值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)文章時，這表示參數會在 HTTP 訊息本文中並不是以 URL 傳遞作為查詢字串使用提交表單資料。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-218">The default [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="1c3e4-219">當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-219">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="1c3e4-220">[W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)指定動作不會產生更新時應使用 GET。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-220">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) specify that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="1c3e4-221">文字方塊會初始化與目前的搜尋字串中，當您按一下新的頁面時，您就可以看到目前的搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-221">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

<span data-ttu-id="1c3e4-222">資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-222">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

<span data-ttu-id="1c3e4-223">會顯示目前頁面和總頁數。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-223">The current page and total number of pages are displayed.</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

<span data-ttu-id="1c3e4-224">如果不沒有顯示任何頁面，會顯示 「 頁面 0 的 0 」。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-224">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="1c3e4-225">(在此情況下頁編號大於頁面計數因為`Model.PageNumber`為 1，和`Model.PageCount`為 0。)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-225">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

<span data-ttu-id="1c3e4-226">分頁按鈕會顯示`PagedListPager`協助程式：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-226">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="1c3e4-227">`PagedListPager`協助程式提供數個選項，您可以自訂，包括 Url 和樣式設定。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-227">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="1c3e4-228">如需詳細資訊，請參閱 < [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 網站上。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-228">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

<span data-ttu-id="1c3e4-229">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-229">Run the page.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="1c3e4-231">以不同排序次序按一下分頁連結，以確定分頁運作正常。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-231">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="1c3e4-232">然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-232">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="1c3e4-233">建立顯示學生統計資料的頁面</span><span class="sxs-lookup"><span data-stu-id="1c3e4-233">Create an About Page That Shows Student Statistics</span></span>

<span data-ttu-id="1c3e4-234">Contoso 大學網站的相關頁面，您將會顯示多少學生註冊每個註冊日期。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-234">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="1c3e4-235">這需要對群組進行分組和簡單計算。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-235">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="1c3e4-236">若要完成此工作，您需要執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-236">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="1c3e4-237">針對您需要傳遞至檢視的資料，建立檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-237">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="1c3e4-238">修改`About`方法中的`Home`控制站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-238">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="1c3e4-239">修改`About`檢視。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-239">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="1c3e4-240">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="1c3e4-240">Create the View Model</span></span>

<span data-ttu-id="1c3e4-241">建立*Viewmodel*資料夾。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-241">Create a *ViewModels* folder.</span></span> <span data-ttu-id="1c3e4-242">在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-242">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="1c3e4-243">修改 Home 控制器</span><span class="sxs-lookup"><span data-stu-id="1c3e4-243">Modify the Home Controller</span></span>

<span data-ttu-id="1c3e4-244">在  *HomeController.cs*，新增下列`using`陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-244">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="1c3e4-245">此類別的左大括弧之後立即新增資料庫內容的類別變數：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-245">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

<span data-ttu-id="1c3e4-246">以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-246">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

<span data-ttu-id="1c3e4-247">LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-247">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

<span data-ttu-id="1c3e4-248">新增`Dispose`方法：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-248">Add a `Dispose` method:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="1c3e4-249">修改 About 檢視</span><span class="sxs-lookup"><span data-stu-id="1c3e4-249">Modify the About View</span></span>

<span data-ttu-id="1c3e4-250">中的程式碼取代*Views\Home\About.cshtml*為下列程式碼的檔案：</span><span class="sxs-lookup"><span data-stu-id="1c3e4-250">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

<span data-ttu-id="1c3e4-251">執行應用程式，然後按一下**關於**連結。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-251">Run the app and click the **About** link.</span></span> <span data-ttu-id="1c3e4-252">每個註冊日期的學生人數將會顯示在資料表中。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-252">The count of students for each enrollment date is displayed in a table.</span></span>

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a><span data-ttu-id="1c3e4-254">選擇性： 將應用程式部署至 Windows Azure</span><span class="sxs-lookup"><span data-stu-id="1c3e4-254">Optional: Deploy the app to Windows Azure</span></span>

<span data-ttu-id="1c3e4-255">到目前為止您的應用程式已經執行本機 IIS Express 中的開發電腦上。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-255">So far your application has been running locally in IIS Express on your development computer.</span></span> <span data-ttu-id="1c3e4-256">若要使其可供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-256">To make it available for other people to use over the Internet, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="1c3e4-257">選擇性的本節的教學課程中您會將它部署至 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-257">In this optional section of the tutorial you'll deploy it to a Windows Azure Web Site.</span></span>

### <a name="using-code-first-migrations-to-deploy-the-database"></a><span data-ttu-id="1c3e4-258">使用 Code First 移轉，將資料庫部署</span><span class="sxs-lookup"><span data-stu-id="1c3e4-258">Using Code First Migrations to Deploy the Database</span></span>

<span data-ttu-id="1c3e4-259">若要將資料庫部署中，您將使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-259">To deploy the database you'll use Code First Migrations.</span></span> <span data-ttu-id="1c3e4-260">當您建立發行設定檔，您用來設定從 Visual Studio 部署的設定時，您會選取標示的核取方塊**Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-260">When you create the publish profile that you use to configure settings for deploying from Visual Studio, you'll select a check box that is labeled **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="1c3e4-261">此設定會使部署程序會自動設定應用程式*Web.config*檔案在目的地伺服器上，讓 Code First 會使用`MigrateDatabaseToLatestVersion`初始設定式類別。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-261">This setting causes the deployment process to automatically configure the application *Web.config* file on the destination server so that Code First uses the `MigrateDatabaseToLatestVersion` initializer class.</span></span>

<span data-ttu-id="1c3e4-262">Visual Studio 不會在部署程序執行與資料庫的任何項目。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-262">Visual Studio does not do anything with the database during the deployment process.</span></span> <span data-ttu-id="1c3e4-263">當部署的應用程式來存取資料庫第一次部署之後時，Code First 自動建立資料庫或更新至最新版本的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-263">When the deployed application accesses the database for the first time after deployment, Code First automatically creates the database or updates the database schema to the latest version.</span></span> <span data-ttu-id="1c3e4-264">如果應用程式實作移轉`Seed`方法，在方法執行之後建立資料庫，或在更新結構描述。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-264">If the application implements a Migrations `Seed` method, the method runs after the database is created or the schema is updated.</span></span>

<span data-ttu-id="1c3e4-265">移轉`Seed`方法會將測試資料。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-265">Your Migrations `Seed` method inserts test data.</span></span> <span data-ttu-id="1c3e4-266">如果您已部署至生產環境中，您必須變更`Seed`方法，讓它只會插入您想要插入到您的生產資料庫的資料。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-266">If you were deploying to a production environment, you would have to change the `Seed` method so that it only inserts data that you want to be inserted into your production database.</span></span> <span data-ttu-id="1c3e4-267">比方說，您目前的資料模型中可能會想要在開發資料庫的實際課程但虛構的學生。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-267">For example, in your current data model you might want to have real courses but fictional students in the development database.</span></span> <span data-ttu-id="1c3e4-268">您可以撰寫`Seed`負載皆在開發中，並再標記為註解虛構的學生在部署至生產環境之前的方法。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-268">You can write a `Seed` method to load both in development, and then comment out the fictional students before you deploy to production.</span></span> <span data-ttu-id="1c3e4-269">或者您可以撰寫`Seed`方法來載入只有課程，並使用應用程式的 UI 手動輸入虛構的學生在測試資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-269">Or you can write a `Seed` method to load only courses, and enter the fictional students in the test database manually by using the application's UI.</span></span>

### <a name="get-a-windows-azure-account"></a><span data-ttu-id="1c3e4-270">取得 Windows Azure 帳戶</span><span class="sxs-lookup"><span data-stu-id="1c3e4-270">Get a Windows Azure account</span></span>

<span data-ttu-id="1c3e4-271">您必須在 Windows Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-271">You'll need a Windows Azure account.</span></span> <span data-ttu-id="1c3e4-272">如果您還沒有做，您可以建立免費的試用帳戶，只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-272">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1c3e4-273">如需詳細資訊，請參閱 < [Windows Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-273">For details, see [Windows Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a><span data-ttu-id="1c3e4-274">在 Windows Azure 中建立網站和 SQL database</span><span class="sxs-lookup"><span data-stu-id="1c3e4-274">Create a web site and a SQL database in Windows Azure</span></span>

<span data-ttu-id="1c3e4-275">Windows Azure 網站會在共用主控環境中，這表示它會在與其他 Windows Azure 用戶端所共用的虛擬機器 (Vm) 上執行。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-275">Your Windows Azure Web Site will run in a shared hosting environment, which means it runs on virtual machines (VMs) that are shared with other Windows Azure clients.</span></span> <span data-ttu-id="1c3e4-276">共用主控環境是開始在雲端中的低成本方法。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-276">A shared hosting environment is a low-cost way to get started in the cloud.</span></span> <span data-ttu-id="1c3e4-277">稍後，如果您的 web 流量增加，應用程式可擴充至專用的 Vm 上執行依需求。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-277">Later, if your web traffic increases, the application can scale to meet the need by running on dedicated VMs.</span></span> <span data-ttu-id="1c3e4-278">如果您需要更複雜的架構，您可以移轉至 Windows Azure 雲端服務。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-278">If you need a more complex architecture, you can migrate to a Windows Azure Cloud Service.</span></span> <span data-ttu-id="1c3e4-279">您可以根據您的需求設定的專用 Vm 上，執行雲端服務。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-279">Cloud services run on dedicated VMs that you can configure according to your needs.</span></span>

<span data-ttu-id="1c3e4-280">Windows Azure SQL Database 是建置在 SQL Server 技術的雲端型關聯式資料庫服務。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-280">Windows Azure SQL Database is a cloud-based relational database service that is built on SQL Server technologies.</span></span> <span data-ttu-id="1c3e4-281">工具和使用 SQL Server 的應用程式也可以使用 SQL Database。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-281">Tools and applications that work with SQL Server also work with SQL Database.</span></span>

1. <span data-ttu-id="1c3e4-282">在  [Windows Azure 管理入口網站](https://manage.windowsazure.com/)，按一下**網站**中的索引標籤，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-282">In the [Windows Azure Management Portal](https://manage.windowsazure.com/), click **Web Sites** in the left tab, and then click **New**.</span></span>

    ![在管理入口網站中的新按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. <span data-ttu-id="1c3e4-284">按一下 **自訂建立**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-284">Click **CUSTOM CREATE**.</span></span>

    ![建立具有在管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   <span data-ttu-id="1c3e4-286">**新的網站-自訂建立**精靈 隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-286">The **New Web Site - Custom Create** wizard opens.</span></span>
3. <span data-ttu-id="1c3e4-287">在  **New Web Site**步驟在精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-287">In the **New Web Site** step of the wizard, enter a string in the **URL** box to use as the unique URL for your application.</span></span> <span data-ttu-id="1c3e4-288">完整的 URL 將會包含您在此處輸入，加上尾碼，您會看到的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-288">The complete URL will consist of what you enter here plus the suffix that you see next to the text box.</span></span> <span data-ttu-id="1c3e4-289">下圖顯示 「 ConU"，但該 URL 可能會讓您將必須選擇其他名稱。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-289">The illustration shows "ConU", but that URL is probably taken so you will have to choose a different one.</span></span>

    ![建立具有在管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. <span data-ttu-id="1c3e4-291">在 **地區**下拉式清單中，選擇接近您的區域。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-291">In the **Region** drop-down list, choose a region close to you.</span></span> <span data-ttu-id="1c3e4-292">此設定會指定您的網站會以哪個資料中心。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-292">This setting specifies which data center your web site will run in.</span></span>
5. <span data-ttu-id="1c3e4-293">在 **資料庫**下拉式清單中，選擇**建立免費的 20MB SQL 資料庫**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-293">In the **Database** drop-down list, choose **Create a free 20 MB SQL database**.</span></span>

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. <span data-ttu-id="1c3e4-294">在  **DB 連接字串名稱**，輸入*SchoolContext*。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-294">In the **DB CONNECTION STRING NAME**, enter *SchoolContext*.</span></span>

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. <span data-ttu-id="1c3e4-295">按一下底部的方塊右邊的箭號。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-295">Click the arrow that points to the right at the bottom of the box.</span></span> <span data-ttu-id="1c3e4-296">精靈會進入**資料庫設定**步驟。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-296">The wizard advances to the **Database Settings** step.</span></span>
8. <span data-ttu-id="1c3e4-297">在 **名稱**方塊中，輸入*ContosoUniversityDB*。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-297">In the **Name** box, enter *ContosoUniversityDB*.</span></span>
9. <span data-ttu-id="1c3e4-298">在 **伺服器**方塊中，選取**新的 SQL Database 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-298">In the **Server** box, select **New SQL Database server**.</span></span> <span data-ttu-id="1c3e4-299">或者，如果您先前建立的伺服器，您可以從下拉式清單中選取該伺服器。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-299">Alternatively, if you previously created a server, you can select that server from the drop-down list.</span></span>
10. <span data-ttu-id="1c3e4-300">輸入系統管理員**登入名稱**並**密碼**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-300">Enter an administrator **LOGIN NAME** and **PASSWORD**.</span></span> <span data-ttu-id="1c3e4-301">如果您選取**新的 SQL Database 伺服器**您不要在此輸入現有的名稱和密碼，您輸入新名稱和密碼，您現在定義以供未來存取資料庫時使用。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-301">If you selected **New SQL Database server** you aren't entering an existing name and password here, you're entering a new name and password that you're defining now to use later when you access the database.</span></span> <span data-ttu-id="1c3e4-302">如果您選取您先前建立的伺服器，您要輸入認證，該伺服器。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-302">If you selected a server that you created previously, you'll enter credentials for that server.</span></span> <span data-ttu-id="1c3e4-303">本教學課程中，您將不會選取***進階***核取方塊。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-303">For this tutorial, you won't select the ***Advanced*** check box.</span></span> <span data-ttu-id="1c3e4-304">***進階***選項可讓您將資料庫設為[定序](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-304">The ***Advanced*** options enable you to set the database [collation](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).</span></span>
11. <span data-ttu-id="1c3e4-305">選擇相同**地區**您選擇的網站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-305">Choose the same **Region** that you chose for the web site.</span></span>
12. <span data-ttu-id="1c3e4-306">按一下核取記號在右下方的方塊以表示您已完成。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-306">Click the check mark at the bottom right of the box to indicate that you're finished.</span></span>   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    <span data-ttu-id="1c3e4-308">下圖顯示使用現有的 SQL Server 和登入。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-308">The following image shows using an existing SQL Server and Login.</span></span>   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    <span data-ttu-id="1c3e4-310">在管理入口網站會回到 網站 頁面中，而**狀態**欄顯示 正在建立網站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-310">The Management Portal returns to the Web Sites page, and the **Status** column shows that the site is being created.</span></span> <span data-ttu-id="1c3e4-311">一段時間之後 （通常少於一分鐘），**狀態**資料行會顯示已成功建立網站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-311">After a while (typically less than a minute), the **Status** column shows that the site was successfully created.</span></span> <span data-ttu-id="1c3e4-312">在左側導覽列中，您會在您的帳戶的網站數目旁會出現**Web Sites**圖示，然後資料庫數目旁會出現**SQL 資料庫**圖示。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-312">In the navigation bar at the left, the number of sites you have in your account appears next to the **Web Sites** icon, and the number of databases appears next to the **SQL Databases** icon.</span></span>

## <a name="deploy-the-application-to-windows-azure"></a><span data-ttu-id="1c3e4-313">部署至 Windows Azure 應用程式</span><span class="sxs-lookup"><span data-stu-id="1c3e4-313">Deploy the application to Windows Azure</span></span>

1. <span data-ttu-id="1c3e4-314">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-314">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![在專案操作功能表中發行](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. <span data-ttu-id="1c3e4-316">在 **設定檔** 索引標籤**發佈 Web**精靈 中，按一下**匯入**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-316">In the **Profile** tab of the **Publish Web** wizard, click **Import**.</span></span>  
  
    ![匯入發行設定](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. <span data-ttu-id="1c3e4-318">如果您未在 Visual Studio 中先前新增 Windows Azure 訂用帳戶，請執行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-318">If you have not previously added your Windows Azure subscription in Visual Studio, perform the following steps.</span></span> <span data-ttu-id="1c3e4-319">在這些步驟中您新增您的訂用帳戶以便下拉式清單底下**從 Windows Azure 網站上的匯入**會包含您的網站。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-319">In these steps you add your subscription so that the drop-down list under **Import from a Windows Azure web site** will include your web site.</span></span>

    <span data-ttu-id="1c3e4-320">a.</span><span class="sxs-lookup"><span data-stu-id="1c3e4-320">a.</span></span> <span data-ttu-id="1c3e4-321">在 **匯入發行設定檔** 對話方塊中，按一下**從 Windows Azure 網站上的匯入**，然後按一下 **新增 Windows Azure 訂用帳戶**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-321">In the **Import Publish Profile** dialog box, click **Import from a Windows Azure web site**, and then click **Add Windows Azure subscription**.</span></span>

    ![新增 Windows Azure 訂用帳戶](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    <span data-ttu-id="1c3e4-323">b.</span><span class="sxs-lookup"><span data-stu-id="1c3e4-323">b.</span></span> <span data-ttu-id="1c3e4-324">在 [**匯入 Windows Azure 訂用帳戶**] 對話方塊中，按一下**下載訂用帳戶檔案**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-324">In the **Import Windows Azure Subscriptions** dialog box, click **Download subscription file**.</span></span>

    ![下載訂閱檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    <span data-ttu-id="1c3e4-326">c. </span><span class="sxs-lookup"><span data-stu-id="1c3e4-326">c.</span></span> <span data-ttu-id="1c3e4-327">在瀏覽器視窗中，儲存 *.publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-327">In your browser window, save the *.publishsettings* file.</span></span>

    ![下載.publishsettings 檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > <span data-ttu-id="1c3e4-329">安全性- *publishsettings*檔案包含您的認證 （未編碼），用來管理您的 Windows Azure 訂用帳戶和服務。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-329">Security - The *publishsettings* file contains your credentials (unencoded) that are used to administer your Windows Azure subscriptions and services.</span></span> <span data-ttu-id="1c3e4-330">此檔案的安全性最佳作法是暫時儲存在來源目錄之外 (秷濆 菾*Libraries\Documents*資料夾)，然後再匯入完成後刪除它。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-330">The security best practice for this file is to store it temporarily outside your source directories (for example in the *Libraries\Documents* folder), and then delete it once the import has completed.</span></span> <span data-ttu-id="1c3e4-331">取得存取權的惡意使用者`.publishsettings`檔案可以編輯、 建立和刪除您的 Windows Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-331">A malicious user who gains access to the `.publishsettings` file can edit, create, and delete your Windows Azure services.</span></span>

    <span data-ttu-id="1c3e4-332">d.</span><span class="sxs-lookup"><span data-stu-id="1c3e4-332">d.</span></span> <span data-ttu-id="1c3e4-333">在 [**匯入 Windows Azure 訂用帳戶**] 對話方塊中，按一下**瀏覽**並瀏覽至 *.publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-333">In the **Import Windows Azure Subscriptions** dialog box, click **Browse** and navigate to the *.publishsettings* file.</span></span>

    ![下載 sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    <span data-ttu-id="1c3e4-335">e.</span><span class="sxs-lookup"><span data-stu-id="1c3e4-335">e.</span></span> <span data-ttu-id="1c3e4-336">按一下 [匯入] 。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-336">Click **Import**.</span></span>

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. <span data-ttu-id="1c3e4-338">在 **匯入發行設定檔**對話方塊中，選取**從 Windows Azure 網站上的匯入**，從下拉式清單中，選取您的網站，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-338">In the **Import Publish Profile** dialog box, select **Import from a Windows Azure web site**, select your web site from the drop-down list, and then click **OK**.</span></span>  
  
    ![匯入發行設定檔](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. <span data-ttu-id="1c3e4-340">在 **連接**索引標籤上，按一下**驗證連線**藉此確定設定正確無誤。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-340">In the **Connection** tab, click **Validate Connection** to make sure that the settings are correct.</span></span>  
  
    ![驗證連線](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. <span data-ttu-id="1c3e4-342">當已驗證的連接時旁, 顯示綠色的核取記號**驗證連線** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-342">When the connection has been validated, a green check mark is shown next to the **Validate Connection** button.</span></span> <span data-ttu-id="1c3e4-343">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-343">Click **Next**.</span></span>  
  
    ![已成功驗證的連接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. <span data-ttu-id="1c3e4-345">開啟**遠端的連接字串**下方的下拉式清單**SchoolContext** ，然後選取您建立的資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-345">Open the **Remote connection string** drop-down list under **SchoolContext** and select the connection string for the database you created.</span></span>
8. <span data-ttu-id="1c3e4-346">選取  **Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-346">Select **Execute Code First Migrations (runs on application start)**.</span></span>
9. <span data-ttu-id="1c3e4-347">取消核取**在執行階段使用此連接字串**for **UserContext (DefaultConnection)**，因為此應用程式未使用成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-347">Uncheck **Use this connection string at runtime** for the **UserContext (DefaultConnection)**, since this application is not using the membership database.</span></span>   
  
    ![設定 索引標籤](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. <span data-ttu-id="1c3e4-349">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-349">Click **Next**.</span></span>
11. <span data-ttu-id="1c3e4-350">在  **Preview**索引標籤上，按一下**開始預覽**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-350">In the **Preview** tab, click **Start Preview**.</span></span>  
  
    ![[預覽] 索引標籤中的 [StartPreview] 按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    <span data-ttu-id="1c3e4-352">[] 索引標籤會顯示將會複製到伺服器的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-352">The tab displays a list of the files that will be copied to the server.</span></span> <span data-ttu-id="1c3e4-353">顯示預覽不需要發佈應用程式，但是是有用的函式，要注意。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-353">Displaying the preview isn't required to publish the application but is a useful function to be aware of.</span></span> <span data-ttu-id="1c3e4-354">在此情況下，您不需要採取任何動作顯示的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-354">In this case, you don't need to do anything with the list of files that is displayed.</span></span> <span data-ttu-id="1c3e4-355">下次您部署此應用程式中，只有已變更的檔案會出現在此清單。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-355">The next time you deploy this application, only the files that have changed will be in this list.</span></span>  
  
    ![StartPreview 檔案輸出](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. <span data-ttu-id="1c3e4-357">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-357">Click **Publish**.</span></span>  
    <span data-ttu-id="1c3e4-358">Visual Studio 會開始將檔案複製到 Windows Azure 伺服器的程序。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-358">Visual Studio begins the process of copying the files to the Windows Azure server.</span></span>
13. <span data-ttu-id="1c3e4-359">**輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-359">The **Output** window shows what deployment actions were taken and reports successful completion of the deployment.</span></span>  
  
    ![報告部署成功的 [輸出] 視窗](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. <span data-ttu-id="1c3e4-361">部署成功時，預設瀏覽器會自動開啟至已部署的網站的 URL。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-361">Upon successful deployment, the default browser automatically opens to the URL of the deployed web site.</span></span>  
    <span data-ttu-id="1c3e4-362">您所建立的應用程式現在正在雲端中執行的。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-362">The application you created is now running in the cloud.</span></span> <span data-ttu-id="1c3e4-363">按一下 [Students] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-363">Click the Students tab.</span></span>  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

<span data-ttu-id="1c3e4-365">此時您*SchoolContext*資料庫已在 Windows Azure SQL Database 中已建立，因為您選取**Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-365">At this point your *SchoolContext* database has been created in the Windows Azure SQL Database because you selected **Execute Code First Migrations (runs on app start)**.</span></span> <span data-ttu-id="1c3e4-366">*Web.config*在已部署的網站上的檔案已變更，讓[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始設定式會執行第一次您的程式碼中讀取或寫入資料庫中的資料 （其中您選取時發生**學生** 索引標籤上):</span><span class="sxs-lookup"><span data-stu-id="1c3e4-366">The *Web.config* file in the deployed web site has been changed so that the [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) initializer would run the first time your code reads or writes data in the database (which happened when you selected the **Students** tab):</span></span>

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

<span data-ttu-id="1c3e4-367">部署程序也會建立新的連接字串 *(SchoolContext\_DatabasePublish*) 對要用於更新資料庫結構描述和植入資料庫的 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-367">The deployment process also created a new connection string *(SchoolContext\_DatabasePublish*) for Code First Migrations to use for updating the database schema and seeding the database.</span></span>

![Database_Publish 連接字串](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

<span data-ttu-id="1c3e4-369">*DefaultConnection*連接字串做為成員資格資料庫 （我們不會使用在本教學課程）。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-369">The *DefaultConnection* connection string is for the membership database (which we are not using in this tutorial).</span></span> <span data-ttu-id="1c3e4-370">*SchoolContext*是 ContosoUniversity 資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-370">The *SchoolContext* connection string is for the ContosoUniversity database.</span></span>

<span data-ttu-id="1c3e4-371">您可以找到 Web.config 檔案在電腦上部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。您可以存取已部署*Web.config*檔案本身使用 FTP。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-371">You can find the deployed version of the Web.config file on your own computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. You can access the deployed *Web.config* file itself by using FTP.</span></span> <span data-ttu-id="1c3e4-372">如需相關指示，請參閱 <<c0> [ 使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-372">For instructions, see [ASP.NET Web Deployment using Visual Studio: Deploying a Code Update](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md).</span></span> <span data-ttu-id="1c3e4-373">遵循指示以開始 「 若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。 」</span><span class="sxs-lookup"><span data-stu-id="1c3e4-373">Follow the instructions that start with "To use an FTP tool, you need three things: the FTP URL, the user name, and the password."</span></span>

> [!NOTE]
> <span data-ttu-id="1c3e4-374">Web 應用程式不會實作安全性，以便找到 URL 的任何人可以變更的資料。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-374">The web app doesn't implement security, so anyone who finds the URL can change the data.</span></span> <span data-ttu-id="1c3e4-375">如需有關如何保護網站上的指示，請參閱[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-375">For instructions on how to secure the web site, see [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="1c3e4-376">您可以防止其他人使用 Windows Azure 管理入口網站中使用網站或**伺服器總管**停止站台的 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-376">You can prevent other people from using the site by using the Windows Azure Management Portal or **Server Explorer** in Visual Studio to stop the site.</span></span>


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a><span data-ttu-id="1c3e4-377">程式碼的第一個初始設定式</span><span class="sxs-lookup"><span data-stu-id="1c3e4-377">Code First Initializers</span></span>

<span data-ttu-id="1c3e4-378">在 [部署] 區段中您所見[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)所使用的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-378">In the deployment section you saw the [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) initializer being used.</span></span> <span data-ttu-id="1c3e4-379">程式碼第一次也會提供您可以使用，包括其他初始設定式[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （預設值）， [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-379">Code First also provides other initializers that you can use, including [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (the default), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) and [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx).</span></span> <span data-ttu-id="1c3e4-380">`DropCreateAlways`初始設定式可用於設定 單元測試的條件。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-380">The `DropCreateAlways` initializer can be useful for setting up conditions for unit tests.</span></span> <span data-ttu-id="1c3e4-381">您也可以撰寫您自己的初始設定式，而您可以呼叫初始設定式明確地如果您不想等候，直到應用程式讀取或寫入資料庫。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-381">You can also write your own initializers, and you can call an initializer explicitly if you don't want to wait until the application reads from or writes to the database.</span></span> <span data-ttu-id="1c3e4-382">如需初始設定式的完整說明，請參閱本書第 6 章[Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-382">For a comprehensive explanation of initializers, see chapter 6 of the book [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) by Julie Lerman and Rowan Miller.</span></span>

## <a name="summary"></a><span data-ttu-id="1c3e4-383">總結</span><span class="sxs-lookup"><span data-stu-id="1c3e4-383">Summary</span></span>

<span data-ttu-id="1c3e4-384">在本教學課程中，您已了解如何建立資料模型，並實作基本的 CRUD、 排序、 篩選、 分頁和分組功能。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-384">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="1c3e4-385">在下一個教學課程中，您將開始查看更進階的主題，方法是展開的資料模型。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-385">In the next tutorial you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="1c3e4-386">其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="1c3e4-386">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c3e4-387">[上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="1c3e4-387">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)</span></span>
