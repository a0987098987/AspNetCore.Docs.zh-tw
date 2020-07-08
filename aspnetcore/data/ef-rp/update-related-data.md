---
title: 第7部分， Razor ASP.NET Core 更新相關資料中包含 EF Core 的頁面
author: rick-anderson
description: 第7部分 Razor 頁面和 Entity Framework 教學課程系列。
ms.author: riande
ms.date: 07/22/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: data/ef-rp/update-related-data
ms.openlocfilehash: b442a4ce1f63c047c123315626f559155fd06424
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060133"
---
# <a name="part-7-razor-pages-with-ef-core-in-aspnet-core---update-related-data"></a><span data-ttu-id="fe434-103">第7部分， Razor ASP.NET Core 更新相關資料中包含 EF Core 的頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-103">Part 7, Razor Pages with EF Core in ASP.NET Core - Update Related Data</span></span>

<span data-ttu-id="fe434-104">由[Tom 作者: dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe434-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fe434-105">本教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="fe434-105">This tutorial shows how to update related data.</span></span> <span data-ttu-id="fe434-106">下圖顯示一些已完成的頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-106">The following illustrations show some of the completed pages.</span></span>

<span data-ttu-id="fe434-107">![課程 [編輯] 頁面](update-related-data/_static/course-edit30.png)
![講師 [編輯] 頁面](update-related-data/_static/instructor-edit-courses30.png)</span><span class="sxs-lookup"><span data-stu-id="fe434-107">![Course Edit page](update-related-data/_static/course-edit30.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses30.png)</span></span>

## <a name="update-the-course-create-and-edit-pages"></a><span data-ttu-id="fe434-108">更新 Course Create 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-108">Update the Course Create and Edit pages</span></span>

<span data-ttu-id="fe434-109">Course Create 和 Edit 頁面的 scaffold 程式碼包含一個 Department 下拉式清單，其顯示 Department 識別碼 (整數)。</span><span class="sxs-lookup"><span data-stu-id="fe434-109">The scaffolded code for the Course Create and Edit pages has a Department drop-down list that shows Department ID (an integer).</span></span> <span data-ttu-id="fe434-110">下拉式清單應該會顯示 Department 名稱，因此這兩個頁面需要部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="fe434-110">The drop-down should show the Department name, so both of these pages need a list of department names.</span></span> <span data-ttu-id="fe434-111">若要提供該清單，請使用 Create 和 Edit 頁面的基底類別。</span><span class="sxs-lookup"><span data-stu-id="fe434-111">To provide that list, use a base class for the Create and Edit pages.</span></span>

### <a name="create-a-base-class-for-course-create-and-edit"></a><span data-ttu-id="fe434-112">建立 Course Create 和 Edit 的基底類別</span><span class="sxs-lookup"><span data-stu-id="fe434-112">Create a base class for Course Create and Edit</span></span>

<span data-ttu-id="fe434-113">使用下列程式碼建立 *Pages/Courses/DepartmentNamePageModel.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="fe434-113">Create a *Pages/Courses/DepartmentNamePageModel.cs* file with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/DepartmentNamePageModel.cs)]

