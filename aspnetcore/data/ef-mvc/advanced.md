---
title: ASP.NET Core MVC 與 EF Core - 進階 - 10/10
author: rick-anderson
description: 本教學課程介紹一些實用主題，這些主題超出開發 ASP.NET Core Web 應用程式 (使用 Entity Framework Core ) 的基本概念。
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/advanced
ms.openlocfilehash: be44ef115ce72e1571bbdea2c609ea6c53792c59
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093071"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="b37db-103">ASP.NET Core MVC 與 EF Core - 進階 - 10/10</span><span class="sxs-lookup"><span data-stu-id="b37db-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b37db-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b37db-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b37db-105">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b37db-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="b37db-106">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="b37db-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="b37db-107">在上一個教學課程中，您實作了單表繼承。</span><span class="sxs-lookup"><span data-stu-id="b37db-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="b37db-108">本教學課程介紹幾個實用的主題，在超出開發 ASP.NET Core Web 應用程式 (使用 Entity Framework Core ) 的基本概念時，需要注意這些主題。</span><span class="sxs-lookup"><span data-stu-id="b37db-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="b37db-109">原始 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="b37db-109">Raw SQL Queries</span></span>

<span data-ttu-id="b37db-110">使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。</span><span class="sxs-lookup"><span data-stu-id="b37db-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="b37db-111">它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="b37db-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="b37db-112">但當您需要執行手動建立的特定 SQL 查詢時，會出現例外情況。</span><span class="sxs-lookup"><span data-stu-id="b37db-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="b37db-113">針對這些情況，Entity Framework Code 的第一個 API 會包含可讓您將 SQL 命令直接傳遞至資料庫的方法。</span><span class="sxs-lookup"><span data-stu-id="b37db-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="b37db-114">EF Core 1.0 中有下列選項可供您選擇：</span><span class="sxs-lookup"><span data-stu-id="b37db-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="b37db-115">針對傳回實體類型的查詢使用 `DbSet.FromSql` 方法。</span><span class="sxs-lookup"><span data-stu-id="b37db-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="b37db-116">傳回的物件必須是 `DbSet` 物件所預期的類型，而且除非您[關閉追蹤](crud.md#no-tracking-queries)，否則資料庫內容將自動追蹤它們。</span><span class="sxs-lookup"><span data-stu-id="b37db-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="b37db-117">針對非查詢命令使用 `Database.ExecuteSqlCommand`。</span><span class="sxs-lookup"><span data-stu-id="b37db-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="b37db-118">如果您需要執行傳回類型不是實體的查詢，則可以搭配使用 ADO.NET 與 EF 所提供的資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="b37db-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="b37db-119">即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="b37db-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="b37db-120">如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="b37db-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="b37db-121">執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="b37db-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="b37db-122">在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。</span><span class="sxs-lookup"><span data-stu-id="b37db-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="b37db-123">呼叫傳回實體的查詢</span><span class="sxs-lookup"><span data-stu-id="b37db-123">Call a query that returns entities</span></span>

<span data-ttu-id="b37db-124">`DbSet<TEntity>` 類別會提供一種方法，您可使用它來執行查詢，以傳回類型 `TEntity` 的實體。</span><span class="sxs-lookup"><span data-stu-id="b37db-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="b37db-125">若要查看其運作方式，您需要變更 Department 控制器的 `Details` 方法中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b37db-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="b37db-126">在 *DepartmentsController.cs* 中，將 `Details` 方法中擷取部門的程式碼取代為 `FromSql` 方法呼叫，如下列醒目提顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="b37db-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="b37db-127">若要確認新的程式碼運作正常，請選取 [部門]  索引標籤，然後針對其中一個部門選取 [詳細資料] 。</span><span class="sxs-lookup"><span data-stu-id="b37db-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門詳細資料](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="b37db-129">呼叫傳回其他類型的查詢</span><span class="sxs-lookup"><span data-stu-id="b37db-129">Call a query that returns other types</span></span>

<span data-ttu-id="b37db-130">先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。</span><span class="sxs-lookup"><span data-stu-id="b37db-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="b37db-131">您已從學生實體集 (`_context.Students`) 取得資料，並使用 LINQ 將結果投射到 `EnrollmentDateGroup` 檢視模型物件清單。</span><span class="sxs-lookup"><span data-stu-id="b37db-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="b37db-132">假設您想要撰寫 SQL 本身，而不是使用 LINQ。</span><span class="sxs-lookup"><span data-stu-id="b37db-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="b37db-133">若要執行這項操作，您必須執行 SQL 查詢以傳回實體物件以外的某些項目。</span><span class="sxs-lookup"><span data-stu-id="b37db-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="b37db-134">在 EF Core 1.0 中，執行這項操作的方法之一是撰寫 ADO.NET 程式碼，並從 EF 取得資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="b37db-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="b37db-135">在 *HomeController.cs* 中，以下列程式碼取代 `About` 方法：</span><span class="sxs-lookup"><span data-stu-id="b37db-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="b37db-136">新增 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="b37db-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="b37db-137">執行應用程式並移至 About 頁面。</span><span class="sxs-lookup"><span data-stu-id="b37db-137">Run the app and go to the About page.</span></span> <span data-ttu-id="b37db-138">它會顯示與之前相同的資料。</span><span class="sxs-lookup"><span data-stu-id="b37db-138">It displays the same data it did before.</span></span>

![About 頁面](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="b37db-140">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="b37db-140">Call an update query</span></span>

<span data-ttu-id="b37db-141">假設 Contoso 大學的系統管理員想要在資料庫中執行全域變更，例如變更每個課程的學分數。</span><span class="sxs-lookup"><span data-stu-id="b37db-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="b37db-142">如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。</span><span class="sxs-lookup"><span data-stu-id="b37db-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="b37db-143">在本節中，您將實作網頁，讓使用者能夠指定變更所有課程的學分數所要依據的因素，並將執行 SQL UPDATE 陳述式來進行變更。</span><span class="sxs-lookup"><span data-stu-id="b37db-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="b37db-144">網頁看起來將如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="b37db-144">The web page will look like the following illustration:</span></span>

![更新課程學分數頁面](advanced/_static/update-credits.png)

<span data-ttu-id="b37db-146">在 *CoursesContoller.cs* 中，為 HttpGet 和 HttpPost 新增 UpdateCourseCredits 方法：</span><span class="sxs-lookup"><span data-stu-id="b37db-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="b37db-147">當控制器處理 HttpGet 要求時，不會在 `ViewData["RowsAffected"]` 中傳回任何項目，而檢視會顯示空白的文字方塊和提交按鈕，如上圖所示。</span><span class="sxs-lookup"><span data-stu-id="b37db-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="b37db-148">按一下 [更新] 按鈕後，會呼叫 HttpPost 方法，而乘數具有在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="b37db-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="b37db-149">程式碼接著執行的 SQL 會更新課程，並將受影響的資料列數目傳回至 `ViewData` 中的檢視。</span><span class="sxs-lookup"><span data-stu-id="b37db-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="b37db-150">當檢視取得 `RowsAffected` 值時，它會顯示更新的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="b37db-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="b37db-151">在方案總管中，以滑鼠右鍵按一下 *Views/Courses* 資料夾，然後按一下 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="b37db-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="b37db-152">在 [新增項目] 對話方塊中，按一下左窗格中 [已安裝] 下的 [ASP.NET]，按一下 [MVC 檢視頁面]，並將新的檢視命名為 *UpdateCourseCredits.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="b37db-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="b37db-153">在 *Views/Courses/UpdateCourseCredits.cshtml* 中，以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="b37db-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="b37db-154">藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:5813/Courses/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。</span><span class="sxs-lookup"><span data-stu-id="b37db-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="b37db-155">在文字方塊中輸入數目：</span><span class="sxs-lookup"><span data-stu-id="b37db-155">Enter a number in the text box:</span></span>

![更新課程學分數頁面](advanced/_static/update-credits.png)

<span data-ttu-id="b37db-157">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="b37db-157">Click **Update**.</span></span> <span data-ttu-id="b37db-158">您會看到受影響的資料列數目：</span><span class="sxs-lookup"><span data-stu-id="b37db-158">You see the number of rows affected:</span></span>

![更新課程學分數頁面之受影響的資料列](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="b37db-160">按一下 [回到清單]，以查看課程與已修訂學分數的清單。</span><span class="sxs-lookup"><span data-stu-id="b37db-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="b37db-161">請注意，生產環境程式碼可確保更新一律會產生有效的資料。</span><span class="sxs-lookup"><span data-stu-id="b37db-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="b37db-162">此處顯示的簡化程式碼會增加足夠的學分數而使其數目大於 5。</span><span class="sxs-lookup"><span data-stu-id="b37db-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="b37db-163">(`Credits` 屬性具有 `[Range(0, 5)]` 屬性。)更新查詢可正常運作，但是無效的資料可能會導致系統的其他部分假設學分數為 5 或更少，進而發生非預期的結果。</span><span class="sxs-lookup"><span data-stu-id="b37db-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="b37db-164">如需原始 SQL 查詢的詳細資訊，請參閱[原始 SQL 查詢](https://docs.microsoft.com/ef/core/querying/raw-sql)。</span><span class="sxs-lookup"><span data-stu-id="b37db-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="b37db-165">檢查傳送至資料庫的 SQL</span><span class="sxs-lookup"><span data-stu-id="b37db-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="b37db-166">有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。</span><span class="sxs-lookup"><span data-stu-id="b37db-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="b37db-167">EF Core 會自動使用 ASP.NET Core 的內建記錄功能來寫入記錄檔，以包含用於查詢和更新的 SQL。</span><span class="sxs-lookup"><span data-stu-id="b37db-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="b37db-168">在本節中，您將看到 SQL 記錄的一些範例。</span><span class="sxs-lookup"><span data-stu-id="b37db-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="b37db-169">開啟 *StudentsController.cs*，並在 `Details` 方法中設定 `if (student == null)` 陳述式的中斷點。</span><span class="sxs-lookup"><span data-stu-id="b37db-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="b37db-170">以偵錯模式執行應用程式，並移至學生的 [詳細資料] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b37db-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="b37db-171">移至顯示偵錯輸出的 [輸出]  視窗，您會看到查詢：</span><span class="sxs-lookup"><span data-stu-id="b37db-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

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

<span data-ttu-id="b37db-172">您在這裡將發現可能會令您吃驚的某些事項：SQL 從 Person 資料表最多選取 2 個資料列 (`TOP(2)`)。</span><span class="sxs-lookup"><span data-stu-id="b37db-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="b37db-173">`SingleOrDefaultAsync` 方法不會解析為伺服器上的 1 個資料列。</span><span class="sxs-lookup"><span data-stu-id="b37db-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="b37db-174">原因如下：</span><span class="sxs-lookup"><span data-stu-id="b37db-174">Here's why:</span></span>

* <span data-ttu-id="b37db-175">如果查詢會傳回多個資料列，則方法會傳回 null。</span><span class="sxs-lookup"><span data-stu-id="b37db-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="b37db-176">若要判斷查詢是否會傳回多個資料列，EF 必須檢查它是否會至少傳回 2。</span><span class="sxs-lookup"><span data-stu-id="b37db-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="b37db-177">請注意，您不必使用偵錯模式並在中斷點處停止，便能在 [輸出]  視窗中取得記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="b37db-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="b37db-178">它只是在您想要查看輸出的點上停止記錄的便利方式。</span><span class="sxs-lookup"><span data-stu-id="b37db-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="b37db-179">如果不這樣做，記錄將繼續進行，而您必須往回捲動以尋找您感興趣的部分。</span><span class="sxs-lookup"><span data-stu-id="b37db-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="b37db-180">存放庫和工作單元模式</span><span class="sxs-lookup"><span data-stu-id="b37db-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="b37db-181">許多開發人員撰寫程式碼以實作存放庫和工作單元模式，作為使用 Entity Framework 之程式碼周圍的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="b37db-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="b37db-182">這些模式主要用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="b37db-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="b37db-183">實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="b37db-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="b37db-184">不過，撰寫額外的程式碼來實作這些模式並非一直是使用 EF 之應用程式的最佳選擇，原因如下：</span><span class="sxs-lookup"><span data-stu-id="b37db-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="b37db-185">EF 內容類別本身會隔離您的程式碼與資料存放區特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b37db-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="b37db-186">EF 內容類別可作為您使用 EF 進行之資料庫更新的工作單元類別。</span><span class="sxs-lookup"><span data-stu-id="b37db-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="b37db-187">EF 包含實作 TDD 而不需要撰寫存放庫程式碼的功能。</span><span class="sxs-lookup"><span data-stu-id="b37db-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="b37db-188">如需如何實作存放庫和工作單元模式的資訊，請參閱[本教學課程系列的 Entity Framework 第 5 版](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。</span><span class="sxs-lookup"><span data-stu-id="b37db-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="b37db-189">Entity Framework Core 實作了可用於測試的記憶體資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="b37db-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="b37db-190">如需詳細資訊，請參閱[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)。</span><span class="sxs-lookup"><span data-stu-id="b37db-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="b37db-191">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="b37db-191">Automatic change detection</span></span>

<span data-ttu-id="b37db-192">Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。</span><span class="sxs-lookup"><span data-stu-id="b37db-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="b37db-193">查詢或附加實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="b37db-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="b37db-194">會導致自動變更偵測的一些方法如下：</span><span class="sxs-lookup"><span data-stu-id="b37db-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="b37db-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="b37db-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="b37db-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="b37db-196">DbContext.Entry</span></span>

* <span data-ttu-id="b37db-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="b37db-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="b37db-198">如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，您可能會透過使用 `ChangeTracker.AutoDetectChangesEnabled` 屬性暫時關閉自動變更偵測，使效能獲得顯著改善。</span><span class="sxs-lookup"><span data-stu-id="b37db-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="b37db-199">例如: </span><span class="sxs-lookup"><span data-stu-id="b37db-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="b37db-200">Entity Framework Core 的原始程式碼和開發計劃</span><span class="sxs-lookup"><span data-stu-id="b37db-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="b37db-201">Entity Framework Core 來源位於 [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="b37db-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="b37db-202">EF Core 存放庫包含每夜組建、問題追蹤、功能規格、設計會議記錄和[未來開發藍圖](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)。</span><span class="sxs-lookup"><span data-stu-id="b37db-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="b37db-203">您可以提交或尋找 Bug，並做出貢獻。</span><span class="sxs-lookup"><span data-stu-id="b37db-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="b37db-204">雖然原始程式碼是開放式程式碼，但 Entity Framework Core 也作為 Microsoft 產品完整支援。</span><span class="sxs-lookup"><span data-stu-id="b37db-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="b37db-205">Microsoft Entity Framework 小組將控制接受哪些貢獻，並測試所有的程式碼變更以確保每次發行的品質。</span><span class="sxs-lookup"><span data-stu-id="b37db-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="b37db-206">從現有資料庫進行還原工程</span><span class="sxs-lookup"><span data-stu-id="b37db-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="b37db-207">若要從現有資料庫對包括實體類別的資料模型進行還原工程，請使用 [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) 命令。</span><span class="sxs-lookup"><span data-stu-id="b37db-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="b37db-208">請參閱[快速入門教學課程](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)。</span><span class="sxs-lookup"><span data-stu-id="b37db-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="b37db-209">使用動態的 LINQ 來簡化排序選取程式碼</span><span class="sxs-lookup"><span data-stu-id="b37db-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="b37db-210">[本系列的第三個教學課程](sort-filter-page.md)示範如何在 `switch` 陳述式中，以硬式編碼的資料行名稱來撰寫 LINQ 程式碼。</span><span class="sxs-lookup"><span data-stu-id="b37db-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="b37db-211">若有兩個資料行可供選擇，這可正常運作；但是如果您有許多資料行，程式碼可能變得冗長。</span><span class="sxs-lookup"><span data-stu-id="b37db-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="b37db-212">若要解決該問題，您可以使用 `EF.Property` 方法，以指定屬性的名稱作為字串。</span><span class="sxs-lookup"><span data-stu-id="b37db-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="b37db-213">若要試用這種方法，請以下列程式碼取代 `StudentsController` 中的 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="b37db-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="b37db-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b37db-214">Next steps</span></span>

<span data-ttu-id="b37db-215">如此即完成本系列中在 ASP.NET MVC 應用程式中使用 Entity Framework Core 的教學課程。</span><span class="sxs-lookup"><span data-stu-id="b37db-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="b37db-216">如需 EF Core 的詳細資訊，請參閱 [Entity Framework Core 文件](https://docs.microsoft.com/ef/core)。</span><span class="sxs-lookup"><span data-stu-id="b37db-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="b37db-217">另外，還提供了一本書：[Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action) (Entity Framework Core 實戰)。</span><span class="sxs-lookup"><span data-stu-id="b37db-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="b37db-218">如需如何部署 Web 應用程式的資訊，請參閱[裝載和部署](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="b37db-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="b37db-219">如需與 ASP.NET Core MVC 相關之其他主題 (例如驗證和授權) 的資訊，請參閱 [ASP.NET Core 文件](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="b37db-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](xref:index).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="b37db-220">感謝</span><span class="sxs-lookup"><span data-stu-id="b37db-220">Acknowledgments</span></span>

<span data-ttu-id="b37db-221">Tom Dykstra 和 Rick Anderson (Twitter @RickAndMSFT) 撰寫了本教學課程。</span><span class="sxs-lookup"><span data-stu-id="b37db-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="b37db-222">Rowan Miller、Diego Vega 和其他 Entity Framework 小組成員協助進行程式碼檢閱，並協助對撰寫本教學課程的程式碼時發生的問題進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="b37db-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="b37db-223">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="b37db-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="b37db-224">ContosoUniversity.dll 已由其他處理序使用</span><span class="sxs-lookup"><span data-stu-id="b37db-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="b37db-225">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="b37db-225">Error message:</span></span>

> <span data-ttu-id="b37db-226">無法開啟 '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 進行寫入 -- '處理序無法存取 '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 檔案，因為其他處理序正在使用它。</span><span class="sxs-lookup"><span data-stu-id="b37db-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="b37db-227">解決方案:</span><span class="sxs-lookup"><span data-stu-id="b37db-227">Solution:</span></span>

<span data-ttu-id="b37db-228">停止 IIS Express 中的網站。</span><span class="sxs-lookup"><span data-stu-id="b37db-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="b37db-229">移至 Windows 系統匣中，尋找 IIS Express 並以滑鼠右鍵按一下其圖示，選取 Contoso 大學網站，然後按一下 [停止網站]。</span><span class="sxs-lookup"><span data-stu-id="b37db-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="b37db-230">Up 和 Down 方法中沒有程式碼的 Scaffold 移轉</span><span class="sxs-lookup"><span data-stu-id="b37db-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="b37db-231">可能的原因：</span><span class="sxs-lookup"><span data-stu-id="b37db-231">Possible cause:</span></span>

<span data-ttu-id="b37db-232">EF CLI 命令不會自動關閉並儲存程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="b37db-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="b37db-233">如果您有未儲存的變更，當您執行 `migrations add` 命令時，EF 找不到您的變更。</span><span class="sxs-lookup"><span data-stu-id="b37db-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="b37db-234">解決方案:</span><span class="sxs-lookup"><span data-stu-id="b37db-234">Solution:</span></span>

<span data-ttu-id="b37db-235">執行 `migrations remove` 命令，儲存您的程式碼變更，並重新執行 `migrations add` 命令。</span><span class="sxs-lookup"><span data-stu-id="b37db-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="b37db-236">執行資料庫更新時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="b37db-236">Errors while running database update</span></span>

<span data-ttu-id="b37db-237">在具有現有資料的資料庫中進行結構描述變更時，可能會收到其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="b37db-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="b37db-238">如果收到無法解決的移轉錯誤，您可以變更連接字串中的資料庫名稱，或刪除該資料庫。</span><span class="sxs-lookup"><span data-stu-id="b37db-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="b37db-239">使用新資料庫時，沒有可移轉的資料，因此 update-database 命令更可能會在沒有錯誤的情況下完成。</span><span class="sxs-lookup"><span data-stu-id="b37db-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="b37db-240">最簡單的方法是在 *appsettings.json* 中重新命名資料庫。</span><span class="sxs-lookup"><span data-stu-id="b37db-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="b37db-241">下次您執行 `database update` 時，就會建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b37db-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="b37db-242">若要刪除 SSOX 中的資料庫，請以滑鼠右鍵按一下該資料庫，按一下 [刪除]，然後在 [刪除資料庫] 對話方塊中選取 [關閉現有的連線]，並按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="b37db-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="b37db-243">若要使用 CLI 來刪除資料庫，請執行 `database drop` CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="b37db-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="b37db-244">搜尋 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="b37db-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="b37db-245">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="b37db-245">Error Message:</span></span>

> <span data-ttu-id="b37db-246">建立與 SQL Server　的連線時，發生與網路相關的錯誤或是執行個體特有的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b37db-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="b37db-247">找不到或無法存取伺服器。</span><span class="sxs-lookup"><span data-stu-id="b37db-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="b37db-248">確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線</span><span class="sxs-lookup"><span data-stu-id="b37db-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="b37db-249">(提供者：SQL 網路介面，錯誤：26 - 搜尋指定的伺服器/執行個體時發生錯誤)</span><span class="sxs-lookup"><span data-stu-id="b37db-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="b37db-250">解決方案:</span><span class="sxs-lookup"><span data-stu-id="b37db-250">Solution:</span></span>

<span data-ttu-id="b37db-251">檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="b37db-251">Check the connection string.</span></span> <span data-ttu-id="b37db-252">如果您已手動刪除資料庫檔案，請變更建構字串的資料庫名稱，以重新開始使用新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b37db-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="b37db-253">上一步</span><span class="sxs-lookup"><span data-stu-id="b37db-253">Previous</span></span>](inheritance.md)
