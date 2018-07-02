---
title: ASP.NET Core MVC 與 EF Core - 讀取相關資料 - 6/10
author: rick-anderson
description: 在此教學課程中，您將讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092980"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a>ASP.NET Core MVC 與 EF Core - 讀取相關資料 - 6/10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在上一個教學課程中，您已完成 School 資料模型。 在此教學課程中，您將讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。

下列圖例顯示了您將操作的頁面。

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>相關資料的積極式、明確式和消極式載入

Entity Framework 等物件關聯式對應 (ORM) 有幾種方式可以將相關資料載入到實體的導覽屬性：

* 積極式載入。 讀取實體時，將會同時擷取其相關資料。 這通常會導致單一聯結查詢，其可擷取所有需要的資料。 您可以使用 `Include` 和 `ThenInclude` 方法，在 Entity Framework Core 中指定積極式載入。

  ![積極式載入範例](read-related-data/_static/eager-loading.png)

  您可以在個別查詢中擷取一些資料，而 EF 會「修正」導覽屬性。  也就是說，EF 會自動新增個別擷取的實體，其中它們屬於先前擷取之實體的導覽屬性。 對於擷取相關資料的查詢，您可以使用 `Load` 方法，而不是傳回清單或物件的方法，例如 `ToList` 或 `Single`。

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

* 明確式載入。 第一次讀取實體時，不會擷取相關資料。 您可以撰寫程式碼，以擷取需要的相關資料。 如同使用個別查詢的積極式載入，明確式載入會導致多個查詢傳送至資料庫。 不同之處在於，使用明確式載入時，程式碼會指定要載入的導覽屬性。 在 Entity Framework Core 1.1 中，您可以使用 `Load` 方法以執行明確式載入。 例如: 

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* 消極式載入。 第一次讀取實體時，不會擷取相關資料。 不過，第一次嘗試存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。 每當您第一次嘗試從導覽屬性取得資料時，就會傳送查詢到資料庫。 Entity Framework Core 1.0 不支援消極式載入。

### <a name="performance-considerations"></a>效能考量

如果您知道擷取的每個實體需要相關資料，積極式載入通常可以提供最佳效能，因為傳送至資料庫的單一查詢通常比所擷取每個實體的個別查詢更有效率。 例如，假設每個部門各有十個相關課程。 所有相關資料的積極式載入會導致只有單一 (聯結) 查詢，以及資料庫的單一來回行程。 每個部門課程的個別查詢則會導致資料庫的十一個來回行程。 當延遲很高時，資料庫的額外來回行程對效能特別不利。

相反地，在某些案例中，個別查詢更有效率。 在單一查詢中進行所有相關資料的積極式載入時，可能會導致產生非常複雜的聯結，SQL Server 無法有效率地進行處理。 或者，如果您只需要針對所處理之實體集的子集存取實體的導覽屬性，則執行個別查詢可能會更好；因為預先進行所有項目的積極式載入可能會擷取比您所需更多的資料。 如果效能嚴重不足，最好先測試這兩種方式的效能，才能做出最好的選擇。

## <a name="create-a-courses-page-that-displays-department-name"></a>建立顯示部門名稱的 Courses 頁面

Course 實體包括一個導覽屬性，其中包含已指派課程之部門的 Department 實體。 若要在課程清單中顯示所指派部門的名稱，您需要從位於 `Course.Department` 導覽屬性的 Department 實體中取得 Name 屬性。

針對 Course 實體類型建立名為 CoursesController 的控制器，並對**使用 Entity Framework 執行檢視的 MVC 控制器**框架使用先前針對學生控制器使用的相同選項，如下圖所示：

![新增課程控制器](read-related-data/_static/add-courses-controller.png)

開啟 *CoursesController.cs*，並檢查 `Index` 方法。 自動 Scaffolding 已使用 `Include` 方法，針對 `Department` 導覽屬性指定積極式載入。

以下列程式碼取代 `Index` 方法，以針對傳回 Course 實體的 `IQueryable` 使用更合適的名稱 (`courses` 而不是 `schoolContext`)：

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

開啟 *Views/Courses/Index.cshtml*，並以下列程式碼取代範本程式碼。 所做的變更已醒目提示：

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

您已對包含 Scaffold 的程式碼進行下列變更：

* 已將標題從「索引」) 變更為「課程」。

