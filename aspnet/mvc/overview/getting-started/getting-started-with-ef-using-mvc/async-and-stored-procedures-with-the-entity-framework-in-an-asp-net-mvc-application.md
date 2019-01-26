---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：使用 async 和 ASP.NET MVC 應用程式中的 ef 的預存程序
description: 在本教學課程中，您會看到如何實作非同步程式設計模型，並了解如何使用預存程序。
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889869"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="5e12d-103">教學課程：使用 async 和 ASP.NET MVC 應用程式中的 ef 的預存程序</span><span class="sxs-lookup"><span data-stu-id="5e12d-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="5e12d-104">在先前的教學課程中，您學會如何讀取和更新使用同步程式設計模型的資料。</span><span class="sxs-lookup"><span data-stu-id="5e12d-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="5e12d-105">在本教學課程中，您會看到如何實作非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="5e12d-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="5e12d-106">非同步程式碼有助於比較好，因為它能夠更妥善運用伺服器資源的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e12d-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="5e12d-107">在本教學課程中您也了解如何使用預存程序來插入、 更新和刪除作業，在實體上。</span><span class="sxs-lookup"><span data-stu-id="5e12d-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="5e12d-108">最後，您重新部署至 Azure，以及您部署的第一次之後，您已實作的資料庫變更的所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e12d-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="5e12d-109">下列圖例顯示了您將操作的一些頁面。</span><span class="sxs-lookup"><span data-stu-id="5e12d-109">The following illustrations show some of the pages that you'll work with.</span></span>

