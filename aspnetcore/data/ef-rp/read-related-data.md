---
title: "使用 EF Core-razor 頁面讀取相關的資料-6，8 個"
author: rick-anderson
description: "在此教學課程中您可以讀取並顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: d0cdb5aaa4b1129c3f2404d069e9781ca16260b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>讀取的相關資料-Razor 頁面 (8 個 6) 使用的 EF 核心

由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

在本教學課程中，會讀取及顯示相關的資料。 EF 核心載入導覽屬性的資料相關的資料。

如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)。

下圖顯示已完成的頁面在此教學課程：

![課程索引頁](read-related-data/_static/courses-index.png)

![講師索引頁](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>急切、 明確，和消極式載入之相關資料

有幾種 EF 核心可以將實體的導覽屬性載入相關的資料：

* [積極式載入](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)。 積極式載入時，一種實體類型的查詢也會載入相關的實體。 實體讀取時，會擷取其相關的資料。 這通常會導致單一聯結查詢以擷取所有所需的資料。 EF 核心會發出多個查詢，針對某些類型的積極式載入。 發行多個查詢的內容可能會更有效率，比起 EF6 中的某些查詢的情況在先前存在的單一查詢。 積極式載入指定`Include`和`ThenInclude`方法。

 ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
 積極式載入集合 nvavigation 包含在內時，會傳送多個查詢：

 * 針對主查詢的一個查詢 
 * 每個集合負載樹狀目錄中的 「 邊緣 」 的一個查詢。

* 個別查詢`Load`： 可以在個別的查詢，擷取的資料和 EF 核心 「 修復 」 的導覽屬性。 「 向上修正 」 表示 EF 核心會自動填入的導覽屬性。 個別查詢`Load`則更像 explict 載入比積極式載入。

 ![個別查詢範例](read-related-data/_static/separate-queries.png)

 注意： EF 核心自動修復與先前已載入至內容執行個體的任何其他實體的導覽屬性。 即使瀏覽屬性資料是*不*明確包含，屬性可能仍然會填入某些或所有相關的項目先前已載入。

* [明確式載入](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)。 第一次讀取實體時，不被擷取相關的資料。 必須撰寫程式碼會在需要時擷取的相關的資料。 明確式載入個別的查詢結果傳送至資料庫的多個查詢。 明確式載入，與程式碼會指定要載入的導覽屬性。 使用`Load`方法以明確式載入。 例如: 

 ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* [消極式載入](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)。 [EF 核心目前不支援延遲載入](https://github.com/aspnet/EntityFrameworkCore/issues/3797)。 第一次讀取實體時，不被擷取相關的資料。 第一次存取導覽屬性時，自動擷取該導覽屬性所需的資料。 查詢會傳送至資料庫，每次存取導覽屬性第一次。

* `Select`運算子載入只有所需的相關的資料。

## <a name="create-a-courses-page-that-displays-department-name"></a>建立可顯示部門名稱的課程頁面

課程實體包含導覽屬性，其中包含`Department`實體。 `Department`實體包含課程指派給的部門。

若要指派的部門名稱在清單中顯示的課程：

* 取得`Name`屬性從`Department`實體。
* `Department`實體來自`Course.Department`導覽屬性。

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Scaffold 課程模型

* 結束 Visual Studio。
* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

上述命令 scaffold`Course`模型。 在 Visual Studio 中開啟專案。

建置專案。 建置會產生類似如下的錯誤：

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 全域變更`_context.Course`至`_context.Courses`(亦即，加入"s" `Course`)。 找到項目 7 及更新。

開啟*Pages/Courses/Index.cshtml.cs*並檢查`OnGetAsync`方法。 Scaffolding 引擎所指定的積極式載入`Department`導覽屬性。 `Include`方法指定積極式載入。

執行應用程式並選取**課程**連結。 Department 資料行顯示`DepartmentID`，不是很有用。

以下列程式碼更新 `OnGetAsync` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

上述程式碼會加入`AsNoTracking`。 `AsNoTracking`改善效能，因為傳回的實體不會受到追蹤。 因為它們不會更新目前的內容中，不會追蹤實體。

更新*Views/Courses/Index.cshtml*以反白顯示下列標記：

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

已 scaffold 的程式碼進行下列變更：

* 從索引的標題變更為 Courses 中。
* 加入**數目**顯示的資料行`CourseID`屬性值。 根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。 不過，在此情況下的主索引鍵是有意義。
* 變更**部門**資料行來顯示部門名稱。 程式碼會顯示`Name`屬性`Department`實體載入到`Department`導覽屬性：

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

執行應用程式並選取**課程**索引標籤，查看與部門名稱清單。

![課程索引頁](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>正在載入相關的資料與 Select

`OnGetAsync`方法會載入相關的資料與`Include`方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select`運算子載入只有所需的相關的資料。 針對單一項目，例如`Department.Name`它會使用 SQL INNER JOIN。 集合的方法，它可以使用另一個資料庫存取權，但會跟著移動。`Include` 在集合上的運算子。

下列程式碼會載入相關的資料與`Select`方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

請參閱[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml)和[IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)完成完整的範例。

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立講師頁面，其中顯示 Courses 以及註冊項目

在本節中，會建立講師頁面。

<a name="IP"></a>
![講師索引頁](read-related-data/_static/instructors-index.png)

此頁面讀取，並會顯示相關的資料以下列方式：

* 講師清單會顯示相關的資料`OfficeAssignment`實體 (Office 的上述影像中)。 `Instructor`和`OfficeAssignment`實體是以 0 或-1 一個關聯性中。 積極式載入用於`OfficeAssignment`實體。 需要顯示相關的資料時，通常更有效率積極式載入。 在此情況下，會顯示對講師 office 指派。
* 當使用者選取講師 (Harui 的上述影像中) 與相關`Course`實體便會顯示。 `Instructor`和`Course`實體是位於多對多關聯性。 Eager 載入`Course`實體和其相關`Department`使用實體。 在此情況下，個別查詢可能會更有效率因為只選取講師課程所需。 這個範例示範如何使用中實體的導覽屬性中的導覽屬性的積極式載入。
* 當使用者選取課程 （化學的上述影像中） 的相關資料從`Enrollments`顯示實體。 在上述映像中，會顯示學生名稱以及等級。 `Course`和`Enrollment`實體是位於一個對多關聯性。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立講師索引檢視的檢視模型

講師頁面會顯示下列三個不同資料表的資料。 檢視模型就會建立包含三個實體代表三個資料表。

在*SchoolViewModels*資料夾中，建立*InstructorIndexData.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Scaffold 講師模型

* 結束 Visual Studio。
* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

上述命令 scaffold`Instructor`模型。 在 Visual Studio 中開啟專案。

建置專案。 建置會產生錯誤。

全域變更`_context.Instructor`至`_context.Instructors`(亦即，加入"s" `Instructor`)。 找到項目 7 及更新。

執行應用程式，並瀏覽至講師頁面。

取代*Pages/Instructors/Index.cshtml.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

`OnGetAsync`方法會接受選擇性的路由資料選取講師的識別碼。

檢查查詢上*Pages/Instructors/Index.cshtml*頁面：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

查詢具有兩個包含：

* `OfficeAssignment`： 顯示在[講師檢視](#IP)。
* `CourseAssignments`： 它會教課程。


### <a name="update-the-instructors-index-page"></a>更新講師索引頁

更新*Pages/Instructors/Index.cshtml*以下列標記：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

上述標記進行下列變更：

* 更新`page`指示詞從`@page`至`@page "{id:int?}"`。 `"{id:int?}"`為路由範本。 路由範本變更路由資料的整數在 URL 中的查詢字串。 例如，按一下**選取**講師頁面指示詞會產生類似下列的 URL 的連結：

    `http://localhost:1234/Instructors?id=2`

    頁面指示詞時`@page "{id:int?}"`，先前的 URL 是：

    `http://localhost:1234/Instructors/2`

* 頁面標題**講師**。
* 加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。 因為這是以 0 或-1 一個關聯性時，有可能不相關的 OfficeAssignment 實體。

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* 加入**課程**資料行，顯示課程教導每個講師所。 請參閱[使用明確列轉換`@:`](xref:mvc/views/razor#explicit-line-transition-with-)如需有關此 razor 語法。

* 加入的程式碼動態新增`class="success"`至`tr`選取講師項目。 這會設定使用啟動程序的類別所選取的資料列的背景色彩。

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 加入新的超連結標示為**選取**。 此連結傳送所選的講師識別碼`Index`方法，並設定背景色彩。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

執行應用程式並選取**講師** 索引標籤。此頁面會顯示`Location`(office) 從相關`OfficeAssignment`實體。 如果 OfficeAssignment' 是空的資料表資料格會顯示 null。

![選取任何項目講師索引頁](read-related-data/_static/instructors-index-no-selection.png)

按一下**選取**連結。 資料列的樣式變更。

### <a name="add-courses-taught-by-selected-instructor"></a>加入所選講師教導課程

更新`OnGetAsync`方法中的*Pages/Instructors/Index.cshtml.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

檢查有更新的查詢：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

上述查詢會將加入`Department`實體。

下列程式碼執行時已選取 instructor (`id != null`)。 選取的講師會擷取從講師中檢視模型的清單。 檢視模型`Courses`屬性載入`Course`實體從該講師`CourseAssignments`導覽屬性。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where`方法傳回的集合。 在上述`Where`方法，只有一個`Instructor`傳回實體。 `Single`方法會將集合轉換成單一`Instructor`實體。 `Instructor`實體提供存取`CourseAssignments`屬性。 `CourseAssignments`提供存取相關的`Course`實體。

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

`Single`集合具有一個項目時，將會使用在集合上的方法。 `Single`方法擲回例外狀況，如果集合是空的或若有多個項目。 替代方式是`SingleOrDefault`，傳回的預設值 (null 在此情況下) 如果集合是空的。 使用`SingleOrDefault`空集合上：

* 造成例外狀況 (從嘗試尋找`Courses`對 null 參考的屬性)。
* 例外狀況訊息會較不清楚指出問題的原因。

下列程式碼會填入檢視模型`Enrollments`屬性選取課程時：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

加入下列標記的結尾*Pages/Courses/Index.cshtml* Razor 頁面：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

上述的標記會顯示一份時選取講師講師相關的課程。

測試應用程式。 按一下**選取**講師頁面上的連結。

![選取講師索引頁面講師](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>顯示學生資料

在本節中，應用程式會更新以顯示所選課程的學生資料。

更新中的查詢`OnGetAsync`方法中的*Pages/Instructors/Index.cshtml.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

更新*Pages/Instructors/Index.cshtml*。 將下列標記加入至檔案結尾：

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

上述的標記會顯示一份註冊的學生在選取的期間。

重新整理頁面，然後選取 instructor。 選取的課程，若要查看已註冊的學生版和其等級清單。

![講師索引頁面講師和選取的課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>使用單一

`Single`方法可以傳入`Where`條件，而不是呼叫`Where`方法分別：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

上述`Single`方法提供任何好處，透過使用`Where`。 某些開發人員偏好使用`Single`接近樣式。

## <a name="explicit-loading"></a>明確式載入

目前的程式碼指定的積極式載入`Enrollments`和`Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

假設很少，使用者會想要看的課程中的註冊項目。 在此情況下，最佳方法，就是要求時，只載入註冊資料。 在本節中，`OnGetAsync`更新為使用明確載入`Enrollments`和`Students`。

更新`OnGetAsync`為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

上述程式碼中會卸除*ThenInclude*方法會呼叫來註冊和學生的資料。 如果選取課程時，反白顯示的程式碼就會擷取：

* `Enrollment`實體所選的課程。
* `Student`每個實體`Enrollment`。

請注意上述程式碼註解`.AsNoTracking()`。 導覽屬性只能為明確載入的追蹤實體。

測試應用程式。 使用者的觀點而言，應用程式的行為即會相同到之前的版本。

下一個教學課程會示範如何更新相關的資料。

>[!div class="step-by-step"]
>[上一頁](xref:data/ef-rp/complex-data-model)
>[下一頁](xref:data/ef-rp/update-related-data)
