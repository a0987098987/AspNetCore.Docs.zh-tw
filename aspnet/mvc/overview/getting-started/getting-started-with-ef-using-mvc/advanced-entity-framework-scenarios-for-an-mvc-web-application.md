---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 教學課程：深入了解的 MVC 5 Web 應用程式的進階 EF 案例
description: 本教學課程包含介紹數個會留意在超出開發 ASP.NET web 應用程式使用 Entity Framework Code First 的基本概念時很有用的主題。
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d0208c8890467ec6044d807aeee7c7ae02e18790
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889999"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a><span data-ttu-id="0d55b-103">教學課程：深入了解的 MVC 5 Web 應用程式的進階 EF 案例</span><span class="sxs-lookup"><span data-stu-id="0d55b-103">Tutorial: Learn about advanced EF Scenarios for an MVC 5 Web app</span></span>

<span data-ttu-id="0d55b-104">在上一個教學課程中，您會實作每個階層的資料表繼承。</span><span class="sxs-lookup"><span data-stu-id="0d55b-104">In the previous tutorial you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="0d55b-105">本教學課程包含介紹數個會留意在超出開發 ASP.NET web 應用程式使用 Entity Framework Code First 的基本概念時很有用的主題。</span><span class="sxs-lookup"><span data-stu-id="0d55b-105">This tutorial includes introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span> <span data-ttu-id="0d55b-106">第一個的幾節有逐步引導您完成程式碼的逐步指示，並使用 Visual Studio 來完成工作的下列各節引進數個主題簡要介紹其後加上行連結資源，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0d55b-106">The first few sections have step-by-step instructions that walk you through the code and using Visual Studio to complete tasks The sections that follow introduce several topics with brief introductions followed by links to resources for more information.</span></span>

<span data-ttu-id="0d55b-107">對於大部分的這些主題，您將使用您已建立的頁面。</span><span class="sxs-lookup"><span data-stu-id="0d55b-107">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="0d55b-108">若要執行大量更新使用原始 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的學分數：</span><span class="sxs-lookup"><span data-stu-id="0d55b-108">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="0d55b-110">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="0d55b-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d55b-111">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-111">Perform raw SQL queries</span></span>
> * <span data-ttu-id="0d55b-112">執行無追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-112">Perform no-tracking queries</span></span>
> * <span data-ttu-id="0d55b-113">檢查 SQL 查詢傳送至資料庫</span><span class="sxs-lookup"><span data-stu-id="0d55b-113">Examine SQL queries sent to database</span></span>

<span data-ttu-id="0d55b-114">您也了解：</span><span class="sxs-lookup"><span data-stu-id="0d55b-114">You also learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d55b-115">建立一個抽象層</span><span class="sxs-lookup"><span data-stu-id="0d55b-115">Creating an abstraction layer</span></span>
> * <span data-ttu-id="0d55b-116">Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="0d55b-116">Proxy classes</span></span>
> * <span data-ttu-id="0d55b-117">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="0d55b-117">Automatic change detection</span></span>
> * <span data-ttu-id="0d55b-118">自動驗證</span><span class="sxs-lookup"><span data-stu-id="0d55b-118">Automatic validation</span></span>
> * <span data-ttu-id="0d55b-119">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="0d55b-119">Entity Framework Power Tools</span></span>
> * <span data-ttu-id="0d55b-120">Entity Framework 原始碼</span><span class="sxs-lookup"><span data-stu-id="0d55b-120">Entity Framework source code</span></span>

## <a name="prerequisite"></a><span data-ttu-id="0d55b-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="0d55b-121">Prerequisite</span></span>

* [<span data-ttu-id="0d55b-122">實作繼承</span><span class="sxs-lookup"><span data-stu-id="0d55b-122">Implementing Inheritance</span></span>](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a><span data-ttu-id="0d55b-123">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-123">Perform raw SQL queries</span></span>

<span data-ttu-id="0d55b-124">Entity Framework 程式碼的第一個 API 包含可讓您的 SQL 命令直接傳遞至資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="0d55b-124">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="0d55b-125">下列選項可供您選擇：</span><span class="sxs-lookup"><span data-stu-id="0d55b-125">You have the following options:</span></span>

- <span data-ttu-id="0d55b-126">使用[DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)方法會傳回實體類型的查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-126">Use the [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) method for queries that return entity types.</span></span> <span data-ttu-id="0d55b-127">傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤的資料庫內容除非您關閉追蹤。</span><span class="sxs-lookup"><span data-stu-id="0d55b-127">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="0d55b-128">(請參閱下一節，關於[AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)方法。)</span><span class="sxs-lookup"><span data-stu-id="0d55b-128">(See the following section about the [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) method.)</span></span>
- <span data-ttu-id="0d55b-129">使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)方法的傳回類型不是實體的查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-129">Use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) method for queries that return types that aren't entities.</span></span> <span data-ttu-id="0d55b-130">即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="0d55b-130">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="0d55b-131">使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非查詢命令。</span><span class="sxs-lookup"><span data-stu-id="0d55b-131">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) for non-query commands.</span></span>

