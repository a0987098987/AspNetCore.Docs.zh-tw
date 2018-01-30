---
title: "使用 EF 核心並行-8 8 的 razor 頁面"
author: rick-anderson
description: "本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<span data-ttu-id="b74d0-103">en-us/</span><span class="sxs-lookup"><span data-stu-id="b74d0-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="b74d0-104">處理並行存取衝突-Razor 頁面 (8 個 8) 使用的 EF 核心</span><span class="sxs-lookup"><span data-stu-id="b74d0-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="b74d0-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，和[Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="b74d0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="b74d0-106">本教學課程會示範如何處理衝突，當多位使用者同時 （在相同的時間） 更新實體。</span><span class="sxs-lookup"><span data-stu-id="b74d0-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="b74d0-107">如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。</span><span class="sxs-lookup"><span data-stu-id="b74d0-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="b74d0-108">並行衝突</span><span class="sxs-lookup"><span data-stu-id="b74d0-108">Concurrency conflicts</span></span>

<span data-ttu-id="b74d0-109">發生並行衝突時：</span><span class="sxs-lookup"><span data-stu-id="b74d0-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="b74d0-110">使用者巡覽至實體的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="b74d0-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="b74d0-111">第一個使用者的變更寫入資料庫之前，另一位使用者更新相同的實體。</span><span class="sxs-lookup"><span data-stu-id="b74d0-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="b74d0-112">如果未啟用並行偵測，並行更新時：</span><span class="sxs-lookup"><span data-stu-id="b74d0-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="b74d0-113">最後的更新成功。</span><span class="sxs-lookup"><span data-stu-id="b74d0-113">The last update wins.</span></span> <span data-ttu-id="b74d0-114">也就是上次更新的值會儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="b74d0-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="b74d0-115">目前的更新中的第一個會遺失。</span><span class="sxs-lookup"><span data-stu-id="b74d0-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="b74d0-116">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="b74d0-116">Optimistic concurrency</span></span>

<span data-ttu-id="b74d0-117">開放式並行存取可讓發生並行衝突，然後回應適當當它們執行。</span><span class="sxs-lookup"><span data-stu-id="b74d0-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="b74d0-118">例如，Jane 造訪部門編輯頁面，並從 $350,000.00 變更 $0.00 英文部門預算。</span><span class="sxs-lookup"><span data-stu-id="b74d0-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![將變更為 0 的預算](concurrency/_static/change-budget.png)

<span data-ttu-id="b74d0-120">Jane 按一下之前**儲存**，John 造訪相同頁面上，並從 2007 年 9 月 1 日的開始日期] 欄位變成 2013 年 9 月 1 日。</span><span class="sxs-lookup"><span data-stu-id="b74d0-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![變更開始日期至 2013](concurrency/_static/change-date.png)

<span data-ttu-id="b74d0-122">Jane 按一下**儲存**第一次，並觀察其瀏覽器顯示的索引頁面時變更。</span><span class="sxs-lookup"><span data-stu-id="b74d0-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![變更為零的預算](concurrency/_static/budget-zero.png)

