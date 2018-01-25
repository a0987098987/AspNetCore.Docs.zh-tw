---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "處理並行與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-7) |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 87bb08a4d16965a10112a42c4e9318c32f192c04
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a><span data-ttu-id="bd9be-103">處理並行與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-7)</span><span class="sxs-lookup"><span data-stu-id="bd9be-103">Handling Concurrency with the Entity Framework in an ASP.NET MVC Application (7 of 10)</span></span>
====================
<span data-ttu-id="bd9be-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bd9be-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="bd9be-105">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="bd9be-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="bd9be-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="bd9be-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="bd9be-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="bd9be-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="bd9be-108">您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="bd9be-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="bd9be-109">如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。</span><span class="sxs-lookup"><span data-stu-id="bd9be-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="bd9be-110">您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="bd9be-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="bd9be-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="bd9be-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="bd9be-112">在先前的兩個教學課程中您工作與相關資料。</span><span class="sxs-lookup"><span data-stu-id="bd9be-112">In the previous two tutorials you worked with related data.</span></span> <span data-ttu-id="bd9be-113">本教學課程會示範如何處理並行存取。</span><span class="sxs-lookup"><span data-stu-id="bd9be-113">This tutorial shows how to handle concurrency.</span></span> <span data-ttu-id="bd9be-114">您將建立可搭配 web 網頁`Department`實體，以及編輯和刪除的頁面`Department`實體處理的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd9be-114">You'll create web pages that work with the `Department` entity, and the pages that edit and delete `Department` entities will handle concurrency errors.</span></span> <span data-ttu-id="bd9be-115">下圖顯示的索引和刪除的頁面，包括發生並行衝突時，會顯示一些訊息。</span><span class="sxs-lookup"><span data-stu-id="bd9be-115">The following illustrations show the Index and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="bd9be-118">並行衝突</span><span class="sxs-lookup"><span data-stu-id="bd9be-118">Concurrency Conflicts</span></span>

<span data-ttu-id="bd9be-119">當一位使用者才能加以編輯，顯示實體的資料，然後另一位使用者更新相同的實體資料時的第一個使用者變更寫入資料庫之前，就會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="bd9be-119">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="bd9be-120">如果您未啟用的這類的衝突偵測，所有人員都更新資料庫最後會覆寫其他使用者的變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-120">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="bd9be-121">在許多應用程式，這項風險是可接受： 如果有少數的使用者或一些更新，或如果不是最關鍵的某些變更會覆寫時，如果並行程式設計的成本可能會超過的好處。</span><span class="sxs-lookup"><span data-stu-id="bd9be-121">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="bd9be-122">在此情況下，您不需要設定應用程式處理並行存取衝突。</span><span class="sxs-lookup"><span data-stu-id="bd9be-122">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="bd9be-123">封閉式並行存取 （鎖定）</span><span class="sxs-lookup"><span data-stu-id="bd9be-123">Pessimistic Concurrency (Locking)</span></span>