<span data-ttu-id="fe434-114">上述程式碼會建立 [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="fe434-114">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="fe434-115">如果指定了 `selectedDepartment`，就會在 `SelectList` 中選取該部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-115">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="fe434-116">*Create* 和 *Edit* 頁面模型類別將衍生自 `DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="fe434-116">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

### <a name="update-the-course-create-page-model"></a><span data-ttu-id="fe434-117">更新 Course Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-117">Update the Course Create page model</span></span>

<span data-ttu-id="fe434-118">Course 會指派給 Department。</span><span class="sxs-lookup"><span data-stu-id="fe434-118">A Course is assigned to a Department.</span></span> <span data-ttu-id="fe434-119">Create 和 Edit 頁面的基底類別會提供 `SelectList` 以用來選取部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-119">The base class for the Create and Edit pages provides a `SelectList` for selecting the department.</span></span> <span data-ttu-id="fe434-120">使用 `SelectList` 的下拉式清單會設定 `Course.DepartmentID` 外部索引鍵 (FK) 屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-120">The drop-down list that uses the `SelectList` sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="fe434-121">EF Core 則使用 `Course.DepartmentID` FK 來載入 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-121">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![建立課程](update-related-data/_static/ddl30.png)

<span data-ttu-id="fe434-123">使用下列程式碼更新 *Pages/Courses/Create.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-123">Update *Pages/Courses/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Create.cshtml.cs?highlight=7,18,27-41)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="fe434-124">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-124">The preceding code:</span></span>

* <span data-ttu-id="fe434-125">衍生自 `DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="fe434-125">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="fe434-126">使用 `TryUpdateModelAsync` 來防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。</span><span class="sxs-lookup"><span data-stu-id="fe434-126">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="fe434-127">移除 `ViewData["DepartmentID"]`。</span><span class="sxs-lookup"><span data-stu-id="fe434-127">Removes `ViewData["DepartmentID"]`.</span></span> <span data-ttu-id="fe434-128">`DepartmentNameSL`從基類是強型別模型，而且會供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="fe434-128">`DepartmentNameSL` from the base class is a strongly typed model and will be used by the Razor page.</span></span> <span data-ttu-id="fe434-129">強型別的模型優先於弱型別。</span><span class="sxs-lookup"><span data-stu-id="fe434-129">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="fe434-130">如需詳細資訊，請參閱[弱型別資料 (ViewData 和 ViewBag)](xref:mvc/views/overview#VD_VB)。</span><span class="sxs-lookup"><span data-stu-id="fe434-130">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-course-create-razor-page"></a><span data-ttu-id="fe434-131">更新課程 [建立] Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-131">Update the Course Create Razor page</span></span>

<span data-ttu-id="fe434-132">使用下列程式碼更新 *Pages/Courses/Create.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-132">Update *Pages/Courses/Create.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="fe434-133">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-133">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="fe434-134">將標題從 **DepartmentID** 變更為 **Department**。</span><span class="sxs-lookup"><span data-stu-id="fe434-134">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="fe434-135">以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。</span><span class="sxs-lookup"><span data-stu-id="fe434-135">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="fe434-136">新增 [選取部門] 選項。</span><span class="sxs-lookup"><span data-stu-id="fe434-136">Adds the "Select Department" option.</span></span> <span data-ttu-id="fe434-137">此變更會在尚未選取任何部門時，在下拉式清單中轉譯「選取部門」而非第一個部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-137">This change renders "Select Department" in the drop-down when no department has been selected yet, rather than the first department.</span></span>
* <span data-ttu-id="fe434-138">未選取部門時，請新增驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="fe434-138">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="fe434-139">Razor頁面會使用[選取](xref:mvc/views/working-with-forms#the-select-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="fe434-139">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="fe434-140">測試 *Create* 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-140">Test the Create page.</span></span> <span data-ttu-id="fe434-141">*Create* 頁面會顯示部門名稱，而不是部門識別碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-141">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-course-edit-page-model"></a><span data-ttu-id="fe434-142">更新 Course Edit 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-142">Update the Course Edit page model</span></span>

<span data-ttu-id="fe434-143">使用下列程式碼更新 *Pages/Courses/Edit.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-143">Update *Pages/Courses/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40-66)]

<span data-ttu-id="fe434-144">這些變更類似於 *Create* 頁面模型中所做的變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-144">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="fe434-145">在上述程式碼中，`PopulateDepartmentsDropDownList` 會傳遞部門識別碼，其在下拉式清單中選取部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-145">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which selects that department in the drop-down list.</span></span>

### <a name="update-the-course-edit-razor-page"></a><span data-ttu-id="fe434-146">更新課程 [編輯] Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-146">Update the Course Edit Razor page</span></span>

<span data-ttu-id="fe434-147">使用下列程式碼更新 *Pages/Courses/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-147">Update *Pages/Courses/Edit.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="fe434-148">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-148">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="fe434-149">顯示課程識別碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-149">Displays the course ID.</span></span> <span data-ttu-id="fe434-150">通常不會顯示實體的主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="fe434-150">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="fe434-151">PK 對使用者來說通常是沒有意義的。</span><span class="sxs-lookup"><span data-stu-id="fe434-151">PKs are usually meaningless to users.</span></span> <span data-ttu-id="fe434-152">在此情況下，PK 是課程編號。</span><span class="sxs-lookup"><span data-stu-id="fe434-152">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="fe434-153">將 Department 下拉式清單的標題從 **DepartmentID** 變更為 **Department**。</span><span class="sxs-lookup"><span data-stu-id="fe434-153">Changes the caption for the Department drop-down from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="fe434-154">以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。</span><span class="sxs-lookup"><span data-stu-id="fe434-154">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="fe434-155">此頁面包含課程編號的隱藏欄位 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="fe434-155">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="fe434-156">新增 `<label>` 標籤協助程式與 `asp-for="Course.CourseID"` 無法免除隱藏欄位的需求。</span><span class="sxs-lookup"><span data-stu-id="fe434-156">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="fe434-157">當使用者按一下 [儲存] \*\*\*\* 時，需要有 `<input type="hidden">` 才能將課程編號包含在張貼資料中。</span><span class="sxs-lookup"><span data-stu-id="fe434-157">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

## <a name="update-the-course-details-and-delete-pages"></a><span data-ttu-id="fe434-158">更新 Course Detail 和 Delete 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-158">Update the Course Details and Delete pages</span></span>

<span data-ttu-id="fe434-159">不需要追蹤時，[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以改善效能。</span><span class="sxs-lookup"><span data-stu-id="fe434-159">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span>

### <a name="update-the-course-page-models"></a><span data-ttu-id="fe434-160">更新 Course 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-160">Update the Course page models</span></span>

<span data-ttu-id="fe434-161">使用下列程式碼更新 *Pages/Courses/Delete.cshtml.cs* 來新增 `AsNoTracking`：</span><span class="sxs-lookup"><span data-stu-id="fe434-161">Update *Pages/Courses/Delete.cshtml.cs* with the following code to add `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Delete.cshtml.cs?highlight=29)]

<span data-ttu-id="fe434-162">在 *Pages/Courses/Details.cshtml.cs* 檔案中進行相同的變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-162">Make the same change in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Details.cshtml.cs?highlight=28)]

### <a name="update-the-course-razor-pages"></a><span data-ttu-id="fe434-163">更新課程 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-163">Update the Course Razor pages</span></span>

<span data-ttu-id="fe434-164">使用下列程式碼更新 *Pages/Courses/Delete.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-164">Update *Pages/Courses/Delete.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Delete.cshtml?highlight=15-20,37)]

<span data-ttu-id="fe434-165">對 *Details* 頁面進行相同的變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-165">Make the same changes to the Details page.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Details.cshtml?highlight=14-19,36)]