<span data-ttu-id="0d55b-132">使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。</span><span class="sxs-lookup"><span data-stu-id="0d55b-132">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="0d55b-133">它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="0d55b-133">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="0d55b-134">但有例外情況時您需要執行特定 SQL 查詢您以手動方式建立，而且這些方法可讓您處理這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0d55b-134">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="0d55b-135">如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="0d55b-135">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="0d55b-136">執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="0d55b-136">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="0d55b-137">在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-137">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="0d55b-138">呼叫查詢，傳回的實體</span><span class="sxs-lookup"><span data-stu-id="0d55b-138">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="0d55b-139">[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)類別會提供您可用來執行可傳回實體類型的查詢方法`TEntity`。</span><span class="sxs-lookup"><span data-stu-id="0d55b-139">The [DbSet&lt;TEntity&gt;](https://msdn.microsoft.com/library/gg696460.aspx) class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="0d55b-140">若要查看這對您所做的運作將會變更中的程式碼`Details`方法的`Department`控制站。</span><span class="sxs-lookup"><span data-stu-id="0d55b-140">To see how this works you'll change the code in the `Details` method of the `Department` controller.</span></span>

<span data-ttu-id="0d55b-141">在  *DepartmentController.cs*，請在`Details`方法，取代`db.Departments.FindAsync`方法呼叫與`db.Departments.SqlQuery`方法呼叫，如下列醒目提示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="0d55b-141">In *DepartmentController.cs*, in the `Details` method, replace the `db.Departments.FindAsync` method call with a `db.Departments.SqlQuery` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

<span data-ttu-id="0d55b-142">若要確認新的程式碼運作正常，請選取 [部門]  索引標籤，然後針對其中一個部門選取 [詳細資料] 。</span><span class="sxs-lookup"><span data-stu-id="0d55b-142">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span> <span data-ttu-id="0d55b-143">請確定所有的資料會顯示如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="0d55b-143">Make sure all of the data displays as expected.</span></span>

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="0d55b-144">呼叫查詢，傳回其他類型的物件</span><span class="sxs-lookup"><span data-stu-id="0d55b-144">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="0d55b-145">先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。</span><span class="sxs-lookup"><span data-stu-id="0d55b-145">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="0d55b-146">在程式碼*HomeController.cs*使用 LINQ:</span><span class="sxs-lookup"><span data-stu-id="0d55b-146">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="0d55b-147">假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的這項資料。</span><span class="sxs-lookup"><span data-stu-id="0d55b-147">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="0d55b-148">若要執行，您需要執行的查詢會傳回實體物件以外的項目，這表示您需要使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="0d55b-148">To do that you need to run a query that returns something other than entity objects, which means you need to use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) method.</span></span>

<span data-ttu-id="0d55b-149">在  *HomeController.cs*，取代中的 LINQ 陳述式`About`方法使用 SQL 陳述式，如下列醒目提示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="0d55b-149">In *HomeController.cs*, replace the LINQ statement in the `About` method with a SQL statement, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

<span data-ttu-id="0d55b-150">執行 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="0d55b-150">Run the About page.</span></span> <span data-ttu-id="0d55b-151">請確認它會顯示相同的資料，跟之前一樣。</span><span class="sxs-lookup"><span data-stu-id="0d55b-151">Verify that it displays the same data it did before.</span></span>

### <a name="calling-an-update-query"></a><span data-ttu-id="0d55b-152">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-152">Calling an Update Query</span></span>