<span data-ttu-id="bd9be-124">如果您的應用程式需要以防止資料意外遺失並行案例中的，方法之一是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="bd9be-124">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="bd9be-125">這稱為*封閉式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="bd9be-125">This is called *pessimistic concurrency*.</span></span> <span data-ttu-id="bd9be-126">例如，從資料庫讀取資料列之前，您要求的鎖定唯讀或 update 存取權。</span><span class="sxs-lookup"><span data-stu-id="bd9be-126">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="bd9be-127">如果您鎖定的資料列進行 update 存取權，允許任何其他使用者鎖定資料列的唯讀或更新存取權，因為他們會取得正在變更的資料副本。</span><span class="sxs-lookup"><span data-stu-id="bd9be-127">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="bd9be-128">如果鎖定為唯讀存取的資料列時，其他人可以唯讀存取權，但不適用於更新也鎖定。</span><span class="sxs-lookup"><span data-stu-id="bd9be-128">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="bd9be-129">管理鎖定有缺點。</span><span class="sxs-lookup"><span data-stu-id="bd9be-129">Managing locks has disadvantages.</span></span> <span data-ttu-id="bd9be-130">是相當複雜的程式。</span><span class="sxs-lookup"><span data-stu-id="bd9be-130">It can be complex to program.</span></span> <span data-ttu-id="bd9be-131">這需要大量的資料庫管理資源，而且它會造成效能問題的應用程式的使用者數目增加時 （亦即，將無法妥善調整）。</span><span class="sxs-lookup"><span data-stu-id="bd9be-131">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases (that is, it doesn't scale well).</span></span> <span data-ttu-id="bd9be-132">基於這些理由，並非所有的資料庫管理系統支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="bd9be-132">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="bd9be-133">Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。</span><span class="sxs-lookup"><span data-stu-id="bd9be-133">The Entity Framework provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="bd9be-134">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="bd9be-134">Optimistic Concurrency</span></span>

<span data-ttu-id="bd9be-135">封閉式並行存取另一種是*開放式並行存取*。</span><span class="sxs-lookup"><span data-stu-id="bd9be-135">The alternative to pessimistic concurrency is *optimistic concurrency*.</span></span> <span data-ttu-id="bd9be-136">開放式並行存取表示允許發生並行衝突，然後適當地回應如果沒有的話。</span><span class="sxs-lookup"><span data-stu-id="bd9be-136">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="bd9be-137">例如，John 會執行 部門編輯頁面上，變更**預算**$0.00 從 $350,000.00 英文部門的數量。</span><span class="sxs-lookup"><span data-stu-id="bd9be-137">For example, John runs the Departments Edit page, changes the **Budget** amount for the English department from $350,000.00 to $0.00.</span></span>

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="bd9be-139">John 按之前**儲存**，Jane 執行相同的頁面和變更**開始日期**從 2007 年 9 月 1 日欄位設為 2013 年 8 月 8 日。</span><span class="sxs-lookup"><span data-stu-id="bd9be-139">Before John clicks **Save**, Jane runs the same page and changes the **Start Date** field from 9/1/2007 to 8/8/2013.</span></span>

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="bd9be-141">John 按**儲存**第一個和所看到的索引頁，然後 Jane 的瀏覽器傳回時，他變更按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="bd9be-141">John clicks **Save** first and sees his change when the browser returns to the Index page, then Jane clicks **Save**.</span></span> <span data-ttu-id="bd9be-142">接下來的情況取決於您如何處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="bd9be-142">What happens next is determined by how you handle concurrency conflicts.</span></span> <span data-ttu-id="bd9be-143">某些選項包括：</span><span class="sxs-lookup"><span data-stu-id="bd9be-143">Some of the options include the following:</span></span>

- <span data-ttu-id="bd9be-144">您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="bd9be-144">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span> <span data-ttu-id="bd9be-145">在範例案例中，任何資料將不會遺失，因為兩位使用者已更新不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="bd9be-145">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="bd9be-146">在下一次有人瀏覽英文部門，他們會看到的 John 和 Jane 的變更 — 2013 年 8 月 8 日的開始日期和零美元的預算。</span><span class="sxs-lookup"><span data-stu-id="bd9be-146">The next time someone browses the English department, they'll see both John's and Jane's changes — a start date of 8/8/2013 and a budget of Zero dollars.</span></span>

    <span data-ttu-id="bd9be-147">這種更新方法可以減少可能會導致資料遺失的衝突數目，但它無法避免資料遺失，如果相同的實體屬性進行競爭的變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-147">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="bd9be-148">Entity Framework 是否這種方式運作，端視實作更新程式碼的方式而定。</span><span class="sxs-lookup"><span data-stu-id="bd9be-148">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="bd9be-149">它通常中並不實用的 web 應用程式，因為它可能需要您維護大量狀態，若要追蹤的所有實體的原始屬性值以及新的值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-149">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="bd9be-150">維護大量的狀態會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁上 （例如，在隱藏的欄位）。</span><span class="sxs-lookup"><span data-stu-id="bd9be-150">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields).</span></span>
