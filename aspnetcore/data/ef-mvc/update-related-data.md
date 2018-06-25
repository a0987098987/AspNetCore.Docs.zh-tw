---
title: ASP.NET Core MVC 與 EF Core - 更新相關資料 - 7/10
author: rick-anderson
description: 在本教學課程中，您會藉由更新外部索引鍵欄位和導覽屬性來更新相關資料。
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 53f1607d96a9a1db98f4e80e9582c124cedf6c8d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272646"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a>ASP.NET Core MVC 與 EF Core - 更新相關資料 - 7/10

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在先前的教學課程中，您顯示了相關資料。在本教學課程中，您會藉由更新外部索引鍵欄位和導覽屬性來更新相關資料。

下列圖例顯示了您將操作的一些頁面。

![Course [編輯] 頁面](update-related-data/_static/course-edit.png)

![Instructor [編輯] 頁面](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>自訂 Courses 的 [建立] 和 [編輯] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。 下拉式清單會設定 `Course.DepartmentID` 外部索引鍵屬性，以讓 Entity Framework 使用適當的 Department 實體載入 `Department` 導覽屬性。 您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。

在 *CoursesController.cs* 中，刪除四個 Create 及 Edit 方法，並以下列程式碼取代：

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

在 `Edit` HttpPost 方法後，建立一個新的方法，該方法會將部門資訊載入下拉式清單。

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` 方法會取得依照名稱排序的所有部門清單，為下拉式清單建立 `SelectList` 集合，然後將集合傳遞給位於 `ViewBag` 中的檢視。 方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。 檢視會將名稱 "DepartmentID" 傳遞到 `<select>` 標籤協助程式，協助程式接著便會知道要在 `ViewBag` 物件中尋找一個名為 "DepartmentID" 的 `SelectList`。

HttpGet `Create` 方法會呼叫 `PopulateDepartmentsDropDownList` 方法，而不設定選取項目，因為新課程所屬的部門還未建立：

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet `Edit` 方法會根據已指派給正在編輯之課程的部門識別碼來設定選取項目：

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

`Create` 和 `Edit` 的 HttpPost 方法都同時包含了在錯誤之後重新顯示頁面時設定選取項目的程式碼。 這可確保當頁面重新顯示以顯示錯誤訊息時，任何已選取的部門都會維持該選取狀態。

### <a name="add-asnotracking-to-details-and-delete-methods"></a>將 .AsNoTracking 新增至 Details 及 Delete 方法

若要最佳化 Course [詳細資料] 和 [刪除] 頁面的效能，請在 `Details` 和 HttpGet `Delete` 方法中新增 `AsNoTracking` 呼叫。

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>修改 Course 檢視

在 *Views/Courses/Create.cshtml* 中，將一個「選取部門」選項新增至 [部門] 下拉式清單，將標題從 **DepartmentID** 變更為 **Department**，然後新增一個驗證訊息。

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

在 *Views/Courses/Edit.cshtml* 中，為 [部門] 欄位進行您剛剛為 *Create.cshtml* 進行的相同變更。

同樣的，在 *Views/Courses/Edit.cshtml* 中，在 [標題] 欄位之前新增一個課程號碼欄位。 由於課程號碼是主索引鍵，雖然會顯示，但您無法變更它。

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

在 [編輯] 檢視中已有一個針對課程號碼的隱藏欄位 (`<input type="hidden">`)。 新增 `<label>` 標籤協助程式無法消除隱藏欄位的必要，因為它無法讓課程號碼包含在使用者按一下位於 [編輯] 頁面上的 [儲存] 時以 Post 方式提交的資料中。

在 *Views/Courses/Delete.cshtml* 中，在頂端新增一個課程號碼欄位，並將部門識別碼變更為部門名稱。

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

在 *Views/Courses/Details.cshtml* 中，進行您方才對 *Delete.cshtml* 進行的相同變更。

### <a name="test-the-course-pages"></a>測試 Course 頁面

執行應用程式，選取 [Course] 索引標籤，按一下 [新建]，並輸入新的課程資料：

![Course [建立] 頁面](update-related-data/_static/course-create.png)

按一下 [建立] 。 Courses [索引] 頁面便會顯示，並且清單中已有新建立的課程。 [索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。

按一下 Courses [索引] 頁面中課程的 [編輯]。

![Course [編輯] 頁面](update-related-data/_static/course-edit.png)

變更頁面上的資料，然後按一下 [儲存]。 Courses [索引] 頁面便會顯示，並且清單中已有更新的課程資料。

## <a name="add-an-edit-page-for-instructors"></a>為 Instructors 新增 [編輯 ] 頁面

當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。 Instructor 實體與 OfficeAssignment 實體具有一對零或一關聯性，表示您的程式碼必須處理下列狀況：

* 若使用者清除了原先擁有值的辦公室指派，刪除 OfficeAssignment 實體。

* 若使用者輸入了辦公室指派的值，而該指派原先是空白的，請建立新的 OfficeAssignment 實體。

* 若使用者變更辦公室指派的值，請變更現有 OfficeAssignment 實體中的值。

### <a name="update-the-instructors-controller"></a>更新 Instructor 控制器

在 *InstructorsController.cs* 中，變更 HttpGet `Edit` 方法中的程式碼，使其載入 Instructor 實體的 `OfficeAssignment` 導覽屬性，並呼叫 `AsNoTracking`：

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

使用下列程式碼取代 HttpPost `Edit` 方法來處理辦公室指派更新：

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

程式碼會執行下列操作：

-  將方法名稱變更為 `EditPost`，因為簽章目前與 HttpGet `Edit` 方法相同 (`ActionName` 屬性指出 `/Edit/` URL 仍在使用中)。

-  針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 Instructor 實體。 這與您在 HttpGet `Edit` 方法中所做的事情一樣。

-  使用從模型繫結器取得的值更新擷取的 Instructor 實體。 `TryUpdateModel` 多載可讓您將要包含的屬性加入允許清單中。 這可防止大量指派，如同在[第二個教學課程](crud.md)中所解釋的。

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   若辦公室位置為空白，將 Instructor.OfficeAssignment 屬性設為 Null，以刪除在 OfficeAssignment 資料表中的相關資料列。

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- 將變更儲存到資料庫。

### <a name="update-the-instructor-edit-view"></a>更新 Instructor [編輯] 檢視

在 *Views/Instructors/Edit.cshtml* 中，在 [儲存] 按鈕前的結尾處新增一個用於編輯辦公室位置的欄位：

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

執行應用程式，選取 [Instructor] 索引標籤，然後在講師上按一下 [編輯]。 變更 [辦公室位置]，然後按一下 [儲存]。

![Instructor [編輯] 頁面](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>將 Course 指派新增到 Instructor [編輯] 頁面

講師可教授任何數量的課程。 現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses.png)

Course 與 Instructor 實體的關係為多對多。 若要新增和移除關聯性，您必須在 CourseAssignments 聯結實體集中新增和移除實體。

可讓您變更講師指派之課程的 UI 為一組核取方塊。 資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。 使用者可選取或清除核取方塊來變更課程指派。 若課程數量要大上許多，您可能會想要使用不同的方法來在檢視中呈現資料，但您操縱聯結實體以建立或刪除關聯性的方法是相同的。

### <a name="update-the-instructors-controller"></a>更新 Instructor 控制器

若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。

在 *SchoolViewModels* 資料夾中建立 *AssignedCourseData.cs*，然後以下列程式碼取代現有的程式碼：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

在 *InstructorsController.cs* 中，使用下列程式碼取代 HttpGet `Edit` 方法。 所做的變更已醒目提示。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。

`PopulateAssignedCourseData` 方法中的程式碼會讀取所有的 Course 實體以使用檢視模型類別載入課程清單。 針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。 為了在檢查課程是否已指派給講師的過程中更有效率，指派給講師的課程會放入一個 `HashSet` 集合中。 `Assigned` 屬性會針對已指派給講師的課程設定為 true。 檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。 最後，清單會傳遞至位於 `ViewData` 的檢視中。

接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。 使用下列程式碼取代 `EditPost` 方法，然後新增一個方法，該方法會更新 Instructor 實體的 `Courses` 導覽屬性。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

方法簽章現在已和 HttpGet `Edit` 方法不同，因此方法名稱會從 `EditPost` 變回 `Edit`。

由於檢視沒有 Course 實體的集合，模型繫結器無法自動更新 `CourseAssignments` 導覽屬性。 相較於使用模型繫結器更新 `CourseAssignments` 導覽屬性，您會在新的 `UpdateInstructorCourses` 方法中進行相同的操作。 因此您必須從模型繫結器中排除 `CourseAssignments` 屬性。 這並不需要對呼叫 `TryUpdateModel` 的程式碼進行任何變更，因為您使用的是允許清單多載，且 `CourseAssignments` 並未位於包含清單中。

若沒有選取任何核取方塊，`UpdateInstructorCourses` 中的程式碼會使用空集合初始化 `CourseAssignments` 導覽屬性並傳回：

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。 為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。

若課程的核取方塊已被選取，但課程並未位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會新增至導覽屬性的集合中。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

若課程的核取方塊未被選取，但課程卻位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會從導覽屬性的集合中移除。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>更新 Instructor 檢視

在 *Views/Instructors/Edit.cshtml* 中，藉由將下列程式碼新增到 [辦公室] 欄位的 `div` 項目後及 [儲存] 按鈕的 `div` 項目前，來新增 [課程 ] 欄位與核取方塊陣列。

<a id="notepad"></a>
> [!NOTE] 
> 當您將程式碼貼至 Visual Studio 時，分行符號可能會產生變更使程式碼失效。  按 Ctrl+Z 來復原自動格式化。  這會修正分行符號，使他們看起來就跟您在這裡看到的一樣。 縮排不一定要是完美的，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@:</tr>` 必須要如顯示般各自在獨立的一行上，否則您會接收到執行階段錯誤。 當選取新的程式碼區塊時，按 Tab 鍵三次來讓新的程式碼對準現有的程式碼。 您可以在[這裡](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)檢查此問題的狀態。

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

此程式碼會建立一個 HTML 表格，該表格中有三個資料行。 在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。 所有核取方塊的名稱都是 ("selectedCourses")，會告知模型繫結器應將其視為一個群組。 每個核取方塊的 Value 屬性都會設為 `CourseID` 的值。 當頁面以 post 方式提交時，模型繫結器便會將只包含我們選取核取方塊之 `CourseID` 值的陣列傳遞到控制器。

核取方塊一開始呈現時，已指派給該名講師的課程便會帶有 Checked 屬性，使其顯示為已選取狀態。

執行應用程式，選取 [Instructor] 索引標籤，然後按一下講師上的 [編輯] 以查看 [編輯] 頁面。

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses.png)

