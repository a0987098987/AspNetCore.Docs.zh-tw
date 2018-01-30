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
en-us/

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>處理並行存取衝突-Razor 頁面 (8 個 8) 使用的 EF 核心

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，和[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教學課程會示範如何處理衝突，當多位使用者同時 （在相同的時間） 更新實體。 如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。

## <a name="concurrency-conflicts"></a>並行衝突

發生並行衝突時：

* 使用者巡覽至實體的編輯頁面。
* 第一個使用者的變更寫入資料庫之前，另一位使用者更新相同的實體。

如果未啟用並行偵測，並行更新時：

* 最後的更新成功。 也就是上次更新的值會儲存到資料庫。
* 目前的更新中的第一個會遺失。

### <a name="optimistic-concurrency"></a>開放式並行存取

開放式並行存取可讓發生並行衝突，然後回應適當當它們執行。 例如，Jane 造訪部門編輯頁面，並從 $350,000.00 變更 $0.00 英文部門預算。

![將變更為 0 的預算](concurrency/_static/change-budget.png)

Jane 按一下之前**儲存**，John 造訪相同頁面上，並從 2007 年 9 月 1 日的開始日期] 欄位變成 2013 年 9 月 1 日。

![變更開始日期至 2013](concurrency/_static/change-date.png)

Jane 按一下**儲存**第一次，並觀察其瀏覽器顯示的索引頁面時變更。

![變更為零的預算](concurrency/_static/budget-zero.png)

John 按**儲存**上仍會顯示 $350,000.00 的預算的編輯頁面。 接下來的情況取決於您如何處理並行衝突。

開放式並行存取包含下列選項：

* 您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。

 在案例中，將不會遺失任何資料。 兩位使用者已更新不同的屬性。 下一次有人瀏覽英文的部門，就會看到 Jane 的和 John 的變更。 這種更新方法可以減少可能會導致資料遺失的衝突數目。 這種方法: * 無法避免資料遺失，如果競爭變更至相同的屬性。
        * 是在 web 應用程式通常不實用。 它需要維護重要狀態來追蹤感所有擷取的值和新值。 維護大量狀態，可能會影響應用程式效能。
        * 可以提高應用程式複雜度相較於實體上的並行偵測。

* 您可以讓 John 的變更覆寫 Jane 的變更。

 在下一次有人瀏覽英文部門，他們會看到時間 2013 年 9 月 1 日和 $350,000.00 將擷取的值。 這種方法稱為*用戶端獲勝*或*Wins 中的最後一個*案例。 （從用戶端的所有值都優先於什麼是資料存放區中。）如果您沒有進行任何撰寫的程式碼的並行處理，Wins 用戶端會自動發生。

* 您可以在資料庫中更新，防止 John 的變更。 一般而言，應用程式會: * 顯示錯誤訊息。
        * 顯示資料的目前狀態。
        * 允許使用者重新套用變更。

 這稱為*存放區 Wins*案例。 （資料存放區的值優先於用戶端所送出的值。）您在本教學課程實作的存放區 Wins 的案例。 這個方法可確保任何變更會覆寫沒有警示使用者。

## <a name="handling-concurrency"></a>處理並行 

當屬性設定為[並行語彙基元](https://docs.microsoft.com/ef/core/modeling/concurrency):

* EF 核心確認屬性有未被修改它提取之後。 不會進行檢查時[SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)或[SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)呼叫。
* 如果屬性已變更，則提取之後, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)就會擲回。 

資料庫和資料模型必須設定為支援擲回`DbUpdateConcurrencyException`。

### <a name="detecting-concurrency-conflicts-on-a-property"></a>偵測並行衝突的屬性

在屬性層級的偵測並行衝突[ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)屬性。 屬性可以套用到模型上的多個屬性。 如需詳細資訊，請參閱[資料註解 ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations)。

`[ConcurrencyCheck]`屬性不會使用在本教學課程。

### <a name="detecting-concurrency-conflicts-on-a-row"></a>偵測並行衝突的資料列

若要偵測並行衝突[rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)追蹤資料行加入至模型。  `rowversion` :

* 為特定的 SQL Server。 其他資料庫可能不會提供類似的功能。
* 用來判斷該實體不之後已經變更它已從 DB 擷取。 

