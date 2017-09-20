---
title: "使用 EF 核心並行-8 10 個 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。"
keywords: "ASP.NET Core，Entity Framework Core，並行存取"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: fc6b218034183a9153c1ef22c99d920a942d2d09
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="75f04-104">處理並行衝突的 EF Core 與 ASP.NET Core MVC 教學課程 (10-8)</span><span class="sxs-lookup"><span data-stu-id="75f04-104">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="75f04-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75f04-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="75f04-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="75f04-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="75f04-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="75f04-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="75f04-108">在先前的教學課程中，您將學會如何更新資料。</span><span class="sxs-lookup"><span data-stu-id="75f04-108">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="75f04-109">本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。</span><span class="sxs-lookup"><span data-stu-id="75f04-109">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="75f04-110">您要建立網頁，以使用 Department 實體，以及處理的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="75f04-110">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="75f04-111">下圖顯示的編輯和刪除的頁面，包括發生並行衝突時，會顯示一些訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-111">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![部門編輯頁面](concurrency/_static/edit-error.png)

![部門刪除頁面](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="75f04-114">並行衝突</span><span class="sxs-lookup"><span data-stu-id="75f04-114">Concurrency conflicts</span></span>

<span data-ttu-id="75f04-115">當一位使用者才能加以編輯，顯示實體的資料，然後另一位使用者更新相同的實體資料時的第一個使用者變更寫入資料庫之前，就會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="75f04-115">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="75f04-116">如果您未啟用的這類的衝突偵測，所有人員都更新資料庫最後會覆寫其他使用者的變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-116">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="75f04-117">在許多應用程式，這項風險是可接受： 如果有少數的使用者或一些更新，或如果不是最關鍵的某些變更會覆寫時，如果並行程式設計的成本可能會超過的好處。</span><span class="sxs-lookup"><span data-stu-id="75f04-117">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="75f04-118">在此情況下，您不需要設定應用程式處理並行存取衝突。</span><span class="sxs-lookup"><span data-stu-id="75f04-118">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="75f04-119">封閉式並行存取 （鎖定）</span><span class="sxs-lookup"><span data-stu-id="75f04-119">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="75f04-120">如果您的應用程式需要以防止資料意外遺失並行案例中的，方法之一是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="75f04-120">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="75f04-121">這就叫做封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="75f04-121">This is called pessimistic concurrency.</span></span> <span data-ttu-id="75f04-122">例如，從資料庫讀取資料列之前，您要求的鎖定唯讀或 update 存取權。</span><span class="sxs-lookup"><span data-stu-id="75f04-122">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="75f04-123">如果您鎖定的資料列進行 update 存取權，允許任何其他使用者鎖定資料列的唯讀或更新存取權，因為他們會取得正在變更的資料副本。</span><span class="sxs-lookup"><span data-stu-id="75f04-123">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="75f04-124">如果鎖定為唯讀存取的資料列時，其他人可以唯讀存取權，但不適用於更新也鎖定。</span><span class="sxs-lookup"><span data-stu-id="75f04-124">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="75f04-125">管理鎖定有缺點。</span><span class="sxs-lookup"><span data-stu-id="75f04-125">Managing locks has disadvantages.</span></span> <span data-ttu-id="75f04-126">是相當複雜的程式。</span><span class="sxs-lookup"><span data-stu-id="75f04-126">It can be complex to program.</span></span> <span data-ttu-id="75f04-127">這需要大量的資料庫管理資源，而且它會造成效能問題的應用程式的使用者數目會增加。</span><span class="sxs-lookup"><span data-stu-id="75f04-127">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="75f04-128">基於這些理由，並非所有的資料庫管理系統支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="75f04-128">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="75f04-129">Entity Framework Core 提供的內建支援，與本教學課程不會顯示如何實作它。</span><span class="sxs-lookup"><span data-stu-id="75f04-129">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="75f04-130">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="75f04-130">Optimistic Concurrency</span></span>

<span data-ttu-id="75f04-131">封閉式並行存取另一種是開放式並行存取。</span><span class="sxs-lookup"><span data-stu-id="75f04-131">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="75f04-132">開放式並行存取表示允許發生並行衝突，然後適當地回應如果沒有的話。</span><span class="sxs-lookup"><span data-stu-id="75f04-132">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="75f04-133">例如，Jane 造訪部門編輯網頁，並從 $350,000.00 變更 $0.00 英文部門的預算金額。</span><span class="sxs-lookup"><span data-stu-id="75f04-133">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![將變更為 0 的預算](concurrency/_static/change-budget.png)

<span data-ttu-id="75f04-135">Jane 按一下之前**儲存**，John 造訪相同頁面上，並從 2007 年 9 月 1 日的開始日期 欄位變成 2013 年 9 月 1 日。</span><span class="sxs-lookup"><span data-stu-id="75f04-135">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![變更開始日期至 2013](concurrency/_static/change-date.png)

<span data-ttu-id="75f04-137">Jane 按一下**儲存**第一次，並觀察其瀏覽器會返回索引頁面時變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-137">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![變更為零的預算](concurrency/_static/budget-zero.png)

<span data-ttu-id="75f04-139">然後 John 按**儲存**上仍會顯示 $350,000.00 的預算的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="75f04-139">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="75f04-140">接下來的情況取決於您如何處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="75f04-140">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="75f04-141">某些選項包括：</span><span class="sxs-lookup"><span data-stu-id="75f04-141">Some of the options include the following:</span></span>

* <span data-ttu-id="75f04-142">您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="75f04-142">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="75f04-143">在範例案例中，任何資料將不會遺失，因為兩位使用者已更新不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="75f04-143">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="75f04-144">下一次有人瀏覽英文的部門，他們會看到 Jane 的和 John 的變更-2013 年 9 月 1 日的開始日期和零美元的預算。</span><span class="sxs-lookup"><span data-stu-id="75f04-144">The next time someone browses the English department, they'll see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="75f04-145">這種更新方法可以減少可能會導致資料遺失的衝突數目，但它無法避免資料遺失，如果相同的實體屬性進行競爭的變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-145">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="75f04-146">Entity Framework 是否這種方式運作，端視實作更新程式碼的方式而定。</span><span class="sxs-lookup"><span data-stu-id="75f04-146">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="75f04-147">它通常中並不實用的 web 應用程式，因為它可能需要您維護大量狀態，若要追蹤的所有實體的原始屬性值以及新的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-147">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="75f04-148">維護大量的狀態會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁上 （例如，在隱藏的欄位） 或在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="75f04-148">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="75f04-149">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-149">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="75f04-150">在下一次有人瀏覽英文部門，他們會看到時間 2013 年 9 月 1 日和還原 $350,000.00 值。</span><span class="sxs-lookup"><span data-stu-id="75f04-150">The next time someone browses the English department, they'll see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="75f04-151">這稱為*用戶端獲勝*或*Wins 中的最後一個*案例。</span><span class="sxs-lookup"><span data-stu-id="75f04-151">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="75f04-152">（從用戶端的所有值都優先於什麼是資料存放區中。）所述的簡介本節中，如果您不執行任何撰寫的程式碼的並行處理，這會自動發生。</span><span class="sxs-lookup"><span data-stu-id="75f04-152">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="75f04-153">您可以在資料庫中更新，防止 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-153">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="75f04-154">一般而言，您會顯示錯誤訊息、 顯示他目前的狀態資料，並允許他他仍然想要讓它們重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-154">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="75f04-155">這稱為*存放區 Wins*案例。</span><span class="sxs-lookup"><span data-stu-id="75f04-155">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="75f04-156">（資料存放區的值優先於用戶端所送出的值。）您將在本教學課程實作的儲存區 Wins 的案例。</span><span class="sxs-lookup"><span data-stu-id="75f04-156">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="75f04-157">這個方法會確保使用者正在發出警示，這為什麼不會覆寫任何變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-157">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="75f04-158">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="75f04-158">Detecting concurrency conflicts</span></span>

<span data-ttu-id="75f04-159">您可以藉由處理解決衝突`DbConcurrencyException`Entity Framework 就會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="75f04-159">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="75f04-160">若要知道何時擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="75f04-160">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="75f04-161">因此，您必須設定資料庫和資料模型適當。</span><span class="sxs-lookup"><span data-stu-id="75f04-161">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="75f04-162">啟用衝突偵測的一些選項如下：</span><span class="sxs-lookup"><span data-stu-id="75f04-162">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="75f04-163">在資料庫資料表中，包含可用來判斷當資料列已經變更的追蹤資料行。</span><span class="sxs-lookup"><span data-stu-id="75f04-163">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="75f04-164">接著，您可以設定 Entity Framework 了包含該資料行，在 where 子句的 SQL Update 或 Delete 命令。</span><span class="sxs-lookup"><span data-stu-id="75f04-164">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="75f04-165">追蹤資料行的資料類型通常是`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="75f04-165">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="75f04-166">`rowversion`值是連續的數字已遞增資料列更新一次。</span><span class="sxs-lookup"><span data-stu-id="75f04-166">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="75f04-167">在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行 （原始資料列版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="75f04-167">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="75f04-168">如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此 Update 或 Delete 陳述式找不到資料列更新，因為 Where 子句。</span><span class="sxs-lookup"><span data-stu-id="75f04-168">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="75f04-169">時，Entity Framework 會發現的任何資料列已更新的 Update 或 Delete 命令時 （亦即，受影響的資料列數目為零） 時，它可解譯的並行存取衝突。</span><span class="sxs-lookup"><span data-stu-id="75f04-169">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="75f04-170">設定 Entity Framework 了包含每個資料行的原始值的資料表中的 Where 子句的 Update 和 Delete 命令。</span><span class="sxs-lookup"><span data-stu-id="75f04-170">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="75f04-171">如同第一個選項，如果資料列中的任何項目已變更第一次讀取的資料列，因為 Where 子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。</span><span class="sxs-lookup"><span data-stu-id="75f04-171">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="75f04-172">對於具有許多資料行的資料庫資料表，這種方法可能會導致非常大的 Where 子句，而且可能需要您維護大量的狀態。</span><span class="sxs-lookup"><span data-stu-id="75f04-172">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="75f04-173">如前文所述，維護大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="75f04-173">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="75f04-174">因此這種方法通常不建議，並不在本教學課程使用的方法。</span><span class="sxs-lookup"><span data-stu-id="75f04-174">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="75f04-175">如果您想要實作這個方法來同步存取，您必須將您想要加入追蹤的並行存取實體中的所有非-主索引鍵屬性標記`ConcurrencyCheck`這些屬性。</span><span class="sxs-lookup"><span data-stu-id="75f04-175">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="75f04-176">該變更可讓 Entity Framework 了包含 「 SQL Where 子句的 Update 和 Delete 陳述式中的所有資料行。</span><span class="sxs-lookup"><span data-stu-id="75f04-176">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="75f04-177">在本教學課程的其餘部分，您會加入`rowversion`追蹤 Department 實體的屬性，建立控制器和檢視，並進行測試，確認一切運作正常。</span><span class="sxs-lookup"><span data-stu-id="75f04-177">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="75f04-178">將追蹤的屬性加入 Department 實體</span><span class="sxs-lookup"><span data-stu-id="75f04-178">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="75f04-179">在*Models/Department.cs*，加入名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="75f04-179">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="75f04-180">`Timestamp`屬性會指定此資料行，將會包含在 where 子句的 Update 和 Delete 命令傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="75f04-180">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="75f04-181">該屬性稱為`Timestamp`因為舊版的 SQL Server 使用 SQL`timestamp`資料類型，再將 SQL`rowversion`取代它。</span><span class="sxs-lookup"><span data-stu-id="75f04-181">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="75f04-182">.NET 類型`rowversion`是位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="75f04-182">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="75f04-183">如果您想要使用 fluent API，您可以使用`IsConcurrencyToken`方法 (在*Data/SchoolContext.cs*) 來指定追蹤屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="75f04-183">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="75f04-184">將屬性加入您變更資料庫模型，因此您必須進行其他移轉。</span><span class="sxs-lookup"><span data-stu-id="75f04-184">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="75f04-185">儲存您的變更和建置專案時，，然後在 [命令] 視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="75f04-185">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="75f04-186">建立部門控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="75f04-186">Create a Departments controller and views</span></span>

<span data-ttu-id="75f04-187">如先前般學生、 課程、 和講師，scaffold 部門控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="75f04-187">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Scaffold 部門](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="75f04-189">在*DepartmentsController.cs*檔案中，將所有的四個出現的"FirstMidName"變更成"FullName 」，讓部門系統管理員的下拉式清單將包含講師的完整名稱，而不是最後一個的名稱。</span><span class="sxs-lookup"><span data-stu-id="75f04-189">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="75f04-190">更新部門索引檢視</span><span class="sxs-lookup"><span data-stu-id="75f04-190">Update the Departments Index view</span></span>

<span data-ttu-id="75f04-191">Scaffolding 引擎建立 RowVersion 資料行在索引檢視中，但不應該顯示該欄位。</span><span class="sxs-lookup"><span data-stu-id="75f04-191">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="75f04-192">中的程式碼取代*Views/Departments/Index.cshtml*為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="75f04-192">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="75f04-193">這樣會變更 「 部門 」 標題、 刪除 RowVersion 資料行中，並且系統管理員會顯示完整名稱，而非第一個名稱。</span><span class="sxs-lookup"><span data-stu-id="75f04-193">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="75f04-194">更新部門控制器中的編輯方法</span><span class="sxs-lookup"><span data-stu-id="75f04-194">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="75f04-195">在這兩個 HttpGet`Edit`方法和`Details`方法，加入`AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="75f04-195">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="75f04-196">在 HttpGet`Edit`方法中，新增系統管理員的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="75f04-196">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="75f04-197">將現有的程式碼取代 HttpPost`Edit`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75f04-197">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="75f04-198">程式碼首先會嘗試讀取要更新的部門。</span><span class="sxs-lookup"><span data-stu-id="75f04-198">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="75f04-199">如果`SingleOrDefaultAsync`方法會傳回 null，另一個使用者已刪除的部門。</span><span class="sxs-lookup"><span data-stu-id="75f04-199">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="75f04-200">在此情況下的程式碼會使用已張貼的表單值建立 department 實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-200">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="75f04-201">或者，您不會有重新建立 department 實體，如果您沒有顯示 [部門] 欄位顯示僅為錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-201">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="75f04-202">檢視會儲存原始`RowVersion`隱藏的欄位，而且這個方法中的值會收到在該值`rowVersion`參數。</span><span class="sxs-lookup"><span data-stu-id="75f04-202">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="75f04-203">之前先呼叫`SaveChanges`，您必須將它放在原始`RowVersion`屬性值在`OriginalValues`實體的集合。</span><span class="sxs-lookup"><span data-stu-id="75f04-203">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="75f04-204">當 Entity Framework 建立 SQL UPDATE 命令時，該命令會將包含 WHERE 子句會尋找具有原始的資料列，然後`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="75f04-204">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="75f04-205">如果任何資料列不受到 UPDATE 命令 (沒有資料列的原始`RowVersion`值)，Entity Framework 就會擲回`DbUpdateConcurrencyException`例外狀況。</span><span class="sxs-lookup"><span data-stu-id="75f04-205">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="75f04-206">針對該例外狀況的 catch 區塊中的程式碼取得受影響的 Department 實體具有更新的值從`Entries`例外狀況物件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="75f04-206">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="75f04-207">`Entries`集合將會有一個`EntityEntry`物件。</span><span class="sxs-lookup"><span data-stu-id="75f04-207">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="75f04-208">您可以使用該物件，以取得新的使用者所輸入的值和目前的資料庫值。</span><span class="sxs-lookup"><span data-stu-id="75f04-208">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="75f04-209">程式碼會加入每個資料行具有不同的資料庫值從使用者所輸入的編輯頁面上的自訂錯誤訊息 （只有一個欄位此處為畫面簡潔起見）。</span><span class="sxs-lookup"><span data-stu-id="75f04-209">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="75f04-210">最後，此程式碼設定`RowVersion`值`departmentToUpdate`從資料庫擷取新的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-210">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="75f04-211">這個新`RowVersion`值將會儲存在隱藏的欄位中，每當使用者按一下 編輯 頁面會重新顯示和下一步 時**儲存**，時，會發生因為編輯頁面的顯示會攔截到的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="75f04-211">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="75f04-212">`ModelState.Remove`陳述式是必要的因為`ModelState`具有舊`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="75f04-212">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="75f04-213">在檢視中，`ModelState`欄位會優先於模型屬性的值兩者均存在時的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-213">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="75f04-214">更新部門編輯檢視</span><span class="sxs-lookup"><span data-stu-id="75f04-214">Update the Department Edit view</span></span>

<span data-ttu-id="75f04-215">在*Views/Departments/Edit.cshtml*，進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="75f04-215">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="75f04-216">加入隱藏的欄位，以儲存`RowVersion`屬性值，緊接的隱藏的欄位`DepartmentID`屬性。</span><span class="sxs-lookup"><span data-stu-id="75f04-216">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="75f04-217">下拉式清單中加入 「 選取系統管理員 」 選項。</span><span class="sxs-lookup"><span data-stu-id="75f04-217">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="75f04-218">在 [編輯] 頁面中測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="75f04-218">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="75f04-219">執行應用程式，並移至部門索引頁面。</span><span class="sxs-lookup"><span data-stu-id="75f04-219">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="75f04-220">以滑鼠右鍵按一下**編輯**英文部門並選取超連結**新索引標籤中開啟**，然後按一下 **編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="75f04-220">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="75f04-221">兩個瀏覽器索引標籤現在會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="75f04-221">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="75f04-222">變更第一個瀏覽器索引標籤中的欄位，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="75f04-222">Change a field in the first browser tab and click **Save**.</span></span>

![部門編輯變更後的第 1 頁](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="75f04-224">瀏覽器會顯示已變更值的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="75f04-224">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="75f04-225">變更第二個瀏覽器索引標籤中的欄位。</span><span class="sxs-lookup"><span data-stu-id="75f04-225">Change a field in the second browser tab.</span></span>

![部門編輯變更後的第 2 頁](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="75f04-227">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="75f04-227">Click **Save**.</span></span> <span data-ttu-id="75f04-228">您會看到一則錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="75f04-228">You see an error message:</span></span>

![部門編輯頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="75f04-230">按一下**儲存**一次。</span><span class="sxs-lookup"><span data-stu-id="75f04-230">Click **Save** again.</span></span> <span data-ttu-id="75f04-231">會儲存在第二個瀏覽器索引標籤中所輸入的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-231">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="75f04-232">索引頁出現時，您會看到已儲存的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-232">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="75f04-233">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="75f04-233">Update the Delete page</span></span>

<span data-ttu-id="75f04-234">刪除設定 頁面上，針對 Entity Framework 會偵測並行衝突造成的其他人其他類似的方式編輯部門。</span><span class="sxs-lookup"><span data-stu-id="75f04-234">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="75f04-235">當 HttpGet`Delete`方法會顯示 [確認] 檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="75f04-235">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="75f04-236">值，就可以使用 HttpPost`Delete`使用者確認刪除作業時所呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="75f04-236">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="75f04-237">當 Entity Framework 建立 SQL DELETE 命令時，會包含 WHERE 子句使用原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="75f04-237">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="75f04-238">如果命令結果中零個資料列受影響 （已顯示 [刪除確認] 頁面之後，資料列已變更的意義），則會擲回並行存取例外狀況，以及 HttpGet`Delete`為 true，才能重新顯示的錯誤設定旗標呼叫方法確認頁面，並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-238">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="75f04-239">此外，也可以零個資料列已受到影響，因為資料列已刪除另一位使用者，因此在此情況下不會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-239">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="75f04-240">更新部門控制器中的 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="75f04-240">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="75f04-241">在*DepartmentsController.cs*，取代 HttpGet`Delete`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75f04-241">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="75f04-242">方法會接受選擇性參數，指出頁面是否正在重新顯示之後並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="75f04-242">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="75f04-243">如果這個旗標為 true，部門指定不再存在，它已被其他使用者刪除。</span><span class="sxs-lookup"><span data-stu-id="75f04-243">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="75f04-244">在此情況下，程式碼重新導向至索引頁面。</span><span class="sxs-lookup"><span data-stu-id="75f04-244">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="75f04-245">如果這個旗標為 true，該部門存在，它已由其他使用者變更。</span><span class="sxs-lookup"><span data-stu-id="75f04-245">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="75f04-246">在此情況下，程式碼將錯誤訊息傳送至檢視使用`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="75f04-246">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="75f04-247">取代的程式碼中 HttpPost`Delete`方法 (名為`DeleteConfirmed`) 取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75f04-247">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="75f04-248">在您剛才取代的 scaffold 程式碼，這個方法會接受只記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="75f04-248">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="75f04-249">您已將此參數變更為 Department 實體的執行個體，建立的模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="75f04-249">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="75f04-250">這可讓 EF 存取 RowVersion 屬性值，除了記錄索引鍵。</span><span class="sxs-lookup"><span data-stu-id="75f04-250">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="75f04-251">您也已經變更的動作方法名稱`DeleteConfirmed`至`Delete`。</span><span class="sxs-lookup"><span data-stu-id="75f04-251">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="75f04-252">Scaffold 的程式碼使用了名稱`DeleteConfirmed`給 HttpPost 方法，唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="75f04-252">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="75f04-253">（在 CLR 需要有不同的方法參數的多載的方法）。現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用 HttpPost 和 HttpGet 方法中刪除相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="75f04-253">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="75f04-254">若已刪除的部門，`AnyAsync`方法會傳回 false，而應用程式就會回到索引方法。</span><span class="sxs-lookup"><span data-stu-id="75f04-254">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="75f04-255">如果會攔截到並行處理錯誤，程式碼會重新顯示 [刪除確認] 頁面，並提供旗標，指出它應該會顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="75f04-255">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="75f04-256">更新刪除檢視</span><span class="sxs-lookup"><span data-stu-id="75f04-256">Update the Delete view</span></span>

<span data-ttu-id="75f04-257">在*Views/Departments/Delete.cshtml*，scaffold 的程式碼取代下列程式碼新增的錯誤訊息欄位和 DepartmentID 和 RowVersion 屬性的隱藏的欄位。</span><span class="sxs-lookup"><span data-stu-id="75f04-257">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="75f04-258">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="75f04-258">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="75f04-259">這會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="75f04-259">This makes the following changes:</span></span>

* <span data-ttu-id="75f04-260">新增錯誤訊息之間`h2`和`h3`標題。</span><span class="sxs-lookup"><span data-stu-id="75f04-260">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="75f04-261">取代中的 FullName FirstMidName**管理員**欄位。</span><span class="sxs-lookup"><span data-stu-id="75f04-261">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="75f04-262">移除 RowVersion 的欄位。</span><span class="sxs-lookup"><span data-stu-id="75f04-262">Removes the RowVersion field.</span></span>

* <span data-ttu-id="75f04-263">將隱藏的欄位加入`RowVersion`屬性。</span><span class="sxs-lookup"><span data-stu-id="75f04-263">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="75f04-264">執行應用程式，並移至部門索引頁面。</span><span class="sxs-lookup"><span data-stu-id="75f04-264">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="75f04-265">以滑鼠右鍵按一下**刪除**英文部門並選取超連結**新索引標籤中開啟**，然後在第一個索引標籤中按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="75f04-265">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="75f04-266">在第一個視窗中，變更其中一個值，然後按一下**儲存**:</span><span class="sxs-lookup"><span data-stu-id="75f04-266">In the first window, change one of the values, and click **Save**:</span></span>

![刪除前變更後的部門編輯頁面](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="75f04-268">在第二個索引標籤上，按一下**刪除**。</span><span class="sxs-lookup"><span data-stu-id="75f04-268">In the second tab, click **Delete**.</span></span> <span data-ttu-id="75f04-269">您看到並行處理錯誤訊息和部門值都以目前是在資料庫中重新整理。</span><span class="sxs-lookup"><span data-stu-id="75f04-269">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![與並行錯誤部門刪除確認 頁面](concurrency/_static/delete-error.png)

<span data-ttu-id="75f04-271">如果您按一下**刪除**同樣地，您要重新導向，索引頁，會顯示已刪除的部門。</span><span class="sxs-lookup"><span data-stu-id="75f04-271">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="75f04-272">更新詳細資料，並建立檢視</span><span class="sxs-lookup"><span data-stu-id="75f04-272">Update Details and Create views</span></span>

<span data-ttu-id="75f04-273">您可以選擇性地清除 scaffold 的程式碼，詳細資料，並建立檢視。</span><span class="sxs-lookup"><span data-stu-id="75f04-273">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="75f04-274">中的程式碼取代*Views/Departments/Details.cshtml*刪除 RowVersion 資料行，並顯示系統管理員的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="75f04-274">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="75f04-275">中的程式碼取代*Views/Departments/Create.cshtml*加入下拉式清單中選取的選項。</span><span class="sxs-lookup"><span data-stu-id="75f04-275">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="75f04-276">總結</span><span class="sxs-lookup"><span data-stu-id="75f04-276">Summary</span></span>

<span data-ttu-id="75f04-277">如此即完成處理並行衝突的簡介。</span><span class="sxs-lookup"><span data-stu-id="75f04-277">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="75f04-278">如需如何處理 EF 核心中的並行存取的詳細資訊，請參閱[並行衝突](https://docs.microsoft.com/ef/core/saving/concurrency)。</span><span class="sxs-lookup"><span data-stu-id="75f04-278">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="75f04-279">下一個教學課程會示範如何實作講師及學生實體資料表每個階層繼承。</span><span class="sxs-lookup"><span data-stu-id="75f04-279">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="75f04-280">[上一頁](update-related-data.md)
[下一頁](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="75f04-280">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