## <a name="test-the-course-pages"></a><span data-ttu-id="fe434-166">測試 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-166">Test the Course pages</span></span>

<span data-ttu-id="fe434-167">測試建立、編輯、詳細資料和刪除頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-167">Test the create, edit, details, and delete pages.</span></span>

## <a name="update-the-instructor-create-and-edit-pages"></a><span data-ttu-id="fe434-168">更新講師 Create 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-168">Update the instructor Create and Edit pages</span></span>

<span data-ttu-id="fe434-169">講師可教授任何數量的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-169">Instructors may teach any number of courses.</span></span> <span data-ttu-id="fe434-170">下圖顯示講師的 Edit 頁面，其中包含一個課程核取方塊的陣列。</span><span class="sxs-lookup"><span data-stu-id="fe434-170">The following image shows the instructor Edit page with an array of course checkboxes.</span></span>

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses30.png)

<span data-ttu-id="fe434-172">核取方塊可讓您變更指派給講師的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-172">The checkboxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="fe434-173">資料庫中的每個課程都會顯示一個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fe434-173">A checkbox is displayed for every course in the database.</span></span> <span data-ttu-id="fe434-174">指派給講師的課程會處於選取狀態。</span><span class="sxs-lookup"><span data-stu-id="fe434-174">Courses that the instructor is assigned to are selected.</span></span> <span data-ttu-id="fe434-175">使用者可以選取或清除核取方塊來變更課程指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-175">The user can select or clear checkboxes to change course assignments.</span></span> <span data-ttu-id="fe434-176">若課程數要多上許多，則使用不同 UI 的效能可能會更好。</span><span class="sxs-lookup"><span data-stu-id="fe434-176">If the number of courses were much greater, a different UI might work better.</span></span> <span data-ttu-id="fe434-177">但此處所顯示管理多對多關聯性的方法不會變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-177">But the method of managing a many-to-many relationship shown here wouldn't change.</span></span> <span data-ttu-id="fe434-178">若要建立或刪除關聯性，您可以操作聯結實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-178">To create or delete relationships, you manipulate a join entity.</span></span>

### <a name="create-a-class-for-assigned-courses-data"></a><span data-ttu-id="fe434-179">建立受指派課程資料的類別</span><span class="sxs-lookup"><span data-stu-id="fe434-179">Create a class for assigned courses data</span></span>

<span data-ttu-id="fe434-180">以下列程式碼建立 *SchoolViewModels/AssignedCourseData.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-180">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="fe434-181">`AssignedCourseData` 類別包含資料，用來建立指派給講師課程的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fe434-181">The `AssignedCourseData` class contains data to create the check boxes for courses assigned to an instructor.</span></span>

### <a name="create-an-instructor-page-model-base-class"></a><span data-ttu-id="fe434-182">建立 Instructor 頁面模型基底類別</span><span class="sxs-lookup"><span data-stu-id="fe434-182">Create an Instructor page model base class</span></span>

<span data-ttu-id="fe434-183">建立 *Pages/Instructors/InstructorCoursesPageModel.cs* 基底類別：</span><span class="sxs-lookup"><span data-stu-id="fe434-183">Create the *Pages/Instructors/InstructorCoursesPageModel.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_All)]

<span data-ttu-id="fe434-184">`InstructorCoursesPageModel` 是您將用於 *Edit* 和 *Create* 頁面模型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="fe434-184">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="fe434-185">`PopulateAssignedCourseData` 會讀取所有 `Course` 實體來擴展 `AssignedCourseDataList`。</span><span class="sxs-lookup"><span data-stu-id="fe434-185">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="fe434-186">對於每個課程，此程式碼設定 `CourseID`、標題以及是否將講師指派給課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-186">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="fe434-187">[HashSet](/dotnet/api/system.collections.generic.hashset-1) 會用來進行有效率的查閱。</span><span class="sxs-lookup"><span data-stu-id="fe434-187">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used for efficient lookups.</span></span>

<span data-ttu-id="fe434-188">由於 Razor 頁面沒有課程實體的集合，因此模型系結器無法自動更新 `CourseAssignments` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-188">Since the Razor page doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="fe434-189">相較於使用模型繫結器更新 `CourseAssignments` 導覽屬性，您會在新的 `UpdateInstructorCourses` 方法中進行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="fe434-189">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="fe434-190">因此您必須從模型繫結器中排除 `CourseAssignments` 屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-190">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="fe434-191">這不需要對呼叫的程式碼進行任何變更， `TryUpdateModel` 因為您是使用具有宣告屬性的多載，而且 `CourseAssignments` 不在包含清單中。</span><span class="sxs-lookup"><span data-stu-id="fe434-191">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the overload with declared properties and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="fe434-192">若沒有選取任何核取方塊，`UpdateInstructorCourses` 中的程式碼會使用空集合初始化 `CourseAssignments` 導覽屬性並傳回：</span><span class="sxs-lookup"><span data-stu-id="fe434-192">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_IfNull)]

<span data-ttu-id="fe434-193">然後程式碼會以迴圈逐一巡覽資料庫中的所有課程，並檢查每個已指派給講師的課程，以及在頁面中選取的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-193">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the page.</span></span> <span data-ttu-id="fe434-194">為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。</span><span class="sxs-lookup"><span data-stu-id="fe434-194">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="fe434-195">若課程的核取方塊已被選取，但課程並未位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會新增至導覽屬性的集合中。</span><span class="sxs-lookup"><span data-stu-id="fe434-195">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCourses)]

