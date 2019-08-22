---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8
author: tdykstra
description: 示範如何以 EF Core 來建立、讀取、更新、刪除。
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/crud
ms.openlocfilehash: 57c4a1789d54c29a28ba7e67a1d15815415a461c
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583124"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

在本教學課程中，將會檢閱並自訂 Scaffold CRUD (建立、讀取、更新、刪除)。

## <a name="no-repository"></a>沒有任何存放庫

有些開發人員會使用服務層或存放庫模式來建立介於 UI (Razor Pages) 和資料存取層之間的抽象層。 本教學課程不會這麼做。 為降低複雜性並將教學課程聚焦於 EF Core，EF Core 程式碼會直接新增至頁面模型類別中。 

## <a name="update-the-details-page"></a>更新 [詳細資料] 頁面

Students 頁面的 Scaffold 程式碼不包含註冊資料。 在本節中，您會將註冊新增至 [詳細資料] 頁面。

### <a name="read-enrollments"></a>讀取註冊

若要在頁面上顯示學生的註冊資料，您必須讀取該資料。 *Pages/Students/Details.cshtml.cs* 中的 scaffold 程式碼只會讀取 Students 資料，而不包含 Enrollment 資料：

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/Details1.cshtml.cs?name=snippet_OnGetAsync&highlight=8)]

將 `OnGetAsync` 方法取代為下列程式碼，以讀取所選學生的註冊資料。 所做的變更已醒目標示。

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Details.cshtml.cs?name=snippet_OnGetAsync&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) 及 [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。 您可在[讀取相關資料](xref:data/ef-rp/read-related-data)教學課程中詳細檢視這些方法。

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 方法在傳回實體未在目前內容中更新的情況下可改善效能。 `AsNoTracking` 稍後在本教學課程中將會討論。

### <a name="display-enrollments"></a>顯示註冊

將 *Pages/Students/Details.cshtml* 中的程式碼取代為下列程式碼以顯示註冊清單。 所做的變更已醒目標示。

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Details.cshtml?highlight=32-53)]

上述程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。 針對每個註冊，會顯示課程標題及成績。 課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。

執行應用程式，選取 [Students]  索引標籤，然後按一下學生的 [詳細資料]  連結。 將會顯示所選取學生的課程及成績清單。

### <a name="ways-to-read-one-entity"></a>讀取單一實體的方式

產生的程式碼會使用 [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 來讀取單一實體。 如果找不到任何內容，則此方法會傳回 Null；否則會傳回符合查詢篩選準則的第一個資料列。 `FirstOrDefaultAsync` 通常是比下列替代選項更好的選擇：

* [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) - 如果有超過一個滿足查詢篩選準則的實體，就會擲回例外狀況。 為了判斷查詢是否可能傳回超過一個資料列，`SingleOrDefaultAsync` 會嘗試提取多個資料列。 如果查詢只能傳回單一實體 (如同在搜尋唯一索引鍵時一樣)，則此額外工作是不必要的。
* [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) - 尋找含有主索引鍵 (PK) 的實體。 如果內容正在追蹤具有主索引鍵的實體，則會在沒有傳送要求至資料庫的情況下傳回。 此方法已經過最佳化以便查閱單一實體，但是您無法使用 `FindAsync` 呼叫 `Include`。  因此如果需要相關資料，`FirstOrDefaultAsync` 是較好的選擇。

### <a name="route-data-vs-query-string"></a>路由資料與查詢字串

[詳細資料] 頁面的 URL 是 `https://localhost:<port>/Students/Details?id=1`。 實體的主要索引鍵值位於查詢字串中。 某些開發人員偏好在路由資料中傳遞索引鍵值：`https://localhost:<port>/Students/Details/1`。 如需詳細資訊，請參閱[更新產生的程式碼](xref:tutorials/razor-pages/da1#update-the-generated-code)。

## <a name="update-the-create-page"></a>更新 [建立] 頁面

[建立] 頁面的 scaffold `OnPostAsync` 程式碼容易受到[大量指派](#overposting)攻擊。 使用下列程式碼取代 *Pages/Students/Create.cshtml.cs* 中的 `OnPostAsync` 方法。

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

上述程式碼會建立 Student 物件，然後使用張貼的表單欄位來更新 Student 物件屬性。 [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) 方法：

* 使用 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 中 [PageCoNtext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) 屬性的張貼表單值。
* 僅更新列出的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。
* 尋找具有 "student" 前置詞的表單欄位。 例如，`Student.FirstMidName`。 不區分大小寫。
* 使用[模型繫結](xref:mvc/models/model-binding)系統，將字串中表單值轉換成 `Student` 模型中的型別。 例如，`EnrollmentDate` 必須轉換成 DateTime。

執行應用程式，並建立 Student 實體來測試 [建立] 頁面。

## <a name="overposting"></a>大量指派 (overposting)

使用 `TryUpdateModel` 來更新具有已張貼值欄位是最安全的做法，因為它會防止大量指派 (overposting)。 例如，假設 Student 實體包含了一個此網頁不應更新或新增的 `Secret` 屬性：

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

即使應用程式在建立或更新 Razor 頁面上沒有 `Secret` 欄位，駭客仍然可以藉由大量指派來設定 `Secret` 值。 駭客仍可能使用 Fiddler 等工具，或是撰寫 JavaScript，來張貼 `Secret` 表單值。 原始程式碼並不會限制模型繫結器在建立 Student 執行個體時所使用的欄位。

無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到資料庫中。 下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。

![Fiddler 新增 [Secret] 欄位](../ef-mvc/crud/_static/fiddler.png)

"OverPost" 值已成功新增至插入資料列的 `Secret` 屬性。 即使應用程式設計師從未打算在 [建立] 頁面設定 `Secret` 屬性，還是會發生此情況。

### <a name="view-model"></a>檢視模型

檢視模型提供防止大量指派 (overposting) 的替代方法。

應用程式模型通常稱為網域模型。 網域模型通常會包含資料庫中對應實體所需要的所有屬性。 檢視模型只包含 UI 所需要的屬性 (例如 [建立] 頁面)。

除了檢視模型之外，某些應用程式會使用繫結模型或輸入模型，在 Razor 頁面的頁面模型類別和瀏覽器之間傳遞資料。 

請看看下列 `Student` 檢視模型：

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentVM.cs)]

下列程式碼會使用 `StudentVM` 檢視模型來建立一名新學生：

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) 會設定這個物件的值，方法是藉由從其他 [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) 物件中讀取值。 `SetValues` 使用屬性名稱比對。 檢視模型類型不需要與模型類型相關，只需要有符合的屬性。

