---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8
author: tdykstra
description: 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: c9cbf8fd3ed85f32b3c166bf2df702fd26df4fc3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080982"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)，以及 [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

本教學課程會顯示如何在多位使用者同時並行更新實體時處理衝突。

## <a name="concurrency-conflicts"></a>並行衝突

並行衝突發生的時機：

* 使用者巡覽至實體的編輯頁面。
* 另一位使用者在第一位使用者的變更寫入到資料庫前更新相同實體。

若未啟用並行偵測，則最後更新資料庫的任何使用者所作出變更會覆寫前一位使用者所作出變更。 若您可以接受這種風險，則並行程式設計所帶來的成本便可能會超過其效益。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 (鎖定)

其中一種避免並行衝突的方式是使用資料庫鎖定。 這稱之為封閉式並行存取。 在應用程式讀取要更新的資料庫資料列前，它會先要求鎖定。 針對更新存取鎖定資料列後，在第一個鎖定解除前，其他使用者都無法鎖定該資料列。

管理鎖定有幾個缺點。 這種程式可能會相當複雜，且可能會隨著使用者數量的增加而造成效能問題。 Entity Framework Core 沒有提供這種功能的內建支援，本教學課程也不會示範如何進行實作。

### <a name="optimistic-concurrency"></a>開放式並行存取

開放式並行存取允許並行衝突發生，並會在衝突發生時適當的作出反應。 例如，Jane 造訪了 Department 編輯頁面，然後將英文部門的預算從美金 $350,000.00 元調整到美金 $0.00 元。

![將預算變更為 0](concurrency/_static/change-budget30.png)

在 Jane 按一下 [儲存] 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。

![將開始日期變更為 2013 年](concurrency/_static/change-date30.png)

Jane 先按了一下 [儲存] 並看到她所做的變更生效，因為瀏覽器顯示 Budget 金額為零的 Index 頁。

John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]。 接下來將發生情況是由您處理並行衝突的方式決定：

* 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。

  在此案例中，您將不會遺失任何資料。 兩位使用者更新了不同的屬性。 下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。 這種更新方法可以減少可能會導致資料遺失的衝突數目。 此方法有一些缺點：
 
  * 無法在使用者更新相同屬性時避免資料遺失。
  * 表示通常在 Web 應用程式中不實用。 它必須維持大量的狀態來追蹤所有擷取的值和新的值。 維持大量的狀態可能會影響應用程式的效能。
  * 與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。

* 您可以讓 John 的變更覆寫 Jane 的變更。

  下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 (所有來自用戶端的值都會優先於資料存放區中的資料。)若您沒有針對並行處理撰寫任何程式碼，便會自動發生「用戶端獲勝 (Client Wins)」的情況。

* 您可以防止 John 的變更更新到資料庫中。 一般而言，應用程式會：

  * 顯示錯誤訊息。
  * 顯示資料的目前狀態。
  * 允許使用者重新套用變更。

  這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您會在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。

## <a name="conflict-detection-in-ef-core"></a>EF Core 中的衝突偵測

EF Core 會在偵測到衝突時擲回 `DbConcurrencyException` 例外狀況。 資料模型必須進行設定才能啟用衝突偵測。 啟用衝突偵測的選項包括下列項目：

* 設定 EF Core，在 Update 和 Delete 命令的 Where 子句中將資料行的原始值設為[並行權杖](/ef/core/modeling/concurrency)並包含在其中。

  呼叫 `SaveChanges` 時，Where 子句會尋找任何標註 [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) 屬性的屬性原始值。 若從第一次讀取資料列後有任何的並行權杖屬性產生變更，更新陳述式便不會尋找要更新的資料列。 EF Core 會將其解譯為並行衝突。 針對擁有許多資料行的資料庫資料表，這種方法可能導致非常龐大的 Where 子句，且可能會需要維持龐大數量的狀態。 因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。

* 在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。

  在 SQL Server 資料庫中，追蹤資料行的資料型別是 `rowversion`。 `rowversion` 的值是一個循序號碼，每一次資料列更新時都會遞增。 在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行的原始值 (原始資料列版本號碼)。 若正在更新的資料列已由另一位使用者進行變更，則 `rowversion` 資料行中的值便會和原始值不同。 在這種情況下，Update 或 Delete 陳述式便會因為 Where 子句而無法找到要更新的資料列。 EF Core 會在 Update 或 Delete 命令沒有影響任何資料列時擲回並行例外狀況。

## <a name="add-a-tracking-property"></a>新增追蹤屬性