<span data-ttu-id="fe434-196">若課程的核取方塊未被選取，但課程卻位於 `Instructor.CourseAssignments` 導覽屬性中，則課程便會從導覽屬性的集合中移除。</span><span class="sxs-lookup"><span data-stu-id="fe434-196">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/InstructorCoursesPageModel.cs?name=snippet_UpdateCoursesElse)]

### <a name="handle-office-location"></a><span data-ttu-id="fe434-197">處理辦公室位置</span><span class="sxs-lookup"><span data-stu-id="fe434-197">Handle office location</span></span>

<span data-ttu-id="fe434-198">另一個編輯頁面必須處理的關聯性是 Instructor 實體針對 `OfficeAssignment` 實體所具有一對零或一對一關聯性。</span><span class="sxs-lookup"><span data-stu-id="fe434-198">Another relationship the edit page has to handle is the one-to-zero-or-one relationship that the Instructor entity has with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="fe434-199">講師編輯程式碼必須處理下列案例：</span><span class="sxs-lookup"><span data-stu-id="fe434-199">The instructor edit code must handle the following scenarios:</span></span> 

* <span data-ttu-id="fe434-200">如果使用者清除辦公室指派，請刪除 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-200">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="fe434-201">如果使用者輸入辦公室指派，但它是空的，請建立新的 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-201">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="fe434-202">如果使用者變更辦公室指派，請更新 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-202">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

### <a name="update-the-instructor-edit-page-model"></a><span data-ttu-id="fe434-203">更新 Instructor Edit 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-203">Update the Instructor Edit page model</span></span>

<span data-ttu-id="fe434-204">使用下列程式碼更新 *Pages/Instructors/Edit.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-204">Update *Pages/Instructors/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Edit.cshtml.cs?name=snippet_All&highlight=9,28-32,38,42-77)]

<span data-ttu-id="fe434-205">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-205">The preceding code:</span></span>

* <span data-ttu-id="fe434-206">使用 `OfficeAssignment`、`CourseAssignment` 和 `CourseAssignment.Course` 導覽屬性的積極式載入，從資料庫取得目前的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-206">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment`, `CourseAssignment`, and `CourseAssignment.Course` navigation properties.</span></span>
* <span data-ttu-id="fe434-207">使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-207">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="fe434-208">`TryUpdateModel` 會防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。</span><span class="sxs-lookup"><span data-stu-id="fe434-208">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="fe434-209">如果辦公室位置為空白，請將 `Instructor.OfficeAssignment` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="fe434-209">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="fe434-210">當 `Instructor.OfficeAssignment` 為 Null 時，將會刪除 `OfficeAssignment` 資料表中的相關資料列。</span><span class="sxs-lookup"><span data-stu-id="fe434-210">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>
* <span data-ttu-id="fe434-211">在 `OnGetAsync` 中呼叫 `PopulateAssignedCourseData` 來使用 `AssignedCourseData` 檢視模型類別，以提供資訊給核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fe434-211">Calls `PopulateAssignedCourseData` in `OnGetAsync` to provide information for the checkboxes using the `AssignedCourseData` view model class.</span></span>
* <span data-ttu-id="fe434-212">在 `OnPostAsync` 中呼叫 `UpdateInstructorCourses` 來將核取方塊的資訊套用到正在編輯的 Instructor 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-212">Calls `UpdateInstructorCourses` in `OnPostAsync` to apply information from the checkboxes to the Instructor entity being edited.</span></span>
* <span data-ttu-id="fe434-213">若 `TryUpdateModel` 失敗，則在 `OnPostAsync` 中呼叫 `PopulateAssignedCourseData` 和 `UpdateInstructorCourses`。</span><span class="sxs-lookup"><span data-stu-id="fe434-213">Calls `PopulateAssignedCourseData` and `UpdateInstructorCourses` in `OnPostAsync` if `TryUpdateModel` fails.</span></span> <span data-ttu-id="fe434-214">這些方法呼叫會在重新顯示並附帶錯誤訊息時，還原在頁面上輸入的已指派課程資料。</span><span class="sxs-lookup"><span data-stu-id="fe434-214">These method calls restore the assigned course data entered on the page when it is redisplayed with an error message.</span></span>

### <a name="update-the-instructor-edit-razor-page"></a><span data-ttu-id="fe434-215">更新講師 [編輯] Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-215">Update the Instructor Edit Razor page</span></span>

<span data-ttu-id="fe434-216">使用下列程式碼更新 *Pages/Instructors/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-216">Update *Pages/Instructors/Edit.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Edit.cshtml?highlight=29-59)]

<span data-ttu-id="fe434-217">上述程式碼會建立一個 HTML 資料表，該資料表中有三個資料行。</span><span class="sxs-lookup"><span data-stu-id="fe434-217">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="fe434-218">每個資料行都有一個核取方塊和包含課程號碼及標題的標題。</span><span class="sxs-lookup"><span data-stu-id="fe434-218">Each column has a checkbox and a caption containing the course number and title.</span></span> <span data-ttu-id="fe434-219">核取方塊全部都具有相同的名稱 ("selectedCourses")。</span><span class="sxs-lookup"><span data-stu-id="fe434-219">The checkboxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="fe434-220">使用相同的名稱可告知模型繫結器將它們視為一個群組。</span><span class="sxs-lookup"><span data-stu-id="fe434-220">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="fe434-221">每個核取方塊的 Value 屬性都會設為 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="fe434-221">The value attribute of each checkbox is set to `CourseID`.</span></span> <span data-ttu-id="fe434-222">張貼頁面時，模型繫結器會傳遞陣列，該陣列當中只包含已選取核取方塊的 `CourseID` 值。</span><span class="sxs-lookup"><span data-stu-id="fe434-222">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the checkboxes that are selected.</span></span>

