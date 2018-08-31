---
title: ASP.NET Core MVC 和 EF Core - CRUD - 2/10
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/crud
ms.openlocfilehash: 626b828e2391d3982ff2cf393f0c9e0748c12810
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751726"
---
# <a name="aspnet-core-mvc-with-ef-core---crud---2-of-10"></a>ASP.NET Core MVC 和 EF Core - CRUD - 2/10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在前一個教學課程中，您建立了一個使用 Entity Framework 及 SQL Server LocalDB 來儲存及顯示資料的 MVC 應用程式。 在本教學課程中，您將檢閱並自訂 MVC Scaffolding 自動為您在控制器及檢視中建立的 CRUD (建立、讀取、更新、刪除) 程式碼。

> [!NOTE]
> 實作[存放庫模式](xref:fundamentals/repository-pattern)，以在您的控制器及資料存取層之間建立抽象層是一種非常常見的做法。 為了使這些教學課程維持簡單，並聚焦於教導如何使用 Entity Framework，課程中將不會使用儲存機制。 如需儲存機制的詳細資訊，請參閱[本系列的最後一個教學課程](advanced.md)。

在本教學課程中，您將使用下列網頁：

![Student [詳細資料] 頁面](crud/_static/student-details.png)

![Student [建立] 頁面](crud/_static/student-create.png)

![Student [編輯] 頁面](crud/_static/student-edit.png)

