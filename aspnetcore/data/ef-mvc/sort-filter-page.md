---
title: ASP.NET Core MVC 與 EF Core - 排序、篩選、分頁 - 3/10
author: rick-anderson
description: 在本教學課程中，您將會使用 ASP.NET Core 和 Entity Framework Core 將排序、篩選、分頁功能新增至頁面。
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 344e3a1806ff21d8ce335b2b407a8a93baf72c1b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="37da9-103">ASP.NET Core MVC 與 EF Core - 排序、篩選、分頁 - 3/10</span><span class="sxs-lookup"><span data-stu-id="37da9-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

<span data-ttu-id="37da9-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37da9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37da9-105">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="37da9-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="37da9-106">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="37da9-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="37da9-107">在上一個教學課程中，您已針對學生實體的基本 CRUD 作業實作一組網頁。</span><span class="sxs-lookup"><span data-stu-id="37da9-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="37da9-108">在本教學課程中，您要將排序、篩選和分頁功能新增至 Students 的 [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="37da9-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="37da9-109">此外，還要建立將執行簡易群組的頁面。</span><span class="sxs-lookup"><span data-stu-id="37da9-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="37da9-110">下圖顯示當您完成時的頁面外觀。</span><span class="sxs-lookup"><span data-stu-id="37da9-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="37da9-111">資料行標題是使用者可以按一下以依據該資料行排序的連結。</span><span class="sxs-lookup"><span data-stu-id="37da9-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="37da9-112">重覆按一下資料行標題，可切換遞增和遞減排序次序。</span><span class="sxs-lookup"><span data-stu-id="37da9-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students [索引] 頁面](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="37da9-114">將資料行排序連結新增至 Students 的 [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="37da9-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="37da9-115">若要將排序新增至學生的 [索引] 頁面，您要變更 Students 控制器的 `Index` 方法，並將程式碼新增至學生的 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="37da9-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="37da9-116">將排序功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="37da9-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="37da9-117">在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="37da9-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="37da9-118">此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。</span><span class="sxs-lookup"><span data-stu-id="37da9-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="37da9-119">查詢字串值是由 ASP.NET Core MVC 提供，作為動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="37da9-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="37da9-120">該參數是 "Name" 或 "Date" 的字串，後面可選擇性地接著底線和字串 "desc" 來指定遞減順序。</span><span class="sxs-lookup"><span data-stu-id="37da9-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="37da9-121">預設的排序次序為遞增。</span><span class="sxs-lookup"><span data-stu-id="37da9-121">The default sort order is ascending.</span></span>

<span data-ttu-id="37da9-122">第一次要求 [索引] 頁面時，沒有任何查詢字串。</span><span class="sxs-lookup"><span data-stu-id="37da9-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="37da9-123">學生會依姓氏的遞增順序顯示，這是 `switch` 陳述式中失敗案例所建立的預設值。</span><span class="sxs-lookup"><span data-stu-id="37da9-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="37da9-124">使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。</span><span class="sxs-lookup"><span data-stu-id="37da9-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="37da9-125">檢視會使用兩個 `ViewData` 項目 (NameSortParm 和 DateSortParm)，以適當的查詢字串值設定資料行標題超連結。</span><span class="sxs-lookup"><span data-stu-id="37da9-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="37da9-126">這些是三元陳述式。</span><span class="sxs-lookup"><span data-stu-id="37da9-126">These are ternary statements.</span></span> <span data-ttu-id="37da9-127">第一個陳述式指定當 `sortOrder` 參數為 null 或是空的時，NameSortParm 應該設定為 "name_desc"；否則它應該設定為空字串。</span><span class="sxs-lookup"><span data-stu-id="37da9-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="37da9-128">這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：</span><span class="sxs-lookup"><span data-stu-id="37da9-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="37da9-129">目前排序次序</span><span class="sxs-lookup"><span data-stu-id="37da9-129">Current sort order</span></span>  | <span data-ttu-id="37da9-130">姓氏超連結</span><span class="sxs-lookup"><span data-stu-id="37da9-130">Last Name Hyperlink</span></span> | <span data-ttu-id="37da9-131">日期超連結</span><span class="sxs-lookup"><span data-stu-id="37da9-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="37da9-132">姓氏遞增</span><span class="sxs-lookup"><span data-stu-id="37da9-132">Last Name ascending</span></span>  | <span data-ttu-id="37da9-133">descending</span><span class="sxs-lookup"><span data-stu-id="37da9-133">descending</span></span>          | <span data-ttu-id="37da9-134">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-134">ascending</span></span>      |
| <span data-ttu-id="37da9-135">姓氏遞減</span><span class="sxs-lookup"><span data-stu-id="37da9-135">Last Name descending</span></span> | <span data-ttu-id="37da9-136">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-136">ascending</span></span>           | <span data-ttu-id="37da9-137">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-137">ascending</span></span>      |
| <span data-ttu-id="37da9-138">日期遞增</span><span class="sxs-lookup"><span data-stu-id="37da9-138">Date ascending</span></span>       | <span data-ttu-id="37da9-139">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-139">ascending</span></span>           | <span data-ttu-id="37da9-140">descending</span><span class="sxs-lookup"><span data-stu-id="37da9-140">descending</span></span>     |
| <span data-ttu-id="37da9-141">日期遞減</span><span class="sxs-lookup"><span data-stu-id="37da9-141">Date descending</span></span>      | <span data-ttu-id="37da9-142">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-142">ascending</span></span>           | <span data-ttu-id="37da9-143">ascending</span><span class="sxs-lookup"><span data-stu-id="37da9-143">ascending</span></span>      |

<span data-ttu-id="37da9-144">此方法使用 LINQ to Entities 來指定排序所依據的資料行。</span><span class="sxs-lookup"><span data-stu-id="37da9-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="37da9-145">此程式碼會在 switch 陳述式之前建立 `IQueryable` 變數、在 switch 陳述式中修改它，並在 `switch` 陳述式之後呼叫 `ToListAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="37da9-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="37da9-146">當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="37da9-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="37da9-147">在您呼叫 `ToListAsync` 等方法以將 `IQueryable` 物件轉換成集合之前，不會執行查詢。</span><span class="sxs-lookup"><span data-stu-id="37da9-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="37da9-148">因此，此程式碼會產生一個直到 `return View` 陳述式才會執行的單一查詢。</span><span class="sxs-lookup"><span data-stu-id="37da9-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="37da9-149">此程式碼可取得使用大量資料行數目的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="37da9-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="37da9-150">[本系列中的最後一個教學課程](advanced.md#dynamic-linq)示範如何撰寫程式碼，讓您以字串變數傳遞 `OrderBy` 資料行的名稱。</span><span class="sxs-lookup"><span data-stu-id="37da9-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="37da9-151">將資料行標題超連結新增至學生的 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="37da9-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="37da9-152">以下列程式碼取代 *Views/Students/Index.cshtml* 中的程式碼，以新增資料行標題超連結。</span><span class="sxs-lookup"><span data-stu-id="37da9-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="37da9-153">變更的行已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="37da9-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="37da9-154">此程式碼使用 `ViewData` 屬性中的資訊，以適當的查詢字串值設定超連結。</span><span class="sxs-lookup"><span data-stu-id="37da9-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="37da9-155">執行應用程式，選取 [Students] 索引標籤，然後按一下 [姓氏] 和 [註冊日期] 資料行標題，以確認排序的運作正常。</span><span class="sxs-lookup"><span data-stu-id="37da9-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![以姓名順序排列的 Students [索引] 頁面](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="37da9-157">將搜尋方塊新增至 Students 的 [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="37da9-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="37da9-158">若要將篩選新增至 Students 的 [索引] 頁面，您要將文字方塊和提交按鈕新增至檢視，並在 `Index` 方法中進行對應的變更。</span><span class="sxs-lookup"><span data-stu-id="37da9-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="37da9-159">文字方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。</span><span class="sxs-lookup"><span data-stu-id="37da9-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="37da9-160">將篩選功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="37da9-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="37da9-161">在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法 (變更已醒目提示)。</span><span class="sxs-lookup"><span data-stu-id="37da9-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="37da9-162">您已將 `searchString` 參數新增至 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="37da9-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="37da9-163">從將新增至 [索引] 檢視的文字方塊中接收搜尋字串值。</span><span class="sxs-lookup"><span data-stu-id="37da9-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="37da9-164">您也已在 LINQ 陳述式中新增 where 子句，該子句只會選取其名字或姓氏包含搜尋字串的學生。</span><span class="sxs-lookup"><span data-stu-id="37da9-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="37da9-165">唯有當具有要搜尋的值時，才會執行新增 where 子句的陳述式。</span><span class="sxs-lookup"><span data-stu-id="37da9-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="37da9-166">在這裡，您可以在 `IQueryable` 物件上呼叫 `Where` 方法，而篩選將會在伺服器上處理。</span><span class="sxs-lookup"><span data-stu-id="37da9-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="37da9-167">在某些情況下，您可能會呼叫 `Where` 方法在記憶體內部集合上作為擴充方法。</span><span class="sxs-lookup"><span data-stu-id="37da9-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="37da9-168">(例如，假設您變更了 `_context.Students` 的參考，以便它參考傳回 `IEnumerable` 集合的存放庫方法，而不是參考 EF `DbSet`)。結果通常都是一樣的，但在某些情況下可能會不同。</span><span class="sxs-lookup"><span data-stu-id="37da9-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="37da9-169">例如，.NET Framework 實作的 `Contains` 方法預設會執行區分大小寫的比較，但在 SQL Server 中，這取決於 SQL Server 執行個體的定序設定。</span><span class="sxs-lookup"><span data-stu-id="37da9-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="37da9-170">該設定預設為不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="37da9-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="37da9-171">您可以呼叫 `ToUpper` 方法，使測試明確地不區分大小寫：*Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*。</span><span class="sxs-lookup"><span data-stu-id="37da9-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="37da9-172">如果您稍後變更程式碼，以使用傳回 `IEnumerable` 集合 (而不是 `IQueryable` 物件) 的存放庫，這會確保結果保持不變。</span><span class="sxs-lookup"><span data-stu-id="37da9-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="37da9-173">(當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)不過，此解決方案會對效能帶來負面影響。</span><span class="sxs-lookup"><span data-stu-id="37da9-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="37da9-174">`ToUpper` 程式碼會將一個函式置於 TSQL SELECT 陳述式的 WHERE 子句中。</span><span class="sxs-lookup"><span data-stu-id="37da9-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="37da9-175">這會防止最佳化工具使用索引。</span><span class="sxs-lookup"><span data-stu-id="37da9-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="37da9-176">假設 SQL 大部分安裝為不區分大小寫，最好避免使用 `ToUpper` 程式碼，直到您移轉至區分大小寫的資料存放區為止。</span><span class="sxs-lookup"><span data-stu-id="37da9-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="37da9-177">將搜尋方塊新增至學生的 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="37da9-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="37da9-178">在 *Views/Student/Index.cshtml* 中，於開始表格標記之前立即新增醒目提示的程式碼，以建立標題、文字方塊及 [搜尋]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="37da9-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="37da9-179">此程式碼會使用 `<form>` [標籤協助程式](xref:mvc/views/tag-helpers/intro) 來新增搜尋文字方塊和按鈕。</span><span class="sxs-lookup"><span data-stu-id="37da9-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="37da9-180">`<form>` 標籤協助程式預設會使用 POST 提交表單資料，這表示參數會以 HTTP 訊息本文傳遞，而不是以 URL 作為查詢字串傳遞。</span><span class="sxs-lookup"><span data-stu-id="37da9-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="37da9-181">當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。</span><span class="sxs-lookup"><span data-stu-id="37da9-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="37da9-182">W3C 指導方針建議，只有在動作不會產生更新時才應使用 GET。</span><span class="sxs-lookup"><span data-stu-id="37da9-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="37da9-183">執行應用程式，選取 [Students] 索引標籤，輸入搜尋字串，然後按一下 [搜尋] 以確認篩選可以運作。</span><span class="sxs-lookup"><span data-stu-id="37da9-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![含篩選的 Students [索引] 頁面](sort-filter-page/_static/filtering.png)

<span data-ttu-id="37da9-185">請注意，URL 中包含了搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="37da9-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="37da9-186">如果您為此頁面加上書籤，則會在使用書籤時取得篩選的清單。</span><span class="sxs-lookup"><span data-stu-id="37da9-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="37da9-187">將 `method="get"` 新增至 `form` 標籤會導致查詢字串的產生。</span><span class="sxs-lookup"><span data-stu-id="37da9-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="37da9-188">在這個階段，如果您按一下資料行標題排序連結，將會遺失您在 [搜尋] 方塊中輸入的篩選值。</span><span class="sxs-lookup"><span data-stu-id="37da9-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="37da9-189">您將在下節修正該問題。</span><span class="sxs-lookup"><span data-stu-id="37da9-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="37da9-190">將分頁功能新增至 Students 的 [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="37da9-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="37da9-191">若要將分頁新增至 Students 的 [索引] 頁面，您要建立使用 `Skip` 和 `Take` 陳述式的 `PaginatedList` 類別來篩選伺服器上的資料，而不是一直擷取資料表的所有資料列。</span><span class="sxs-lookup"><span data-stu-id="37da9-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="37da9-192">然後，您要在 `Index` 方法中進行其他變更，並將分頁按鈕新增至 `Index` 檢視。</span><span class="sxs-lookup"><span data-stu-id="37da9-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="37da9-193">下圖顯示分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="37da9-193">The following illustration shows the paging buttons.</span></span>

![含分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

<span data-ttu-id="37da9-195">在專案資料夾中，建立 `PaginatedList.cs`，然後以下列程式碼取代範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="37da9-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="37da9-196">此程式碼中的 `CreateAsync` 方法會採用頁面大小和頁面數，並會將適當的 `Skip` 和 `Take` 陳述式套用至 `IQueryable`。</span><span class="sxs-lookup"><span data-stu-id="37da9-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="37da9-197">在 `IQueryable` 上呼叫 `ToListAsync` 時，會傳回僅包含所要求頁面的清單。</span><span class="sxs-lookup"><span data-stu-id="37da9-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="37da9-198">`HasPreviousPage` 和 `HasNextPage` 屬性可用來啟用或停用 [上一頁]  和 [下一頁] 分頁按鈕。</span><span class="sxs-lookup"><span data-stu-id="37da9-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="37da9-199">`CreateAsync` 方法用來建立 `PaginatedList<T>` 物件而不是建構函式，因為建構函式無法執行非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="37da9-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="37da9-200">將分頁功能新增至 Index 方法</span><span class="sxs-lookup"><span data-stu-id="37da9-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="37da9-201">在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="37da9-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="37da9-202">此程式碼會將頁碼參數、目前排序次序參數和目前篩選參數新增至方法簽章。</span><span class="sxs-lookup"><span data-stu-id="37da9-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="37da9-203">第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。</span><span class="sxs-lookup"><span data-stu-id="37da9-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="37da9-204">如果按一下分頁連結，頁面變數將包含要顯示的頁碼。</span><span class="sxs-lookup"><span data-stu-id="37da9-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="37da9-205">名為 CurrentSort 的 `ViewData` 項目會提供使用目前排序次序的檢視，因為這必須包含在分頁連結中，才能保持與分頁時相同的排序順序。</span><span class="sxs-lookup"><span data-stu-id="37da9-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="37da9-206">名為 CurrentFilter 的 `ViewData` 項目則提供使用目前篩選字串的檢視。</span><span class="sxs-lookup"><span data-stu-id="37da9-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="37da9-207">此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。</span><span class="sxs-lookup"><span data-stu-id="37da9-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="37da9-208">如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。</span><span class="sxs-lookup"><span data-stu-id="37da9-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="37da9-209">在文字方塊中輸入值並按下 [提交] 按鈕時，即會變更搜尋字串。</span><span class="sxs-lookup"><span data-stu-id="37da9-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="37da9-210">在此情況下，`searchString` 參數不是 null。</span><span class="sxs-lookup"><span data-stu-id="37da9-210">In that case, the `searchString` parameter isn't null.</span></span>

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

<span data-ttu-id="37da9-211">在 `Index` 方法的結尾處，`PaginatedList.CreateAsync` 方法會以支援分頁的集合類型，將學生查詢轉換成學生單一頁面。</span><span class="sxs-lookup"><span data-stu-id="37da9-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="37da9-212">該學生單一頁面接著會傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="37da9-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="37da9-213">`PaginatedList.CreateAsync` 方法會採用頁面數。</span><span class="sxs-lookup"><span data-stu-id="37da9-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="37da9-214">兩個問號代表 null 聯合運算子。</span><span class="sxs-lookup"><span data-stu-id="37da9-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="37da9-215">Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。</span><span class="sxs-lookup"><span data-stu-id="37da9-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="37da9-216">將分頁連結新增至學生 [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="37da9-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="37da9-217">在 *Views/Students/Index.cshtml* 中，以下列程式碼取代現有程式碼。</span><span class="sxs-lookup"><span data-stu-id="37da9-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="37da9-218">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="37da9-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="37da9-219">頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PaginatedList<T>` 物件，而不是 `List<T>` 物件。</span><span class="sxs-lookup"><span data-stu-id="37da9-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="37da9-220">資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：</span><span class="sxs-lookup"><span data-stu-id="37da9-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="37da9-221">分頁按鈕會透過標籤協助程式來顯示：</span><span class="sxs-lookup"><span data-stu-id="37da9-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="37da9-222">執行應用程式並移至 Students 頁面。</span><span class="sxs-lookup"><span data-stu-id="37da9-222">Run the app and go to the Students page.</span></span>

![含分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

<span data-ttu-id="37da9-224">以不同排序次序按一下分頁連結，以確定分頁運作正常。</span><span class="sxs-lookup"><span data-stu-id="37da9-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="37da9-225">然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。</span><span class="sxs-lookup"><span data-stu-id="37da9-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="37da9-226">建立顯示學生統計資料的 About 頁面</span><span class="sxs-lookup"><span data-stu-id="37da9-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="37da9-227">對於 Contoso 大學網站的 **About** 頁面，您將顯示每個註冊日期已有多少學生註冊。</span><span class="sxs-lookup"><span data-stu-id="37da9-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="37da9-228">這需要對群組進行分組和簡單計算。</span><span class="sxs-lookup"><span data-stu-id="37da9-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="37da9-229">若要完成此工作，您需要執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="37da9-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="37da9-230">針對您需要傳遞至檢視的資料，建立檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="37da9-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="37da9-231">修改 Home 控制器中的 About 方法。</span><span class="sxs-lookup"><span data-stu-id="37da9-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="37da9-232">修改 About 檢視。</span><span class="sxs-lookup"><span data-stu-id="37da9-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="37da9-233">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="37da9-233">Create the view model</span></span>

<span data-ttu-id="37da9-234">在 *Models* 資料夾中建立 *SchoolViewModels* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="37da9-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="37da9-235">在新的資料夾中，新增類別檔案 *EnrollmentDateGroup.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="37da9-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="37da9-236">修改 Home 控制器</span><span class="sxs-lookup"><span data-stu-id="37da9-236">Modify the Home Controller</span></span>

<span data-ttu-id="37da9-237">在 *HomeController.cs* 中，將下列 using 陳述式新增在檔案頂最上方：</span><span class="sxs-lookup"><span data-stu-id="37da9-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="37da9-238">在類別的左大括弧之後立即新增資料庫內容的類別變數，並從 ASP.NET Core DI 取得內容的執行個體：</span><span class="sxs-lookup"><span data-stu-id="37da9-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="37da9-239">以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="37da9-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="37da9-240">LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。</span><span class="sxs-lookup"><span data-stu-id="37da9-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="37da9-241">在 Entity Framework Core 1.0 版中，整個結果集會傳回到用戶端，並且在用戶端上完成群組作業。</span><span class="sxs-lookup"><span data-stu-id="37da9-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="37da9-242">在某些情況下，這可能會造成效能問題。</span><span class="sxs-lookup"><span data-stu-id="37da9-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="37da9-243">請務必使用生產環境數量的資料來測試效能，如有必要，請使用原始 SQL 在伺服器上執行群組作業。</span><span class="sxs-lookup"><span data-stu-id="37da9-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="37da9-244">如需如何使用原始 SQL 的資訊，請參閱[本系列的最後一個教學課程](advanced.md)。</span><span class="sxs-lookup"><span data-stu-id="37da9-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="37da9-245">修改 About 檢視</span><span class="sxs-lookup"><span data-stu-id="37da9-245">Modify the About View</span></span>

<span data-ttu-id="37da9-246">以下列程式碼取代 *Views/Home/About.cshtml* 檔案中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="37da9-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="37da9-247">執行應用程式並移至 About 頁面。</span><span class="sxs-lookup"><span data-stu-id="37da9-247">Run the app and go to the About page.</span></span> <span data-ttu-id="37da9-248">每個註冊日期的學生人數將會顯示在資料表中。</span><span class="sxs-lookup"><span data-stu-id="37da9-248">The count of students for each enrollment date is displayed in a table.</span></span>

![About 頁面](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="37da9-250">總結</span><span class="sxs-lookup"><span data-stu-id="37da9-250">Summary</span></span>

<span data-ttu-id="37da9-251">在本教學課程中，您已了解如何執行排序、篩選、分頁和群組。</span><span class="sxs-lookup"><span data-stu-id="37da9-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="37da9-252">在下一個教學課程中，您將學習如何使用移轉來處理資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="37da9-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37da9-253">[上一頁](crud.md)
> [下一頁](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="37da9-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