變更一些課程指派，然後按一下 [儲存]。 您所做的變更會反映在 [索引] 頁面上。

> [!NOTE] 
> 這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。 針對更大的集合，將需要不同的 UI 和不同的更新方法。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

在 *InstructorsController.cs* 中，刪除 `DeleteConfirmed` 方法並在相同位置插入下列程式碼。

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

此程式碼會進行下列變更：

* 為 `CourseAssignments` 導覽屬性進行積極式載入。  您必須包含這個，否則 EF 將無法得知相關 `CourseAssignment` 而無法刪除他們。  若要避免在此讀取他們，您可以在資料庫中設定串聯刪除。

* 若要刪除的講師已指派為任何部門的系統管理員，請先從部門中移除講師的指派。

## <a name="add-office-location-and-courses-to-the-create-page"></a>將辦公室位置和課程新增至 [新增] 頁面

在 *InstructosController.cs* 中，刪除 HttpGet 和 HttpPost `Create` 方法，然後在相同位置新增下列程式碼：

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

此程式碼與您在 `Edit` 方法中看到的類似，除了一開始沒有選取任何課程之外。 HttpGet `Create` 方法會呼叫 `PopulateAssignedCourseData` 方法，不是因為可能會有已選取的課程，而是為了提供空集合給檢視中的 `foreach` 迴圈 (否則檢視程式碼會擲回 Null 參考例外狀況)。

