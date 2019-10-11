---
title: 教學課程：處理並行 - ASP.NET MVC 搭配 EF Core
description: 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 227128607460f9b5821bd0697fde3f393cf6daa9
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259436"
---
# <a name="tutorial-handle-concurrency---aspnet-mvc-with-ef-core"></a>教學課程：處理並行 - ASP.NET MVC 搭配 EF Core

在先前的教學課程中，您學會了如何更新資料。 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。

您會建立操作 Department 實體的網頁，並處理並行錯誤。 下列圖例顯示了 [編輯] 和 [刪除] 頁面，包括一些發生並行衝突時會顯示的訊息。

![Department [編輯] 頁面](concurrency/_static/edit-error.png)

![Department [刪除] 頁面](concurrency/_static/delete-error.png)

在本教學課程中，您已：

> [!div class="checklist"]
> * 了解並行衝突
> * 新增追蹤屬性
> * 建立 Departments 控制器和檢視
> * 更新 [索引] 檢視
> * 更新 [編輯] 方法
> * 更新 [編輯] 檢視
> * 測試並行衝突
> * 更新 *Delete* 頁面
> * 更新 [詳細資料] 及 [建立] 檢視

## <a name="prerequisites"></a>必要條件

* [更新相關資料](update-related-data.md)

## <a name="concurrency-conflicts"></a>並行衝突

當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。 若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。 在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。 在此情況下，您便不需要設定應用程式來處理並行衝突。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 (鎖定)

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這稱之為封閉式並行存取。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定有幾個缺點。 其程式可能相當複雜。 這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework Core 並未提供內建支援，並且此教學課程也不會教導您如何實作封閉式並行存取。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取的替代方案便是開放式並行存取。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 例如，Jane 造訪了 Department [編輯] 頁面，然後將英文部門的預算金額從美金 $350,000.00 元調整到美金 $0.00 元。

![將預算變更為 0](concurrency/_static/change-budget.png)

在 Jane 按一下 [儲存] 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

Jana 先按了一下 [儲存]，並且在瀏覽器返回 [索引] 頁面時看到她做出的變更。

![預算已變更為 0](concurrency/_static/budget-zero.png)

然後 John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]。 接下來發生的情況便是由您處理並行衝突的方式決定。

一部分選項包括下列項目：

* 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。

     在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更 -- 開始日期為 2013/9/1，預算為美金 0 元。 這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。 Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。 通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。 維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。

