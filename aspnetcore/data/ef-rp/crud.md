---
title: "使用 EF 核心 CRUD-2 的 8 razor 頁面"
author: rick-anderson
description: "示範如何建立、 讀取、 更新、 刪除與 EF 核心"
keywords: "ASP.NET Core、 Entity Framework Core、 CRUD，建立、 讀取、 更新、 刪除"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>建立、 讀取、 更新和刪除-Razor 頁面 (8 個 2) 使用的 EF 核心

由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

在本教學課程，scaffold CRUD （建立、 讀取、 更新、 刪除） 的程式碼檢閱並自訂。

注意： 若要降低複雜性並將焦點放在 EF 核心這些教學課程，EF 核心程式碼會使用 Razor 頁面程式碼後置檔案中。 有些開發人員會使用服務層或儲存機制模式中的建立 UI （Razor 頁面） 和資料存取層之間的抽象層。

在本教學課程、 建立、 編輯、 刪除和詳細資料中的 Razor 頁面*學生*資料夾會被修改。

Scaffold 的程式碼會使用下列模式建立、 編輯和刪除的頁面：

* 取得並顯示要求的資料與 HTTP GET 方法`OnGetAsync`。
* 使用 HTTP POST 方法，將變更儲存至資料`OnPostAsync`。

索引 和 詳細資料頁面中取得並顯示要求的資料與 HTTP GET 方法`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>取代 FirstOrDefaultAsync SingleOrDefaultAsync

產生的程式碼會使用[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)來提取要求的實體。 [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)是更能有效地擷取一個實體：

* 除非程式碼必須以確認沒有任何多個查詢所傳回的實體。 
* `SingleOrDefaultAsync`提取更多資料，而且也不必要的工作。
* `SingleOrDefaultAsync`如果沒有符合篩選組件的多個實體，會擲回例外狀況。
*  `FirstOrDefaultAsync`如果沒有符合篩選組件的多個實體，不會擲回。

全域取代`SingleOrDefaultAsync`與`FirstOrDefaultAsync`。 `SingleOrDefaultAsync`5 個地方使用：

* `OnGetAsync`在 [詳細資料] 頁面中。
* `OnGetAsync`和`OnPostAsync`編輯和刪除的頁面中。

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

在大部分的 scaffold 的程式碼， [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)可用取代`FirstOrDefaultAsync`或`SingleOrDefaultAsync`。 

`FindAsync`：

* 尋找實體主索引鍵 (PK)。 如果內容正在追蹤在 PK 的實體，它會傳回沒有要求至 DB。
* 是簡單且精確。
* 已最佳化來查閱單一實體。
* 可以在某些情況下，具有效能優點，但是它們很少是必須列入的一般 web 案例。
* 隱含使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)而不是[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。
但如果您想要包含其他項目，則尋找已不再適用。 這表示您可能要放棄尋找並將您的應用程式進行時將移至查詢。

## <a name="customize-the-details-page"></a>自訂的詳細資料頁面

瀏覽至`Pages/Students`頁面。 **編輯**，**詳細資料**，和**刪除**所產生連結[錨定標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)中*頁面/學生 /Index.cshtml*檔案。

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

選取 [詳細資料] 連結。 URL 的格式是`http://localhost:5000/Students/Details?id=2`。 使用查詢字串來傳遞學生識別碼 (`?id=2`)。

更新編輯、 詳細資料，以及刪除 Razor 頁面使用`"{id:int}"`路由範本。 將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。

"{Id: int}"的路由範本並與網頁要求**不**包含整數的路由值傳回 HTTP 404 （找不到） 錯誤。 例如，`http://localhost:5000/Students/Details`傳回 404 錯誤。 若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：

 ```cshtml
@page "{id:int?}"
```

執行應用程式，然後按一下 [詳細資料] 連結，再確認 URL 傳遞做為路由資料的識別碼 (`http://localhost:5000/Students/Details/2`)。

請不要全域變更`@page`至`@page "{id:int}"`、 執行中斷首頁連結，以及建立頁面。

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>新增相關的資料

