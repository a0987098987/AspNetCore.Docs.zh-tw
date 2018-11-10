---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 篩選和分頁與 ASP.NET MVC 應用程式中的 Entity Framework |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: riande
ms.date: 10/08/2018
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9fabb5a90af715d4e96ff79b43bfff5a4600ac08
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912770"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="5bfd0-103">排序、 篩選和分頁與 Entity Framework 的 ASP.NET MVC 應用程式中</span><span class="sxs-lookup"><span data-stu-id="5bfd0-103">Sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="5bfd0-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5bfd0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="5bfd0-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="5bfd0-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> <span data-ttu-id="5bfd0-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio.</span></span> <span data-ttu-id="5bfd0-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="5bfd0-108">在上一個教學課程中，您可以實作的基本 CRUD 作業的 web 網頁的一組`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="5bfd0-109">在本教學課程中，您將新增排序、 篩選和分頁功能**學生**索引頁面。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-109">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="5bfd0-110">此外，還要建立將執行簡易群組的頁面。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="5bfd0-111">下圖顯示當您完成時的頁面外觀。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="5bfd0-112">資料行標題是使用者可以按一下以依據該資料行排序的連結。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="5bfd0-113">重覆按一下資料行標題，可切換遞增和遞減排序次序。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="5bfd0-115">將資料行排序連結新增至 Students [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="5bfd0-115">Add column sort links to the Students index page</span></span>

<span data-ttu-id="5bfd0-116">若要將排序新增至學生的 [索引] 頁面，您將會變更`Index`方法`Student`控制器並將程式碼加入`Student`索引檢視。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-116">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="5bfd0-117">將排序功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="5bfd0-117">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="5bfd0-118">在  *Controllers\StudentController.cs*，取代`Index`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-118">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="5bfd0-119">此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="5bfd0-120">查詢字串值是由 ASP.NET MVC 提供，做為動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-120">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="5bfd0-121">（選擇性） 後面接著底線和字串"desc"來指定遞減順序為"Name"Date"的字串參數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-121">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="5bfd0-122">預設的排序次序為遞增。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-122">The default sort order is ascending.</span></span>

<span data-ttu-id="5bfd0-123">第一次要求 [索引] 頁面時，沒有任何查詢字串。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="5bfd0-124">以遞增順序顯示學生`LastName`，這是預設值中的 fallthrough 案例所建立`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-124">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="5bfd0-125">使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="5bfd0-126">這兩個`ViewBag`使用變數時，讓檢視可以使用適當的查詢字串值來設定資料行標題超連結：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-126">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="5bfd0-127">這些是三元陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-127">These are ternary statements.</span></span> <span data-ttu-id="5bfd0-128">第一個指定，如果`sortOrder`參數為 null 或空的`ViewBag.NameSortParm`應設為 「 名稱\_desc"，否則它應該設定為空字串。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-128">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="5bfd0-129">這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="5bfd0-130">目前排序次序</span><span class="sxs-lookup"><span data-stu-id="5bfd0-130">Current sort order</span></span> | <span data-ttu-id="5bfd0-131">姓氏超連結</span><span class="sxs-lookup"><span data-stu-id="5bfd0-131">Last Name Hyperlink</span></span> | <span data-ttu-id="5bfd0-132">日期超連結</span><span class="sxs-lookup"><span data-stu-id="5bfd0-132">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5bfd0-133">姓氏遞增</span><span class="sxs-lookup"><span data-stu-id="5bfd0-133">Last Name ascending</span></span> | <span data-ttu-id="5bfd0-134">descending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-134">descending</span></span> | <span data-ttu-id="5bfd0-135">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-135">ascending</span></span> |
| <span data-ttu-id="5bfd0-136">姓氏遞減</span><span class="sxs-lookup"><span data-stu-id="5bfd0-136">Last Name descending</span></span> | <span data-ttu-id="5bfd0-137">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-137">ascending</span></span> | <span data-ttu-id="5bfd0-138">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-138">ascending</span></span> |
| <span data-ttu-id="5bfd0-139">日期遞增</span><span class="sxs-lookup"><span data-stu-id="5bfd0-139">Date ascending</span></span> | <span data-ttu-id="5bfd0-140">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-140">ascending</span></span> | <span data-ttu-id="5bfd0-141">descending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-141">descending</span></span> |
| <span data-ttu-id="5bfd0-142">日期遞減</span><span class="sxs-lookup"><span data-stu-id="5bfd0-142">Date descending</span></span> | <span data-ttu-id="5bfd0-143">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-143">ascending</span></span> | <span data-ttu-id="5bfd0-144">ascending</span><span class="sxs-lookup"><span data-stu-id="5bfd0-144">ascending</span></span> |