在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 屬性是將資料行識別為並行追蹤資料行的屬性。 Fluent API 是指定追蹤屬性的另外一種方式：

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

針對 SQL Server 資料庫，實體屬性上的 `[Timestamp]` 屬性會定義為位元組陣列：

* 使資料行包含在 DELETE 和 UPDATE WHERE 子句中。
* 將資料庫中的資料行型別設為 [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)。

資料庫會產生循序的資料列版本號碼，會在每一次更新資料列時遞增。 在 `Update` 或 `Delete` 命令中，`Where` 子句會包含擷取到的資料列版本值。 若正在更新的資料列從擷取之後已產生變更：

* 目前的資料列版本值會與所擷取值不相符。
* `Update` 或 `Delete` 命令會因為 `Where` 子句尋找的是所擷取資料列版本值，而找不到資料列。
* 於是便會擲回 `DbUpdateConcurrencyException`。

下列程式碼顯示了當 Department 名稱更新時，由 EF Core 產生的一部分 T-SQL：

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。 若資料庫 `RowVersion` 與 `RowVersion` 參數 (`@p2`) 不相同，便不會更新任何資料列。

下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 會傳回上一個陳述式所影響的資料列數目。 若沒有更新任何資料列，則 EF Core 會擲回 `DbUpdateConcurrencyException`。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

針對 SQLite 資料庫，實體屬性上的 `[Timestamp]` 屬性會定義為位元組陣列：

* 使資料行包含在 DELETE 和 UPDATE WHERE 子句中。
* 對應到 BLOB 資料行型別。

資料庫觸發程序會在每次資料列更新時，使用新的隨機位元組陣列更新 RowVersion 資料行。 在 `Update` 或 `Delete` 命令中，`Where` 子句會包含 RowVersion 資料行的擷取值。 若正在更新的資料列從擷取之後已產生變更：

* 目前的資料列版本值會與所擷取值不相符。
* `Update` 或 `Delete` 命令會因為 `Where` 子句尋找的是原始資料列版本值而找不到資料列。
* 於是便會擲回 `DbUpdateConcurrencyException`。

---

### <a name="update-the-database"></a>更新資料庫

新增 `RowVersion` 屬性會變更資料模型，因此您將需要進行移轉。

建置專案。 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 PMC 中執行下列命令：

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 在終端機中執行下列命令：

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

此命令：

* 建立 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。
* 更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。 更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 PMC 中執行下列命令：

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟 `Migrations/<timestamp>_RowVersion.cs` 檔案並新增醒目提示的程式碼：

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  上述程式碼：

  * 使用隨機 Blob 值更新現有資料列。
  * 新增資料庫觸發程序，在每次資料列更新時將 RowVersion 資料行設為隨機 Blob 值。

* 在終端機中執行下列命令：

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a>Scaffold Department 頁面

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 遵循 [Scaffold Student 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：

* 建立 *Pages/Departments* 資料夾。  
* 使用 `Department` 作為模型類別。
  * 使用現有內容類別，而非建立新的類別。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 建立 *Pages/Departments* 資料夾。

* 執行下列命令來 Scaffold Department 頁面。

  **在 Windows 上**：

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  **在 Linux 或 macOS 上：**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

建置專案。

## <a name="update-the-index-page"></a>更新 Index 頁面

Scaffolding 工具會為 Index 頁面建立 `RowVersion` 資料行，但該欄位不會在生產應用程式中顯示。 在本教學課程中，會顯示 `RowVersion` 的最後一個位元組，來協助示範並行處理的運作方式。 最後一個位元組本身不一定是唯一的。

更新 *Pages\Departments\Index.cshtml* 頁面：

* 使用 Departments 取代 Index。
* 變更包含 `RowVersion` 的程式碼，僅顯示位元組陣列的最後一個位元組。
* 使用 FullName 取代 FirstMidName。

下列程式碼會顯示更新後的頁面：

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a>更新 [編輯] 頁面模型

使用下列程式碼更新 *Pages\Departments\Edit.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) 會在於 `OnGet` 方法中進行擷取時，使用實體的 `rowVersion` 值來進行更新。 EF Core 會產生一個帶有包含了原始 `RowVersion` 值 WHERE 子句的 SQL UPDATE 命令。 若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則便會擲回 `DbUpdateConcurrencyException` 例外狀況。

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

在上述醒目提示的程式碼中：

