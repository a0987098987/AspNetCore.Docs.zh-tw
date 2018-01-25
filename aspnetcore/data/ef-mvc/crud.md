---
title: "ASP.NET Core MVC EF 核心 CRUD-2 的 10"
author: tdykstra
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 873e4592ba668bbcb22f761c2a547a2a27d7e443
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>建立、 讀取、 更新和刪除-EF Core 與 ASP.NET Core MVC 教學課程 (2 / 10)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您可以建立 MVC 應用程式儲存和使用 Entity Framework 和 SQL Server LocalDB 顯示資料。 在本教學課程中，您將檢閱，並自訂 CRUD （建立、 讀取、 更新、 刪除） MVC scaffolding 自動為您建立控制器和檢視中的程式碼。

> [!NOTE] 
> 它是常見的作法，若要建立您的控制器和資料存取層之間的抽象層實作儲存機制模式。 若要簡單，並著重於教導如何使用 Entity Framework 本身，請保留這些教學課程，它們不使用儲存機制。 使用 EF 儲存機制的相關資訊，請參閱[這一系列中的最後一個教學課程](advanced.md)。

在本教學課程中，您將使用下列網頁：

![學生詳細資料頁面](crud/_static/student-details.png)

![學生建立頁面](crud/_static/student-create.png)

![學生編輯頁面](crud/_static/student-edit.png)

