---
title: "使用 EF 核心並行-8 10 個 ASP.NET Core MVC"
author: tdykstra
description: "本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: eee84fe0fbec6ed772342d09931986994903906a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>處理並行衝突的 EF Core 與 ASP.NET Core MVC 教學課程 (10-8)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在先前的教學課程中，您將學會如何更新資料。 本教學課程會示範如何處理衝突，當多位使用者同時更新相同的實體。

您要建立網頁，以使用 Department 實體，以及處理的並行錯誤。 下圖顯示的編輯和刪除的頁面，包括發生並行衝突時，會顯示一些訊息。

![部門編輯頁面](concurrency/_static/edit-error.png)

![部門刪除頁面](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>並行衝突

當一位使用者才能加以編輯，顯示實體的資料，然後另一位使用者更新相同的實體資料時的第一個使用者變更寫入資料庫之前，就會發生並行衝突。 如果您未啟用的這類的衝突偵測，所有人員都更新資料庫最後會覆寫其他使用者的變更。 在許多應用程式，這項風險是可接受： 如果有少數的使用者或一些更新，或如果不是最關鍵的某些變更會覆寫時，如果並行程式設計的成本可能會超過的好處。 在此情況下，您不需要設定應用程式處理並行存取衝突。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

如果您的應用程式需要以防止資料意外遺失並行案例中的，方法之一是使用資料庫鎖定。 這就叫做封閉式並行存取。 例如，從資料庫讀取資料列之前，您要求的鎖定唯讀或 update 存取權。 如果您鎖定的資料列進行 update 存取權，允許任何其他使用者鎖定資料列的唯讀或更新存取權，因為他們會取得正在變更的資料副本。 如果鎖定為唯讀存取的資料列時，其他人可以唯讀存取權，但不適用於更新也鎖定。

管理鎖定有缺點。 是相當複雜的程式。 這需要大量的資料庫管理資源，而且它會造成效能問題的應用程式的使用者數目會增加。 基於這些理由，並非所有的資料庫管理系統支援封閉式並行存取。 Entity Framework Core 提供的內建支援，與本教學課程不會顯示如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取另一種是開放式並行存取。 開放式並行存取表示允許發生並行衝突，然後適當地回應如果沒有的話。 例如，Jane 造訪部門編輯網頁，並從 $350,000.00 變更 $0.00 英文部門的預算金額。

![將變更為 0 的預算](concurrency/_static/change-budget.png)

Jane 按一下之前**儲存**，John 造訪相同頁面上，並從 2007 年 9 月 1 日的開始日期 欄位變成 2013 年 9 月 1 日。

![變更開始日期至 2013](concurrency/_static/change-date.png)

Jane 按一下**儲存**第一次，並觀察其瀏覽器會返回索引頁面時變更。

![變更為零的預算](concurrency/_static/budget-zero.png)

然後 John 按**儲存**上仍會顯示 $350,000.00 的預算的編輯頁面。 接下來的情況取決於您如何處理並行衝突。

某些選項包括：

* 您可以追蹤的哪些使用者已修改的屬性，並更新只對應的資料行在資料庫中。

     在範例案例中，任何資料將不會遺失，因為兩位使用者已更新不同的屬性。 下一次有人瀏覽英文的部門，就會看到 Jane 的和 John 的變更-2013 年 9 月 1 日的開始日期和零美元的預算。 這種更新方法可以減少可能會導致資料遺失的衝突數目，但它無法避免資料遺失，如果相同的實體屬性進行競爭的變更。 Entity Framework 是否這種方式運作，端視實作更新程式碼的方式而定。 它通常中並不實用的 web 應用程式，因為它可能需要您維護大量狀態，若要追蹤的所有實體的原始屬性值以及新的值。 維護大量的狀態會影響應用程式效能，因為它需要的伺服器資源，或必須包含在網頁上 （例如，在隱藏的欄位） 或在 cookie 中。

* 您可以讓 John 的變更覆寫 Jane 的變更。

     在下一次有人瀏覽英文部門，他們會看到時間 2013 年 9 月 1 日和還原 $350,000.00 值。 這稱為*用戶端獲勝*或*Wins 中的最後一個*案例。 （從用戶端的所有值都優先於什麼是資料存放區中。）所述的簡介本節中，如果您不執行任何撰寫的程式碼的並行處理，這會自動發生。

* 您可以在資料庫中更新，防止 John 的變更。

     一般而言，您會顯示錯誤訊息、 顯示他目前的狀態資料，並允許他他仍然想要讓它們重新套用變更。 這稱為*存放區 Wins*案例。 （資料存放區的值優先於用戶端所送出的值。）您將在本教學課程實作的儲存區 Wins 的案例。 這個方法會確保使用者正在發出警示，這為什麼不會覆寫任何變更。

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

您可以藉由處理解決衝突`DbConcurrencyException`Entity Framework 就會擲回的例外狀況。 若要知道何時擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須設定資料庫和資料模型適當。 啟用衝突偵測的一些選項如下：

* 在資料庫資料表中，包含可用來判斷當資料列已經變更的追蹤資料行。 接著，您可以設定 Entity Framework 了包含該資料行，在 where 子句的 SQL Update 或 Delete 命令。

     追蹤資料行的資料類型通常是`rowversion`。 `rowversion`值是連續的數字已遞增資料列更新一次。 在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行 （原始資料列版本） 的原始值。 如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此 Update 或 Delete 陳述式找不到資料列更新，因為 Where 子句。 時，Entity Framework 會發現的任何資料列已更新的 Update 或 Delete 命令時 （亦即，受影響的資料列數目為零） 時，它可解譯的並行存取衝突。

* 設定 Entity Framework 了包含每個資料行的原始值的資料表中的 Where 子句的 Update 和 Delete 命令。

     如同第一個選項，如果資料列中的任何項目已變更第一次讀取的資料列，因為 Where 子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。 對於具有許多資料行的資料庫資料表，這種方法可能會導致非常大的 Where 子句，而且可能需要您維護大量的狀態。 如前文所述，維護大量的狀態可能會影響應用程式的效能。 因此這種方法通常不建議，並不在本教學課程使用的方法。

     如果您想要實作這個方法來同步存取，您必須將您想要加入追蹤的並行存取實體中的所有非-主索引鍵屬性標記`ConcurrencyCheck`這些屬性。 該變更可讓 Entity Framework 了包含 「 SQL Where 子句的 Update 和 Delete 陳述式中的所有資料行。

在本教學課程的其餘部分，您會加入`rowversion`追蹤 Department 實體的屬性，建立控制器和檢視，並進行測試，確認一切運作正常。

## <a name="add-a-tracking-property-to-the-department-entity"></a>將追蹤的屬性加入 Department 實體

在*Models/Department.cs*，加入名為 RowVersion 的追蹤屬性：

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp`屬性會指定此資料行，將會包含在 where 子句的 Update 和 Delete 命令傳送至資料庫。 該屬性稱為`Timestamp`因為舊版的 SQL Server 使用 SQL`timestamp`資料類型，再將 SQL`rowversion`取代它。 .NET 類型`rowversion`是位元組陣列。

如果您想要使用 fluent API，您可以使用`IsConcurrencyToken`方法 (在*Data/SchoolContext.cs*) 來指定追蹤屬性，如下列範例所示：

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

將屬性加入您變更資料庫模型，因此您必須進行其他移轉。

儲存您的變更和建置專案時，，然後在 [命令] 視窗中輸入下列命令：

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>建立部門控制器和檢視

如先前般學生、 課程、 和講師，scaffold 部門控制器和檢視。

![Scaffold 部門](concurrency/_static/add-departments-controller.png)

在*DepartmentsController.cs*檔案中，將所有的四個出現的"FirstMidName"變更成"FullName 」，讓部門系統管理員的下拉式清單將包含講師的完整名稱，而不是最後一個的名稱。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>更新部門索引檢視

Scaffolding 引擎建立 RowVersion 資料行在索引檢視中，但不應該顯示該欄位。

中的程式碼取代*Views/Departments/Index.cshtml*為下列程式碼。

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

這樣會變更 「 部門 」 標題、 刪除 RowVersion 資料行中，並且系統管理員會顯示完整名稱，而非第一個名稱。

## <a name="update-the-edit-methods-in-the-departments-controller"></a>更新部門控制器中的編輯方法

在這兩個 HttpGet`Edit`方法和`Details`方法，加入`AsNoTracking`。 在 HttpGet`Edit`方法中，新增系統管理員的積極式載入。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

將現有的程式碼取代 HttpPost`Edit`方法取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

程式碼首先會嘗試讀取要更新的部門。 如果`SingleOrDefaultAsync`方法會傳回 null，另一個使用者已刪除的部門。 在此情況下的程式碼會使用已張貼的表單值建立 department 實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。 或者，您不會有重新建立 department 實體，如果您沒有顯示 [部門] 欄位顯示僅為錯誤訊息。

檢視會儲存原始`RowVersion`隱藏的欄位，而且這個方法中的值會收到在該值`rowVersion`參數。 之前先呼叫`SaveChanges`，您必須將它放在原始`RowVersion`屬性值在`OriginalValues`實體的集合。

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

當 Entity Framework 建立 SQL UPDATE 命令時，該命令會將包含 WHERE 子句會尋找具有原始的資料列，然後`RowVersion`值。 如果任何資料列不受到 UPDATE 命令 (沒有資料列的原始`RowVersion`值)，Entity Framework 就會擲回`DbUpdateConcurrencyException`例外狀況。

針對該例外狀況的 catch 區塊中的程式碼取得受影響的 Department 實體具有更新的值從`Entries`例外狀況物件上的屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries`集合將會有一個`EntityEntry`物件。  您可以使用該物件，以取得新的使用者所輸入的值和目前的資料庫值。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

程式碼會加入每個資料行具有不同的資料庫值從使用者所輸入的編輯頁面上的自訂錯誤訊息 （只有一個欄位此處為畫面簡潔起見）。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

最後，此程式碼設定`RowVersion`值`departmentToUpdate`從資料庫擷取新的值。 這個新`RowVersion`值將會儲存在隱藏的欄位中，每當使用者按一下 編輯 頁面會重新顯示和下一步 時**儲存**，時，會發生因為編輯頁面的顯示會攔截到的並行錯誤。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove`陳述式是必要的因為`ModelState`具有舊`RowVersion`值。 在檢視中，`ModelState`欄位會優先於模型屬性的值兩者均存在時的值。

## <a name="update-the-department-edit-view"></a>更新部門編輯檢視

在*Views/Departments/Edit.cshtml*，進行下列變更：

* 加入隱藏的欄位，以儲存`RowVersion`屬性值，緊接的隱藏的欄位`DepartmentID`屬性。

* 下拉式清單中加入 「 選取系統管理員 」 選項。

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>在 [編輯] 頁面中測試並行衝突

執行應用程式，並移至部門索引頁面。 以滑鼠右鍵按一下**編輯**英文部門並選取超連結**新索引標籤中開啟**，然後按一下 **編輯**英文部門的超連結。 兩個瀏覽器索引標籤現在會顯示相同的資訊。

變更第一個瀏覽器索引標籤中的欄位，然後按一下**儲存**。

![部門編輯變更後的第 1 頁](concurrency/_static/edit-after-change-1.png)

瀏覽器會顯示已變更值的索引頁面。

變更第二個瀏覽器索引標籤中的欄位。

![部門編輯變更後的第 2 頁](concurrency/_static/edit-after-change-2.png)

按一下 [儲存] 。 您會看到一則錯誤訊息：

![部門編輯頁面錯誤訊息](concurrency/_static/edit-error.png)

按一下**儲存**一次。 會儲存在第二個瀏覽器索引標籤中所輸入的值。 索引頁出現時，您會看到已儲存的值。

## <a name="update-the-delete-page"></a>更新刪除頁面

刪除設定 頁面上，針對 Entity Framework 會偵測並行衝突造成的其他人其他類似的方式編輯部門。 當 HttpGet`Delete`方法會顯示 [確認] 檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。 值，就可以使用 HttpPost`Delete`使用者確認刪除作業時所呼叫的方法。 當 Entity Framework 建立 SQL DELETE 命令時，會包含 WHERE 子句使用原始`RowVersion`值。 如果命令結果中零個資料列受影響 （已顯示 [刪除確認] 頁面之後，資料列已變更的意義），則會擲回並行存取例外狀況，以及 HttpGet`Delete`為 true，才能重新顯示的錯誤設定旗標呼叫方法確認頁面，並出現錯誤訊息。 此外，也可以零個資料列已受到影響，因為資料列已刪除另一位使用者，因此在此情況下不會顯示錯誤訊息。

### <a name="update-the-delete-methods-in-the-departments-controller"></a>更新部門控制器中的 Delete 方法

在*DepartmentsController.cs*，取代 HttpGet`Delete`方法取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

方法會接受選擇性參數，指出頁面是否正在重新顯示之後並行錯誤。 如果這個旗標為 true，部門指定不再存在，它已被其他使用者刪除。 在此情況下，程式碼重新導向至索引頁面。  如果這個旗標為 true，該部門存在，它已由其他使用者變更。 在此情況下，程式碼將錯誤訊息傳送至檢視使用`ViewData`。  

取代的程式碼中 HttpPost`Delete`方法 (名為`DeleteConfirmed`) 取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

在您剛才取代的 scaffold 程式碼，這個方法會接受只記錄識別碼：


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

您已將此參數變更為 Department 實體的執行個體，建立的模型繫結器。 這可讓 EF 存取 RowVersion 屬性值，除了記錄索引鍵。

```csharp
public async Task<IActionResult> Delete(Department department)
```

您也已經變更的動作方法名稱`DeleteConfirmed`至`Delete`。 Scaffold 的程式碼使用了名稱`DeleteConfirmed`給 HttpPost 方法，唯一的簽章。 （在 CLR 需要有不同的方法參數的多載的方法）。現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用 HttpPost 和 HttpGet 方法中刪除相同的名稱。

若已刪除的部門，`AnyAsync`方法會傳回 false，而應用程式就會回到索引方法。

如果會攔截到並行處理錯誤，程式碼會重新顯示 [刪除確認] 頁面，並提供旗標，指出它應該會顯示並行錯誤訊息。

### <a name="update-the-delete-view"></a>更新刪除檢視

在*Views/Departments/Delete.cshtml*，scaffold 的程式碼取代下列程式碼新增的錯誤訊息欄位和 DepartmentID 和 RowVersion 屬性的隱藏的欄位。 所做的變更會反白顯示。

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

這會進行下列變更：

* 新增錯誤訊息之間`h2`和`h3`標題。

* 取代中的 FullName FirstMidName**管理員**欄位。

* 移除 RowVersion 的欄位。

* 將隱藏的欄位加入`RowVersion`屬性。

執行應用程式，並移至部門索引頁面。 以滑鼠右鍵按一下**刪除**英文部門並選取超連結**新索引標籤中開啟**，然後在第一個索引標籤中按一下**編輯**英文部門的超連結。

在第一個視窗中，變更其中一個值，然後按一下**儲存**:

![刪除前變更後的部門編輯頁面](concurrency/_static/edit-after-change-for-delete.png)

在第二個索引標籤上，按一下**刪除**。 您看到並行處理錯誤訊息和部門值都以目前是在資料庫中重新整理。

![與並行錯誤部門刪除確認 頁面](concurrency/_static/delete-error.png)

如果您按一下**刪除**同樣地，您要重新導向，索引頁，會顯示已刪除的部門。

## <a name="update-details-and-create-views"></a>更新詳細資料，並建立檢視

您可以選擇性地清除 scaffold 的程式碼，詳細資料，並建立檢視。

中的程式碼取代*Views/Departments/Details.cshtml*刪除 RowVersion 資料行，並顯示系統管理員的完整名稱。

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

中的程式碼取代*Views/Departments/Create.cshtml*加入下拉式清單中選取的選項。

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>總結

如此即完成處理並行衝突的簡介。 如需如何處理 EF 核心中的並行存取的詳細資訊，請參閱[並行衝突](https://docs.microsoft.com/ef/core/saving/concurrency)。 下一個教學課程會示範如何實作講師及學生實體資料表每個階層繼承。

>[!div class="step-by-step"]
[上一頁](update-related-data.md)
[下一頁](inheritance.md)  
