---
title: "使用 EF Core-ASP.NET Core MVC 進階-10-10"
author: tdykstra
description: "此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET web 應用程式的基本概念的主題。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/advanced
ms.openlocfilehash: 458f2dc8a67f8c706d043f0d9d7cb7ce962e52ce
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="336db-103">進階的主題-EF Core 與 ASP.NET Core MVC 教學課程 (10-10)</span><span class="sxs-lookup"><span data-stu-id="336db-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="336db-104">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="336db-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="336db-105">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="336db-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="336db-106">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="336db-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="336db-107">在上一個教學課程中，您可以實作資料表每個階層繼承。</span><span class="sxs-lookup"><span data-stu-id="336db-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="336db-108">此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET Core web 應用程式的基本概念的主題。</span><span class="sxs-lookup"><span data-stu-id="336db-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="336db-109">原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="336db-109">Raw SQL Queries</span></span>

<span data-ttu-id="336db-110">使用 Entity Framework 的優點之一是它可避免中斷您太接近儲存資料的特定方法的程式碼。</span><span class="sxs-lookup"><span data-stu-id="336db-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="336db-111">它會產生 SQL 查詢和命令，這也讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="336db-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="336db-112">但有例外狀況，當您需要執行手動建立的特定 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="336db-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="336db-113">針對這些案例，Entity Framework 程式碼的第一個 API 會包含可讓您將直接對資料庫的 SQL 命令的方法。</span><span class="sxs-lookup"><span data-stu-id="336db-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="336db-114">您在 EF Core 1.0 中有下列選項：</span><span class="sxs-lookup"><span data-stu-id="336db-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="336db-115">使用`DbSet.FromSql`查詢來傳回實體類型的方法。</span><span class="sxs-lookup"><span data-stu-id="336db-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="336db-116">傳回的物件必須是所預期的類型`DbSet`物件，而且它們自動受到內容追蹤時資料庫除非您[關閉追蹤](crud.md#no-tracking-queries)。</span><span class="sxs-lookup"><span data-stu-id="336db-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="336db-117">使用`Database.ExecuteSqlCommand`非查詢命令。</span><span class="sxs-lookup"><span data-stu-id="336db-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="336db-118">如果您需要執行可傳回類型不是實體的查詢，您可以使用 ADO.NET 與 EF 所提供的資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="336db-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="336db-119">即使您使用這個方法來擷取實體類型不被追蹤的資料庫內容，傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="336db-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="336db-120">因為永遠是 true 時您 web 應用程式中執行 SQL 命令，您必須採取一些預防措施以保護您的網站，SQL 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="336db-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="336db-121">方法之一是使用參數化的查詢，以確定網頁所提交的字串，無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="336db-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="336db-122">在本教學課程中，您將使用參數化的查詢時將使用者輸入整合到查詢。</span><span class="sxs-lookup"><span data-stu-id="336db-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="336db-123">呼叫傳回實體的查詢</span><span class="sxs-lookup"><span data-stu-id="336db-123">Call a query that returns entities</span></span>

<span data-ttu-id="336db-124">`DbSet<TEntity>`類別會提供方法可讓您執行查詢，以傳回型別的實體`TEntity`。</span><span class="sxs-lookup"><span data-stu-id="336db-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="336db-125">若要查看這對您所做的運作將會在變更程式碼`Details`部門控制站的方法。</span><span class="sxs-lookup"><span data-stu-id="336db-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="336db-126">在*DepartmentsController.cs*，請在`Details`方法，擷取的部門使用的程式碼取代`FromSql`方法呼叫，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="336db-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="336db-127">若要確認新的程式碼正常運作，請選取**部門** 索引標籤，然後**詳細資料**其中一個部門。</span><span class="sxs-lookup"><span data-stu-id="336db-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門詳細資料](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="336db-129">呼叫會傳回其他類型的查詢</span><span class="sxs-lookup"><span data-stu-id="336db-129">Call a query that returns other types</span></span>

<span data-ttu-id="336db-130">先前您建立學生統計資料方格中，以顯示每個註冊日期的學生總數一樣的 「 關於 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="336db-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="336db-131">從學生實體集取得資料 (`_context.Students`) 並將結果規劃一份使用 LINQ`EnrollmentDateGroup`檢視模型物件。</span><span class="sxs-lookup"><span data-stu-id="336db-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="336db-132">假設您想要撰寫 SQL 本身而不是使用 LINQ。</span><span class="sxs-lookup"><span data-stu-id="336db-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="336db-133">若要執行此操作，您必須執行 SQL 查詢會傳回實體物件以外的項目。</span><span class="sxs-lookup"><span data-stu-id="336db-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="336db-134">在 EF Core 1.0，方法之一是撰寫 ADO.NET 程式碼，並從 EF 取得資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="336db-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="336db-135">在*HomeController.cs*，取代`About`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="336db-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="336db-136">加入 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="336db-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="336db-137">執行應用程式，並移至 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="336db-137">Run the app and go to the About page.</span></span> <span data-ttu-id="336db-138">它會顯示相同的資料以前一樣。</span><span class="sxs-lookup"><span data-stu-id="336db-138">It displays the same data it did before.</span></span>

![有關頁面](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="336db-140">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="336db-140">Call an update query</span></span>

<span data-ttu-id="336db-141">假設 Contoso 大學系統管理員想要在資料庫中，例如變更的每個課程信用額度數量執行全域的變更。</span><span class="sxs-lookup"><span data-stu-id="336db-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="336db-142">如果該大學有大量的課程，很效率不佳，擷取這些全部都做為實體，並將它們個別變更。</span><span class="sxs-lookup"><span data-stu-id="336db-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="336db-143">在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您將執行 SQL UPDATE 陳述式來進行變更。</span><span class="sxs-lookup"><span data-stu-id="336db-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="336db-144">網頁看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="336db-144">The web page will look like the following illustration:</span></span>

![更新課程信用額度頁面](advanced/_static/update-credits.png)

<span data-ttu-id="336db-146">在*CoursesContoller.cs*，HttpGet 和 HttpPost 加入 UpdateCourseCredits 方法：</span><span class="sxs-lookup"><span data-stu-id="336db-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="336db-147">當控制器處理 HttpGet 要求時，不會傳回在`ViewData["RowsAffected"]`，並檢視顯示空白的文字方塊和送出按鈕，在上圖所示。</span><span class="sxs-lookup"><span data-stu-id="336db-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="336db-148">當**更新**按鈕、 HttpPost 方法呼叫時，和倍增器已在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="336db-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="336db-149">然後執行的程式碼會更新課程並傳回受影響的資料列數目至檢視中的 SQL `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="336db-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="336db-150">當檢視表取得`RowsAffected`值，它會顯示更新的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="336db-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="336db-151">在**方案總管 中**，以滑鼠右鍵按一下*檢視/課程*資料夾，然後再按一下**新增 > 新的項目**。</span><span class="sxs-lookup"><span data-stu-id="336db-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="336db-152">在**加入新項目**] 對話方塊中，按一下**ASP.NET**下**已安裝**在左窗格中，按一下 [ **MVC 檢視頁面**，並將新的檢視*UpdateCourseCredits.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="336db-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="336db-153">在*Views/Courses/UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="336db-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="336db-154">執行`UpdateCourseCredits`方法藉由選取**課程** 索引標籤，然後再新增 「 / UpdateCourseCredits"瀏覽器的網址列中的 URL 的結尾 (例如： `http://localhost:5813/Courses/UpdateCourseCredits`)。</span><span class="sxs-lookup"><span data-stu-id="336db-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="336db-155">在文字方塊中輸入的數字：</span><span class="sxs-lookup"><span data-stu-id="336db-155">Enter a number in the text box:</span></span>

![更新課程信用額度頁面](advanced/_static/update-credits.png)

<span data-ttu-id="336db-157">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="336db-157">Click **Update**.</span></span> <span data-ttu-id="336db-158">您會看到受影響的資料列數目：</span><span class="sxs-lookup"><span data-stu-id="336db-158">You see the number of rows affected:</span></span>

![更新課程信用額度頁面上受影響的資料列](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="336db-160">按一下**返回清單**若要查看課程信用額度的修訂編號取代清單。</span><span class="sxs-lookup"><span data-stu-id="336db-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="336db-161">請注意，實際執行程式碼會確定一律更新中有效的資料結果。</span><span class="sxs-lookup"><span data-stu-id="336db-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="336db-162">簡化如下所示的程式碼無法將信用額度足以導致大於 5 的數字的數目。</span><span class="sxs-lookup"><span data-stu-id="336db-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="336db-163">(`Credits`屬性具有`[Range(0, 5)]`屬性。)更新查詢可行，但是無效的資料可能會導致系統其他部分假設信用額度的數目是 5 或更少的非預期的結果。</span><span class="sxs-lookup"><span data-stu-id="336db-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="336db-164">如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://docs.microsoft.com/ef/core/querying/raw-sql)。</span><span class="sxs-lookup"><span data-stu-id="336db-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="336db-165">檢查傳送至資料庫的 SQL</span><span class="sxs-lookup"><span data-stu-id="336db-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="336db-166">有時候很有幫助能夠看到實際傳送至資料庫的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="336db-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="336db-167">內建的記錄功能，適用於 ASP.NET Core 自動使用 EF 核心撰寫包含 SQL 查詢和更新的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="336db-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="336db-168">在本節中，您會看到 SQL 記錄的一些範例。</span><span class="sxs-lookup"><span data-stu-id="336db-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="336db-169">開啟*StudentsController.cs*和`Details`方法上設定中斷點`if (student == null)`陳述式。</span><span class="sxs-lookup"><span data-stu-id="336db-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="336db-170">執行應用程式中偵錯模式，並移至詳細資料頁面的學生。</span><span class="sxs-lookup"><span data-stu-id="336db-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="336db-171">移至**輸出**視窗顯示偵錯輸出，而且您會看到查詢：</span><span class="sxs-lookup"><span data-stu-id="336db-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="336db-172">您會發現這裡喜歡的項目可能會令您吃驚： SQL 選取最多 2 個資料列 (`TOP(2)`) 從 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="336db-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="336db-173">`SingleOrDefaultAsync`方法未解析為 1 個資料列的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="336db-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="336db-174">原因如下：</span><span class="sxs-lookup"><span data-stu-id="336db-174">Here's why:</span></span>

* <span data-ttu-id="336db-175">如果查詢會傳回多個資料列，則方法會傳回 null。</span><span class="sxs-lookup"><span data-stu-id="336db-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="336db-176">若要判斷查詢是否會傳回多個資料列，必須檢查是否它會傳回至少 2 EF。</span><span class="sxs-lookup"><span data-stu-id="336db-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="336db-177">請注意，您不必使用偵錯模式，並取得記錄輸出中的中斷點處停止**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="336db-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="336db-178">它只是便利方式停止您想要查看輸出的點記錄。</span><span class="sxs-lookup"><span data-stu-id="336db-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="336db-179">如果不這樣做，請繼續記錄，您必須捲動至尋找您感興趣的部分。</span><span class="sxs-lookup"><span data-stu-id="336db-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="336db-180">儲存機制和單位的工作模式</span><span class="sxs-lookup"><span data-stu-id="336db-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="336db-181">許多開發人員撰寫程式碼以搭配 Entity Framework 的程式碼周圍的包裝函式實作的儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="336db-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="336db-182">這些模式被用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="336db-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="336db-183">實作這些模式可以協助您隔離您的應用程式資料存放區中的變更，以及有助於自動化的單元測試為導向的開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="336db-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="336db-184">不過，撰寫額外的程式碼來實作這些模式並非始終使用 EF，有幾個原因的應用程式的最佳選擇：</span><span class="sxs-lookup"><span data-stu-id="336db-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="336db-185">EF 內容類別本身，以隔離您的程式碼，從資料存放區特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="336db-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="336db-186">EF 內容類別可做為資料庫的工作單位的類別可讓您更新您不要使用 EF。</span><span class="sxs-lookup"><span data-stu-id="336db-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="336db-187">EF 包含而不需要撰寫的程式碼儲存機制實作 TDD 的功能。</span><span class="sxs-lookup"><span data-stu-id="336db-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="336db-188">如需如何實作儲存機制和工作模式單位資訊，請參閱[此教學課程系列的 Entity Framework 5 新版](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。</span><span class="sxs-lookup"><span data-stu-id="336db-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="336db-189">Entity Framework Core 實作可用於測試的記憶體中資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="336db-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="336db-190">如需詳細資訊，請參閱[測試 InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)。</span><span class="sxs-lookup"><span data-stu-id="336db-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="336db-191">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="336db-191">Automatic change detection</span></span>

<span data-ttu-id="336db-192">Entity Framework 藉由比較原始值與實體的目前值，決定如何變更實體 （並因此需要哪些更新傳送至資料庫）。</span><span class="sxs-lookup"><span data-stu-id="336db-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="336db-193">查詢或附加的實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="336db-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="336db-194">會導致變更自動偵測的方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="336db-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="336db-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="336db-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="336db-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="336db-196">DbContext.Entry</span></span>

* <span data-ttu-id="336db-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="336db-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="336db-198">如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善`ChangeTracker.AutoDetectChangesEnabled`屬性。</span><span class="sxs-lookup"><span data-stu-id="336db-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="336db-199">例如: </span><span class="sxs-lookup"><span data-stu-id="336db-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="336db-200">Entity Framework Core 來源的程式碼及開發計劃</span><span class="sxs-lookup"><span data-stu-id="336db-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="336db-201">Entity Framework Core 來源位於[https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="336db-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="336db-202">EF 核心儲存機制 含有夜間組建、 問題追蹤、 功能規格、 會議記錄、 設計和[未來的開發藍圖](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)。</span><span class="sxs-lookup"><span data-stu-id="336db-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="336db-203">您可以檔案或尋找 bug，並加入。</span><span class="sxs-lookup"><span data-stu-id="336db-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="336db-204">雖然開啟原始碼時，Entity Framework Core 則完全支援以 Microsoft 產品。</span><span class="sxs-lookup"><span data-stu-id="336db-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="336db-205">Microsoft Entity Framework 小組將控制接受哪些比重保留，並測試所有的程式碼變更，以確保每次發行的品質。</span><span class="sxs-lookup"><span data-stu-id="336db-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="336db-206">從現有資料庫的反向工程</span><span class="sxs-lookup"><span data-stu-id="336db-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="336db-207">若要反向工程資料模型，包括實體類別，從現有的資料庫，使用[scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext)命令。</span><span class="sxs-lookup"><span data-stu-id="336db-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="336db-208">請參閱[-快速入門教學課程](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)。</span><span class="sxs-lookup"><span data-stu-id="336db-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="336db-209">使用動態的 LINQ 來簡化排序選取範圍的程式碼</span><span class="sxs-lookup"><span data-stu-id="336db-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="336db-210">[本系列的第三個教學課程](sort-filter-page.md)示範以硬式編碼的資料行名稱來撰寫 LINQ 程式碼`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="336db-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="336db-211">可從中選擇的兩個資料行，這可正常運作，但如果您有許多資料行的程式碼可以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="336db-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="336db-212">若要解決這個問題，您可以使用`EF.Property`方法，以指定的屬性名稱做為字串。</span><span class="sxs-lookup"><span data-stu-id="336db-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="336db-213">若要試用這種方法，取代`Index`方法中的`StudentsController`為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="336db-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="336db-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="336db-214">Next steps</span></span>

<span data-ttu-id="336db-215">如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="336db-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="336db-216">如需 EF Core 的詳細資訊，請參閱[Entity Framework 的核心文件集](https://docs.microsoft.com/ef/core)。</span><span class="sxs-lookup"><span data-stu-id="336db-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="336db-217">也會提供一本書：[動作中的 Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)。</span><span class="sxs-lookup"><span data-stu-id="336db-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="336db-218">如需如何部署 web 應用程式的資訊，請參閱[主機和部署](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="336db-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="336db-219">如需 ASP.NET Core MVC、 驗證和授權，例如相關的其他主題，請參閱[ASP.NET Core 文件](https://docs.microsoft.com/aspnet/core/)。</span><span class="sxs-lookup"><span data-stu-id="336db-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="336db-220">通知</span><span class="sxs-lookup"><span data-stu-id="336db-220">Acknowledgments</span></span>

<span data-ttu-id="336db-221">Tom Dykstra 和 Rick Anderson (twitter @RickAndMSFT) 撰寫本教學課程。</span><span class="sxs-lookup"><span data-stu-id="336db-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="336db-222">Rowan Miller、 狄 Vega 和 Entity Framework 小組的其他成員協助程式碼檢閱，並幫助我們已針對教學課程中撰寫程式碼時，會發生的問題進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="336db-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="336db-223">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="336db-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="336db-224">另一個處理序所使用的 ContosoUniversity.dll</span><span class="sxs-lookup"><span data-stu-id="336db-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="336db-225">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="336db-225">Error message:</span></span>

> <span data-ttu-id="336db-226">無法開啟 '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 進行寫入-' 處理序無法存取檔案 '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 因為另一個處理序正在使用它。</span><span class="sxs-lookup"><span data-stu-id="336db-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="336db-227">解決方案:</span><span class="sxs-lookup"><span data-stu-id="336db-227">Solution:</span></span>

<span data-ttu-id="336db-228">停止 IIS Express 中的站台。</span><span class="sxs-lookup"><span data-stu-id="336db-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="336db-229">請移至 Windows 系統匣中，尋找 IIS Express 和它的圖示上按一下滑鼠右鍵，選取 Contoso 大學站台，然後按一下**停止站台**。</span><span class="sxs-lookup"><span data-stu-id="336db-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="336db-230">移轉建立結構中向上和向下方法沒有程式碼</span><span class="sxs-lookup"><span data-stu-id="336db-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="336db-231">可能的原因：</span><span class="sxs-lookup"><span data-stu-id="336db-231">Possible cause:</span></span>

<span data-ttu-id="336db-232">EF CLI 命令不會自動關閉並儲存程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="336db-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="336db-233">如果您未儲存變更，當您執行`migrations add`命令、 EF 找不到您的變更。</span><span class="sxs-lookup"><span data-stu-id="336db-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="336db-234">解決方案:</span><span class="sxs-lookup"><span data-stu-id="336db-234">Solution:</span></span>

<span data-ttu-id="336db-235">執行`migrations remove`命令，儲存您的程式碼變更，並重新執行`migrations add`命令。</span><span class="sxs-lookup"><span data-stu-id="336db-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="336db-236">執行資料庫更新時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="336db-236">Errors while running database update</span></span>

<span data-ttu-id="336db-237">很可能在具有現有資料的資料庫進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="336db-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="336db-238">如果您移轉時發生錯誤，您無法解決，您可以變更連接字串中的資料庫名稱，或刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="336db-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="336db-239">使用新資料庫時，若要移轉，沒有資料並更新資料庫指令更有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="336db-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="336db-240">最簡單的方法是在資料庫重新命名*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="336db-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="336db-241">下次您執行`database update`，就會建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="336db-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="336db-242">若要刪除 SSOX 中的資料庫，以滑鼠右鍵按一下資料庫，請按一下**刪除**，然後在**資料庫刪除**對話方塊中，選取**關閉現有連接**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="336db-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="336db-243">若要使用 CLI 來刪除資料庫，執行`database drop`CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="336db-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="336db-244">尋找 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="336db-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="336db-245">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="336db-245">Error Message:</span></span>

> <span data-ttu-id="336db-246">建立 SQL Server 的連接時發生網路相關或執行個體特定錯誤。</span><span class="sxs-lookup"><span data-stu-id="336db-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="336db-247">找不到或無法存取伺服器。</span><span class="sxs-lookup"><span data-stu-id="336db-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="336db-248">確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線</span><span class="sxs-lookup"><span data-stu-id="336db-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="336db-249">(提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)</span><span class="sxs-lookup"><span data-stu-id="336db-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="336db-250">解決方案:</span><span class="sxs-lookup"><span data-stu-id="336db-250">Solution:</span></span>

<span data-ttu-id="336db-251">請檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="336db-251">Check the connection string.</span></span> <span data-ttu-id="336db-252">如果您已經手動刪除資料庫檔案，變更建構字串以使用新的資料庫中資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="336db-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="336db-253">上一步</span><span class="sxs-lookup"><span data-stu-id="336db-253">Previous</span></span>](inheritance.md)