- <span data-ttu-id="bd9be-151">您可以讓 Jane 的變更覆寫 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-151">You can let Jane's change overwrite John's change.</span></span> <span data-ttu-id="bd9be-152">在下一次有人瀏覽英文部門，他們會看到時間 2013 年 8 月 8 日和還原 $350,000.00 值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-152">The next time someone browses the English department, they'll see 8/8/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="bd9be-153">這稱為*用戶端獲勝*或*Wins 中的最後一個*案例。</span><span class="sxs-lookup"><span data-stu-id="bd9be-153">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="bd9be-154">（用戶端的值優先於什麼是資料存放區中。）所述的簡介本節中，如果您不執行任何撰寫的程式碼的並行處理，這會自動發生。</span><span class="sxs-lookup"><span data-stu-id="bd9be-154">(The client's values take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>
- <span data-ttu-id="bd9be-155">您可以防止資料庫中的更新 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-155">You can prevent Jane's change from being updated in the database.</span></span> <span data-ttu-id="bd9be-156">一般而言，您會顯示錯誤訊息、 顯示她目前狀態的資料，並允許她她仍然想要讓它們重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-156">Typically, you would display an error message, show her the current state of the data, and allow her to reapply her changes if she still wants to make them.</span></span> <span data-ttu-id="bd9be-157">這稱為*存放區 Wins*案例。</span><span class="sxs-lookup"><span data-stu-id="bd9be-157">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="bd9be-158">（資料存放區的值優先於用戶端所送出的值。）您將在本教學課程實作的儲存區 Wins 的案例。</span><span class="sxs-lookup"><span data-stu-id="bd9be-158">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="bd9be-159">這個方法會確保使用者正在發出警示，這為什麼不會覆寫任何變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-159">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="bd9be-160">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="bd9be-160">Detecting Concurrency Conflicts</span></span>

<span data-ttu-id="bd9be-161">您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 就會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bd9be-161">You can resolve conflicts by handling [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="bd9be-162">若要知道何時擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="bd9be-162">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="bd9be-163">因此，您必須設定資料庫和資料模型適當。</span><span class="sxs-lookup"><span data-stu-id="bd9be-163">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="bd9be-164">啟用衝突偵測的一些選項如下：</span><span class="sxs-lookup"><span data-stu-id="bd9be-164">Some options for enabling conflict detection include the following:</span></span>