* 新增顯示 `CourseID` 屬性值的 [編號] 資料行。 主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。 不過，在此情況下主索引鍵有意義，因此您想要顯示它。

* 變更 [部門] 資料行來顯示部門名稱。 此程式碼會顯示已載入到 `Department` 導覽屬性之 Department 實體的 `Name` 屬性：

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

執行應用程式，並選取 [Courses] 索引標籤來查看含有部門名稱的清單。

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立顯示課程和註冊的 Instructors 頁面

在本節中，您將建立 Instructor 實體的控制器和檢視，以顯示 Instructors 頁面：

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

此頁面將以下列方式讀取和顯示相關資料：

* 講師清單會顯示來自 OfficeAssignment 實體的相關資料。 Instructor 與 OfficeAssignment 實體具有一對零或一關聯性。 您將針對 OfficeAssignment 實體使用積極式載入。 如上所述，當您需要主要資料表中所有擷取資料列的相關資料時，積極式載入通常更有效率。 在此情況下，您可能想要顯示所有已呈現講師的辦公室指派。

* 當使用者選取講師時，將會顯示相關的 Course 實體。 Instructor 與 Course 實體具有多對多關聯性。 您將針對 Course 實體和其相關的 Department 實體使用積極式載入。 在此情況下，個別查詢可能更有效率，因為您只需要所選取講師的課程。 不過，這個範例會示範如何在本身處於導覽屬性內的實體中，針對導覽屬性使用積極式載入。

* 當使用者選取課程時，將會顯示來自 Enrollments 實體集的相關資料。 Course 與 Enrollment 實體具有一對多關聯性。 您將針對 Enrollment 實體和其相關的 Student 實體使用個別查詢。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立 Instructor [索引] 檢視的檢視模型

Instructors 頁面會顯示下列三個不同資料表的資料。 因此，您將建立包含三個屬性的檢視模型，每個保留其中一個資料表的資料。

在 *SchoolViewModels* 資料夾中建立 *InstructorIndexData.cs*，然後以下列程式碼取代現有的程式碼：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>建立 Instructor 控制器和檢視

使用 EF 讀取/寫入動作建立 Instructors 控制器，如下圖所示：

![新增 Instructors 控制器](read-related-data/_static/add-instructors-controller.png)

開啟 *InstructorsController.cs*，並針對 ViewModels 命名空間新增 using 陳述式：

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

以下列程式碼取代 Index 方法，以便執行相關資料的積極式載入，並將其置於檢視模型中。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

此方法可接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，以提供所選取講師和所選取課程的識別碼值。 這些參數由頁面上的**選取**超連結提供。

此程式碼是從建立檢視模型的執行個體，並將其置於講師清單開始。 這個程式碼會針對 `Instructor.OfficeAssignment` 和 `Instructor.CourseAssignments` 導覽屬性指定積極式載入。 在 `CourseAssignments` 屬性內載入 `Course` 屬性，並在其中載入 `Enrollments` 和 `Department` 屬性，而在每個 `Enrollment` 實體內載入 `Student` 屬性。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

由於檢視一律需要 OfficeAssignment 實體，因此在相同查詢中擷取該實體更有效率。 在網頁上選取講師時需要 Course 實體，因此只有在選取課程時的頁面顯示頻率高於未選取時，單一查詢才優於多個查詢。

此程式碼會重複 `CourseAssignments` 和 `Course`，因為您需要來自 `Course` 的兩個屬性。 `ThenInclude` 呼叫的第一個字串會取得 `CourseAssignment.Course`、`Course.Enrollments` 和 `Enrollment.Student`。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

此時，程式碼中的另一個 `ThenInclude` 將用於您不需要的 `Student` 導覽屬性。 但呼叫 `Include` 會使用 `Instructor`　屬性從頭開始，所以您必須再次瀏覽鏈結，這次請指定 `Course.Department` 而不是 `Course.Enrollments`。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