學生索引頁面的 scaffold 程式碼不包含`Enrollments`屬性。 本節的內容中`Enrollments`集合會顯示在詳細資料頁面。

`OnGetAsync`方法*Pages/Students/Details.cshtml.cs*使用`FirstOrDefaultAsync`方法來擷取單一`Student`實體。 加入下列反白顯示的程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

`Include`和`ThenInclude`方法會造成載入內容`Student.Enrollments`導覽屬性，以及在每個註冊內`Enrollment.Course`導覽屬性。 這些方法是 examinied 詳細閱讀相關的資料 」 教學課程中。

`AsNoTracking`方法可改善案例的效能時不會更新目前的內容中傳回的實體。 `AsNoTracking`在本教學課程稍後討論。

### <a name="display-related-enrollments-on-the-details-page"></a>在詳細資料 頁面上顯示相關的註冊項目

開啟*Pages/Students/Details.cshtml*。 加入下列反白顯示的程式碼，以顯示一份註冊項目：

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

如果程式碼縮排是錯的程式碼就會貼上之後，請按 CTRL-K-D 更正它。

上述程式碼中執行迴圈中的實體`Enrollments`導覽屬性。 針對每個註冊，它會顯示課程標題以及等級。 課程標題會從儲存在課程實體`Course`註冊實體的導覽屬性。

執行應用程式中，選取**學生**索引標籤，然後按一下**詳細資料**學生的連結。 Courses 以及為選取的學生成績的清單隨即顯示。

## <a name="update-the-create-page"></a>更新 [建立] 頁面

更新`OnPostAsync`方法中的*Pages/Students/Create.cshtml.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

檢查[TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

在上述程式碼，`TryUpdateModelAsync<Student>`嘗試更新`emptyStudent`物件使用的已張貼的表單值[PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)屬性[PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)。 `TryUpdateModelAsync`只會更新所列的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。

在上述範例中：

* 第二個引數 (` "student", // Prefix`) 是前置詞會使用來查閱值。 不區分大小寫。
* 已張貼的表單值會轉換成中的型別`Student`模型使用[模型繫結](xref:mvc/models/model-binding#how-model-binding-works)。

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

使用`TryUpdateModel`更新具有已張貼值欄位是安全性最佳作法，因為它會阻止 overposting。 例如，假設 學生實體包含`Secret`這個 web 網頁不應該更新或新增的屬性：

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

即使應用程式沒有`Secret`欄位上建立/更新 Razor 頁面上，駭客無法設定`Secret`overposting 值。 駭客無法使用 Fiddler 之類的工具或撰寫一些 JavaScript，張貼`Secret`形成值。 原始的程式碼並不會限制建立的學生執行個體時，會使用模型繫結器的欄位。

任何值指定給駭客`Secret`表單欄位會在資料庫中更新。 下圖顯示 Fiddler 工具加入`Secret`（具有值"OverPost"） 的已張貼的表單值的欄位。

![Fiddler 加入密碼欄位](../ef-mvc/crud/_static/fiddler.png)

值"OverPost 」 已成功新增到`Secret`插入資料列的屬性。 絕不想要的應用程式設計師`Secret`要使用 [建立] 頁面來設定屬性。

<a name="vm"></a>
### <a name="view-model"></a>檢視模型

檢視模型通常包含應用程式所使用的模型中包含的屬性子集。 應用程式模型通常稱為網域模型。 網域模型通常會包含所有需要對應的實體 DB 中的屬性。 檢視模型包含只針對所需的 UI 層 （例如，[建立] 頁面） 的屬性。 除了檢視模型，某些應用程式會使用繫結模型或輸入的模型，Razor 頁面程式碼後置類別和瀏覽器之間傳遞資料。 請考慮下列`Student`檢視模型：

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

檢視模型提供的替代方法，以防止 overposting。 檢視模型包含檢視 （顯示） 或更新的屬性。

下列程式碼會使用`StudentVM`檢視模型來建立新的學生：

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)方法所讀取的值設定這個物件值[PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)物件。 `SetValues`使用屬性名稱比對。 檢視模型類型不需要有關聯的模型型別，您只需要取得相符的內容。

使用`StudentVM`需要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)更新為使用`StudentVM`而不是`Student`。

