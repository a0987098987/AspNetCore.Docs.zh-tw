---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 進階 MVC Web 應用程式 (10-10) 的 Entity Framework 案例 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 277503b65d9b75a9d3cc05538d5327f9367f45e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a><span data-ttu-id="43238-103">MVC Web 應用程式 (10-10) 的進階的實體架構案例</span><span class="sxs-lookup"><span data-stu-id="43238-103">Advanced Entity Framework Scenarios for an MVC Web Application (10 of 10)</span></span>
====================
<span data-ttu-id="43238-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="43238-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="43238-105">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="43238-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="43238-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="43238-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="43238-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="43238-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="43238-108">您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="43238-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="43238-109">如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。</span><span class="sxs-lookup"><span data-stu-id="43238-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="43238-110">您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="43238-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="43238-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="43238-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="43238-112">在上一個教學課程中您會實作儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="43238-112">In the previous tutorial you implemented the repository and unit of work patterns.</span></span> <span data-ttu-id="43238-113">本教學課程涵蓋下列主題：</span><span class="sxs-lookup"><span data-stu-id="43238-113">This tutorial covers the following topics:</span></span>

- <span data-ttu-id="43238-114">執行原始的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="43238-114">Performing raw SQL queries.</span></span>
- <span data-ttu-id="43238-115">執行無追蹤查詢。</span><span class="sxs-lookup"><span data-stu-id="43238-115">Performing no-tracking queries.</span></span>
- <span data-ttu-id="43238-116">檢查查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="43238-116">Examining queries sent to the database.</span></span>
- <span data-ttu-id="43238-117">使用 proxy 類別。</span><span class="sxs-lookup"><span data-stu-id="43238-117">Working with proxy classes.</span></span>
- <span data-ttu-id="43238-118">停用自動偵測的變更。</span><span class="sxs-lookup"><span data-stu-id="43238-118">Disabling automatic detection of changes.</span></span>
- <span data-ttu-id="43238-119">停用驗證時儲存變更。</span><span class="sxs-lookup"><span data-stu-id="43238-119">Disabling validation when saving changes.</span></span>
- [<span data-ttu-id="43238-120">錯誤和工作的方法</span><span class="sxs-lookup"><span data-stu-id="43238-120">Errors and Work Arounds</span></span>](#errors)

<span data-ttu-id="43238-121">這些主題的大多數時間，您將使用您已經建立的網頁。</span><span class="sxs-lookup"><span data-stu-id="43238-121">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="43238-122">若要執行大量更新使用原始的 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的信用額度的數目：</span><span class="sxs-lookup"><span data-stu-id="43238-122">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="43238-124">並使用無追蹤查詢會將新的驗證邏輯加入至部門編輯頁面：</span><span class="sxs-lookup"><span data-stu-id="43238-124">And to use a no-tracking query you'll add new validation logic to the Department Edit page:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a><span data-ttu-id="43238-126">執行原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="43238-126">Performing Raw SQL Queries</span></span>

<span data-ttu-id="43238-127">Entity Framework 程式碼的第一個 API 包含可讓您將直接對資料庫的 SQL 命令的方法。</span><span class="sxs-lookup"><span data-stu-id="43238-127">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="43238-128">下列選項可供您選擇：</span><span class="sxs-lookup"><span data-stu-id="43238-128">You have the following options:</span></span>

- <span data-ttu-id="43238-129">針對傳回實體類型的查詢使用 `DbSet.SqlQuery` 方法。</span><span class="sxs-lookup"><span data-stu-id="43238-129">Use the `DbSet.SqlQuery` method for queries that return entity types.</span></span> <span data-ttu-id="43238-130">傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您關閉追蹤。</span><span class="sxs-lookup"><span data-stu-id="43238-130">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="43238-131">(請參閱下一節有關`AsNoTracking`方法。)</span><span class="sxs-lookup"><span data-stu-id="43238-131">(See the following section about the `AsNoTracking` method.)</span></span>
- <span data-ttu-id="43238-132">使用`Database.SqlQuery`方法的傳回類型不是實體的查詢。</span><span class="sxs-lookup"><span data-stu-id="43238-132">Use the `Database.SqlQuery` method for queries that return types that aren't entities.</span></span> <span data-ttu-id="43238-133">即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="43238-133">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="43238-134">使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非查詢命令。</span><span class="sxs-lookup"><span data-stu-id="43238-134">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) for non-query commands.</span></span>

<span data-ttu-id="43238-135">使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。</span><span class="sxs-lookup"><span data-stu-id="43238-135">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="43238-136">它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="43238-136">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="43238-137">但有例外狀況時，您需要執行特定 SQL 查詢，以手動方式建立，而且這些方法可讓您處理這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="43238-137">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="43238-138">如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="43238-138">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="43238-139">執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="43238-139">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="43238-140">在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。</span><span class="sxs-lookup"><span data-stu-id="43238-140">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="43238-141">呼叫查詢會傳回實體</span><span class="sxs-lookup"><span data-stu-id="43238-141">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="43238-142">假設您想要`GenericRepository`類別，以提供額外的篩選和排序的彈性，而不需要您使用其他方法建立衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="43238-142">Suppose you want the `GenericRepository` class to provide additional filtering and sorting flexibility without requiring that you create a derived class with additional methods.</span></span> <span data-ttu-id="43238-143">達成此目標的其中一種方式是加入接受 SQL 查詢的方法。</span><span class="sxs-lookup"><span data-stu-id="43238-143">One way to achieve that would be to add a method that accepts a SQL query.</span></span> <span data-ttu-id="43238-144">您可以指定任何一種篩選或排序您想在控制站，例如`Where`子句的聯結或子查詢而定。</span><span class="sxs-lookup"><span data-stu-id="43238-144">You could then specify any kind of filtering or sorting you want in the controller, such as a `Where` clause that depends on a joins or a subquery.</span></span> <span data-ttu-id="43238-145">本節中，您會看到如何實作這種方法。</span><span class="sxs-lookup"><span data-stu-id="43238-145">In this section you'll see how to implement such a method.</span></span>

<span data-ttu-id="43238-146">建立`GetWithRawSql`方法加入下列程式碼加入*GenericRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="43238-146">Create the `GetWithRawSql` method by adding the following code to *GenericRepository.cs*:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

<span data-ttu-id="43238-147">在*CourseController.cs*，呼叫新的方法從`Details`方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="43238-147">In *CourseController.cs*, call the new method from the `Details` method, as shown in the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="43238-148">在此情況下您可能使用`GetByID`方法，但您正在使用`GetWithRawSql`方法可讓您確認`GetWithRawSQL`方法運作。</span><span class="sxs-lookup"><span data-stu-id="43238-148">In this case you could have used the `GetByID` method, but you're using the `GetWithRawSql` method to verify that the `GetWithRawSQL` method works.</span></span>

<span data-ttu-id="43238-149">執行驗證的選取查詢運作的詳細資料頁面 (選取**課程** 索引標籤，然後**詳細資料**一個課程)。</span><span class="sxs-lookup"><span data-stu-id="43238-149">Run the Details page to verify that the select query works (select the **Course** tab and then **Details** for one course).</span></span>

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="43238-151">呼叫查詢傳回其他類型的物件</span><span class="sxs-lookup"><span data-stu-id="43238-151">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="43238-152">先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。</span><span class="sxs-lookup"><span data-stu-id="43238-152">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="43238-153">在程式碼*HomeController.cs*使用 LINQ:</span><span class="sxs-lookup"><span data-stu-id="43238-153">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

<span data-ttu-id="43238-154">假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的此資料。</span><span class="sxs-lookup"><span data-stu-id="43238-154">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="43238-155">若要執行您要執行查詢，以傳回實體物件以外的項目，這表示您需要使用`Database.SqlQuery`方法。</span><span class="sxs-lookup"><span data-stu-id="43238-155">To do that you need to run a query that returns something other than entity objects, which means you need to use the `Database.SqlQuery` method.</span></span>

<span data-ttu-id="43238-156">在*HomeController.cs*，取代中的 LINQ 陳述式`About`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="43238-156">In *HomeController.cs*, replace the LINQ statement in the `About` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="43238-157">執行 「 關於 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="43238-157">Run the About page.</span></span> <span data-ttu-id="43238-158">它會顯示與之前相同的資料。</span><span class="sxs-lookup"><span data-stu-id="43238-158">It displays the same data it did before.</span></span>

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a><span data-ttu-id="43238-160">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="43238-160">Calling an Update Query</span></span>

<span data-ttu-id="43238-161">假設 Contoso 大學系統管理員想要能夠執行大量變更，在資料庫中，例如變更的每個課程信用額度的數目。</span><span class="sxs-lookup"><span data-stu-id="43238-161">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="43238-162">如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。</span><span class="sxs-lookup"><span data-stu-id="43238-162">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="43238-163">在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您會變更執行 SQL`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="43238-163">In this section you'll implement a web page that allows the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> <span data-ttu-id="43238-164">網頁看起來將如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="43238-164">The web page will look like the following illustration:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

<span data-ttu-id="43238-166">在上一個教學課程中您必須使用泛型的儲存機制讀取及更新`Course`中的實體`Course`控制站。</span><span class="sxs-lookup"><span data-stu-id="43238-166">In the previous tutorial you used the generic repository to read and update `Course` entities in the `Course` controller.</span></span> <span data-ttu-id="43238-167">這個大量更新作業中，您需要建立新的儲存機制方法不是泛型的儲存機制中。</span><span class="sxs-lookup"><span data-stu-id="43238-167">For this bulk update operation, you need to create a new repository method that isn't in the generic repository.</span></span> <span data-ttu-id="43238-168">若要這樣做，您將建立專用`CourseRepository`類別衍生自`GenericRepository`類別。</span><span class="sxs-lookup"><span data-stu-id="43238-168">To do that, you'll create a dedicated `CourseRepository` class that derives from the `GenericRepository` class.</span></span>

<span data-ttu-id="43238-169">在*DAL*資料夾中，建立*CourseRepository.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="43238-169">In the *DAL* folder, create *CourseRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

<span data-ttu-id="43238-170">在*UnitOfWork.cs*，變更`Course`從儲存機制類型`GenericRepository<Course>`至 `CourseRepository:`</span><span class="sxs-lookup"><span data-stu-id="43238-170">In *UnitOfWork.cs*, change the `Course` repository type from `GenericRepository<Course>` to `CourseRepository:`</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

<span data-ttu-id="43238-171">在*CourseContoller.cs*，新增`UpdateCourseCredits`方法：</span><span class="sxs-lookup"><span data-stu-id="43238-171">In *CourseContoller.cs*, add an `UpdateCourseCredits` method:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="43238-172">這個方法會使用兩個`HttpGet`和`HttpPost`。</span><span class="sxs-lookup"><span data-stu-id="43238-172">This method will be used for both `HttpGet` and `HttpPost`.</span></span> <span data-ttu-id="43238-173">當`HttpGet``UpdateCourseCredits`方法執行`multiplier`變數將會是 null，而且檢視會顯示空白的文字方塊和 [提交] 按鈕，在上圖所示。</span><span class="sxs-lookup"><span data-stu-id="43238-173">When the `HttpGet` `UpdateCourseCredits` method runs, the `multiplier` variable will be null and the view will display an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="43238-174">當**更新**按鈕和`HttpPost`方法執行`multiplier`必須在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="43238-174">When the **Update** button is clicked and the `HttpPost` method runs, `multiplier` will have the value entered in the text box.</span></span> <span data-ttu-id="43238-175">然後程式碼呼叫的儲存機制`UpdateCourseCredits`方法，傳回的數目會受到影響的資料列，而該值會儲存在`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="43238-175">The code then calls the repository `UpdateCourseCredits` method, which returns the number of affected rows, and that value is stored in the `ViewBag` object.</span></span> <span data-ttu-id="43238-176">當檢視接收中的受影響資料列數目`ViewBag`物件，它會顯示該數字，而不是在文字方塊中，並提交 按鈕，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="43238-176">When the view receives the number of affected rows in the `ViewBag` object, it displays that number instead of the text box and submit button, as shown in the following illustration:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

<span data-ttu-id="43238-178">中建立檢視*Views\Course*更新課程信用額度頁面的資料夾：</span><span class="sxs-lookup"><span data-stu-id="43238-178">Create a view in the *Views\Course* folder for the Update Course Credits page:</span></span>

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

<span data-ttu-id="43238-180">在*Views\Course\UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="43238-180">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="43238-181">藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。</span><span class="sxs-lookup"><span data-stu-id="43238-181">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="43238-182">在文字方塊中輸入數目：</span><span class="sxs-lookup"><span data-stu-id="43238-182">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

<span data-ttu-id="43238-184">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="43238-184">Click **Update**.</span></span> <span data-ttu-id="43238-185">您會看到受影響的資料列數目：</span><span class="sxs-lookup"><span data-stu-id="43238-185">You see the number of rows affected:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

<span data-ttu-id="43238-187">按一下 [回到清單]，以查看課程與已修訂學分數的清單。</span><span class="sxs-lookup"><span data-stu-id="43238-187">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

<span data-ttu-id="43238-189">如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="43238-189">For more information about raw SQL queries, see [Raw SQL Queries](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) on the Entity Framework team blog.</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="43238-190">不追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="43238-190">No-Tracking Queries</span></span>

<span data-ttu-id="43238-191">當資料庫內容擷取資料庫資料列，並建立代表的實體物件時，依預設它會追蹤的是否與資料庫中的實體記憶體中保持同步。</span><span class="sxs-lookup"><span data-stu-id="43238-191">When a database context retrieves database rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="43238-192">記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。</span><span class="sxs-lookup"><span data-stu-id="43238-192">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="43238-193">這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。</span><span class="sxs-lookup"><span data-stu-id="43238-193">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="43238-194">您可以指定內容是否會追蹤查詢的實體物件，使用`AsNoTracking`方法。</span><span class="sxs-lookup"><span data-stu-id="43238-194">You can specify whether the context tracks entity objects for a query by using the `AsNoTracking` method.</span></span> <span data-ttu-id="43238-195">您會想要進行這項操作的常見案例包括下列情況：</span><span class="sxs-lookup"><span data-stu-id="43238-195">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="43238-196">此查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。</span><span class="sxs-lookup"><span data-stu-id="43238-196">The query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="43238-197">您想要將實體附加以更新，但您稍早擷取同一個實體用於不同用途。</span><span class="sxs-lookup"><span data-stu-id="43238-197">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="43238-198">由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。</span><span class="sxs-lookup"><span data-stu-id="43238-198">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="43238-199">若要避免這種情況的一種方式為使用`AsNoTracking`與前面的查詢選項。</span><span class="sxs-lookup"><span data-stu-id="43238-199">One way to prevent this from happening is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="43238-200">在本節中，您將實作說明第二個案例的商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="43238-200">In this section you'll implement business logic that illustrates the second of these scenarios.</span></span> <span data-ttu-id="43238-201">具體來說，您將會強制執行商務規則，表示講師不能多個部門的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="43238-201">Specifically, you'll enforce a business rule that says that an instructor can't be the administrator of more than one department.</span></span>

<span data-ttu-id="43238-202">在*DepartmentController.cs*，加入新的方法，您可以從呼叫`Edit`和`Create`方法，以確定沒有兩個部門有相同的系統管理員：</span><span class="sxs-lookup"><span data-stu-id="43238-202">In *DepartmentController.cs*, add a new method that you can call from the `Edit` and `Create` methods to make sure that no two departments have the same administrator:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

<span data-ttu-id="43238-203">將程式碼中的加入`try`區塊`HttpPost``Edit`方法來呼叫此新方法，如果不有任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="43238-203">Add code in the `try` block of the `HttpPost` `Edit` method to call this new method if there are no validation errors.</span></span> <span data-ttu-id="43238-204">`try`區塊現在看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="43238-204">The `try` block now looks like the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

<span data-ttu-id="43238-205">執行部門編輯網頁，並嘗試將部門的系統管理員變更為講師人員已經是不同的部門的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="43238-205">Run the Department Edit page and try to change a department's administrator to an instructor who is already the administrator of a different department.</span></span> <span data-ttu-id="43238-206">您會收到預期的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-206">You get the expected error message:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="43238-208">現在執行部門編輯頁面一次，此時間變更**預算**數量。</span><span class="sxs-lookup"><span data-stu-id="43238-208">Now run the Department Edit page again and this time change the **Budget** amount.</span></span> <span data-ttu-id="43238-209">當您按一下**儲存**，您會看到錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="43238-209">When you click **Save**, you see an error page:</span></span>

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

<span data-ttu-id="43238-211">例外狀況錯誤訊息是 「`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`「 這是因為以下一連串事件：</span><span class="sxs-lookup"><span data-stu-id="43238-211">The exception error message is "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" This happened because of the following sequence of events:</span></span>

- <span data-ttu-id="43238-212">`Edit`方法呼叫`ValidateOneAdministratorAssignmentPerInstructor`方法，擷取所有具有其系統管理員身分 Kim Abercrombie 的部門。</span><span class="sxs-lookup"><span data-stu-id="43238-212">The `Edit` method calls the `ValidateOneAdministratorAssignmentPerInstructor` method, which retrieves all departments that have Kim Abercrombie as their administrator.</span></span> <span data-ttu-id="43238-213">會導致要讀取的英文部門。</span><span class="sxs-lookup"><span data-stu-id="43238-213">That causes the English department to be read.</span></span> <span data-ttu-id="43238-214">因為這是正在編輯的部門，則會不報告任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="43238-214">Because that's the department being edited, no error is reported.</span></span> <span data-ttu-id="43238-215">由於這個讀取作業，不過，英文 department 實體已從資料庫讀取現在正在追蹤的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="43238-215">As a result of this read operation, however, the English department entity that was read from the database is now being tracked by the database context.</span></span>
- <span data-ttu-id="43238-216">`Edit`方法嘗試設定`Modified`英文旗標 department 實體建立 MVC 模型繫結器，但是會失敗，因為內容已經在追蹤實體的英文版的部門。</span><span class="sxs-lookup"><span data-stu-id="43238-216">The `Edit` method tries to set the `Modified` flag on the English department entity created by the MVC model binder, but that fails because the context is already tracking an entity for the English department.</span></span>

<span data-ttu-id="43238-217">這個問題的解決方案之一是讓內容追蹤記憶體中的部門由驗證查詢所擷取的實體。</span><span class="sxs-lookup"><span data-stu-id="43238-217">One solution to this problem is to keep the context from tracking in-memory department entities retrieved by the validation query.</span></span> <span data-ttu-id="43238-218">不沒有這麼做，因為您將不會更新此實體或可從它在記憶體中快取獲益的方式一次讀取任何缺點。</span><span class="sxs-lookup"><span data-stu-id="43238-218">There's no disadvantage to doing this, because you won't be updating this entity or reading it again in a way that would benefit from it being cached in memory.</span></span>

<span data-ttu-id="43238-219">在*DepartmentController.cs*，請在`ValidateOneAdministratorAssignmentPerInstructor`方法，指定沒有追蹤中，如下列所示：</span><span class="sxs-lookup"><span data-stu-id="43238-219">In *DepartmentController.cs*, in the `ValidateOneAdministratorAssignmentPerInstructor` method, specify no tracking, as shown in the following:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

<span data-ttu-id="43238-220">重複嘗試編輯**預算**部門的數量。</span><span class="sxs-lookup"><span data-stu-id="43238-220">Repeat your attempt to edit the **Budget** amount of a department.</span></span> <span data-ttu-id="43238-221">目前作業成功，並將站台傳回如預期般部門索引頁，顯示已修訂的預算值。</span><span class="sxs-lookup"><span data-stu-id="43238-221">This time the operation is successful, and the site returns as expected to the Departments Index page, showing the revised budget value.</span></span>

## <a name="examining-queries-sent-to-the-database"></a><span data-ttu-id="43238-222">檢查查詢傳送至資料庫</span><span class="sxs-lookup"><span data-stu-id="43238-222">Examining Queries Sent to the Database</span></span>

<span data-ttu-id="43238-223">有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。</span><span class="sxs-lookup"><span data-stu-id="43238-223">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="43238-224">若要這樣做，您可以檢查偵錯工具中的查詢變數或將查詢稱為`ToString`方法。</span><span class="sxs-lookup"><span data-stu-id="43238-224">To do this, you can examine a query variable in the debugger or call the query's `ToString` method.</span></span> <span data-ttu-id="43238-225">再試一次時，您將查看簡單查詢，並查看您新增這類 eager 載入、 篩選和排序選項，它會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="43238-225">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="43238-226">在*控制器/CourseController*，取代`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="43238-226">In *Controllers/CourseController*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

<span data-ttu-id="43238-227">現在中設定中斷點*GenericRepository.cs*上`return query.ToList();`和`return orderBy(query).ToList();`陳述式的`Get`方法。</span><span class="sxs-lookup"><span data-stu-id="43238-227">Now set a breakpoint in *GenericRepository.cs* on the `return query.ToList();` and the `return orderBy(query).ToList();` statements of the `Get` method.</span></span> <span data-ttu-id="43238-228">在偵錯模式中執行專案並選取課程索引頁面。</span><span class="sxs-lookup"><span data-stu-id="43238-228">Run the project in debug mode and select the Course Index page.</span></span> <span data-ttu-id="43238-229">當程式碼到達中斷點時，檢查`query`變數。</span><span class="sxs-lookup"><span data-stu-id="43238-229">When the code reaches the breakpoint, examine the `query` variable.</span></span> <span data-ttu-id="43238-230">您會看到傳送至 SQL Server 的查詢。</span><span class="sxs-lookup"><span data-stu-id="43238-230">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="43238-231">它是簡單`Select`陳述式：</span><span class="sxs-lookup"><span data-stu-id="43238-231">It's a simple `Select` statement:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

<span data-ttu-id="43238-232">查詢可能太長，無法在 Visual Studio 中偵錯視窗中顯示。</span><span class="sxs-lookup"><span data-stu-id="43238-232">Queries can be too long to display in the debugging windows in Visual Studio.</span></span> <span data-ttu-id="43238-233">若要查看整個查詢，您可以複製變數的值，並將它貼到文字編輯器：</span><span class="sxs-lookup"><span data-stu-id="43238-233">To see the entire query, you can copy the variable value and paste it into a text editor:</span></span>

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

<span data-ttu-id="43238-235">現在您將新增下拉式選單課程索引頁，讓使用者可以篩選特定部門。</span><span class="sxs-lookup"><span data-stu-id="43238-235">Now you'll add a drop-down list to the Course Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="43238-236">您將項目，所排序的課程，您將指定的積極式載入`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-236">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="43238-237">在*CourseController.cs*，取代`Index`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="43238-237">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

<span data-ttu-id="43238-238">此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。</span><span class="sxs-lookup"><span data-stu-id="43238-238">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="43238-239">如果選取任何項目，這個參數會是 null。</span><span class="sxs-lookup"><span data-stu-id="43238-239">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="43238-240">A`SelectList`集合，其中包含所有部門傳遞至檢視的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="43238-240">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="43238-241">參數傳遞至`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。</span><span class="sxs-lookup"><span data-stu-id="43238-241">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="43238-242">如`Get`方法`Course`儲存機制、 程式碼會指定篩選條件運算式、 排序次序，以及載入 eager`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-242">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="43238-243">篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。</span><span class="sxs-lookup"><span data-stu-id="43238-243">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="43238-244">在*Views\Course\Index.cshtml*之前開啟,`table`標記中，加入下列程式碼，建立下拉式清單及提交按鈕：</span><span class="sxs-lookup"><span data-stu-id="43238-244">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

<span data-ttu-id="43238-245">與仍設定的中斷點`GenericRepository`類別，執行課程索引頁面。</span><span class="sxs-lookup"><span data-stu-id="43238-245">With the breakpoints still set in the `GenericRepository` class, run the Course Index page.</span></span> <span data-ttu-id="43238-246">繼續透過程式碼叫用中斷點時，前兩個時間如此頁面會顯示在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="43238-246">Continue through the first two times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="43238-247">從下拉式清單中選取部門並按一下**篩選**:</span><span class="sxs-lookup"><span data-stu-id="43238-247">Select a department from the drop-down list and click **Filter**:</span></span>

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="43238-249">這次下拉式清單的部門查詢就是第一個中斷點。</span><span class="sxs-lookup"><span data-stu-id="43238-249">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="43238-250">略過，並檢視`query`變數在下一次程式碼中斷點時若要查看哪些`Course`查詢現在看起來像。</span><span class="sxs-lookup"><span data-stu-id="43238-250">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="43238-251">您會看到類似如下：</span><span class="sxs-lookup"><span data-stu-id="43238-251">You'll see something like the following:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

<span data-ttu-id="43238-252">您可以看到查詢，現在是`JOIN`載入的查詢`Department`資料連同`Course`資料，以及它包含`WHERE`子句。</span><span class="sxs-lookup"><span data-stu-id="43238-252">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a><span data-ttu-id="43238-253">使用 Proxy 類別</span><span class="sxs-lookup"><span data-stu-id="43238-253">Working with Proxy Classes</span></span>

<span data-ttu-id="43238-254">當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立這些動態產生的衍生型別，可做為實體的 proxy 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="43238-254">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="43238-255">此 proxy 會覆寫要插入在屬性經過存取時，自動執行動作的攔截程序之實體的某些虛擬屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-255">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="43238-256">比方說，這項機制用來支援消極式載入的關聯性。</span><span class="sxs-lookup"><span data-stu-id="43238-256">For example, this mechanism is used to support lazy loading of relationships.</span></span>

<span data-ttu-id="43238-257">大部分的情況下您不需要注意這項使用的 proxy，但有例外狀況：</span><span class="sxs-lookup"><span data-stu-id="43238-257">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="43238-258">在某些情況下您可能想要防止 Entity Framework 建立 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="43238-258">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="43238-259">例如，序列化非 proxy 執行個體可能會更有效率序列化 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="43238-259">For example, serializing non-proxy instances might be more efficient than serializing proxy instances.</span></span>
- <span data-ttu-id="43238-260">當您具現化的實體類別使用`new`運算子，就無法取得 proxy 執行個體。</span><span class="sxs-lookup"><span data-stu-id="43238-260">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="43238-261">這表示您不先取得功能，例如消極式載入和自動變更追蹤。</span><span class="sxs-lookup"><span data-stu-id="43238-261">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="43238-262">這通常是好;您通常不需要延遲載入，因為您要建立新的實體不在資料庫中，而且通常不需要變更追蹤，如果您要明確地標示為實體`Added`。</span><span class="sxs-lookup"><span data-stu-id="43238-262">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="43238-263">不過，如果您需要消極式載入，而您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy`Create`方法`DbSet`類別。</span><span class="sxs-lookup"><span data-stu-id="43238-263">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the `Create` method of the `DbSet` class.</span></span>
- <span data-ttu-id="43238-264">您可能想要從的 proxy 型別取得實際的實體類型。</span><span class="sxs-lookup"><span data-stu-id="43238-264">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="43238-265">您可以使用`GetObjectType`方法`ObjectContext`類別取得的 proxy 型別執行個體的實際實體類型。</span><span class="sxs-lookup"><span data-stu-id="43238-265">You can use the `GetObjectType` method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="43238-266">如需詳細資訊，請參閱[使用 Proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="43238-266">For more information, see [Working with Proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) on the Entity Framework team blog.</span></span>

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="43238-267">停用自動偵測的變更</span><span class="sxs-lookup"><span data-stu-id="43238-267">Disabling Automatic Detection of Changes</span></span>

<span data-ttu-id="43238-268">Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。</span><span class="sxs-lookup"><span data-stu-id="43238-268">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="43238-269">當查詢或附加的實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="43238-269">The original values are stored when the entity was queried or attached.</span></span> <span data-ttu-id="43238-270">會導致自動變更偵測的一些方法如下：</span><span class="sxs-lookup"><span data-stu-id="43238-270">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="43238-271">如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-271">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="43238-272">如需詳細資訊，請參閱[自動偵測變更](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)。</span><span class="sxs-lookup"><span data-stu-id="43238-272">For more information, see [Automatically Detecting Changes](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).</span></span>

## <a name="disabling-validation-when-saving-changes"></a><span data-ttu-id="43238-273">停用驗證時儲存變更</span><span class="sxs-lookup"><span data-stu-id="43238-273">Disabling Validation When Saving Changes</span></span>

<span data-ttu-id="43238-274">當您呼叫`SaveChanges`方法，依預設 Entity Framework 中所有已變更實體的所有屬性的資料會先驗證更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="43238-274">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="43238-275">如果您已更新大量實體且您已驗證資料，這項工作不需要確定儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。</span><span class="sxs-lookup"><span data-stu-id="43238-275">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="43238-276">您可以使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-276">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="43238-277">如需詳細資訊，請參閱[驗證](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)。</span><span class="sxs-lookup"><span data-stu-id="43238-277">For more information, see [Validation](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="43238-278">總結</span><span class="sxs-lookup"><span data-stu-id="43238-278">Summary</span></span>

<span data-ttu-id="43238-279">如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="43238-279">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="43238-280">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="43238-280">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="43238-281">如需如何建置它之後部署 web 應用程式的詳細資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx)MSDN Library 中。</span><span class="sxs-lookup"><span data-stu-id="43238-281">For more information about how to deploy your web application after you've built it, see [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx) in the MSDN Library.</span></span>

<span data-ttu-id="43238-282">如需有關 MVC、 驗證和授權，例如其他主題，請參閱[MVC 建議資源](../../getting-started/recommended-resources-for-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="43238-282">For information about other topics related to MVC, such as authentication and authorization, see the [MVC Recommended Resources](../../getting-started/recommended-resources-for-mvc.md).</span></span>

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a><span data-ttu-id="43238-283">感謝</span><span class="sxs-lookup"><span data-stu-id="43238-283">Acknowledgments</span></span>

- <span data-ttu-id="43238-284">Tom Dykstra 寫入此教學課程中的原始版本，並是資深的開發寫入器上的 Microsoft Web 平台和工具的內容團隊。</span><span class="sxs-lookup"><span data-stu-id="43238-284">Tom Dykstra wrote the original version of this tutorial and is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="43238-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 共同撰寫本教學課程中，因此未更新 EF 5 和 MVC 4 工作的絕大部分。</span><span class="sxs-lookup"><span data-stu-id="43238-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) co-authored this tutorial and did most of the work updating it for EF 5 and MVC 4.</span></span> <span data-ttu-id="43238-286">Rick 是將焦點放在 Azure 和 MVC Microsoft 資深程式寫入器。</span><span class="sxs-lookup"><span data-stu-id="43238-286">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="43238-287">[Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助程式碼檢閱，並協助偵錯移轉，我們已更新本教學課程的 EF 5 時，會發生的許多問題。</span><span class="sxs-lookup"><span data-stu-id="43238-287">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5.</span></span>

## <a name="vb"></a><span data-ttu-id="43238-288">VB</span><span class="sxs-lookup"><span data-stu-id="43238-288">VB</span></span>

<span data-ttu-id="43238-289">時原先產生本教學課程，我們會提供 C# 和 VB 專案版本的下載已完成。</span><span class="sxs-lookup"><span data-stu-id="43238-289">When the tutorial was originally produced, we provided both C# and VB versions of the completed download project.</span></span> <span data-ttu-id="43238-290">透過這項更新中，我們會提供 C# 可下載專案中每一章，讓您更輕鬆地在系列，但由於時間限制和其他優先順序，我們沒有做的 VB 的任何地方開始</span><span class="sxs-lookup"><span data-stu-id="43238-290">With this update we are providing a C# downloadable project for each chapter to make it easier to get started anywhere in the series, but due to time limitations and other priorities we did not do that for VB.</span></span> <span data-ttu-id="43238-291">如果您建立使用這些教學課程將 VB 專案，願意，與其他人共用，請讓我們知道。</span><span class="sxs-lookup"><span data-stu-id="43238-291">If you build a VB project using these tutorials and would be willing to share that with others, please let us know.</span></span>

<a id="errors"></a>

## <a name="errors-and-workarounds"></a><span data-ttu-id="43238-292">錯誤和因應措施</span><span class="sxs-lookup"><span data-stu-id="43238-292">Errors and Workarounds</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="43238-293">無法建立或陰影複製</span><span class="sxs-lookup"><span data-stu-id="43238-293">Cannot create/shadow copy</span></span>

<span data-ttu-id="43238-294">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-294">Error Message:</span></span>

<span data-ttu-id="43238-295">*無法建立或陰影複製 'DotNetOpenAuth.OpenId' 時已存在的檔案。*</span><span class="sxs-lookup"><span data-stu-id="43238-295">*Cannot create/shadow copy 'DotNetOpenAuth.OpenId' when that file already exists.*</span></span>

<span data-ttu-id="43238-296">解決方案:</span><span class="sxs-lookup"><span data-stu-id="43238-296">Solution:</span></span>

<span data-ttu-id="43238-297">等候數秒鐘，並重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="43238-297">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="43238-298">更新資料庫無法辨識</span><span class="sxs-lookup"><span data-stu-id="43238-298">Update-Database not recognized</span></span>

<span data-ttu-id="43238-299">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-299">Error Message:</span></span>

<span data-ttu-id="43238-300">*'Update-database' 詞彙無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*(從*`Update-Database`* PMC 命令。)</span><span class="sxs-lookup"><span data-stu-id="43238-300">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="43238-301">解決方案:</span><span class="sxs-lookup"><span data-stu-id="43238-301">Solution:</span></span>

<span data-ttu-id="43238-302">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="43238-302">Exit Visual Studio.</span></span> <span data-ttu-id="43238-303">重新開啟專案，然後再試一次。</span><span class="sxs-lookup"><span data-stu-id="43238-303">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="43238-304">驗證失敗</span><span class="sxs-lookup"><span data-stu-id="43238-304">Validation failed</span></span>

<span data-ttu-id="43238-305">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-305">Error Message:</span></span>

<span data-ttu-id="43238-306">*一個或多個實體的驗證失敗。請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。*</span><span class="sxs-lookup"><span data-stu-id="43238-306">*Validation failed for one or more entities. See 'EntityValidationErrors' property for more details.*</span></span> <span data-ttu-id="43238-307">(從*`Update-Database`* PMC 命令。)</span><span class="sxs-lookup"><span data-stu-id="43238-307">(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="43238-308">解決方案:</span><span class="sxs-lookup"><span data-stu-id="43238-308">Solution:</span></span>

<span data-ttu-id="43238-309">發生此問題的其中一個原因是驗證錯誤時`Seed`方法執行。</span><span class="sxs-lookup"><span data-stu-id="43238-309">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="43238-310">請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="43238-310">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="43238-311">HTTP 500.19 錯誤</span><span class="sxs-lookup"><span data-stu-id="43238-311">HTTP 500.19 error</span></span>

<span data-ttu-id="43238-312">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-312">Error Message:</span></span>

<span data-ttu-id="43238-313">*無法存取 HTTP 錯誤 500.19-內部伺服器錯誤所要求的頁面，因為該頁面的相關的組態資料無效。*</span><span class="sxs-lookup"><span data-stu-id="43238-313">*HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.*</span></span>

<span data-ttu-id="43238-314">解決方案:</span><span class="sxs-lookup"><span data-stu-id="43238-314">Solution:</span></span>

<span data-ttu-id="43238-315">您可以取得此錯誤的其中一種方式是方案的從多個複本時，每個使用相同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="43238-315">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="43238-316">您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動專案上的工作，以解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="43238-316">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project your working on.</span></span> <span data-ttu-id="43238-317">如果無法解決問題，請變更通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="43238-317">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="43238-318">以滑鼠右鍵按一下專案檔，然後按一下 屬性。</span><span class="sxs-lookup"><span data-stu-id="43238-318">Right click on the project file and then click properties.</span></span> <span data-ttu-id="43238-319">選取**Web**索引標籤，然後變更 連接埠號碼**專案 Url**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="43238-319">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="43238-320">搜尋 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="43238-320">Error locating SQL Server instance</span></span>

<span data-ttu-id="43238-321">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="43238-321">Error Message:</span></span>

<span data-ttu-id="43238-322">*建立 SQL Server 的連接時發生網路相關或執行個體特定錯誤。找不到或無法存取伺服器。確認執行個體名稱正確，且 SQL Server 設定為允許遠端連接。(提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)*</span><span class="sxs-lookup"><span data-stu-id="43238-322">*A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)*</span></span>

<span data-ttu-id="43238-323">解決方案:</span><span class="sxs-lookup"><span data-stu-id="43238-323">Solution:</span></span>

<span data-ttu-id="43238-324">請檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="43238-324">Check connection string.</span></span> <span data-ttu-id="43238-325">如果您已經手動刪除資料庫，變更建構字串的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="43238-325">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43238-326">[上一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [下一頁](building-the-ef5-mvc4-chapter-downloads.md)</span><span class="sxs-lookup"><span data-stu-id="43238-326">[Previous](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Next](building-the-ef5-mvc4-chapter-downloads.md)</span></span>
