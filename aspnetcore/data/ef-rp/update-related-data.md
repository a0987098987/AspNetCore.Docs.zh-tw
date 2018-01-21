---
title: "使用 EF Core-razor 頁面更新相關的資料-7，8 個"
author: rick-anderson
description: "在本教學課程中，您要更新相關的資料藉由更新外部索引鍵欄位，而且導覽屬性。"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>更新相關的資料-EF 核心 Razor 頁面 (8 個 7)

由[Tom Dykstra](https://github.com/tdykstra)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教學課程示範如何更新相關的資料。 如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)。

下圖顯示的部分完成的頁面。

![課程編輯頁面](update-related-data/_static/course-edit.png)
![講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

檢查並測試建立與編輯課程頁面。 建立新的課程。 部門會選取其主索引鍵 （整數），不是它的名稱。 編輯新的課程。 當您完成測試時，刪除新的課程。

## <a name="create-a-base-class-to-share-common-code"></a>建立共用通用程式碼基底類別

課程/建立和課程/編輯頁面每個需要部門名稱的清單。 建立*Pages/Courses/DepartmentNamePageModel.cshtml.cs*基底類別建立與編輯頁面：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

上述程式碼會建立[SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0)包含部門名稱的清單。 如果`selectedDepartment`指定，該部門中選取`SelectList`。

建立與編輯頁面模型類別會衍生自`DepartmentNamePageModel`。

## <a name="customize-the-courses-pages"></a>自訂課程頁面

建立新的課程實體時，它必須有現有的部門關聯性。 若要加入的部門建立課程時，建立與編輯的基底類別包含的下拉式清單選取部門。 下拉式清單設定`Course.DepartmentID`外部索引鍵 (FK) 屬性。 使用 EF 核心`Course.DepartmentID`載入 FK`Department`導覽屬性。

![建立課程](update-related-data/_static/ddl.png)

更新下列程式碼建立頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

上述程式碼：

* 衍生自 `DepartmentNamePageModel`。
* 使用`TryUpdateModelAsync`防止[overposting](xref:data/ef-rp/crud#overposting)。
* 取代`ViewData["DepartmentID"]`與`DepartmentNameSL`（來自基底類別中）。

`ViewData["DepartmentID"]`已取代的強型別`DepartmentNameSL`。 強型別的的模型被優於弱型別。 如需詳細資訊，請參閱[弱型別資料 （別的 ViewData 和 ViewBag）](xref:mvc/views/overview#VD_VB)。

### <a name="update-the-courses-create-page"></a>更新課程建立的頁面

更新*Pages/Courses/Create.cshtml*以下列標記：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

上述標記進行下列變更：

* 變更從標題**DepartmentID**至**部門**。
* 取代`"ViewBag.DepartmentID"`與`DepartmentNameSL`（來自基底類別中）。
* 新增 「 選取部門 」 選項。 這項變更會呈現 「 選取部門 」，而不是第一個部門。
* 未選取部門時，請新增驗證訊息。

使用 Razor 頁面[選取標記協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

測試 [建立] 頁面。 [建立] 頁面會顯示部門名稱，而不是部門識別碼。

### <a name="update-the-courses-edit-page"></a>更新課程編輯頁面。

更新下列程式碼編輯頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

就類似於建立頁面模型中所做的變更。 在上述程式碼，`PopulateDepartmentsDropDownList`部門識別碼中，選取下拉式清單中指定的部門的傳遞。

更新*Pages/Courses/Edit.cshtml*以下列標記：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上述標記進行下列變更：

* 顯示課程識別碼。 通常不會顯示在主索引鍵 (PK) 的實體。 PKs 是通常無意義的使用者。 在此情況下，在 PK 是課程數目。
* 變更從標題**DepartmentID**至**部門**。
* 取代`"ViewBag.DepartmentID"`與`DepartmentNameSL`（來自基底類別中）。
* 新增 「 選取部門 」 選項。 這項變更會呈現 「 選取部門 」，而不是第一個部門。
* 未選取部門時，請新增驗證訊息。

此頁面包含隱藏的欄位 (`<input type="hidden">`) 課程編號。 加入`<label>`標記協助程式與`asp-for="Course.CourseID"`不免除為隱藏欄位。 `<input type="hidden">`所要包含在張貼的資料，當使用者按一下的課程編號的**儲存**。

測試更新的程式碼。 建立、 編輯和刪除的課程。

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>AsNoTracking 加入詳細資料，並刪除頁面模型

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__)不需要追蹤時，可以改善效能。 新增`AsNoTracking`刪除和詳細資料頁面模型。 下列程式碼會顯示更新的 Delete 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

更新`OnGetAsync`方法中的*Pages/Courses/Details.cshtml.cs*檔案：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>修改 刪除 和 詳細資料頁面

更新刪除 Razor 頁面以下列標記：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

詳細資料頁面中進行相同變更。

### <a name="test-the-course-pages"></a>測試課程頁面

測試建立、 編輯，詳細資料，以及刪除。

## <a name="update-the-instructor-pages"></a>更新講師頁

下列各節會更新 [instructor] 頁面。

### <a name="add-office-location"></a>新增辦公室位置

當編輯講師記錄時，您可能想要更新的講師 office 指派。 `Instructor`實體具有以零或-1 個關係`OfficeAssignment`實體。 必須處理講師程式碼：

* 如果使用者清除 office 指派，刪除`OfficeAssignment`實體。
* 如果使用者輸入 office 指派，而它是空白，建立新`OfficeAssignment`實體。
* 如果使用者變更 office 指派，更新`OfficeAssignment`實體。

下列程式碼來更新講師編輯頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

上述程式碼：

- 取得目前`Instructor`實體使用的積極式載入從資料庫`OfficeAssignment`導覽屬性。
- 更新擷取`Instructor`實體模型繫結器的值。 `TryUpdateModel`防止[overposting](xref:data/ef-rp/crud#overposting)。
- 如果將辦公室位置為空白，`Instructor.OfficeAssignment`為 null。 當`Instructor.OfficeAssignment`是 null，相關的資料列中`OfficeAssignment`刪除資料表。

### <a name="update-the-instructor-edit-page"></a>更新講師編輯頁面

更新*Pages/Instructors/Edit.cshtml*辦公室位置：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

請確認您可以變更講師辦公室位置。

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>課程作業加入講師編輯頁面

講師可能教導任意數目的課程。 在本節中，您可以將變更課程作業的能力。 下圖顯示更新的講師編輯頁面：

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

`Course`和`Instructor`具有多對多關聯性。 若要加入及移除關聯性，新增和移除從實體`CourseAssignments`加入實體集。

核取方塊會啟用的課程講師指派給變更。 核取方塊會顯示資料庫中的每個課程。 會檢查講師指派給的課程。 使用者可以選取或清除核取方塊，以變更課程作業。 如果更大的課程數目：

* 您可能會使用不同的使用者介面顯示課程。
* 操作來建立或刪除關聯性聯結實體的方法就不會變更。

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>新增類別，來支援建立和編輯講師頁面

建立*SchoolViewModels/AssignedCourseData.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData`類別包含資料，以建立講師所指派的課程核取方塊。

建立*Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*基底類別：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel`的基底類別，您將使用 [編輯]，並建立頁面模型。 `PopulateAssignedCourseData`讀取所有`Course`實體來擴展`AssignedCourseDataList`。 每個課程中，此程式碼設定`CourseID`，標題和講師指派給課程是否。 A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1)用來建立有效查閱。

### <a name="instructors-edit-page-model"></a>講師編輯頁面模型

下列程式碼來更新講師編輯頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

上述程式碼會處理 office 指派變更。

更新講師 Razor 檢視：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> 當您將 Visual Studio 中的程式碼時，分行符號會變更會破壞程式碼的方式。 按一次 Ctrl + Z 復原自動格式化。 Ctrl + Z 會修正換行，讓它們看起來像這裡所示。 縮排不一定要是完美，但是`@</tr><tr>`， `@:<td>`， `@:</td>`，和`@:</tr>`行必須是在單一行所示。 選取新的程式碼區塊時，按下 Tab 鍵三次線與現有的程式碼的新程式碼。 票選或檢閱這個 bug 狀態[與此連結](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)。

上述程式碼會建立具有三個資料行的 HTML 表格。 每個資料行的核取方塊以及內含的課程編號和標題的標題。 所有核取方塊具有相同的名稱 ("selectedCourses")。 使用相同的名稱會告知模型繫結器視為一個群組。 每個核取方塊的 value 屬性的值設定為`CourseID`。 模型繫結器頁面張貼時，將所組成的陣列傳遞`CourseID`值只會選取核取方塊。

一開始會呈現核取方塊，指派給講師課程已屬性。

執行應用程式，並測試更新的講師編輯頁面。 變更一些課程作業。 變更會反映在索引頁面。

注意： 您帶往這裡編輯講師課程資料的方法就可以使用也有限的數目的課程。 對於更大的集合，不同的 UI 和不同的更新方法將會是能更有效率。

### <a name="update-the-instructors-create-page"></a>更新講師建立頁面

下列程式碼來更新講師建立頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

上述程式碼很類似*Pages/Instructors/Edit.cshtml.cs*程式碼。

更新講師建立 Razor 頁面以下列標記：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

測試講師建立頁面。

## <a name="update-the-delete-page"></a>更新刪除頁面

下列程式碼更新刪除頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

上述程式碼進行下列變更：

* 會使用積極式載入的`CourseAssignments`導覽屬性。 `CourseAssignments`必須加入或刪除講師時不刪除它們。 若要避免需要讀取它們，請在資料庫中設定串聯刪除。

* 如果要刪除講師被指派任何部門的系統管理員身分，移除這些部門講師指派。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/read-related-data)
[下一頁](xref:data/ef-rp/concurrency)
