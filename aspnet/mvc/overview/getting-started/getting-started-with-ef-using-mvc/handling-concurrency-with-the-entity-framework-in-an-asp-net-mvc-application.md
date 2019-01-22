---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：ASP.NET MVC 5 應用程式中處理 ef 的並行存取
description: 本教學課程會示範如何使用以多位使用者同時更新相同的實體時處理衝突的開放式並行存取。
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b77b8d6f952472f4d3030f54665f970b8ace2caf
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444177"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a><span data-ttu-id="4de71-103">教學課程：ASP.NET MVC 5 應用程式中處理 ef 的並行存取</span><span class="sxs-lookup"><span data-stu-id="4de71-103">Tutorial: Handle Concurrency with EF in an ASP.NET MVC 5 app</span></span>

<span data-ttu-id="4de71-104">在先前的教學課程中，您學會如何更新資料。</span><span class="sxs-lookup"><span data-stu-id="4de71-104">In earlier tutorials you learned how to update data.</span></span> <span data-ttu-id="4de71-105">本教學課程會示範如何使用以多位使用者同時更新相同的實體時處理衝突的開放式並行存取。</span><span class="sxs-lookup"><span data-stu-id="4de71-105">This tutorial shows how to use optimistic concurrency to handle conflicts when multiple users update the same entity at the same time.</span></span> <span data-ttu-id="4de71-106">變更網頁使用`Department`實體，使它們處理並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="4de71-106">You change the web pages that work with the `Department` entity so that they handle concurrency errors.</span></span> <span data-ttu-id="4de71-107">下列圖例顯示了 [編輯] 和 [刪除] 頁面，包括一些發生並行衝突時會顯示的訊息。</span><span class="sxs-lookup"><span data-stu-id="4de71-107">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="4de71-110">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="4de71-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4de71-111">深入了解並行衝突</span><span class="sxs-lookup"><span data-stu-id="4de71-111">Learn about concurrency conflicts</span></span>
> * <span data-ttu-id="4de71-112">新增開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="4de71-112">Add optimistic concurrency</span></span>
> * <span data-ttu-id="4de71-113">修改 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="4de71-113">Modify Department controller</span></span>
> * <span data-ttu-id="4de71-114">測試並行處理</span><span class="sxs-lookup"><span data-stu-id="4de71-114">Test concurrency handling</span></span>
> * <span data-ttu-id="4de71-115">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="4de71-115">Update the Delete page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4de71-116">必要條件</span><span class="sxs-lookup"><span data-stu-id="4de71-116">Prerequisites</span></span>

