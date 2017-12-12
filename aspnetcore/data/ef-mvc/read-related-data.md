---
title: "ASP.NET MVC EF 核心-讀取核心相關的資料-10-6"
author: tdykstra
description: "在本教學課程中，您將讀取並顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。"
keywords: "加入 ASP.NET Core，Entity Framework Core，相關資料，"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>讀取的相關資料的 EF Core 與 ASP.NET Core MVC 教學課程 (10-6)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您可以完成學校資料模型。 在本教學課程中，將讀取和顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。

下圖顯示的頁面，您將使用。

![課程索引頁](read-related-data/_static/courses-index.png)

![講師索引頁](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>急切、 明確，和消極式載入之相關資料

有幾種物件關聯式對應 (ORM) 軟體例如 Entity Framework 可以載入實體的導覽屬性的相關的資料：

* 積極式載入。 實體讀取時，同時擷取的相關的資料。 這通常會導致單一聯結查詢以擷取所有所需的資料。 在 Entity Framework Core 指定使用積極式載入`Include`和`ThenInclude`方法。

  ![積極式載入範例](read-related-data/_static/eager-loading.png)

  您可以擷取一些個別的查詢中的資料和 EF 「 修復 」 的導覽屬性。  也就是說，EF 中，自動加入個別擷取的實體所屬的先前擷取的實體的導覽屬性。 查詢中擷取相關的資料，您可以使用`Load`方法，而不是傳回的清單或物件，例如方法`ToList`或`Single`。

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

* 明確式載入。 第一次讀取實體時，不被擷取相關的資料。 您撰寫程式碼，以便擷取相關的資料需要。 如同 eager 載入個別查詢的情況，明確載入多個查詢的結果傳送至資料庫。 差別在於，具有明確式載入，程式碼會指定要載入的導覽屬性。 您可以使用 Entity Framework Core 1.1`Load`方法以明確式載入。 例如: 

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* 消極式載入。 第一次讀取實體時，不被擷取相關的資料。 不過，第一次您嘗試存取的導覽屬性，該導覽屬性所需的資料自動擷取。 每次您嘗試從導覽屬性取得資料，第一次傳送查詢到資料庫。 Entity Framework Core 1.0 不支援消極式載入。

### <a name="performance-considerations"></a>效能考量

如果您知道您需要針對擷取每個實體的相關的資料，積極式載入通常提供最佳效能，因為傳送至資料庫的單一查詢通常比針對擷取每個實體個別查詢更有效率。 例如，假設每個部門各有十個相關的課程。 積極式載入的所有相關資料會導致只的單一 （加入） 查詢，並在單一來回行程到資料庫。 個別的查詢，針對每個部門的課程會導致資料庫 11 個往返。 延遲性很高時，特別是危害到效能額外反覆存取資料庫。

相反地，在某些案例中個別查詢會更有效率。 在單一查詢中的所有相關資料的積極式載入可能會導致非常複雜聯結來產生，SQL Server 無法有效率地進行處理。 或如果您需要存取實體的導覽屬性，只會針對您正在處理的實體集的子集，個別查詢可能會更好因為積極式載入的所有項目最前面位置會擷取比您所需的更多資料。 如果效能嚴重不足，所以最好先測試效能才能做出最好的選擇這兩種方式。

## <a name="create-a-courses-page-that-displays-department-name"></a>建立可顯示部門名稱的課程頁面

課程實體包括包含課程指派給部門的 Department 實體的導覽屬性。 若要指派的部門名稱在清單中顯示的課程，您需要從 Department 實體中取得的 Name 屬性`Course.Department`導覽屬性。

建立名為 CoursesController 課程實體類型，使用的相同選項的控制站**使用 Entity Framework 的檢視，MVC 控制器**scaffolder 您對先前的學生控制站，如中所示下圖：

![加入課程控制器](read-related-data/_static/add-courses-controller.png)

開啟*CoursesController.cs*並檢查`Index`方法。 自動 scaffolding 已指定的積極式載入`Department`導覽屬性使用`Include`方法。

取代`Index`方法與使用更適合的名稱，如下列程式碼`IQueryable`傳回課程實體 (`courses`而不是`schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

開啟*Views/Courses/Index.cshtml*和範本程式碼取代為下列程式碼。 所做的變更會反白顯示：

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

您已對 scaffold 的程式碼進行下列變更：

* 從索引的標題變更為 Courses 中。

* 加入**數目**顯示的資料行`CourseID`屬性值。 根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。 不過，在此情況下的主索引鍵是有意義，而且您想要將其顯示。

* 變更**部門**資料行來顯示部門名稱。 程式碼會顯示`Name`Department 實體載入到屬性`Department`導覽屬性：

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

執行應用程式並選取**課程**索引標籤，查看與部門名稱清單。

![課程索引頁](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立講師頁面，其中顯示 Courses 以及註冊項目

在本節中，您將建立控制器和檢視 [Instructor] 實體，以顯示講師頁面：

![講師索引頁](read-related-data/_static/instructors-index.png)

此頁面讀取，並會顯示相關的資料以下列方式：

* 講師清單會顯示從 OfficeAssignment 實體的相關的資料。 「 講師 」 和 「 OfficeAssignment 實體是以 0 或-1 一個關聯性中。 您將用於 OfficeAssignment 實體的積極式載入。 如前所述，積極式載入時，通常更有效率主資料表的所有擷取的資料列所需的相關的資料。 在此情況下，您會想要顯示為顯示的所有講師 office 指派。

* 當使用者選取講師時，會顯示相關的課程實體。 「 講師 」 和 「 課程實體是多對多關聯性中。 您會使用課程實體和其相關的 Department 實體的積極式載入。 在此情況下，個別查詢可能會更有效率因為您只會針對所選講師需要課程。 不過，此範例會示範如何使用積極式載入本身是導覽屬性中的實體內的導覽屬性。

* 當使用者選取課程之後時，會顯示註冊實體集的相關的資料。 「 課程 」 和 「 註冊 」 實體是一對多關聯性中。 您將註冊的實體和其相關的學生實體使用不同的查詢。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立講師索引檢視的檢視模型

講師頁面會顯示下列三個不同資料表的資料。 因此，您將建立包含三個屬性，每個保留其中一個資料表的資料檢視模型。

在*SchoolViewModels*資料夾中，建立*InstructorIndexData.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>建立講師控制器和檢視

建立講師控制器 EF 讀取/寫入動作，如下圖所示：

![新增講師控制站](read-related-data/_static/add-instructors-controller.png)

開啟*InstructorsController.cs*和加入 using ViewModels 命名空間陳述式：

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Index 方法取代為下列程式碼相關資料的積極式載入作業，並將其放在檢視模型。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

該方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`) 所提供的識別碼值選取的講師和選取的課程。 參數所提供的**選取**頁面上的超連結。

程式碼首先會建立檢視模型的執行個體，並放在它的講師清單。 程式碼會指定的積極式載入`Instructor.OfficeAssignment`和`Instructor.CourseAssignments`導覽屬性。 內`CourseAssignments`屬性，`Course`屬性是載入，並在其中，`Enrollments`和`Department`屬性已載入，並在每個`Enrollment`實體`Student`屬性載入。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

由於檢視一律需要 OfficeAssignment 實體，會在相同查詢中擷取的更有效率。 單一查詢是優於多個查詢，只有當頁面會顯示頻率課程，而不選取比在網頁上，選取講師時需要課程實體。

程式碼重複`CourseAssignments`和`Course`因為您需要從兩個屬性`Course`。 第一個字串的`ThenInclude`呼叫取得`CourseAssignment.Course`， `Course.Enrollments`，和`Enrollment.Student`。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

此時，在程式碼的另一個`ThenInclude`導覽屬性是`Student`，您不需要。 但呼叫`Include`透過開頭`Instructor`屬性，所以您不必瀏覽鏈結同樣地，此時間指定`Course.Department`而不是`Course.Enrollments`。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

當講師已選取時，就會執行下列程式碼。 選取的講師會擷取從講師中檢視模型的清單。 檢視模型`Courses`屬性然後載入該講師課程實體與`CourseAssignments`導覽屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where`方法會傳回集合，但準則在此情況下傳遞至該方法會導致只有單一 Instructor 實體傳回。 `Single`方法會將集合轉換成可讓您存取與該實體單一講師實體`CourseAssignments`屬性。 `CourseAssignments`屬性包含`CourseAssignment`實體，在您想只相關`Course`實體。

您使用`Single`集合，當您知道集合的方法將會有只有一個項目。 如果是空的集合傳遞給它，或若有多個項目，則單一方法會擲回例外狀況。 替代方式是`SingleOrDefault`，傳回的預設值 (null 在此情況下) 如果集合是空的。 不過，在此情況下，就仍然造成例外狀況 (從嘗試尋找`Courses`對 null 參考的屬性)，以及例外狀況訊息會較不清楚指出問題的原因。 當您呼叫`Single`方法，您也可以傳遞在 where 條件，而不是呼叫`Where`方法分別：

```csharp
.Single(i => i.ID == id.Value)
```

而非：

```csharp
.Where(I => i.ID == id.Value).Single()
```

接下來，如果已選取課程，選定的課程從擷取中檢視模型的課程的清單。 然後檢視模型的`Enrollments`屬性載入與註冊的實體，從該課程`Enrollments`導覽屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>修改講師索引檢視表

在*Views/Instructors/Index.cshtml*，範本程式碼取代為下列程式碼。 所做的變更會反白顯示。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

您已對現有的程式碼進行下列變更：

* 變更模型類別`InstructorIndexData`。

* 變更頁面標題從**索引**至**講師**。

* 加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。 （因為這是以 0 或-1 一個關聯性，可能不會有相關的 OfficeAssignment 實體。）

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
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 加入新的超連結標示為**選取**之前中每個資料列的其他連結，而使得所選的講師識別碼傳送至`Index`方法。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

執行應用程式並選取**講師** 索引標籤。沒有相關的 OfficeAssignment 實體時，頁面就會顯示相關的 OfficeAssignment 實體和空的資料表資料格的 Location 屬性。

![選取任何項目講師索引頁](read-related-data/_static/instructors-index-no-selection.png)

在*Views/Instructors/Index.cshtml*檔案之後關閉資料表項目 （在檔案的結尾），, 加入下列程式碼。 此程式碼會顯示一份時選取講師講師相關的課程。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

此程式碼讀取`Courses`要顯示的課程清單的檢視模型的屬性。 它也提供**選取**超連結，將傳送至所選的課程識別碼`Index`動作方法。

重新整理頁面，然後選取 instructor。 現在您會看到一個方格，其中會顯示指派給所選講師，課程，每個課程中，您看到指派的部門名稱。

![選取講師索引頁面講師](read-related-data/_static/instructors-index-instructor-selected.png)

之後您剛才加入的程式碼區塊，加入下列程式碼。 在課程中，選取該課程時，這會顯示一份學生註冊。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

此程式碼會讀取檢視模型的註冊項目屬性，以顯示一份學生課程中註冊。

再次重新整理頁面，然後選取 instructor。 然後，選取課程來查看已註冊的學生版和其等級清單。

![講師索引頁面講師和選取的課程](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>明確式載入

當您擷取在講師清單*InstructorsController.cs*，您所指定的積極式載入`CourseAssignments`導覽屬性。

假設您預期使用者很少想要查看所選的講師和課程中的註冊項目。 在此情況下，可能會想要載入註冊資料才會在收到要求。 若要查看如何明確載入的範例，取代`Index`為下列程式碼，以移除註冊的積極式載入，並明確地載入該屬性的方法。 程式碼變更會反白顯示。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

新的程式碼會卸除*ThenInclude*方法會呼叫註冊資料來擷取講師實體的程式碼。 如果選取講師和課程，反白顯示的程式碼會擷取所選的課程中，註冊實體和每個註冊的學生實體。

執行應用程式，請移至講師索引頁面現在，您會看到任何差異顯示在頁面上，雖然您已變更資料擷取的方式。

## <a name="summary"></a>總結

您現在所積極式載入一個查詢與多個查詢相關的資料讀取到導覽屬性。 在下一個教學課程中，您將學習如何更新相關的資料。

>[!div class="step-by-step"]
>[上一頁](complex-data-model.md)
>[下一頁](update-related-data.md)  
