---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 更新相關資料 - 7/8
author: rick-anderson
description: 在本教學課程中，您會藉由更新外部索引鍵欄位和導覽屬性來更新相關資料。
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/update-related-data
ms.openlocfilehash: fdfdb14ff8414b8bf30f9b95be7ba0a6bcbd2995
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656419"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 更新相關資料 - 7/8

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

本教學課程會示範如何更新相關資料。 下圖顯示一些已完成的頁面。

![課程 [編輯] 頁面](update-related-data/_static/course-edit30.png)
![講師 [編輯] 頁面](update-related-data/_static/instructor-edit-courses30.png)

## <a name="update-the-course-create-and-edit-pages"></a>更新 Course Create 和 Edit 頁面

Course Create 和 Edit 頁面的 scaffold 程式碼包含一個 Department 下拉式清單，其顯示 Department 識別碼 (整數)。 下拉式清單應該會顯示 Department 名稱，因此這兩個頁面需要部門名稱的清單。 若要提供該清單，請使用 Create 和 Edit 頁面的基底類別。

### <a name="create-a-base-class-for-course-create-and-edit"></a>建立 Course Create 和 Edit 的基底類別

使用下列程式碼建立 *Pages/Courses/DepartmentNamePageModel.cs* 檔案：

[!code-csharp[](intro/samples/cu30/Pages/Courses/DepartmentNamePageModel.cs)]

上述程式碼會建立 [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含部門名稱的清單。 如果指定了 `selectedDepartment`，就會在 `SelectList` 中選取該部門。

*Create* 和 *Edit* 頁面模型類別將衍生自 `DepartmentNamePageModel`。

### <a name="update-the-course-create-page-model"></a>更新 Course Create 頁面模型

Course 會指派給 Department。 Create 和 Edit 頁面的基底類別會提供 `SelectList` 以用來選取部門。 使用 `SelectList` 的下拉式清單會設定 `Course.DepartmentID` 外部索引鍵 (FK) 屬性。 EF Core 則使用 `Course.DepartmentID` FK 來載入 `Department` 導覽屬性。

![建立課程](update-related-data/_static/ddl30.png)

使用下列程式碼更新 *Pages/Courses/Create.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Courses/Create.cshtml.cs?highlight=7,18,27-41)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

上述程式碼：

* 衍生自 `DepartmentNamePageModel`。
* 使用 `TryUpdateModelAsync` 來防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
* 移除 `ViewData["DepartmentID"]`。 來自基底類別的 `DepartmentNameSL` 是一種強型別模型，並將由 Razor 頁面使用。 強型別的模型優先於弱型別。 如需詳細資訊，請參閱[弱型別資料 (ViewData 和 ViewBag)](xref:mvc/views/overview#VD_VB)。

### <a name="update-the-course-create-razor-page"></a>更新 Course Create Razor 頁面

使用下列程式碼更新 *Pages/Courses/Create.cshtml*：

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Create.cshtml?highlight=29-34)]

上述程式碼會進行下列變更：

* 將標題從 **DepartmentID** 變更為 **Department**。
* 以 `"ViewBag.DepartmentID"` (來自基底類別) 取代 `DepartmentNameSL`。
* 新增 [選取部門] 選項。 此變更會在尚未選取任何部門時，在下拉式清單中轉譯「選取部門」而非第一個部門。
* 未選取部門時，請新增驗證訊息。

Razor 頁面使用[選取標籤協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)：

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

測試 *Create* 頁面。 *Create* 頁面會顯示部門名稱，而不是部門識別碼。

### <a name="update-the-course-edit-page-model"></a>更新 Course Edit 頁面模型

使用下列程式碼更新 *Pages/Courses/Edit.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40-66)]

這些變更類似於 *Create* 頁面模型中所做的變更。 在上述程式碼中，`PopulateDepartmentsDropDownList` 會傳遞部門識別碼，其在下拉式清單中選取部門。

