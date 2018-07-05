---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async 和 Entity framework 的 ASP.NET MVC 應用程式中的預存程序 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c2deb53856d79a52415db48e04ea96111833b02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381925"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="86019-103">Async 和 Entity framework 的 ASP.NET MVC 應用程式中的預存程序</span><span class="sxs-lookup"><span data-stu-id="86019-103">Async and Stored Procedures with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="86019-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="86019-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="86019-105">[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="86019-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="86019-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="86019-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="86019-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="86019-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="86019-108">在先前的教學課程中，您學會如何讀取和更新使用同步程式設計模型的資料。</span><span class="sxs-lookup"><span data-stu-id="86019-108">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="86019-109">在本教學課程中，您會看到如何實作非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="86019-109">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="86019-110">非同步程式碼有助於比較好，因為它能夠更妥善運用伺服器資源的應用程式。</span><span class="sxs-lookup"><span data-stu-id="86019-110">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="86019-111">在本教學課程也會看到如何使用預存程序插入、 更新和刪除作業的實體。</span><span class="sxs-lookup"><span data-stu-id="86019-111">In this tutorial you'll also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="86019-112">最後，您將會重新部署至 Azure，以及您部署的第一次之後，您已實作的資料庫變更的所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="86019-112">Finally, you'll redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="86019-113">下列圖例顯示了您將操作的一些頁面。</span><span class="sxs-lookup"><span data-stu-id="86019-113">The following illustrations show some of the pages that you'll work with.</span></span>

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a><span data-ttu-id="86019-116">為何要麻煩使用非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="86019-116">Why bother with asynchronous code</span></span>

<span data-ttu-id="86019-117">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="86019-117">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="86019-118">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="86019-118">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="86019-119">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="86019-119">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="86019-120">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="86019-120">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="86019-121">如此一來，非同步程式碼可讓伺服器資源會更有效率地使用，而且伺服器已啟用以處理更多的流量，不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="86019-121">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="86019-122">在舊版的.NET 中，撰寫和測試非同步程式碼很複雜，容易出錯且難以偵錯。</span><span class="sxs-lookup"><span data-stu-id="86019-122">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="86019-123">在.NET 4.5 中，撰寫、 測試和偵錯非同步程式碼會更加容易，您通常應該寫入非同步程式碼除非您有理由不要。</span><span class="sxs-lookup"><span data-stu-id="86019-123">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="86019-124">非同步程式碼會導致少量的額外負荷，但低流量情況下效能的衝擊非常微小; 在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="86019-124">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="86019-125">如需有關非同步程式設計的詳細資訊，請參閱 <<c0> [ 使用.NET 4.5 的非同步支援，以避免封鎖呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)。</span><span class="sxs-lookup"><span data-stu-id="86019-125">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-the-department-controller"></a><span data-ttu-id="86019-126">建立 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="86019-126">Create the Department controller</span></span>

<span data-ttu-id="86019-127">建立使用相同方式較早的控制站，但這次選取 Department 控制器**使用非同步控制器**動作 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="86019-127">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller** actions check box.</span></span>