<span data-ttu-id="fe434-223">一開始轉譯核取方塊時，指派給講師的課程會呈現選取狀態。</span><span class="sxs-lookup"><span data-stu-id="fe434-223">When the checkboxes are initially rendered, courses assigned to the instructor are selected.</span></span>

<span data-ttu-id="fe434-224">注意：這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。</span><span class="sxs-lookup"><span data-stu-id="fe434-224">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="fe434-225">針對更大的集合，不同的 UI 和不同的更新方法可能更有用且更有效率。</span><span class="sxs-lookup"><span data-stu-id="fe434-225">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

<span data-ttu-id="fe434-226">執行應用程式並測試更新後的 Instructors Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-226">Run the app and test the updated Instructors Edit page.</span></span> <span data-ttu-id="fe434-227">變更一些課程指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-227">Change some course assignments.</span></span> <span data-ttu-id="fe434-228">所做的變更會反映在 [索引] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="fe434-228">The changes are reflected on the Index page.</span></span>

### <a name="update-the-instructor-create-page"></a><span data-ttu-id="fe434-229">更新 Instructor Create 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-229">Update the Instructor Create page</span></span>

<span data-ttu-id="fe434-230">使用類似于 [編輯] 頁面的程式碼，更新講師 [建立] 頁面模型和 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="fe434-230">Update the Instructor Create page model and Razor page with code similar to the Edit page:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Create.cshtml.cs)]

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Create.cshtml)]

<span data-ttu-id="fe434-231">測試講師 *Create* 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-231">Test the instructor Create page.</span></span>

## <a name="update-the-instructor-delete-page"></a><span data-ttu-id="fe434-232">更新 Instructor Delete 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-232">Update the Instructor Delete page</span></span>

<span data-ttu-id="fe434-233">使用下列程式碼更新 *Pages/Instructors/Delete.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-233">Update *Pages/Instructors/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Delete.cshtml.cs?highlight=45-61)]

<span data-ttu-id="fe434-234">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-234">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="fe434-235">為 `CourseAssignments` 導覽屬性使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="fe434-235">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="fe434-236">必須包含 `CourseAssignments`，否則刪除講師時不會刪除它們。</span><span class="sxs-lookup"><span data-stu-id="fe434-236">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="fe434-237">若要避免需要讀取們，您可以在資料庫中設定串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="fe434-237">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="fe434-238">若要刪除的講師已指派為任何部門的系統管理員，請先從部門中移除講師的指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-238">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