### <a name="update-the-course-edit-razor-page"></a>更新 Course Edit Razor 頁面

使用下列程式碼更新 *Pages/Courses/Edit.cshtml*：

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上述程式碼會進行下列變更：

* 顯示課程識別碼。 通常不會顯示實體的主索引鍵 (PK)。 PK 對使用者來說通常是沒有意義的。 在此情況下，PK 是課程編號。
* 將 Department 下拉式清單的標題從 **DepartmentID** 變更為 **Department**。
* 以 `"ViewBag.DepartmentID"` (來自基底類別) 取代 `DepartmentNameSL`。

此頁面包含課程編號的隱藏欄位 (`<input type="hidden">`)。 新增 `<label>` 標籤協助程式與 `asp-for="Course.CourseID"` 無法免除隱藏欄位的需求。 當使用者按一下 [儲存] `<input type="hidden">`**時，需要有** 才能將課程編號包含在張貼資料中。

## <a name="update-the-course-details-and-delete-pages"></a>更新 Course Detail 和 Delete 頁面

不需要追蹤時，[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以改善效能。

### <a name="update-the-course-page-models"></a>更新 Course 頁面模型

使用下列程式碼更新 *Pages/Courses/Delete.cshtml.cs* 來新增 `AsNoTracking`：

[!code-csharp[](intro/samples/cu30/Pages/Courses/Delete.cshtml.cs?highlight=29)]

在 *Pages/Courses/Details.cshtml.cs* 檔案中進行相同的變更：

[!code-csharp[](intro/samples/cu30/Pages/Courses/Details.cshtml.cs?highlight=28)]

### <a name="update-the-course-razor-pages"></a>更新 Course Razor 頁面

使用下列程式碼更新 *Pages/Courses/Delete.cshtml*：

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Delete.cshtml?highlight=15-20,37)]

對 *Details* 頁面進行相同的變更。

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Details.cshtml?highlight=14-19,36)]

## <a name="test-the-course-pages"></a>測試 Course 頁面

測試建立、編輯、詳細資料和刪除頁面。

## <a name="update-the-instructor-create-and-edit-pages"></a>更新講師 Create 和 Edit 頁面

講師可教授任何數量的課程。 下圖顯示講師的 Edit 頁面，其中包含一個課程核取方塊的陣列。

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses30.png)

核取方塊可讓您變更指派給講師的課程。 資料庫中的每個課程都會顯示一個核取方塊。 指派給講師的課程會處於選取狀態。 使用者可以選取或清除核取方塊來變更課程指派。 若課程數要多上許多，則使用不同 UI 的效能可能會更好。 但此處所顯示管理多對多關聯性的方法不會變更。 若要建立或刪除關聯性，您可以操作聯結實體。

### <a name="create-a-class-for-assigned-courses-data"></a>建立受指派課程資料的類別

