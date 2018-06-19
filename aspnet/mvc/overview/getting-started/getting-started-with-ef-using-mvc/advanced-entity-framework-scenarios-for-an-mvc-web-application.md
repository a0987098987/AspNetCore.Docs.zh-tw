---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 進階 MVC 5 Web 應用程式 (12 / 12) 的 Entity Framework 6 案例 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 50987b30a49173605e7aeb8eb65ff1fe5d5e5753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877446"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a><span data-ttu-id="c7c2d-103">MVC 5 Web 應用程式 (12 / 12) 的進階的 Entity Framework 6 案例</span><span class="sxs-lookup"><span data-stu-id="c7c2d-103">Advanced Entity Framework 6 Scenarios for an MVC 5 Web Application (12 of 12)</span></span>
====================
<span data-ttu-id="c7c2d-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c7c2d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c7c2d-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7c2d-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="c7c2d-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="c7c2d-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="c7c2d-108">在上一個教學課程中，您會實作資料表每個階層繼承。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-108">In the previous tutorial you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="c7c2d-109">本教學課程包含介紹幾個有用，可注意當您超越開發使用 Entity Framework Code First 的 ASP.NET web 應用程式的基本概念的主題。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-109">This tutorial includes introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span> <span data-ttu-id="c7c2d-110">逐步解說會引導您完成程式碼和使用 Visual Studio 的下列主題：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-110">Step-by-step instructions walk you through the code and using Visual Studio for the following topics:</span></span>

