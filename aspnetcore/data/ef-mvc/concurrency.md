---
title: ASP.NET Core MVC 與 EF Core - 並行 - 8/10
author: rick-anderson
description: 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 48aa5a2d47c9d60ab8ac5a25f8f29ce8e462dd61
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---concurrency---8-of-10"></a><span data-ttu-id="d1974-103">ASP.NET Core MVC 與 EF Core - 並行 - 8/10</span><span class="sxs-lookup"><span data-stu-id="d1974-103">ASP.NET Core MVC with EF Core - Concurrency - 8 of 10</span></span>

<span data-ttu-id="d1974-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1974-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d1974-105">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d1974-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="d1974-106">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="d1974-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="d1974-107">在先前的教學課程中，您學會了如何更新資料。</span><span class="sxs-lookup"><span data-stu-id="d1974-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="d1974-108">本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="d1974-109">您會建立操作 Department 實體的網頁，並處理並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1974-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="d1974-110">下列圖例顯示了 [編輯] 和 [刪除] 頁面，包括一些發生並行衝突時會顯示的訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department [編輯] 頁面](concurrency/_static/edit-error.png)

![Department [刪除] 頁面](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="d1974-113">並行衝突</span><span class="sxs-lookup"><span data-stu-id="d1974-113">Concurrency conflicts</span></span>

<span data-ttu-id="d1974-114">當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="d1974-115">若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="d1974-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="d1974-116">在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。</span><span class="sxs-lookup"><span data-stu-id="d1974-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="d1974-117">在此情況下，您便不需要設定應用程式來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="d1974-118">封閉式並行存取 (鎖定)</span><span class="sxs-lookup"><span data-stu-id="d1974-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="d1974-119">若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="d1974-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="d1974-120">這稱之為封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="d1974-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="d1974-121">例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。</span><span class="sxs-lookup"><span data-stu-id="d1974-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="d1974-122">若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。</span><span class="sxs-lookup"><span data-stu-id="d1974-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="d1974-123">若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。</span><span class="sxs-lookup"><span data-stu-id="d1974-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="d1974-124">管理鎖定有幾個缺點。</span><span class="sxs-lookup"><span data-stu-id="d1974-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="d1974-125">其程式可能相當複雜。</span><span class="sxs-lookup"><span data-stu-id="d1974-125">It can be complex to program.</span></span> <span data-ttu-id="d1974-126">這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。</span><span class="sxs-lookup"><span data-stu-id="d1974-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="d1974-127">基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="d1974-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="d1974-128">Entity Framework Core 並未提供內建支援，並且此教學課程也不會教導您如何實作封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="d1974-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="d1974-129">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="d1974-129">Optimistic Concurrency</span></span>

<span data-ttu-id="d1974-130">封閉式並行存取的替代方案便是開放式並行存取。</span><span class="sxs-lookup"><span data-stu-id="d1974-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="d1974-131">開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。</span><span class="sxs-lookup"><span data-stu-id="d1974-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="d1974-132">例如，Jane 造訪了 Department [編輯] 頁面，然後將英文部門的預算金額從美金 $350,000.00 元調整到美金 $0.00 元。</span><span class="sxs-lookup"><span data-stu-id="d1974-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![將預算變更為 0](concurrency/_static/change-budget.png)

<span data-ttu-id="d1974-134">在 Jane 按一下 [儲存] 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。</span><span class="sxs-lookup"><span data-stu-id="d1974-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

<span data-ttu-id="d1974-136">Jana 先按了一下 [儲存]，並且在瀏覽器返回 [索引] 頁面時看到她做出的變更。</span><span class="sxs-lookup"><span data-stu-id="d1974-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![預算已變更為 0](concurrency/_static/budget-zero.png)

<span data-ttu-id="d1974-138">然後 John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="d1974-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="d1974-139">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="d1974-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="d1974-140">一部分選項包括下列項目：</span><span class="sxs-lookup"><span data-stu-id="d1974-140">Some of the options include the following:</span></span>

* <span data-ttu-id="d1974-141">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="d1974-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="d1974-142">在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。</span><span class="sxs-lookup"><span data-stu-id="d1974-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="d1974-143">下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更 -- 開始日期為 2013/9/1，預算為美金 0 元。</span><span class="sxs-lookup"><span data-stu-id="d1974-143">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="d1974-144">這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="d1974-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="d1974-145">Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="d1974-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="d1974-146">通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。</span><span class="sxs-lookup"><span data-stu-id="d1974-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="d1974-147">維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。</span><span class="sxs-lookup"><span data-stu-id="d1974-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="d1974-148">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="d1974-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="d1974-149">下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，且預算的金額已還原到美金 $350,000.00 元。</span><span class="sxs-lookup"><span data-stu-id="d1974-149">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="d1974-150">這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="d1974-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="d1974-151">(所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。</span><span class="sxs-lookup"><span data-stu-id="d1974-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="d1974-152">您可以防止 John 的變更更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d1974-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="d1974-153">一般而言，您會顯示一個錯誤訊息，將資料目前的狀態顯示給他，然後允許他重新套用他所作出的變更 (若他還是要變更的話)。</span><span class="sxs-lookup"><span data-stu-id="d1974-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="d1974-154">這稱為「存放區獲勝 (Store Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="d1974-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="d1974-155">(資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="d1974-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="d1974-156">這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="d1974-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="d1974-157">偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="d1974-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="d1974-158">您可以透過處理 Entity Framework 擲回的 `DbConcurrencyException` 例外狀況來解析衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="d1974-159">若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="d1974-160">因此，您必須適當的設定資料庫及資料模型。</span><span class="sxs-lookup"><span data-stu-id="d1974-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="d1974-161">一部分啟用衝突偵測的選項包括下列選項：</span><span class="sxs-lookup"><span data-stu-id="d1974-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="d1974-162">在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。</span><span class="sxs-lookup"><span data-stu-id="d1974-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="d1974-163">您接著便可以設定 Entity Framework，使其在 SQL Update 或 Delete 命令的 Where 子句中包含該資料行。</span><span class="sxs-lookup"><span data-stu-id="d1974-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="d1974-164">追蹤資料行的資料類型通常是 `rowversion`。</span><span class="sxs-lookup"><span data-stu-id="d1974-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="d1974-165">`rowversion` 的值是一個循序號碼，每一次資料列更新時都會遞增。</span><span class="sxs-lookup"><span data-stu-id="d1974-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="d1974-166">在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行原始的值 (原始的資料列版本)。</span><span class="sxs-lookup"><span data-stu-id="d1974-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="d1974-167">若正在更新的資料列已由其他使用者變更，`rowversion` 中的值便會與原始的值不同，因此 Update 或 Delete 陳述式便會因為 Where 子句而無法找到資料列進行更新。</span><span class="sxs-lookup"><span data-stu-id="d1974-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="d1974-168">當 Entity Framework 發現 Update 或 Delete 命令沒有更新任何資料列時 (即受影響的資料列數目為 0)，它便會將其解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="d1974-169">設定 Entity Framework，使其在 Update 和 Delete 命令中的 Where 子句裡包含資料表中每個資料行原始的值。</span><span class="sxs-lookup"><span data-stu-id="d1974-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="d1974-170">如同第一個選項，若資料列中在一開始讀取之後有任何資料產生變更，Where 子句便不會傳回任何資料列以進行更新，Entity Framework 便會將其解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="d1974-171">針對擁有許多資料行的資料庫資料表，此方法可能導致非常龐大的 Where 子句，並且可能會需要您維持龐大數量的狀態。</span><span class="sxs-lookup"><span data-stu-id="d1974-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="d1974-172">如前文所述，維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="d1974-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="d1974-173">因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="d1974-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="d1974-174">若您想要實作此方法以處理並行衝突，您必須藉由新增 `ConcurrencyCheck` 屬性來標記所有實體中您想要追蹤並行的非主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="d1974-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="d1974-175">該變更會使 Entity Framework 能在 Update 和 Delete 陳述式的 SQL Where 子句中包含所有資料行。</span><span class="sxs-lookup"><span data-stu-id="d1974-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="d1974-176">在本教學課程的其餘部分，您會將一個 `rowversion` 追蹤屬性新增到 Department 實體，建立控制器和檢視，然後進行測試以驗證一切都運作正常。</span><span class="sxs-lookup"><span data-stu-id="d1974-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="d1974-177">將追蹤屬性新增到 Department 實體</span><span class="sxs-lookup"><span data-stu-id="d1974-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="d1974-178">在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="d1974-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="d1974-179">`Timestamp` 屬性會指定此資料行會包含在傳送到資料庫之 Update 和 Delete 命令的 Where 子句中。</span><span class="sxs-lookup"><span data-stu-id="d1974-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="d1974-180">該屬性稱為 `Timestamp`，因為先前版本的 SQL Server 在以 SQL `rowversion` 取代之前使用了 SQL `timestamp` 資料類型。</span><span class="sxs-lookup"><span data-stu-id="d1974-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="d1974-181">`rowversion` 的 .NET 類型為位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="d1974-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="d1974-182">若您偏好使用 Fluent API，您可以使用 `IsConcurrencyToken` 方法 (位於 *Data/SchoolContext.cs* 中) 來指定追蹤屬性，如以下範例所示：</span><span class="sxs-lookup"><span data-stu-id="d1974-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="d1974-183">由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。</span><span class="sxs-lookup"><span data-stu-id="d1974-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="d1974-184">儲存您的變更並建置專案，然後在命令視窗中輸入以下命令：</span><span class="sxs-lookup"><span data-stu-id="d1974-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="d1974-185">建立 Departments 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="d1974-185">Create a Departments controller and views</span></span>

<span data-ttu-id="d1974-186">Scaffold Departments 控制器和檢視，如同您先前為 Students、Courses 和 Instructors 所做的。</span><span class="sxs-lookup"><span data-stu-id="d1974-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Scaffold Department](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="d1974-188">在  *DepartmentsController.cs* 檔案中，將四個 "FirstMidName" 變更為 "FullName" ，使部門系統管理員下拉式清單可包含講師的完整名稱，而非只有姓氏。</span><span class="sxs-lookup"><span data-stu-id="d1974-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="d1974-189">更新 Departments [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="d1974-189">Update the Departments Index view</span></span>

<span data-ttu-id="d1974-190">Scaffolding 引擎會在 [索引] 檢視中建立 RowVersion 資料行，但該欄位不應該顯示出來。</span><span class="sxs-lookup"><span data-stu-id="d1974-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="d1974-191">請以下列程式碼取代 *Views/Departments/Index.cshtml* 中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1974-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="d1974-192">這會將標題變更為"Departments"，刪除 RowVersion 資料行，並為系統管理員顯示完整的名稱而非只有名字。</span><span class="sxs-lookup"><span data-stu-id="d1974-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="d1974-193">更新 Departments 控制器中的 Edit 方法</span><span class="sxs-lookup"><span data-stu-id="d1974-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="d1974-194">在 HttpGet `Edit` 方法和 `Details` 方法中，新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="d1974-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="d1974-195">在 HttpGet `Edit` 方法中，為系統管理員新增積極式載入。</span><span class="sxs-lookup"><span data-stu-id="d1974-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="d1974-196">以下列程式碼取代 HttpPost `Edit` 方法的現有程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1974-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="d1974-197">程式碼開始時便會嘗試讀取要更新的部門。</span><span class="sxs-lookup"><span data-stu-id="d1974-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="d1974-198">若 `SingleOrDefaultAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。</span><span class="sxs-lookup"><span data-stu-id="d1974-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="d1974-199">在此情況下，程式碼會使用 POST 表單的值建立部門實體，使 [編輯] 頁面仍然可以重新顯示，並加上錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="d1974-200">或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。</span><span class="sxs-lookup"><span data-stu-id="d1974-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="d1974-201">檢視會在隱藏欄位中儲存原始的 `RowVersion` 值，並且此方法會在 `rowVersion` 參數中接收該值。</span><span class="sxs-lookup"><span data-stu-id="d1974-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="d1974-202">在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。</span><span class="sxs-lookup"><span data-stu-id="d1974-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="d1974-203">則當 Entity Framework 建立 SQL UPDATE 命令時，該命令便會包含尋找擁有原始 `RowVersion` 值之資料列的 WHERE 子句。</span><span class="sxs-lookup"><span data-stu-id="d1974-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="d1974-204">若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則 Entity Framework 便會擲回 `DbUpdateConcurrencyException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d1974-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="d1974-205">針對該例外狀況的 catch 區塊會取得受影響的 Department 實體。該實體具有從例外物件上的 `Entries` 屬性取得的更新值。</span><span class="sxs-lookup"><span data-stu-id="d1974-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="d1974-206">`Entries` 集合只會有一個 `EntityEntry` 物件。</span><span class="sxs-lookup"><span data-stu-id="d1974-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="d1974-207">您可以使用該物件來取得由使用者輸入的新值，以及目前資料庫的值。</span><span class="sxs-lookup"><span data-stu-id="d1974-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="d1974-208">程式碼會為每個資料庫的值與使用者在編輯頁面上輸入的值不同的資料行新增一個自訂錯誤訊息 (為了簡化，這裡只顯示了一個欄位)。</span><span class="sxs-lookup"><span data-stu-id="d1974-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="d1974-209">最後，程式碼會將 `departmentToUpdate` 的 `RowVersion` 值設為從資料庫取得的新值。</span><span class="sxs-lookup"><span data-stu-id="d1974-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="d1974-210">這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="d1974-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="d1974-211">`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="d1974-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="d1974-212">在檢視中，當兩者同時存在時，欄位的 `ModelState` 值會優先於模型屬性值。</span><span class="sxs-lookup"><span data-stu-id="d1974-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="d1974-213">更新 Department [編輯] 檢視</span><span class="sxs-lookup"><span data-stu-id="d1974-213">Update the Department Edit view</span></span>

<span data-ttu-id="d1974-214">在 *Views/Departments/Edit.cshtml* 中，進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="d1974-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="d1974-215">在 `DepartmentID`  屬性的隱藏欄位之後立即新增另一個隱藏欄位以儲存 `RowVersion` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="d1974-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="d1974-216">將 [選取系統管理員] 選項新增到下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="d1974-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="d1974-217">在 [編輯] 頁面上測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="d1974-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="d1974-218">執行應用程式，移至 Departments [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d1974-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="d1974-219">以滑鼠右鍵按一下 English 部門的**編輯** 超連結，然後選取 [開啟新的索引標籤]，然後按一下 English 部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="d1974-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="d1974-220">兩個瀏覽器索引標籤現在會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="d1974-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="d1974-221">變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="d1974-221">Change a field in the first browser tab and click **Save**.</span></span>

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="d1974-223">瀏覽器會顯示索引頁面，當中包含了變更之後的值。</span><span class="sxs-lookup"><span data-stu-id="d1974-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="d1974-224">變更第二個瀏覽器索引標籤中的欄位。</span><span class="sxs-lookup"><span data-stu-id="d1974-224">Change a field in the second browser tab.</span></span>

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="d1974-226">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="d1974-226">Click **Save**.</span></span> <span data-ttu-id="d1974-227">您會看到一個錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="d1974-227">You see an error message:</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="d1974-229">再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="d1974-229">Click **Save** again.</span></span> <span data-ttu-id="d1974-230">您在第二個瀏覽器索引標籤中輸入的值已儲存。</span><span class="sxs-lookup"><span data-stu-id="d1974-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="d1974-231">您會在索引頁面出現時看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="d1974-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d1974-232">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="d1974-232">Update the Delete page</span></span>

<span data-ttu-id="d1974-233">針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。</span><span class="sxs-lookup"><span data-stu-id="d1974-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="d1974-234">當 HttpGet `Delete` 方法顯示確認檢視時，檢視會在隱藏欄位中包含原始的 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="d1974-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="d1974-235">該值接著便可供當使用者確認刪除時所呼叫的 HttpPost `Delete` 方法使用。</span><span class="sxs-lookup"><span data-stu-id="d1974-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="d1974-236">當 Entity Framework 建立 SQL DELETE 命令時，它會包含一個具有原始 `RowVersion` 值的 WHERE 子句。</span><span class="sxs-lookup"><span data-stu-id="d1974-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="d1974-237">若命令執行的結果為受影響的資料列為零 (即資料列在 [刪除] 確認頁面顯示後發生變更)，便會擲回並行衝突例外狀況，並呼叫 HttpGet `Delete` 方法，並將錯誤旗標設為 true，以在重新顯示的確認頁面上顯示一個錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="d1974-238">當受影響的資料列為零時，也有可能是該資料列已由別的使用者刪除；在這種情況下，將不會顯示任何錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="d1974-239">更新 Departments 控制器中的 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="d1974-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="d1974-240">在 *DepartmentsController.cs* 中，以下列程式碼取代 HttpGet `Delete` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1974-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="d1974-241">方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。</span><span class="sxs-lookup"><span data-stu-id="d1974-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="d1974-242">若此旗標為 true，且指定的部門已不存在，表示該部門已遭其他使用者刪除。</span><span class="sxs-lookup"><span data-stu-id="d1974-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="d1974-243">在此情況下，程式碼會重新導向至索引頁面。</span><span class="sxs-lookup"><span data-stu-id="d1974-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="d1974-244">若此旗標為 true，但指定的部門仍然存在，表示該部門已遭其他使用者變更。</span><span class="sxs-lookup"><span data-stu-id="d1974-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="d1974-245">在此情況下，程式碼會使用 `ViewData` 傳送一個錯誤訊息到檢視。</span><span class="sxs-lookup"><span data-stu-id="d1974-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="d1974-246">以下列程式碼取代 HttpPost `Delete` 方法 (名為 `DeleteConfirmed`) 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d1974-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="d1974-247">在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：</span><span class="sxs-lookup"><span data-stu-id="d1974-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="d1974-248">您已將此參數變更為由模型繫結器建立的 Department 實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="d1974-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="d1974-249">這可讓 EF 除了記錄金鑰外，也能存取 RowVersion 屬性值。</span><span class="sxs-lookup"><span data-stu-id="d1974-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="d1974-250">您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。</span><span class="sxs-lookup"><span data-stu-id="d1974-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="d1974-251">Scaffold 程式碼使用了 `DeleteConfirmed` 的名稱來給予 HttpPost 方法一個唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="d1974-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="d1974-252">(CLR 要求多載方法必須要有不同的方法參數。)現在，該簽章已為唯一的簽章，您現在可以遵循 MVC 慣例，然後為 HttpPost 及 HttpGet 刪除方法使用相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="d1974-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="d1974-253">若部門已遭刪除，`AnyAsync` 方法會傳回 false，應用程式便會直接返回 Index 方法。</span><span class="sxs-lookup"><span data-stu-id="d1974-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="d1974-254">若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="d1974-255">更新 [刪除] 檢視</span><span class="sxs-lookup"><span data-stu-id="d1974-255">Update the Delete view</span></span>

<span data-ttu-id="d1974-256">在 *Views/Departments/Delete.cshtml* 中，以下列新增錯誤訊息欄位和 DepartmentID 及 RowVersion 屬性隱藏欄位的程式碼取代 Scaffold 程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1974-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="d1974-257">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="d1974-257">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="d1974-258">這會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="d1974-258">This makes the following changes:</span></span>

* <span data-ttu-id="d1974-259">在 `h2` 和 `h3` 標題之間新增一個錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d1974-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="d1974-260">在 [系統管理員] 欄位中將 FirstMidName 取代為 FullName。</span><span class="sxs-lookup"><span data-stu-id="d1974-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="d1974-261">移除 RowVersion 欄位。</span><span class="sxs-lookup"><span data-stu-id="d1974-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="d1974-262">為 `RowVersion` 屬性新增一個隱藏欄位。</span><span class="sxs-lookup"><span data-stu-id="d1974-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="d1974-263">執行應用程式，移至 Departments [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d1974-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="d1974-264">以滑鼠右鍵按一下 English 部門的**刪除** 超連結，然後選取 [開啟新的索引標籤]，然後在第一個索引標籤中按一下 English 部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="d1974-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="d1974-265">在第一個視窗中，變更其中一個值，然後按一下 [儲存]：</span><span class="sxs-lookup"><span data-stu-id="d1974-265">In the first window, change one of the values, and click **Save**:</span></span>

![刪除前變更後的 Department [編輯] 頁面](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="d1974-267">在第二個索引標籤中，按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="d1974-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="d1974-268">您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。</span><span class="sxs-lookup"><span data-stu-id="d1974-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department [刪除] 確認頁面，其中包含了並行錯誤](concurrency/_static/delete-error.png)

<span data-ttu-id="d1974-270">若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。</span><span class="sxs-lookup"><span data-stu-id="d1974-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="d1974-271">更新 [詳細資料] 及 [建立] 檢視</span><span class="sxs-lookup"><span data-stu-id="d1974-271">Update Details and Create views</span></span>

<span data-ttu-id="d1974-272">您也可以選擇性的清理 [詳細資料] 和 [建立] 檢視中的 Scaffold 程式碼。</span><span class="sxs-lookup"><span data-stu-id="d1974-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="d1974-273">取代 *Views/Departments/Details.cshtml* 中的程式碼以刪除 RowVersion 資料行並顯示系統管理員的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="d1974-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="d1974-274">取代 *Views/Departments/Create.cshtml* 中的程式碼來將 [選取 ] 選項新增到下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="d1974-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="d1974-275">總結</span><span class="sxs-lookup"><span data-stu-id="d1974-275">Summary</span></span>

<span data-ttu-id="d1974-276">如此即完成了處理並行衝突的簡介。</span><span class="sxs-lookup"><span data-stu-id="d1974-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="d1974-277">如需如何在 EF Core 中處理並行的詳細資訊，請參閱[並行衝突](https://docs.microsoft.com/ef/core/saving/concurrency)。</span><span class="sxs-lookup"><span data-stu-id="d1974-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="d1974-278">下一個教學課程會顯示如何為 Instructor 和 Student 實體實作每個階層資料表的繼承。</span><span class="sxs-lookup"><span data-stu-id="d1974-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1974-279">[上一頁](update-related-data.md)
> [下一頁](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="d1974-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