* `Department.RowVersion` 中的值是原先在 Edit 頁面 Get 要求中所擷取實體中的內容。 該值會透過 Razor 頁面中隱藏欄位的方式提供給 `OnPost` 方法，而 Razor 頁面會顯示要編輯的實體。 隱藏欄位的值會由模型繫結器複製到 `Department.RowVersion`。
* `OriginalValue` 是 EF Core 將在 Where 子句中使用的內容。 在執行醒目提示的程式碼區段前，`OriginalValue` 會擁有在此方法中呼叫 `FirstOrDefaultAsync` 時原先在資料庫內的值，而該值可能會和 Edit 頁面上顯示的內容不同。
* 醒目提示的程式碼會確保 EF Core 使用 SQL UPDATE 陳述式 WHERE 子句中所顯示 `Department` 實體原始 `RowVersion` 值。

發生並行錯誤時，下列醒目提示的程式碼會取得用戶端值 (POST 到此方法的值) 及資料庫值。

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

下列程式碼會為每個其資料庫中值與 POST 到 `OnPostAsync` 值的不同資料行新增一個自訂錯誤訊息：

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

下列醒目提示的程式碼會將 `RowVersion` 值設為從資料庫所擷取新值。 下一次當使用者按一下 [儲存] 時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。 在 Razor 頁面中，當兩者同時存在時，欄位的 `ModelState` 值會優先於模型屬性值。

### <a name="update-the-razor-page"></a>更新 Razor 頁面

使用下列程式碼更新 *Pages/Departments/Edit.cshtml*：

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

上述程式碼：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。
* 新增一個隱藏的資料列版本。 您必須新增 `RowVersion`，以讓回傳繫結值。
* 顯示 `RowVersion` 的最後一個位元組，作為偵錯用途。
* 使用強型別的 `InstructorNameSL` 取代 `ViewData`。

### <a name="test-concurrency-conflicts-with-the-edit-page"></a>使用 [編輯] 頁面測試並行衝突

開啟兩個英文部門上 [編輯] 頁面的瀏覽器執行個體：

* 執行應用程式並選取 Departments。
* 以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。
* 在第一個索引標籤中，按一下英文部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]。

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-130.png)

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

在第二個瀏覽器索引標籤中變更不同欄位。

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-230.png)

按一下 [儲存]。 您會看到所有不符合資料庫值欄位的錯誤訊息：

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error30.png)

此瀏覽器視窗並未嘗試變更 [名稱] 欄位。 複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。 按下 Tab 鍵切換至下一個欄位。用戶端驗證會移除錯誤訊息。

再按一下 [儲存]。 您在第二個瀏覽器索引標籤中輸入的值已儲存。 您會在 [索引] 頁面中看到儲存的值。

## <a name="update-the-delete-page"></a>更新 *Delete* 頁面

使用下列程式碼更新 *ges/Departments/Delete.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

[刪除] 頁面會在實體擷取之後發生變更時偵測並行衝突。 `Department.RowVersion` 是擷取實體時的資料列版本。 當 EF Core 建立 SQL DELETE 命令時，它會包含一個帶有 `RowVersion` 的 WHERE 子句。 若 SQL DELETE 命令影響的資料列數目為零：

* SQL DELETE 命令中 `RowVersion` 與資料庫中 `RowVersion` 不相符。
* 擲回 DbUpdateConcurrencyException。
* 使用 `concurrencyError` 呼叫 `OnGetAsync`。

### <a name="update-the-delete-razor-page"></a>更新刪除 Razor 頁面

使用下列程式碼更新 *ges/Departments/Delete.cshtml*：

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

上述程式碼會進行下列變更：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。
* 新增錯誤訊息。
* 在 [系統管理員] 欄位中將 FirstMidName 取代為 FullName。
* 變更 `RowVersion` 以顯示最後一個位元組。
* 新增一個隱藏的資料列版本。 您必須新增 `RowVersion`，以讓 postgit add back 繫結值。

### <a name="test-concurrency-conflicts"></a>測試並行衝突

建立一個測試部門。

開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：

* 執行應用程式並選取 Departments。
* 以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。
* 按一下測試部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]。

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

從第二個索引標籤刪除測試部門。系統會顯示並行錯誤，且從資料庫取得的目前值。 按一下 [刪除] 會刪除實體，除非 `RowVersion` 已更新。部門已刪除。

## <a name="additional-resources"></a>其他資源