<span data-ttu-id="b74d0-124">John 按**儲存**上仍會顯示 $350,000.00 的預算的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="b74d0-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="b74d0-125">接下來的情況取決於您如何處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="b74d0-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="b74d0-126">開放式並行存取包含下列選項：</span><span class="sxs-lookup"><span data-stu-id="b74d0-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="b74d0-127">您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="b74d0-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="b74d0-128">在案例中，將不會遺失任何資料。</span><span class="sxs-lookup"><span data-stu-id="b74d0-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="b74d0-129">兩位使用者已更新不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="b74d0-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="b74d0-130">下一次有人瀏覽英文的部門，就會看到 Jane 的和 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="b74d0-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="b74d0-131">這種更新方法可以減少可能會導致資料遺失的衝突數目。</span><span class="sxs-lookup"><span data-stu-id="b74d0-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="b74d0-132">這種方法: \* 無法避免資料遺失，如果競爭變更至相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="b74d0-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="b74d0-133">\* 是在 web 應用程式通常不實用。</span><span class="sxs-lookup"><span data-stu-id="b74d0-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="b74d0-134">它需要維護重要狀態來追蹤感所有擷取的值和新值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="b74d0-135">維護大量狀態，可能會影響應用程式效能。</span><span class="sxs-lookup"><span data-stu-id="b74d0-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="b74d0-136">\* 可以提高應用程式複雜度相較於實體上的並行偵測。</span><span class="sxs-lookup"><span data-stu-id="b74d0-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="b74d0-137">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="b74d0-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="b74d0-138">在下一次有人瀏覽英文部門，他們會看到時間 2013 年 9 月 1 日和 $350,000.00 將擷取的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="b74d0-139">這種方法稱為*用戶端獲勝*或*Wins 中的最後一個*案例。</span><span class="sxs-lookup"><span data-stu-id="b74d0-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="b74d0-140">（從用戶端的所有值都優先於什麼是資料存放區中。）如果您沒有進行任何撰寫的程式碼的並行處理，Wins 用戶端會自動發生。</span><span class="sxs-lookup"><span data-stu-id="b74d0-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="b74d0-141">您可以在資料庫中更新，防止 John 的變更。</span><span class="sxs-lookup"><span data-stu-id="b74d0-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="b74d0-142">一般而言，應用程式會: \* 顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b74d0-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="b74d0-143">\* 顯示資料的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="b74d0-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="b74d0-144">\* 允許使用者重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="b74d0-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="b74d0-145">這稱為*存放區 Wins*案例。</span><span class="sxs-lookup"><span data-stu-id="b74d0-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="b74d0-146">（資料存放區的值優先於用戶端所送出的值。）您在本教學課程實作的存放區 Wins 的案例。</span><span class="sxs-lookup"><span data-stu-id="b74d0-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="b74d0-147">這個方法可確保任何變更會覆寫沒有警示使用者。</span><span class="sxs-lookup"><span data-stu-id="b74d0-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="b74d0-148">處理並行</span><span class="sxs-lookup"><span data-stu-id="b74d0-148">Handling concurrency</span></span> 

