---
title: "Razor 頁面與 EF Core - 更新相關資料 - 7/8"
author: rick-anderson
description: "在本教學課程中，您會藉由更新外部索引鍵欄位和導覽屬性來更新相關資料。"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 5c91c91ab938f3aa4abc55049c54f399469f6163
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>更新相關資料 - EF Core 與 Razor 頁面 (7/8)

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

本教學課程將示範如何更新相關資料。 若您遭遇到無法解決的問題，請下載[此階段的完整應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)。

下圖顯示一些完成的頁面。

![課程 [編輯] 頁面](update-related-data/_static/course-edit.png)
![講師 [編輯] 頁面](update-related-data/_static/instructor-edit-courses.png)

檢查並測試 [建立] 與 [編輯] 課程頁面。 建立新的課程。 部門是依照其主索引鍵 (整數) 來進行選取，而不是它的名稱。 編輯新的課程。 當您完成測試時，請刪除新的課程。

## <a name="create-a-base-class-to-share-common-code"></a>建立要共用通用程式碼的基底類別

[課程]/[建立] 和 [課程]/[編輯] 頁面每個都需要部門名稱的清單。 請針對 [建立] 和 [編輯] 頁面建立 *Pages/Courses/DepartmentNamePageModel.cshtml.cs* 基底類別：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