資料庫會產生循序`rowversion`已遞增一次資料列的數目會更新。 在`Update`或`Delete`命令，`Where`子句包含擷取的值的`rowversion`。 如果正在更新的資料列已經變更：

 * `rowversion`不符合已擷取的值。
 * `Update`或`Delete`命令找不到一個資料列因為`Where`子句包含擷取`rowversion`。
 * A`DbUpdateConcurrencyException`就會擲回。

在 EF 核心，當沒有任何資料列已更新`Update`或`Delete`命令時，擲回並行存取例外狀況。

### <a name="add-a-tracking-property-to-the-department-entity"></a>將追蹤的屬性加入 Department 實體

在*Models/Department.cs*，加入名為 RowVersion 的追蹤屬性：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[時間戳記](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)屬性會指定此資料行，會包含在`Where`子句`Update`和`Delete`命令。 該屬性稱為`Timestamp`因為舊版的 SQL Server 使用 SQL`timestamp`資料類型，再將 SQL`rowversion`類型取代它。

關於 fluent API 也可以指定追蹤屬性：

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

下列程式碼會顯示 T-SQL 時就更新該部門名稱產生的 EF 核心部分：

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

上述反白顯示程式碼所示`WHERE`子句，其中包含`RowVersion`。 如果 DB`RowVersion`不等於`RowVersion`參數 (`@p2`)，會更新任何資料列。

下列反白顯示的程式碼顯示，用來驗證正好一個資料列已更新 T-SQL:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql)傳回最後一個陳述式所影響的資料列數目。 在不會更新資料列，會擲回 EF 核心`DbUpdateConcurrencyException`。

您可以看到 T-SQL EF 核心會產生的 Visual Studio [輸出] 視窗中。

### <a name="update-the-db"></a>更新 DB

加入`RowVersion`屬性變更需要移轉的資料庫模型。

建置專案。 在命令視窗中輸入下列：

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

上述命令中：

* 新增*移轉 / {時間 stamp}_RowVersion.cs*移轉檔案。
* 更新*Migrations/SchoolContextModelSnapshot.cs*檔案。 更新會加入下列反白顯示的程式碼加入`BuildModel`方法：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* 執行移轉，以更新資料庫。

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Scaffold 部門模型

* 結束 Visual Studio。
* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

上述命令 scaffold`Department`模型。 在 Visual Studio 中開啟專案。

建置專案。 建置會產生類似如下的錯誤：

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 全域變更`_context.Department`至`_context.Departments`(亦即，加入"s" `Department`)。 找到項目 7 及更新。

### <a name="update-the-departments-index-page"></a>更新部門索引頁

建立的 scaffolding 引擎`RowVersion`不應該顯示的索引] 頁面上，但該欄位的資料行。 在本教學課程的最後一個位元組`RowVersion`顯示有助於了解並行存取。 最後一個位元組不保證是唯一的。 實際的應用程式不會顯示`RowVersion`或最後一個位元組`RowVersion`。

更新索引頁：

* 取代部門中的索引。
* 取代標記包含`RowVersion`與最後一個位元組`RowVersion`。
* FirstMidName 取代 FullName。

下列標記會顯示 [更新] 頁面：

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>更新頁面編輯模型

更新*pages\departments\edit.cshtml.cs*為下列程式碼：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

若要偵測的並行存取問題， [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)以更新`rowVersion`從它已擷取之實體的值。 EF 核心產生 SQL UPDATE 命令搭配 WHERE 子句，其中包含原始`RowVersion`值。 如果任何資料列不受到 UPDATE 命令 (沒有資料列的原始`RowVersion`值)、`DbUpdateConcurrencyException`擲回例外狀況。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

在上述程式碼，`Department.RowVersion`是時已擷取之實體的值。 `OriginalValue`DB 中的值時`FirstOrDefaultAsync`呼叫這個方法中。

下列程式碼會取得用戶端值 （張貼至這個方法的值） 和 DB 值：

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

下列程式碼會加入自訂錯誤訊息，針對每個資料行具有 DB 值不同於什麼公佈到`OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

下列反白顯示的程式碼集`RowVersion`從 DB 擷取新的值的值。 使用者按一下 [下一次**儲存**，時，會發生因為攔截到最後一個顯示的編輯頁面的並行錯誤。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove`陳述式是必要的因為`ModelState`具有舊`RowVersion`值。 在 Razor 頁面中，`ModelState`欄位會優先於模型屬性的值兩者均存在時的值。