在 Razor 頁面`PageModel`衍生的類別是檢視模型。 

## <a name="update-the-edit-page"></a>更新編輯頁面

更新編輯頁面程式碼後置檔案：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

建立頁面，有一些例外狀況的程式碼變更如下：

* `OnPostAsync`具有選擇性`id`參數。
* 目前的學生會擷取從資料庫中，而不是建立空白的學生。
* `FirstOrDefaultAsync`已取代為[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)。 `FindAsync`從主索引鍵選取實體時，是不錯的選擇。 請參閱[FindAsync](#FindAsync)如需詳細資訊。

### <a name="test-the-edit-and-create-pages"></a>測試編輯，以及建立頁面

建立和編輯幾個學生實體。

## <a name="entity-states"></a>實體狀態

在記憶體中的實體 DB 中其對應的資料列同步是否記錄的 DB 內容。 DB 內容的同步資訊會決定會發生什麼事時`SaveChanges`呼叫。 例如，當新的實體傳遞至`Add`方法中，實體的狀態設為`Added`。 當`SaveChanges`呼叫時，資料庫內容發出 SQL INSERT 命令。

實體可能處於下列狀態其中之一：

* `Added`: 實體不在資料庫中尚未存在。 `SaveChanges`方法發出的 INSERT 陳述式。

* `Unchanged`： 需要此實體與此一併儲存沒有變更。 從資料庫讀取時，實體具有此狀態。

* `Modified`： 已修改部分或所有實體的屬性值。 `SaveChanges`方法發出 UPDATE 陳述式。

* `Deleted`： 實體已被標示為刪除。 `SaveChanges`方法發出 DELETE 陳述式。

* `Detached`： 實體沒有正在追蹤的資料庫內容。

在傳統型應用程式，會通常是自動設定的狀態變更。 實體讀取、 進行變更，並自動變更為實體狀態`Modified`。 呼叫`SaveChanges`會產生 SQL UPDATE 陳述式，以更新變更的屬性。

在 web 應用程式中，`DbContext`實體，並顯示呈現頁面之後，會處置資料讀取。 當頁面`OnPostAsync`呼叫方法時，都會建立新的 web 要求的新執行個體的正與`DbContext`。 重新讀取該新的內容中的實體會模擬桌面的處理。

## <a name="update-the-delete-page"></a>更新刪除頁面

在本節中，加入程式碼實作自訂的錯誤訊息時呼叫`SaveChanges`失敗。 加入字串，包含 possile 錯誤訊息：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

以下列程式碼取代 `OnGetAsync` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

上述程式碼中包含選擇性參數`saveChangesError`。 `saveChangesError`指出是否要刪除的學生物件失敗後呼叫方法。 刪除作業可能會因為暫時性的網路問題而失敗。 暫時性的網路錯誤都可能在雲端。 `saveChangesError`為 false 時刪除頁面`OnGetAsync`呼叫從 UI。 當`OnGetAsync`稱為`OnPostAsync`（因為刪除作業失敗），則`saveChangesError`參數為 true。

### <a name="the-delete-pages-onpostasync-method"></a>刪除頁面 OnPostAsync 方法

取代`OnPostAsync`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

上述程式碼擷取選取的實體，然後呼叫`Remove`實體的狀態設為方法`Deleted`。 當`SaveChanges`呼叫時，SQL DELETE 命令產生。 如果`Remove`失敗：

* 資料庫是例外。
* 刪除頁面`OnGetAsync`方法呼叫`saveChangesError=true`。

### <a name="update-the-delete-razor-page"></a>更新刪除 Razor 頁面

將下列反白顯示的錯誤訊息加入刪除 Razor 頁面。

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

測試刪除。

## <a name="common-errors"></a>常見的錯誤

不適用學生/住家或其他連結：

確認 Razor 頁面包含正確`@page`指示詞。 比方說，學生/Razor 首頁應該**不**包含路由範本：

```cshtml
@page "{id:int}"
```

必須包含每個 Razor 頁面`@page`指示詞。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/intro)
[下一頁](xref:data/ef-rp/sort-filter-page)