HttpPost `Create` 方法會在檢查驗證錯誤並將新的講師新增到資料庫前將每個選取的課程新增到 `CourseAssignments` 導覽屬性中。 即使發生模型錯誤，課程也會新增，這使得當發生模型錯誤 (例如使用者鍵入了無效的日期)，並且頁面重新顯示並帶有錯誤訊息時，任何課程選取都會自動還原。

請注意，為了要能夠將課程新增到 `CourseAssignments` 導覽屬性，您必須將屬性以空集合初始化：

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

作為在控制器程式碼中完成這項操作的替代方案，您可以在 Instructor 模型中藉由將屬性 getter 變更為在不存在時自動建立集合來完成，如以下範例所示：

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

若您使用這種方式修改了 `CourseAssignments` 屬性，您便可以移除控制器中的明確屬性初始化程式碼。

在 *Views/Instructor/Create.cshtml* 中，在 [提交] 按鈕前新增一個辦公室位置文字方塊及課程核取方塊。 若為 [編輯] 頁面，請[修正 Visual Studio 於您貼上時重新格式化程式碼](#notepad)。

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

執行應用程式並建立一名講師，以進行測試。 

## <a name="handling-transactions"></a>處理交易

如同在 [CRUD 教學課程](crud.md)中所述，Entity Framework 隱含實作了交易。 針對您需要更多控制的案例 -- 例如，若您想要在一個交易中包含在 Entity Framework 之外完成的作業 -- 請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。

## <a name="summary"></a>總結

現在您已完成了操作相關資料的簡介。 在下一個教學課程中，您會了解到如何處理並行衝突。

> [!div class="step-by-step"]
> [上一頁](read-related-data.md)
> [下一頁](concurrency.md)  