使用 `StudentVM` 需要 [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml) 更新為使用 `StudentVM` 而不是 `Student`。

## <a name="update-the-edit-page"></a>更新 [編輯] 頁面

在 *Pages/Students/Create.cshtml.cs* 中，使用下列程式碼取代 `OnGetAsync` 和 `OnPostAsync` 方法。

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Edit.cshtml.cs?name=snippet_OnGetPost)]

程式碼的變更與 [建立] 頁面相似，但有一些例外狀況：

* `FirstOrDefaultAsync` 已取代為 [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync)。 如果您不需要包含相關資料，則 `FindAsync` 效率較高。
* `OnPostAsync` 具有 `id` 參數。
* 會從資料庫中擷取出目前的學生，而不會建立一個空白的學生資料。

執行應用程式，並藉由建立和編輯學生來進行測試。

## <a name="entity-states"></a>實體狀態

無論記憶體中實體是否與資料庫中相對應資料列同步，資料庫內容都會持續追蹤。 呼叫 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時，此追蹤資訊會決定該怎麼做。 例如，當傳遞一個新的實體到 [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) 方法時，該實體的狀態便會設定為 [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added)。 呼叫 `SaveChangesAsync` 時，資料庫內容會發出 SQL INSERT 命令。

實體可為[下列狀態](/dotnet/api/microsoft.entityframeworkcore.entitystate)中的其中一個：

* `Added`：實體尚未存在於資料庫中。 `SaveChanges` 方法會發出 INSERT 陳述式。

* `Unchanged`：此實體沒有需要儲存的變更。 從資料庫讀取時，此實體將會有這個狀態。

* `Modified`：實體中一部分或全部的屬性值已經過修改。 `SaveChanges` 方法會發出 UPDATE 陳述式。

* `Deleted`：實體已遭標示刪除。 `SaveChanges` 方法會發出 DELETE 陳述式。

* `Detached`：實體未獲得資料庫內容追蹤。

在桌面應用程式中，狀態變更通常會自動進行設定。 實體已讀取、已進行變更，且實體狀態會自動變更為 `Modified`。 呼叫 `SaveChanges` 會產生 SQL UPDATE 陳述式，此陳述式只會更新變更過的屬性。

在 Web 應用程式中，讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。 當呼叫頁面的 `OnPostAsync` 方法時，會發出新的 Web 要求，並且會擁有一個新的 `DbContext` 執行個體。 在新的內容中重新讀取實體時，會模擬桌面處理流程。

## <a name="update-the-delete-page"></a>更新 *Delete* 頁面

在本節中，您將實作一個呼叫至 `SaveChanges` 失敗時的自訂錯誤訊息。