<span data-ttu-id="b74d0-149">當屬性設定為[並行語彙基元](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="b74d0-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="b74d0-150">EF 核心確認屬性有未被修改它提取之後。</span><span class="sxs-lookup"><span data-stu-id="b74d0-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="b74d0-151">不會進行檢查時[SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)或[SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)呼叫。</span><span class="sxs-lookup"><span data-stu-id="b74d0-151">The check occurs when [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="b74d0-152">如果屬性已變更，則提取之後, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)就會擲回。</span><span class="sxs-lookup"><span data-stu-id="b74d0-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="b74d0-153">資料庫和資料模型必須設定為支援擲回`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="b74d0-154">偵測並行衝突的屬性</span><span class="sxs-lookup"><span data-stu-id="b74d0-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="b74d0-155">在屬性層級的偵測並行衝突[ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)屬性。</span><span class="sxs-lookup"><span data-stu-id="b74d0-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="b74d0-156">屬性可以套用到模型上的多個屬性。</span><span class="sxs-lookup"><span data-stu-id="b74d0-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="b74d0-157">如需詳細資訊，請參閱[資料註解 ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations)。</span><span class="sxs-lookup"><span data-stu-id="b74d0-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="b74d0-158">`[ConcurrencyCheck]`屬性不會使用在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="b74d0-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="b74d0-159">偵測並行衝突的資料列</span><span class="sxs-lookup"><span data-stu-id="b74d0-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="b74d0-160">若要偵測並行衝突[rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)追蹤資料行加入至模型。</span><span class="sxs-lookup"><span data-stu-id="b74d0-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="b74d0-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="b74d0-161">`rowversion` :</span></span>

* <span data-ttu-id="b74d0-162">為特定的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="b74d0-162">Is SQL Server specific.</span></span> <span data-ttu-id="b74d0-163">其他資料庫可能不會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="b74d0-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="b74d0-164">用來判斷該實體不之後已經變更它已從 DB 擷取。</span><span class="sxs-lookup"><span data-stu-id="b74d0-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="b74d0-165">資料庫會產生循序`rowversion`已遞增一次資料列的數目會更新。</span><span class="sxs-lookup"><span data-stu-id="b74d0-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="b74d0-166">在`Update`或`Delete`命令，`Where`子句包含擷取的值的`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="b74d0-167">如果正在更新的資料列已經變更：</span><span class="sxs-lookup"><span data-stu-id="b74d0-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="b74d0-168">`rowversion`不符合已擷取的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="b74d0-169">`Update`或`Delete`命令找不到一個資料列因為`Where`子句包含擷取`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="b74d0-170">A`DbUpdateConcurrencyException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="b74d0-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="b74d0-171">在 EF 核心，當沒有任何資料列已更新`Update`或`Delete`命令時，擲回並行存取例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b74d0-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="b74d0-172">將追蹤的屬性加入 Department 實體</span><span class="sxs-lookup"><span data-stu-id="b74d0-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="b74d0-173">在*Models/Department.cs*，加入名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="b74d0-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="b74d0-174">[時間戳記](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)屬性會指定此資料行，會包含在`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="b74d0-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="b74d0-175">該屬性稱為`Timestamp`因為舊版的 SQL Server 使用 SQL`timestamp`資料類型，再將 SQL`rowversion`類型取代它。</span><span class="sxs-lookup"><span data-stu-id="b74d0-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="b74d0-176">關於 fluent API 也可以指定追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="b74d0-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="b74d0-177">下列程式碼會顯示 T-SQL 時就更新該部門名稱產生的 EF 核心部分：</span><span class="sxs-lookup"><span data-stu-id="b74d0-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="b74d0-178">上述反白顯示程式碼所示`WHERE`子句，其中包含`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="b74d0-179">如果 DB`RowVersion`不等於`RowVersion`參數 (`@p2`)，會更新任何資料列。</span><span class="sxs-lookup"><span data-stu-id="b74d0-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="b74d0-180">下列反白顯示的程式碼顯示，用來驗證正好一個資料列已更新 T-SQL:</span><span class="sxs-lookup"><span data-stu-id="b74d0-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="b74d0-181">[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql)傳回最後一個陳述式所影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="b74d0-181">[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="b74d0-182">在不會更新資料列，會擲回 EF 核心`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="b74d0-183">您可以看到 T-SQL EF 核心會產生的 Visual Studio [輸出] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="b74d0-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="b74d0-184">更新 DB</span><span class="sxs-lookup"><span data-stu-id="b74d0-184">Update the DB</span></span>

<span data-ttu-id="b74d0-185">加入`RowVersion`屬性變更需要移轉的資料庫模型。</span><span class="sxs-lookup"><span data-stu-id="b74d0-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="b74d0-186">建置專案。</span><span class="sxs-lookup"><span data-stu-id="b74d0-186">Build the project.</span></span> <span data-ttu-id="b74d0-187">在命令視窗中輸入下列：</span><span class="sxs-lookup"><span data-stu-id="b74d0-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="b74d0-188">上述命令中：</span><span class="sxs-lookup"><span data-stu-id="b74d0-188">The preceding commands:</span></span>

* <span data-ttu-id="b74d0-189">新增*移轉 / {時間 stamp}_RowVersion.cs*移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="b74d0-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="b74d0-190">更新*Migrations/SchoolContextModelSnapshot.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="b74d0-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="b74d0-191">更新會加入下列反白顯示的程式碼加入`BuildModel`方法：</span><span class="sxs-lookup"><span data-stu-id="b74d0-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="b74d0-192">執行移轉，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="b74d0-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="b74d0-193">Scaffold 部門模型</span><span class="sxs-lookup"><span data-stu-id="b74d0-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="b74d0-194">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="b74d0-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="b74d0-195">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="b74d0-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b74d0-196">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b74d0-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="b74d0-197">上述命令 scaffold`Department`模型。</span><span class="sxs-lookup"><span data-stu-id="b74d0-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="b74d0-198">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="b74d0-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="b74d0-199">建置專案。</span><span class="sxs-lookup"><span data-stu-id="b74d0-199">Build the project.</span></span> <span data-ttu-id="b74d0-200">建置會產生類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="b74d0-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="b74d0-201">全域變更`_context.Department`至`_context.Departments`(亦即，加入"s" `Department`)。</span><span class="sxs-lookup"><span data-stu-id="b74d0-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="b74d0-202">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="b74d0-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="b74d0-203">更新部門索引頁</span><span class="sxs-lookup"><span data-stu-id="b74d0-203">Update the Departments Index page</span></span>

<span data-ttu-id="b74d0-204">建立的 scaffolding 引擎`RowVersion`不應該顯示的索引] 頁面上，但該欄位的資料行。</span><span class="sxs-lookup"><span data-stu-id="b74d0-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="b74d0-205">在本教學課程的最後一個位元組`RowVersion`顯示有助於了解並行存取。</span><span class="sxs-lookup"><span data-stu-id="b74d0-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="b74d0-206">最後一個位元組不保證是唯一的。</span><span class="sxs-lookup"><span data-stu-id="b74d0-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="b74d0-207">實際的應用程式不會顯示`RowVersion`或最後一個位元組`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="b74d0-208">更新索引頁：</span><span class="sxs-lookup"><span data-stu-id="b74d0-208">Update the Index page:</span></span>

