---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 處理使用 Entity Framework 6 在 ASP.NET MVC 5 應用程式 (10 小時，共 12) 中的並行 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: aspnetcontent
ms.date: 12/08/2014
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b971e4ca16ecbf79f47e0aab8303a05452279b68
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837227"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a><span data-ttu-id="9f4ec-103">處理並行使用 Entity Framework 6 在 ASP.NET MVC 5 應用程式 (10 小時，共 12)</span><span class="sxs-lookup"><span data-stu-id="9f4ec-103">Handling Concurrency with the Entity Framework 6 in an ASP.NET MVC 5 Application (10 of 12)</span></span>
====================
<span data-ttu-id="9f4ec-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9f4ec-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9f4ec-105">[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="9f4ec-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="9f4ec-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="9f4ec-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="9f4ec-108">在先前的教學課程中，您學會如何更新資料。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-108">In earlier tutorials you learned how to update data.</span></span> <span data-ttu-id="9f4ec-109">本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-109">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="9f4ec-110">您要變更使用的網頁`Department`實體，使它們處理並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-110">You'll change the web pages that work with the `Department` entity so that they handle concurrency errors.</span></span> <span data-ttu-id="9f4ec-111">下列圖例顯示索引 [刪除] 頁面，其中包括一些發生並行衝突會顯示的訊息。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-111">The following illustrations show the Index and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="9f4ec-114">並行衝突</span><span class="sxs-lookup"><span data-stu-id="9f4ec-114">Concurrency Conflicts</span></span>

<span data-ttu-id="9f4ec-115">當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-115">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="9f4ec-116">若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-116">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="9f4ec-117">在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-117">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="9f4ec-118">在此情況下，您便不需要設定應用程式來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-118">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="9f4ec-119">封閉式並行存取 （鎖定）</span><span class="sxs-lookup"><span data-stu-id="9f4ec-119">Pessimistic Concurrency (Locking)</span></span>

<span data-ttu-id="9f4ec-120">若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-120">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="9f4ec-121">這就叫做*封閉式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-121">This is called *pessimistic concurrency*.</span></span> <span data-ttu-id="9f4ec-122">例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-122">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="9f4ec-123">若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-123">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="9f4ec-124">若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-124">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="9f4ec-125">管理鎖定有幾個缺點。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-125">Managing locks has disadvantages.</span></span> <span data-ttu-id="9f4ec-126">其程式可能相當複雜。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-126">It can be complex to program.</span></span> <span data-ttu-id="9f4ec-127">這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-127">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="9f4ec-128">基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-128">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="9f4ec-129">Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-129">The Entity Framework provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="9f4ec-130">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="9f4ec-130">Optimistic Concurrency</span></span>

<span data-ttu-id="9f4ec-131">封閉式並行存取的替代方案是*開放式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-131">The alternative to pessimistic concurrency is *optimistic concurrency*.</span></span> <span data-ttu-id="9f4ec-132">開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-132">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="9f4ec-133">例如，John 在部門編輯頁面上，將英文部門的**預算**從$350,000.00 變更為$0.00。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-133">For example, John runs the Departments Edit page, changes the **Budget** amount for the English department from $350,000.00 to $0.00.</span></span>

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="9f4ec-135">John 按之前**儲存**，Jane 執行相同的頁面和變更**Start Date**欄位從 時間 2007 年 9 月 1 日到 2013 年 8 月 8 日。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-135">Before John clicks **Save**, Jane runs the same page and changes the **Start Date** field from 9/1/2007 to 8/8/2013.</span></span>

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="9f4ec-137">John 按**儲存**第一個和所看到他的變更，回到 [索引] 頁面，然後 Jane 的瀏覽器時按下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-137">John clicks **Save** first and sees his change when the browser returns to the Index page, then Jane clicks **Save**.</span></span> <span data-ttu-id="9f4ec-138">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-138">What happens next is determined by how you handle concurrency conflicts.</span></span> <span data-ttu-id="9f4ec-139">一部分選項包括下列項目：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-139">Some of the options include the following:</span></span>

- <span data-ttu-id="9f4ec-140">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-140">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span> <span data-ttu-id="9f4ec-141">在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-141">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="9f4ec-142">在下一次有人瀏覽英文部門時，就會看到 Jane 和 John 所作出的變更，一開始的時間 2013 年 8 月 8 日的日期及預算為美金 0 元。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-142">The next time someone browses the English department, they'll see both John's and Jane's changes — a start date of 8/8/2013 and a budget of Zero dollars.</span></span>

    <span data-ttu-id="9f4ec-143">這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-143">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="9f4ec-144">Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-144">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="9f4ec-145">通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-145">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="9f4ec-146">維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-146">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>
- <span data-ttu-id="9f4ec-147">您可以讓 Jane 的變更覆寫 John 所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-147">You can let Jane's change overwrite John's change.</span></span> <span data-ttu-id="9f4ec-148">在下一次有人瀏覽英文部門時，就會看到時間 2013 年 8 月 8 日和還原的美金 $350,000.00 元調整值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-148">The next time someone browses the English department, they'll see 8/8/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="9f4ec-149">這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-149">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="9f4ec-150">(所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-150">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>
- <span data-ttu-id="9f4ec-151">您可以防止資料庫中的更新 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-151">You can prevent Jane's change from being updated in the database.</span></span> <span data-ttu-id="9f4ec-152">一般而言，您會顯示錯誤訊息、 她的目前狀態的資料，並允許她在她仍然想要讓它們重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-152">Typically, you would display an error message, show her the current state of the data, and allow her to reapply her changes if she still wants to make them.</span></span> <span data-ttu-id="9f4ec-153">這稱之為「存放區獲勝 (Store Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-153">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="9f4ec-154">(資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-154">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="9f4ec-155">這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-155">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="9f4ec-156">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="9f4ec-156">Detecting Concurrency Conflicts</span></span>

<span data-ttu-id="9f4ec-157">您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-157">You can resolve conflicts by handling [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="9f4ec-158">若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-158">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="9f4ec-159">因此，您必須適當的設定資料庫及資料模型。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-159">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="9f4ec-160">一部分啟用衝突偵測的選項包括下列選項：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-160">Some options for enabling conflict detection include the following:</span></span>

- <span data-ttu-id="9f4ec-161">在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-161">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="9f4ec-162">接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-162">You can then configure the Entity Framework to include that column in the `Where` clause of SQL `Update` or `Delete` commands.</span></span>

    <span data-ttu-id="9f4ec-163">追蹤資料行的資料類型是通常[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-163">The data type of the tracking column is typically [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx).</span></span> <span data-ttu-id="9f4ec-164">[Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是一個循序號碼，都會遞增每個資料列更新的時間。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-164">The [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="9f4ec-165">在 `Update`或是`Delete`命令，`Where`子句會包含追蹤資料行 （原始的資料列版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-165">In an `Update` or `Delete` command, the `Where` clause includes the original value of the tracking column (the original row version).</span></span> <span data-ttu-id="9f4ec-166">如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此`Update`或是`Delete`陳述式找不到的資料列，因為更新`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-166">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the `Update` or `Delete` statement can't find the row to update because of the `Where` clause.</span></span> <span data-ttu-id="9f4ec-167">當 Entity Framework 會尋找已藉由更新任何資料列`Update`或`Delete`命令 （也就是當受影響的資料列數目為零），它會解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-167">When the Entity Framework finds that no rows have been updated by the `Update` or `Delete` command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>
- <span data-ttu-id="9f4ec-168">設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-168">Configure the Entity Framework to include the original values of every column in the table in the `Where` clause of `Update` and `Delete` commands.</span></span>

    <span data-ttu-id="9f4ec-169">第一個選項，如果資料列中的任何項目已變更因為這是第一個讀取的資料列，如同`Where`子句不會可解譯為並行衝突的 Entity Framework 傳回資料列來更新。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-169">As in the first option, if anything in the row has changed since the row was first read, the `Where` clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="9f4ec-170">針對資料庫有許多資料行的資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維持大量的狀態。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-170">For database tables that have many columns, this approach can result in very large `Where` clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="9f4ec-171">如前文所述，維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-171">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="9f4ec-172">因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-172">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

    <span data-ttu-id="9f4ec-173">如果您想要實作這種並行存取的方法，您必須將標記您想要追蹤的並行存取，藉由新增實體中的所有非-主索引鍵屬性[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-173">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribute to them.</span></span> <span data-ttu-id="9f4ec-174">變更可讓 Entity Framework，包含 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-174">That change enables the Entity Framework to include all columns in the SQL `WHERE` clause of `UPDATE` statements.</span></span>

<span data-ttu-id="9f4ec-175">在本教學課程的其餘部分您可以加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體，建立控制器和檢視，並測試以確認一切都運作正常。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-175">In the remainder of this tutorial you'll add a [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking property to the `Department` entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a><span data-ttu-id="9f4ec-176">新增到 Department 實體的開放式並行存取屬性</span><span class="sxs-lookup"><span data-stu-id="9f4ec-176">Add an Optimistic Concurrency Property to the Department Entity</span></span>

<span data-ttu-id="9f4ec-177">在  *Models\Department.cs*，新增一個名為的追蹤屬性`RowVersion`:</span><span class="sxs-lookup"><span data-stu-id="9f4ec-177">In *Models\Department.cs*, add a tracking property named `RowVersion`:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

<span data-ttu-id="9f4ec-178">[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定這個資料行都將包含在`Where`子句`Update`和`Delete`命令傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-178">The [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribute specifies that this column will be included in the `Where` clause of `Update` and `Delete` commands sent to the database.</span></span> <span data-ttu-id="9f4ec-179">該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 在以 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)之前使用了 SQL 資料型別[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)加以取代。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-179">The attribute is called [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) because previous versions of SQL Server used a SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) data type before the SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) replaced it.</span></span> <span data-ttu-id="9f4ec-180">.Net 類型[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)是一個位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-180">The .Net type for [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) is a byte array.</span></span>

<span data-ttu-id="9f4ec-181">如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定追蹤屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-181">If you prefer to use the fluent API, you can use the [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) method to specify the tracking property, as shown in the following example:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="9f4ec-182">由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-182">By adding a property you changed the database model, so you need to do another migration.</span></span> <span data-ttu-id="9f4ec-183">請在套件管理員主控台 (PMC) 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-183">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a><span data-ttu-id="9f4ec-184">修改 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="9f4ec-184">Modify the Department Controller</span></span>

<span data-ttu-id="9f4ec-185">在  *Controllers\DepartmentController.cs*，新增`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-185">In *Controllers\DepartmentController.cs*, add a `using` statement:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="9f4ec-186">在  *DepartmentController.cs*檔案中，所有的四個出現的"LastName"變更為"FullName"，使部門系統管理員下拉式清單將包含講師的完整名稱，而非只有姓氏。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-186">In the *DepartmentController.cs* file, change all four occurrences of "LastName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

<span data-ttu-id="9f4ec-187">取代現有的程式碼，如`HttpPost``Edit`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-187">Replace the existing code for the `HttpPost` `Edit` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="9f4ec-188">若 `FindAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-188">If the `FindAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="9f4ec-189">顯示的程式碼會使用已張貼的表單值建立部門實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-189">The code shown uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="9f4ec-190">或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-190">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="9f4ec-191">檢視儲存的原始`RowVersion`隱藏的欄位和方法中的值會接收在`rowVersion`參數。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-191">The view stores the original `RowVersion` value in a hidden field, and the method receives it in the `rowVersion` parameter.</span></span> <span data-ttu-id="9f4ec-192">在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-192">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span> <span data-ttu-id="9f4ec-193">然後當 Entity Framework 建立 SQL 時，才`UPDATE`命令，命令便會包含`WHERE`尋找擁有原始的資料列的子句`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-193">Then when the Entity Framework creates a SQL `UPDATE` command, that command will include a `WHERE` clause that looks for a row that has the original `RowVersion` value.</span></span>

<span data-ttu-id="9f4ec-194">如果沒有任何資料列會受到`UPDATE`命令 (沒有任何資料列具有原始`RowVersion`值)，Entity Framework 擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得受影響`Department`例外狀況中的實體物件。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-194">If no rows are affected by the `UPDATE` command (no rows have the original `RowVersion` value), the Entity Framework throws a `DbUpdateConcurrencyException` exception, and the code in the `catch` block gets the affected `Department` entity from the exception object.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="9f4ec-195">此物件具有新的值，在使用者輸入其`Entity`屬性，而且可以取得的值從資料庫讀取藉由呼叫`GetDatabaseValues`方法。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-195">This object has the new values entered by the user in its `Entity` property, and you can get the values read from the database by calling the `GetDatabaseValues` method.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="9f4ec-196">`GetDatabaseValues`方法會傳回 null，如果有人已從資料庫刪除資料列; 否則您必須將傳回的物件轉型`Department`類別，才能存取`Department`屬性。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-196">The `GetDatabaseValues` method returns null if someone has deleted the row from the database; otherwise, you have to cast the returned object to the `Department` class in order to access the `Department` properties.</span></span> <span data-ttu-id="9f4ec-197">(您已核取為刪除，因為`databaseEntry`會是 null 之後已刪除部門時，才`FindAsync`執行之前`SaveChanges`執行。)</span><span class="sxs-lookup"><span data-stu-id="9f4ec-197">(Because you already checked for deletion, `databaseEntry` would be null only if the department was deleted after `FindAsync` executes and before `SaveChanges` executes.)</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="9f4ec-198">接下來，程式碼會加入每個資料行具有資料庫的值不同於在 [編輯] 頁面上輸入的使用者自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-198">Next, the code adds a custom error message for each column that has database values different from what the user entered on the Edit page:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="9f4ec-199">較長的錯誤訊息，說明發生了什麼事，以及如何處理它：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-199">A longer error message explains what happened and what to do about it:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="9f4ec-200">最後，程式碼會設定`RowVersion`的值`Department`從資料庫擷取物件的新值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-200">Finally, the code sets the `RowVersion` value of the `Department` object to the new value retrieved from the database.</span></span> <span data-ttu-id="9f4ec-201">這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-201">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="9f4ec-202">在  *Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值的隱藏的欄位的正後方`DepartmentID`屬性：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-202">In *Views\Department\Edit.cshtml*, add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a><span data-ttu-id="9f4ec-203">測試開放式並行存取處理</span><span class="sxs-lookup"><span data-stu-id="9f4ec-203">Testing Optimistic Concurrency Handling</span></span>

<span data-ttu-id="9f4ec-204">執行站台，然後按一下**部門**:</span><span class="sxs-lookup"><span data-stu-id="9f4ec-204">Run the site and click **Departments**:</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="9f4ec-206">以滑鼠右鍵按一下**編輯**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-206">Right click the **Edit** hyperlink for the English department and select **Open in new tab,** then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="9f4ec-207">兩個索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-207">The two tabs display the same information.</span></span>

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="9f4ec-209">變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-209">Change a field in the first browser tab and click **Save**.</span></span>

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="9f4ec-211">瀏覽器會顯示索引頁面，當中包含了變更之後的值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-211">The browser shows the Index page with the changed value.</span></span>

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="9f4ec-213">變更第二個瀏覽器索引標籤中的欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-213">Change a field in the second browser tab and click **Save**.</span></span>

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="9f4ec-215">按一下 **儲存**第二個瀏覽器索引標籤中。您會看到一個錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-215">Click **Save** in the second browser tab. You see an error message:</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="9f4ec-217">再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-217">Click **Save** again.</span></span> <span data-ttu-id="9f4ec-218">您在第二個瀏覽器索引標籤中輸入的值會儲存與您在第一個瀏覽器中變更資料的原始值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-218">The value you entered in the second browser tab is saved along with the original value of the data you changed in the first browser.</span></span> <span data-ttu-id="9f4ec-219">您會在索引頁面出現時看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-219">You see the saved values when the Index page appears.</span></span>

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a><span data-ttu-id="9f4ec-221">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="9f4ec-221">Updating the Delete Page</span></span>

<span data-ttu-id="9f4ec-222">針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-222">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="9f4ec-223">當`HttpGet``Delete`方法會顯示確認檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-223">When the `HttpGet` `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="9f4ec-224">值，便可`HttpPost``Delete`在使用者確認刪除時呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-224">That value is then available to the `HttpPost` `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="9f4ec-225">當 Entity Framework 建立 SQL`DELETE`命令，其中包含`WHERE`子句，而其原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-225">When the Entity Framework creates the SQL `DELETE` command, it includes a `WHERE` clause with the original `RowVersion` value.</span></span> <span data-ttu-id="9f4ec-226">如果命令會導致零個資料列受影響 （亦即已顯示 [刪除] 確認頁面之後，已變更資料列），則會擲回並行例外狀況，而`HttpGet Delete`方法呼叫錯誤旗標設為`true`才能重新顯示確認頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-226">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the `HttpGet Delete` method is called with an error flag set to `true` in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="9f4ec-227">此外，也可以零個資料列已受到影響，因為另一位使用者，已刪除資料列，因此在此情況下會顯示不同的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-227">It's also possible that zero rows were affected because the row was deleted by another user, so in that case a different error message is displayed.</span></span>

<span data-ttu-id="9f4ec-228">在  *DepartmentController.cs*，取代`HttpGet``Delete`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-228">In *DepartmentController.cs*, replace the `HttpGet` `Delete` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="9f4ec-229">方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-229">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="9f4ec-230">如果這個旗標`true`，將錯誤訊息傳送以檢視使用`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-230">If this flag is `true`, an error message is sent to the view using a `ViewBag` property.</span></span>

<span data-ttu-id="9f4ec-231">中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-231">Replace the code in the `HttpPost` `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="9f4ec-232">在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-232">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="9f4ec-233">您已變更此參數，以`Department`模型繫結所建立的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-233">You've changed this parameter to a `Department` entity instance created by the model binder.</span></span> <span data-ttu-id="9f4ec-234">這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-234">This gives you access to the `RowVersion` property value in addition to the record key.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="9f4ec-235">您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-235">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="9f4ec-236">名為的 scaffold 程式碼`HttpPost``Delete`方法`DeleteConfirmed`給`HttpPost`方法唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-236">The scaffolded code named the `HttpPost` `Delete` method `DeleteConfirmed` to give the `HttpPost` method a unique signature.</span></span> <span data-ttu-id="9f4ec-237">（CLR 要求多載的方法，以不同的方法參數）。既然簽章是唯一的您可以繼續使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`delete 方法。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-237">( The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the `HttpPost` and `HttpGet` delete methods.</span></span>

<span data-ttu-id="9f4ec-238">若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-238">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

<span data-ttu-id="9f4ec-239">在  *Views\Department\Delete.cshtml*，將錯誤訊息欄位和 DepartmentID 及 RowVersion 屬性隱藏的欄位的下列程式碼取代 scaffold 程式碼。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-239">In *Views\Department\Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="9f4ec-240">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-240">The changes are highlighted.</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

<span data-ttu-id="9f4ec-241">此程式碼會新增一則錯誤訊息之間`h2`和`h3`標題：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-241">This code adds an error message between the `h2` and `h3` headings:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="9f4ec-242">它會取代`LastName`具有`FullName`在`Administrator`欄位：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-242">It replaces `LastName` with `FullName` in the `Administrator` field:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="9f4ec-243">最後，它會新增隱藏的欄位`DepartmentID`並`RowVersion`屬性之後`Html.BeginForm`陳述式：</span><span class="sxs-lookup"><span data-stu-id="9f4ec-243">Finally, it adds hidden fields for the `DepartmentID` and `RowVersion` properties after the `Html.BeginForm` statement:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

<span data-ttu-id="9f4ec-244">執行 Departments 索引 頁面。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-244">Run the Departments Index page.</span></span> <span data-ttu-id="9f4ec-245">以滑鼠右鍵按一下**刪除**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後在第一個索引標籤中按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-245">Right click the **Delete** hyperlink for the English department and select **Open in new tab,** then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="9f4ec-246">在第一個視窗中，請變更其中一個值，然後按一下**儲存**:</span><span class="sxs-lookup"><span data-stu-id="9f4ec-246">In the first window, change one of the values, and click **Save** :</span></span>

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<span data-ttu-id="9f4ec-248">[索引] 頁面會確認變更。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-248">The Index page confirms the change.</span></span>

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

<span data-ttu-id="9f4ec-250">在第二個索引標籤中，按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-250">In the second tab, click **Delete**.</span></span>

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

<span data-ttu-id="9f4ec-252">您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-252">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="9f4ec-254">若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-254">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="9f4ec-255">總結</span><span class="sxs-lookup"><span data-stu-id="9f4ec-255">Summary</span></span>

<span data-ttu-id="9f4ec-256">如此即完成了處理並行衝突的簡介。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-256">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="9f4ec-257">其他方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://msdn.microsoft.com/data/jj592904)並[屬性值使用](https://msdn.microsoft.com/data/jj592677)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-257">For information about other ways to handle various concurrency scenarios, see [Optimistic Concurrency Patterns](https://msdn.microsoft.com/data/jj592904) and [Working with Property Values](https://msdn.microsoft.com/data/jj592677) on MSDN.</span></span> <span data-ttu-id="9f4ec-258">下一個教學課程示範如何實作每個階層的資料表繼承`Instructor`和`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-258">The next tutorial shows how to implement table-per-hierarchy inheritance for the `Instructor` and `Student` entities.</span></span>

<span data-ttu-id="9f4ec-259">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="9f4ec-259">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9f4ec-260">[上一頁](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="9f4ec-260">[Previous](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