![部門控制站 scaffold](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="86019-129">下列反白顯示的同步程式碼新增了哪些功能`Index`進行非同步的方法：</span><span class="sxs-lookup"><span data-stu-id="86019-129">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="86019-130">若要啟用 Entity Framework 資料庫查詢，以非同步方式執行，已套用四個變更：</span><span class="sxs-lookup"><span data-stu-id="86019-130">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="86019-131">方法會標示`async`關鍵字，它會告訴編譯器方法主體的一部分產生回呼，並自動建立`Task<ActionResult>`傳回物件。</span><span class="sxs-lookup"><span data-stu-id="86019-131">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="86019-132">傳回的型別已經從`ActionResult`至`Task<ActionResult>`。</span><span class="sxs-lookup"><span data-stu-id="86019-132">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="86019-133">`Task<T>`型別代表進行中的工作，其結果型別的`T`。</span><span class="sxs-lookup"><span data-stu-id="86019-133">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="86019-134">`await`關鍵字已套用至 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="86019-134">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="86019-135">當編譯器看到這個關鍵字時，在幕後它會將方法分成兩個部分。</span><span class="sxs-lookup"><span data-stu-id="86019-135">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="86019-136">第一個部分的結尾會以非同步方式啟動的作業。</span><span class="sxs-lookup"><span data-stu-id="86019-136">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="86019-137">第二個部分會放入作業完成時呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="86019-137">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="86019-138">非同步版本`ToList`呼叫擴充方法。</span><span class="sxs-lookup"><span data-stu-id="86019-138">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="86019-139">為什麼會`departments.ToList`陳述式但不是修改`departments = db.Departments`陳述式？</span><span class="sxs-lookup"><span data-stu-id="86019-139">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="86019-140">原因是只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="86019-140">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="86019-141">`departments = db.Departments`陳述式會設定查詢，但查詢不會執行直到`ToList`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="86019-141">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="86019-142">因此，只有`ToList`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="86019-142">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="86019-143">在`Details`方法和`HttpGet``Edit`並`Delete`方法，`Find`方法就是會使查詢傳送至資料庫，因此會以非同步方式執行的方法：</span><span class="sxs-lookup"><span data-stu-id="86019-143">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="86019-144">在  `Create`， `HttpPost Edit`，和`DeleteConfirmed`方法，它是`SaveChanges`會導致要執行命令的方法呼叫，不等的陳述式`db.Departments.Add(department)`這只會導致實體記憶體中要修改。</span><span class="sxs-lookup"><span data-stu-id="86019-144">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="86019-145">開啟*Views\Department\Index.cshtml*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="86019-145">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="86019-146">此程式碼索引從標題變更為 「 部門，往右移動，系統管理員名稱，並提供系統管理員的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="86019-146">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="86019-147">在 Create、 Delete、 Details、 和編輯檢視、 變更標題`InstructorID`欄位設為 「 系統管理員 」 課程檢視中變更 「 部門 」 部門的 [名稱] 欄位的方式相同。</span><span class="sxs-lookup"><span data-stu-id="86019-147">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="86019-148">在 建立 和 編輯檢視，請使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="86019-148">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="86019-149">在 刪除 和 詳細資料檢視中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="86019-149">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="86019-150">執行應用程式，然後按一下**部門** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="86019-150">Run the application, and click the **Departments** tab.</span></span>

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="86019-152">一切都運作中的相同其他控制器，但此控制器中所有的 SQL 查詢會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="86019-152">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="86019-153">要注意當您使用 Entity Framework 使用非同步程式設計的一些事項：</span><span class="sxs-lookup"><span data-stu-id="86019-153">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="86019-154">非同步程式碼不是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="86019-154">The async code is not thread safe.</span></span> <span data-ttu-id="86019-155">換句話說，也就是說，不要嘗試執行多個作業以平行方式使用相同的內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="86019-155">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="86019-156">若您想要充分利用非同步程式碼帶來的效能優點，請確保任何您正在使用的程式庫 (例如分頁) 也使用了非同步 (若它們有呼叫任何可能會傳送查詢到資料庫的 Entity Framework 方法的話)。</span><span class="sxs-lookup"><span data-stu-id="86019-156">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a><span data-ttu-id="86019-157">用於插入、 更新和刪除的預存程序</span><span class="sxs-lookup"><span data-stu-id="86019-157">Use stored procedures for inserting, updating, and deleting</span></span>

<span data-ttu-id="86019-158">有些開發人員和 Dba 想要使用預存程序來存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="86019-158">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="86019-159">在舊版的 Entity Framework 中，您可以擷取使用預存程序的資料[執行原始的 SQL 查詢](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)，但您無法指示 EF 來使用預存程序進行更新作業。</span><span class="sxs-lookup"><span data-stu-id="86019-159">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="86019-160">在 EF 6 很容易設定 Code First 至使用預存程序。</span><span class="sxs-lookup"><span data-stu-id="86019-160">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="86019-161">在  *DAL\SchoolContext.cs*，將反白顯示的程式碼加入`OnModelCreating`方法。</span><span class="sxs-lookup"><span data-stu-id="86019-161">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="86019-162">此程式碼會指示 Entity Framework 使用預存程序插入、 更新和刪除作業上`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="86019-162">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="86019-163">在封裝管理主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="86019-163">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="86019-164">開啟*移轉\&l t; 時間戳記&gt;\_DepartmentSP.cs*若要查看中的程式碼`Up`方法，以建立 Insert、 Update 和 Delete 預存程序：</span><span class="sxs-lookup"><span data-stu-id="86019-164">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="86019-165">在封裝管理主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="86019-165">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="86019-166">在 偵錯模式中執行應用程式中，按一下**部門**索引標籤，然後再按一下**建立新**。</span><span class="sxs-lookup"><span data-stu-id="86019-166">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="86019-167">新的部門中，輸入資料，然後按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="86019-167">Enter data for a new department, and then click **Create**.</span></span>

     ![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. <span data-ttu-id="86019-169">在 Visual Studio 中檢視記錄檔，在**輸出**視窗來查看預存程序用來插入新的 Department 資料列。</span><span class="sxs-lookup"><span data-stu-id="86019-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![部門 Insert 預存程序](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="86019-171">程式碼首先會建立預設預存程序名稱。</span><span class="sxs-lookup"><span data-stu-id="86019-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="86019-172">如果您使用現有的資料庫，您可能需要自訂預存程序名稱，若要使用已經在資料庫中定義的預存程序。</span><span class="sxs-lookup"><span data-stu-id="86019-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="86019-173">如需如何執行該動作的資訊，請參閱[Entity Framework 程式碼第一個 Insert/Update/Delete 預存程序](https://msdn.microsoft.com/data/dn468673)。</span><span class="sxs-lookup"><span data-stu-id="86019-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="86019-174">如果您想要自訂項目產生預存程序執行，您可以編輯移轉的 scaffold 程式碼`Up`方法，以建立預存程序。</span><span class="sxs-lookup"><span data-stu-id="86019-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="86019-175">如此一來您的變更會反映每當移轉執行時，並會在部署後的生產環境中自動執行移轉時套用到您的生產資料庫。</span><span class="sxs-lookup"><span data-stu-id="86019-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="86019-176">如果您想要變更現有的預存程序中先前移轉所建立，您可以使用新增移轉命令來產生空白的移轉，並以手動方式撰寫程式碼呼叫[AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)方法.</span><span class="sxs-lookup"><span data-stu-id="86019-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="86019-177">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="86019-177">Deploy to Azure</span></span>

<span data-ttu-id="86019-178">本章節會要求您已經完成選擇性**將應用程式部署至 Azure**一節[移轉和部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教學課程。</span><span class="sxs-lookup"><span data-stu-id="86019-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="86019-179">如果您刪除本機專案中的資料庫來判斷已解決的移轉錯誤，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="86019-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="86019-180">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="86019-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="86019-181">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="86019-181">Click **Publish**.</span></span>

    <span data-ttu-id="86019-182">Visual Studio 會部署至 Azure，應用程式和應用程式會開啟預設瀏覽器，在 Azure 中執行。</span><span class="sxs-lookup"><span data-stu-id="86019-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="86019-183">測試應用程式，以確認它是否運作。</span><span class="sxs-lookup"><span data-stu-id="86019-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="86019-184">第一次您執行頁面，來存取資料庫，Entity Framework 便會執行所有移轉`Up`才能讓資料庫維持在最新狀態與目前的資料模型的方法。</span><span class="sxs-lookup"><span data-stu-id="86019-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="86019-185">您現在可以使用所有的部署，包括您在本教學課程新增部門頁面最後一次您新增的網頁。</span><span class="sxs-lookup"><span data-stu-id="86019-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="summary"></a><span data-ttu-id="86019-186">總結</span><span class="sxs-lookup"><span data-stu-id="86019-186">Summary</span></span>

<span data-ttu-id="86019-187">在本教學課程中您會看到如何增進伺服器的效能，撰寫程式碼以非同步方式執行，以及如何使用預存程序插入、 更新和刪除作業。</span><span class="sxs-lookup"><span data-stu-id="86019-187">In this tutorial you saw how to improve server efficiency by writing code that executes asynchronously, and how to use stored procedures for insert, update, and delete operations.</span></span> <span data-ttu-id="86019-188">在下一個教學課程中，您會看到如何防止資料遺失，當多位使用者嘗試同時編輯同一筆記錄。</span><span class="sxs-lookup"><span data-stu-id="86019-188">In the next tutorial, you'll see how to prevent data loss when multiple users try to edit the same record at the same time.</span></span>

<span data-ttu-id="86019-189">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="86019-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86019-190">[上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="86019-190">[Previous](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
