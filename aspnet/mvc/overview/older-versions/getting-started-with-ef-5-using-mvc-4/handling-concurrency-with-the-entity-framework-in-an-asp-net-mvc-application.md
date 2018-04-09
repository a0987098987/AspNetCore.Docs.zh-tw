---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 處理並行與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-7) |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 609f493845f1d00a47d175a1b623a7f4866d191e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a><span data-ttu-id="0127c-103">處理並行與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-7)</span><span class="sxs-lookup"><span data-stu-id="0127c-103">Handling Concurrency with the Entity Framework in an ASP.NET MVC Application (7 of 10)</span></span>
====================
<span data-ttu-id="0127c-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0127c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0127c-105">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="0127c-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="0127c-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="0127c-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="0127c-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="0127c-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0127c-108">您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="0127c-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="0127c-109">如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。</span><span class="sxs-lookup"><span data-stu-id="0127c-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="0127c-110">您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="0127c-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="0127c-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="0127c-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="0127c-112">在先前的兩個教學課程中您工作與相關資料。</span><span class="sxs-lookup"><span data-stu-id="0127c-112">In the previous two tutorials you worked with related data.</span></span> <span data-ttu-id="0127c-113">本教學課程會示範如何處理並行存取。</span><span class="sxs-lookup"><span data-stu-id="0127c-113">This tutorial shows how to handle concurrency.</span></span> <span data-ttu-id="0127c-114">您將建立可搭配 web 網頁`Department`實體，以及編輯和刪除的頁面`Department`實體處理的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="0127c-114">You'll create web pages that work with the `Department` entity, and the pages that edit and delete `Department` entities will handle concurrency errors.</span></span> <span data-ttu-id="0127c-115">下圖顯示的索引和刪除的頁面，包括發生並行衝突時，會顯示一些訊息。</span><span class="sxs-lookup"><span data-stu-id="0127c-115">The following illustrations show the Index and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="0127c-118">並行衝突</span><span class="sxs-lookup"><span data-stu-id="0127c-118">Concurrency Conflicts</span></span>

<span data-ttu-id="0127c-119">當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="0127c-119">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="0127c-120">若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="0127c-120">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="0127c-121">在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。</span><span class="sxs-lookup"><span data-stu-id="0127c-121">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="0127c-122">在此情況下，您便不需要設定應用程式來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="0127c-122">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="0127c-123">封閉式並行存取 （鎖定）</span><span class="sxs-lookup"><span data-stu-id="0127c-123">Pessimistic Concurrency (Locking)</span></span>

