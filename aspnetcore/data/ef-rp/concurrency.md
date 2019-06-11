---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8
author: rick-anderson
description: 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8430f8e720870a7b541655ea8bcfe2f67c942bb3
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458422"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)，以及 [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

本教學課程會顯示如何在多位使用者同時並行更新實體時處理衝突。 若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:index#how-to-download-a-sample)。

## <a name="concurrency-conflicts"></a>並行衝突

並行衝突發生的時機：

* 使用者巡覽至實體的編輯頁面。
* 另一個使用者在第一個使用者的變更寫入到資料庫前更新了相同的實體。

當未啟用並行偵測時卻發生並行存取時：

* 最後作出的更新會成功。 亦即，最後更新的值會儲存到資料庫。
* 第一個作出的更新會遺失。

### <a name="optimistic-concurrency"></a>開放式並行存取

開放式並行存取允許並行衝突發生，並會在衝突發生時適當的作出反應。 例如，Jane 造訪了 Department 編輯頁面，然後將英文部門的預算從美金 $350,000.00 元調整到美金 $0.00 元。

![將預算變更為 0](concurrency/_static/change-budget.png)

在 Jane 按一下 [儲存]  前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

Jane 先按了一下 [儲存]  ，並且在瀏覽器顯示 [索引] 頁面時看到她作出的變更。

![預算已變更為 0](concurrency/_static/budget-zero.png)

John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]  。 接下來發生的情況便是由您處理並行衝突的方式決定。

開放式並行存取包含下列選項：

* 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。

  在此案例中，您將不會遺失任何資料。 兩位使用者更新了不同的屬性。 下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。 這種更新方法可以減少可能會導致資料遺失的衝突數目。 這種方法：
 
  * 無法在使用者更新相同屬性時避免資料遺失。
  * 表示通常在 Web 應用程式中不實用。 它必須維持大量的狀態來追蹤所有擷取的值和新的值。 維持大量的狀態可能會影響應用程式的效能。
  * 與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。

* 您可以讓 John 的變更覆寫 Jane 的變更。

  下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。 這稱之為「用戶端獲勝 (Client Wins)」  或「最後寫入為準 (Last in Wins)」  案例。 (所有來自用戶端的值都會優先於資料存放區中的資料。)若您沒有針對並行處理撰寫任何程式碼，便會自動發生「用戶端獲勝 (Client Wins)」的情況。

* 您可以防止 John 的變更更新到資料庫中。 一般而言，應用程式會：

  * 顯示錯誤訊息。
  * 顯示資料的目前狀態。
  * 允許使用者重新套用變更。

  這稱之為「存放區獲勝 (Store Wins)」  案例。 (資料存放區的值會優先於用戶端所提交的值。)您會在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。

## <a name="handling-concurrency"></a>處理並行 

當屬性已設定為[並行權杖](/ef/core/modeling/concurrency)時：

* EF Core 會驗證屬性在擷取之後尚未被修改。 該檢查會在呼叫 [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) 或 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時發生。
* 若屬性在擷取之後已遭修改，則系統便會擲回 [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)。 

資料庫和資料模型必須設定為支援擲回 `DbUpdateConcurrencyException`。

### <a name="detecting-concurrency-conflicts-on-a-property"></a>在屬性上偵測並行衝突