使用下列程式碼取代 *Pages/Students/Delete.cshtml.cs* 中的程式碼。 變更會醒目提示 (除了清除 `using` 陳述式之外)。

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Delete.cshtml.cs?name=snippet_All&highlight=20,22,30,38-41,53-71)]

上述程式碼會將選擇性參數 `saveChangesError` 新增至 `OnGetAsync` 方法簽章。 `saveChangesError` 會顯示出此方法是否已在刪除學生物件失敗後呼叫。 刪除作業可能會因為暫時性的網路問題而失敗。 資料庫位於雲端時，較可能發生暫時性的網路錯誤。 從 UI 中呼叫 [刪除] 頁面 `OnGetAsync` 時，`saveChangesError` 參數為 false。 當 `OnPostAsync` 呼叫 `OnGetAsync` 時 (因刪除作業失敗)，則 `saveChangesError` 參數為 true。

`OnPostAsync` 方法會擷取選取的實體，然後呼叫 [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) 方法來將實體的狀態設定為 `Deleted`。 當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。 如果 `Remove` 失敗：

* 攔截到資料庫例外狀況。
* [刪除] 頁面的 `OnGetAsync` 方法會以 `saveChangesError=true` 呼叫。

將錯誤訊息新增至 [刪除] Razor Pages (*Pages/student/Delete. cshtml*)：

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Delete.cshtml?highlight=10)]

執行應用程式並刪除學生以測試 [刪除] 頁面。

## <a name="next-steps"></a>後續步驟

> [!div class="step-by-step"]
> [上一個教學課程](xref:data/ef-rp/intro)
> [下一個教學課程](xref:data/ef-rp/sort-filter-page)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在本教學課程中，將會檢閱並自訂 Scaffold CRUD (建立、讀取、更新、刪除)。

為降低複雜性並將教學課程聚焦於 EF Core，EF Core 程式碼會使用於頁面模型中。 有些開發人員會使用服務層或儲存機制模式來建立介於 UI (Razor 頁面) 和資料存取層之間的抽象層。

在本教學課程中，會檢查位於 *Students* 資料夾中的 [建立]、[編輯]、[刪除] 和 [詳細資料] Razor 頁面。

Scaffold 程式碼會為 [建立]、[編輯]、[刪除] 頁面使用下列模式：

* 使用 HTTP GET 方法 `OnGetAsync` 來取得並顯示所要求的資料。
* 使用 HTTP POST `OnPostAsync` 方法將變更儲存至資料。

[索引] 頁面和 [詳細資料] 頁面以 HTTP GET 方法 `OnGetAsync` 取得並顯示所要求的資料

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync 與FirstOrDefaultAsync