* <span data-ttu-id="b74d0-209">取代部門中的索引。</span><span class="sxs-lookup"><span data-stu-id="b74d0-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="b74d0-210">取代標記包含`RowVersion`與最後一個位元組`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="b74d0-211">FirstMidName 取代 FullName。</span><span class="sxs-lookup"><span data-stu-id="b74d0-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="b74d0-212">下列標記會顯示 [更新] 頁面：</span><span class="sxs-lookup"><span data-stu-id="b74d0-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="b74d0-213">更新頁面編輯模型</span><span class="sxs-lookup"><span data-stu-id="b74d0-213">Update the Edit page model</span></span>

<span data-ttu-id="b74d0-214">更新*pages\departments\edit.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b74d0-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="b74d0-215">若要偵測的並行存取問題， [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)以更新`rowVersion`從它已擷取之實體的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="b74d0-216">EF 核心產生 SQL UPDATE 命令搭配 WHERE 子句，其中包含原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="b74d0-217">如果任何資料列不受到 UPDATE 命令 (沒有資料列的原始`RowVersion`值)、`DbUpdateConcurrencyException`擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b74d0-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="b74d0-218">在上述程式碼，`Department.RowVersion`是時已擷取之實體的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="b74d0-219">`OriginalValue`DB 中的值時`FirstOrDefaultAsync`呼叫這個方法中。</span><span class="sxs-lookup"><span data-stu-id="b74d0-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="b74d0-220">下列程式碼會取得用戶端值 （張貼至這個方法的值） 和 DB 值：</span><span class="sxs-lookup"><span data-stu-id="b74d0-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="b74d0-221">下列程式碼會加入自訂錯誤訊息，針對每個資料行具有 DB 值不同於什麼公佈到`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="b74d0-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="b74d0-222">下列反白顯示的程式碼集`RowVersion`從 DB 擷取新的值的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="b74d0-223">使用者按一下 [下一次**儲存**，時，會發生因為攔截到最後一個顯示的編輯頁面的並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="b74d0-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="b74d0-224">`ModelState.Remove`陳述式是必要的因為`ModelState`具有舊`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="b74d0-225">在 Razor 頁面中，`ModelState`欄位會優先於模型屬性的值兩者均存在時的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="b74d0-226">更新編輯頁面</span><span class="sxs-lookup"><span data-stu-id="b74d0-226">Update the Edit page</span></span>