<span data-ttu-id="5bfd0-145">此方法會使用[LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)指定排序所依據的資料行。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-145">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="5bfd0-146">程式碼會建立<xref:System.Linq.IQueryable%601>變數之前`switch`陳述式，會修改它`switch`陳述式，並呼叫`ToList`方法之後`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-146">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="5bfd0-147">當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="5bfd0-148">查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="5bfd0-149">因此，此程式碼會產生之前不會執行單一查詢`return View`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="5bfd0-150">撰寫不同的 LINQ 陳述式，針對每個排序順序的替代方案，您可以動態地建立 LINQ 陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-150">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="5bfd0-151">如需動態 LINQ 的資訊，請參閱[動態 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-151">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="5bfd0-152">將資料行標題超連結新增至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="5bfd0-152">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="5bfd0-153">在  *Views\Student\Index.cshtml*，取代`<tr>`和`<th>`醒目提示的程式碼的標題資料列的項目：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-153">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="5bfd0-154">此程式碼使用中的資訊`ViewBag`屬性來設定超連結，以適當的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-154">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="5bfd0-155">執行頁面，然後按一下**姓氏**並**註冊日期**資料行標題，以確認該排序運作。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-155">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   ![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

   <span data-ttu-id="5bfd0-157">按一下 之後**姓氏**標題之下，學生以遞減顯示姓氏來排序。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-157">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

   ![在網頁瀏覽器學生 [索引] 檢視](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="5bfd0-159">在搜尋方塊新增至 Students [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="5bfd0-159">Add a search box to the Students index page</span></span>

<span data-ttu-id="5bfd0-160">若要新增至 Students [索引] 頁面的篩選，您會將文字方塊和提交按鈕新增至檢視，並進行對應的變更，在`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-160">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="5bfd0-161">[文字] 方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-161">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="5bfd0-162">將篩選功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="5bfd0-162">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="5bfd0-163">在  *Controllers\StudentController.cs*，取代`Index`（變更已醒目提示） 為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-163">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="5bfd0-164">程式碼新增`searchString`參數來`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-164">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="5bfd0-165">從將新增至 [索引] 檢視的文字方塊中接收搜尋字串值。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-165">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="5bfd0-166">它也會新增`where`子句來選取其名字或姓氏包含搜尋字串的學生將 LINQ 陳述式。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-166">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="5bfd0-167">陳述式，將它新增<xref:System.Linq.Queryable.Where%2A>子句執行只有當沒有要搜尋的值。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-167">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="5bfd0-168">在許多情況下，Entity Framework 實體集上，或是當做擴充方法在記憶體中的集合，可以呼叫相同的方法。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-168">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="5bfd0-169">結果通常都相同，但在某些情況下可能會不同。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-169">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="5bfd0-170">例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回空字串的零個資料列。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-170">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="5bfd0-171">因此在範例程式碼 (將放`Where`內的陳述式`if`陳述式) 可確保您取得所有的 SQL Server 版本相同的結果。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-171">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="5bfd0-172">此外，.NET Framework 實作的`Contains`方法依預設，執行區分大小寫比較，但 Entity Framework 的 SQL Server 提供者預設情況下執行不區分大小寫的比較。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-172">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="5bfd0-173">因此，呼叫`ToUpper`若要使測試明確不區分大小寫的方法可確保當您變更程式碼稍後使用的儲存機制，這會傳回結果不會變更`IEnumerable`而不是集合`IQueryable`物件。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-173">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="5bfd0-174">(當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)</span><span class="sxs-lookup"><span data-stu-id="5bfd0-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="5bfd0-175">Null 處理也可能是不同的資料庫提供者，或當您使用不同`IQueryable`當您使用物件相較於`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-175">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="5bfd0-176">例如，在某些情況下`Where`這類條件`table.Column != 0`可能不會傳回具有資料行`null`做為值。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-176">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="5bfd0-177">如需詳細資訊，請參閱 <<c0> [ 為 null 的變數 'where' 子句中不正確地處理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-177">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="5bfd0-178">在搜尋方塊新增至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="5bfd0-178">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="5bfd0-179">在*Views\Student\Index.cshtml*，新增醒目提示的程式碼開頭之前，立即`table`標記，以建立標題，文字方塊中，和**搜尋** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-179">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="5bfd0-180">執行頁面，輸入搜尋字串，然後按一下**搜尋**以確認篩選可以運作。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-180">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   ![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

   <span data-ttu-id="5bfd0-182">請注意 URL 未包含"an"搜尋字串，這表示，如果您將本頁加入標籤，您無法取得篩選的清單，當您使用書籤。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-182">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="5bfd0-183">這也適用於資料行排序連結時，因為它們會排序整個清單。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-183">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="5bfd0-184">您將會變更**搜尋**来用於篩選準則中的查詢字串，稍後在本教學課程中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-184">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="5bfd0-185">將分頁新增至 Students [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="5bfd0-185">Add paging to the Students index page</span></span>

<span data-ttu-id="5bfd0-186">若要將分頁新增至 Students [索引] 頁面中，您將先安裝**PagedList.Mvc** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-186">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="5bfd0-187">然後您將進行中的其他變更`Index`方法，並新增到分頁連結`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-187">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="5bfd0-188">**PagedList.Mvc**許多很好的分頁和排序封裝的 ASP.NET MVC 的其中一個，而且它的使用僅為例，不做為它的建議，透過其他選項。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-188">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="5bfd0-189">下圖顯示分頁連結。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-189">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="5bfd0-191">安裝 PagedList.MVC NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="5bfd0-191">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="5bfd0-192">NuGet **PagedList.Mvc**會自動安裝套件**PagedList**套件作為相依性。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-192">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="5bfd0-193">**PagedList**封裝會安裝`PagedList`集合型別和擴充功能方法`IQueryable`和`IEnumerable`集合。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-193">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="5bfd0-194">擴充方法建立單一頁面中的資料`PagedList`共收集您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法可簡化分頁。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-194">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="5bfd0-195">**PagedList.Mvc**套件會安裝的分頁的協助程式會顯示分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-195">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="5bfd0-196">從**工具**功能表上，選取**NuGet 套件管理員**，然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-196">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="5bfd0-197">在**Package Manager Console**  視窗中，請確定**套件來源**是**nuget.org**而**預設專案**是**ContosoUniversity**，然後輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-197">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

   ![安裝 PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

3. <span data-ttu-id="5bfd0-199">建置專案。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-199">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="5bfd0-200">將分頁功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="5bfd0-200">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="5bfd0-201">在  *Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-201">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="5bfd0-202">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-202">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="5bfd0-203">這個程式碼加入`page`參數、 目前的排序次序參數和目前的篩選參數至方法簽章：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-203">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="5bfd0-204">第一次顯示頁面，或如果使用者尚未按一下分頁或排序連結，所有參數都是 null。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="5bfd0-205">如果按一下分頁連結，`page`變數包含要顯示的頁碼。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-205">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="5bfd0-206">A`ViewBag`屬性會提供與目前的排序次序中，檢視，因為這必須包含在分頁連結，才能保留分頁時相同的排序次序：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-206">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="5bfd0-207">另一個屬性， `ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-207">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="5bfd0-208">此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="5bfd0-209">如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="5bfd0-210">在文字方塊中輸入值，並按下 [提交] 按鈕會變更搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-210">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="5bfd0-211">在此情況下，`searchString`參數不是 null。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-211">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="5bfd0-212">方法的結尾`ToPagedList`擴充方法給學生們`IQueryable`物件將學生查詢轉換成支援分頁的集合類型的單一頁面。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-212">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="5bfd0-213">該學生單一頁面接著會傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-213">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="5bfd0-214">`ToPagedList` 方法會採用頁面數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-214">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="5bfd0-215">兩個問號代表[null 聯合運算子](/dotnet/csharp/language-reference/operators/null-coalescing-operator)。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-215">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="5bfd0-216">Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="5bfd0-217">將分頁連結新增至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="5bfd0-217">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="5bfd0-218">在  *Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-218">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="5bfd0-219">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-219">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="5bfd0-220">頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-220">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="5bfd0-221">`using`陳述式`PagedList.Mvc`分頁按鈕讓 MVC 協助程式的存取。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-221">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="5bfd0-222">此程式碼使用的多載[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))可指定[FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100))。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-222">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="5bfd0-223">預設值[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))文章時，這表示參數會在 HTTP 訊息本文中並不是以 URL 傳遞作為查詢字串使用提交表單資料。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-223">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="5bfd0-224">當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-224">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="5bfd0-225">[W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建議的動作不會產生更新時，您應該使用 GET。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-225">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="5bfd0-226">文字方塊會初始化與目前的搜尋字串中，當您按一下新的頁面時，您就可以看到目前的搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-226">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="5bfd0-227">資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-227">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="5bfd0-228">會顯示目前頁面和總頁數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-228">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="5bfd0-229">如果不沒有顯示任何頁面，會顯示 「 頁面 0 的 0 」。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-229">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="5bfd0-230">(在此情況下頁編號大於頁面計數因為`Model.PageNumber`為 1，和`Model.PageCount`為 0。)</span><span class="sxs-lookup"><span data-stu-id="5bfd0-230">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="5bfd0-231">分頁按鈕會顯示`PagedListPager`協助程式：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-231">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="5bfd0-232">`PagedListPager`協助程式提供數個選項，您可以自訂，包括 Url 和樣式設定。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-232">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="5bfd0-233">如需詳細資訊，請參閱 < [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 網站上。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-233">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="5bfd0-234">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-234">Run the page.</span></span>

   ![Students [索引] 頁面上，使用分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

   <span data-ttu-id="5bfd0-236">以不同排序次序按一下分頁連結，以確定分頁運作正常。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-236">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="5bfd0-237">然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-237">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

   ![Students [索引] 頁面搜尋篩選條件文字](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="5bfd0-239">建立顯示學生統計資料的 About 頁面</span><span class="sxs-lookup"><span data-stu-id="5bfd0-239">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="5bfd0-240">Contoso 大學網站的相關頁面，您將會顯示多少學生註冊每個註冊日期。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-240">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="5bfd0-241">這需要對群組進行分組和簡單計算。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-241">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="5bfd0-242">若要完成此工作，您需要執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-242">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="5bfd0-243">針對您需要傳遞至檢視的資料，建立檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-243">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="5bfd0-244">修改`About`方法中的`Home`控制站。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-244">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="5bfd0-245">修改`About`檢視。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-245">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="5bfd0-246">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="5bfd0-246">Create the View Model</span></span>

<span data-ttu-id="5bfd0-247">建立*Viewmodel*專案資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-247">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="5bfd0-248">在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-248">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="5bfd0-249">修改 Home 控制器</span><span class="sxs-lookup"><span data-stu-id="5bfd0-249">Modify the Home Controller</span></span>

1. <span data-ttu-id="5bfd0-250">在  *HomeController.cs*，新增下列`using`陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-250">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="5bfd0-251">此類別的左大括弧之後立即新增資料庫內容的類別變數：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-251">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="5bfd0-252">以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-252">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="5bfd0-253">LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-253">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="5bfd0-254">新增`Dispose`方法：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-254">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="5bfd0-255">修改 About 檢視</span><span class="sxs-lookup"><span data-stu-id="5bfd0-255">Modify the About View</span></span>

1. <span data-ttu-id="5bfd0-256">中的程式碼取代*Views\Home\About.cshtml*為下列程式碼的檔案：</span><span class="sxs-lookup"><span data-stu-id="5bfd0-256">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="5bfd0-257">執行應用程式，然後按一下**關於**連結。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-257">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="5bfd0-258">以表格顯示每個註冊日期的學生人數。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-258">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="5bfd0-260">總結</span><span class="sxs-lookup"><span data-stu-id="5bfd0-260">Summary</span></span>

<span data-ttu-id="5bfd0-261">在本教學課程中，您已了解如何建立資料模型，並實作基本的 CRUD、 排序、 篩選、 分頁和分組功能。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-261">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="5bfd0-262">在下一個教學課程中，您就可以展開資料模型以學習更進階的主題。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-262">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="5bfd0-263">您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-263">Please leave feedback on how you liked this tutorial and what we could improve.</span></span>

<span data-ttu-id="5bfd0-264">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="5bfd0-264">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5bfd0-265">[上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一頁](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="5bfd0-265">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