* 您可以讓 John 的變更覆寫 Jane 的變更。

     下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，且預算的金額已還原到美金 $350,000.00 元。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 (所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。

* 您可以防止 John 的變更更新到資料庫中。

     一般而言，您會顯示一個錯誤訊息，將資料目前的狀態顯示給他，然後允許他重新套用他所作出的變更 (若他還是要變更的話)。 這稱為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

您可以透過處理 Entity Framework 擲回的 `DbConcurrencyException` 例外狀況來解析衝突。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

* 在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。 您接著便可以設定 Entity Framework，使其在 SQL Update 或 Delete 命令的 Where 子句中包含該資料行。

     追蹤資料行的資料類型通常是 `rowversion`。 `rowversion` 的值是一個循序號碼，每一次資料列更新時都會遞增。 在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行原始的值 (原始的資料列版本)。 若正在更新的資料列已由其他使用者變更，`rowversion` 中的值便會與原始的值不同，因此 Update 或 Delete 陳述式便會因為 Where 子句而無法找到資料列進行更新。 當 Entity Framework 發現 Update 或 Delete 命令沒有更新任何資料列時 (即受影響的資料列數目為 0)，它便會將其解譯為並行衝突。

* 設定 Entity Framework，使其在 Update 和 Delete 命令中的 Where 子句裡包含資料表中每個資料行原始的值。

     如同第一個選項，若資料列中在一開始讀取之後有任何資料產生變更，Where 子句便不會傳回任何資料列以進行更新，Entity Framework 便會將其解譯為並行衝突。 針對擁有許多資料行的資料庫資料表，此方法可能導致非常龐大的 Where 子句，並且可能會需要您維持龐大數量的狀態。 如前文所述，維持大量的狀態可能會影響應用程式的效能。 因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。

     若您想要實作此方法以處理並行衝突，您必須藉由新增 `ConcurrencyCheck` 屬性來標記所有實體中您想要追蹤並行的非主索引鍵屬性。 該變更會使 Entity Framework 能在 Update 和 Delete 陳述式的 SQL Where 子句中包含所有資料行。

在本教學課程的其餘部分，您會將一個 `rowversion` 追蹤屬性新增到 Department 實體，建立控制器和檢視，然後進行測試以驗證一切都運作正常。

## <a name="add-a-tracking-property"></a>新增追蹤屬性

在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp` 屬性會指定此資料行會包含在傳送到資料庫之 Update 和 Delete 命令的 Where 子句中。 該屬性稱為 `Timestamp`，因為先前版本的 SQL Server 在以 SQL `rowversion` 取代之前使用了 SQL `timestamp` 資料類型。 `rowversion` 的 .NET 類型為位元組陣列。

若您偏好使用 Fluent API，您可以使用 `IsConcurrencyToken` 方法 (位於 *Data/SchoolContext.cs* 中) 來指定追蹤屬性，如以下範例所示：

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。

儲存您的變更並建置專案，然後在命令視窗中輸入以下命令：

```dotnetcli
dotnet ef migrations add RowVersion
```

```dotnetcli
dotnet ef database update
```

## <a name="create-departments-controller-and-views"></a>建立 Departments 控制器和檢視

Scaffold Departments 控制器和檢視，如同您先前為 Students、Courses 和 Instructors 所做的。

![Scaffold Department](concurrency/_static/add-departments-controller.png)

在  *DepartmentsController.cs* 檔案中，將四個 "FirstMidName" 變更為 "FullName" ，使部門系統管理員下拉式清單可包含講師的完整名稱，而非只有姓氏。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-index-view"></a>更新 [索引] 檢視

Scaffolding 引擎會在 [索引] 檢視中建立 RowVersion 資料行，但該欄位不應該顯示出來。

請以下列程式碼取代 *Views/Departments/Index.cshtml* 中的程式碼。

[!code-html[](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

這會將標題變更為"Departments"，刪除 RowVersion 資料行，並為系統管理員顯示完整的名稱而非只有名字。

## <a name="update-edit-methods"></a>更新 [編輯] 方法

在 HttpGet `Edit` 方法和 `Details` 方法中，新增 `AsNoTracking`。 在 HttpGet `Edit` 方法中，為系統管理員新增積極式載入。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading)]

以下列程式碼取代 HttpPost `Edit` 方法的現有程式碼：

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

程式碼開始時便會嘗試讀取要更新的部門。 若 `FirstOrDefaultAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。 在此情況下，程式碼會使用 POST 表單的值建立部門實體，使 [編輯] 頁面仍然可以重新顯示，並加上錯誤訊息。 或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。

檢視會在隱藏欄位中儲存原始的 `RowVersion` 值，並且此方法會在 `rowVersion` 參數中接收該值。 在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

則當 Entity Framework 建立 SQL UPDATE 命令時，該命令便會包含尋找擁有原始 `RowVersion` 值之資料列的 WHERE 子句。 若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則 Entity Framework 便會擲回 `DbUpdateConcurrencyException` 例外狀況。

針對該例外狀況的 catch 區塊會取得受影響的 Department 實體。該實體具有從例外物件上的 `Entries` 屬性取得的更新值。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries` 集合只會有一個 `EntityEntry` 物件。  您可以使用該物件來取得由使用者輸入的新值，以及目前資料庫的值。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

程式碼會為每個資料庫的值與使用者在編輯頁面上輸入的值不同的資料行新增一個自訂錯誤訊息 (為了簡化，這裡只顯示了一個欄位)。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

最後，程式碼會將 `departmentToUpdate` 的 `RowVersion` 值設為從資料庫取得的新值。 這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。 在檢視中，當兩者同時存在時，欄位的 `ModelState` 值會優先於模型屬性值。

## <a name="update-edit-view"></a>更新 [編輯] 檢視

在 *Views/Departments/Edit.cshtml* 中，進行下列變更：

* 在 `DepartmentID`  屬性的隱藏欄位之後立即新增另一個隱藏欄位以儲存 `RowVersion` 屬性值。

* 將 [選取系統管理員] 選項新增到下拉式清單中。

[!code-html[](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts"></a>測試並行衝突

執行應用程式，移至 Departments [索引] 頁面。 以滑鼠右鍵按一下 English 部門的**編輯** 超連結，然後選取 [開啟新的索引標籤]，然後按一下 English 部門的**編輯**超連結。 兩個瀏覽器索引標籤現在會顯示相同的資訊。

變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

瀏覽器會顯示索引頁面，當中包含了變更之後的值。

變更第二個瀏覽器索引標籤中的欄位。

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

按一下 [儲存]。 您會看到一個錯誤訊息：

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

再按一下 [儲存]。 您在第二個瀏覽器索引標籤中輸入的值已儲存。 您會在索引頁面出現時看到儲存的值。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。 當 HttpGet `Delete` 方法顯示確認檢視時，檢視會在隱藏欄位中包含原始的 `RowVersion` 值。 該值接著便可供當使用者確認刪除時所呼叫的 HttpPost `Delete` 方法使用。 當 Entity Framework 建立 SQL DELETE 命令時，它會包含一個具有原始 `RowVersion` 值的 WHERE 子句。 若命令執行的結果為受影響的資料列為零 (即資料列在 [刪除] 確認頁面顯示後發生變更)，便會擲回並行衝突例外狀況，並呼叫 HttpGet `Delete` 方法，並將錯誤旗標設為 true，以在重新顯示的確認頁面上顯示一個錯誤訊息。 當受影響的資料列為零時，也有可能是該資料列已由別的使用者刪除；在這種情況下，將不會顯示任何錯誤訊息。

### <a name="update-the-delete-methods-in-the-departments-controller"></a>更新 Departments 控制器中的 Delete 方法

在 *DepartmentsController.cs* 中，以下列程式碼取代 HttpGet `Delete` 方法：

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。 若此旗標為 true，且指定的部門已不存在，表示該部門已遭其他使用者刪除。 在此情況下，程式碼會重新導向至索引頁面。  若此旗標為 true，但指定的部門仍然存在，表示該部門已遭其他使用者變更。 在此情況下，程式碼會使用 `ViewData` 傳送一個錯誤訊息到檢視。

以下列程式碼取代 HttpPost `Delete` 方法 (名為 `DeleteConfirmed`) 中的程式碼：

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：

```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

您已將此參數變更為由模型繫結器建立的 Department 實體執行個體。 這可讓 EF 除了記錄金鑰外，也能存取 RowVersion 屬性值。

```csharp
public async Task<IActionResult> Delete(Department department)
```

您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。 Scaffold 程式碼使用了 `DeleteConfirmed` 的名稱來給予 HttpPost 方法一個唯一的簽章。 (CLR 要求多載方法必須要有不同的方法參數。)現在，該簽章已為唯一的簽章，您現在可以遵循 MVC 慣例，然後為 HttpPost 及 HttpGet 刪除方法使用相同的名稱。

若部門已遭刪除，`AnyAsync` 方法會傳回 false，應用程式便會直接返回 Index 方法。

若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。

### <a name="update-the-delete-view"></a>更新 [刪除] 檢視

在 *Views/Departments/Delete.cshtml* 中，以下列新增錯誤訊息欄位和 DepartmentID 及 RowVersion 屬性隱藏欄位的程式碼取代 Scaffold 程式碼。 所做的變更已醒目提示。

[!code-html[](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

這會進行下列變更：

* 在 `h2` 和 `h3` 標題之間新增一個錯誤訊息。

* 在 [系統管理員] 欄位中將 FirstMidName 取代為 FullName。

* 移除 RowVersion 欄位。

* 為 `RowVersion` 屬性新增一個隱藏欄位。

執行應用程式，移至 Departments [索引] 頁面。 以滑鼠右鍵按一下 English 部門的**刪除** 超連結，然後選取 [開啟新的索引標籤]，然後在第一個索引標籤中按一下 English 部門的**編輯**超連結。

在第一個視窗中，變更其中一個值，然後按一下 [儲存]：

![刪除前變更後的 Department [編輯] 頁面](concurrency/_static/edit-after-change-for-delete.png)

在第二個索引標籤中，按一下 [刪除]。 您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。

![Department [刪除] 確認頁面，其中包含了並行錯誤](concurrency/_static/delete-error.png)

若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。

## <a name="update-details-and-create-views"></a>更新 [詳細資料] 及 [建立] 檢視

您也可以選擇性的清理 [詳細資料] 和 [建立] 檢視中的 Scaffold 程式碼。

取代 *Views/Departments/Details.cshtml* 中的程式碼以刪除 RowVersion 資料行並顯示系統管理員的完整名稱。

[!code-html[](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

取代 *Views/Departments/Create.cshtml* 中的程式碼來將 [選取 ] 選項新增到下拉式清單中。

[!code-html[](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="get-the-code"></a>取得程式碼

[下載或檢視已完成的應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>其他資源

 如需如何在 EF Core 中處理並行的詳細資訊，請參閱[並行衝突](/ef/core/saving/concurrency)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 了解並行衝突
> * 新增追蹤屬性
> * 建立 Departments 控制器和檢視
> * 更新 [索引] 檢視
> * 更新 [編輯] 方法
> * 更新 [編輯] 檢視
> * 測試並行衝突
> * 更新 [刪除] 頁面
> * 更新 [詳細資料] 和 [建立] 檢視

若要了解如何為 Instructor 和 Student 實體實作依階層建立資料表的繼承，請前往下一個教學課程。

> [!div class="nextstepaction"]
> [下一步：實作依階層建立資料表的繼承](inheritance.md)