<span data-ttu-id="0d55b-153">假設 Contoso 大學的系統管理員想要能夠在資料庫中，例如變更每個課程的學分數執行大量變更。</span><span class="sxs-lookup"><span data-stu-id="0d55b-153">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="0d55b-154">如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。</span><span class="sxs-lookup"><span data-stu-id="0d55b-154">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="0d55b-155">在本節中，您將實作網頁，可讓使用者指定要變更所有課程的學分數的因數，您會變更執行 SQL`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0d55b-155">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> 

<span data-ttu-id="0d55b-156">在  *CourseContoller.cs*，新增`UpdateCourseCredits`方法`HttpGet`和`HttpPost`:</span><span class="sxs-lookup"><span data-stu-id="0d55b-156">In *CourseContoller.cs*, add `UpdateCourseCredits` methods for `HttpGet` and `HttpPost`:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="0d55b-157">控制器會處理當`HttpGet`要求，不會傳回在`ViewBag.RowsAffected`變數，並檢視會顯示空白的文字方塊和提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="0d55b-157">When the controller processes an `HttpGet` request, nothing is returned in the `ViewBag.RowsAffected` variable, and the view displays an empty text box and a submit button.</span></span>

<span data-ttu-id="0d55b-158">當**更新**按一下按鈕時，`HttpPost`呼叫方法時，和`multiplier`已在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="0d55b-158">When the **Update** button is clicked, the `HttpPost` method is called, and `multiplier` has the value entered in the text box.</span></span> <span data-ttu-id="0d55b-159">程式碼接著會更新課程，並會受影響的資料列數目傳回至檢視中的 SQL`ViewBag.RowsAffected`變數。</span><span class="sxs-lookup"><span data-stu-id="0d55b-159">The code then executes the SQL that updates courses and returns the number of affected rows to the view in the `ViewBag.RowsAffected` variable.</span></span> <span data-ttu-id="0d55b-160">檢視在該變數取得值，它會顯示更新而不是文字方塊中的資料列數目，並提交 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0d55b-160">When the view gets a value in that variable, it displays the number of rows updated instead of the text box and submit button.</span></span>

<span data-ttu-id="0d55b-161">在  *CourseController.cs*，以滑鼠右鍵按一下其中一個`UpdateCourseCredits`方法，然後再按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="0d55b-161">In *CourseController.cs*, right-click one of the `UpdateCourseCredits` methods, and then click **Add View**.</span></span> <span data-ttu-id="0d55b-162">**加入檢視**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="0d55b-162">The **Add View** dialog appears.</span></span> <span data-ttu-id="0d55b-163">保留預設值，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="0d55b-163">Leave the defaults and select **Add**.</span></span>

<span data-ttu-id="0d55b-164">在  *Views\Course\UpdateCourseCredits.cshtml*，以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="0d55b-164">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

<span data-ttu-id="0d55b-165">藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。</span><span class="sxs-lookup"><span data-stu-id="0d55b-165">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="0d55b-166">在文字方塊中輸入數目：</span><span class="sxs-lookup"><span data-stu-id="0d55b-166">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="0d55b-168">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="0d55b-168">Click **Update**.</span></span> <span data-ttu-id="0d55b-169">您會看到受影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="0d55b-169">You see the number of rows affected.</span></span>

<span data-ttu-id="0d55b-170">按一下 [回到清單]，以查看課程與已修訂學分數的清單。</span><span class="sxs-lookup"><span data-stu-id="0d55b-170">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="0d55b-171">如需原始 SQL 查詢的詳細資訊，請參閱[原始 SQL 查詢](https://msdn.microsoft.com/data/jj592907)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="0d55b-171">For more information about raw SQL queries, see [Raw SQL Queries](https://msdn.microsoft.com/data/jj592907) on MSDN.</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="0d55b-172">無追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-172">No-tracking queries</span></span>

<span data-ttu-id="0d55b-173">當資料庫內容擷取資料表資料列並建立代表他們的實體物件時，根據預設，它會追蹤在記憶體中的實體是否與資料庫中的內容保持同步。</span><span class="sxs-lookup"><span data-stu-id="0d55b-173">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="0d55b-174">記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。</span><span class="sxs-lookup"><span data-stu-id="0d55b-174">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="0d55b-175">這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。</span><span class="sxs-lookup"><span data-stu-id="0d55b-175">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="0d55b-176">您可以使用連線，停用追蹤記憶體中的實體物件[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="0d55b-176">You can disable tracking of entity objects in memory by using the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method.</span></span> <span data-ttu-id="0d55b-177">您會想要進行這項操作的常見案例包括下列情況：</span><span class="sxs-lookup"><span data-stu-id="0d55b-177">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="0d55b-178">查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。</span><span class="sxs-lookup"><span data-stu-id="0d55b-178">A query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="0d55b-179">您想要將實體附加才能加以更新，但您稍早針對不同用途擷取相同的實體。</span><span class="sxs-lookup"><span data-stu-id="0d55b-179">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="0d55b-180">由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。</span><span class="sxs-lookup"><span data-stu-id="0d55b-180">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="0d55b-181">若要處理這種情況的一個方式是使用`AsNoTracking`選項與先前的查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-181">One way to handle this situation is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="0d55b-182">如需範例，示範如何使用[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法，請參閱[本教學課程的舊版](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-182">For an example that demonstrates how to use the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method, see [the earlier version of this tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span> <span data-ttu-id="0d55b-183">本教學課程的這個版本不修改的旗標實體上設定模型繫結器建立在編輯方法中，因此它不需要`AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="0d55b-183">This version of the tutorial doesn't set the Modified flag on a model-binder-created entity in the Edit method, so it doesn't need `AsNoTracking`.</span></span>

## <a name="examine-sql-sent-to-database"></a><span data-ttu-id="0d55b-184">檢查傳送至資料庫的 SQL</span><span class="sxs-lookup"><span data-stu-id="0d55b-184">Examine SQL sent to database</span></span>

<span data-ttu-id="0d55b-185">有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。</span><span class="sxs-lookup"><span data-stu-id="0d55b-185">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="0d55b-186">在先前的教學課程中，您看到如何執行該作業在攔截器程式碼現在，您會看到一些方法，請在不撰寫攔截器程式碼。</span><span class="sxs-lookup"><span data-stu-id="0d55b-186">In an earlier tutorial you saw how to do that in interceptor code; now you'll see some ways to do it without writing interceptor code.</span></span> <span data-ttu-id="0d55b-187">若要這麼做時，您將看看一個簡單的查詢，並查看當您加入這類的積極式載入、 篩選和排序選項，它會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="0d55b-187">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="0d55b-188">在 *控制器/CourseController*，取代`Index`方法取代下列程式碼中，若要暫時停止積極式載入：</span><span class="sxs-lookup"><span data-stu-id="0d55b-188">In *Controllers/CourseController*, replace the `Index` method with the following code, in order to temporarily stop eager loading:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