您可以使用 [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 屬性來在屬性層級上偵測並行衝突。 屬性可套用至模型上的多個屬性。 如需詳細資訊，請參閱[資料註解 - ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations)。

此教學課程中不會使用 `[ConcurrencyCheck]` 屬性。

### <a name="detecting-concurrency-conflicts-on-a-row"></a>在資料列上偵測並行衝突

為了偵測並行衝突，一個 [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) 追蹤資料行會新增到模型中。  `rowversion`：

* 是 SQL Server 的特定功能。 其他資料庫可能不會提供類似的功能。
* 是用來判斷實體在從資料庫擷取之後是否有變更。 

資料庫會產生一個循序 `rowversion` 數字，每一次資料列更新時該數字都會遞增。 在 `Update` 或 `Delete` 命令中，`Where` 子句會包含 `rowversion` 的擷取值。 如果更新的資料列已變更：

* `rowversion` 便不會符合擷取的值。
* `Update` 或 `Delete` 命令便會找不到資料列，因為 `Where` 子句包含了擷取的 `rowversion`。
* 於是便會擲回 `DbUpdateConcurrencyException`。

在 EF Core 中，當 `Update` 或 `Delete` 命令沒有更新任何資料列時，系統便會擲回並行例外狀況。

### <a name="add-a-tracking-property-to-the-department-entity"></a>將追蹤屬性新增到 Department 實體

在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 屬性表示此資料行會包含在 `Update` 和 `Delete` 命令的 `Where` 子句中。 該屬性稱為 `Timestamp`，因為先前版本的 SQL Server 在以 SQL `rowversion` 類型取代之前使用了 SQL `timestamp` 資料類型。

Fluent API 也可以指定追蹤屬性：

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

下列程式碼顯示了當 Department 名稱更新時，由 EF Core 產生的一部分 T-SQL：

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。 若資料庫 `RowVersion` 不等於 `RowVersion` 參數 (`@p2`)，則沒有任何資料列會獲得更新。

下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 會傳回上一個陳述式所影響的資料列數目。 當沒有更新任何資料列時，EF Core 便會擲回 `DbUpdateConcurrencyException`。

您可以在 Visual Studio 的輸出視窗中看到 EF Core 產生的 T-SQL。

### <a name="update-the-db"></a>更新資料庫

新增 `RowVersion` 屬性會變更資料庫模型，因此您將需要進行移轉。

建置專案。 在命令視窗中輸入下列命令：

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

上述命令：

* 新增一個 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。
* 更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。 更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* 執行移轉，以更新資料庫。

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Scaffold Departments 模型

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Department`。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

上述命令會 Scaffold `Department` 模型。 在 Visual Studio 中開啟專案。

建置專案。

### <a name="update-the-departments-index-page"></a>更新 Departments [索引] 頁面

Scaffolding 引擎會在 [索引] 頁面中建立 `RowVersion` 資料行，但該欄位不應該顯示出來。 在本教學課程中，`RowVersion` 的最後一個位元組會顯示出來，以協助您了解並行。 最後一個位元組不一定是唯一的。 實際的應用程式不會顯示 `RowVersion` 或 `RowVersion` 的最後一個位元組。

更新 [索引] 頁面：

* 使用 Departments 取代 Index。
* 使用 `RowVersion` 的最後一個位元組取代包含 `RowVersion` 的標記。
* 使用 FullName 取代 FirstMidName。

下列標記顯示了更新後的頁面：

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>更新 [編輯] 頁面模型

使用下列程式碼更新 *Pages\Departments\Edit.cshtml.cs*：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

為了偵測並行問題，系統會使用擷取實體時的 `rowVersion` 值更新 [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)。 EF Core 會產生一個帶有包含了原始 `RowVersion` 值 WHERE 子句的 SQL UPDATE 命令。 若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則便會擲回 `DbUpdateConcurrencyException` 例外狀況。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

在上述程式碼中，`Department.RowVersion` 的值為擷取實體時的值。 `OriginalValue` 的值為此方法呼叫 `FirstOrDefaultAsync` 時在資料庫中的值。

下列程式碼會取得用戶端的值 (POST 到此方法的值) 以及資料庫的值：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

下列程式碼會為每個資料庫中的值與發佈到 `OnPostAsync` 的值不同的資料行新增一個自訂錯誤訊息：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

下列醒目提示程式碼會將 `RowVersion` 的值設為從資料庫取得的新值。 下一次當使用者按一下 [儲存]  時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。 在 Razor 頁面中，當兩者同時存在時，欄位的 `ModelState` 值會優先於模型屬性值。

## <a name="update-the-edit-page"></a>更新 [編輯] 頁面

以下列標記更新 *Pages/Departments/Edit.cshtml*：

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

上述標記：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。
* 新增一個隱藏的資料列版本。 您必須新增 `RowVersion`，以讓回傳繫結值。
* 顯示 `RowVersion` 的最後一個位元組，作為偵錯用途。
* 使用強型別的 `InstructorNameSL` 取代 `ViewData`。

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>使用 [編輯] 頁面測試並行衝突

開啟兩個英文部門上 [編輯] 頁面的瀏覽器執行個體：

* 執行應用程式並選取 Departments。
* 以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)  。
* 在第一個索引標籤中，按一下英文部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]  。

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

在第二個瀏覽器索引標籤中變更不同欄位。

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

按一下 [儲存]  。 您會看到所有不符合資料庫值之欄位的錯誤訊息：

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

此瀏覽器視窗並未嘗試變更 [名稱] 欄位。 複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。 按下 Tab 鍵切換至下一個欄位。用戶端驗證會移除錯誤訊息。

![Department [編輯] 頁面錯誤訊息](concurrency/_static/cv.png)

再按一下 [儲存]  。 您在第二個瀏覽器索引標籤中輸入的值已儲存。 您會在 [索引] 頁面中看到儲存的值。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

以下列程式碼更新 [刪除] 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

[刪除] 頁面會在實體擷取之後發生變更時偵測並行衝突。 `Department.RowVersion` 是擷取實體時的資料列版本。 當 EF Core 建立 SQL DELETE 命令時，它會包含一個帶有 `RowVersion` 的 WHERE 子句。 若 SQL DELETE 命令影響的資料列數目為零：

* 表示 SQL DELETE 命令中的 `RowVersion` 不符合資料庫中的 `RowVersion`。
* 擲回 DbUpdateConcurrencyException。
* 使用 `concurrencyError` 呼叫 `OnGetAsync`。

### <a name="update-the-delete-page"></a>更新 [刪除] 頁面

使用下列程式碼更新 *ges/Departments/Delete.cshtml*：

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

上述標記會進行下列變更：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。
* 新增錯誤訊息。
* 在 [系統管理員]  欄位中將 FirstMidName 取代為 FullName。
* 變更 `RowVersion` 以顯示最後一個位元組。
* 新增一個隱藏的資料列版本。 您必須新增 `RowVersion`，以讓回傳繫結值。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>使用 [刪除] 頁面測試並行衝突

建立一個測試部門。

開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：

* 執行應用程式並選取 Departments。
* 以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)  。
* 按一下測試部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]  。

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

從第二個索引標籤刪除測試部門。系統會使用從資料庫取得之目前的值顯示並行錯誤。 按一下 [刪除]  會刪除實體，除非 `RowVersion` 已更新。部門已刪除。

請參閱[繼承](xref:data/ef-mvc/inheritance)以了解如何繼承資料模型。

### <a name="additional-resources"></a>其他資源

* [EF Core 中的並行權杖](/ef/core/modeling/concurrency)
* [在 EF Core 中處理並行](/ef/core/saving/concurrency)
* [這個教學課程的 YouTube 版本 (處理並行存取衝突)](https://youtu.be/EosxHTFgYps)
* [這個教學課程的 YouTube 版本 (第 2 部分)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [這個教學課程的 YouTube 版本 (第 3 部分)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [上一步](xref:data/ef-rp/update-related-data)