<span data-ttu-id="fe434-239">執行應用程式及測試 Delete 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-239">Run the app and test the Delete page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe434-240">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fe434-240">Next steps</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe434-241">[上一個教學](xref:data/ef-rp/read-related-data) 
>  課程[下一個教學](xref:data/ef-rp/concurrency)課程</span><span class="sxs-lookup"><span data-stu-id="fe434-241">[Previous tutorial](xref:data/ef-rp/read-related-data)
[Next tutorial](xref:data/ef-rp/concurrency)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fe434-242">本教學課程將示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="fe434-242">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="fe434-243">若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="fe434-243">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="fe434-244">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="fe434-244">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="fe434-245">下圖顯示一些完成的頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-245">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="fe434-246">![課程 [編輯] 頁面](update-related-data/_static/course-edit.png)
![講師 [編輯] 頁面](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="fe434-246">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="fe434-247">檢查並測試 *Create* 與 *Edit* 課程頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-247">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="fe434-248">建立新的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-248">Create a new course.</span></span> <span data-ttu-id="fe434-249">部門是依照其主索引鍵 (整數) 來進行選取，而不是它的名稱。</span><span class="sxs-lookup"><span data-stu-id="fe434-249">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="fe434-250">編輯新的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-250">Edit the new course.</span></span> <span data-ttu-id="fe434-251">當您完成測試時，請刪除新的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-251">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="fe434-252">建立要共用通用程式碼的基底類別</span><span class="sxs-lookup"><span data-stu-id="fe434-252">Create a base class to share common code</span></span>

<span data-ttu-id="fe434-253">`Courses/Create` 和 `Courses/Edit` 頁面每個都需要部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="fe434-253">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="fe434-254">請針對 *Create* 和 *Edit* 頁面建立 *Pages/Courses/DepartmentNamePageModel.cshtml.cs* 基底類別：</span><span class="sxs-lookup"><span data-stu-id="fe434-254">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="fe434-255">上述程式碼會建立 [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) 以包含部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="fe434-255">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="fe434-256">如果指定了 `selectedDepartment`，就會在 `SelectList` 中選取該部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-256">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="fe434-257">*Create* 和 *Edit* 頁面模型類別將衍生自 `DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="fe434-257">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="fe434-258">自訂 [課程] 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-258">Customize the Courses Pages</span></span>

<span data-ttu-id="fe434-259">當新的課程實體建立時，其必須要與現有的部門具有關聯性。</span><span class="sxs-lookup"><span data-stu-id="fe434-259">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="fe434-260">為了在建立課程新增部門，*Create* 和 *Edit* 的基底類別包含用來選取部門的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="fe434-260">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="fe434-261">下拉式清單會設定 `Course.DepartmentID` 外部索引鍵 (FK) 屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-261">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="fe434-262">EF Core 則使用 `Course.DepartmentID` FK 來載入 `Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-262">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![建立課程](update-related-data/_static/ddl.png)

<span data-ttu-id="fe434-264">以下列程式碼更新 [建立] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-264">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="fe434-265">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-265">The preceding code:</span></span>

* <span data-ttu-id="fe434-266">衍生自 `DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="fe434-266">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="fe434-267">使用 `TryUpdateModelAsync` 來防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。</span><span class="sxs-lookup"><span data-stu-id="fe434-267">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="fe434-268">以 `DepartmentNameSL` (來自基底類別) 取代 `ViewData["DepartmentID"]`。</span><span class="sxs-lookup"><span data-stu-id="fe434-268">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="fe434-269">`ViewData["DepartmentID"]` 已取代為強型別的 `DepartmentNameSL`。</span><span class="sxs-lookup"><span data-stu-id="fe434-269">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="fe434-270">強型別的模型優先於弱型別。</span><span class="sxs-lookup"><span data-stu-id="fe434-270">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="fe434-271">如需詳細資訊，請參閱[弱型別資料 (ViewData 和 ViewBag)](xref:mvc/views/overview#VD_VB)。</span><span class="sxs-lookup"><span data-stu-id="fe434-271">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="fe434-272">更新 Courses 的 *Create* 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-272">Update the Courses Create page</span></span>

<span data-ttu-id="fe434-273">使用下列程式碼更新 *Pages/Courses/Create.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-273">Update *Pages/Courses/Create.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="fe434-274">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-274">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="fe434-275">將標題從 **DepartmentID** 變更為 **Department**。</span><span class="sxs-lookup"><span data-stu-id="fe434-275">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="fe434-276">以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。</span><span class="sxs-lookup"><span data-stu-id="fe434-276">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="fe434-277">新增 [選取部門] 選項。</span><span class="sxs-lookup"><span data-stu-id="fe434-277">Adds the "Select Department" option.</span></span> <span data-ttu-id="fe434-278">這項變更會呈現 [選取部門] ，而不是第一個部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-278">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="fe434-279">未選取部門時，請新增驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="fe434-279">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="fe434-280">Razor頁面會使用[選取](xref:mvc/views/working-with-forms#the-select-tag-helper)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="fe434-280">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="fe434-281">測試 *Create* 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-281">Test the Create page.</span></span> <span data-ttu-id="fe434-282">*Create* 頁面會顯示部門名稱，而不是部門識別碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-282">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="fe434-283">更新 Courses 的 *Edit* 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-283">Update the Courses Edit page.</span></span>

<span data-ttu-id="fe434-284">使用下列程式碼取代 *Pages/Courses/Edit.cshtml.cs* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-284">Replace the code in *Pages/Courses/Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="fe434-285">這些變更類似於 *Create* 頁面模型中所做的變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-285">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="fe434-286">在上述程式碼中，`PopulateDepartmentsDropDownList` 會傳入部門識別碼，以選取下拉式清單中指定的部門。</span><span class="sxs-lookup"><span data-stu-id="fe434-286">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="fe434-287">以下列標記更新 *Pages/Courses/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-287">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="fe434-288">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-288">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="fe434-289">顯示課程識別碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-289">Displays the course ID.</span></span> <span data-ttu-id="fe434-290">通常不會顯示實體的主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="fe434-290">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="fe434-291">PK 對使用者來說通常是沒有意義的。</span><span class="sxs-lookup"><span data-stu-id="fe434-291">PKs are usually meaningless to users.</span></span> <span data-ttu-id="fe434-292">在此情況下，PK 是課程編號。</span><span class="sxs-lookup"><span data-stu-id="fe434-292">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="fe434-293">將標題從 **DepartmentID** 變更為 **Department**。</span><span class="sxs-lookup"><span data-stu-id="fe434-293">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="fe434-294">以 `DepartmentNameSL` (來自基底類別) 取代 `"ViewBag.DepartmentID"`。</span><span class="sxs-lookup"><span data-stu-id="fe434-294">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="fe434-295">此頁面包含課程編號的隱藏欄位 (`<input type="hidden">`)。</span><span class="sxs-lookup"><span data-stu-id="fe434-295">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="fe434-296">新增 `<label>` 標籤協助程式與 `asp-for="Course.CourseID"` 無法免除隱藏欄位的需求。</span><span class="sxs-lookup"><span data-stu-id="fe434-296">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="fe434-297">當使用者按一下 [儲存] \*\*\*\* 時，需要有 `<input type="hidden">` 才能將課程編號包含在張貼資料中。</span><span class="sxs-lookup"><span data-stu-id="fe434-297">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="fe434-298">測試更新過的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-298">Test the updated code.</span></span> <span data-ttu-id="fe434-299">建立、編輯和刪除課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-299">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="fe434-300">將 AsNoTracking 新增至 *Details* 和 *Delete* 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-300">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="fe434-301">不需要追蹤時，[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 可以改善效能。</span><span class="sxs-lookup"><span data-stu-id="fe434-301">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="fe434-302">將 `AsNoTracking` 新增至 *Delete* 和 *Details* 頁面模型。</span><span class="sxs-lookup"><span data-stu-id="fe434-302">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="fe434-303">下列程式碼顯示已更新的 *Delete* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-303">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="fe434-304">在 *Pages/Courses/Details.cshtml.cs* 檔案中更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="fe434-304">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="fe434-305">修改 *Delete* 和 *Details* 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-305">Modify the Delete and Details pages</span></span>

<span data-ttu-id="fe434-306">以下列標記更新 [刪除] Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="fe434-306">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="fe434-307">對 *Details* 頁面進行相同的變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-307">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="fe434-308">測試 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-308">Test the Course pages</span></span>

<span data-ttu-id="fe434-309">測試建立、編輯、詳細資料和刪除。</span><span class="sxs-lookup"><span data-stu-id="fe434-309">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="fe434-310">更新講師頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-310">Update the instructor pages</span></span>

<span data-ttu-id="fe434-311">下列各節會更新講師頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-311">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="fe434-312">新增辦公室位置</span><span class="sxs-lookup"><span data-stu-id="fe434-312">Add office location</span></span>

<span data-ttu-id="fe434-313">當您編輯講師記錄時，可能會想要更新講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-313">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="fe434-314">`Instructor` 實體與 `OfficeAssignment` 實體具有一對零或一的關聯性。</span><span class="sxs-lookup"><span data-stu-id="fe434-314">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="fe434-315">必須處理講師程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-315">The instructor code must handle:</span></span>

* <span data-ttu-id="fe434-316">如果使用者清除辦公室指派，請刪除 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-316">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="fe434-317">如果使用者輸入辦公室指派，但它是空的，請建立新的 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-317">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="fe434-318">如果使用者變更辦公室指派，請更新 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-318">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="fe434-319">以下列程式碼更新講師 [編輯] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-319">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="fe434-320">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="fe434-320">The preceding code:</span></span>

* <span data-ttu-id="fe434-321">針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-321">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="fe434-322">使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-322">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="fe434-323">`TryUpdateModel` 會防止[大量指派](xref:data/ef-rp/crud#overposting) (overposting)。</span><span class="sxs-lookup"><span data-stu-id="fe434-323">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="fe434-324">如果辦公室位置為空白，請將 `Instructor.OfficeAssignment` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="fe434-324">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="fe434-325">當 `Instructor.OfficeAssignment` 為 Null 時，將會刪除 `OfficeAssignment` 資料表中的相關資料列。</span><span class="sxs-lookup"><span data-stu-id="fe434-325">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="fe434-326">更新講師 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-326">Update the instructor Edit page</span></span>

<span data-ttu-id="fe434-327">使用辦公室位置更新 *Pages/Instructors/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="fe434-327">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="fe434-328">請確認您可以變更講師辦公室位置。</span><span class="sxs-lookup"><span data-stu-id="fe434-328">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="fe434-329">將課程指派新增至講師 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-329">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="fe434-330">講師可教授任何數量的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-330">Instructors may teach any number of courses.</span></span> <span data-ttu-id="fe434-331">在本節中，您可以新增變更課程指派的能力。</span><span class="sxs-lookup"><span data-stu-id="fe434-331">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="fe434-332">下圖顯示已更新的講師 [編輯] 頁面：</span><span class="sxs-lookup"><span data-stu-id="fe434-332">The following image shows the updated instructor Edit page:</span></span>

![Instructor [編輯] 頁面與課程](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="fe434-334">`Course` 和 `Instructor` 具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="fe434-334">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="fe434-335">若要新增和移除關聯性，您必須在 `CourseAssignments` 聯結實體集中新增和移除實體。</span><span class="sxs-lookup"><span data-stu-id="fe434-335">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="fe434-336">核取方塊可變更指派給講師的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-336">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="fe434-337">資料庫中的每個課程各顯示一個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fe434-337">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="fe434-338">已核取指派給講師的課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-338">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="fe434-339">使用者可選取或清除核取方塊來變更課程指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-339">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="fe434-340">如果課程數目大上許多：</span><span class="sxs-lookup"><span data-stu-id="fe434-340">If the number of courses were much greater:</span></span>

* <span data-ttu-id="fe434-341">您可能會使用不同的使用者介面來顯示課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-341">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="fe434-342">操作聯結實體來建立或刪除關聯性的方法不會變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-342">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="fe434-343">新增類別來支援 *Create* 和 *Edit* 講師頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-343">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="fe434-344">以下列程式碼建立 *SchoolViewModels/AssignedCourseData.cs*：</span><span class="sxs-lookup"><span data-stu-id="fe434-344">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="fe434-345">`AssignedCourseData` 類別包含資料，用來建立講師所指派課程的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fe434-345">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="fe434-346">建立 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* 基底類別：</span><span class="sxs-lookup"><span data-stu-id="fe434-346">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="fe434-347">`InstructorCoursesPageModel` 是您將用於 *Edit* 和 *Create* 頁面模型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="fe434-347">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="fe434-348">`PopulateAssignedCourseData` 會讀取所有 `Course` 實體來擴展 `AssignedCourseDataList`。</span><span class="sxs-lookup"><span data-stu-id="fe434-348">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="fe434-349">對於每個課程，此程式碼設定 `CourseID`、標題以及是否將講師指派給課程。</span><span class="sxs-lookup"><span data-stu-id="fe434-349">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="fe434-350">[HashSet](/dotnet/api/system.collections.generic.hashset-1) 用來建立有效查閱。</span><span class="sxs-lookup"><span data-stu-id="fe434-350">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="fe434-351">講師 *Edit* 頁面模型</span><span class="sxs-lookup"><span data-stu-id="fe434-351">Instructors Edit page model</span></span>

<span data-ttu-id="fe434-352">以下列程式碼更新講師 [編輯] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-352">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="fe434-353">上述程式碼會處理辦公室指派變更。</span><span class="sxs-lookup"><span data-stu-id="fe434-353">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="fe434-354">更新講師 Razor View：</span><span class="sxs-lookup"><span data-stu-id="fe434-354">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="fe434-355">當您將程式碼貼上到 Visual Studio 時，分行符號可能會產生變更，致使程式碼失效。</span><span class="sxs-lookup"><span data-stu-id="fe434-355">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="fe434-356">按 Ctrl+Z 來復原自動格式化。</span><span class="sxs-lookup"><span data-stu-id="fe434-356">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="fe434-357">Ctrl+Z 會修正分行符號，使它們看起來就跟您在這裡看到的一樣。</span><span class="sxs-lookup"><span data-stu-id="fe434-357">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="fe434-358">縮排不一定要是完美的，但 `@:</tr><tr>`、`@:<td>`、`@:</td>` 和 `@:</tr>` 必須要如顯示般各自在獨立的一行上。</span><span class="sxs-lookup"><span data-stu-id="fe434-358">The indentation doesn't have to be perfect, but the `@:</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="fe434-359">當選取新的程式碼區塊時，按 Tab 鍵三次來讓新的程式碼對準現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-359">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="fe434-360">[使用此連結](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)票選或檢閱這個錯誤的狀態。</span><span class="sxs-lookup"><span data-stu-id="fe434-360">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="fe434-361">上述程式碼會建立一個 HTML 資料表，該資料表中有三個資料行。</span><span class="sxs-lookup"><span data-stu-id="fe434-361">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="fe434-362">每個資料行都有一個核取方塊，以及內含課程編號和標題 (title) 的標題 (caption)。</span><span class="sxs-lookup"><span data-stu-id="fe434-362">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="fe434-363">所有核取方塊都具有相同的名稱 ("selectedCourses")。</span><span class="sxs-lookup"><span data-stu-id="fe434-363">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="fe434-364">使用相同的名稱可告知模型繫結器將它們視為一個群組。</span><span class="sxs-lookup"><span data-stu-id="fe434-364">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="fe434-365">每個核取方塊的 Value 屬性都會設定為 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="fe434-365">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="fe434-366">當頁面發佈時，模型繫結器便會傳遞只包含所選取核取方塊之 `CourseID` 值的陣列。</span><span class="sxs-lookup"><span data-stu-id="fe434-366">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="fe434-367">一開始呈現核取方塊時，指派給講師的課程已核取屬性。</span><span class="sxs-lookup"><span data-stu-id="fe434-367">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="fe434-368">執行應用程式，並測試已更新的講師 [編輯] 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-368">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="fe434-369">變更一些課程指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-369">Change some course assignments.</span></span> <span data-ttu-id="fe434-370">所做的變更會反映在 [索引] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="fe434-370">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="fe434-371">注意：這裡所用來編輯講師課程資料的方法在課程的數量有限時運作相當良好。</span><span class="sxs-lookup"><span data-stu-id="fe434-371">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="fe434-372">針對更大的集合，不同的 UI 和不同的更新方法可能更有用且更有效率。</span><span class="sxs-lookup"><span data-stu-id="fe434-372">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="fe434-373">更新講師 *Create* 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-373">Update the instructors Create page</span></span>

<span data-ttu-id="fe434-374">以下列程式碼更新講師 *Create* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-374">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="fe434-375">上述程式碼類似於 *Pages/Instructors/Edit.cshtml.cs* 程式碼。</span><span class="sxs-lookup"><span data-stu-id="fe434-375">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="fe434-376">以下列標記更新講師 [建立] Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="fe434-376">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="fe434-377">測試講師 *Create* 頁面。</span><span class="sxs-lookup"><span data-stu-id="fe434-377">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="fe434-378">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="fe434-378">Update the Delete page</span></span>

<span data-ttu-id="fe434-379">以下列程式碼更新 [刪除] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="fe434-379">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="fe434-380">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="fe434-380">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="fe434-381">為 `CourseAssignments` 導覽屬性使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="fe434-381">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="fe434-382">必須包含 `CourseAssignments`，否則刪除講師時不會刪除它們。</span><span class="sxs-lookup"><span data-stu-id="fe434-382">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="fe434-383">若要避免需要讀取們，您可以在資料庫中設定串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="fe434-383">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="fe434-384">若要刪除的講師已指派為任何部門的系統管理員，請先從部門中移除講師的指派。</span><span class="sxs-lookup"><span data-stu-id="fe434-384">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe434-385">其他資源</span><span class="sxs-lookup"><span data-stu-id="fe434-385">Additional resources</span></span>

* [<span data-ttu-id="fe434-386">這個教學課程的 YouTube 版本 (第 1 部分)</span><span class="sxs-lookup"><span data-stu-id="fe434-386">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="fe434-387">這個教學課程的 YouTube 版本 (第 2 部分)</span><span class="sxs-lookup"><span data-stu-id="fe434-387">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="fe434-388">[上一個](xref:data/ef-rp/read-related-data) 
> [下一步](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="fe434-388">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>

::: moniker-end