<span data-ttu-id="b74d0-227">更新*Pages/Departments/Edit.cshtml*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="b74d0-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="b74d0-228">上述的標記：</span><span class="sxs-lookup"><span data-stu-id="b74d0-228">The preceding markup:</span></span>

* <span data-ttu-id="b74d0-229">更新`page`指示詞從`@page`至`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="b74d0-230">將隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="b74d0-230">Adds a hidden row version.</span></span> <span data-ttu-id="b74d0-231">`RowVersion`必須讓回傳值繫結加入。</span><span class="sxs-lookup"><span data-stu-id="b74d0-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="b74d0-232">顯示的最後一個位元組`RowVersion`以進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="b74d0-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="b74d0-233">取代`ViewData`與強型別`InstructorNameSL`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="b74d0-234">測試並行衝突的編輯頁面</span><span class="sxs-lookup"><span data-stu-id="b74d0-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="b74d0-235">開啟兩個瀏覽器上的執行個體編輯英文部門：</span><span class="sxs-lookup"><span data-stu-id="b74d0-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="b74d0-236">執行應用程式並選取部門。</span><span class="sxs-lookup"><span data-stu-id="b74d0-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="b74d0-237">以滑鼠右鍵按一下**編輯**英文部門並選取超連結**新索引標籤中開啟**。</span><span class="sxs-lookup"><span data-stu-id="b74d0-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="b74d0-238">在第一個索引標籤上，按一下**編輯**英文部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="b74d0-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="b74d0-239">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="b74d0-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="b74d0-240">變更第一個瀏覽器索引標籤中的名稱，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="b74d0-240">Change the name in the first browser tab and click **Save**.</span></span>

![部門編輯變更後的第 1 頁](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="b74d0-242">瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="b74d0-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="b74d0-243">請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。</span><span class="sxs-lookup"><span data-stu-id="b74d0-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="b74d0-244">變更第二個瀏覽器索引標籤中的不同欄位。</span><span class="sxs-lookup"><span data-stu-id="b74d0-244">Change a different field in the second browser tab.</span></span>

![部門編輯變更後的第 2 頁](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="b74d0-246">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="b74d0-246">Click **Save**.</span></span> <span data-ttu-id="b74d0-247">您看到錯誤訊息不相符的 DB 值的所有欄位：</span><span class="sxs-lookup"><span data-stu-id="b74d0-247">You see error messages for all fields that don't match the DB values:</span></span>

![部門編輯頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="b74d0-249">此瀏覽器視窗中不想要變更 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="b74d0-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="b74d0-250">複製並貼入 [名稱] 欄位中的目前值 （語言）。</span><span class="sxs-lookup"><span data-stu-id="b74d0-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="b74d0-251">索引標籤時。用戶端驗證移除錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b74d0-251">Tab out. Client-side validation removes the error message.</span></span>

![部門編輯頁面錯誤訊息](concurrency/_static/cv.png)

<span data-ttu-id="b74d0-253">按一下**儲存**一次。</span><span class="sxs-lookup"><span data-stu-id="b74d0-253">Click **Save** again.</span></span> <span data-ttu-id="b74d0-254">會儲存在第二個瀏覽器索引標籤中所輸入的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="b74d0-255">您看到儲存的索引頁中的值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="b74d0-256">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="b74d0-256">Update the Delete page</span></span>

<span data-ttu-id="b74d0-257">下列程式碼更新刪除頁面模型：</span><span class="sxs-lookup"><span data-stu-id="b74d0-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="b74d0-258">當實體已變更其提取之後，刪除頁面會偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="b74d0-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="b74d0-259">`Department.RowVersion`實體已提取時的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="b74d0-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="b74d0-260">當 EF 核心建立 SQL DELETE 命令時，會包含具有的 WHERE 子句`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="b74d0-261">如果 SQL DELETE 命令結果中零個資料列受影響：</span><span class="sxs-lookup"><span data-stu-id="b74d0-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="b74d0-262">`RowVersion`中 SQL DELETE 命令不符`RowVersion`DB 中。</span><span class="sxs-lookup"><span data-stu-id="b74d0-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="b74d0-263">DbUpdateConcurrencyException 擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b74d0-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="b74d0-264">`OnGetAsync`使用呼叫`concurrencyError`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="b74d0-265">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="b74d0-265">Update the Delete page</span></span>