<span data-ttu-id="0127c-124">若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="0127c-124">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="0127c-125">這稱為*封閉式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="0127c-125">This is called *pessimistic concurrency*.</span></span> <span data-ttu-id="0127c-126">例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。</span><span class="sxs-lookup"><span data-stu-id="0127c-126">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="0127c-127">若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。</span><span class="sxs-lookup"><span data-stu-id="0127c-127">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="0127c-128">若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。</span><span class="sxs-lookup"><span data-stu-id="0127c-128">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="0127c-129">管理鎖定有幾個缺點。</span><span class="sxs-lookup"><span data-stu-id="0127c-129">Managing locks has disadvantages.</span></span> <span data-ttu-id="0127c-130">其程式可能相當複雜。</span><span class="sxs-lookup"><span data-stu-id="0127c-130">It can be complex to program.</span></span> <span data-ttu-id="0127c-131">這需要大量的資料庫管理資源，而且它會造成效能問題的應用程式的使用者數目增加時 （亦即，將無法妥善調整）。</span><span class="sxs-lookup"><span data-stu-id="0127c-131">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases (that is, it doesn't scale well).</span></span> <span data-ttu-id="0127c-132">基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="0127c-132">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="0127c-133">Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。</span><span class="sxs-lookup"><span data-stu-id="0127c-133">The Entity Framework provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="0127c-134">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="0127c-134">Optimistic Concurrency</span></span>

<span data-ttu-id="0127c-135">封閉式並行存取另一種是*開放式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="0127c-135">The alternative to pessimistic concurrency is *optimistic concurrency*.</span></span> <span data-ttu-id="0127c-136">開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。</span><span class="sxs-lookup"><span data-stu-id="0127c-136">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="0127c-137">例如，John 會執行 部門編輯頁面上，變更**預算**$0.00 從 $350,000.00 英文部門的數量。</span><span class="sxs-lookup"><span data-stu-id="0127c-137">For example, John runs the Departments Edit page, changes the **Budget** amount for the English department from $350,000.00 to $0.00.</span></span>

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="0127c-139">John 按之前**儲存**，Jane 執行相同的頁面和變更**開始日期**從 2007 年 9 月 1 日欄位設為 2013 年 8 月 8 日。</span><span class="sxs-lookup"><span data-stu-id="0127c-139">Before John clicks **Save**, Jane runs the same page and changes the **Start Date** field from 9/1/2007 to 8/8/2013.</span></span>

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="0127c-141">John 按**儲存**第一個和所看到的索引頁，然後 Jane 的瀏覽器傳回時，他變更按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0127c-141">John clicks **Save** first and sees his change when the browser returns to the Index page, then Jane clicks **Save**.</span></span> <span data-ttu-id="0127c-142">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="0127c-142">What happens next is determined by how you handle concurrency conflicts.</span></span> <span data-ttu-id="0127c-143">一部分選項包括下列項目：</span><span class="sxs-lookup"><span data-stu-id="0127c-143">Some of the options include the following:</span></span>

- <span data-ttu-id="0127c-144">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="0127c-144">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span> <span data-ttu-id="0127c-145">在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。</span><span class="sxs-lookup"><span data-stu-id="0127c-145">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="0127c-146">在下一次有人瀏覽英文部門，他們會看到的 John 和 Jane 的變更 — 2013 年 8 月 8 日的開始日期和零美元的預算。</span><span class="sxs-lookup"><span data-stu-id="0127c-146">The next time someone browses the English department, they'll see both John's and Jane's changes — a start date of 8/8/2013 and a budget of Zero dollars.</span></span>

    <span data-ttu-id="0127c-147">這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="0127c-147">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="0127c-148">Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="0127c-148">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="0127c-149">通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-149">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="0127c-150">維護大量的狀態會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁上 （例如，在隱藏的欄位）。</span><span class="sxs-lookup"><span data-stu-id="0127c-150">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields).</span></span>
- <span data-ttu-id="0127c-151">您可以讓 Jane 的變更覆寫 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="0127c-151">You can let Jane's change overwrite John's change.</span></span> <span data-ttu-id="0127c-152">在下一次有人瀏覽英文部門，他們會看到時間 2013 年 8 月 8 日和還原 $350,000.00 值。</span><span class="sxs-lookup"><span data-stu-id="0127c-152">The next time someone browses the English department, they'll see 8/8/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="0127c-153">這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="0127c-153">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="0127c-154">（用戶端的值優先於什麼是資料存放區中。）如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。</span><span class="sxs-lookup"><span data-stu-id="0127c-154">(The client's values take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>
- <span data-ttu-id="0127c-155">您可以防止資料庫中的更新 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="0127c-155">You can prevent Jane's change from being updated in the database.</span></span> <span data-ttu-id="0127c-156">一般而言，您會顯示錯誤訊息、 顯示她目前狀態的資料，並允許她她仍然想要讓它們重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="0127c-156">Typically, you would display an error message, show her the current state of the data, and allow her to reapply her changes if she still wants to make them.</span></span> <span data-ttu-id="0127c-157">這稱之為「存放區獲勝 (Store Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="0127c-157">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="0127c-158">(資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="0127c-158">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="0127c-159">這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="0127c-159">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="0127c-160">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="0127c-160">Detecting Concurrency Conflicts</span></span>

<span data-ttu-id="0127c-161">您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 就會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0127c-161">You can resolve conflicts by handling [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="0127c-162">若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="0127c-162">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="0127c-163">因此，您必須適當的設定資料庫及資料模型。</span><span class="sxs-lookup"><span data-stu-id="0127c-163">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="0127c-164">一部分啟用衝突偵測的選項包括下列選項：</span><span class="sxs-lookup"><span data-stu-id="0127c-164">Some options for enabling conflict detection include the following:</span></span>

- <span data-ttu-id="0127c-165">在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。</span><span class="sxs-lookup"><span data-stu-id="0127c-165">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="0127c-166">接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="0127c-166">You can then configure the Entity Framework to include that column in the `Where` clause of SQL `Update` or `Delete` commands.</span></span>

    <span data-ttu-id="0127c-167">追蹤資料行的資料類型通常是[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="0127c-167">The data type of the tracking column is typically [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx).</span></span> <span data-ttu-id="0127c-168">[Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是連續的數字已遞增資料列更新一次。</span><span class="sxs-lookup"><span data-stu-id="0127c-168">The [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="0127c-169">在`Update`或`Delete`命令，`Where`子句會包含追蹤資料行 （原始資料列版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="0127c-169">In an `Update` or `Delete` command, the `Where` clause includes the original value of the tracking column (the original row version).</span></span> <span data-ttu-id="0127c-170">如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，所以`Update`或`Delete`陳述式找不到的資料列，因為更新`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="0127c-170">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the `Update` or `Delete` statement can't find the row to update because of the `Where` clause.</span></span> <span data-ttu-id="0127c-171">Entity Framework 找到的任何資料列已更新`Update`或`Delete`命令 （亦即，受影響的資料列數目為零） 時，它將會解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="0127c-171">When the Entity Framework finds that no rows have been updated by the `Update` or `Delete` command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>
- <span data-ttu-id="0127c-172">設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="0127c-172">Configure the Entity Framework to include the original values of every column in the table in the `Where` clause of `Update` and `Delete` commands.</span></span>

    <span data-ttu-id="0127c-173">如同第一個選項之後第一次讀取的資料列，, 資料列中的任何項目已變更`Where`子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。</span><span class="sxs-lookup"><span data-stu-id="0127c-173">As in the first option, if anything in the row has changed since the row was first read, the `Where` clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="0127c-174">對於具有許多資料行的資料庫資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維護大量的狀態。</span><span class="sxs-lookup"><span data-stu-id="0127c-174">For database tables that have many columns, this approach can result in very large `Where` clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="0127c-175">如前文所述，維護大量狀態可能會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁本身。</span><span class="sxs-lookup"><span data-stu-id="0127c-175">As noted earlier, maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself.</span></span> <span data-ttu-id="0127c-176">因此一般而言不建議使用這個方法，並不在本教學課程使用的方法。</span><span class="sxs-lookup"><span data-stu-id="0127c-176">Therefore this approach generally not recommended, and it isn't the method used in this tutorial.</span></span>

    <span data-ttu-id="0127c-177">如果您想要實作這個方法來同步存取，您必須將您想要加入追蹤的並行存取實體中的所有非-主索引鍵屬性標記[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)這些屬性。</span><span class="sxs-lookup"><span data-stu-id="0127c-177">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribute to them.</span></span> <span data-ttu-id="0127c-178">變更可讓 Entity Framework 了包含在 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0127c-178">That change enables the Entity Framework to include all columns in the SQL `WHERE` clause of `UPDATE` statements.</span></span>

<span data-ttu-id="0127c-179">在本教學課程的其餘部分，您會加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體建立控制器和檢視，和測試以確認一切運作正常。</span><span class="sxs-lookup"><span data-stu-id="0127c-179">In the remainder of this tutorial you'll add a [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking property to the `Department` entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a><span data-ttu-id="0127c-180">Department 實體中加入的開放式並行存取屬性</span><span class="sxs-lookup"><span data-stu-id="0127c-180">Add an Optimistic Concurrency Property to the Department Entity</span></span>

<span data-ttu-id="0127c-181">在*Models\Department.cs*，加入名為的追蹤屬性`RowVersion`:</span><span class="sxs-lookup"><span data-stu-id="0127c-181">In *Models\Department.cs*, add a tracking property named `RowVersion`:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

<span data-ttu-id="0127c-182">[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定此資料行，將會併入`Where`子句`Update`和`Delete`命令傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="0127c-182">The [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribute specifies that this column will be included in the `Where` clause of `Update` and `Delete` commands sent to the database.</span></span> <span data-ttu-id="0127c-183">該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 使用 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)資料類型，再將 SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)取代它。</span><span class="sxs-lookup"><span data-stu-id="0127c-183">The attribute is called [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) because previous versions of SQL Server used a SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) data type before the SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) replaced it.</span></span> <span data-ttu-id="0127c-184">.Net 類型`rowversion`是位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="0127c-184">The .Net type for `rowversion` is a byte array.</span></span> <span data-ttu-id="0127c-185">如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定的追蹤屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0127c-185">If you prefer to use the fluent API, you can use the [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) method to specify the tracking property, as shown in the following example:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="0127c-186">由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。</span><span class="sxs-lookup"><span data-stu-id="0127c-186">By adding a property you changed the database model, so you need to do another migration.</span></span> <span data-ttu-id="0127c-187">請在套件管理員主控台 (PMC) 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="0127c-187">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a><span data-ttu-id="0127c-188">建立部門控制站</span><span class="sxs-lookup"><span data-stu-id="0127c-188">Create a Department Controller</span></span>

<span data-ttu-id="0127c-189">建立`Department`控制器和檢視表相同的方式就像其他控制站，使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="0127c-189">Create a `Department` controller and views the same way you did the other controllers, using the following settings:</span></span>

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

<span data-ttu-id="0127c-191">在*Controllers\DepartmentController.cs*，新增`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="0127c-191">In *Controllers\DepartmentController.cs*, add a `using` statement:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="0127c-192">"LastName"到"FullName 」 這個檔案 （四個相符項目） 中的所有變更使部門系統管理員下拉式清單將包含講師的完整名稱，而不是最後一個的名稱。</span><span class="sxs-lookup"><span data-stu-id="0127c-192">Change "LastName" to "FullName" everywhere in this file (four occurrences) so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

<span data-ttu-id="0127c-193">現有程式碼取代`HttpPost``Edit`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0127c-193">Replace the existing code for the `HttpPost` `Edit` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="0127c-194">檢視會儲存原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-194">The view will store the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="0127c-195">當模型繫結器建立`department`執行個體，該物件會有原始`RowVersion`屬性值和其他屬性，因為使用者輸入的 [編輯] 頁面上的新值。</span><span class="sxs-lookup"><span data-stu-id="0127c-195">When the model binder creates the `department` instance, that object will have the original `RowVersion` property value and the new values for the other properties, as entered by the user on the Edit page.</span></span> <span data-ttu-id="0127c-196">然後當 Entity Framework 建立 SQL`UPDATE`命令時，命令將會包含`WHERE`會尋找具有原始的資料列的子句`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="0127c-196">Then when the Entity Framework creates a SQL `UPDATE` command, that command will include a `WHERE` clause that looks for a row that has the original `RowVersion` value.</span></span>

<span data-ttu-id="0127c-197">如果任何資料列不受到`UPDATE`命令 (沒有資料列的原始`RowVersion`值)，Entity Framework 就會擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得在受影響`Department`例外狀況的實體物件。</span><span class="sxs-lookup"><span data-stu-id="0127c-197">If no rows are affected by the `UPDATE` command (no rows have the original `RowVersion` value), the Entity Framework throws a `DbUpdateConcurrencyException` exception, and the code in the `catch` block gets the affected `Department` entity from the exception object.</span></span> <span data-ttu-id="0127c-198">此實體已從資料庫讀取的值和新的使用者所輸入的值：</span><span class="sxs-lookup"><span data-stu-id="0127c-198">This entity has both the values read from the database and the new values entered by the user:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="0127c-199">接下來，程式碼會加入每個資料行，其中包含資料庫值不同於 [編輯] 頁面上輸入的使用者自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0127c-199">Next, the code adds a custom error message for each column that has database values different from what the user entered on the Edit page:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="0127c-200">較長的錯誤訊息，說明發生了什麼事和資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="0127c-200">A longer error message explains what happened and what to do about it:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="0127c-201">最後，此程式碼設定`RowVersion`值`Department`從資料庫擷取物件新的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-201">Finally, the code sets the `RowVersion` value of the `Department` object to the new value retrieved from the database.</span></span> <span data-ttu-id="0127c-202">這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="0127c-202">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="0127c-203">在*Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值，緊接的隱藏的欄位`DepartmentID`屬性：</span><span class="sxs-lookup"><span data-stu-id="0127c-203">In *Views\Department\Edit.cshtml*, add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

<span data-ttu-id="0127c-204">在*Views\Department\Index.cshtml*，現有的程式碼取代為下列程式碼移到最左邊的資料列的連結，並變更頁面標題和資料行標題，以顯示`FullName`而不是`LastName`中**管理員**資料行：</span><span class="sxs-lookup"><span data-stu-id="0127c-204">In *Views\Department\Index.cshtml*, replace the existing code with the following code to move row links to the left and change the page title and column headings to display `FullName` instead of `LastName` in the **Administrator** column:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a><span data-ttu-id="0127c-205">測試開放式並行存取處理</span><span class="sxs-lookup"><span data-stu-id="0127c-205">Testing Optimistic Concurrency Handling</span></span>

<span data-ttu-id="0127c-206">執行站台，然後按一下**部門**:</span><span class="sxs-lookup"><span data-stu-id="0127c-206">Run the site and click **Departments**:</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="0127c-208">以滑鼠右鍵按一下**編輯**Kim Abercrombie 並選取超連結**中新的索引標籤上，開啟**然後按一下 **編輯**Kim Abercrombie 的超連結。</span><span class="sxs-lookup"><span data-stu-id="0127c-208">Right click the **Edit** hyperlink for Kim Abercrombie and select **Open in new tab,** then click the **Edit** hyperlink for Kim Abercrombie.</span></span> <span data-ttu-id="0127c-209">兩個視窗會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="0127c-209">The two windows display the same information.</span></span>

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="0127c-211">變更第一個瀏覽器視窗中的欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0127c-211">Change a field in the first browser window and click **Save**.</span></span>

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="0127c-213">瀏覽器會顯示索引頁面，當中包含了變更之後的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-213">The browser shows the Index page with the changed value.</span></span>

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="0127c-215">變更第二個瀏覽器視窗中的任何欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0127c-215">Change the any field in the second browser window and click **Save**.</span></span>

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="0127c-217">按一下**儲存**第二個瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="0127c-217">Click **Save** in the second browser window.</span></span> <span data-ttu-id="0127c-218">您會看到一個錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0127c-218">You see an error message:</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="0127c-220">再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="0127c-220">Click **Save** again.</span></span> <span data-ttu-id="0127c-221">您在第二個瀏覽器中輸入的值會與您在第一個瀏覽器中變更資料的原始值一起儲存。</span><span class="sxs-lookup"><span data-stu-id="0127c-221">The value you entered in the second browser is saved along with the original value of the data you change in the first browser.</span></span> <span data-ttu-id="0127c-222">您會在索引頁面出現時看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-222">You see the saved values when the Index page appears.</span></span>

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a><span data-ttu-id="0127c-224">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="0127c-224">Updating the Delete Page</span></span>

<span data-ttu-id="0127c-225">針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。</span><span class="sxs-lookup"><span data-stu-id="0127c-225">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="0127c-226">當`HttpGet``Delete`方法會顯示 [確認] 檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="0127c-226">When the `HttpGet` `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="0127c-227">值，就可以使用`HttpPost``Delete`使用者確認刪除作業時所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="0127c-227">That value is then available to the `HttpPost` `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="0127c-228">當 Entity Framework 建立 SQL`DELETE`命令時，它包含`WHERE`子句使用原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="0127c-228">When the Entity Framework creates the SQL `DELETE` command, it includes a `WHERE` clause with the original `RowVersion` value.</span></span> <span data-ttu-id="0127c-229">如果命令結果中零個資料列受影響 （已顯示 [刪除確認] 頁面之後，資料列已變更的意義），則會擲回並行存取例外狀況，而`HttpGet Delete`錯誤旗標設定為呼叫方法`true`以重新顯示確認頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0127c-229">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the `HttpGet Delete` method is called with an error flag set to `true` in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="0127c-230">此外，也可以零個資料列已受到影響，因為如此不同的錯誤訊息就會顯示在此情況下，資料列由其他使用者時，所刪除。</span><span class="sxs-lookup"><span data-stu-id="0127c-230">It's also possible that zero rows were affected because the row was deleted by another user, so in that case a different error message is displayed.</span></span>

<span data-ttu-id="0127c-231">在*DepartmentController.cs*，取代`HttpGet``Delete`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0127c-231">In *DepartmentController.cs*, replace the `HttpGet` `Delete` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="0127c-232">方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。</span><span class="sxs-lookup"><span data-stu-id="0127c-232">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="0127c-233">如果這個旗標`true`，錯誤訊息傳送至檢視使用`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="0127c-233">If this flag is `true`, an error message is sent to the view using a `ViewBag` property.</span></span>

<span data-ttu-id="0127c-234">中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0127c-234">Replace the code in the `HttpPost` `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="0127c-235">在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="0127c-235">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="0127c-236">您已變更此參數，以`Department`模型繫結器所建立的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="0127c-236">You've changed this parameter to a `Department` entity instance created by the model binder.</span></span> <span data-ttu-id="0127c-237">這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。</span><span class="sxs-lookup"><span data-stu-id="0127c-237">This gives you access to the `RowVersion` property value in addition to the record key.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="0127c-238">您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。</span><span class="sxs-lookup"><span data-stu-id="0127c-238">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="0127c-239">名為 scaffold 的程式碼`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="0127c-239">The scaffolded code named the `HttpPost` `Delete` method `DeleteConfirmed` to give the `HttpPost` method a unique signature.</span></span> <span data-ttu-id="0127c-240">(CLR 要求多載方法必須要有不同的方法參數。)現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`刪除方法。</span><span class="sxs-lookup"><span data-stu-id="0127c-240">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the `HttpPost` and `HttpGet` delete methods.</span></span>

<span data-ttu-id="0127c-241">若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0127c-241">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

<span data-ttu-id="0127c-242">在*Views\Department\Delete.cshtml*，取代 scaffold 的程式碼，讓某些格式設定為下列程式碼變更，並加入錯誤訊息 欄位。</span><span class="sxs-lookup"><span data-stu-id="0127c-242">In *Views\Department\Delete.cshtml*, replace the scaffolded code with the following code that makes some formatting changes and adds an error message field.</span></span> <span data-ttu-id="0127c-243">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="0127c-243">The changes are highlighted.</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

<span data-ttu-id="0127c-244">這個程式碼加入錯誤訊息之間`h2`和`h3`標題：</span><span class="sxs-lookup"><span data-stu-id="0127c-244">This code adds an error message between the `h2` and `h3` headings:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="0127c-245">它會取代`LastName`與`FullName`中`Administrator`欄位：</span><span class="sxs-lookup"><span data-stu-id="0127c-245">It replaces `LastName` with `FullName` in the `Administrator` field:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="0127c-246">最後，它會加入隱藏的欄位`DepartmentID`和`RowVersion`屬性之後`Html.BeginForm`陳述式：</span><span class="sxs-lookup"><span data-stu-id="0127c-246">Finally, it adds hidden fields for the `DepartmentID` and `RowVersion` properties after the `Html.BeginForm` statement:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="0127c-247">執行部門索引頁面。</span><span class="sxs-lookup"><span data-stu-id="0127c-247">Run the Departments Index page.</span></span> <span data-ttu-id="0127c-248">以滑鼠右鍵按一下**刪除**英文部門並選取超連結**在新視窗中，開啟**然後在第一個視窗中按一下**編輯**英文的超連結部門。</span><span class="sxs-lookup"><span data-stu-id="0127c-248">Right click the **Delete** hyperlink for the English department and select **Open in new window,** then in the first window click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="0127c-249">在第一個視窗中，變更其中一個值，然後按一下**儲存**:</span><span class="sxs-lookup"><span data-stu-id="0127c-249">In the first window, change one of the values, and click **Save** :</span></span>

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<span data-ttu-id="0127c-251">索引頁確認變更。</span><span class="sxs-lookup"><span data-stu-id="0127c-251">The Index page confirms the change.</span></span>

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

<span data-ttu-id="0127c-253">在第二個視窗中，按一下 **刪除**。</span><span class="sxs-lookup"><span data-stu-id="0127c-253">In the second window, click **Delete**.</span></span>

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

<span data-ttu-id="0127c-255">您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。</span><span class="sxs-lookup"><span data-stu-id="0127c-255">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="0127c-257">若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。</span><span class="sxs-lookup"><span data-stu-id="0127c-257">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="0127c-258">總結</span><span class="sxs-lookup"><span data-stu-id="0127c-258">Summary</span></span>

<span data-ttu-id="0127c-259">如此即完成了處理並行衝突的簡介。</span><span class="sxs-lookup"><span data-stu-id="0127c-259">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="0127c-260">其他的方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)和[屬性值使用](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="0127c-260">For information about other ways to handle various concurrency scenarios, see [Optimistic Concurrency Patterns](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) and [Working with Property Values](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) on the Entity Framework team blog.</span></span> <span data-ttu-id="0127c-261">下一個教學課程示範如何實作的每個階層的資料表繼承`Instructor`和`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="0127c-261">The next tutorial shows how to implement table-per-hierarchy inheritance for the `Instructor` and `Student` entities.</span></span>

<span data-ttu-id="0127c-262">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="0127c-262">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0127c-263">[上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="0127c-263">[Previous](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
