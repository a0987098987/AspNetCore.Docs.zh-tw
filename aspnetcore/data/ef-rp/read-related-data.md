---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8
author: rick-anderson
description: 在此教學課程中，您可以讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 93fbb4741f476368d75d23162d6e2597de7b263e
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819912"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

在本教學課程中，將會讀取和顯示相關資料。 相關資料是 EF Core 載入到導覽屬性的資料。

若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:index#how-to-download-a-sample)。

下圖顯示本教學課程的已完成頁面：

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>相關資料的積極式、明確式和消極式載入

EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：

* [積極式載入](/ef/core/querying/related-data#eager-loading)。 積極式載入是指某一類型實體的查詢同時也會載入相關實體。 讀取實體時，將會擷取其相關資料。 這通常會導致單一聯結查詢，其可擷取所有需要的資料。 EF Core 將針對某些類型的積極式載入發出多個查詢。 相較於 EF6 中的某些查詢只有單一查詢的情況，發出多個查詢可能更有效率。 積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  當集合導覽包含在內時，積極式載入會傳送多個查詢：

  * 針對主查詢傳送一個查詢 
  * 針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。

* 使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。 「修正」表示 EF Core 會自動填入導覽屬性。 使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  注意:EF Core 會將導覽屬性自動修正為先前已載入至內容執行個體的任何其他實體。 即使「未」  明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。

* [明確式載入](/ef/core/querying/related-data#explicit-loading)。 第一次讀取實體時，不會擷取相關資料。 必須撰寫程式碼，才能在需要時擷取相關資料。 使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。 透過明確式載入，程式碼會指定要載入的導覽屬性。 請使用 `Load` 方法來執行明確式載入。 例如：

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* [消極式載入](/ef/core/querying/related-data#lazy-loading)。 [EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。 第一次讀取實體時，不會擷取相關資料。 第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。 每當第一次存取導覽屬性時，查詢會傳送至資料庫。

* `Select` 運算子只會載入所需的相關資料。

## <a name="create-a-course-page-that-displays-department-name"></a>建立顯示部門名稱的 Course 頁面

Course 實體包含導覽屬性，其中包含 `Department` 實體。 `Department` 實體包含已指派課程的部門。

若要在課程清單中顯示所指派部門的名稱：

* 從 `Department` 實體取得 `Name` 屬性。
* `Department` 實體來自 `Course.Department` 導覽屬性。

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a>Scaffold Course 模型

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Course`。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

上述命令會 Scaffold `Course` 模型。 在 Visual Studio 中開啟專案。

開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。 Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。 `Include` 方法可指定積極式載入。

執行應用程式並選取**課程**連結。 部門資料行便會顯示沒有用的 `DepartmentID`。

以下列程式碼更新 `OnGetAsync` 方法：

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

上述程式碼會新增 `AsNoTracking`。 `AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。 不會追蹤實體的原因是它們不會在目前的內容中更新。

以下列醒目提示的標記更新 *Pages/Courses/Index.cshtml*：

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

已對包含 Scaffold 的程式碼進行下列變更：

* 已將標題從 Index 變更為 Courses。
* 新增顯示 `CourseID` 屬性值的 [編號]  資料行。 主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。 不過，在此情況下，主索引鍵有意義。
* 變更 [部門]  資料行來顯示部門名稱。 此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

執行應用程式，並選取 [Courses]  索引標籤來查看含有部門名稱的清單。

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>使用 Select 載入相關資料

`OnGetAsync` 方法使用 `Include` 方法載入相關資料：

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` 運算子只會載入所需的相關資料。 如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。 如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。

下列程式碼使用 `Select` 方法載入相關資料：

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)。

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立顯示「課程」和「註冊」的 Instructors 頁面

在本節中，將會建立 Instructors 頁面。

<a name="IP"></a>
![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

此頁面將以下列方式讀取和顯示相關資料：

* 講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。 `Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。 積極式載入用於 `OfficeAssignment` 實體。 若需要顯示相關資料，積極式載入通常更有效率。 在此情況下，將會顯示講師的辦公室指派。
* 當使用者選取講師 (上述映像中的 Harui) 時，便會顯示相關的 `Course` 實體。 `Instructor` 與 `Course` 實體具有多對多關聯性。 將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。 在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。 這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。
* 當使用者選取課程 (上述映像中的 Chemistry)，隨即顯示 `Enrollments` 中的相關資料。 在上述映像中，將會顯示學生姓名和年級。 `Course` 與 `Enrollment` 實體具有一對多關聯性。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立 Instructor 索引檢視的檢視模型

講師頁面會顯示下列三個不同資料表的資料。 建立的檢視模型會包含三個實體代表三個資料表。

在 *SchoolViewModels* 資料夾中，以下列程式碼建立 *InstructorIndexData.cs*：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Scaffold Instructor 模型

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Instructor`。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

 執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

上述命令會 Scaffold `Instructor` 模型。 執行應用程式並巡覽至講師頁面。

以下列程式碼取代 *Pages/Instructors/Index.cshtml.cs*：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。

檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

查詢具有兩個 Include：

* `OfficeAssignment`：顯示在 [Instructors 檢視](#IP)中。
* `CourseAssignments`：它顯示所教授的課程。

### <a name="update-the-instructors-index-page"></a>更新 Instructors [索引] 頁面

以下列標記更新 *Pages/Instructors/Index.cshtml*：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

上述標記會進行下列變更：

* 將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。 `"{id:int?}"` 是路由範本。 路由範本將 URL 中的整數查詢字串變更為路由資料。 例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取]  連結：

  `http://localhost:1234/Instructors?id=2`

  頁面指示詞是 `@page "{id:int?}"` 時，先前的 URL 為：

  `http://localhost:1234/Instructors/2`

* 頁面標題是 **Instructors**。
* 新增 [辦公室]  資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。 因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* 新增 [課程]  資料行，以顯示每位講師所教授的課程。 若要深入了解此 Razor 語法，請參閱[使用 `@:` 進行明確的行轉換](xref:mvc/views/razor#explicit-line-transition-with-)。

* 新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。 這會使用啟動程序類別設定所選取資料列的背景色彩。

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 新增標示為**選取**的超連結。 此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

執行應用程式並選取 [Instructors]  索引標籤。此頁面會顯示來自相關 `OfficeAssignment` 實體的 `Location` (辦公室)。 如果 OfficeAssignment 是 Null，就會顯示空的資料表資料格。

![Instructors [索引] 頁面未選取任何項目](read-related-data/_static/instructors-index-no-selection.png)

按一下**選取**連結。 資料列樣式變更。

### <a name="add-courses-taught-by-selected-instructor"></a>新增選取的講師所教授的課程

以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中的 `OnGetAsync` 方法：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

新增 `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

檢查已更新的查詢：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

上述查詢會新增 `Department` 實體。

下列程式碼會在已選取講師 (`id != null`) 時執行。 選取的講師會從檢視模型的講師清單中擷取。 檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` 方法會傳回集合。 在上述 `Where` 方法中，只會傳回一個單一 `Instructor` 實體。 `Single` 方法會將集合轉換成單一 `Instructor` 實體。 `Instructor` 實體提供對 `CourseAssignments` 屬性的存取。 `CourseAssignments` 提供對相關 `Course` 實體的存取。

![講師對課程 m:M](complex-data-model/_static/courseassignment.png)

當集合只有一個項目時，將會在集合上使用 `Single` 方法。 如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。 替代方式是 `SingleOrDefault`，它會在集合為空時傳回預設值 (在此情況下為 Null)。 在空集合上使用 `SingleOrDefault`：

* 造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)。
* 例外狀況訊息會不太清楚地指出問題的原因。

選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

將下列標記新增至 *Pages/Instructors/Index.cshtml* Razor 頁面的結尾：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

當選取講師時，上述標記會顯示與講師相關的課程。

測試應用程式。 按一下講師頁面上的**選取**連結。

![Instructors [索引] 頁面已選取講師](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>顯示學生資料

在本節中，應用程式會更新以顯示所選取課程的學生資料。

以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中 `OnGetAsync` 方法的查詢：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

更新 *Pages/Instructors/Index.cshtml*。 將下列標記新增至檔案結尾：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

上述標記會顯示註冊所選取課程的學生清單。

重新整理頁面，然後選取講師。 選取課程，以查看已註冊學生和其年級的清單。

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>使用 Single

`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

比起使用 `Where`，上述 `Single` 方法並沒有任何優勢。 某些開發人員偏好使用 `Single` 方法樣式。

## <a name="explicit-loading"></a>明確式載入

目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

假設使用者很少會想要查看課程中的註冊項目。 在此情況下，最佳方法就是在要求時，只載入註冊資料。 在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。

使用下列程式碼更新 `OnGetAsync`：

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。 如果選取了課程，醒目提示的程式碼就會擷取：

* 所選取課程的 `Enrollment` 實體。
* 每個 `Enrollment` 的 `Student` 實體。

請注意，上述程式碼會將 `.AsNoTracking()` 註解化。 針對所追蹤的實體，導覽屬性只能進行明確式載入。

測試應用程式。 就使用者的觀點而言，應用程式行為與之前的版本相同。

下一個教學課程會示範如何更新相關資料。

## <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本 (第 1 部分)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [這個教學課程的 YouTube 版本 (第 2 部分)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[上一頁](xref:data/ef-rp/complex-data-model)
>[下一頁](xref:data/ef-rp/update-related-data)