- <span data-ttu-id="bd9be-165">在資料庫資料表中，包含可用來判斷當資料列已經變更的追蹤資料行。</span><span class="sxs-lookup"><span data-stu-id="bd9be-165">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="bd9be-166">接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="bd9be-166">You can then configure the Entity Framework to include that column in the `Where` clause of SQL `Update` or `Delete` commands.</span></span>

    <span data-ttu-id="bd9be-167">追蹤資料行的資料類型通常是[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="bd9be-167">The data type of the tracking column is typically [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx).</span></span> <span data-ttu-id="bd9be-168">[Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是連續的數字已遞增資料列更新一次。</span><span class="sxs-lookup"><span data-stu-id="bd9be-168">The [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="bd9be-169">在`Update`或`Delete`命令，`Where`子句會包含追蹤資料行 （原始資料列版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-169">In an `Update` or `Delete` command, the `Where` clause includes the original value of the tracking column (the original row version).</span></span> <span data-ttu-id="bd9be-170">如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，所以`Update`或`Delete`陳述式找不到的資料列，因為更新`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="bd9be-170">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the `Update` or `Delete` statement can't find the row to update because of the `Where` clause.</span></span> <span data-ttu-id="bd9be-171">Entity Framework 找到的任何資料列已更新`Update`或`Delete`命令 （亦即，受影響的資料列數目為零） 時，它將會解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="bd9be-171">When the Entity Framework finds that no rows have been updated by the `Update` or `Delete` command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>
- <span data-ttu-id="bd9be-172">設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="bd9be-172">Configure the Entity Framework to include the original values of every column in the table in the `Where` clause of `Update` and `Delete` commands.</span></span>

    <span data-ttu-id="bd9be-173">如同第一個選項之後第一次讀取的資料列，, 資料列中的任何項目已變更`Where`子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。</span><span class="sxs-lookup"><span data-stu-id="bd9be-173">As in the first option, if anything in the row has changed since the row was first read, the `Where` clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="bd9be-174">對於具有許多資料行的資料庫資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維護大量的狀態。</span><span class="sxs-lookup"><span data-stu-id="bd9be-174">For database tables that have many columns, this approach can result in very large `Where` clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="bd9be-175">如前文所述，維護大量狀態可能會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁本身。</span><span class="sxs-lookup"><span data-stu-id="bd9be-175">As noted earlier, maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself.</span></span> <span data-ttu-id="bd9be-176">因此一般而言不建議使用這個方法，並不在本教學課程使用的方法。</span><span class="sxs-lookup"><span data-stu-id="bd9be-176">Therefore this approach generally not recommended, and it isn't the method used in this tutorial.</span></span>

    <span data-ttu-id="bd9be-177">如果您想要實作這個方法來同步存取，您必須將您想要加入追蹤的並行存取實體中的所有非-主索引鍵屬性標記[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)這些屬性。</span><span class="sxs-lookup"><span data-stu-id="bd9be-177">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribute to them.</span></span> <span data-ttu-id="bd9be-178">變更可讓 Entity Framework 了包含在 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="bd9be-178">That change enables the Entity Framework to include all columns in the SQL `WHERE` clause of `UPDATE` statements.</span></span>

<span data-ttu-id="bd9be-179">在本教學課程的其餘部分，您會加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體建立控制器和檢視，和測試以確認一切運作正常。</span><span class="sxs-lookup"><span data-stu-id="bd9be-179">In the remainder of this tutorial you'll add a [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking property to the `Department` entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a><span data-ttu-id="bd9be-180">Department 實體中加入的開放式並行存取屬性</span><span class="sxs-lookup"><span data-stu-id="bd9be-180">Add an Optimistic Concurrency Property to the Department Entity</span></span>

<span data-ttu-id="bd9be-181">在*Models\Department.cs*，加入名為的追蹤屬性`RowVersion`:</span><span class="sxs-lookup"><span data-stu-id="bd9be-181">In *Models\Department.cs*, add a tracking property named `RowVersion`:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

<span data-ttu-id="bd9be-182">[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定此資料行，將會併入`Where`子句`Update`和`Delete`命令傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="bd9be-182">The [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribute specifies that this column will be included in the `Where` clause of `Update` and `Delete` commands sent to the database.</span></span> <span data-ttu-id="bd9be-183">該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 使用 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)資料類型，再將 SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)取代它。</span><span class="sxs-lookup"><span data-stu-id="bd9be-183">The attribute is called [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) because previous versions of SQL Server used a SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) data type before the SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) replaced it.</span></span> <span data-ttu-id="bd9be-184">.Net 類型`rowversion`是位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="bd9be-184">The .Net type for `rowversion` is a byte array.</span></span> <span data-ttu-id="bd9be-185">如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定的追蹤屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bd9be-185">If you prefer to use the fluent API, you can use the [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) method to specify the tracking property, as shown in the following example:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="bd9be-186">將屬性加入您變更資料庫模型，因此您必須進行其他移轉。</span><span class="sxs-lookup"><span data-stu-id="bd9be-186">By adding a property you changed the database model, so you need to do another migration.</span></span> <span data-ttu-id="bd9be-187">在 封裝管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="bd9be-187">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a><span data-ttu-id="bd9be-188">建立部門控制站</span><span class="sxs-lookup"><span data-stu-id="bd9be-188">Create a Department Controller</span></span>

<span data-ttu-id="bd9be-189">建立`Department`控制器和檢視表相同的方式就像其他控制站，使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="bd9be-189">Create a `Department` controller and views the same way you did the other controllers, using the following settings:</span></span>

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

<span data-ttu-id="bd9be-191">在*Controllers\DepartmentController.cs*，新增`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="bd9be-191">In *Controllers\DepartmentController.cs*, add a `using` statement:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="bd9be-192">"LastName"到"FullName 」 這個檔案 （四個相符項目） 中的所有變更使部門系統管理員下拉式清單將包含講師的完整名稱，而不是最後一個的名稱。</span><span class="sxs-lookup"><span data-stu-id="bd9be-192">Change "LastName" to "FullName" everywhere in this file (four occurrences) so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

<span data-ttu-id="bd9be-193">現有程式碼取代`HttpPost``Edit`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd9be-193">Replace the existing code for the `HttpPost` `Edit` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="bd9be-194">檢視會儲存原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-194">The view will store the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="bd9be-195">當模型繫結器建立`department`執行個體，該物件會有原始`RowVersion`屬性值和其他屬性，因為使用者輸入的 [編輯] 頁面上的新值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-195">When the model binder creates the `department` instance, that object will have the original `RowVersion` property value and the new values for the other properties, as entered by the user on the Edit page.</span></span> <span data-ttu-id="bd9be-196">然後當 Entity Framework 建立 SQL`UPDATE`命令時，命令將會包含`WHERE`會尋找具有原始的資料列的子句`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-196">Then when the Entity Framework creates a SQL `UPDATE` command, that command will include a `WHERE` clause that looks for a row that has the original `RowVersion` value.</span></span>

<span data-ttu-id="bd9be-197">如果任何資料列不受到`UPDATE`命令 (沒有資料列的原始`RowVersion`值)，Entity Framework 就會擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得在受影響`Department`例外狀況的實體物件。</span><span class="sxs-lookup"><span data-stu-id="bd9be-197">If no rows are affected by the `UPDATE` command (no rows have the original `RowVersion` value), the Entity Framework throws a `DbUpdateConcurrencyException` exception, and the code in the `catch` block gets the affected `Department` entity from the exception object.</span></span> <span data-ttu-id="bd9be-198">此實體已從資料庫讀取的值和新的使用者所輸入的值：</span><span class="sxs-lookup"><span data-stu-id="bd9be-198">This entity has both the values read from the database and the new values entered by the user:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="bd9be-199">接下來，程式碼會加入每個資料行，其中包含資料庫值不同於 [編輯] 頁面上輸入的使用者自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="bd9be-199">Next, the code adds a custom error message for each column that has database values different from what the user entered on the Edit page:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="bd9be-200">較長的錯誤訊息，說明發生了什麼事和資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="bd9be-200">A longer error message explains what happened and what to do about it:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="bd9be-201">最後，此程式碼設定`RowVersion`值`Department`從資料庫擷取物件新的值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-201">Finally, the code sets the `RowVersion` value of the `Department` object to the new value retrieved from the database.</span></span> <span data-ttu-id="bd9be-202">這個新`RowVersion`值將會儲存在隱藏的欄位中，每當使用者按一下 編輯 頁面會重新顯示和下一步 時**儲存**，時，會發生因為編輯頁面的顯示會攔截到的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd9be-202">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="bd9be-203">在*Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值，緊接的隱藏的欄位`DepartmentID`屬性：</span><span class="sxs-lookup"><span data-stu-id="bd9be-203">In *Views\Department\Edit.cshtml*, add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

<span data-ttu-id="bd9be-204">在*Views\Department\Index.cshtml*，現有的程式碼取代為下列程式碼移到最左邊的資料列的連結，並變更頁面標題和資料行標題，以顯示`FullName`而不是`LastName`中**管理員**資料行：</span><span class="sxs-lookup"><span data-stu-id="bd9be-204">In *Views\Department\Index.cshtml*, replace the existing code with the following code to move row links to the left and change the page title and column headings to display `FullName` instead of `LastName` in the **Administrator** column:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a><span data-ttu-id="bd9be-205">測試開放式並行存取處理</span><span class="sxs-lookup"><span data-stu-id="bd9be-205">Testing Optimistic Concurrency Handling</span></span>

<span data-ttu-id="bd9be-206">執行站台，然後按一下**部門**:</span><span class="sxs-lookup"><span data-stu-id="bd9be-206">Run the site and click **Departments**:</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="bd9be-208">以滑鼠右鍵按一下**編輯**Kim Abercrombie 並選取超連結**中新的索引標籤上，開啟**然後按一下 **編輯**Kim Abercrombie 的超連結。</span><span class="sxs-lookup"><span data-stu-id="bd9be-208">Right click the **Edit** hyperlink for Kim Abercrombie and select **Open in new tab,** then click the **Edit** hyperlink for Kim Abercrombie.</span></span> <span data-ttu-id="bd9be-209">兩個視窗會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="bd9be-209">The two windows display the same information.</span></span>

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="bd9be-211">變更第一個瀏覽器視窗中的欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="bd9be-211">Change a field in the first browser window and click **Save**.</span></span>

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="bd9be-213">瀏覽器會顯示已變更值的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="bd9be-213">The browser shows the Index page with the changed value.</span></span>

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="bd9be-215">變更第二個瀏覽器視窗中的任何欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="bd9be-215">Change the any field in the second browser window and click **Save**.</span></span>

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="bd9be-217">按一下**儲存**第二個瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="bd9be-217">Click **Save** in the second browser window.</span></span> <span data-ttu-id="bd9be-218">您會看到一則錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="bd9be-218">You see an error message:</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="bd9be-220">按一下**儲存**一次。</span><span class="sxs-lookup"><span data-stu-id="bd9be-220">Click **Save** again.</span></span> <span data-ttu-id="bd9be-221">您在第二個瀏覽器中輸入的值會與您在第一個瀏覽器中變更資料的原始值一起儲存。</span><span class="sxs-lookup"><span data-stu-id="bd9be-221">The value you entered in the second browser is saved along with the original value of the data you change in the first browser.</span></span> <span data-ttu-id="bd9be-222">索引頁出現時，您會看到已儲存的值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-222">You see the saved values when the Index page appears.</span></span>

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a><span data-ttu-id="bd9be-224">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="bd9be-224">Updating the Delete Page</span></span>

<span data-ttu-id="bd9be-225">刪除設定 頁面上，針對 Entity Framework 會偵測並行衝突造成的其他人其他類似的方式編輯部門。</span><span class="sxs-lookup"><span data-stu-id="bd9be-225">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="bd9be-226">當`HttpGet``Delete`方法會顯示 [確認] 檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-226">When the `HttpGet` `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="bd9be-227">值，就可以使用`HttpPost``Delete`使用者確認刪除作業時所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="bd9be-227">That value is then available to the `HttpPost` `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="bd9be-228">當 Entity Framework 建立 SQL`DELETE`命令時，它包含`WHERE`子句使用原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-228">When the Entity Framework creates the SQL `DELETE` command, it includes a `WHERE` clause with the original `RowVersion` value.</span></span> <span data-ttu-id="bd9be-229">如果命令結果中零個資料列受影響 （已顯示 [刪除確認] 頁面之後，資料列已變更的意義），則會擲回並行存取例外狀況，而`HttpGet Delete`錯誤旗標設定為呼叫方法`true`以重新顯示確認頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bd9be-229">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the `HttpGet Delete` method is called with an error flag set to `true` in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="bd9be-230">此外，也可以零個資料列已受到影響，因為如此不同的錯誤訊息就會顯示在此情況下，資料列由其他使用者時，所刪除。</span><span class="sxs-lookup"><span data-stu-id="bd9be-230">It's also possible that zero rows were affected because the row was deleted by another user, so in that case a different error message is displayed.</span></span>

<span data-ttu-id="bd9be-231">在*DepartmentController.cs*，取代`HttpGet``Delete`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd9be-231">In *DepartmentController.cs*, replace the `HttpGet` `Delete` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="bd9be-232">方法會接受選擇性參數，指出頁面是否正在重新顯示之後並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd9be-232">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="bd9be-233">如果這個旗標`true`，錯誤訊息傳送至檢視使用`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="bd9be-233">If this flag is `true`, an error message is sent to the view using a `ViewBag` property.</span></span>

<span data-ttu-id="bd9be-234">中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd9be-234">Replace the code in the `HttpPost` `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="bd9be-235">在您剛才取代的 scaffold 程式碼，這個方法會接受只記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="bd9be-235">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="bd9be-236">您已變更此參數，以`Department`模型繫結器所建立的實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="bd9be-236">You've changed this parameter to a `Department` entity instance created by the model binder.</span></span> <span data-ttu-id="bd9be-237">這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。</span><span class="sxs-lookup"><span data-stu-id="bd9be-237">This gives you access to the `RowVersion` property value in addition to the record key.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="bd9be-238">您也已經變更的動作方法名稱`DeleteConfirmed`至`Delete`。</span><span class="sxs-lookup"><span data-stu-id="bd9be-238">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="bd9be-239">名為 scaffold 的程式碼`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="bd9be-239">The scaffolded code named the `HttpPost` `Delete` method `DeleteConfirmed` to give the `HttpPost` method a unique signature.</span></span> <span data-ttu-id="bd9be-240">（在 CLR 需要有不同的方法參數的多載的方法）。現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`刪除方法。</span><span class="sxs-lookup"><span data-stu-id="bd9be-240">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the `HttpPost` and `HttpGet` delete methods.</span></span>

<span data-ttu-id="bd9be-241">如果會攔截到並行處理錯誤，程式碼會重新顯示 [刪除確認] 頁面，並提供旗標，指出它應該會顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bd9be-241">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

<span data-ttu-id="bd9be-242">在*Views\Department\Delete.cshtml*，取代 scaffold 的程式碼，讓某些格式設定為下列程式碼變更，並加入錯誤訊息 欄位。</span><span class="sxs-lookup"><span data-stu-id="bd9be-242">In *Views\Department\Delete.cshtml*, replace the scaffolded code with the following code that makes some formatting changes and adds an error message field.</span></span> <span data-ttu-id="bd9be-243">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="bd9be-243">The changes are highlighted.</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

<span data-ttu-id="bd9be-244">這個程式碼加入錯誤訊息之間`h2`和`h3`標題：</span><span class="sxs-lookup"><span data-stu-id="bd9be-244">This code adds an error message between the `h2` and `h3` headings:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="bd9be-245">它會取代`LastName`與`FullName`中`Administrator`欄位：</span><span class="sxs-lookup"><span data-stu-id="bd9be-245">It replaces `LastName` with `FullName` in the `Administrator` field:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="bd9be-246">最後，它會加入隱藏的欄位`DepartmentID`和`RowVersion`屬性之後`Html.BeginForm`陳述式：</span><span class="sxs-lookup"><span data-stu-id="bd9be-246">Finally, it adds hidden fields for the `DepartmentID` and `RowVersion` properties after the `Html.BeginForm` statement:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="bd9be-247">執行部門索引頁面。</span><span class="sxs-lookup"><span data-stu-id="bd9be-247">Run the Departments Index page.</span></span> <span data-ttu-id="bd9be-248">以滑鼠右鍵按一下**刪除**英文部門並選取超連結**在新視窗中，開啟**然後在第一個視窗中按一下**編輯**英文的超連結部門。</span><span class="sxs-lookup"><span data-stu-id="bd9be-248">Right click the **Delete** hyperlink for the English department and select **Open in new window,** then in the first window click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="bd9be-249">在第一個視窗中，變更其中一個值，然後按一下**儲存**:</span><span class="sxs-lookup"><span data-stu-id="bd9be-249">In the first window, change one of the values, and click **Save** :</span></span>

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<span data-ttu-id="bd9be-251">索引頁確認變更。</span><span class="sxs-lookup"><span data-stu-id="bd9be-251">The Index page confirms the change.</span></span>

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

<span data-ttu-id="bd9be-253">在第二個視窗中，按一下 **刪除**。</span><span class="sxs-lookup"><span data-stu-id="bd9be-253">In the second window, click **Delete**.</span></span>

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

<span data-ttu-id="bd9be-255">您看到並行處理錯誤訊息和部門值都以目前是在資料庫中重新整理。</span><span class="sxs-lookup"><span data-stu-id="bd9be-255">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="bd9be-257">如果您按一下**刪除**同樣地，您要重新導向，索引頁，會顯示已刪除的部門。</span><span class="sxs-lookup"><span data-stu-id="bd9be-257">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="bd9be-258">總結</span><span class="sxs-lookup"><span data-stu-id="bd9be-258">Summary</span></span>

<span data-ttu-id="bd9be-259">如此即完成處理並行衝突的簡介。</span><span class="sxs-lookup"><span data-stu-id="bd9be-259">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="bd9be-260">其他的方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)和[屬性值使用](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)Entity Framework 小組部落格上。</span><span class="sxs-lookup"><span data-stu-id="bd9be-260">For information about other ways to handle various concurrency scenarios, see [Optimistic Concurrency Patterns](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) and [Working with Property Values](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) on the Entity Framework team blog.</span></span> <span data-ttu-id="bd9be-261">下一個教學課程示範如何實作的每個階層的資料表繼承`Instructor`和`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="bd9be-261">The next tutorial shows how to implement table-per-hierarchy inheritance for the `Instructor` and `Student` entities.</span></span>

<span data-ttu-id="bd9be-262">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="bd9be-262">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bd9be-263">[上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="bd9be-263">[Previous](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