![Student [刪除] 頁面](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>自訂 [詳細資料] 頁面

為 Students [索引] 頁面建立的 Scaffold 程式碼省略了 `Enrollments` 屬性，因為該屬性的值為一個集合。 在 [詳細資料] 頁面中，您會在一個 HTML 表格中顯示集合的內容。

在 *Controllers/StudentsController.cs* 中，[詳細資料] 檢視的動作方法會使用 `SingleOrDefaultAsync` 方法來擷取單一 `Student` 實體。 新增呼叫 `Include` 的程式碼。 `ThenInclude`，以及 `AsNoTracking` 方法，如下列醒目提示的程式碼所示。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` 及 `ThenInclude` 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。  您會在[讀取相關資料](read-related-data.md)教學課程中進一步了解這些方法。

`AsNoTracking` 方法在傳回實體不會在目前內容的存留期中更新的案例下可改善效能。 您會在本教學課程的最後進一步了解 `AsNoTracking`。

### <a name="route-data"></a>路由資料

傳遞至 `Details` 方法的索引鍵值是來自「路由資料」。 路由資料是模型繫結器在 URL 區段中找到的資料。 例如，預設路由指定了控制器、動作，以及識別碼區段：

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

在下列 URL 中，預設路由會將講師 (Instructor) 對應為控制器，索引 (Index) 對應為動作，以及 1 對應為識別碼。這些是路由資料的值。

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

URL 最後的部分 ("?courseID=2021") 為查詢字串的值。 模型繫結器也會將識別碼的值作為 `id` 參數傳遞給 `Details` 方法 (若您將其作為查詢字串的值傳遞)：

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

在 [索引] 頁面中，Razor 檢視裡的標籤協助程式陳述式會建立超連結 URL。 在下列 Razor 程式碼中，由於 `id` 參數符合預設路由，因此 `id` 便會新增至路由資料中。

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

這會在 `item.ID` 為 6 時產生下列 HTML：

```html
<a href="/Students/Edit/6">Edit</a>
```

在下列 Razor 程式碼中，由於 `studentID` 不符合預設路由中的參數，因此將會作為查詢字串新增。

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

這會在 `item.ID` 為 6 時產生下列 HTML：

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

如需標籤協助程式的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。

### <a name="add-enrollments-to-the-details-view"></a>將註冊新增至 [詳細資料] 檢視中

開啟 *Views/Students/Details.cshtml*。 每個欄位都會使用 `DisplayNameFor` 及 `DisplayFor` 協助程式顯示，如下列範例所示：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

在最後一個欄位之後，並在結尾的 `</dl>` 標籤之前，新增下列程式碼以顯示註冊清單：

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

若在您貼上程式碼之後，程式碼的縮排發生錯誤，請按 CTRL-K-D 修正它。

此程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。 針對每個註冊，它會顯示課程標題及成績。 課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。

執行應用程式，選取 [Students] 索引標籤，然後按一下學生的 [詳細資料] 連結。 您會看到選取學生的課程及成績清單：

![Student [詳細資料] 頁面](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>更新 [建立] 頁面

在 *StudentsController.cs* 中，藉由新增 try-catch 區塊並從 `Bind` 屬性移除識別碼來修改 HttpPost `Create` 方法。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

此程式碼會將 ASP.NET Core MVC 模型繫結器建立的 Student 實體新增至 Student 實體集，然後將變更儲存至資料庫。 (模型繫結器指的是 ASP.NET Core MVC 功能，它可讓您更輕鬆地操作由表單送出的資料；模型繫結器會將以 POST 方式送出的表單值轉換為 CLR 類型，然後傳遞給參數中的動作方法。 在此案例中，模型繫結器會使用來自表單 (Form) 集合的屬性值，為您執行個體化 Student 實體。)

您從 `Bind` 屬性移除了 `ID`，因為該識別碼是 SQL Server 在插入該資料列時自動為其建立的主索引鍵值。 使用者輸入的內容不會設定識別碼值。

除了 `Bind` 屬性外，try-catch 區塊是您對 Scaffold 程式碼進行的唯一變更。 若在儲存變更時捕捉到衍生自 `DbUpdateException` 的例外狀況，則會顯示一般錯誤訊息。 `DbUpdateException` 例外狀況有時候是因為某些外部因素造成的，而非程式設計上的錯誤，因此系統會建議使用者再試一次。 雖然在此範例中並未實作，但生產環境品質的應用程式應記錄例外狀況。 如需詳細資訊，請參閱[監視及遙測 (使用 Azure 建置現實世界的雲端應用程式)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)中的**深入解析記錄檔**一節。

`ValidateAntiForgeryToken` 屬性可協助防止跨網站偽造要求 (CSRF) 攻擊。 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) 會自動將權杖插入檢視中，並在使用者提交表單時包含在內。 權杖會由 `ValidateAntiForgeryToken` 屬性進行驗證。 如需 CSRF 的詳細資訊，請參閱[防偽要求](../../security/anti-request-forgery.md)。

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>關於大量指派 (overposting) 的安全性注意事項

Scaffold 程式碼在 `Create` 方法上包含的 `Bind` 屬性是在建立案例中防止大量指派 (overposting) 的一種方法。 例如，假設 Student 實體包含了一個 `Secret` 屬性，而您不希望這個網頁設定這個屬性。

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

即使您在網頁上並未包含 `Secret` 欄位，駭客仍有可能會使用像是 Fiddler 等工具，或撰寫一些 JavaScript，來在表單中提交 `Secret` 值。 若沒有 `Bind` 屬性限制模型繫結器在建立 Student 執行個體時能夠使用的欄位，則模型繫結器會揀選 `Secret` 表單值並使用它建立 Student 實體執行個體。 則無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到您的資料庫中。 下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。

![Fiddler 新增 Secret 欄位](crud/_static/fiddler.png)

"OverPost" 的值會成功新增到插入資料列的 `Secret` 屬性中，即使您沒有要讓網頁設定該屬性。

您可以藉由先從資料庫讀取實體，然後呼叫 `TryUpdateModel`，明確傳遞允許的屬性清單，來在編輯案例中防止大量指派。 這便是這些教學課程所使用的方法。

另一個許多開發人員偏好的防止大量指派的方法，便是使用檢視模型搭配模型繫結，而非實體類別。 意即僅在檢視模型中包含您想要更新的屬性。 待 MVC 模型繫結器完成後，將檢視模型屬性複製到實體執行個體，並選擇性的使用像是 AutoMapper 等工具。 在實體執行個體上使用 `_context.Entry` 來將其狀態設定為 `Unchanged`，然後在每個包含在檢視模型中的實體屬性上將 `Property("PropertyName").IsModified` 設定為 true。 這個方法可同時運用在編輯及建立案例中。

### <a name="test-the-create-page"></a>測試 [建立] 頁面

在 *Views/Students/Create.cshtml* 中的程式碼會針對各個欄位使用 `label`、`input`，以及 `span` (以驗證訊息) 標籤協助程式。

執行應用程式，選取 [Students] 索引標籤，然後按一下 [新建]。

輸入名稱和日期。 嘗試輸入無效的日期 (若您的瀏覽器允許的話)。 (某些瀏覽器會強制要求您使用日期選擇器。)然後按一下 [建立] 以查看錯誤訊息。

![日期驗證錯誤](crud/_static/date-error.png)

這是根據預設您會取得的伺服器端驗證。在稍後的教學課程中，您也會了解如何新增為用戶端驗證產生程式碼的屬性。 下列醒目提示的程式碼顯示了在 `Create` 方法中進行的模型驗證檢查。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

將日期變更為有效的值，然後按一下 [建立] 來在 [索引] 頁面上查看新增的學生。

## <a name="update-the-edit-page"></a>更新 [編輯] 頁面

在 *StudentController.cs* 中，HttpGet `Edit` 方法 (沒有 `HttpPost` 屬性的方法) 會使用 `SingleOrDefaultAsync` 方法來擷取選取的 Student 實體，如同您在 `Details` 方法中所看到的。 您不需要變更這個方法。

### <a name="recommended-httppost-edit-code-read-and-update"></a>建議的 HttpPost Edit 程式碼：讀取及更新

將 HttpPost Edit 動作方法以下列程式碼取代。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

這些變更會實作安全性最佳做法來防止大量指派。 框架會產生一個 `Bind` 屬性，並為模型繫結器建立的實體加上 `Modified` 旗標，新增到實體組。 該程式碼不建議用於許多案例，因為 `Bind` 屬性會清空 `Include` 參數未包含的欄位中任何預先存在的資料。

新的程式碼會讀取現有的實體，呼叫 `TryUpdateModel` 來[根據使用者在 Post 表單資料中輸入的內容](xref:mvc/models/model-binding#how-model-binding-works)來更新已擷取實體中的欄位。 Entity Framework 的自動變更追蹤會在表單輸入變更的欄位上設定 `Modified` 旗標。 當呼叫 `SaveChanges` 方法時，Entity Framework 會建立 SQL 陳述式來更新資料庫的資料列。 系統會忽略並行衝突，並且只有使用者更新的資料表資料行會在資料庫中獲得更新。 (之後的教學課程會顯示如何處理並行衝突。)

作為防止大量指派的最佳做法，您要允許 [編輯] 頁面更新的欄位會在 `TryUpdateModel` 參數的允許清單中。 (參數清單中欄位清單前的空字串是針對與表單欄位名稱一同使用的前置詞。)雖然目前沒有額外保護的欄位，但列出您希望模型繫結器繫結的欄位可確保您於未來將欄位新增到資料模型中時，新增的欄位會自動獲得保護，直到您明確的在這裡新增它們為止。

作為這些變更的結果，HttpPost `Edit` 方法的方法簽章會與 HttpGet `Edit` 方法的簽章一樣；因此，您已將方法重新命名為 `EditPost`。

### <a name="alternative-httppost-edit-code-create-and-attach"></a>HttpPost Edit 程式碼的替代方案：建立及連結

建議的 HttpPost Edit 程式碼可確保只有受到變更的資料行會獲得更新，並且會保留您不想要在資料繫結中包含之屬性內的資料。 然而，讀取優先的方法會需要額外的資料庫讀取，因此可能導致需要更多複雜的程式碼來處理並行衝突。 其替代方案便是將模型繫結器建立的實體連結到 EF 內容，並將其標示為已修改。 (請不要使用此程式碼更新您的專案。此程式碼僅作為展示選擇性的方法之用。)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

當網頁 UI 包含了所有實體中的欄位，並且可以更新其中的任何一個欄位時，您可以使用此方法。

Scaffold 程式碼會使用「建立及連結 」方法，但僅會捕捉到 `DbUpdateConcurrencyException` 例外狀況並傳回 404 錯誤代碼。  此處顯示的範例會捕捉到任何資料庫更新例外狀況，並顯示一個錯誤訊息。

### <a name="entity-states"></a>實體狀態

資料庫內容會追蹤實體在記憶體中是否與其在資料庫中相對應的資料列保持同步，並且此項資訊會決定當您呼叫 `SaveChanges` 方法時會發生什麼事情。 例如，當您傳遞一個新的實體到 `Add` 方法時，該實體的狀態便會設定為 `Added`。 然後當您呼叫 `SaveChanges` 方法時，資料庫內容便會發出 SQL INSERT 命令。

實體可為下列狀態中的其中一個：

* `Added`. 實體尚未存在於資料庫中。 `SaveChanges` 方法會發出 INSERT 陳述式。

* `Unchanged`. `SaveChanges` 方法針對這個實體不需要進行任何動作。 當您從資料庫讀取一個實體時，實體便會以此狀態開始。

* `Modified`. 實體中一部分或全部的屬性值已經過修改。 `SaveChanges` 方法會發出 UPDATE 陳述式。

* `Deleted`. 實體已遭標示刪除。 `SaveChanges` 方法會發出 DELETE 陳述式。

* `Detached`. 實體未獲得資料庫內容追蹤。

在桌面應用程式中，狀態變更通常會自動進行設定。 您讀取了一個實體並對其中的一些屬性值進行變更。 這會使得其實體狀態自動變更為 `Modified`。 當您呼叫 `SaveChanges` 時，Entity Framework 會產生 SQL UPDATE 陳述式，並且只會更新您實際變更的屬性。

在 Web 應用程式中，初始讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。 當呼叫 HttpPost `Edit` 動作方法時，會發出新的 Web 要求，並且您便會擁有一個新的 `DbContext` 執行個體。 當您在新的內容中重新讀取時，您便會模擬桌面處理流程。

但若您不想要進行額外的讀取作業，則您必須使用由模型繫結器建立的實體物件。  完成這項作業最簡單的方式，便是將實體狀態設定為「已修改」(Modified)，作為先前顯示之 HttpPost Edit 程式碼的替代方案。 則當您呼叫 `SaveChanges` 時，Entity Framework 會更新資料庫資料列中所有的資料行，因為內容無法得知您變更的屬性為何。

若您想要避免讀取優先的方法，但又想要 SQL UPDATE 陳述式僅更新使用者實際變更的欄位，則程式碼會變得更為複雜。 您必須先以某種方式儲存原始的值 (例如使用隱藏欄位)，使得在呼叫 HttpPost `Edit` 方法時仍可以使用那些值。 然後您便可以使用原始的值建立 Student 實體，使用原始版本的實體呼叫 `Attach` 方法，將實體的值更新為新的值，然後呼叫 `SaveChanges`。

### <a name="test-the-edit-page"></a>測試 [編輯] 頁面

執行應用程式，選取 [Students] 索引標籤，然後按一下 [編輯] 超連結。

![Students [編輯] 頁面](crud/_static/student-edit.png)

變更一部分的資料，然後按一下 [儲存]。 [索引] 頁面便會開啟，而您會看到更新的資料。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

在 *StudentController.cs* 中，HttpGet `Delete` 方法的範本程式碼使用了 `SingleOrDefaultAsync` 方法來擷取選取的 Student 實體，如您在 Details 及 Edit 方法中所看到的。 然而，若要在呼叫 `SaveChanges` 失敗時實作自訂錯誤訊息，您需要將一些功能新增至此方法及其對應的檢視。

如同您在更新及建立作業中所看到的，刪除作業需要兩個動作方法。 為回應 GET 要求而呼叫的方法會顯示一個檢視，給予使用者機會核准或取消刪除作業。 若使用者核准，則便會建立 POST 要求。 當這種情況發生時，便會呼叫 HttpPost `Delete` 方法，然後該方法便會實際執行刪除作業。

您會將一個 try-catch 區塊新增到 HttpPost `Delete` 方法，來處理任何可能在資料庫更新時可能發生的錯誤。 若發生錯誤，則 HttpPost Delete 方法會呼叫 HttpGet Delete 方法，將一個指示發生錯誤的參數傳遞給它。 HttpGet Delete 方法接著便會重新顯示確認頁面及錯誤訊息，給予使用者機會取消或再試一次。

請以下列程式碼取代 HttpGet `Delete` 動作方法。這段程式碼會管理錯誤報告。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

此程式碼會接受一個選用的參數，該參數會在儲存變更發生錯誤之後指示方法是否有進行呼叫。 當呼叫 HttpGet `Delete` 方法時，若先前沒有發生錯誤，則此參數將為 false。 當此方法是由 HttpPost `Delete` 方法為回應資料庫更新錯誤而呼叫時，則參數將為 true，且錯誤訊息將會傳遞給檢視。

### <a name="the-read-first-approach-to-httppost-delete"></a>HttpPost Delete 讀取優先方法

請以下列程式碼取代 HttpPost `Delete` 動作方法 (名為 `DeleteConfirmed`)。此程式碼會執行實際的刪除作業並捕捉任何資料庫更新錯誤。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

此程式碼會擷取選取的實體，然後呼叫 `Remove` 方法來將實體的狀態設定為 `Deleted`。 當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>HttpPost Delete 的「建立及連結 」方法

若在一個高容量的應用程式中改善效能是您的優先事項，您可以透過僅使用主索引鍵值執行個體化 Student 實體，然後將實體狀態設定為 `Deleted`，來避免不必要的 SQL 查詢。 這便是 Entity Framework 要刪除實體所需要的一切資訊。 (請不要將此程式碼放在您的專案中。此程式碼僅作為展示替代方案之用。)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

若實體有也需要刪除的相關資料，請確認您已在資料庫中設定串聯刪除。 若使用此方法進行實體刪除，EF 可能無法得知有相關實體需要進行刪除。

### <a name="update-the-delete-view"></a>更新 [刪除] 檢視

在 *Views/Student/Delete.cshtml* 中，在 h2 標題和 h3 標題之間新增一個錯誤訊息，如下列範例所示：

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

執行應用程式，選取 [Students] 索引標籤，然後按一下**刪除**超連結：

![刪除確認頁面](crud/_static/student-delete.png)

按一下 [刪除]。 顯示的 [索引] 頁面將不會包含遭刪除的學生。 (您會在並行教學課程中看到錯誤處理程式碼範例的實際情況。)

## <a name="closing-database-connections"></a>關閉資料庫連線

若要釋放資料庫連線保留的資源，您必須在完成作業並不再需要內容執行個體時盡快處置它。 ASP.NET Core 內建的[相依性插入](../../fundamentals/dependency-injection.md)會為您完成這項工作。

在 *Startup.cs* 中，您會呼叫 [AddDbContext 擴充方法](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) 來在 ASP.NET Core DI 容器中佈建 `DbContext` 類別。 根據預設，該方法會將服務存留期設定為 `Scoped`。 `Scoped` 表示內容物件的存留期會與 Web 要求的存留期保持一致，並且在 Web 要求結束時會自動呼叫 `Dispose` 方法。

## <a name="handling-transactions"></a>處理交易

根據預設，Entity Framework 隱含性的實作了交易。 在您對多個資料列或資料表進行變更，然後呼叫 `SaveChanges` 的案例下，Entity Framework 會自動確認您所有的變更都已成功或是失敗。 若有些變更已先完成，之後卻發生錯誤，則這些變更都會自動進行復原。 針對您需要更多控制的案例 -- 例如，若您想要在一個交易中包含在 Entity Framework 之外完成的作業 -- 請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。

## <a name="no-tracking-queries"></a>無追蹤查詢

當資料庫內容擷取資料表資料列並建立代表他們的實體物件時，根據預設，它會追蹤在記憶體中的實體是否與資料庫中的內容保持同步。 記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。 這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。

您可以藉由呼叫 `AsNoTracking` 方法來停用追蹤記憶體中的實體物件。 您會想要進行這項操作的常見案例包括下列情況：

* 在內容的存留期間，您不需要更新任何實體，並且您也不需要 EF [使用透過個別查詢擷取的實體自動載入導覽屬性](read-related-data.md)。 控制器中的 HttpGet 動作方法常常會符合這些條件。

* 您正在執行一個擷取大量資料的查詢，但是傳回資料之中只有一小部分需要更新。 在這種情況下，為大型查詢關閉追蹤，然後在稍後為需要更新的幾個實體執行查詢可能會比較有效率。

* 您想要連結到一個實體以更新它，但稍早之前您才為了不同的目的擷取過相同的實體。 由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。 處理這種情況的一種方法，便是在稍早之前的查詢中呼叫 `AsNoTracking`。

如需詳細資訊，請參閱[追蹤與不追蹤](https://docs.microsoft.com/ef/core/querying/tracking)。

## <a name="summary"></a>總結

您現在已有完整的頁面組，可為 Student 實體進行簡易的 CRUD 作業。 在下一個教學課程中，您會將 [索引] 頁面的功能進行拓展，新增排序、篩選和分頁功能。

::: moniker-end

> [!div class="step-by-step"]
> [上一頁](intro.md)
> [下一頁](sort-filter-page.md)