![部門頁面](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![建立部門](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="5e12d-112">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="5e12d-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e12d-113">深入了解非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="5e12d-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="5e12d-114">建立 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="5e12d-114">Create a Department controller</span></span>
> * <span data-ttu-id="5e12d-115">使用預存程序</span><span class="sxs-lookup"><span data-stu-id="5e12d-115">Use stored procedures</span></span>
> * <span data-ttu-id="5e12d-116">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="5e12d-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e12d-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="5e12d-117">Prerequisites</span></span>

* [<span data-ttu-id="5e12d-118">更新相關資料</span><span class="sxs-lookup"><span data-stu-id="5e12d-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="5e12d-119">為何要使用非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="5e12d-119">Why use asynchronous code</span></span>

<span data-ttu-id="5e12d-120">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="5e12d-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="5e12d-121">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="5e12d-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="5e12d-122">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="5e12d-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="5e12d-123">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="5e12d-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="5e12d-124">如此一來，非同步程式碼可讓伺服器資源會更有效率地使用，而且伺服器已啟用以處理更多的流量，不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="5e12d-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="5e12d-125">在舊版的.NET 中，撰寫和測試非同步程式碼很複雜，容易出錯且難以偵錯。</span><span class="sxs-lookup"><span data-stu-id="5e12d-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="5e12d-126">在.NET 4.5 中，撰寫、 測試和偵錯非同步程式碼會更加容易，您通常應該寫入非同步程式碼除非您有理由不要。</span><span class="sxs-lookup"><span data-stu-id="5e12d-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="5e12d-127">非同步程式碼會導致少量的額外負荷，但低流量情況下效能的衝擊非常微小; 在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="5e12d-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="5e12d-128">如需有關非同步程式設計的詳細資訊，請參閱 <<c0> [ 使用.NET 4.5 的非同步支援，以避免封鎖呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)。</span><span class="sxs-lookup"><span data-stu-id="5e12d-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="5e12d-129">建立 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="5e12d-129">Create Department controller</span></span>

<span data-ttu-id="5e12d-130">建立使用相同方式較早的控制站，但這次選取 Department 控制器**使用非同步控制器動作**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5e12d-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="5e12d-131">下列反白顯示的同步程式碼新增了哪些功能`Index`進行非同步的方法：</span><span class="sxs-lookup"><span data-stu-id="5e12d-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="5e12d-132">若要啟用 Entity Framework 資料庫查詢，以非同步方式執行，已套用四個變更：</span><span class="sxs-lookup"><span data-stu-id="5e12d-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="5e12d-133">方法會標示`async`關鍵字，它會告訴編譯器方法主體的一部分產生回呼，並自動建立`Task<ActionResult>`傳回物件。</span><span class="sxs-lookup"><span data-stu-id="5e12d-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="5e12d-134">傳回的型別已經從`ActionResult`至`Task<ActionResult>`。</span><span class="sxs-lookup"><span data-stu-id="5e12d-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="5e12d-135">`Task<T>`型別代表進行中的工作，其結果型別的`T`。</span><span class="sxs-lookup"><span data-stu-id="5e12d-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="5e12d-136">`await`關鍵字已套用至 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="5e12d-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="5e12d-137">當編譯器看到這個關鍵字時，在幕後它會將方法分成兩個部分。</span><span class="sxs-lookup"><span data-stu-id="5e12d-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="5e12d-138">第一個部分的結尾會以非同步方式啟動的作業。</span><span class="sxs-lookup"><span data-stu-id="5e12d-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="5e12d-139">第二個部分會放入作業完成時呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="5e12d-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="5e12d-140">非同步版本`ToList`呼叫擴充方法。</span><span class="sxs-lookup"><span data-stu-id="5e12d-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="5e12d-141">為什麼會`departments.ToList`陳述式但不是修改`departments = db.Departments`陳述式？</span><span class="sxs-lookup"><span data-stu-id="5e12d-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="5e12d-142">原因是只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="5e12d-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="5e12d-143">`departments = db.Departments`陳述式會設定查詢，但查詢不會執行直到`ToList`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="5e12d-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="5e12d-144">因此，只有`ToList`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="5e12d-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="5e12d-145">在`Details`方法和`HttpGet``Edit`並`Delete`方法，`Find`方法就是會使查詢傳送至資料庫，因此會以非同步方式執行的方法：</span><span class="sxs-lookup"><span data-stu-id="5e12d-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="5e12d-146">在  `Create`， `HttpPost Edit`，和`DeleteConfirmed`方法，它是`SaveChanges`會導致要執行命令的方法呼叫，不等的陳述式`db.Departments.Add(department)`這只會導致實體記憶體中要修改。</span><span class="sxs-lookup"><span data-stu-id="5e12d-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="5e12d-147">開啟*Views\Department\Index.cshtml*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e12d-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="5e12d-148">此程式碼索引從標題變更為 「 部門，往右移動，系統管理員名稱，並提供系統管理員的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="5e12d-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="5e12d-149">在 Create、 Delete、 Details、 和編輯檢視、 變更標題`InstructorID`欄位設為 「 系統管理員 」 課程檢視中變更 「 部門 」 部門的 [名稱] 欄位的方式相同。</span><span class="sxs-lookup"><span data-stu-id="5e12d-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="5e12d-150">在 建立 和 編輯檢視，請使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e12d-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="5e12d-151">在 刪除 和 詳細資料檢視中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5e12d-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="5e12d-152">執行應用程式，然後按一下**部門** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="5e12d-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="5e12d-153">一切都運作中的相同其他控制器，但此控制器中所有的 SQL 查詢會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="5e12d-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="5e12d-154">要注意當您使用 Entity Framework 使用非同步程式設計的一些事項：</span><span class="sxs-lookup"><span data-stu-id="5e12d-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="5e12d-155">非同步程式碼不是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="5e12d-155">The async code is not thread safe.</span></span> <span data-ttu-id="5e12d-156">換句話說，也就是說，不要嘗試執行多個作業以平行方式使用相同的內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="5e12d-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="5e12d-157">若您想要充分利用非同步程式碼帶來的效能優點，請確保任何您正在使用的程式庫 (例如分頁) 也使用了非同步 (若它們有呼叫任何可能會傳送查詢到資料庫的 Entity Framework 方法的話)。</span><span class="sxs-lookup"><span data-stu-id="5e12d-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="5e12d-158">使用預存程序</span><span class="sxs-lookup"><span data-stu-id="5e12d-158">Use stored procedures</span></span>

<span data-ttu-id="5e12d-159">有些開發人員和 Dba 想要使用預存程序來存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="5e12d-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="5e12d-160">在舊版的 Entity Framework 中，您可以擷取使用預存程序的資料[執行原始的 SQL 查詢](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)，但您無法指示 EF 來使用預存程序進行更新作業。</span><span class="sxs-lookup"><span data-stu-id="5e12d-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="5e12d-161">在 EF 6 很容易設定 Code First 至使用預存程序。</span><span class="sxs-lookup"><span data-stu-id="5e12d-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="5e12d-162">在  *DAL\SchoolContext.cs*，將反白顯示的程式碼加入`OnModelCreating`方法。</span><span class="sxs-lookup"><span data-stu-id="5e12d-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="5e12d-163">此程式碼會指示 Entity Framework 使用預存程序插入、 更新和刪除作業上`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="5e12d-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="5e12d-164">在封裝管理主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5e12d-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="5e12d-165">開啟*移轉\&l t; 時間戳記&gt;\_DepartmentSP.cs*若要查看中的程式碼`Up`方法，以建立 Insert、 Update 和 Delete 預存程序：</span><span class="sxs-lookup"><span data-stu-id="5e12d-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="5e12d-166">在封裝管理主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5e12d-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="5e12d-167">在 偵錯模式中執行應用程式中，按一下**部門**索引標籤，然後再按一下**建立新**。</span><span class="sxs-lookup"><span data-stu-id="5e12d-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="5e12d-168">新的部門中，輸入資料，然後按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="5e12d-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="5e12d-169">在 Visual Studio 中檢視記錄檔，在**輸出**視窗來查看預存程序用來插入新的 Department 資料列。</span><span class="sxs-lookup"><span data-stu-id="5e12d-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![部門 Insert 預存程序](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="5e12d-171">程式碼首先會建立預設預存程序名稱。</span><span class="sxs-lookup"><span data-stu-id="5e12d-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="5e12d-172">如果您使用現有的資料庫，您可能需要自訂預存程序名稱，若要使用已經在資料庫中定義的預存程序。</span><span class="sxs-lookup"><span data-stu-id="5e12d-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="5e12d-173">如需如何執行該動作的資訊，請參閱[Entity Framework 程式碼第一個 Insert/Update/Delete 預存程序](https://msdn.microsoft.com/data/dn468673)。</span><span class="sxs-lookup"><span data-stu-id="5e12d-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="5e12d-174">如果您想要自訂項目產生預存程序執行，您可以編輯移轉的 scaffold 程式碼`Up`方法，以建立預存程序。</span><span class="sxs-lookup"><span data-stu-id="5e12d-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="5e12d-175">如此一來您的變更會反映每當移轉執行時，並會在部署後的生產環境中自動執行移轉時套用到您的生產資料庫。</span><span class="sxs-lookup"><span data-stu-id="5e12d-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="5e12d-176">如果您想要變更現有的預存程序中先前移轉所建立，您可以使用新增移轉命令來產生空白的移轉，並以手動方式撰寫程式碼呼叫[AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)方法.</span><span class="sxs-lookup"><span data-stu-id="5e12d-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="5e12d-177">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="5e12d-177">Deploy to Azure</span></span>

<span data-ttu-id="5e12d-178">本章節會要求您已經完成選擇性**將應用程式部署至 Azure**一節[移轉和部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教學課程。</span><span class="sxs-lookup"><span data-stu-id="5e12d-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="5e12d-179">如果您刪除本機專案中的資料庫來判斷已解決的移轉錯誤，請略過本節。</span><span class="sxs-lookup"><span data-stu-id="5e12d-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="5e12d-180">在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="5e12d-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="5e12d-181">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="5e12d-181">Click **Publish**.</span></span>

    <span data-ttu-id="5e12d-182">Visual Studio 會部署至 Azure，應用程式和應用程式會開啟預設瀏覽器，在 Azure 中執行。</span><span class="sxs-lookup"><span data-stu-id="5e12d-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="5e12d-183">測試應用程式，以確認它是否運作。</span><span class="sxs-lookup"><span data-stu-id="5e12d-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="5e12d-184">第一次您執行頁面，來存取資料庫，Entity Framework 便會執行所有移轉`Up`才能讓資料庫維持在最新狀態與目前的資料模型的方法。</span><span class="sxs-lookup"><span data-stu-id="5e12d-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="5e12d-185">您現在可以使用所有的部署，包括您在本教學課程新增部門頁面最後一次您新增的網頁。</span><span class="sxs-lookup"><span data-stu-id="5e12d-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="5e12d-186">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="5e12d-186">Get the code</span></span>

[<span data-ttu-id="5e12d-187">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="5e12d-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="5e12d-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="5e12d-188">Additional resources</span></span>

<span data-ttu-id="5e12d-189">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="5e12d-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e12d-190">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5e12d-190">Next steps</span></span>

<span data-ttu-id="5e12d-191">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="5e12d-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e12d-192">了解非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="5e12d-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="5e12d-193">建立 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="5e12d-193">Created a Department controller</span></span>
> * <span data-ttu-id="5e12d-194">使用預存程序</span><span class="sxs-lookup"><span data-stu-id="5e12d-194">Used stored procedures</span></span>
> * <span data-ttu-id="5e12d-195">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="5e12d-195">Deployed to Azure</span></span>

<span data-ttu-id="5e12d-196">請前往下一篇文章，以了解如何在多位使用者同時更新相同的實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="5e12d-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="5e12d-197">處理並行</span><span class="sxs-lookup"><span data-stu-id="5e12d-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