<span data-ttu-id="b74d0-266">更新*Pages/Departments/Delete.cshtml*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b74d0-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="b74d0-267">上述標記進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b74d0-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="b74d0-268">更新`page`指示詞從`@page`至`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="b74d0-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="b74d0-269">新增錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b74d0-269">Adds an error message.</span></span>
* <span data-ttu-id="b74d0-270">取代中的 FullName FirstMidName**管理員**欄位。</span><span class="sxs-lookup"><span data-stu-id="b74d0-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="b74d0-271">變更`RowVersion`来顯示的最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="b74d0-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="b74d0-272">將隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="b74d0-272">Adds a hidden row version.</span></span> <span data-ttu-id="b74d0-273">`RowVersion`必須讓回傳值繫結加入。</span><span class="sxs-lookup"><span data-stu-id="b74d0-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="b74d0-274">測試並行衝突，以刪除頁面</span><span class="sxs-lookup"><span data-stu-id="b74d0-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="b74d0-275">建立測試部門。</span><span class="sxs-lookup"><span data-stu-id="b74d0-275">Create a test department.</span></span>

<span data-ttu-id="b74d0-276">開啟兩個瀏覽器上的執行個體刪除測試部門：</span><span class="sxs-lookup"><span data-stu-id="b74d0-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="b74d0-277">執行應用程式並選取部門。</span><span class="sxs-lookup"><span data-stu-id="b74d0-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="b74d0-278">以滑鼠右鍵按一下**刪除**超連結做為測試部門選取**新索引標籤中開啟**。</span><span class="sxs-lookup"><span data-stu-id="b74d0-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="b74d0-279">按一下**編輯**測試部門的超連結。</span><span class="sxs-lookup"><span data-stu-id="b74d0-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="b74d0-280">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="b74d0-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="b74d0-281">變更的預算，第一個瀏覽器索引標籤，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="b74d0-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="b74d0-282">瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。</span><span class="sxs-lookup"><span data-stu-id="b74d0-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="b74d0-283">請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。</span><span class="sxs-lookup"><span data-stu-id="b74d0-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="b74d0-284">刪除測試部門中的第二個索引標籤。並行處理錯誤會顯示資料庫的目前值。</span><span class="sxs-lookup"><span data-stu-id="b74d0-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="b74d0-285">按一下**刪除**刪除實體，除非`RowVersion`已 updated.department 已被刪除。</span><span class="sxs-lookup"><span data-stu-id="b74d0-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="b74d0-286">請參閱[繼承](xref:data/ef-mvc/inheritance)如何繼承的資料模型。</span><span class="sxs-lookup"><span data-stu-id="b74d0-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="b74d0-287">其他資源</span><span class="sxs-lookup"><span data-stu-id="b74d0-287">Additional resources</span></span>

* [<span data-ttu-id="b74d0-288">在 EF 核心中的並行語彙基元</span><span class="sxs-lookup"><span data-stu-id="b74d0-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [<span data-ttu-id="b74d0-289">處理 EF 核心中的並行存取</span><span class="sxs-lookup"><span data-stu-id="b74d0-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="b74d0-290">上一步</span><span class="sxs-lookup"><span data-stu-id="b74d0-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