![學生刪除頁面](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>自訂的詳細資料頁面

學生索引頁面的 scaffold 程式碼中省略了`Enrollments`屬性，因為該屬性會保存集合。 在**詳細資料** 頁面上，您要以 HTML 表格顯示集合的內容。

在*Controllers/StudentsController.cs*，動作方法的詳細資料檢視會使用`SingleOrDefaultAsync`方法來擷取單一`Student`實體。 加入程式碼，呼叫`Include`。 `ThenInclude`與`AsNoTracking`方法，如下列反白顯示的程式碼所示。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include`和`ThenInclude`方法會造成載入內容`Student.Enrollments`導覽屬性，以及在每個註冊內`Enrollment.Course`導覽屬性。  您將進一步了解這些方法進行[讀取相關的資料](read-related-data.md)教學課程。

`AsNoTracking`方法可改善的案例，其中傳回的實體不會更新在目前內容的存留期間的效能。 您將深入了解`AsNoTracking`在本教學課程結尾處。

### <a name="route-data"></a>路由資料

索引鍵的值，傳遞至`Details`方法來自*路由資料*。 路由資料是 URL 的區段中找到的模型繫結器的資料。 例如，預設路由會指定控制器、 動作和 id 的區段：

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

在下列 URL 中，預設路由對應講師為控制站，做為動作的索引和 1 做為識別碼;這種路由資料值。

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL 的最後一個部分 (「？ courseID = 2021年 」) 是查詢字串值。 模型繫結器也會傳遞至的識別碼值`Details`方法`id`參數，如果您將它當做查詢字串值：

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

在索引頁面中，Razor 檢視中的標記協助程式陳述式會建立超連結 Url。 下列的 Razor 程式碼，`id`參數必須符合預設路由，因此`id`會加入至路由資料。

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

這會產生下列 HTML 時`item.ID`為 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

下列的 Razor 程式碼，`studentID`不符合預設路由中的參數，所以它會加入做為查詢字串。

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

這會產生下列 HTML 時`item.ID`為 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

如需標記協助程式的詳細資訊，請參閱[標記協助程式中 ASP.NET Core](xref:mvc/views/tag-helpers/intro)。

### <a name="add-enrollments-to-the-details-view"></a>加入詳細資料檢視中的註冊項目

開啟*Views/Students/Details.cshtml*。 每個欄位會顯示使用`DisplayNameFor`和`DisplayFor`helper，如下列範例所示：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

之後的最後一個欄位，並立即結束之前`</dl>`標記中，加入下列程式碼，顯示一份註冊項目：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

如果程式碼縮排錯誤貼上程式碼後，請按 CTRL-K-D 更正它。

此程式碼執行迴圈中的實體`Enrollments`導覽屬性。 針對每個註冊，它會顯示課程標題以及等級。 課程標題會從儲存在課程實體`Course`註冊實體的導覽屬性。

執行應用程式中，選取**學生**索引標籤，然後按一下**詳細資料**學生的連結。 您的選取學生看到 courses 以及等級清單：

![學生詳細資料頁面](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>更新 [建立] 頁面

在*StudentsController.cs*，修改 HttpPost`Create`方法加入 try-catch 區塊並移除從 ID`Bind`屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

這個程式碼加入 ASP.NET MVC 模型繫結器的學生實體所建立的學生實體設定，然後將變更儲存到資料庫。 （模型繫結器指的是 ASP.NET MVC 功能，可讓您更輕鬆地使用 送出表單的資料，將已張貼的表單值轉換成 CLR 類型的模型繫結器，並將其傳遞至動作方法參數中。 在此情況下，在模型繫結具現化學生實體為您使用表單集合中的屬性值。）

您移除`ID`從`Bind`屬性，因為識別碼是 SQL Server 會插入資料列時自動設定的主要金鑰值。 使用者輸入未設定的識別碼值。

除了`Bind`屬性，try catch 區塊是唯一的 scaffold 的程式碼所做的變更。 如果例外狀況衍生自`DbUpdateException`會攔截到儲存變更時，會顯示一般錯誤訊息。 `DbUpdateException`例外狀況有時是由外部應用程式，而不是程式設計錯誤，造成，因此再試一次會建議使用者。 未實作此範例中，雖然生產品質應用程式會記錄該例外狀況。 如需詳細資訊，請參閱**記錄檔，以深入了解**一節中[監控與遙測 （建置真實世界雲端應用程式與 Azure）](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)。

`ValidateAntiForgeryToken`屬性有助於防止跨網站要求偽造 (CSRF) 攻擊。 語彙基元自動插入檢視[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)和由使用者提交表單時，會包含。 權杖由驗證`ValidateAntiForgeryToken`屬性。 如需 CSRF 的詳細資訊，請參閱[反要求偽造](../../security/anti-request-forgery.md)。

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>關於 overposting 安全性注意事項

`Bind` Scaffold 的程式碼包含的屬性`Create`方法是一種方式防止 overposting 中建立的案例。 例如，假設 學生實體包含`Secret`屬性，您不想要設定這個 web 網頁。

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

即使您沒有`Secret`在網頁上，駭客的欄位無法使用 Fiddler 之類的工具或撰寫一些 JavaScript，張貼`Secret`形成值。 不含`Bind`限制建立的 「 學生 」 執行個體模型繫結器時，會使用模型繫結器的欄位的屬性將會收取，`Secret`形成值，並使用它來建立學生實體執行個體。 然後任何值指定給駭客`Secret`表單欄位會更新您的資料庫中。 下圖顯示 Fiddler 工具加入`Secret`（具有值"OverPost"） 的已張貼的表單值的欄位。

![Fiddler 加入密碼欄位](crud/_static/fiddler.png)

值"OverPost 」 會接著已成功加入至`Secret`屬性插入的資料列，雖然您不想網頁能夠將該屬性設定。

您可以避免在編輯案例 overposting 先從資料庫讀取的實體，然後再呼叫`TryUpdateModel`，並傳遞明確允許的內容清單中。 這是這些教學課程所使用的方法。

若要避免許多開發人員慣用的 overposting 替代方式是使用模型繫結的檢視模型，而不是實體類別。 包含只想要更新的檢視模型中的屬性。 一旦完成 MVC 模型繫結器，請將檢視模型內容複製到實體執行個體，選擇性地使用 AutoMapper 之類的工具。 使用`_context.Entry`實體執行個體，其狀態設定為上`Unchanged`，然後設定`Property("PropertyName").IsModified`為 true 的包含檢視模型中每一個實體屬性。 這個方法的運作方式同時編輯，並建立案例。

### <a name="test-the-create-page"></a>測試 [建立] 頁面

中的程式碼*Views/Students/Create.cshtml*使用`label`， `input`，和`span`（適用於驗證的訊息） 標記協助程式的每個欄位。

執行應用程式中，選取**學生**索引標籤，然後按一下**新建**。

輸入名稱和日期。 請嘗試輸入無效的日期，如果您的瀏覽器可讓您執行此作業。 （某些瀏覽器會強制要求您使用 日期選擇器。）然後按一下 **建立**以查看錯誤訊息。

![日期的驗證錯誤](crud/_static/date-error.png)

這是預設值; 您取得的伺服器端驗證在稍後的教學課程中，您會看到如何加入屬性也會產生用戶端驗證的程式碼。 將下列反白顯示的程式碼顯示中的模型驗證檢查`Create`方法。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

將日期變更為有效的值，然後按一下**建立**以查看出現在新的學生**索引**頁面。

## <a name="update-the-edit-page"></a>更新編輯頁面

在*StudentController.cs*，HttpGet`Edit`方法 (不含一個`HttpPost`屬性) 會使用`SingleOrDefaultAsync`方法來擷取所選的學生實體，如同`Details`方法。 您不需要變更這個方法。

### <a name="recommended-httppost-edit-code-read-and-update"></a>建議 HttpPost 編輯程式碼： 讀取和更新

下列程式碼中取代 HttpPost 編輯動作方法。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

這些變更實作安全性最佳作法，以防止 overposting。 產生 scaffolder`Bind`屬性和加入的實體集與模型繫結器所建立的實體`Modified`旗標。 程式碼不建議用於許多案例，因為`Bind`屬性清除任何預先存在的資料中未列出的欄位中`Include`參數。

新的程式碼會讀取現有的實體和呼叫`TryUpdateModel`更新中擷取之實體的欄位[根據使用者輸入中已張貼的表單資料](xref:mvc/models/model-binding#how-model-binding-works)。 Entity Framework 自動變更追蹤設定`Modified`旗標變更表單輸入的欄位。 當`SaveChanges`呼叫方法時，Entity Framework 建立 SQL 陳述式來更新資料庫的資料列。 並行衝突，系統會忽略，只有使用者已更新的資料表資料行在更新資料庫。 （之後的教學課程示範如何處理並行存取衝突）。

最佳作法是避免 overposting，您想要透過可更新的欄位**編輯**頁面會在允許清單中的`TryUpdateModel`參數。 （空的字串前面的參數清單中的欄位清單是針對使用表單欄位名稱的前置詞）。目前沒有額外的欄位，您要保護，但列出您想要繫結模型繫結器的欄位可確保您將欄位加入至資料模型在未來，如果它們自動保護直到您明確地在此處加入。

由於這些變更，HttpPost 的方法簽章`Edit`方法等同於 HttpGet`Edit`方法; 因此已重新命名方法`EditPost`。

### <a name="alternative-httppost-edit-code-create-and-attach"></a>替代 HttpPost 編輯程式碼： 建立並附加

建議的 HttpPost 編輯程式碼可確保只有已變更的資料行取得更新，並保留您不想包含模型繫結的屬性中的資料。 不過，讀取第一個方法都需要額外的資料庫讀取，並可能會導致更複雜的程式碼來處理並行衝突。 替代方法是附加至 EF 內容的模型繫結器所建立的實體，並將其標示為已修改。 (沒有更新您的專案與此程式碼，它只顯示說明的選擇性的方法。) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

網頁 UI 實體中包含的所有欄位，可以更新任何語言時，您可以使用這個方法。

Scaffold 的程式碼使用的建立和附加方法，但只會攔截`DbUpdateConcurrencyException`例外狀況，會傳回 404 錯誤代碼。  顯示的範例攔截任何資料庫更新例外狀況，並顯示錯誤訊息。

### <a name="entity-states"></a>實體狀態

是否在記憶體中的實體會在資料庫中，其對應的資料列和同步，這項資訊會決定當您呼叫時，會發生什麼情況，會持續追蹤的資料庫內容`SaveChanges`方法。 例如，當您傳遞至新的實體`Add`方法中，實體的狀態設為`Added`。 然後當您呼叫`SaveChanges`方法，將資料庫內容發出 SQL INSERT 命令。

實體可能處於下列狀態其中之一：

* `Added`. 實體還不存在資料庫中。 `SaveChanges`方法發出的 INSERT 陳述式。

* `Unchanged`. 與這個實體所完成，不需要`SaveChanges`方法。 當您從資料庫讀取實體時，實體開始於此狀態。

* `Modified`. 修改部分或所有實體的屬性值。 `SaveChanges`方法發出 UPDATE 陳述式。

* `Deleted`. 實體已被標示為刪除。 `SaveChanges`方法發出 DELETE 陳述式。

* `Detached`. 不追蹤實體的資料庫內容。

在桌面應用程式，會通常是自動設定的狀態變更。 您讀取實體，並變更它的某些屬性值。 這會導致其實體狀態，以自動變更為`Modified`。 然後當您呼叫`SaveChanges`，Entity Framework 會產生 SQL UPDATE 陳述式，以更新只針對您所變更的實際屬性。

在 web 應用程式中，`DbContext`一開始讀取，而且會顯示来編輯其資料已處置之後網頁呈現的實體。 當 HttpPost`Edit`呼叫動作方法，建立新的 web 要求，而且您必須的新執行個體`DbContext`。 如果您重新讀取該新的內容中的實體，您會模擬桌面的處理。

但如果您不想要執行額外的讀取作業，您必須使用模型繫結器所建立的實體物件。  若要這樣做最簡單的方式是將實體狀態設定修改，如同您在稍早所示的替代 HttpPost 編輯程式碼。 然後當您呼叫`SaveChanges`，Entity Framework 更新的資料庫資料列的所有資料行，因為內容中有無從得知您已變更的屬性。

如果您想要避免讀取第一個方法，但您也想要更新的使用者實際上已變更的欄位更新 SQL 陳述式，程式碼是更複雜。 您必須將原始值儲存在某些方面 (例如使用隱藏的欄位)，使其更可用時 HttpPost`Edit`方法呼叫。 然後您可以建立使用原始值，也就是呼叫學生實體`Attach`方法的原始版本，在實體的實體的值更新為新的值，然後呼叫`SaveChanges`。

### <a name="test-the-edit-page"></a>測試 編輯頁面

執行應用程式中，選取**學生**索引標籤，然後按一下 **編輯**超連結。

![學生編輯頁面](crud/_static/student-edit.png)

變更部分的資料和按一下**儲存**。 **索引**頁面隨即開啟，並查看變更的資料。

## <a name="update-the-delete-page"></a>更新刪除頁面

在*StudentController.cs*，HttpGet 的範本程式碼`Delete`方法會使用`SingleOrDefaultAsync`方法來擷取所選的學生實體，如您所見的詳細資料和編輯方法。 不過，若要實作自訂的錯誤訊息時呼叫`SaveChanges`失敗，您會加入一些功能至這個方法，其對應的檢視。

當您看到更新，並建立作業，刪除作業都需要兩個動作方法。 呼叫以回應 GET 要求的方法會顯示可讓使用者有機會核准或取消刪除作業的檢視。 如果使用者核准時，會建立 POST 要求。 當發生這種情況，HttpPost`Delete`方法呼叫，然後該方法會實際執行刪除作業。

您會加入 try-catch 區塊至 HttpPost`Delete`方法來處理資料庫的更新時可能會發生任何錯誤。 如果發生錯誤，HttpPost Delete 方法會呼叫 HttpGet Delete 方法，將其傳遞的參數，表示已發生錯誤。 HttpGet Delete 方法然後重新顯示 [確認] 頁面，以及錯誤訊息，讓使用者有機會取消，或再試一次。

取代 HttpGet`Delete`動作方法，以下列程式碼，管理錯誤報告。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

此程式碼會接受選擇性參數，指出是否將變更儲存在失敗之後呼叫此方法。 此參數為 false 時 HttpGet`Delete`呼叫方法時沒有先前的失敗。 當呼叫它的 HttpPost`Delete`方法以回應資料庫更新錯誤，參數為 true，且錯誤訊息會傳遞至檢視。

### <a name="the-read-first-approach-to-httppost-delete"></a>HttpPost Delete 讀取第一個方法

取代 HttpPost`Delete`動作方法 (名為`DeleteConfirmed`) 取代下列程式碼，這會執行實際的刪除作業和攔截的任何資料庫更新錯誤。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

此程式碼會擷取選取的實體，然後呼叫`Remove`實體的狀態設為方法`Deleted`。 當`SaveChanges`呼叫時，SQL DELETE 命令產生。

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>建立並附加 HttpPost Delete 方法

如果改善大量的應用程式中的效能是優先考量，就可以避免不必要的 SQL 查詢，藉由執行個體化使用只在主要的學生實體索引鍵的值，然後將實體狀態設定為`Deleted`。 這是所有 Entity Framework 必須以刪除實體。 （不要將這段程式碼放置在專案中，就在這裡只是為了說明替代）。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

如果實體有相關應該一併刪除的資料，請確定該串聯刪除已在資料庫中。 使用這個方法以刪除實體，EF 可能不了解有相關的實體被刪除。

### <a name="update-the-delete-view"></a>更新刪除檢視

在*Views/Student/Delete.cshtml*，新增 h2 標題之間 h3 標題、 錯誤訊息，如下列範例所示：

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

執行應用程式中，選取**學生**索引標籤，然後按一下**刪除**超連結：

![刪除確認 頁面](crud/_static/student-delete.png)

按一下**刪除**。 索引頁會顯示不含已刪除的學生。 （您會看到錯誤處理並行教學課程中的動作中的程式碼的範例）。

## <a name="closing-database-connections"></a>關閉資料庫連接

若要釋放出保留的資料庫連接的資源，當您在使用它，則必須儘速處置內容執行個體。 ASP.NET Core 內建[相依性插入](../../fundamentals/dependency-injection.md)會負責您該工作。

在*Startup.cs*，您呼叫[AddDbContext 擴充方法](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs)佈建`DbContext`ASP.NET DI 容器中的類別。 方法會設定為服務的存留期`Scoped`預設。 `Scoped`表示內容物件存留期伴隨 web 要求存留時間，而`Dispose`方法將會自動呼叫 web 要求的結尾。

## <a name="handling-transactions"></a>處理交易

根據預設 Entity Framework 會以隱含方式實作交易。 在案例中，您對多個資料列或資料表的變更，然後呼叫`SaveChanges`，Entity Framework 自動確保可能進行的所有變更成功，或完全失敗。 如果先完成某些變更，然後就會發生錯誤，這些變更會自動回復運作。 如案例，您需要更大的控制權-例如，如果您想要包含在交易-外面 Entity Framework 執行的作業，請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。

## <a name="no-tracking-queries"></a>不追蹤查詢

當資料庫內容中擷取資料表資料列，並建立代表的實體物件時，依預設它會追蹤的是否與資料庫中的實體記憶體中保持同步。 記憶體中的資料做為快取，並更新實體時，會使用。 這種快取，所以通常不必要的 web 應用程式通常存留較短 （新的其中一個是建立及處置每個要求） 以及內容的內容執行個體讀取實體通常處置之前會再次使用該實體。

您可以藉由呼叫停用記憶體中的實體物件的追蹤`AsNoTracking`方法。 您可以執行此作業的一般案例包括下列：

* 內容存留期間，您不需要更新任何實體，所以您無須以 EF[自動載入具有不同的查詢所擷取的實體的導覽屬性](read-related-data.md)。 經常在控制器的 HttpGet 動作方法中符合這些條件。

* 您正在執行的查詢會擷取大量資料，並將更新傳回的資料一小部分。 它可能會關閉追蹤大型查詢，並執行更新版本需要更新的幾個實體的查詢更有效率。

* 您想要將實體附加以更新，但您稍早針對不同用途擷取相同的實體。 因為實體已經受到追蹤的資料庫內容，您無法附加您想要變更的實體。 若要處理這種情況的一種方式為呼叫`AsNoTracking`上前面的查詢。

如需詳細資訊，請參閱[追蹤 vs。不追蹤](https://docs.microsoft.com/ef/core/querying/tracking)。

## <a name="summary"></a>總結

現在，您會有一組完整的頁面，執行簡單的 CRUD 作業的學生實體。 在下一個教學課程中，您將擴充的功能**索引**加入排序、 篩選和分頁的頁面。

>[!div class="step-by-step"]
[上一頁](intro.md)
[下一頁](sort-filter-page.md)  