下列程式碼會在已選取講師時執行。 選取的講師會從檢視模型的講師清單中擷取。 檢視模型的 `Courses` 屬性則使用 Course 實體從該講師的 `CourseAssignments` 導覽屬性載入。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` 方法會傳回集合，但在此情況下，傳遞至該方法的準則將導致只傳回單一 Instructor 實體。 `Single` 方法會將集合轉換單一 Instructor 實體，這可讓您存取該實體的 `CourseAssignments` 屬性。 `CourseAssignments` 屬性包含 `CourseAssignment` 實體，您只想要來自該實體的相關 `Course` 實體。

當您知道集合只會有一個項目時，就可以在集合上使用 `Single` 方法。 如果傳遞的集合是空的或有多個項目，Single 方法會擲回例外狀況。 替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。 不過，在此情況下仍然會造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)，而例外狀況訊息會不太清楚地指出問題的原因。 當您呼叫 `Single` 方法時，也可以傳入 Where 條件，而不是分別呼叫 `Where` 方法：

```csharp
.Single(i => i.ID == id.Value)
```

而非：

```csharp
.Where(I => i.ID == id.Value).Single()
```

接下來，如果已選取課程，則會從檢視模型的課程清單中擷取選取的課程。 然後，檢視模型的 `Enrollments` 屬性會使用 Enrollment 實體從該課程的 `Enrollments` 導覽屬性載入。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>修改 Instructor [索引] 檢視

在 *Views/Instructors/Index.cshtml* 中，以下列程式碼取代範本程式碼。 所做的變更已醒目提示。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

您已對現有程式碼進行下列變更：

* 已將模型類別變更為 `InstructorIndexData`。

* 已將頁面標題從**索引**變更為**講師**。

* 新增 [辦公室] 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。 (因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* 新增 [課程] 資料行，以顯示每位講師所教授的課程。 如需此 Razor 語法的詳細資訊，請參閱[使用 `@:` 的明確行轉換](xref:mvc/views/razor#explicit-line-transition-with-)。

* 新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。 這會使用啟動程序類別設定所選取資料列的背景色彩。

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 在每個資料列的其他連結之前，立即新增標示為**選取**的超連結，這會使得選取的講師識別碼傳送到 `Index` 方法。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

執行應用程式並選取 [講師]  索引標籤。沒有相關的 OfficeAssignment 實體時，此頁面就會顯示相關 OfficeAssignment 實體的 Location 屬性和空的資料表儲存格。

![Instructors [索引] 頁面未選取任何項目](read-related-data/_static/instructors-index-no-selection.png)

在 *Views/Instructors/Index.cshtml* 檔案的結束資料表項目 (在檔案的結尾) 之後，新增下列程式碼。 當選取講師時，此程式碼會顯示與講師相關的課程。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

此程式碼會讀取檢視模型的 `Courses` 屬性以顯示課程清單。 它也會提供**選取**超連結，將所選取課程的識別碼傳送至 `Index` 動作方法。

重新整理頁面，然後選取講師。 現在您會看到一個方格，其中顯示指派給所選取講師的課程，而且在每個課程中，您可以看到指派的部門名稱。

![Instructors [索引] 頁面已選取講師](read-related-data/_static/instructors-index-instructor-selected.png)

在您剛才新增的程式碼區塊之後，新增下列程式碼。 這會在選取課程時，顯示已註冊該課程的學生清單。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

此程式碼會讀取檢視模型的 Enrollments 屬性，以顯示已註冊課程的學生清單。

再次重新整理頁面，然後選取講師。 接著選取課程，以查看已註冊學生和其年級的清單。

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>明確式載入

當您在 *InstructorsController.cs* 中擷取講師清單時，已針對 `CourseAssignments` 導覽屬性指定積極式載入。

假設您預期使用者很少想要查看所選取講師和課程中的註冊項目。 在此情況下，您可能想要只在要求時才載入註冊資料。 若要查看如何執行明確式載入的範例，請以下列程式碼取代 `Index` 方法，這會移除 Enrollments 的積極式載入，並明確地載入該屬性。 程式碼變更已醒目提示。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

新的程式碼會從擷取 Instructor 實體的程式碼中，捨棄用於註冊資料的 *ThenInclude* 方法呼叫。 如果選取了講師和課程，醒示提示的程式碼就會針對所選取的課程擷取 Enrollment 實體，並針對每個 Enrollment 擷取 Student 實體。

執行應用程式，並立即移至 Instructors [索引] 頁面；雖然您已變更資料的擷取方式，但您會發現頁面上顯示的內容沒有任何差異。

## <a name="summary"></a>總結

您現在已使用積極式載入搭配一個查詢與多個查詢，將相關資料讀取到導覽屬性。 在下一個教學課程中，您將了解如何更新相關資料。

::: moniker-end

>[!div class="step-by-step"]
>[上一頁](complex-data-model.md)
>[下一頁](update-related-data.md)
