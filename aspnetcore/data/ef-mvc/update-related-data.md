---
title: "ASP.NET Core MVC EF Core-更新與相關資料-10-7"
author: tdykstra
description: "在本教學課程中，您要更新相關的資料藉由更新外部索引鍵欄位，而且導覽屬性。"
keywords: "加入 ASP.NET Core，Entity Framework Core，相關資料，"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 85686fe4ebf95f95dc672fbc2d23cddd5bee85e5
ms.sourcegitcommit: 605dc99d241b6d955432bcd42c0178e6e6a212fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>更新相關的資料-EF Core 與 ASP.NET Core MVC 教學課程 (10-7)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

您可以在上一個教學課程中顯示相關的資料。在本教學課程中，您要更新相關的資料藉由更新外部索引鍵欄位，而且導覽屬性。

下圖顯示一些您將使用的頁面。

![課程編輯頁面](update-related-data/_static/course-edit.png)

![講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>建立和編輯網頁自訂課程

建立新的課程實體時，它必須有現有的部門關聯性。 若要達成此目的，scaffold 的程式碼包含控制器方法以及建立與編輯檢視，包含下拉式清單中選取部門。 下拉式清單設定`Course.DepartmentID`外部索引鍵屬性，而這就是 Entity Framework 必須以載入所有`Department`與適當的 Department 實體的導覽屬性。 您將使用 scaffold 的程式碼，但變更它稍微來加入錯誤處理和排序下拉式清單。

在*CoursesController.cs*刪除四種建立與編輯方法，取代為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

之後`Edit`HttpPost 方法建立新的方法，以載入部門資訊的下拉式清單。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`集合下拉式清單中，並將集合傳遞給在檢視`ViewBag`。 該方法會接受選擇性`selectedDepartment`參數，可讓呼叫的程式碼，指定將會呈現下拉式清單時所選取的項目。 檢視會將"DepartmentID 」 的名稱傳遞給`<select>`標記協助程式，以及協助程式就會知道要查看`ViewBag`物件`SelectList`名為"DepartmentID"。

HttpGet`Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新的課程部門不會建立尚未方法：

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet`Edit`方法設定選取的項目，根據已指派給正在編輯的課程部門的識別碼：

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

兩者的 HttpPost 方法`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼。 這可確保，當頁面會重新顯示，以顯示錯誤訊息，已選取任何部門會持續選取。

### <a name="add-asnotracking-to-details-and-delete-methods"></a>加入。AsNoTracking 詳細資料和 Delete 方法

若要最佳化效能的課程詳細資料及刪除頁，將`AsNoTracking`中呼叫`Details`和 HttpGet`Delete`方法。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>修改課程檢視

在*Views/Courses/Create.cshtml*，新增 「 選取部門 」 選項，以**部門**下拉式清單中，變更從標題**DepartmentID**至**部門**，並新增驗證訊息。

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

在*Views/Courses/Edit.cshtml*，進行相同的變更，只要未在該部門欄位*Create.cshtml*。

此外，在*Views/Courses/Edit.cshtml*，加入課程數字欄位之前**標題**欄位。 課程數字是主索引鍵，因為它會顯示，但無法變更。

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

已隱藏的欄位 (`<input type="hidden">`) 中編輯檢視課程編號。 加入`<label>`標記協助程式不會消除隱藏欄位的需要，因為它並不會造成要包含在張貼的資料，當使用者按一下的課程編號**儲存**上**編輯**頁面。

在*Views/Courses/Delete.cshtml*、 上方加入課程數字欄位，並將 department ID 變更為部門名稱。

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

在*Views/Course/Details.cshtml*，進行相同的變更，您只對*Delete.cshtml*。

### <a name="test-the-course-pages"></a>測試課程頁面

執行**建立**頁面 (顯示課程索引頁，然後按一下**新建**) 並輸入新的課程資料：

![課程建立頁面](update-related-data/_static/course-create.png)

按一下 [建立] 。 課程索引頁會顯示新的課程新增至清單。 在索引頁面的清單中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。

執行**編輯**頁面 (按一下**編輯**課程中的課程索引頁面上)。

![課程編輯頁面](update-related-data/_static/course-edit.png)

變更網頁上的資料，然後按一下**儲存**。 課程索引頁會顯示更新的課程資料。

## <a name="add-an-edit-page-for-instructors"></a>新增對講師編輯頁面

當您編輯講師記錄時，您想要能夠更新講師 office 指派。 [Instructor] 實體具有以零或-1 個關係 OfficeAssignment 實體，這表示您的程式碼必須處理在下列情況：

* 如果使用者清除 office 指派它最初的值，請刪除 OfficeAssignment 實體。

* 如果使用者輸入 office 指派值，而且它最初是空的建立新的 OfficeAssignment 實體。

* 如果使用者變更 office 指派的值，變更現有 OfficeAssignment 實體中的值。

### <a name="update-the-instructors-controller"></a>更新講師控制站

在*InstructorsController.cs*，HttpGet 在變更程式碼`Edit`方法，使它載入 [Instructor] 實體`OfficeAssignment`導覽屬性並呼叫`AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

取代 HttpPost`Edit`方法來處理 office 指派更新為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

程式碼會執行下列動作：

-  變更方法名稱，以`EditPost`因為簽章現在是 HttpGet 相同`Edit`方法 (`ActionName`屬性指定`/Edit/`仍會使用 URL)。

-  取得資料庫使用的目前 [Instructor] 實體積極式載入`OfficeAssignment`導覽屬性。 這是您未在 HttpGet 相同`Edit`方法。

-  更新擷取 [Instructor] 實體中的模型繫結器的值。 `TryUpdateModel`多載可讓您將白名單您想要包含的屬性。 這可防止過度公佈時，所述[第二個教學課程](crud.md)。

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   如果辦公室位置為空白，將 Instructor.OfficeAssignment 屬性設定為 null，如此就會刪除 OfficeAssignment 資料表中相關的資料列。

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- 將變更儲存至資料庫。

### <a name="update-the-instructor-edit-view"></a>更新 Instructor 編輯檢視

在*Views/Instructors/Edit.cshtml*，加入新欄位來編輯辦公室位置，在結束之前**儲存**按鈕：

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

執行網頁 (選取**講師**索引標籤，然後按一下 **編輯**講師上)。 變更**辦公室位置**按一下**儲存**。

![講師編輯頁面](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>加入課程指派至講師編輯頁面

講師可能教導任意數目的課程。 現在您將會增強講師編輯網頁所加入的能力變更課程作業使用的群組核取方塊，下列螢幕擷取畫面所示：

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

「 課程 」 和 「 講師 」 實體之間的關聯性是多對多。 若要加入及移除關聯性，您可以加入和移除 CourseAssignments 聯結實體集的實體。

可讓您變更哪些課程 UI 講師指派給已核取方塊的群組。 顯示每個課程資料庫中的核取方塊，並選取講師目前指派給項目。 使用者可以選取或清除核取方塊，以變更課程作業。 如果課程數目大很多，您可能會想要使用其他方法將資料呈現在檢視中，但您要用於建立或刪除關聯性的處理聯結實體相同的方法。

### <a name="update-the-instructors-controller"></a>更新講師控制站

若要提供的核取方塊清單檢視資料，您將使用的檢視模型類別。

建立*AssignedCourseData.cs*中*SchoolViewModels*資料夾並取代現有的程式碼取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

在*InstructorsController.cs*，取代 HttpGet`Edit`方法取代下列程式碼。 所做的變更會反白顯示。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

加入的程式碼的積極式載入`Courses`導覽屬性，並呼叫新`PopulateAssignedCourseData`方法以提供的核取方塊陣列使用資訊`AssignedCourseData`檢視模型類別。

中的程式碼`PopulateAssignedCourseData`方法會讀取所有課程實體透過以載入課程使用的檢視模型類別的清單。 每個課程中，程式碼會檢查課程是否存在於講師`Courses`導覽屬性。 若要建立有效率查閱，檢查是否課程已指派給講師時，指派給講師的課程會放入`HashSet`集合。 `Assigned`屬性設定為 true 的課程講師指派給。 檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為選取。 最後，清單會傳遞至檢視中`ViewData`。

接下來，新增使用者時所執行的程式碼**儲存**。 取代`EditPost`方法，以下列程式碼，並增加新方法，以更新`Courses`[Instructor] 實體導覽屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

方法簽章現在是不同於 HttpGet`Edit`方法，以便從變更的方法名稱`EditPost`回到`Edit`。

由於檢視不會有課程實體的集合，所以無法自動更新模型繫結器`CourseAssignments`導覽屬性。 而不是使用模型繫結器，來更新`CourseAssignments`導覽屬性，在中的新執行`UpdateInstructorCourses`方法。 因此您必須排除`CourseAssignments`屬性從模型繫結。 這並不需要呼叫的程式碼的任何變更`TryUpdateModel`因為您正在使用的允許清單的多載和`CourseAssignments`不在 include 清單中。

如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`CourseAssignments`空集合，並傳回具有瀏覽屬性：

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

程式碼執行迴圈，在資料庫中的所有課程，然後檢查每個課程，針對目前已指派給講師對照檢視中所選取的項目。 為了有效率查閱，後者的兩個集合會儲存在`HashSet`物件。

如果課程核取方塊已選取，但過程中沒有`Instructor.CourseAssignments`導覽屬性，過程會加入至集合中的導覽屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

如果未選取課程核取方塊，但是課程在`Instructor.CourseAssignments`導覽屬性，課程移除導覽屬性。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>更新講師檢視

在*Views/Instructors/Edit.cshtml*，新增**課程**欄位與陣列中加入下列的核取方塊的程式碼後立即`div`項目**Office**欄位，以及之前`div`元素**儲存** 按鈕。

<a id="notepad"></a>
> [!NOTE] 
> 當您將程式碼貼在 Visual Studio 中時，插入換行符號將會破壞程式碼的方式。  按一次 Ctrl + Z 復原自動格式化。  這會修正換行，讓它們看起來像這裡所示。 縮排不一定要是完美，但是`@</tr><tr>`， `@:<td>`， `@:</td>`，和`@:</tr>`行必須是在單一行所示，否則將會執行階段錯誤。 選取新的程式碼區塊時，按下 Tab 鍵三次線與現有的程式碼的新程式碼。

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

此程式碼會建立具有三個資料行的 HTML 表格。 在每個資料行是核取方塊，後面的課程編號和標題所組成的標題。 所有核取方塊具有相同名稱 ("selectedCourses 」) 會通知它們會被視為一個群組的模型繫結器。 每個核取方塊的 value 屬性的值設定為值`CourseID`。 當網頁回傳時，模型繫結器會將陣列傳遞至所組成的控制站`CourseID`只核取方塊已選取的值。

當一開始會呈現核取方塊時，為指派給講師課程的已簽屬性，以選取它們 （顯示其核取）。

執行 Instructor 索引頁面上，然後按一下 **編輯**上以查看講師**編輯**頁面。

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

變更一些課程作業，並按一下 [儲存]。 所做的變更會反映在索引頁面。

> [!NOTE] 
> 沒有有限的數目的課程時，就會運作帶往這裡編輯講師課程資料的方法。 對於更大的集合，不同的 UI 和不同的更新方法將會是必要。

## <a name="update-the-delete-page"></a>更新刪除頁面

在*InstructorsController.cs*，刪除`DeleteConfirmed`方法並插入下列程式碼會在其位置。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

此程式碼進行下列變更：

* 載入未 eager`CourseAssignments`導覽屬性。  EF 不了解相關或您必須加入此`CourseAssignment`實體並不會刪除它們。  若要避免需要讀取他們這裡您可以設定串聯刪除資料庫中。

* 如果要刪除講師被指派任何部門的系統管理員身分，移除這些部門講師指派。

## <a name="add-office-location-and-courses-to-the-create-page"></a>將辦公室位置和課程加入至 [建立] 頁面

在*InstructorsController.cs*，刪除 HttpGet 和 HttpPost`Create`方法，然後在適當位置中加入下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

此程式碼是類似您為已看到`Edit`選取最初沒有課程除外的方法。 HttpGet`Create`方法呼叫`PopulateAssignedCourseData`方法不是因為可能有選取，但在課程，才能提供空的集合`foreach`（否則檢視程式碼會擲回 null 參考例外狀況） 的檢視中的迴圈。

HttpPost`Create`方法會加入至每個所選的課程`CourseAssignments`之前它會檢查是否有驗證錯誤，並在資料庫中加入新講師的導覽屬性。 即使有模型錯誤，以便於當模型錯誤 （例如，做為索引鍵的日期不正確的使用者），而頁面會重新顯示一則錯誤訊息，會自動還原所做的任何課程選取項目，就會加入課程。

請注意，為了能夠新增至課程`CourseAssignments`導覽屬性，您必須初始化為空集合屬性：

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

替代執行此動作控制器的程式碼中，您無法操作講師模型中變更屬性 getter，來自動建立集合如果它不存在，如下列範例所示：

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

如果您修改`CourseAssignments`屬性如此一來，您可以移除控制器中的明確的屬性初始化程式碼。

在*Views/Instructor/Create.cshtml*、 新增辦公室位置 文字方塊和核取方塊之前送出按鈕的課程。 如果是 編輯頁面上，[修正格式時將它貼入 Visual Studio 重新格式化程式碼如果](#notepad)。

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

藉由執行測試**建立**頁並加入講師。 

## <a name="handling-transactions"></a>處理交易

中所述[CRUD 教學課程](crud.md)，Entity Framework 會實作隱含交易。 如案例，您需要更大的控制權-例如，如果您想要包含在交易-外面 Entity Framework 執行的作業，請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。

## <a name="summary"></a>總結

您現在已完成處理的相關資料的簡介。 在下一個教學課程中，您會看到如何處理並行衝突。

>[!div class="step-by-step"]
[上一頁](read-related-data.md)
[下一頁](concurrency.md)  