<span data-ttu-id="0d55b-189">現在上設定中斷點`return`陳述式 (在該行游標 F9)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-189">Now set a breakpoint on the `return` statement (F9 with the cursor on that line).</span></span> <span data-ttu-id="0d55b-190">按下**F5**執行偵錯模式中的專案，然後選取 [課程索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="0d55b-190">Press **F5** to run the project in debug mode, and select the Course Index page.</span></span> <span data-ttu-id="0d55b-191">當程式碼到達中斷點時，檢查`sql`變數。</span><span class="sxs-lookup"><span data-stu-id="0d55b-191">When the code reaches the breakpoint, examine the `sql` variable.</span></span> <span data-ttu-id="0d55b-192">您會看到傳送到 SQL Server 的查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-192">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="0d55b-193">它是一項簡單`Select`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0d55b-193">It's a simple `Select` statement.</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

<span data-ttu-id="0d55b-194">按一下放大鏡，請參閱中的查詢**文字視覺化檢視**。</span><span class="sxs-lookup"><span data-stu-id="0d55b-194">Click the magnifying glass to see the query in the **Text Visualizer**.</span></span>

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="0d55b-195">現在將下拉式清單加入 Courses 索引 頁面，讓使用者可以篩選特定的部門。</span><span class="sxs-lookup"><span data-stu-id="0d55b-195">Now you'll add a drop-down list to the Courses Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="0d55b-196">您將項目，所排序的課程，並指定積極式載入`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-196">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span>

<span data-ttu-id="0d55b-197">在  *CourseController.cs*，取代`Index`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="0d55b-197">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="0d55b-198">還原上的中斷點`return`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0d55b-198">Restore the breakpoint on the `return` statement.</span></span>