產生的程式碼會使用 [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)，一般會偏好它而非 [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。

 `FirstOrDefaultAsync` 在擷取單一實體上比 `SingleOrDefaultAsync` 更有效率：

* 除非程式碼需要確認從查詢傳回的實體不多於一個。
* `SingleOrDefaultAsync` 擷取更多資料並進行不必要的工作。
* 如果有多於一個符合篩選組件的實體，`SingleOrDefaultAsync` 將會擲回例外狀況。
* 如果有多於一個符合篩選組件的實體，`FirstOrDefaultAsync` 不會擲回。

<a name="FindAsync"></a>

### <a name="findasync"></a>FindAsync

在大部分的 Scaffold 程式碼中，[FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) 可用來取代 `FirstOrDefaultAsync`。

`FindAsync`：

* 尋找含有主索引鍵 (PK) 的實體。 如果內容正在追蹤含有主索引鍵的實體，會在沒有傳送要求至資料庫的情況下傳回。
* 簡單並精確。
* 已最佳化來查閱單一實體。
* 在某些情況下可具有效能優勢，但是在一般的 Web 應用程式很少見。
* 隱含使用 [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 而不是 [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。

但是如果您想要 `Include` 其他項目，則 `FindAsync` 已不再適用。 這表示您可能要放棄 `FindAsync` 並隨著您的應用程式進行而移至查詢。

## <a name="customize-the-details-page"></a>自訂 [詳細資料] 頁面

瀏覽至 `Pages/Students` 頁面。 在 *Pages/Students/Index.cshtml* 檔案中，**編輯**、**詳細資料**、**刪除** 連結是由[錨點標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)所產生。

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

執行應用程式並選取 [詳細資料]  連結。 URL 的格式為 `http://localhost:5000/Students/Details?id=2` 。 使用 (`?id=2`) 查詢字串來傳遞 Student ID。

更新 [編輯]、[詳細資料] 和 [刪除] Razor 頁面，以使用 `"{id:int}"` 路由範本。 將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。

對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數路由值，將傳回 HTTP 404 (找不到) 錯誤。 例如，`http://localhost:5000/Students/Details` 傳回 404 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

執行應用程式，按一下詳細資料連結，並確認 URL 作為路由資料 (`http://localhost:5000/Students/Details/2`) 來傳遞識別碼。

請勿全域變更 `@page` 至 `@page "{id:int}"`，這麼做會中斷 [首頁] 頁面與 [建立] 頁面之間的連結。

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>新增相關資料

Students [索引] 頁面的 Scaffold 程式碼不包含 `Enrollments` 屬性。 在本節中，`Enrollments` 集合的內容會顯示於 [詳細資料] 頁面。

*Pages/Students/Details.cshtml.cs* 的 `OnGetAsync` 方法使用 `FirstOrDefaultAsync` 方法來擷取單一 `Student` 實體。 請新增下列醒目提示的程式碼：

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) 及 [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。 將會在讀取相關資料教學課程中詳細檢視這些方法。

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 方法在傳回實體未在目前內容中更新的案例下可改善效能。 `AsNoTracking` 稍後在本教學課程中將會討論。

### <a name="display-related-enrollments-on-the-details-page"></a>在 [詳細資料] 頁面上顯示相關的註冊

開啟 *Pages/Students/Details.cshtml*。 請新增下列醒目顯示的程式碼，以顯示一份註冊清單：

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

若在貼上程式碼之後，程式碼的縮排發生錯誤，請按 CTRL-K-D 修正它。

上述程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。 針對每個註冊，會顯示課程標題及成績。 課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。

執行應用程式，選取 [Students]  索引標籤，然後按一下學生的 [詳細資料]  連結。 將會顯示所選取學生的課程及成績清單。

## <a name="update-the-create-page"></a>更新 [建立] 頁面

以下列程式碼更新 *Pages/Students/Create.cshtml.cs* 中的 `OnPostAsync` 方法：

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

檢查 [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) 程式碼：

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

在上述程式碼，`TryUpdateModelAsync<Student>` 從 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 中的 [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) 屬性中，使用已張貼的表單值來嘗試更新 `emptyStudent` 物件。 `TryUpdateModelAsync` 只會更新列出的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。

在上述範例中：

* 第二個引數 (`"student", // Prefix`) 是使用來查閱值的前置詞。 不區分大小寫。
* 已張貼的表單值會使用[模型繫結](xref:mvc/models/model-binding)轉換至 `Student` 模型中的類型。

<a id="overpost"></a>

### <a name="overposting"></a>大量指派 (overposting)

使用 `TryUpdateModel` 來更新具有已張貼值欄位是最安全的做法，因為它會防止大量指派 (overposting)。 例如，假設 Student 實體包含了一個此網頁不應更新或新增的 `Secret` 屬性：

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

即使應用程式在建立/更新 Razor 頁面上沒有 `Secret` 欄位，駭客仍然可以藉由大量指派 (overposting) 來設定 `Secret` 值。 駭客仍可能使用 Fiddler 等工具，或是撰寫 JavaScript，來張貼 `Secret` 表單值。 原始程式碼並不會限制模型繫結器在建立 Student 執行個體時所使用的欄位。

無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到資料庫中。 下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。

![Fiddler 新增 [Secret] 欄位](../ef-mvc/crud/_static/fiddler.png)

"OverPost" 值已成功新增至插入資料列的 `Secret` 屬性。 應用程式設計師從未打算在 [建立] 頁面設定 `Secret` 屬性。

<a name="vm"></a>

### <a name="view-model"></a>檢視模型

檢視模型通常包含屬性中的子集，這些屬性包含在應用程式使用的模型中。 應用程式模型通常稱為網域模型。 網域模型通常會包含資料庫中對應實體所需要的所有屬性。 檢視模型只包含 UI 層所需要的屬性 (例如 [建立] 頁面)。 除了檢視模型之外，某些應用程式會使用繫結模型或輸入模型，在 Razor 頁面的頁面模型類別和瀏覽器之間傳遞資料。 請看看下列 `Student` 檢視模型：

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

檢視模型提供防止大量指派 (overposting) 的替代方法。 檢視模型只包含用來檢視 (顯示) 或更新的屬性。

下列程式碼會使用 `StudentVM` 檢視模型來建立一名新學生：

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) 會設定這個物件的值，方法是藉由從其他 [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) 物件中讀取值。 `SetValues` 使用屬性名稱比對。 檢視模型類型不需要與模型類型相關，只需要有符合的屬性。

使用 `StudentVM` 需要 [CreateVM.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) 更新為使用 `StudentVM`，而不是使用 `Student`。

在 Razor 頁面中，`PageModel` 的衍生類別是檢視模型。

## <a name="update-the-edit-page"></a>更新 [編輯] 頁面

為 [編輯] 頁面更新頁面模型。 主要變更已反白顯示：

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

程式碼的變更與 [建立] 頁面相似，但有一些例外狀況：

* `OnPostAsync` 具有選擇性 `id` 參數。
* 目前的學生資料會從資料庫中擷取出來，而不會建立一個空白的學生資料。
* `FirstOrDefaultAsync` 已取代為 [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync)。 從主索引鍵選取實體時，`FindAsync` 是不錯的選擇。 請參閱 [FindAsync](#FindAsync) 以取得更詳細的資訊。

### <a name="test-the-edit-and-create-pages"></a>測試 [編輯] 頁面和 [建立] 頁面

建立並編輯一些學生實體。

## <a name="entity-states"></a>實體狀態

無論記憶體中的實體是否與資料庫中相對應的資料列同步，資料庫內容都會持續追蹤。 呼叫 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時，資料庫內容的同步資訊會決定該怎麼做。 例如，當傳遞一個新的實體到 [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) 方法時，該實體的狀態便會設定為 [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added)。 呼叫 `SaveChangesAsync` 時，資料庫內容會發出 SQL INSERT 命令。

實體可為[下列狀態](/dotnet/api/microsoft.entityframeworkcore.entitystate)中的其中一個：

* `Added`：實體還不存在 DB 中。 `SaveChanges` 方法會發出 INSERT 陳述式。

* `Unchanged`：此實體沒有需要儲存的變更。 從資料庫讀取時，此實體將會有這個狀態。

* `Modified`：實體中一部分或全部的屬性值已經過修改。 `SaveChanges` 方法會發出 UPDATE 陳述式。

* `Deleted`：實體已遭標示刪除。 `SaveChanges` 方法會發出 DELETE 陳述式。

* `Detached`：實體未獲得 DB 內容追蹤。

在桌面應用程式中，狀態變更通常會自動進行設定。 實體已讀取、已進行變更，且實體狀態會自動變更為 `Modified`。 呼叫 `SaveChanges` 會產生 SQL UPDATE 陳述式，此陳述式只會更新變更過的屬性。

在 Web 應用程式中，讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。 當呼叫頁面的 `OnPostAsync` 方法時，會發出新的 Web 要求，並且會擁有一個新的 `DbContext` 執行個體。 在新的內容中重新讀取實體時，會模擬桌面處理流程。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

在本節中，將新增程式碼來實作一個呼叫至 `SaveChanges` 失敗時的自訂錯誤訊息。 請新增字串來包含可能的錯誤訊息：

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

以下列程式碼取代 `OnGetAsync` 方法：

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

上述程式碼中包含選擇性參數 `saveChangesError`。 `saveChangesError` 會顯示出此方法是否已在刪除學生物件失敗後呼叫。 刪除作業可能會因為暫時性的網路問題而失敗。 暫時性的網路錯誤較可能是在雲端。 [刪除] 頁面 `OnGetAsync` 從 UI 中呼叫時，`saveChangesError` 為 false。 當 `OnPostAsync` 呼叫 `OnGetAsync` 時 (因刪除作業失敗)，則 `saveChangesError` 參數為 true。

### <a name="the-delete-pages-onpostasync-method"></a>[刪除] 頁面的 OnPostAsync 方法

使用下列程式碼取代 `OnPostAsync`：

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

上述程式碼會擷取選取的實體，然後呼叫 [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) 方法來將實體的狀態設定為 `Deleted`。 當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。 如果 `Remove` 失敗：

* 攔截到資料庫例外狀況。
* [刪除] 頁面的 `OnGetAsync` 方法會以 `saveChangesError=true` 呼叫。

### <a name="update-the-delete-razor-page"></a>更新 [刪除 Razor] 頁面

將下列醒目標示的錯誤訊息新增至 [刪除 Razor] 頁面。
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

測試刪除。

## <a name="common-errors"></a>常見的錯誤

Students/Index 或其他連結運作失常：

確認 Razor 頁面包含正確的 `@page` 指示詞。 例如，Students/Index Razor 頁面**不應**包含路由範本：

```cshtml
@page "{id:int}"
```

每個 Razor 頁面都必須包含 `@page` 指示詞。



## <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本](https://www.youtube.com/watch?v=K4X1MT2jt6o)

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/intro)
> [下一頁](xref:data/ef-rp/sort-filter-page)

::: moniker-end