## <a name="update-the-edit-page"></a>更新編輯頁面

更新*Pages/Departments/Edit.cshtml*以下列標記：

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

上述的標記：

* 更新`page`指示詞從`@page`至`@page "{id:int}"`。
* 將隱藏的資料列版本。 `RowVersion`必須讓回傳值繫結加入。
* 顯示的最後一個位元組`RowVersion`以進行偵錯。
* 取代`ViewData`與強型別`InstructorNameSL`。

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>測試並行衝突的編輯頁面

開啟兩個瀏覽器上的執行個體編輯英文部門：

* 執行應用程式並選取部門。
* 以滑鼠右鍵按一下**編輯**英文部門並選取超連結**新索引標籤中開啟**。
* 在第一個索引標籤上，按一下**編輯**英文部門的超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

變更第一個瀏覽器索引標籤中的名稱，然後按一下**儲存**。

![部門編輯變更後的第 1 頁](concurrency/_static/edit-after-change-1.png)

瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。 請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。

變更第二個瀏覽器索引標籤中的不同欄位。

![部門編輯變更後的第 2 頁](concurrency/_static/edit-after-change-2.png)

按一下 [儲存] 。 您看到錯誤訊息不相符的 DB 值的所有欄位：

![部門編輯頁面錯誤訊息](concurrency/_static/edit-error.png)

此瀏覽器視窗中不想要變更 [名稱] 欄位。 複製並貼入 [名稱] 欄位中的目前值 （語言）。 索引標籤時。用戶端驗證移除錯誤訊息。

![部門編輯頁面錯誤訊息](concurrency/_static/cv.png)

按一下**儲存**一次。 會儲存在第二個瀏覽器索引標籤中所輸入的值。 您看到儲存的索引頁中的值。

## <a name="update-the-delete-page"></a>更新刪除頁面

下列程式碼更新刪除頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

當實體已變更其提取之後，刪除頁面會偵測並行衝突。 `Department.RowVersion`實體已提取時的資料列版本。 當 EF 核心建立 SQL DELETE 命令時，會包含具有的 WHERE 子句`RowVersion`。 如果 SQL DELETE 命令結果中零個資料列受影響：

* `RowVersion`中 SQL DELETE 命令不符`RowVersion`DB 中。
* DbUpdateConcurrencyException 擲回例外狀況。
* `OnGetAsync`使用呼叫`concurrencyError`。

### <a name="update-the-delete-page"></a>更新刪除頁面

更新*Pages/Departments/Delete.cshtml*為下列程式碼：

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


上述標記進行下列變更：

* 更新`page`指示詞從`@page`至`@page "{id:int}"`。
* 新增錯誤訊息。
* 取代中的 FullName FirstMidName**管理員**欄位。
* 變更`RowVersion`来顯示的最後一個位元組。
* 將隱藏的資料列版本。 `RowVersion`必須讓回傳值繫結加入。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>測試並行衝突，以刪除頁面

建立測試部門。

開啟兩個瀏覽器上的執行個體刪除測試部門：

* 執行應用程式並選取部門。
* 以滑鼠右鍵按一下**刪除**超連結做為測試部門選取**新索引標籤中開啟**。
* 按一下**編輯**測試部門的超連結。

兩個瀏覽器索引標籤會顯示相同的資訊。

變更的預算，第一個瀏覽器索引標籤，然後按一下**儲存**。

瀏覽器會顯示已變更的值與更新的 rowVersion 指標的索引頁面。 請注意更新的 rowVersion 指標，它會顯示在 [其他] 索引標籤中的第二個回傳。

刪除測試部門中的第二個索引標籤。並行處理錯誤會顯示資料庫的目前值。 按一下**刪除**刪除實體，除非`RowVersion`已 updated.department 已被刪除。

請參閱[繼承](xref:data/ef-mvc/inheritance)如何繼承的資料模型。

### <a name="additional-resources"></a>其他資源

* [在 EF 核心中的並行語彙基元](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [處理 EF 核心中的並行存取](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[上一步](xref:data/ef-rp/update-related-data)
