---
title: "使用 EF 核心並行-8 8 的 razor 頁面"
author: rick-anderson
description: "本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。"
keywords: "ASP.NET Core，Entity Framework Core，並行存取"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 841c638b2cacaab7970f2b173fee488972957b63
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2018
---
<span data-ttu-id="44971-104">en-我們 /</span><span class="sxs-lookup"><span data-stu-id="44971-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="44971-105">處理並行存取衝突-Razor 頁面 (8 個 8) 使用的 EF 核心</span><span class="sxs-lookup"><span data-stu-id="44971-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="44971-106">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，和[Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="44971-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="44971-107">本教學課程會示範如何處理衝突，當多位使用者同時 （在相同的時間） 更新實體。</span><span class="sxs-lookup"><span data-stu-id="44971-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="44971-108">如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。</span><span class="sxs-lookup"><span data-stu-id="44971-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="44971-109">並行衝突</span><span class="sxs-lookup"><span data-stu-id="44971-109">Concurrency conflicts</span></span>

<span data-ttu-id="44971-110">發生並行衝突時：</span><span class="sxs-lookup"><span data-stu-id="44971-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="44971-111">使用者巡覽至實體的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="44971-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="44971-112">第一個使用者的變更寫入資料庫之前，另一位使用者更新相同的實體。</span><span class="sxs-lookup"><span data-stu-id="44971-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="44971-113">如果未啟用並行偵測，並行更新時：</span><span class="sxs-lookup"><span data-stu-id="44971-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="44971-114">最後的更新成功。</span><span class="sxs-lookup"><span data-stu-id="44971-114">The last update wins.</span></span> <span data-ttu-id="44971-115">也就是上次更新的值會儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="44971-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="44971-116">目前的更新中的第一個會遺失。</span><span class="sxs-lookup"><span data-stu-id="44971-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="44971-117">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="44971-117">Optimistic concurrency</span></span>

<span data-ttu-id="44971-118">開放式並行存取可讓發生並行衝突，然後回應適當當它們執行。</span><span class="sxs-lookup"><span data-stu-id="44971-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="44971-119">例如，Jane 造訪部門編輯頁面，並從 $350,000.00 變更 $0.00 英文部門預算。</span><span class="sxs-lookup"><span data-stu-id="44971-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![將變更為 0 的預算](concurrency/_static/change-budget.png)

<span data-ttu-id="44971-121">Jane 按一下之前**儲存**，John 造訪相同頁面上，並從 2007 年 9 月 1 日的開始日期 欄位變成 2013 年 9 月 1 日。</span><span class="sxs-lookup"><span data-stu-id="44971-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![變更開始日期至 2013](concurrency/_static/change-date.png)

<span data-ttu-id="44971-123">Jane 按一下**儲存**第一次，並觀察其瀏覽器顯示的索引頁面時變更。</span><span class="sxs-lookup"><span data-stu-id="44971-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![變更為零的預算](concurrency/_static/budget-zero.png)

<span data-ttu-id="44971-125">John 按**儲存**上仍會顯示 $350,000.00 的預算的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="44971-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="44971-126">接下來的情況取決於您如何處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="44971-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="44971-127">開放式並行存取包含下列選項：</span><span class="sxs-lookup"><span data-stu-id="44971-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="44971-128">您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="44971-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="44971-129">在案例中，將不會遺失任何資料。</span><span class="sxs-lookup"><span data-stu-id="44971-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="44971-130">兩位使用者已更新不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="44971-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="44971-131">下一次有人瀏覽英文的部門，他們會看到 Jane 的和 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="44971-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="44971-132">這種更新方法可以減少可能會導致資料遺失的衝突數目。</span><span class="sxs-lookup"><span data-stu-id="44971-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="44971-133">這種方法: * 無法避免資料遺失，如果競爭變更至相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="44971-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="44971-134">* 是在 web 應用程式通常不實用。</span><span class="sxs-lookup"><span data-stu-id="44971-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="44971-135">它需要維護重要狀態來追蹤感所有擷取的值和新值。</span><span class="sxs-lookup"><span data-stu-id="44971-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="44971-136">維護大量狀態，可能會影響應用程式效能。</span><span class="sxs-lookup"><span data-stu-id="44971-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="44971-137">* 可以提高應用程式複雜度相較於實體上的並行偵測。</span><span class="sxs-lookup"><span data-stu-id="44971-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="44971-138">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="44971-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="44971-139">在下一次有人瀏覽英文部門，他們會看到時間 2013 年 9 月 1 日和 $350,000.00 將擷取的值。</span><span class="sxs-lookup"><span data-stu-id="44971-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="44971-140">這種方法稱為*用戶端獲勝*或*Wins 中的最後一個*案例。</span><span class="sxs-lookup"><span data-stu-id="44971-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="44971-141">（從用戶端的所有值都優先於什麼是資料存放區中。）如果您沒有進行任何撰寫的程式碼的並行處理，Wins 用戶端會自動發生。</span><span class="sxs-lookup"><span data-stu-id="44971-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="44971-142">您可以在資料庫中更新，防止 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="44971-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="44971-143">一般而言，應用程式會: * 顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="44971-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="44971-144">* 顯示資料的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="44971-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="44971-145">* 允許使用者重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="44971-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="44971-146">這稱為*存放區 Wins*案例。</span><span class="sxs-lookup"><span data-stu-id="44971-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="44971-147">（資料存放區的值優先於用戶端所送出的值。）您在本教學課程實作的存放區 Wins 的案例。</span><span class="sxs-lookup"><span data-stu-id="44971-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="44971-148">這個方法可確保任何變更會覆寫沒有警示使用者。</span><span class="sxs-lookup"><span data-stu-id="44971-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="44971-149">處理並行</span><span class="sxs-lookup"><span data-stu-id="44971-149">Handling concurrency</span></span> 

<span data-ttu-id="44971-150">當屬性設定為[並行語彙基元](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="44971-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="44971-151">EF 核心確認屬性有未被修改它提取之後。</span><span class="sxs-lookup"><span data-stu-id="44971-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="44971-152">不會進行檢查時[SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)或[SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)呼叫。</span><span class="sxs-lookup"><span data-stu-id="44971-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="44971-153">如果屬性已變更，則提取之後, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)就會擲回。</span><span class="sxs-lookup"><span data-stu-id="44971-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="44971-154">資料庫和資料模型必須設定為支援擲回`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="44971-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="44971-155">偵測並行衝突的屬性</span><span class="sxs-lookup"><span data-stu-id="44971-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="44971-156">在屬性層級的偵測並行衝突[ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)屬性。</span><span class="sxs-lookup"><span data-stu-id="44971-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="44971-157">屬性可以套用到模型上的多個屬性。</span><span class="sxs-lookup"><span data-stu-id="44971-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="44971-158">如需詳細資訊，請參閱[資料註解 ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations)。</span><span class="sxs-lookup"><span data-stu-id="44971-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="44971-159">`[ConcurrencyCheck]`屬性不是在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="44971-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="44971-160">偵測並行衝突的資料列</span><span class="sxs-lookup"><span data-stu-id="44971-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="44971-161">若要偵測並行衝突[rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)追蹤資料行加入至模型。</span><span class="sxs-lookup"><span data-stu-id="44971-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="44971-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="44971-162">`rowversion` :</span></span>

* <span data-ttu-id="44971-163">為特定的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="44971-163">Is SQL Server specific.</span></span> <span data-ttu-id="44971-164">其他資料庫可能不會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="44971-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="44971-165">用來判斷該實體不之後已經變更它已從 DB 擷取。</span><span class="sxs-lookup"><span data-stu-id="44971-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="44971-166">資料庫會產生循序`rowversion`已遞增一次資料列的數目會更新。</span><span class="sxs-lookup"><span data-stu-id="44971-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="44971-167">在`Update`或`Delete`命令，`Where`子句包含擷取的值的`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="44971-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="44971-168">如果正在更新的資料列已經變更：</span><span class="sxs-lookup"><span data-stu-id="44971-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="44971-169">`rowversion`不符合已擷取的值。</span><span class="sxs-lookup"><span data-stu-id="44971-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="44971-170">`Update`或`Delete`命令找不到一個資料列因為`Where`子句包含擷取`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="44971-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="44971-171">A`DbUpdateConcurrencyException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="44971-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="44971-172">在 EF 核心，當沒有任何資料列已更新`Update`或`Delete`命令時，擲回並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="44971-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="44971-173">將追蹤的屬性加入 Department 實體</span><span class="sxs-lookup"><span data-stu-id="44971-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="44971-174">在*Models/Department.cs*，加入名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="44971-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="44971-175">[時間戳記](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)屬性會指定此資料行，會包含在`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="44971-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="44971-176">該屬性稱為`Timestamp`因為舊版的 SQL Server 使用 SQL`timestamp`資料類型，再將 SQL`rowversion`類型取代它。</span><span class="sxs-lookup"><span data-stu-id="44971-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="44971-177">關於 fluent API 也可以指定追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="44971-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="44971-178">下列程式碼會顯示 T-SQL 時就更新該部門名稱產生的 EF 核心部分：</span><span class="sxs-lookup"><span data-stu-id="44971-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="44971-179">上述反白顯示程式碼所示`WHERE`子句，其中包含`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="44971-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="44971-180">如果 DB`RowVersion`不等於`RowVersion`參數 (`@p2`)，會更新任何資料列。</span><span class="sxs-lookup"><span data-stu-id="44971-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="44971-181">下列反白顯示的程式碼顯示，用來驗證正好一個資料列已更新 T-SQL:</span><span class="sxs-lookup"><span data-stu-id="44971-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="44971-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql)傳回最後一個陳述式所影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="44971-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="44971-183">在不會更新資料列，會擲回 EF 核心`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="44971-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="44971-184">您可以看到 T-SQL EF 核心會產生的 Visual Studio [輸出] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="44971-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="44971-185">更新 DB</span><span class="sxs-lookup"><span data-stu-id="44971-185">Update the DB</span></span>

<span data-ttu-id="44971-186">加入`RowVersion`屬性變更需要移轉的資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="44971-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="44971-187">建置專案。</span><span class="sxs-lookup"><span data-stu-id="44971-187">Build the project.</span></span> <span data-ttu-id="44971-188">在命令視窗中輸入下列：</span><span class="sxs-lookup"><span data-stu-id="44971-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="44971-189">上述命令中：</span><span class="sxs-lookup"><span data-stu-id="44971-189">The preceding commands:</span></span>

* <span data-ttu-id="44971-190">新增*移轉 / {時間 stamp}_RowVersion.cs*移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="44971-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="44971-191">更新*Migrations/SchoolContextModelSnapshot.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="44971-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="44971-192">更新會加入下列反白顯示的程式碼加入`BuildModel`方法：</span><span class="sxs-lookup"><span data-stu-id="44971-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="44971-193">執行移轉，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="44971-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="44971-194">Scaffold 部門模型</span><span class="sxs-lookup"><span data-stu-id="44971-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="44971-195">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="44971-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="44971-196">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="44971-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="44971-197">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="44971-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="44971-198">上述命令 scaffold`Department`模型。</span><span class="sxs-lookup"><span data-stu-id="44971-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="44971-199">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="44971-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="44971-200">建置專案。</span><span class="sxs-lookup"><span data-stu-id="44971-200">Build the project.</span></span> <span data-ttu-id="44971-201">建置會產生類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="44971-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="44971-202">全域變更`_context.Department`至`_context.Departments`(亦即，加入"s" `Department`)。</span><span class="sxs-lookup"><span data-stu-id="44971-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="44971-203">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="44971-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="44971-204">更新部門索引頁</span><span class="sxs-lookup"><span data-stu-id="44971-204">Update the Departments Index page</span></span>

<span data-ttu-id="44971-205">建立的 scaffolding 引擎`RowVersion`不應該顯示的索引 頁面上，但該欄位的資料行。</span><span class="sxs-lookup"><span data-stu-id="44971-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="44971-206">在本教學課程的最後一個位元組`RowVersion`顯示有助於了解並行存取。</span><span class="sxs-lookup"><span data-stu-id="44971-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="44971-207">最後一個位元組不保證是唯一的。</span><span class="sxs-lookup"><span data-stu-id="44971-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="44971-208">實際的應用程式不會顯示`RowVersion`或最後一個位元組`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="44971-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="44971-209">更新索引頁：</span><span class="sxs-lookup"><span data-stu-id="44971-209">Update the Index page:</span></span>

* <span data-ttu-id="44971-210">取代部門中的索引。</span><span class="sxs-lookup"><span data-stu-id="44971-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="44971-211">取代標記包含`RowVersion`與最後一個位元組`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="44971-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="44971-212">FirstMidName 取代 FullName。</span><span class="sxs-lookup"><span data-stu-id="44971-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="44971-213">下列標記會顯示 [更新] 頁面：</span><span class="sxs-lookup"><span data-stu-id="44971-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="44971-214">更新頁面編輯模型</span><span class="sxs-lookup"><span data-stu-id="44971-214">Update the Edit page model</span></span>

<span data-ttu-id="44971-215">更新*pages\departments\edit.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="44971-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="44971-216">若要偵測的並行存取問題， [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)以更新`rowVersion`從它已擷取之實體的值。</span><span class="sxs-lookup"><span data-stu-id="44971-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="44971-217">EF 核心產生 SQL UPDATE 命令搭配 WHERE 子句，其中包含原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="44971-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="44971-218">如果任何資料列不受到 UPDATE 命令 (沒有資料列的原始`RowVersion`值)、`DbUpdateConcurrencyException`擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="44971-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="44971-219">在上述程式碼，`Department.RowVersion`是時已擷取之實體的值。</span><span class="sxs-lookup"><span data-stu-id="44971-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="44971-220">`OriginalValue`DB 中的值時`FirstOrDefaultAsync`呼叫這個方法中。</span><span class="sxs-lookup"><span data-stu-id="44971-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="44971-221">下列程式碼會取得用戶端值 （張貼至這個方法的值） 和 DB 值：</span><span class="sxs-lookup"><span data-stu-id="44971-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="44971-222">下列程式碼會加入自訂錯誤訊息，針對每個資料行具有 DB 值不同於什麼公佈到`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="44971-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="44971-223">下列反白顯示的程式碼集`RowVersion`從 DB 擷取新的值的值。</span><span class="sxs-lookup"><span data-stu-id="44971-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="44971-224">使用者按一下 下一次**儲存**，時，會發生因為攔截到最後一個顯示的編輯頁面的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="44971-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="44971-225">`ModelState.Remove`陳述式是必要的因為`ModelState`具有舊`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="44971-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="44971-226">在 Razor 頁面中，`ModelState`欄位會優先於模型屬性的值兩者均存在時的值。</span><span class="sxs-lookup"><span data-stu-id="44971-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="44971-227">更新編輯頁面</span><span class="sxs-lookup"><span data-stu-id="44971-227">Update the Edit page</span></span>

<span data-ttu-id="44971-228">更新*Pages/Departments/Edit.cshtml*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="44971-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="44971-229">上述的標記：</span><span class="sxs-lookup"><span data-stu-id="44971-229">The preceding markup:</span></span>

* <span data-ttu-id="44971-230">更新`page`指示詞從`@page`至`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="44971-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="44971-231">將隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="44971-231">Adds a hidden row version.</span></span> <span data-ttu-id="44971-232">`RowVersion`必須讓回傳值繫結加入。</span><span class="sxs-lookup"><span data-stu-id="44971-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="44971-233">顯示的最後一個位元組`RowVersion`以進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="44971-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="44971-234">取代`ViewData`與強型別`InstructorNameSL`。</span><span class="sxs-lookup"><span data-stu-id="44971-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="44971-235">測試並行衝突的編輯頁面</span><span class="sxs-lookup"><span data-stu-id="44971-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="44971-236">開啟兩個瀏覽器上的執行個體編輯英文部門：</span><span class="sxs-lookup"><span data-stu-id="44971-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="44971-237">執行應用程式並選取部門。</span><span class="sxs-lookup"><span data-stu-id="44971-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="44971-238">以滑鼠右鍵按一下**編輯**英文部門並選取超連結**新索引標籤中開啟**。</span><span class="sxs-lookup"><span data-stu-id="44971-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="44971-239">在第一個索引標籤上，按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="44971-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="44971-240">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="44971-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="44971-241">變更第一個瀏覽器索引標籤中的名稱，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="44971-241">Change the name in the first browser tab and click **Save**.</span></span>

![部門編輯變更後的第 1 頁](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="44971-243">瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="44971-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="44971-244">請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。</span><span class="sxs-lookup"><span data-stu-id="44971-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="44971-245">變更第二個瀏覽器索引標籤中的不同欄位。</span><span class="sxs-lookup"><span data-stu-id="44971-245">Change a different field in the second browser tab.</span></span>

![部門編輯變更後的第 2 頁](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="44971-247">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="44971-247">Click **Save**.</span></span> <span data-ttu-id="44971-248">您看到錯誤訊息不相符的 DB 值的所有欄位：</span><span class="sxs-lookup"><span data-stu-id="44971-248">You see error messages for all fields that don't match the DB values:</span></span>

![部門編輯頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="44971-250">此瀏覽器視窗中不想要變更 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="44971-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="44971-251">複製並貼入 [名稱] 欄位中的目前值 （語言）。</span><span class="sxs-lookup"><span data-stu-id="44971-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="44971-252">索引標籤時。用戶端驗證移除錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="44971-252">Tab out. Client-side validation removes the error message.</span></span>

![部門編輯頁面錯誤訊息](concurrency/_static/cv.png)

<span data-ttu-id="44971-254">按一下**儲存**一次。</span><span class="sxs-lookup"><span data-stu-id="44971-254">Click **Save** again.</span></span> <span data-ttu-id="44971-255">會儲存在第二個瀏覽器索引標籤中所輸入的值。</span><span class="sxs-lookup"><span data-stu-id="44971-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="44971-256">您看到儲存的索引頁中的值。</span><span class="sxs-lookup"><span data-stu-id="44971-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="44971-257">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="44971-257">Update the Delete page</span></span>

<span data-ttu-id="44971-258">下列程式碼更新刪除頁面模型：</span><span class="sxs-lookup"><span data-stu-id="44971-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="44971-259">當實體已變更其提取之後，刪除頁面會偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="44971-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="44971-260">`Department.RowVersion`實體已提取時的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="44971-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="44971-261">當 EF 核心建立 SQL DELETE 命令時，會包含具有的 WHERE 子句`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="44971-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="44971-262">如果 SQL DELETE 命令結果中零個資料列受影響：</span><span class="sxs-lookup"><span data-stu-id="44971-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="44971-263">`RowVersion`中 SQL DELETE 命令不符`RowVersion`DB 中。</span><span class="sxs-lookup"><span data-stu-id="44971-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="44971-264">DbUpdateConcurrencyException 擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="44971-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="44971-265">`OnGetAsync`使用呼叫`concurrencyError`。</span><span class="sxs-lookup"><span data-stu-id="44971-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="44971-266">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="44971-266">Update the Delete page</span></span>

<span data-ttu-id="44971-267">更新*Pages/Departments/Delete.cshtml*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="44971-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="44971-268">上述標記進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="44971-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="44971-269">更新`page`指示詞從`@page`至`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="44971-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="44971-270">新增錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="44971-270">Adds an error message.</span></span>
* <span data-ttu-id="44971-271">取代中的 FullName FirstMidName**管理員**欄位。</span><span class="sxs-lookup"><span data-stu-id="44971-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="44971-272">變更`RowVersion`来顯示的最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="44971-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="44971-273">將隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="44971-273">Adds a hidden row version.</span></span> <span data-ttu-id="44971-274">`RowVersion`必須讓回傳值繫結加入。</span><span class="sxs-lookup"><span data-stu-id="44971-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="44971-275">測試並行衝突，以刪除頁面</span><span class="sxs-lookup"><span data-stu-id="44971-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="44971-276">建立測試部門。</span><span class="sxs-lookup"><span data-stu-id="44971-276">Create a test department.</span></span>

<span data-ttu-id="44971-277">開啟兩個瀏覽器上的執行個體刪除測試部門：</span><span class="sxs-lookup"><span data-stu-id="44971-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="44971-278">執行應用程式並選取部門。</span><span class="sxs-lookup"><span data-stu-id="44971-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="44971-279">以滑鼠右鍵按一下**刪除**超連結做為測試部門選取**新索引標籤中開啟**。</span><span class="sxs-lookup"><span data-stu-id="44971-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="44971-280">按一下**編輯**測試部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="44971-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="44971-281">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="44971-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="44971-282">變更的預算，第一個瀏覽器索引標籤，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="44971-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="44971-283">瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="44971-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="44971-284">請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。</span><span class="sxs-lookup"><span data-stu-id="44971-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="44971-285">刪除測試部門中的第二個索引標籤。並行處理錯誤會顯示資料庫的目前值。</span><span class="sxs-lookup"><span data-stu-id="44971-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="44971-286">按一下**刪除**刪除實體，除非`RowVersion`已 updated.department 已被刪除。</span><span class="sxs-lookup"><span data-stu-id="44971-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="44971-287">請參閱[繼承](xref:data/ef-mvc/inheritance)如何繼承的資料模型。</span><span class="sxs-lookup"><span data-stu-id="44971-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="44971-288">其他資源</span><span class="sxs-lookup"><span data-stu-id="44971-288">Additional resources</span></span>

* [<span data-ttu-id="44971-289">在 EF 核心中的並行語彙基元</span><span class="sxs-lookup"><span data-stu-id="44971-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="44971-290">處理 EF 核心中的並行存取</span><span class="sxs-lookup"><span data-stu-id="44971-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="44971-291">上一步</span><span class="sxs-lookup"><span data-stu-id="44971-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