以下列程式碼建立 *SchoolViewModels/AssignedCourseData.cs*：

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` 類別包含資料，用來建立指派給講師課程的核取方塊。

### <a name="create-an-instructor-page-model-base-class"></a>建立 Instructor 頁面模型基底類別

建立 *Pages/Instructors/InstructorCoursesPageModel.cs* 基底類別：

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_All)]

`InstructorCoursesPageModel` 是您將用於 *Edit* 和 *Create* 頁面模型的基底類別。 `PopulateAssignedCourseData` 會讀取所有 `Course` 實體來擴展 `AssignedCourseDataList`。 對於每個課程，此程式碼設定 `CourseID`、標題以及是否將講師指派給課程。 [HashSet](/dotnet/api/system.collections.generic.hashset-1) 會用來進行有效率的查閱。

由於 Razor 頁面沒有 Course 實體的集合，因此模型繫結器無法自動更新 `CourseAssignments` 導覽屬性。 相較於使用模型繫結器更新 `CourseAssignments` 導覽屬性，您會在新的 `UpdateInstructorCourses` 方法中進行相同的操作。 因此您必須從模型繫結器中排除 `CourseAssignments` 屬性。 這並不需要對呼叫 `TryUpdateModel` 的程式碼進行任何變更，因為您使用的是允許清單多載，且 `CourseAssignments` 並未位於包含清單中。

若沒有選取任何核取方塊，`UpdateInstructorCourses` 中的程式碼會使用空集合初始化 `CourseAssignments` 導覽屬性並傳回：

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_IfNull)]

然後程式碼會以迴圈逐一巡覽資料庫中的所有課程，並檢查每個已指派給講師的課程，以及在頁面中選取的課程。 為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。

若課程的核取方塊已被選取，但課程並未位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會新增至導覽屬性的集合中。

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCourses)]

若課程的核取方塊未被選取，但課程卻位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會從導覽屬性的集合中移除。

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCoursesElse)]

### <a name="handle-office-location"></a>處理辦公室位置

另一個編輯頁面必須處理的關聯性是 Instructor 實體針對 `OfficeAssignment` 實體所具有一對零或一對一關聯性。 講師編輯程式碼必須處理下列案例： 

* 如果使用者清除辦公室指派，請刪除 `OfficeAssignment` 實體。
* 如果使用者輸入辦公室指派，但它是空的，請建立新的 `OfficeAssignment` 實體。
* 如果使用者變更辦公室指派，請更新 `OfficeAssignment` 實體。

### <a name="update-the-instructor-edit-page-model"></a>更新 Instructor Edit 頁面模型

使用下列程式碼更新 *Pages/Instructors/Edit.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Edit.cshtml.cs?name=snippet_All&highlight=9,28-32,38,42-77)]

上述程式碼：

* 使用 `Instructor`、`OfficeAssignment` 和 `CourseAssignment` 導覽屬性的積極式載入，從資料庫取得目前的 `CourseAssignment.Course` 實體。
* 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 `TryUpdateModel` 會防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
* 如果辦公室位置為空白，請將 `Instructor.OfficeAssignment` 設定為 Null。 當 `Instructor.OfficeAssignment` 為 Null 時，將會刪除 `OfficeAssignment` 資料表中的相關資料列。
* 在 `PopulateAssignedCourseData` 中呼叫 `OnGetAsync` 來使用 `AssignedCourseData` 檢視模型類別，以提供資訊給核取方塊。
* 在 `UpdateInstructorCourses` 中呼叫 `OnPostAsync` 來將核取方塊的資訊套用到正在編輯的 Instructor 實體。
* 若 `PopulateAssignedCourseData` 失敗，則在 `UpdateInstructorCourses` 中呼叫 `OnPostAsync` 和 `TryUpdateModel`。 這些方法呼叫會在重新顯示並附帶錯誤訊息時，還原在頁面上輸入的已指派課程資料。

### <a name="update-the-instructor-edit-razor-page"></a>更新 Instructor Edit Razor 頁面

使用下列程式碼更新 *Pages/Instructors/Edit.cshtml*：

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Edit.cshtml?highlight=29-59)]

上述程式碼會建立一個 HTML 資料表，該資料表中有三個資料行。 每個資料行都有一個核取方塊和包含課程號碼及標題的標題。 核取方塊全部都具有相同的名稱 ("selectedCourses")。 使用相同的名稱可告知模型繫結器將它們視為一個群組。 每個核取方塊的 Value 屬性都會設為 `CourseID`。 張貼頁面時，模型繫結器會傳遞陣列，該陣列當中只包含已選取核取方塊的 `CourseID` 值。

一開始轉譯核取方塊時，指派給講師的課程會呈現選取狀態。

注意：這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。 針對更大的集合，不同的 UI 和不同的更新方法可能更有用且更有效率。

執行應用程式並測試更新後的 Instructors Edit 頁面。 變更一些課程指派。 所做的變更會反映在 [索引] 頁面上。

### <a name="update-the-instructor-create-page"></a>更新 Instructor Create 頁面

使用與 Edit 頁面相似的程式碼來更新 Instructor Create 頁面模型和 Razor 頁面：

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Create.cshtml.cs)]

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Create.cshtml)]

測試講師 *Create* 頁面。

## <a name="update-the-instructor-delete-page"></a>更新 Instructor Delete 頁面

使用下列程式碼更新 *Pages/Instructors/Delete.cshtml.cs*：

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Delete.cshtml.cs?highlight=45-61)]

上述程式碼會進行下列變更：

* 為 `CourseAssignments` 導覽屬性使用積極式載入。 必須包含 `CourseAssignments`，否則刪除講師時不會刪除它們。 若要避免需要讀取們，您可以在資料庫中設定串聯刪除。

* 若要刪除的講師已指派為任何部門的系統管理員，請先從部門中移除講師的指派。

執行應用程式及測試 Delete 頁面。

## <a name="next-steps"></a>後續步驟

> [!div class="step-by-step"]
> [上一個教學課程](xref:data/ef-rp/read-related-data)
> [下一個教學課程](xref:data/ef-rp/concurrency)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本教學課程將示範如何更新相關資料。 若您遇到無法解決的問題，請[下載或檢視完整應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。 [下載指示](xref:index#how-to-download-a-sample)。

下圖顯示一些完成的頁面。

![課程 [編輯] 頁面](update-related-data/_static/course-edit.png)
![講師 [編輯] 頁面](update-related-data/_static/instructor-edit-courses.png)

檢查並測試 *Create* 與 *Edit* 課程頁面。 建立新的課程。 部門是依照其主索引鍵 (整數) 來進行選取，而不是它的名稱。 編輯新的課程。 當您完成測試時，請刪除新的課程。

## <a name="create-a-base-class-to-share-common-code"></a>建立要共用通用程式碼的基底類別

`Courses/Create` 和 `Courses/Edit` 頁面每個都需要部門名稱的清單。 請針對 *Create* 和 *Edit* 頁面建立 *Pages/Courses/DepartmentNamePageModel.cshtml.cs* 基底類別：

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

上述程式碼會建立 [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含部門名稱的清單。 如果指定了 `selectedDepartment`，就會在 `SelectList` 中選取該部門。

*Create* 和 *Edit* 頁面模型類別將衍生自 `DepartmentNamePageModel`。

## <a name="customize-the-courses-pages"></a>自訂 [課程] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 為了在建立課程新增部門，*Create* 和 *Edit* 的基底類別包含用來選取部門的下拉式清單。 下拉式清單會設定 `Course.DepartmentID` 外部索引鍵 (FK) 屬性。 EF Core 則使用 `Course.DepartmentID` FK 來載入 `Department` 導覽屬性。

![建立課程](update-related-data/_static/ddl.png)

以下列程式碼更新 [建立] 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

上述程式碼：

* 衍生自 `DepartmentNamePageModel`。
* 使用 `TryUpdateModelAsync` 來防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
* 以 `ViewData["DepartmentID"]` (來自基底類別) 取代 `DepartmentNameSL`。

`ViewData["DepartmentID"]` 已取代為強型別的 `DepartmentNameSL`。 強型別的模型優先於弱型別。 如需詳細資訊，請參閱[弱型別資料 (ViewData 和 ViewBag)](xref:mvc/views/overview#VD_VB)。

### <a name="update-the-courses-create-page"></a>更新 Courses 的 *Create* 頁面

使用下列程式碼更新 *Pages/Courses/Create.cshtml*：

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

上述標記會進行下列變更：

* 將標題從 **DepartmentID** 變更為 **Department**。
* 以 `"ViewBag.DepartmentID"` (來自基底類別) 取代 `DepartmentNameSL`。
* 新增 [選取部門] 選項。 這項變更會呈現 [選取部門] ，而不是第一個部門。
* 未選取部門時，請新增驗證訊息。

Razor 頁面使用[選取標籤協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)：

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

測試 *Create* 頁面。 *Create* 頁面會顯示部門名稱，而不是部門識別碼。

### <a name="update-the-courses-edit-page"></a>更新 Courses 的 *Edit* 頁面。

使用下列程式碼取代 *Pages/Courses/Edit.cshtml.cs* 中的程式碼：

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

這些變更類似於 *Create* 頁面模型中所做的變更。 在上述程式碼中，`PopulateDepartmentsDropDownList` 會傳入部門識別碼，以選取下拉式清單中指定的部門。

以下列標記更新 *Pages/Courses/Edit.cshtml*：

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上述標記會進行下列變更：

* 顯示課程識別碼。 通常不會顯示實體的主索引鍵 (PK)。 PK 對使用者來說通常是沒有意義的。 在此情況下，PK 是課程編號。
* 將標題從 **DepartmentID** 變更為 **Department**。
* 以 `"ViewBag.DepartmentID"` (來自基底類別) 取代 `DepartmentNameSL`。

此頁面包含課程編號的隱藏欄位 (`<input type="hidden">`)。 新增 `<label>` 標籤協助程式與 `asp-for="Course.CourseID"` 無法免除隱藏欄位的需求。 當使用者按一下 [儲存] `<input type="hidden">`**時，需要有** 才能將課程編號包含在張貼資料中。

測試更新過的程式碼。 建立、編輯和刪除課程。

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>將 AsNoTracking 新增至 *Details* 和 *Delete* 頁面模型

不需要追蹤時，[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以改善效能。 將 `AsNoTracking` 新增至 *Delete* 和 *Details* 頁面模型。 下列程式碼顯示已更新的 *Delete* 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

在 `OnGetAsync`Pages/Courses/Details.cshtml.cs*檔案中更新* 方法：

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>修改 *Delete* 和 *Details* 頁面

以下列標記更新 [刪除 Razor] 頁面：

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

對 *Details* 頁面進行相同的變更。

### <a name="test-the-course-pages"></a>測試 Course 頁面

測試建立、編輯、詳細資料和刪除。

## <a name="update-the-instructor-pages"></a>更新講師頁面

下列各節會更新講師頁面。

### <a name="add-office-location"></a>新增辦公室位置

當您編輯講師記錄時，可能會想要更新講師的辦公室指派。 `Instructor` 實體與 `OfficeAssignment` 實體具有一對零或一的關聯性。 必須處理講師程式碼：

* 如果使用者清除辦公室指派，請刪除 `OfficeAssignment` 實體。
* 如果使用者輸入辦公室指派，但它是空的，請建立新的 `OfficeAssignment` 實體。
* 如果使用者變更辦公室指派，請更新 `OfficeAssignment` 實體。

以下列程式碼更新講師 [編輯] 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

上述程式碼：

* 針對 `Instructor` 導覽屬性使用積極式載入從資料庫中取得目前的 `OfficeAssignment` 實體。
* 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 `TryUpdateModel` 會防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
* 如果辦公室位置為空白，請將 `Instructor.OfficeAssignment` 設定為 Null。 當 `Instructor.OfficeAssignment` 為 Null 時，將會刪除 `OfficeAssignment` 資料表中的相關資料列。

### <a name="update-the-instructor-edit-page"></a>更新講師 [編輯] 頁面

使用辦公室位置更新 *Pages/Instructors/Edit.cshtml*：

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

請確認您可以變更講師辦公室位置。

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>將課程指派新增至講師 [編輯] 頁面

講師可教授任何數量的課程。 在本節中，您可以新增變更課程指派的能力。 下圖顯示已更新的講師 [編輯] 頁面：

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses.png)

`Course` 和 `Instructor` 具有多對多關聯性。 若要新增和移除關聯性，您必須在 `CourseAssignments` 聯結實體集中新增和移除實體。

核取方塊可變更指派給講師的課程。 資料庫中的每個課程各顯示一個核取方塊。 已核取指派給講師的課程。 使用者可選取或清除核取方塊來變更課程指派。 如果課程數目大上許多：

* 您可能會使用不同的使用者介面來顯示課程。
* 操作聯結實體來建立或刪除關聯性的方法不會變更。

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>新增類別來支援 *Create* 和 *Edit* 講師頁面

以下列程式碼建立 *SchoolViewModels/AssignedCourseData.cs*：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` 類別包含資料，用來建立講師所指派課程的核取方塊。