* [EF Core 中的並行權杖](/ef/core/modeling/concurrency)
* [在 EF Core 中處理並行](/ef/core/saving/concurrency)
* [偵錯 ASP.NET Core 2.x 原始檔](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a>後續步驟

這是一系列教學課程的最後一個教學課程。 [本教學課程系列的 MVC 版本](xref:data/ef-mvc/index)涵蓋了其他主題。

> [!div class="step-by-step"]
> [上一個教學課程](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

在 Jane 按一下 [儲存] 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

Jane 先按了一下 [儲存]，並且在瀏覽器顯示 [索引] 頁面時看到她作出的變更。

![預算已變更為 0](concurrency/_static/budget-zero.png)

John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]。 接下來發生的情況便是由您處理並行衝突的方式決定。

開放式並行存取包含下列選項：

* 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。

  在此案例中，您將不會遺失任何資料。 兩位使用者更新了不同的屬性。 下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。 這種更新方法可以減少可能會導致資料遺失的衝突數目。 這種方法：
 
  * 無法在使用者更新相同屬性時避免資料遺失。
  * 表示通常在 Web 應用程式中不實用。 它必須維持大量的狀態來追蹤所有擷取的值和新的值。 維持大量的狀態可能會影響應用程式的效能。
  * 與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。

* 您可以讓 John 的變更覆寫 Jane 的變更。

  下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 (所有來自用戶端的值都會優先於資料存放區中的資料。)若您沒有針對並行處理撰寫任何程式碼，便會自動發生「用戶端獲勝 (Client Wins)」的情況。

* 您可以防止 John 的變更更新到資料庫中。 一般而言，應用程式會：

  * 顯示錯誤訊息。
  * 顯示資料的目前狀態。
  * 允許使用者重新套用變更。

  這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您會在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。

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

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。 若資料庫 `RowVersion` 不等於 `RowVersion` 參數 (`@p2`)，則沒有任何資料列會獲得更新。

下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 會傳回上一個陳述式所影響的資料列數目。 當沒有更新任何資料列時，EF Core 便會擲回 `DbUpdateConcurrencyException`。

您可以在 Visual Studio 的輸出視窗中看到 EF Core 產生的 T-SQL。

### <a name="update-the-db"></a>更新資料庫

新增 `RowVersion` 屬性會變更資料庫模型，因此您將需要進行移轉。

建置專案。 在命令視窗中輸入下列命令：

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

上述命令：

* 新增一個 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。
* 更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。 更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* 執行移轉，以更新資料庫。

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a>Scaffold Departments 模型

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，並為模型類別使用 `Department`。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

 執行下列命令：

  ```dotnetcli
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

下列醒目提示程式碼會將 `RowVersion` 的值設為從資料庫取得的新值。 下一次當使用者按一下 [儲存] 時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。

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
* 以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。
* 在第一個索引標籤中，按一下英文部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]。

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

在第二個瀏覽器索引標籤中變更不同欄位。

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

按一下 [儲存]。 您會看到所有不符合資料庫值之欄位的錯誤訊息：

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

此瀏覽器視窗並未嘗試變更 [名稱] 欄位。 複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。 按下 Tab 鍵切換至下一個欄位。用戶端驗證會移除錯誤訊息。

![Department [編輯] 頁面錯誤訊息](concurrency/_static/cv.png)

再按一下 [儲存]。 您在第二個瀏覽器索引標籤中輸入的值已儲存。 您會在 [索引] 頁面中看到儲存的值。

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

上述程式碼會進行下列變更：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。
* 新增錯誤訊息。
* 在 [系統管理員] 欄位中將 FirstMidName 取代為 FullName。
* 變更 `RowVersion` 以顯示最後一個位元組。
* 新增一個隱藏的資料列版本。 您必須新增 `RowVersion`，以讓回傳繫結值。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>使用 [刪除] 頁面測試並行衝突

建立一個測試部門。

開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：

* 執行應用程式並選取 Departments。
* 以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。
* 按一下測試部門的**編輯**超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]。

瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。 請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。

從第二個索引標籤刪除測試部門。系統會使用從資料庫取得之目前的值顯示並行錯誤。 按一下 [刪除] 會刪除實體，除非 `RowVersion` 已更新。部門已刪除。

請參閱[繼承](xref:data/ef-mvc/inheritance)以了解如何繼承資料模型。

### <a name="additional-resources"></a>其他資源

* [EF Core 中的並行權杖](/ef/core/modeling/concurrency)
* [在 EF Core 中處理並行](/ef/core/saving/concurrency)
* [這個教學課程的 YouTube 版本 (處理並行存取衝突)](https://youtu.be/EosxHTFgYps)
* [這個教學課程的 YouTube 版本 (第 2 部分)](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [這個教學課程的 YouTube 版本 (第 3 部分)](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [上一步](xref:data/ef-rp/update-related-data)

::: moniker-end