上述程式碼會建立 [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含部門名稱的清單。 如果指定了 `selectedDepartment`，就會在 `SelectList` 中選取該部門。

[建立] 和 [編輯] 頁面模型類別將衍生自 `DepartmentNamePageModel`。

## <a name="customize-the-courses-pages"></a>自訂 [課程] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 為了在建立課程新增部門，[建立] 和 [編輯] 的基底類別包含用來選取部門的下拉式清單。 下拉式清單會設定 `Course.DepartmentID` 外部索引鍵 (FK) 屬性。 EF Core 則使用 `Course.DepartmentID` FK 來載入 `Department` 導覽屬性。

![建立課程](update-related-data/_static/ddl.png)

以下列程式碼更新 [建立] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

上述程式碼：

* 衍生自 `DepartmentNamePageModel`。
* 使用 `TryUpdateModelAsync` 來防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
* 以 `DepartmentNameSL` (來自基底類別) 取代 `ViewData["DepartmentID"]`。

`ViewData["DepartmentID"]` 已取代為強型別的 `DepartmentNameSL`。 強型別的模型優先於弱型別。 如需詳細資訊，請參閱[弱型別資料 (ViewData 和 ViewBag)](xref:mvc/views/overview#VD_VB)。

### <a name="update-the-courses-create-page"></a>更新 Courses 的 [建立] 頁面

以下列標記更新 *Pages/Courses/Create.cshtml*：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

上述標記會進行下列變更：

* 將標題從 **DepartmentID** 變更為 **Department**。
* 以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。
* 新增 [選取部門] 選項。 這項變更會呈現 [選取部門] ，而不是第一個部門。
* 未選取部門時，請新增驗證訊息。

Razor 頁面使用[選取標籤協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

測試 [建立] 頁面。 [建立] 頁面會顯示部門名稱，而不是部門識別碼。

### <a name="update-the-courses-edit-page"></a>更新 Courses 的 [編輯] 頁面。

以下列程式碼更新 [編輯] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

這些變更類似於 [建立] 頁面模型中所做的變更。 在上述程式碼中，`PopulateDepartmentsDropDownList` 會傳入部門識別碼，以選取下拉式清單中指定的部門。

以下列標記更新 *Pages/Courses/Edit.cshtml*：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上述標記會進行下列變更：

* 顯示課程識別碼。 通常不會顯示實體的主索引鍵 (PK)。 PK 對使用者來說通常是沒有意義的。 在此情況下，PK 是課程編號。
* 將標題從 **DepartmentID** 變更為 **Department**。
* 以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。
* 新增 [選取部門] 選項。 這項變更會呈現 [選取部門] ，而不是第一個部門。
* 未選取部門時，請新增驗證訊息。

此頁面包含課程編號的隱藏欄位 (`<input type="hidden">`)。 新增 `<label>` 標籤協助程式與 `asp-for="Course.CourseID"` 無法免除隱藏欄位的需求。 當使用者按一下 [儲存]  時，需要有 `<input type="hidden">` 才能將課程編號包含在張貼資料中。

測試更新過的程式碼。 建立、編輯和刪除課程。

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>將 AsNoTracking 新增至 [詳細資料] 和 [刪除] 頁面模型

不需要追蹤時，[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以改善效能。 將 `AsNoTracking` 新增至 [刪除] 和 [詳細資料] 頁面模型。 下列程式碼顯示已更新的 [刪除] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

在 *Pages/Courses/Details.cshtml.cs* 檔案中更新 `OnGetAsync` 方法：

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>修改 [刪除] 和 [詳細資料] 頁面

以下列標記更新 [刪除 Razor] 頁面：

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

對 [詳細資料] 頁面進行相同的變更。

### <a name="test-the-course-pages"></a>測試 [課程] 頁面

測試建立、編輯、詳細資料和刪除。

## <a name="update-the-instructor-pages"></a>更新講師頁面

下列各節會更新講師頁面。

### <a name="add-office-location"></a>新增辦公室位置

當您編輯講師記錄時，可能會想要更新講師的辦公室指派。 `Instructor` 實體與 `OfficeAssignment` 實體具有一對零或一的關聯性。 必須處理講師程式碼：

* 如果使用者清除辦公室指派，請刪除 `OfficeAssignment` 實體。
* 如果使用者輸入辦公室指派，但它是空的，請建立新的 `OfficeAssignment` 實體。
* 如果使用者變更辦公室指派，請更新 `OfficeAssignment` 實體。

以下列程式碼更新講師 [編輯] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

上述程式碼：

- 針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。
- 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 `TryUpdateModel` 會防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。
- 如果辦公室位置為空白，請將 `Instructor.OfficeAssignment` 設定為 Null。 當 `Instructor.OfficeAssignment` 為 Null 時，將會刪除 `OfficeAssignment` 資料表中的相關資料列。

### <a name="update-the-instructor-edit-page"></a>更新講師 [編輯] 頁面

使用辦公室位置更新 *Pages/Instructors/Edit.cshtml*：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

請確認您可以變更講師辦公室位置。

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>將課程指派新增至講師 [編輯] 頁面

講師可教授任何數量的課程。 在本節中，您可以新增變更課程指派的能力。 下圖顯示已更新的講師 [編輯] 頁面：

![講師 [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses.png)

`Course` 和 `Instructor` 具有多對多關聯性。 若要新增和移除關聯性，您必須在 `CourseAssignments` 聯結實體集中新增和移除實體。

核取方塊可變更指派給講師的課程。 資料庫中的每個課程各顯示一個核取方塊。 已核取指派給講師的課程。 使用者可選取或清除核取方塊來變更課程指派。 如果課程數目大上許多：

* 您可能會使用不同的使用者介面來顯示課程。
* 操作聯結實體來建立或刪除關聯性的方法不會變更。

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>新增類別來支援 [建立] 和 [編輯] 講師頁面

以下列程式碼建立 *SchoolViewModels/AssignedCourseData.cs*：

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` 類別包含資料，用來建立講師所指派課程的核取方塊。

建立 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* 基底類別：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` 是您將用於 [編輯] 和 [建立] 頁面模型的基底類別。 `PopulateAssignedCourseData` 會讀取所有 `Course` 實體來擴展 `AssignedCourseDataList`。 對於每個課程，此程式碼設定 `CourseID`、標題以及是否將講師指派給課程。 [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) 用來建立有效查閱。

### <a name="instructors-edit-page-model"></a>講師 [編輯] 頁面模型

以下列程式碼更新講師 [編輯] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

上述程式碼會處理辦公室指派變更。

更新講師 Razor 檢視：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> 當您將程式碼貼上到 Visual Studio 時，分行符號可能會產生變更，致使程式碼失效。 按下 Ctrl+Z 來復原自動格式化。 Ctrl+Z 會修正分行符號，使它們看起來就跟您在這裡看到的一樣。 縮排不一定要是完美的，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@:</tr>` 必須要如顯示般各自在獨立的一行上。 當選取新的程式碼區塊時，按下 Tab 鍵三次來讓新的程式碼對準現有的程式碼。 [使用此連結](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)票選或檢閱這個錯誤的狀態。

上述程式碼會建立一個 HTML 資料表，該資料表中有三個資料行。 每個資料行都有一個核取方塊，以及內含課程編號和標題 (title) 的標題 (caption)。 所有核取方塊都具有相同的名稱 ("selectedCourses")。 使用相同的名稱可告知模型繫結器將它們視為一個群組。 每個核取方塊的 Value 屬性都會設定為 `CourseID`。 當頁面發佈時，模型繫結器便會傳遞只包含所選取核取方塊之 `CourseID` 值的陣列。

一開始呈現核取方塊時，指派給講師的課程已核取屬性。

執行應用程式，並測試已更新的講師 [編輯] 頁面。 變更一些課程指派。 所做的變更會反映在 [索引] 頁面上。

注意：這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。 針對更大的集合，不同的 UI 和不同的更新方法可能更有用且更有效率。

### <a name="update-the-instructors-create-page"></a>更新講師 [建立] 頁面

以下列程式碼更新講師 [建立] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

上述程式碼類似於 *Pages/Instructors/Edit.cshtml.cs* 程式碼。

以下列標記更新講師 [建立 Razor] 頁面：

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

測試講師 [建立] 頁面。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

以下列程式碼更新 [刪除] 頁面模型：

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

上述程式碼會進行下列變更：

* 為 `CourseAssignments` 導覽屬性使用積極式載入。 必須包含 `CourseAssignments`，否則刪除講師時不會刪除它們。 若要避免需要讀取們，您可以在資料庫中設定串聯刪除。

* 若要刪除的講師已指派為任何部門的系統管理員，請先從這些部門中移除講師的指派。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/read-related-data)
[下一頁](xref:data/ef-rp/concurrency)