* [<span data-ttu-id="4de71-117">非同步的預存程序</span><span class="sxs-lookup"><span data-stu-id="4de71-117">Async and Stored Procedures</span></span>](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a><span data-ttu-id="4de71-118">並行衝突</span><span class="sxs-lookup"><span data-stu-id="4de71-118">Concurrency conflicts</span></span>

<span data-ttu-id="4de71-119">當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="4de71-119">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="4de71-120">若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="4de71-120">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="4de71-121">在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。</span><span class="sxs-lookup"><span data-stu-id="4de71-121">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="4de71-122">在此情況下，您便不需要設定應用程式來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="4de71-122">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="4de71-123">封閉式並行存取 （鎖定）</span><span class="sxs-lookup"><span data-stu-id="4de71-123">Pessimistic Concurrency (Locking)</span></span>

<span data-ttu-id="4de71-124">若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="4de71-124">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="4de71-125">這就叫做*封閉式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="4de71-125">This is called *pessimistic concurrency*.</span></span> <span data-ttu-id="4de71-126">例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。</span><span class="sxs-lookup"><span data-stu-id="4de71-126">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="4de71-127">若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。</span><span class="sxs-lookup"><span data-stu-id="4de71-127">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="4de71-128">若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。</span><span class="sxs-lookup"><span data-stu-id="4de71-128">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="4de71-129">管理鎖定有幾個缺點。</span><span class="sxs-lookup"><span data-stu-id="4de71-129">Managing locks has disadvantages.</span></span> <span data-ttu-id="4de71-130">其程式可能相當複雜。</span><span class="sxs-lookup"><span data-stu-id="4de71-130">It can be complex to program.</span></span> <span data-ttu-id="4de71-131">這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。</span><span class="sxs-lookup"><span data-stu-id="4de71-131">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="4de71-132">基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="4de71-132">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="4de71-133">Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。</span><span class="sxs-lookup"><span data-stu-id="4de71-133">The Entity Framework provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="4de71-134">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="4de71-134">Optimistic Concurrency</span></span>

<span data-ttu-id="4de71-135">封閉式並行存取的替代方案是*開放式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="4de71-135">The alternative to pessimistic concurrency is *optimistic concurrency*.</span></span> <span data-ttu-id="4de71-136">開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。</span><span class="sxs-lookup"><span data-stu-id="4de71-136">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="4de71-137">例如，John 在部門編輯頁面上，將英文部門的**預算**從$350,000.00 變更為$0.00。</span><span class="sxs-lookup"><span data-stu-id="4de71-137">For example, John runs the Departments Edit page, changes the **Budget** amount for the English department from $350,000.00 to $0.00.</span></span>

<span data-ttu-id="4de71-138">John 按之前**儲存**，Jane 執行相同的頁面和變更**Start Date**欄位從 時間 2007 年 9 月 1 日到 2013 年 8 月 8 日。</span><span class="sxs-lookup"><span data-stu-id="4de71-138">Before John clicks **Save**, Jane runs the same page and changes the **Start Date** field from 9/1/2007 to 8/8/2013.</span></span>

<span data-ttu-id="4de71-139">John 按**儲存**第一個和所看到他的變更，回到 [索引] 頁面，然後 Jane 的瀏覽器時按下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="4de71-139">John clicks **Save** first and sees his change when the browser returns to the Index page, then Jane clicks **Save**.</span></span> <span data-ttu-id="4de71-140">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="4de71-140">What happens next is determined by how you handle concurrency conflicts.</span></span> <span data-ttu-id="4de71-141">一部分選項包括下列項目：</span><span class="sxs-lookup"><span data-stu-id="4de71-141">Some of the options include the following:</span></span>

- <span data-ttu-id="4de71-142">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="4de71-142">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span> <span data-ttu-id="4de71-143">在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。</span><span class="sxs-lookup"><span data-stu-id="4de71-143">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="4de71-144">在下一次有人瀏覽英文部門時，就會看到 Jane 和 John 所作出的變更，一開始的時間 2013 年 8 月 8 日的日期及預算為美金 0 元。</span><span class="sxs-lookup"><span data-stu-id="4de71-144">The next time someone browses the English department, they'll see both John's and Jane's changes — a start date of 8/8/2013 and a budget of Zero dollars.</span></span>

    <span data-ttu-id="4de71-145">這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="4de71-145">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="4de71-146">Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="4de71-146">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="4de71-147">通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。</span><span class="sxs-lookup"><span data-stu-id="4de71-147">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="4de71-148">維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。</span><span class="sxs-lookup"><span data-stu-id="4de71-148">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>
- <span data-ttu-id="4de71-149">您可以讓 Jane 的變更覆寫 John 所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="4de71-149">You can let Jane's change overwrite John's change.</span></span> <span data-ttu-id="4de71-150">在下一次有人瀏覽英文部門時，就會看到時間 2013 年 8 月 8 日和還原的美金 $350,000.00 元調整值。</span><span class="sxs-lookup"><span data-stu-id="4de71-150">The next time someone browses the English department, they'll see 8/8/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="4de71-151">這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="4de71-151">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="4de71-152">(所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。</span><span class="sxs-lookup"><span data-stu-id="4de71-152">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>
- <span data-ttu-id="4de71-153">您可以防止資料庫中的更新 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="4de71-153">You can prevent Jane's change from being updated in the database.</span></span> <span data-ttu-id="4de71-154">一般而言，您會顯示錯誤訊息、 她的目前狀態的資料，並允許她在她仍然想要讓它們重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="4de71-154">Typically, you would display an error message, show her the current state of the data, and allow her to reapply her changes if she still wants to make them.</span></span> <span data-ttu-id="4de71-155">這稱之為「存放區獲勝 (Store Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="4de71-155">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="4de71-156">(資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="4de71-156">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="4de71-157">這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="4de71-157">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="4de71-158">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="4de71-158">Detecting Concurrency Conflicts</span></span>

<span data-ttu-id="4de71-159">您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4de71-159">You can resolve conflicts by handling [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="4de71-160">若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="4de71-160">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="4de71-161">因此，您必須適當的設定資料庫及資料模型。</span><span class="sxs-lookup"><span data-stu-id="4de71-161">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="4de71-162">一部分啟用衝突偵測的選項包括下列選項：</span><span class="sxs-lookup"><span data-stu-id="4de71-162">Some options for enabling conflict detection include the following:</span></span>

- <span data-ttu-id="4de71-163">在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。</span><span class="sxs-lookup"><span data-stu-id="4de71-163">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="4de71-164">接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="4de71-164">You can then configure the Entity Framework to include that column in the `Where` clause of SQL `Update` or `Delete` commands.</span></span>

    <span data-ttu-id="4de71-165">追蹤資料行的資料類型是通常[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="4de71-165">The data type of the tracking column is typically [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx).</span></span> <span data-ttu-id="4de71-166">[Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是一個循序號碼，都會遞增每個資料列更新的時間。</span><span class="sxs-lookup"><span data-stu-id="4de71-166">The [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="4de71-167">在 `Update`或是`Delete`命令，`Where`子句會包含追蹤資料行 （原始的資料列版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="4de71-167">In an `Update` or `Delete` command, the `Where` clause includes the original value of the tracking column (the original row version).</span></span> <span data-ttu-id="4de71-168">如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此`Update`或是`Delete`陳述式找不到的資料列，因為更新`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="4de71-168">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the `Update` or `Delete` statement can't find the row to update because of the `Where` clause.</span></span> <span data-ttu-id="4de71-169">當 Entity Framework 會尋找已藉由更新任何資料列`Update`或`Delete`命令 （也就是當受影響的資料列數目為零），它會解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="4de71-169">When the Entity Framework finds that no rows have been updated by the `Update` or `Delete` command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>
- <span data-ttu-id="4de71-170">設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="4de71-170">Configure the Entity Framework to include the original values of every column in the table in the `Where` clause of `Update` and `Delete` commands.</span></span>

    <span data-ttu-id="4de71-171">第一個選項，如果資料列中的任何項目已變更因為這是第一個讀取的資料列，如同`Where`子句不會可解譯為並行衝突的 Entity Framework 傳回資料列來更新。</span><span class="sxs-lookup"><span data-stu-id="4de71-171">As in the first option, if anything in the row has changed since the row was first read, the `Where` clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="4de71-172">針對資料庫有許多資料行的資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維持大量的狀態。</span><span class="sxs-lookup"><span data-stu-id="4de71-172">For database tables that have many columns, this approach can result in very large `Where` clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="4de71-173">如前文所述，維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="4de71-173">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="4de71-174">因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="4de71-174">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

    <span data-ttu-id="4de71-175">如果您想要實作這種並行存取的方法，您必須將標記您想要追蹤的並行存取，藉由新增實體中的所有非-主索引鍵屬性[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="4de71-175">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribute to them.</span></span> <span data-ttu-id="4de71-176">變更可讓 Entity Framework，包含 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="4de71-176">That change enables the Entity Framework to include all columns in the SQL `WHERE` clause of `UPDATE` statements.</span></span>

<span data-ttu-id="4de71-177">在本教學課程的其餘部分您可以加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體，建立控制器和檢視，並測試以確認一切都運作正常。</span><span class="sxs-lookup"><span data-stu-id="4de71-177">In the remainder of this tutorial you'll add a [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking property to the `Department` entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-optimistic-concurrency"></a><span data-ttu-id="4de71-178">新增開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="4de71-178">Add optimistic concurrency</span></span>

<span data-ttu-id="4de71-179">在  *Models\Department.cs*，新增一個名為的追蹤屬性`RowVersion`:</span><span class="sxs-lookup"><span data-stu-id="4de71-179">In *Models\Department.cs*, add a tracking property named `RowVersion`:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

<span data-ttu-id="4de71-180">[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定這個資料行都將包含在`Where`子句`Update`和`Delete`命令傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="4de71-180">The [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribute specifies that this column will be included in the `Where` clause of `Update` and `Delete` commands sent to the database.</span></span> <span data-ttu-id="4de71-181">該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 在以 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)之前使用了 SQL 資料型別[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)加以取代。</span><span class="sxs-lookup"><span data-stu-id="4de71-181">The attribute is called [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) because previous versions of SQL Server used a SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) data type before the SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) replaced it.</span></span> <span data-ttu-id="4de71-182">.Net 類型[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)是一個位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="4de71-182">The .Net type for [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) is a byte array.</span></span>

<span data-ttu-id="4de71-183">如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定追蹤屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4de71-183">If you prefer to use the fluent API, you can use the [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) method to specify the tracking property, as shown in the following example:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="4de71-184">由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。</span><span class="sxs-lookup"><span data-stu-id="4de71-184">By adding a property you changed the database model, so you need to do another migration.</span></span> <span data-ttu-id="4de71-185">請在套件管理員主控台 (PMC) 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4de71-185">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a><span data-ttu-id="4de71-186">修改 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="4de71-186">Modify Department controller</span></span>

<span data-ttu-id="4de71-187">在  *Controllers\DepartmentController.cs*，新增`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="4de71-187">In *Controllers\DepartmentController.cs*, add a `using` statement:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="4de71-188">在  *DepartmentController.cs*檔案中，所有的四個出現的"LastName"變更為"FullName"，使部門系統管理員下拉式清單將包含講師的完整名稱，而非只有姓氏。</span><span class="sxs-lookup"><span data-stu-id="4de71-188">In the *DepartmentController.cs* file, change all four occurrences of "LastName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

<span data-ttu-id="4de71-189">取代現有的程式碼，如`HttpPost``Edit`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="4de71-189">Replace the existing code for the `HttpPost` `Edit` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="4de71-190">若 `FindAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。</span><span class="sxs-lookup"><span data-stu-id="4de71-190">If the `FindAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="4de71-191">顯示的程式碼會使用已張貼的表單值建立部門實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4de71-191">The code shown uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="4de71-192">或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。</span><span class="sxs-lookup"><span data-stu-id="4de71-192">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="4de71-193">檢視儲存的原始`RowVersion`隱藏的欄位和方法中的值會接收在`rowVersion`參數。</span><span class="sxs-lookup"><span data-stu-id="4de71-193">The view stores the original `RowVersion` value in a hidden field, and the method receives it in the `rowVersion` parameter.</span></span> <span data-ttu-id="4de71-194">在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。</span><span class="sxs-lookup"><span data-stu-id="4de71-194">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span> <span data-ttu-id="4de71-195">然後當 Entity Framework 建立 SQL 時，才`UPDATE`命令，命令便會包含`WHERE`尋找擁有原始的資料列的子句`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="4de71-195">Then when the Entity Framework creates a SQL `UPDATE` command, that command will include a `WHERE` clause that looks for a row that has the original `RowVersion` value.</span></span>

<span data-ttu-id="4de71-196">如果沒有任何資料列會受到`UPDATE`命令 (沒有任何資料列具有原始`RowVersion`值)，Entity Framework 擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得受影響`Department`例外狀況中的實體物件。</span><span class="sxs-lookup"><span data-stu-id="4de71-196">If no rows are affected by the `UPDATE` command (no rows have the original `RowVersion` value), the Entity Framework throws a `DbUpdateConcurrencyException` exception, and the code in the `catch` block gets the affected `Department` entity from the exception object.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="4de71-197">此物件具有新的值，在使用者輸入其`Entity`屬性，而且可以取得的值從資料庫讀取藉由呼叫`GetDatabaseValues`方法。</span><span class="sxs-lookup"><span data-stu-id="4de71-197">This object has the new values entered by the user in its `Entity` property, and you can get the values read from the database by calling the `GetDatabaseValues` method.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="4de71-198">`GetDatabaseValues`方法會傳回 null，如果有人已從資料庫刪除資料列; 否則您必須將傳回的物件轉型`Department`類別，才能存取`Department`屬性。</span><span class="sxs-lookup"><span data-stu-id="4de71-198">The `GetDatabaseValues` method returns null if someone has deleted the row from the database; otherwise, you have to cast the returned object to the `Department` class in order to access the `Department` properties.</span></span> <span data-ttu-id="4de71-199">(您已核取為刪除，因為`databaseEntry`會是 null 之後已刪除部門時，才`FindAsync`執行之前`SaveChanges`執行。)</span><span class="sxs-lookup"><span data-stu-id="4de71-199">(Because you already checked for deletion, `databaseEntry` would be null only if the department was deleted after `FindAsync` executes and before `SaveChanges` executes.)</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="4de71-200">接下來，程式碼會加入每個資料行具有資料庫的值不同於在 [編輯] 頁面上輸入的使用者自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="4de71-200">Next, the code adds a custom error message for each column that has database values different from what the user entered on the Edit page:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="4de71-201">較長的錯誤訊息，說明發生了什麼事，以及如何處理它：</span><span class="sxs-lookup"><span data-stu-id="4de71-201">A longer error message explains what happened and what to do about it:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="4de71-202">最後，程式碼會設定`RowVersion`的值`Department`從資料庫擷取物件的新值。</span><span class="sxs-lookup"><span data-stu-id="4de71-202">Finally, the code sets the `RowVersion` value of the `Department` object to the new value retrieved from the database.</span></span> <span data-ttu-id="4de71-203">這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="4de71-203">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="4de71-204">在  *Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值的隱藏的欄位的正後方`DepartmentID`屬性：</span><span class="sxs-lookup"><span data-stu-id="4de71-204">In *Views\Department\Edit.cshtml*, add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a><span data-ttu-id="4de71-205">測試並行處理</span><span class="sxs-lookup"><span data-stu-id="4de71-205">Test concurrency handling</span></span>

<span data-ttu-id="4de71-206">執行站台，然後按一下**部門**。</span><span class="sxs-lookup"><span data-stu-id="4de71-206">Run the site and click **Departments**.</span></span>

<span data-ttu-id="4de71-207">以滑鼠右鍵按一下**編輯**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="4de71-207">Right click the **Edit** hyperlink for the English department and select **Open in new tab,** then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="4de71-208">兩個索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="4de71-208">The two tabs display the same information.</span></span>

<span data-ttu-id="4de71-209">變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="4de71-209">Change a field in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="4de71-210">瀏覽器會顯示索引頁面，當中包含了變更之後的值。</span><span class="sxs-lookup"><span data-stu-id="4de71-210">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="4de71-211">變更第二個瀏覽器索引標籤中的欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="4de71-211">Change a field in the second browser tab and click **Save**.</span></span> <span data-ttu-id="4de71-212">您會看到一個錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="4de71-212">You see an error message:</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="4de71-214">再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="4de71-214">Click **Save** again.</span></span> <span data-ttu-id="4de71-215">您在第二個瀏覽器索引標籤中輸入的值會儲存與您在第一個瀏覽器中變更資料的原始值。</span><span class="sxs-lookup"><span data-stu-id="4de71-215">The value you entered in the second browser tab is saved along with the original value of the data you changed in the first browser.</span></span> <span data-ttu-id="4de71-216">您會在索引頁面出現時看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="4de71-216">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="4de71-217">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="4de71-217">Update the Delete page</span></span>

<span data-ttu-id="4de71-218">針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。</span><span class="sxs-lookup"><span data-stu-id="4de71-218">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="4de71-219">當`HttpGet``Delete`方法會顯示確認檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="4de71-219">When the `HttpGet` `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="4de71-220">值，便可`HttpPost``Delete`在使用者確認刪除時呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="4de71-220">That value is then available to the `HttpPost` `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="4de71-221">當 Entity Framework 建立 SQL`DELETE`命令，其中包含`WHERE`子句，而其原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="4de71-221">When the Entity Framework creates the SQL `DELETE` command, it includes a `WHERE` clause with the original `RowVersion` value.</span></span> <span data-ttu-id="4de71-222">如果命令會導致零個資料列受影響 （亦即已顯示 [刪除] 確認頁面之後，已變更資料列），則會擲回並行例外狀況，而`HttpGet Delete`方法呼叫錯誤旗標設為`true`才能重新顯示確認頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4de71-222">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the `HttpGet Delete` method is called with an error flag set to `true` in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="4de71-223">此外，也可以零個資料列已受到影響，因為另一位使用者，已刪除資料列，因此在此情況下會顯示不同的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4de71-223">It's also possible that zero rows were affected because the row was deleted by another user, so in that case a different error message is displayed.</span></span>

<span data-ttu-id="4de71-224">在  *DepartmentController.cs*，取代`HttpGet``Delete`為下列程式碼的方法：</span><span class="sxs-lookup"><span data-stu-id="4de71-224">In *DepartmentController.cs*, replace the `HttpGet` `Delete` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="4de71-225">方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。</span><span class="sxs-lookup"><span data-stu-id="4de71-225">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="4de71-226">如果這個旗標`true`，將錯誤訊息傳送以檢視使用`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="4de71-226">If this flag is `true`, an error message is sent to the view using a `ViewBag` property.</span></span>

<span data-ttu-id="4de71-227">中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4de71-227">Replace the code in the `HttpPost` `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="4de71-228">在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="4de71-228">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="4de71-229">您已變更此參數，以`Department`模型繫結所建立的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="4de71-229">You've changed this parameter to a `Department` entity instance created by the model binder.</span></span> <span data-ttu-id="4de71-230">這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。</span><span class="sxs-lookup"><span data-stu-id="4de71-230">This gives you access to the `RowVersion` property value in addition to the record key.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="4de71-231">您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。</span><span class="sxs-lookup"><span data-stu-id="4de71-231">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="4de71-232">名為的 scaffold 程式碼`HttpPost``Delete`方法`DeleteConfirmed`給`HttpPost`方法唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="4de71-232">The scaffolded code named the `HttpPost` `Delete` method `DeleteConfirmed` to give the `HttpPost` method a unique signature.</span></span> <span data-ttu-id="4de71-233">（CLR 要求多載的方法，以不同的方法參數）。既然簽章是唯一的您可以繼續使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`delete 方法。</span><span class="sxs-lookup"><span data-stu-id="4de71-233">( The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the `HttpPost` and `HttpGet` delete methods.</span></span>

<span data-ttu-id="4de71-234">若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4de71-234">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

<span data-ttu-id="4de71-235">在  *Views\Department\Delete.cshtml*，將錯誤訊息欄位和 DepartmentID 及 RowVersion 屬性隱藏的欄位的下列程式碼取代 scaffold 程式碼。</span><span class="sxs-lookup"><span data-stu-id="4de71-235">In *Views\Department\Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="4de71-236">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="4de71-236">The changes are highlighted.</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

<span data-ttu-id="4de71-237">此程式碼會新增一則錯誤訊息之間`h2`和`h3`標題：</span><span class="sxs-lookup"><span data-stu-id="4de71-237">This code adds an error message between the `h2` and `h3` headings:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="4de71-238">它會取代`LastName`具有`FullName`在`Administrator`欄位：</span><span class="sxs-lookup"><span data-stu-id="4de71-238">It replaces `LastName` with `FullName` in the `Administrator` field:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="4de71-239">最後，它會新增隱藏的欄位`DepartmentID`並`RowVersion`屬性之後`Html.BeginForm`陳述式：</span><span class="sxs-lookup"><span data-stu-id="4de71-239">Finally, it adds hidden fields for the `DepartmentID` and `RowVersion` properties after the `Html.BeginForm` statement:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

<span data-ttu-id="4de71-240">執行 Departments 索引 頁面。</span><span class="sxs-lookup"><span data-stu-id="4de71-240">Run the Departments Index page.</span></span> <span data-ttu-id="4de71-241">以滑鼠右鍵按一下**刪除**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後在第一個索引標籤中按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="4de71-241">Right click the **Delete** hyperlink for the English department and select **Open in new tab,** then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="4de71-242">在第一個視窗中，請變更其中一個值，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="4de71-242">In the first window, change one of the values, and click **Save**.</span></span>

<span data-ttu-id="4de71-243">[索引] 頁面會確認變更。</span><span class="sxs-lookup"><span data-stu-id="4de71-243">The Index page confirms the change.</span></span>

<span data-ttu-id="4de71-244">在第二個索引標籤中，按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="4de71-244">In the second tab, click **Delete**.</span></span>

<span data-ttu-id="4de71-245">您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。</span><span class="sxs-lookup"><span data-stu-id="4de71-245">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="4de71-247">若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。</span><span class="sxs-lookup"><span data-stu-id="4de71-247">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="4de71-248">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="4de71-248">Get the code</span></span>

[<span data-ttu-id="4de71-249">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="4de71-249">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="4de71-250">其他資源</span><span class="sxs-lookup"><span data-stu-id="4de71-250">Additional resources</span></span>

<span data-ttu-id="4de71-251">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="4de71-251">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="4de71-252">其他方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://msdn.microsoft.com/data/jj592904)並[屬性值使用](https://msdn.microsoft.com/data/jj592677)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="4de71-252">For information about other ways to handle various concurrency scenarios, see [Optimistic Concurrency Patterns](https://msdn.microsoft.com/data/jj592904) and [Working with Property Values](https://msdn.microsoft.com/data/jj592677) on MSDN.</span></span> <span data-ttu-id="4de71-253">下一個教學課程示範如何實作每個階層的資料表繼承`Instructor`和`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="4de71-253">The next tutorial shows how to implement table-per-hierarchy inheritance for the `Instructor` and `Student` entities.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4de71-254">後續步驟</span><span class="sxs-lookup"><span data-stu-id="4de71-254">Next steps</span></span>

<span data-ttu-id="4de71-255">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="4de71-255">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4de71-256">了解並行衝突</span><span class="sxs-lookup"><span data-stu-id="4de71-256">Learned about concurrency conflicts</span></span>
> * <span data-ttu-id="4de71-257">已新增的開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="4de71-257">Added optimistic concurrency</span></span>
> * <span data-ttu-id="4de71-258">修改後的 Department 控制器</span><span class="sxs-lookup"><span data-stu-id="4de71-258">Modified Department controller</span></span>
> * <span data-ttu-id="4de71-259">已測試的並行處理</span><span class="sxs-lookup"><span data-stu-id="4de71-259">Tested concurrency handling</span></span>
> * <span data-ttu-id="4de71-260">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="4de71-260">Updated the Delete page</span></span>

<span data-ttu-id="4de71-261">請前往下一篇文章，以了解如何在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="4de71-261">Advance to the next article to learn how to implement inheritance in the data model.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4de71-262">在資料模型中實作繼承</span><span class="sxs-lookup"><span data-stu-id="4de71-262">Implement inheritance in the data model</span></span>](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)