<span data-ttu-id="0d55b-199">此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。</span><span class="sxs-lookup"><span data-stu-id="0d55b-199">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="0d55b-200">如果未選取，則會有此參數是 null。</span><span class="sxs-lookup"><span data-stu-id="0d55b-200">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="0d55b-201">A`SelectList`集合，其中包含所有部門的下拉式清單，會傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="0d55b-201">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="0d55b-202">參數傳遞給`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。</span><span class="sxs-lookup"><span data-stu-id="0d55b-202">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="0d55b-203">針對`Get`方法`Course`存放庫中，程式碼會指定篩選條件運算式、 排序次序，以及積極式載入`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-203">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="0d55b-204">篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-204">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="0d55b-205">在  *Views\Course\Index.cshtml*，開啟之前，立即`table`標記中加入下列程式碼，建立下拉式清單和提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="0d55b-205">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="0d55b-206">使用中斷點仍組，請執行 [課程索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="0d55b-206">With the breakpoint still set, run the Course Index page.</span></span> <span data-ttu-id="0d55b-207">使瀏覽器中會顯示頁面時，請繼續透過程式碼叫用中斷點的行，第一次。</span><span class="sxs-lookup"><span data-stu-id="0d55b-207">Continue through the first times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="0d55b-208">從下拉式清單中選取一個部門，然後按一下**篩選**。</span><span class="sxs-lookup"><span data-stu-id="0d55b-208">Select a department from the drop-down list and click **Filter**.</span></span>

<span data-ttu-id="0d55b-209">這次的第一個中斷點會部門查詢的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="0d55b-209">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="0d55b-210">略過，並檢視`query`變數在下一次程式碼觸達中斷點時若要查看`Course`現在看起來就像查詢。</span><span class="sxs-lookup"><span data-stu-id="0d55b-210">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="0d55b-211">您會看到類似如下：</span><span class="sxs-lookup"><span data-stu-id="0d55b-211">You'll see something like the following:</span></span>

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

<span data-ttu-id="0d55b-212">您可以看到查詢現在`JOIN`載入的查詢`Department`資料連同`Course`資料，且其中包含`WHERE`子句。</span><span class="sxs-lookup"><span data-stu-id="0d55b-212">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<span data-ttu-id="0d55b-213">移除`var sql = courses.ToString()`列。</span><span class="sxs-lookup"><span data-stu-id="0d55b-213">Remove the `var sql = courses.ToString()` line.</span></span>

## <a name="create-an-abstraction-layer"></a><span data-ttu-id="0d55b-214">建立一個抽象層</span><span class="sxs-lookup"><span data-stu-id="0d55b-214">Create an abstraction layer</span></span>

<span data-ttu-id="0d55b-215">許多開發人員撰寫程式碼以實作存放庫和工作單元模式，作為使用 Entity Framework 之程式碼周圍的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="0d55b-215">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="0d55b-216">這些模式主要用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="0d55b-216">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="0d55b-217">實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-217">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="0d55b-218">不過，撰寫額外的程式碼來實作這些模式不是一律使用 EF，有幾個原因的應用程式的最佳選擇：</span><span class="sxs-lookup"><span data-stu-id="0d55b-218">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

- <span data-ttu-id="0d55b-219">EF 內容類別本身會隔離您的程式碼與資料存放區特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0d55b-219">The EF context class itself insulates your code from data-store-specific code.</span></span>
- <span data-ttu-id="0d55b-220">EF 內容類別可作為您使用 EF 進行之資料庫更新的工作單元類別。</span><span class="sxs-lookup"><span data-stu-id="0d55b-220">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>
- <span data-ttu-id="0d55b-221">Entity Framework 6 中引進的功能，讓您更輕鬆地實作 TDD 而不需要撰寫存放庫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0d55b-221">Features introduced in Entity Framework 6 make it easier to implement TDD without writing repository code.</span></span>

<span data-ttu-id="0d55b-222">如需如何實作存放庫和工作單元模式的詳細資訊，請參閱[本教學課程系列的 Entity Framework 5 版本](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-222">For more information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0d55b-223">如需 Entity Framework 6 中實作 TDD 方式的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="0d55b-223">For information about ways to implement TDD in Entity Framework 6, see the following resources:</span></span>

- [<span data-ttu-id="0d55b-224">如何 EF6 讓模擬 DbSets 更輕鬆地</span><span class="sxs-lookup"><span data-stu-id="0d55b-224">How EF6 Enables Mocking DbSets more easily</span></span>](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [<span data-ttu-id="0d55b-225">測試的模擬架構</span><span class="sxs-lookup"><span data-stu-id="0d55b-225">Testing with a mocking framework</span></span>](https://msdn.microsoft.com/data/dn314429)
- [<span data-ttu-id="0d55b-226">使用您自己的測試替身進行測試</span><span class="sxs-lookup"><span data-stu-id="0d55b-226">Testing with your own test doubles</span></span>](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a><span data-ttu-id="0d55b-227">Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="0d55b-227">Proxy classes</span></span>

<span data-ttu-id="0d55b-228">當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立它們的動態產生的衍生型別，做為實體的 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0d55b-228">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="0d55b-229">例如，請參閱下列兩個偵錯工具映像。</span><span class="sxs-lookup"><span data-stu-id="0d55b-229">For example, see the following two debugger images.</span></span> <span data-ttu-id="0d55b-230">在第一個映像，您會看到`student`變數是預期`Student`您具現化實體之後，立即輸入。</span><span class="sxs-lookup"><span data-stu-id="0d55b-230">In the first image, you see that the `student` variable is the expected `Student` type immediately after you instantiate the entity.</span></span> <span data-ttu-id="0d55b-231">在第二個影像中，已從資料庫讀取 student 實體使用 EF 之後會看到 proxy 類別。</span><span class="sxs-lookup"><span data-stu-id="0d55b-231">In the second image, after EF has been used to read a student entity from the database, you see the proxy class.</span></span>

![Proxy 類別之前](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Proxy 類別](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="0d55b-234">這個 proxy 類別會覆寫要插入存取屬性時，就會自動執行動作的攔截程序之實體的某些虛擬屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-234">This proxy class overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="0d55b-235">用於這項機制的一個函式是消極式載入。</span><span class="sxs-lookup"><span data-stu-id="0d55b-235">One function this mechanism is used for is lazy loading.</span></span>

<span data-ttu-id="0d55b-236">大多時候您不需要注意這項使用的 proxy，但有例外狀況：</span><span class="sxs-lookup"><span data-stu-id="0d55b-236">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="0d55b-237">在某些情況下，您可能想要防止 Entity Framework 建立 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0d55b-237">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="0d55b-238">例如，您要序列化實體時您通常想 POCO 類別，而不是 proxy 類別。</span><span class="sxs-lookup"><span data-stu-id="0d55b-238">For example, when you're serializing entities you generally want the POCO classes, not the proxy classes.</span></span> <span data-ttu-id="0d55b-239">若要避免發生序列化問題的一個方式是序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="0d55b-239">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects, as shown in the [Using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial.</span></span> <span data-ttu-id="0d55b-240">另一個方法是以[停用 proxy 建立](https://msdn.microsoft.com/data/jj592886.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-240">Another way is to [disable proxy creation](https://msdn.microsoft.com/data/jj592886.aspx).</span></span>
- <span data-ttu-id="0d55b-241">當您具現化實體類別使用`new`運算子，您不會取得 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0d55b-241">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="0d55b-242">這表示您未獲得的功能，例如消極式載入和自動變更追蹤。</span><span class="sxs-lookup"><span data-stu-id="0d55b-242">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="0d55b-243">這通常是好;您通常不需要消極式載入，因為您要建立新的實體不在資料庫中，而且您通常不需要變更追蹤，如果您要明確地標示為實體`Added`。</span><span class="sxs-lookup"><span data-stu-id="0d55b-243">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="0d55b-244">不過，如果您需要消極式載入，而且您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy [Create](https://msdn.microsoft.com/library/gg679504.aspx)方法`DbSet`類別。</span><span class="sxs-lookup"><span data-stu-id="0d55b-244">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the [Create](https://msdn.microsoft.com/library/gg679504.aspx) method of the `DbSet` class.</span></span>
- <span data-ttu-id="0d55b-245">您可以從 proxy 型別取得實際的實體類型。</span><span class="sxs-lookup"><span data-stu-id="0d55b-245">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="0d55b-246">您可以使用[GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)方法`ObjectContext`類別，以取得 proxy 型別執行個體的實際實體類型。</span><span class="sxs-lookup"><span data-stu-id="0d55b-246">You can use the [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="0d55b-247">如需詳細資訊，請參閱 <<c0> [ 使用 Proxy](https://msdn.microsoft.com/data/JJ592886.aspx) MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="0d55b-247">For more information, see [Working with Proxies](https://msdn.microsoft.com/data/JJ592886.aspx) on MSDN.</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="0d55b-248">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="0d55b-248">Automatic change detection</span></span>

<span data-ttu-id="0d55b-249">Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-249">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="0d55b-250">查詢或附加實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="0d55b-250">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="0d55b-251">會導致自動變更偵測的一些方法如下：</span><span class="sxs-lookup"><span data-stu-id="0d55b-251">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="0d55b-252">如果您正在追蹤大量的實體，而且您在迴圈中呼叫其中一個方法許多次，您可能會暫時關閉自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-252">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) property.</span></span> <span data-ttu-id="0d55b-253">如需詳細資訊，請參閱 <<c0> [ 自動偵測變更](https://msdn.microsoft.com/data/jj556205)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="0d55b-253">For more information, see [Automatically Detecting Changes](https://msdn.microsoft.com/data/jj556205) on MSDN.</span></span>

## <a name="automatic-validation"></a><span data-ttu-id="0d55b-254">自動驗證</span><span class="sxs-lookup"><span data-stu-id="0d55b-254">Automatic validation</span></span>

<span data-ttu-id="0d55b-255">當您呼叫`SaveChanges`方法，預設的 Entity Framework 會驗證所有屬性的所有變更的實體中的資料更新資料庫之前。</span><span class="sxs-lookup"><span data-stu-id="0d55b-255">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="0d55b-256">如果您已更新大量的實體和您已驗證資料，這項工作不需要，而且您可以讓儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。</span><span class="sxs-lookup"><span data-stu-id="0d55b-256">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="0d55b-257">您可以執行的方法使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-257">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) property.</span></span> <span data-ttu-id="0d55b-258">如需詳細資訊，請參閱 <<c0> [ 驗證](https://msdn.microsoft.com/data/gg193959)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="0d55b-258">For more information, see [Validation](https://msdn.microsoft.com/data/gg193959) on MSDN.</span></span>

## <a name="entity-framework-power-tools"></a><span data-ttu-id="0d55b-259">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="0d55b-259">Entity Framework Power Tools</span></span>

<span data-ttu-id="0d55b-260">[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)用來建立資料模型圖表的 Visual Studio 增益集顯示在這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="0d55b-260">[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) is a Visual Studio add-in that was used to create the data model diagrams shown in these tutorials.</span></span> <span data-ttu-id="0d55b-261">這些工具也可以執行其他函式，例如產生實體類別根據現有的資料庫中的資料表，讓您可以使用 Code First 使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="0d55b-261">The tools can also do other function such as generate entity classes based on the tables in an existing database so that you can use the database with Code First.</span></span> <span data-ttu-id="0d55b-262">安裝工具之後，其他選項會出現在內容功能表中。</span><span class="sxs-lookup"><span data-stu-id="0d55b-262">After you install the tools, some additional options appear in context menus.</span></span> <span data-ttu-id="0d55b-263">比方說，當您以滑鼠右鍵按一下您的內容類別中**方案總管**，您會看到並**Entity Framework**選項。</span><span class="sxs-lookup"><span data-stu-id="0d55b-263">For example, when you right-click your context class in **Solution Explorer**, you see and **Entity Framework** option.</span></span> <span data-ttu-id="0d55b-264">這會讓您能夠產生圖表。</span><span class="sxs-lookup"><span data-stu-id="0d55b-264">This gives you the ability to generate a diagram.</span></span> <span data-ttu-id="0d55b-265">當您使用 Code First 時您無法變更在圖表中的資料模型，但是您可以移動項目，讓您更輕鬆地了解。</span><span class="sxs-lookup"><span data-stu-id="0d55b-265">When you're using Code First you can't change the data model in the diagram, but you can move things around to make it easier to understand.</span></span>

![EF 圖表](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a><span data-ttu-id="0d55b-267">Entity Framework 原始碼</span><span class="sxs-lookup"><span data-stu-id="0d55b-267">Entity Framework source code</span></span>

<span data-ttu-id="0d55b-268">Entity Framework 6 的原始程式碼位於[GitHub](https://github.com/aspnet/EntityFramework6)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-268">The source code for Entity Framework 6 is available at [GitHub](https://github.com/aspnet/EntityFramework6).</span></span> <span data-ttu-id="0d55b-269">您可以提報 bug，以及您可以發表自己的 EF 來源程式碼的增強功能。</span><span class="sxs-lookup"><span data-stu-id="0d55b-269">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="0d55b-270">雖然原始程式碼是開放，Entity Framework 完全受到支援作為 Microsoft 產品。</span><span class="sxs-lookup"><span data-stu-id="0d55b-270">Although the source code is open, Entity Framework is fully supported as a Microsoft product.</span></span> <span data-ttu-id="0d55b-271">Microsoft Entity Framework 小組將控制接受哪些貢獻，並測試所有的程式碼變更以確保每次發行的品質。</span><span class="sxs-lookup"><span data-stu-id="0d55b-271">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="0d55b-272">感謝</span><span class="sxs-lookup"><span data-stu-id="0d55b-272">Acknowledgments</span></span>

- <span data-ttu-id="0d55b-273">Tom Dykstra 撰寫本教學課程中的原始版本、 合著的 EF 5 更新，並撰寫 EF 6 更新。</span><span class="sxs-lookup"><span data-stu-id="0d55b-273">Tom Dykstra wrote the original version of this tutorial, co-authored the EF 5 update, and wrote the EF 6 update.</span></span> <span data-ttu-id="0d55b-274">Tom 是擔任 Microsoft Web Platform and Tools 內容小組的資深程式設計作家。</span><span class="sxs-lookup"><span data-stu-id="0d55b-274">Tom is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="0d55b-275">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 完成大部分的 EF 5 和 MVC 4 更新本教學課程的工作和共同著述的 EF 6 更新。</span><span class="sxs-lookup"><span data-stu-id="0d55b-275">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) did most of the work updating the tutorial for EF 5 and MVC 4 and co-authored the EF 6 update.</span></span> <span data-ttu-id="0d55b-276">Rick 是將焦點放在 Azure 和 MVC 的 microsoft 的資深程式設計作家。</span><span class="sxs-lookup"><span data-stu-id="0d55b-276">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="0d55b-277">[Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助進行程式碼檢閱，並協助偵錯時發生我們已更新本教學課程的 EF 5 和 EF 6 移轉的許多問題。</span><span class="sxs-lookup"><span data-stu-id="0d55b-277">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5 and EF 6.</span></span>

## <a name="troubleshoot-common-errors"></a><span data-ttu-id="0d55b-278">針對常見錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="0d55b-278">Troubleshoot common errors</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="0d55b-279">無法建立/陰影複製</span><span class="sxs-lookup"><span data-stu-id="0d55b-279">Cannot create/shadow copy</span></span>

<span data-ttu-id="0d55b-280">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0d55b-280">Error Message:</span></span>

> <span data-ttu-id="0d55b-281">無法建立/陰影複製 '&lt;filename&gt;' 已存在的檔案。</span><span class="sxs-lookup"><span data-stu-id="0d55b-281">Cannot create/shadow copy '&lt;filename&gt;' when that file already exists.</span></span>

<span data-ttu-id="0d55b-282">方案</span><span class="sxs-lookup"><span data-stu-id="0d55b-282">Solution</span></span>

<span data-ttu-id="0d55b-283">等候幾秒鐘的時間，然後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="0d55b-283">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="0d55b-284">更新資料庫無法辨識</span><span class="sxs-lookup"><span data-stu-id="0d55b-284">Update-Database not recognized</span></span>

<span data-ttu-id="0d55b-285">錯誤訊息 (從`Update-Database`PMC 命令):</span><span class="sxs-lookup"><span data-stu-id="0d55b-285">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="0d55b-286">詞彙 更新資料庫 ' 無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="0d55b-286">The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="0d55b-287">請檢查名稱的拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。</span><span class="sxs-lookup"><span data-stu-id="0d55b-287">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>

<span data-ttu-id="0d55b-288">方案</span><span class="sxs-lookup"><span data-stu-id="0d55b-288">Solution</span></span>

<span data-ttu-id="0d55b-289">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0d55b-289">Exit Visual Studio.</span></span> <span data-ttu-id="0d55b-290">重新開啟專案，並再試一次。</span><span class="sxs-lookup"><span data-stu-id="0d55b-290">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="0d55b-291">驗證失敗</span><span class="sxs-lookup"><span data-stu-id="0d55b-291">Validation failed</span></span>

<span data-ttu-id="0d55b-292">錯誤訊息 (從`Update-Database`PMC 命令):</span><span class="sxs-lookup"><span data-stu-id="0d55b-292">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="0d55b-293">一個或多個實體的驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="0d55b-293">Validation failed for one or more entities.</span></span> <span data-ttu-id="0d55b-294">請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0d55b-294">See 'EntityValidationErrors' property for more details.</span></span>

<span data-ttu-id="0d55b-295">方案</span><span class="sxs-lookup"><span data-stu-id="0d55b-295">Solution</span></span>

<span data-ttu-id="0d55b-296">此問題的其中一個原因是驗證錯誤時`Seed`方法執行。</span><span class="sxs-lookup"><span data-stu-id="0d55b-296">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="0d55b-297">請參閱[植入及偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="0d55b-297">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="0d55b-298">HTTP 500.19 錯誤</span><span class="sxs-lookup"><span data-stu-id="0d55b-298">HTTP 500.19 error</span></span>

<span data-ttu-id="0d55b-299">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0d55b-299">Error Message:</span></span>

> <span data-ttu-id="0d55b-300">無法存取 HTTP 錯誤 500.19-內部伺服器錯誤 」 要求的頁面，因為頁面的相關的組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="0d55b-300">HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

<span data-ttu-id="0d55b-301">方案</span><span class="sxs-lookup"><span data-stu-id="0d55b-301">Solution</span></span>

<span data-ttu-id="0d55b-302">您可以取得此錯誤的一個方式是解決方案的從多個複本，每個使用相同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="0d55b-302">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="0d55b-303">您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動您正在使用的專案，藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="0d55b-303">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project you're working on.</span></span> <span data-ttu-id="0d55b-304">如果這個方式沒有用，請嘗試變更連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="0d55b-304">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="0d55b-305">以滑鼠右鍵按一下專案檔，然後按一下 屬性。</span><span class="sxs-lookup"><span data-stu-id="0d55b-305">Right click on the project file and then click properties.</span></span> <span data-ttu-id="0d55b-306">選取  **Web**索引標籤，然後將變更的連接埠號碼**專案 Url**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0d55b-306">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="0d55b-307">搜尋 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="0d55b-307">Error locating SQL Server instance</span></span>

<span data-ttu-id="0d55b-308">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0d55b-308">Error Message:</span></span>

> <span data-ttu-id="0d55b-309">建立與 SQL Server　的連線時，發生與網路相關的錯誤或是執行個體特有的錯誤。</span><span class="sxs-lookup"><span data-stu-id="0d55b-309">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="0d55b-310">找不到或無法存取伺服器。</span><span class="sxs-lookup"><span data-stu-id="0d55b-310">The server was not found or was not accessible.</span></span> <span data-ttu-id="0d55b-311">確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線</span><span class="sxs-lookup"><span data-stu-id="0d55b-311">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="0d55b-312">(提供者：SQL 網路介面，錯誤：26 - 尋找指定的伺服器/執行個體時發生錯誤)</span><span class="sxs-lookup"><span data-stu-id="0d55b-312">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="0d55b-313">方案</span><span class="sxs-lookup"><span data-stu-id="0d55b-313">Solution</span></span>

<span data-ttu-id="0d55b-314">檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="0d55b-314">Check the connection string.</span></span> <span data-ttu-id="0d55b-315">如果您已經手動刪除資料庫，請變更建構字串的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="0d55b-315">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="0d55b-316">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="0d55b-316">Get the code</span></span>

[<span data-ttu-id="0d55b-317">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="0d55b-317">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="0d55b-318">其他資源</span><span class="sxs-lookup"><span data-stu-id="0d55b-318">Additional resources</span></span>

 <span data-ttu-id="0d55b-319">如需如何使用 Entity Framework 處理資料的詳細資訊，請參閱[MSDN 上的 EF 文件頁面](https://msdn.microsoft.com/data/ee712907)並[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-319">For more information about how to work with data using the Entity Framework, see the [EF documentation page on MSDN](https://msdn.microsoft.com/data/ee712907) and [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="0d55b-320">如需如何在您已經建置它後，部署您的 web 應用程式的詳細資訊，請參閱[ASP.NET Web 部署-建議資源](../../../../whitepapers/aspnet-web-deployment-content-map.md)MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="0d55b-320">For more information about how to deploy your web application after you've built it, see [ASP.NET Web Deployment - Recommended Resources](../../../../whitepapers/aspnet-web-deployment-content-map.md) in the MSDN Library.</span></span>

<span data-ttu-id="0d55b-321">如需其他相關 MVC，例如驗證和授權，主題的詳細資訊[ASP.NET MVC-建議資源](../recommended-resources-for-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="0d55b-321">For information about other topics related to MVC, such as authentication and authorization, see the [ASP.NET MVC - Recommended Resources](../recommended-resources-for-mvc.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d55b-322">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0d55b-322">Next steps</span></span>

<span data-ttu-id="0d55b-323">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="0d55b-323">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d55b-324">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-324">Performed raw SQL queries</span></span>
> * <span data-ttu-id="0d55b-325">執行無追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-325">Performed no-tracking queries</span></span>
> * <span data-ttu-id="0d55b-326">檢查傳送至資料庫的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="0d55b-326">Examined SQL queries sent to the database</span></span>

<span data-ttu-id="0d55b-327">您也已了解：</span><span class="sxs-lookup"><span data-stu-id="0d55b-327">You also learned about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d55b-328">建立一個抽象層</span><span class="sxs-lookup"><span data-stu-id="0d55b-328">Creating an abstraction layer</span></span>
> * <span data-ttu-id="0d55b-329">Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="0d55b-329">Proxy classes</span></span>
> * <span data-ttu-id="0d55b-330">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="0d55b-330">Automatic change detection</span></span>
> * <span data-ttu-id="0d55b-331">自動驗證</span><span class="sxs-lookup"><span data-stu-id="0d55b-331">Automatic validation</span></span>
> * <span data-ttu-id="0d55b-332">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="0d55b-332">Entity Framework Power Tools</span></span>
> * <span data-ttu-id="0d55b-333">Entity Framework 原始碼</span><span class="sxs-lookup"><span data-stu-id="0d55b-333">Entity Framework source code</span></span>

<span data-ttu-id="0d55b-334">如此即完成本系列的教學課程使用 Entity Framework 的 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0d55b-334">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="0d55b-335">如果您想要深入了解 EF Database First，請參閱 DB 第一個教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="0d55b-335">If you want to learn about EF Database First, see the DB First tutorial series.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0d55b-336">Entity Framework Database First</span><span class="sxs-lookup"><span data-stu-id="0d55b-336">Entity Framework Database First</span></span>](../database-first-development/setting-up-database.md)