建立 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* 基底類別：

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` 是您將用於 *Edit* 和 *Create* 頁面模型的基底類別。 `PopulateAssignedCourseData` 會讀取所有 `Course` 實體來擴展 `AssignedCourseDataList`。 對於每個課程，此程式碼設定 `CourseID`、標題以及是否將講師指派給課程。 [HashSet](/dotnet/api/system.collections.generic.hashset-1) 用來建立有效查閱。

### <a name="instructors-edit-page-model"></a>講師 *Edit* 頁面模型

以下列程式碼更新講師 [編輯] 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

上述程式碼會處理辦公室指派變更。

更新講師 Razor 檢視：

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> 當您將程式碼貼上到 Visual Studio 時，分行符號可能會產生變更，致使程式碼失效。 按 Ctrl+Z 來復原自動格式化。 Ctrl+Z 會修正分行符號，使它們看起來就跟您在這裡看到的一樣。 縮排不一定要是完美的，但 `@:</tr><tr>`、`@:<td>`、`@:</td>` 和 `@:</tr>` 必須要如顯示般各自在獨立的一行上。 當選取新的程式碼區塊時，按 Tab 鍵三次來讓新的程式碼對準現有的程式碼。 [使用此連結](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)票選或檢閱這個錯誤的狀態。

上述程式碼會建立一個 HTML 資料表，該資料表中有三個資料行。 每個資料行都有一個核取方塊，以及內含課程編號和標題 (title) 的標題 (caption)。 所有核取方塊都具有相同的名稱 ("selectedCourses")。 使用相同的名稱可告知模型繫結器將它們視為一個群組。 每個核取方塊的 Value 屬性都會設定為 `CourseID`。 當頁面發佈時，模型繫結器便會傳遞只包含所選取核取方塊之 `CourseID` 值的陣列。

一開始呈現核取方塊時，指派給講師的課程已核取屬性。

執行應用程式，並測試已更新的講師 [編輯] 頁面。 變更一些課程指派。 所做的變更會反映在 [索引] 頁面上。

注意：這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。 針對更大的集合，不同的 UI 和不同的更新方法可能更有用且更有效率。

### <a name="update-the-instructors-create-page"></a>更新講師 *Create* 頁面

以下列程式碼更新講師 *Create* 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

上述程式碼類似於 *Pages/Instructors/Edit.cshtml.cs* 程式碼。

以下列標記更新講師 [建立 Razor] 頁面：

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

測試講師 *Create* 頁面。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

以下列程式碼更新 [刪除] 頁面模型：

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

上述程式碼會進行下列變更：

* 為 `CourseAssignments` 導覽屬性使用積極式載入。 必須包含 `CourseAssignments`，否則刪除講師時不會刪除它們。 若要避免需要讀取們，您可以在資料庫中設定串聯刪除。

* 若要刪除的講師已指派為任何部門的系統管理員，請先從部門中移除講師的指派。

## <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本 (第 1 部分)](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [這個教學課程的 YouTube 版本 (第 2 部分)](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/read-related-data)
> [下一頁](xref:data/ef-rp/concurrency)

::: moniker-end