- [<span data-ttu-id="c7c2d-111">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="c7c2d-111">Performing raw SQL queries</span></span>](#rawsql)
- [<span data-ttu-id="c7c2d-112">執行無追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="c7c2d-112">Performing no-tracking queries</span></span>](#notracking)
- [<span data-ttu-id="c7c2d-113">檢查 SQL 傳送至資料庫</span><span class="sxs-lookup"><span data-stu-id="c7c2d-113">Examining SQL sent to the database</span></span>](#sql)

<span data-ttu-id="c7c2d-114">本教學課程介紹幾個主題的簡介，其後加上行連結資源，如需詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-114">The tutorial introduces several topics with brief introductions followed by links to resources for more information:</span></span>

- [<span data-ttu-id="c7c2d-115">儲存機制和單位的工作模式</span><span class="sxs-lookup"><span data-stu-id="c7c2d-115">Repository and unit of work patterns</span></span>](#repo)
- [<span data-ttu-id="c7c2d-116">Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="c7c2d-116">Proxy classes</span></span>](#proxies)
- [<span data-ttu-id="c7c2d-117">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="c7c2d-117">Automatic change detection</span></span>](#changedetection)
- [<span data-ttu-id="c7c2d-118">自動驗證</span><span class="sxs-lookup"><span data-stu-id="c7c2d-118">Automatic validation</span></span>](#validation)
- [<span data-ttu-id="c7c2d-119">Visual Studio 的 EF 工具</span><span class="sxs-lookup"><span data-stu-id="c7c2d-119">EF tools for Visual Studio</span></span>](#tools)
- [<span data-ttu-id="c7c2d-120">Entity Framework 原始碼</span><span class="sxs-lookup"><span data-stu-id="c7c2d-120">Entity Framework source code</span></span>](#source)

<span data-ttu-id="c7c2d-121">本教學課程也包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-121">The tutorial also includes the following sections:</span></span>

- [<span data-ttu-id="c7c2d-122">摘要</span><span class="sxs-lookup"><span data-stu-id="c7c2d-122">Summary</span></span>](#summary)
- [<span data-ttu-id="c7c2d-123">通知</span><span class="sxs-lookup"><span data-stu-id="c7c2d-123">Acknowledgments</span></span>](#acknowledgments)
- [<span data-ttu-id="c7c2d-124">關於 VB 的附註</span><span class="sxs-lookup"><span data-stu-id="c7c2d-124">A note about VB</span></span>](#vb)
- [<span data-ttu-id="c7c2d-125">常見的錯誤，和它們的因應措施或解決方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-125">Common errors, and solutions or workarounds for them</span></span>](#errors)

<span data-ttu-id="c7c2d-126">這些主題的大多數時間，您將使用您已經建立的網頁。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-126">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="c7c2d-127">若要執行大量更新使用原始的 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的信用額度的數目：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-127">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a><span data-ttu-id="c7c2d-129">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="c7c2d-129">Performing Raw SQL Queries</span></span>

<span data-ttu-id="c7c2d-130">Entity Framework 程式碼的第一個 API 包含可讓您將直接對資料庫的 SQL 命令的方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-130">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="c7c2d-131">下列選項可供您選擇：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-131">You have the following options:</span></span>

- <span data-ttu-id="c7c2d-132">使用[DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)查詢來傳回實體類型的方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-132">Use the [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) method for queries that return entity types.</span></span> <span data-ttu-id="c7c2d-133">傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您關閉追蹤。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-133">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="c7c2d-134">(請參閱下一節有關[AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)方法。)</span><span class="sxs-lookup"><span data-stu-id="c7c2d-134">(See the following section about the [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) method.)</span></span>
- <span data-ttu-id="c7c2d-135">使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)方法的傳回類型不是實體的查詢。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-135">Use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) method for queries that return types that aren't entities.</span></span> <span data-ttu-id="c7c2d-136">即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-136">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="c7c2d-137">使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非查詢命令。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-137">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) for non-query commands.</span></span>

<span data-ttu-id="c7c2d-138">使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-138">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="c7c2d-139">它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-139">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="c7c2d-140">但有例外狀況時，您需要執行特定 SQL 查詢，以手動方式建立，而且這些方法可讓您處理這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-140">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="c7c2d-141">如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-141">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="c7c2d-142">執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-142">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="c7c2d-143">在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-143">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="c7c2d-144">呼叫查詢會傳回實體</span><span class="sxs-lookup"><span data-stu-id="c7c2d-144">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="c7c2d-145">[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)類別會提供方法可讓您執行查詢，以傳回型別的實體`TEntity`。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-145">The [DbSet&lt;TEntity&gt;](https://msdn.microsoft.com/library/gg696460.aspx) class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="c7c2d-146">若要查看這對您所做的運作將會在變更程式碼`Details`方法`Department`控制站。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-146">To see how this works you'll change the code in the `Details` method of the `Department` controller.</span></span>

<span data-ttu-id="c7c2d-147">在*DepartmentController.cs*，請在`Details`方法，取代`db.Departments.FindAsync`方法呼叫`db.Departments.SqlQuery`方法呼叫，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-147">In *DepartmentController.cs*, in the `Details` method, replace the `db.Departments.FindAsync` method call with a `db.Departments.SqlQuery` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

<span data-ttu-id="c7c2d-148">若要確認新的程式碼運作正常，請選取 [部門]  索引標籤，然後針對其中一個部門選取 [詳細資料] 。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-148">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門詳細資料](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="c7c2d-150">呼叫查詢傳回其他類型的物件</span><span class="sxs-lookup"><span data-stu-id="c7c2d-150">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="c7c2d-151">先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-151">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="c7c2d-152">在程式碼*HomeController.cs*使用 LINQ:</span><span class="sxs-lookup"><span data-stu-id="c7c2d-152">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="c7c2d-153">假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的此資料。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-153">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="c7c2d-154">若要執行您要執行查詢，以傳回實體物件以外的項目，這表示您需要使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-154">To do that you need to run a query that returns something other than entity objects, which means you need to use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) method.</span></span>

<span data-ttu-id="c7c2d-155">在*HomeController.cs*，取代中的 LINQ 陳述式`About`方法使用 SQL 陳述式，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-155">In *HomeController.cs*, replace the LINQ statement in the `About` method with a SQL statement, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

<span data-ttu-id="c7c2d-156">執行 「 關於 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-156">Run the About page.</span></span> <span data-ttu-id="c7c2d-157">它會顯示與之前相同的資料。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-157">It displays the same data it did before.</span></span>

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a><span data-ttu-id="c7c2d-159">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="c7c2d-159">Calling an Update Query</span></span>

<span data-ttu-id="c7c2d-160">假設 Contoso 大學系統管理員想要能夠執行大量變更，在資料庫中，例如變更的每個課程信用額度的數目。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-160">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="c7c2d-161">如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-161">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="c7c2d-162">在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您會變更執行 SQL`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-162">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> <span data-ttu-id="c7c2d-163">網頁看起來將如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-163">The web page will look like the following illustration:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

<span data-ttu-id="c7c2d-165">在*CourseContoller.cs*，新增`UpdateCourseCredits`方法`HttpGet`和`HttpPost`:</span><span class="sxs-lookup"><span data-stu-id="c7c2d-165">In *CourseContoller.cs*, add `UpdateCourseCredits` methods for `HttpGet` and `HttpPost`:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="c7c2d-166">當控制器處理`HttpGet`要求，不會傳回在`ViewBag.RowsAffected`變數，並檢視會顯示空白的文字方塊和送出按鈕，在上圖所示。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-166">When the controller processes an `HttpGet` request, nothing is returned in the `ViewBag.RowsAffected` variable, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="c7c2d-167">當**更新**按一下按鈕時，`HttpPost`呼叫方法時，和`multiplier`已在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-167">When the **Update** button is clicked, the `HttpPost` method is called, and `multiplier` has the value entered in the text box.</span></span> <span data-ttu-id="c7c2d-168">然後執行的程式碼會更新課程並傳回受影響的資料列數目至檢視中的 SQL`ViewBag.RowsAffected`變數。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-168">The code then executes the SQL that updates courses and returns the number of affected rows to the view in the `ViewBag.RowsAffected` variable.</span></span> <span data-ttu-id="c7c2d-169">當檢視中，取得值，變數時，它會顯示更新而不是在文字方塊中的資料列數目並提交 按鈕，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-169">When the view gets a value in that variable, it displays the number of rows updated instead of the text box and submit button, as shown in the following illustration:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

<span data-ttu-id="c7c2d-171">在*CourseController.cs*，以滑鼠右鍵按一下其中一個`UpdateCourseCredits`方法，然後再按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-171">In *CourseController.cs*, right-click one of the `UpdateCourseCredits` methods, and then click **Add View**.</span></span>

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

<span data-ttu-id="c7c2d-173">在*Views\Course\UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-173">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

<span data-ttu-id="c7c2d-174">藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-174">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="c7c2d-175">在文字方塊中輸入數目：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-175">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

<span data-ttu-id="c7c2d-177">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-177">Click **Update**.</span></span> <span data-ttu-id="c7c2d-178">您會看到受影響的資料列數目：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-178">You see the number of rows affected:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

<span data-ttu-id="c7c2d-180">按一下 [回到清單]，以查看課程與已修訂學分數的清單。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-180">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

<span data-ttu-id="c7c2d-182">如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://msdn.microsoft.com/data/jj592907)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-182">For more information about raw SQL queries, see [Raw SQL Queries](https://msdn.microsoft.com/data/jj592907) on MSDN.</span></span>

<a id="notracking"></a>
## <a name="no-tracking-queries"></a><span data-ttu-id="c7c2d-183">不追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="c7c2d-183">No-Tracking Queries</span></span>

<span data-ttu-id="c7c2d-184">當資料庫內容擷取資料表資料列並建立代表他們的實體物件時，根據預設，它會追蹤在記憶體中的實體是否與資料庫中的內容保持同步。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-184">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="c7c2d-185">記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-185">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="c7c2d-186">這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-186">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="c7c2d-187">您可以使用停用記憶體中的實體物件的追蹤[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-187">You can disable tracking of entity objects in memory by using the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method.</span></span> <span data-ttu-id="c7c2d-188">您會想要進行這項操作的常見案例包括下列情況：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-188">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="c7c2d-189">查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-189">A query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="c7c2d-190">您想要將實體附加以更新，但您稍早擷取同一個實體用於不同用途。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-190">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="c7c2d-191">由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-191">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="c7c2d-192">若要處理這種情況的一種方式為使用`AsNoTracking`與前面的查詢選項。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-192">One way to handle this situation is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="c7c2d-193">如需範例，示範如何使用[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法，請參閱[本教學課程的舊版](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-193">For an example that demonstrates how to use the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method, see [the earlier version of this tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span> <span data-ttu-id="c7c2d-194">此版的教學課程不會修改旗標上設定繫結器建立模型中實體的編輯方法，讓它不需要`AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-194">This version of the tutorial doesn't set the Modified flag on a model-binder-created entity in the Edit method, so it doesn't need `AsNoTracking`.</span></span>

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a><span data-ttu-id="c7c2d-195">檢查 SQL 傳送至資料庫</span><span class="sxs-lookup"><span data-stu-id="c7c2d-195">Examining SQL sent to the database</span></span>

<span data-ttu-id="c7c2d-196">有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-196">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="c7c2d-197">在先前的教學課程中您可了解如何在攔截器的程式碼; 執行現在您會看到一些不寫入攔截器程式碼。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-197">In an earlier tutorial you saw how to do that in interceptor code; now you'll see some ways to do it without writing interceptor code.</span></span> <span data-ttu-id="c7c2d-198">再試一次時，您將查看簡單查詢，並查看您新增這類 eager 載入、 篩選和排序選項，它會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-198">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="c7c2d-199">在*控制器/CourseController*，取代`Index`方法取代下列程式碼中，若要暫時停止積極式載入：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-199">In *Controllers/CourseController*, replace the `Index` method with the following code, in order to temporarily stop eager loading:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

<span data-ttu-id="c7c2d-200">現在上設定中斷點`return`陳述式 (F9 與該行上指標)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-200">Now set a breakpoint on the `return` statement (F9 with the cursor on that line).</span></span> <span data-ttu-id="c7c2d-201">按 F5 以偵錯模式執行專案並選取課程索引頁面。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-201">Press F5 to run the project in debug mode, and select the Course Index page.</span></span> <span data-ttu-id="c7c2d-202">當程式碼到達中斷點時，檢查`sql`變數。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-202">When the code reaches the breakpoint, examine the `sql` variable.</span></span> <span data-ttu-id="c7c2d-203">您會看到傳送至 SQL Server 的查詢。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-203">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="c7c2d-204">它是簡單`Select`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-204">It's a simple `Select` statement.</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

<span data-ttu-id="c7c2d-205">按一下放大的類別，以查看中的查詢**文字視覺化檢視**。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-205">Click the magnifying class to see the query in the **Text Visualizer**.</span></span>

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="c7c2d-206">現在您將新增下拉式選單課程索引頁，讓使用者可以篩選特定部門。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-206">Now you'll add a drop-down list to the Courses Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="c7c2d-207">您將項目，所排序的課程，您將指定的積極式載入`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-207">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span>

<span data-ttu-id="c7c2d-208">在*CourseController.cs*，取代`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-208">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="c7c2d-209">在還原中斷點`return`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-209">Restore the breakpoint on the `return` statement.</span></span>

<span data-ttu-id="c7c2d-210">此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-210">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="c7c2d-211">如果選取任何項目，這個參數會是 null。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-211">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="c7c2d-212">A`SelectList`集合，其中包含所有部門傳遞至檢視的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-212">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="c7c2d-213">參數傳遞至`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-213">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="c7c2d-214">如`Get`方法`Course`儲存機制、 程式碼會指定篩選條件運算式、 排序次序，以及載入 eager`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-214">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="c7c2d-215">篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-215">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="c7c2d-216">在*Views\Course\Index.cshtml*之前開啟,`table`標記中，加入下列程式碼，建立下拉式清單及提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-216">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="c7c2d-217">中斷點仍組，請執行課程索引頁面。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-217">With the breakpoint still set, run the Course Index page.</span></span> <span data-ttu-id="c7c2d-218">繼續透過程式碼叫用中斷點的行，第一次，讓瀏覽器中會顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-218">Continue through the first times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="c7c2d-219">從下拉式清單中選取部門並按一下**篩選**:</span><span class="sxs-lookup"><span data-stu-id="c7c2d-219">Select a department from the drop-down list and click **Filter**:</span></span>

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

<span data-ttu-id="c7c2d-221">這次下拉式清單的部門查詢就是第一個中斷點。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-221">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="c7c2d-222">略過，並檢視`query`變數在下一次程式碼中斷點時若要查看哪些`Course`查詢現在看起來像。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-222">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="c7c2d-223">您會看到類似如下：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-223">You'll see something like the following:</span></span>

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

<span data-ttu-id="c7c2d-224">您可以看到查詢，現在是`JOIN`載入的查詢`Department`資料連同`Course`資料，以及它包含`WHERE`子句。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-224">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<span data-ttu-id="c7c2d-225">移除`var sql = courses.ToString()`列。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-225">Remove the `var sql = courses.ToString()` line.</span></span>

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="c7c2d-226">存放庫和工作單元模式</span><span class="sxs-lookup"><span data-stu-id="c7c2d-226">Repository and unit of work patterns</span></span>

<span data-ttu-id="c7c2d-227">許多開發人員撰寫程式碼以實作存放庫和工作單元模式，作為使用 Entity Framework 之程式碼周圍的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-227">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="c7c2d-228">這些模式主要用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-228">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="c7c2d-229">實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-229">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="c7c2d-230">不過，撰寫額外的程式碼來實作這些模式做不一定有數種原因使用 EF，應用程式的最佳選擇：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-230">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

- <span data-ttu-id="c7c2d-231">EF 內容類別本身會隔離您的程式碼與資料存放區特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-231">The EF context class itself insulates your code from data-store-specific code.</span></span>
- <span data-ttu-id="c7c2d-232">EF 內容類別可作為您使用 EF 進行之資料庫更新的工作單元類別。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-232">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>
- <span data-ttu-id="c7c2d-233">Entity Framework 6 中導入的功能可讓您更輕鬆地實作 TDD，而不需要撰寫的程式碼儲存機制。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-233">Features introduced in Entity Framework 6 make it easier to implement TDD without writing repository code.</span></span>

<span data-ttu-id="c7c2d-234">如需如何實作儲存機制和單位的工作模式的詳細資訊，請參閱[此教學課程系列的 Entity Framework 5 新版](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-234">For more information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="c7c2d-235">在 Entity Framework 6 中實作 TDD 的方式的相關資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-235">For information about ways to implement TDD in Entity Framework 6, see the following resources:</span></span>

- [<span data-ttu-id="c7c2d-236">如何 EF6 讓 Mocking DbSets 更輕鬆地</span><span class="sxs-lookup"><span data-stu-id="c7c2d-236">How EF6 Enables Mocking DbSets more easily</span></span>](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [<span data-ttu-id="c7c2d-237">測試模擬架構</span><span class="sxs-lookup"><span data-stu-id="c7c2d-237">Testing with a mocking framework</span></span>](https://msdn.microsoft.com/data/dn314429)
- [<span data-ttu-id="c7c2d-238">使用您自己的測試複本測試</span><span class="sxs-lookup"><span data-stu-id="c7c2d-238">Testing with your own test doubles</span></span>](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a><span data-ttu-id="c7c2d-239">Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="c7c2d-239">Proxy classes</span></span>

<span data-ttu-id="c7c2d-240">當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立這些動態產生的衍生型別，可做為實體的 proxy 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-240">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="c7c2d-241">例如，請參閱下列兩個偵錯工具映像。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-241">For example, see the following two debugger images.</span></span> <span data-ttu-id="c7c2d-242">在第一個映像，您會看到`student`變數是預期`Student`您具現化實體之後，立即輸入。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-242">In the first image, you see that the `student` variable is the expected `Student` type immediately after you instantiate the entity.</span></span> <span data-ttu-id="c7c2d-243">在第二個影像中，已從資料庫讀取學生實體使用 EF 之後您會看到 proxy 類別。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-243">In the second image, after EF has been used to read a student entity from the database, you see the proxy class.</span></span>

![Proxy 類別之前](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Proxy 類別之後](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="c7c2d-246">這個 proxy 類別會覆寫要插入在屬性經過存取時，自動執行動作的攔截程序之實體的某些虛擬屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-246">This proxy class overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="c7c2d-247">一個用於這項機制的函式是消極式載入。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-247">One function this mechanism is used for is lazy loading.</span></span>

<span data-ttu-id="c7c2d-248">大部分的情況下您不需要注意這項使用的 proxy，但有例外狀況：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-248">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="c7c2d-249">在某些情況下您可能想要防止 Entity Framework 建立 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-249">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="c7c2d-250">例如，當您序列化的實體您通常會想 POCO 類別，而不是 proxy 類別。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-250">For example, when you're serializing entities you generally want the POCO classes, not the proxy classes.</span></span> <span data-ttu-id="c7c2d-251">一種方式以避免發生序列化問題為序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-251">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects, as shown in the [Using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial.</span></span> <span data-ttu-id="c7c2d-252">另一個方法是[停用 proxy 建立](https://msdn.microsoft.com/data/jj592886.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-252">Another way is to [disable proxy creation](https://msdn.microsoft.com/data/jj592886.aspx).</span></span>
- <span data-ttu-id="c7c2d-253">當您具現化的實體類別使用`new`運算子，就無法取得 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-253">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="c7c2d-254">這表示您不先取得功能，例如消極式載入和自動變更追蹤。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-254">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="c7c2d-255">這通常是好;您通常不需要延遲載入，因為您要建立新的實體不在資料庫中，而且通常不需要變更追蹤，如果您要明確地標示為實體`Added`。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-255">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="c7c2d-256">不過，如果您需要消極式載入，而您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy[建立](https://msdn.microsoft.com/library/gg679504.aspx)方法`DbSet`類別。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-256">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the [Create](https://msdn.microsoft.com/library/gg679504.aspx) method of the `DbSet` class.</span></span>
- <span data-ttu-id="c7c2d-257">您可能想要從的 proxy 型別取得實際的實體類型。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-257">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="c7c2d-258">您可以使用[GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)方法`ObjectContext`類別取得的 proxy 型別執行個體的實際實體類型。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-258">You can use the [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="c7c2d-259">如需詳細資訊，請參閱[使用 Proxy](https://msdn.microsoft.com/data/JJ592886.aspx) MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-259">For more information, see [Working with Proxies](https://msdn.microsoft.com/data/JJ592886.aspx) on MSDN.</span></span>

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a><span data-ttu-id="c7c2d-260">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="c7c2d-260">Automatic change detection</span></span>

<span data-ttu-id="c7c2d-261">Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-261">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="c7c2d-262">查詢或附加實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-262">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="c7c2d-263">會導致自動變更偵測的一些方法如下：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-263">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="c7c2d-264">如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-264">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) property.</span></span> <span data-ttu-id="c7c2d-265">如需詳細資訊，請參閱[自動偵測變更](https://msdn.microsoft.com/data/jj556205)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-265">For more information, see [Automatically Detecting Changes](https://msdn.microsoft.com/data/jj556205) on MSDN.</span></span>

<a id="validation"></a>
## <a name="automatic-validation"></a><span data-ttu-id="c7c2d-266">自動驗證</span><span class="sxs-lookup"><span data-stu-id="c7c2d-266">Automatic validation</span></span>

<span data-ttu-id="c7c2d-267">當您呼叫`SaveChanges`方法，依預設 Entity Framework 中所有已變更實體的所有屬性的資料會先驗證更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-267">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="c7c2d-268">如果您已更新大量實體且您已驗證資料，這項工作不需要確定儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-268">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="c7c2d-269">您可以使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-269">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) property.</span></span> <span data-ttu-id="c7c2d-270">如需詳細資訊，請參閱[驗證](https://msdn.microsoft.com/data/gg193959)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-270">For more information, see [Validation](https://msdn.microsoft.com/data/gg193959) on MSDN.</span></span>

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a><span data-ttu-id="c7c2d-271">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="c7c2d-271">Entity Framework Power Tools</span></span>

<span data-ttu-id="c7c2d-272">[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)用來建立資料模型圖表的 Visual Studio 增益集顯示在這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-272">[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) is a Visual Studio add-in that was used to create the data model diagrams shown in these tutorials.</span></span> <span data-ttu-id="c7c2d-273">工具也可以執行其他函式，例如產生實體類別根據現有的資料庫中的資料表，讓您可以使用 Code first 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-273">The tools can also do other function such as generate entity classes based on the tables in an existing database so that you can use the database with Code First.</span></span> <span data-ttu-id="c7c2d-274">安裝工具之後，其他選項會出現在內容功能表。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-274">After you install the tools, some additional options appear in context menus.</span></span> <span data-ttu-id="c7c2d-275">例如，當您以滑鼠右鍵按一下您的內容類別中**方案總管 中**，會產生圖表選項。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-275">For example, when you right-click your context class in **Solution Explorer**, you get an option to generate a diagram.</span></span> <span data-ttu-id="c7c2d-276">當您使用 Code First 時您無法變更圖表中的資料模型，但是您可以移動項目，讓您更輕鬆地了解。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-276">When you're using Code First you can't change the data model in the diagram, but you can move things around to make it easier to understand.</span></span>

![在內容功能表中的 EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF 圖表](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a><span data-ttu-id="c7c2d-279">Entity Framework 原始碼</span><span class="sxs-lookup"><span data-stu-id="c7c2d-279">Entity Framework source code</span></span>

<span data-ttu-id="c7c2d-280">Entity Framework 6 的原始程式碼位於[GitHub](https://github.com/aspnet/EntityFramework6)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-280">The source code for Entity Framework 6 is available at [GitHub](https://github.com/aspnet/EntityFramework6).</span></span> <span data-ttu-id="c7c2d-281">您可以檔 bug，而且您可能會造成您自己的 EF 原始碼的增強功能。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-281">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="c7c2d-282">雖然開啟原始碼時，Entity Framework 則完全支援以 Microsoft 產品。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-282">Although the source code is open, Entity Framework is fully supported as a Microsoft product.</span></span> <span data-ttu-id="c7c2d-283">Microsoft Entity Framework 小組將控制接受哪些貢獻，並測試所有的程式碼變更以確保每次發行的品質。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-283">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="c7c2d-284">總結</span><span class="sxs-lookup"><span data-stu-id="c7c2d-284">Summary</span></span>

<span data-ttu-id="c7c2d-285">如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-285">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="c7c2d-286">如需如何搭配使用 Entity Framework 資料的詳細資訊，請參閱[EF MSDN 上的文件頁面](https://msdn.microsoft.com/data/ee712907)和[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-286">For more information about how to work with data using the Entity Framework, see the [EF documentation page on MSDN](https://msdn.microsoft.com/data/ee712907) and [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="c7c2d-287">如需如何建置它之後部署 web 應用程式的詳細資訊，請參閱[ASP.NET Web 部署的建議資源](../../../../whitepapers/aspnet-web-deployment-content-map.md)MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-287">For more information about how to deploy your web application after you've built it, see [ASP.NET Web Deployment - Recommended Resources](../../../../whitepapers/aspnet-web-deployment-content-map.md) in the MSDN Library.</span></span>

<span data-ttu-id="c7c2d-288">如需有關 MVC、 驗證和授權，例如其他主題，請參閱[ASP.NET MVC-建議資源](../recommended-resources-for-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-288">For information about other topics related to MVC, such as authentication and authorization, see the [ASP.NET MVC - Recommended Resources](../recommended-resources-for-mvc.md).</span></span>

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a><span data-ttu-id="c7c2d-289">感謝</span><span class="sxs-lookup"><span data-stu-id="c7c2d-289">Acknowledgments</span></span>

- <span data-ttu-id="c7c2d-290">Tom Dykstra 寫入此教學課程中的原始版本、 共同撰寫 EF 5 更新，並撰寫 EF 6 更新。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-290">Tom Dykstra wrote the original version of this tutorial, co-authored the EF 5 update, and wrote the EF 6 update.</span></span> <span data-ttu-id="c7c2d-291">Tom 是資深的開發寫入器上的 Microsoft Web 平台和工具的內容團隊。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-291">Tom is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="c7c2d-292">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 未的大部分工作更新的教學課程 EF 5 和 MVC 4 和共同撰寫 EF 6 更新。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-292">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) did most of the work updating the tutorial for EF 5 and MVC 4 and co-authored the EF 6 update.</span></span> <span data-ttu-id="c7c2d-293">Rick 是將焦點放在 Azure 和 MVC Microsoft 資深程式寫入器。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-293">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="c7c2d-294">[Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助程式碼檢閱，並協助偵錯移轉，我們已更新本教學課程 EF 5 和 EF 6 時，會發生的許多問題。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-294">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5 and EF 6.</span></span>

<a id="vb"></a>
## <a name="vb"></a><span data-ttu-id="c7c2d-295">VB</span><span class="sxs-lookup"><span data-stu-id="c7c2d-295">VB</span></span>

<span data-ttu-id="c7c2d-296">本教學課程原本所產生的 EF 4.1，當我們提供 C# 和 VB 專案版本的下載已完成。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-296">When the tutorial was originally produced for EF 4.1, we provided both C# and VB versions of the completed download project.</span></span> <span data-ttu-id="c7c2d-297">由於時間限制和其他優先順序我們您尚未這樣做，此版本。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-297">Due to time limitations and other priorities we have not done that for this version.</span></span> <span data-ttu-id="c7c2d-298">如果您建立使用這些教學課程將 VB 專案，願意，與其他人共用，請讓我們知道。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-298">If you build a VB project using these tutorials and would be willing to share that with others, please let us know.</span></span>

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a><span data-ttu-id="c7c2d-299">常見的錯誤，和它們的因應措施或解決方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-299">Common errors, and solutions or workarounds for them</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="c7c2d-300">無法建立或陰影複製</span><span class="sxs-lookup"><span data-stu-id="c7c2d-300">Cannot create/shadow copy</span></span>

<span data-ttu-id="c7c2d-301">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-301">Error Message:</span></span>

> <span data-ttu-id="c7c2d-302">無法建立或陰影複製 '&lt;filename&gt;' 已存在該檔案。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-302">Cannot create/shadow copy '&lt;filename&gt;' when that file already exists.</span></span>


<span data-ttu-id="c7c2d-303">方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-303">Solution</span></span>

<span data-ttu-id="c7c2d-304">等候數秒鐘，並重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-304">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="c7c2d-305">更新資料庫無法辨識</span><span class="sxs-lookup"><span data-stu-id="c7c2d-305">Update-Database not recognized</span></span>

<span data-ttu-id="c7c2d-306">錯誤訊息 (從`Update-Database`PMC 命令):</span><span class="sxs-lookup"><span data-stu-id="c7c2d-306">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="c7c2d-307">'Update-database' 詞彙無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-307">The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="c7c2d-308">請檢查名稱拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-308">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>


<span data-ttu-id="c7c2d-309">方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-309">Solution</span></span>

<span data-ttu-id="c7c2d-310">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-310">Exit Visual Studio.</span></span> <span data-ttu-id="c7c2d-311">重新開啟專案，然後再試一次。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-311">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="c7c2d-312">驗證失敗</span><span class="sxs-lookup"><span data-stu-id="c7c2d-312">Validation failed</span></span>

<span data-ttu-id="c7c2d-313">錯誤訊息 (從`Update-Database`PMC 命令):</span><span class="sxs-lookup"><span data-stu-id="c7c2d-313">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="c7c2d-314">一個或多個實體的驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-314">Validation failed for one or more entities.</span></span> <span data-ttu-id="c7c2d-315">請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-315">See 'EntityValidationErrors' property for more details.</span></span>


<span data-ttu-id="c7c2d-316">方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-316">Solution</span></span>

<span data-ttu-id="c7c2d-317">發生此問題的其中一個原因是驗證錯誤時`Seed`方法執行。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-317">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="c7c2d-318">請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-318">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="c7c2d-319">HTTP 500.19 錯誤</span><span class="sxs-lookup"><span data-stu-id="c7c2d-319">HTTP 500.19 error</span></span>

<span data-ttu-id="c7c2d-320">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-320">Error Message:</span></span>

> <span data-ttu-id="c7c2d-321">HTTP 錯誤 500.19-內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="c7c2d-321">HTTP Error 500.19 - Internal Server Error</span></span>  
> <span data-ttu-id="c7c2d-322">無法存取要求的網頁，因為該頁面的相關的組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-322">The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>


<span data-ttu-id="c7c2d-323">方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-323">Solution</span></span>

<span data-ttu-id="c7c2d-324">您可以取得此錯誤的其中一種方式是方案的從多個複本時，每個使用相同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-324">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="c7c2d-325">您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動的專案正努力解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-325">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project you're working on.</span></span> <span data-ttu-id="c7c2d-326">如果無法解決問題，請變更通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-326">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="c7c2d-327">以滑鼠右鍵按一下專案檔，然後按一下 屬性。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-327">Right click on the project file and then click properties.</span></span> <span data-ttu-id="c7c2d-328">選取**Web**索引標籤，然後變更 連接埠號碼**專案 Url**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-328">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="c7c2d-329">搜尋 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="c7c2d-329">Error locating SQL Server instance</span></span>

<span data-ttu-id="c7c2d-330">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="c7c2d-330">Error Message:</span></span>

> <span data-ttu-id="c7c2d-331">建立與 SQL Server　的連線時，發生與網路相關的錯誤或是執行個體特有的錯誤。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-331">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="c7c2d-332">找不到或無法存取伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-332">The server was not found or was not accessible.</span></span> <span data-ttu-id="c7c2d-333">確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線</span><span class="sxs-lookup"><span data-stu-id="c7c2d-333">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="c7c2d-334">(提供者：SQL 網路介面，錯誤：26 - 搜尋指定的伺服器/執行個體時發生錯誤)</span><span class="sxs-lookup"><span data-stu-id="c7c2d-334">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>


<span data-ttu-id="c7c2d-335">方案</span><span class="sxs-lookup"><span data-stu-id="c7c2d-335">Solution</span></span>

<span data-ttu-id="c7c2d-336">檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-336">Check the connection string.</span></span> <span data-ttu-id="c7c2d-337">如果您已經手動刪除資料庫，變更建構字串的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="c7c2d-337">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c7c2d-338">上一步</span><span class="sxs-lookup"><span data-stu-id="c7c2d-338">Previous</span></span>](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
