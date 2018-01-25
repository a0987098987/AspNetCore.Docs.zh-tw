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
ms.openlocfilehash: 236589d0202a7f30f1e1a9d69902000fd9a2dd71
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="14047-103">更新相關的資料-EF 核心 Razor 頁面 (8 個 7)</span><span class="sxs-lookup"><span data-stu-id="14047-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="14047-104">由[Tom Dykstra](https://github.com/tdykstra)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14047-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="14047-105">本教學課程示範如何更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="14047-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="14047-106">如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)。</span><span class="sxs-lookup"><span data-stu-id="14047-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="14047-107">下圖顯示的部分完成的頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="14047-108">![課程編輯頁面](update-related-data/_static/course-edit.png)
![講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="14047-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="14047-109">檢查並測試建立與編輯課程頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="14047-110">建立新的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-110">Create a new course.</span></span> <span data-ttu-id="14047-111">部門會選取其主索引鍵 （整數），不是它的名稱。</span><span class="sxs-lookup"><span data-stu-id="14047-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="14047-112">編輯新的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-112">Edit the new course.</span></span> <span data-ttu-id="14047-113">當您完成測試時，刪除新的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="14047-114">建立共用通用程式碼基底類別</span><span class="sxs-lookup"><span data-stu-id="14047-114">Create a base class to share common code</span></span>

<span data-ttu-id="14047-115">課程/建立和課程/編輯頁面每個需要部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="14047-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="14047-116">建立*Pages/Courses/DepartmentNamePageModel.cshtml.cs*基底類別建立與編輯頁面：</span><span class="sxs-lookup"><span data-stu-id="14047-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="14047-117">上述程式碼會建立[SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0)包含部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="14047-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="14047-118">如果`selectedDepartment`指定，該部門中選取`SelectList`。</span><span class="sxs-lookup"><span data-stu-id="14047-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="14047-119">建立與編輯頁面模型類別會衍生自`DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="14047-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="14047-120">自訂課程頁面</span><span class="sxs-lookup"><span data-stu-id="14047-120">Customize the Courses Pages</span></span>

<span data-ttu-id="14047-121">建立新的課程實體時，它必須有現有的部門關聯性。</span><span class="sxs-lookup"><span data-stu-id="14047-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="14047-122">若要加入的部門建立課程時，建立與編輯的基底類別包含的下拉式清單選取部門。</span><span class="sxs-lookup"><span data-stu-id="14047-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="14047-123">下拉式清單設定`Course.DepartmentID`外部索引鍵 (FK) 屬性。</span><span class="sxs-lookup"><span data-stu-id="14047-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="14047-124">使用 EF 核心`Course.DepartmentID`載入 FK`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14047-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![建立課程](update-related-data/_static/ddl.png)

<span data-ttu-id="14047-126">更新下列程式碼建立頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="14047-127">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="14047-127">The preceding code:</span></span>

* <span data-ttu-id="14047-128">衍生自 `DepartmentNamePageModel`。</span><span class="sxs-lookup"><span data-stu-id="14047-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="14047-129">使用`TryUpdateModelAsync`防止[overposting](xref:data/ef-rp/crud#overposting)。</span><span class="sxs-lookup"><span data-stu-id="14047-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="14047-130">取代`ViewData["DepartmentID"]`與`DepartmentNameSL`（來自基底類別中）。</span><span class="sxs-lookup"><span data-stu-id="14047-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="14047-131">`ViewData["DepartmentID"]`已取代的強型別`DepartmentNameSL`。</span><span class="sxs-lookup"><span data-stu-id="14047-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="14047-132">強型別的的模型被優於弱型別。</span><span class="sxs-lookup"><span data-stu-id="14047-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="14047-133">如需詳細資訊，請參閱[弱型別資料 （別的 ViewData 和 ViewBag）](xref:mvc/views/overview#VD_VB)。</span><span class="sxs-lookup"><span data-stu-id="14047-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="14047-134">更新課程建立的頁面</span><span class="sxs-lookup"><span data-stu-id="14047-134">Update the Courses Create page</span></span>

<span data-ttu-id="14047-135">更新*Pages/Courses/Create.cshtml*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="14047-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="14047-136">上述標記進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="14047-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="14047-137">變更從標題**DepartmentID**至**部門**。</span><span class="sxs-lookup"><span data-stu-id="14047-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="14047-138">取代`"ViewBag.DepartmentID"`與`DepartmentNameSL`（來自基底類別中）。</span><span class="sxs-lookup"><span data-stu-id="14047-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="14047-139">新增 「 選取部門 」 選項。</span><span class="sxs-lookup"><span data-stu-id="14047-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="14047-140">這項變更會呈現 「 選取部門 」，而不是第一個部門。</span><span class="sxs-lookup"><span data-stu-id="14047-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="14047-141">部門尚未被選取時，請新增驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="14047-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="14047-142">使用 Razor 頁面[選取標記協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="14047-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="14047-143">測試 [建立] 頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-143">Test the Create page.</span></span> <span data-ttu-id="14047-144">[建立] 頁面會顯示部門名稱，而不是部門識別碼。</span><span class="sxs-lookup"><span data-stu-id="14047-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="14047-145">更新課程編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="14047-146">更新下列程式碼編輯頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="14047-147">就類似於建立頁面模型中所做的變更。</span><span class="sxs-lookup"><span data-stu-id="14047-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="14047-148">在上述程式碼，`PopulateDepartmentsDropDownList`部門識別碼中，選取下拉式清單中指定的部門的傳遞。</span><span class="sxs-lookup"><span data-stu-id="14047-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="14047-149">更新*Pages/Courses/Edit.cshtml*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="14047-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="14047-150">上述標記進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="14047-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="14047-151">顯示課程識別碼。</span><span class="sxs-lookup"><span data-stu-id="14047-151">Displays the course ID.</span></span> <span data-ttu-id="14047-152">通常不會顯示在主索引鍵 (PK) 的實體。</span><span class="sxs-lookup"><span data-stu-id="14047-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="14047-153">PKs 是通常無意義的使用者。</span><span class="sxs-lookup"><span data-stu-id="14047-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="14047-154">在此情況下，在 PK 是課程數目。</span><span class="sxs-lookup"><span data-stu-id="14047-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="14047-155">變更從標題**DepartmentID**至**部門**。</span><span class="sxs-lookup"><span data-stu-id="14047-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="14047-156">取代`"ViewBag.DepartmentID"`與`DepartmentNameSL`（來自基底類別中）。</span><span class="sxs-lookup"><span data-stu-id="14047-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="14047-157">新增 「 選取部門 」 選項。</span><span class="sxs-lookup"><span data-stu-id="14047-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="14047-158">這項變更會呈現 「 選取部門 」，而不是第一個部門。</span><span class="sxs-lookup"><span data-stu-id="14047-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="14047-159">部門尚未被選取時，請新增驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="14047-159">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="14047-160">此頁面包含隱藏的欄位 (`<input type="hidden">`) 課程編號。</span><span class="sxs-lookup"><span data-stu-id="14047-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="14047-161">加入`<label>`標記協助程式與`asp-for="Course.CourseID"`不免除為隱藏欄位。</span><span class="sxs-lookup"><span data-stu-id="14047-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="14047-162">`<input type="hidden">`所要包含在張貼的資料，當使用者按一下的課程編號的**儲存**。</span><span class="sxs-lookup"><span data-stu-id="14047-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="14047-163">測試更新的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14047-163">Test the updated code.</span></span> <span data-ttu-id="14047-164">建立、 編輯和刪除的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="14047-165">AsNoTracking 加入詳細資料，並刪除頁面模型</span><span class="sxs-lookup"><span data-stu-id="14047-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="14047-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__)時不需要追蹤可以改進效能。</span><span class="sxs-lookup"><span data-stu-id="14047-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="14047-167">新增`AsNoTracking`刪除和詳細資料頁面模型。</span><span class="sxs-lookup"><span data-stu-id="14047-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="14047-168">下列程式碼會顯示更新的 Delete 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="14047-169">更新`OnGetAsync`方法中的*Pages/Courses/Details.cshtml.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="14047-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="14047-170">修改 刪除 和 詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="14047-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="14047-171">更新刪除 Razor 頁面以下列標記：</span><span class="sxs-lookup"><span data-stu-id="14047-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="14047-172">詳細資料頁面中進行相同變更。</span><span class="sxs-lookup"><span data-stu-id="14047-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="14047-173">測試課程頁面</span><span class="sxs-lookup"><span data-stu-id="14047-173">Test the Course pages</span></span>

<span data-ttu-id="14047-174">測試建立、 編輯，詳細資料，以及刪除。</span><span class="sxs-lookup"><span data-stu-id="14047-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="14047-175">更新講師頁</span><span class="sxs-lookup"><span data-stu-id="14047-175">Update the instructor pages</span></span>

<span data-ttu-id="14047-176">下列各節會更新 [instructor] 頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="14047-177">新增辦公室位置</span><span class="sxs-lookup"><span data-stu-id="14047-177">Add office location</span></span>

<span data-ttu-id="14047-178">當編輯講師記錄時，您可能想要更新的講師 office 指派。</span><span class="sxs-lookup"><span data-stu-id="14047-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="14047-179">`Instructor`實體具有以零或-1 個關係`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14047-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="14047-180">必須處理講師程式碼：</span><span class="sxs-lookup"><span data-stu-id="14047-180">The instructor code must handle:</span></span>

* <span data-ttu-id="14047-181">如果使用者清除 office 指派，刪除`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14047-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="14047-182">如果使用者輸入 office 指派，而它是空白，建立新`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14047-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="14047-183">如果使用者變更 office 指派，更新`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14047-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="14047-184">下列程式碼來更新講師編輯頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="14047-185">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="14047-185">The preceding code:</span></span>

- <span data-ttu-id="14047-186">取得目前`Instructor`實體使用的積極式載入從資料庫`OfficeAssignment`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14047-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="14047-187">更新擷取`Instructor`實體模型繫結器的值。</span><span class="sxs-lookup"><span data-stu-id="14047-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="14047-188">`TryUpdateModel`防止[overposting](xref:data/ef-rp/crud#overposting)。</span><span class="sxs-lookup"><span data-stu-id="14047-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="14047-189">如果將辦公室位置為空白，`Instructor.OfficeAssignment`為 null。</span><span class="sxs-lookup"><span data-stu-id="14047-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="14047-190">當`Instructor.OfficeAssignment`是 null，相關的資料列中`OfficeAssignment`刪除資料表。</span><span class="sxs-lookup"><span data-stu-id="14047-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="14047-191">更新講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="14047-191">Update the instructor Edit page</span></span>

<span data-ttu-id="14047-192">更新*Pages/Instructors/Edit.cshtml*辦公室位置：</span><span class="sxs-lookup"><span data-stu-id="14047-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="14047-193">請確認您可以變更講師辦公室位置。</span><span class="sxs-lookup"><span data-stu-id="14047-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="14047-194">課程作業加入講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="14047-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="14047-195">講師可能教導任意數目的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="14047-196">在本節中，您可以將變更課程作業的能力。</span><span class="sxs-lookup"><span data-stu-id="14047-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="14047-197">下圖顯示更新的講師編輯頁面：</span><span class="sxs-lookup"><span data-stu-id="14047-197">The following image shows the updated instructor Edit page:</span></span>

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="14047-199">`Course`和`Instructor`具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="14047-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="14047-200">若要加入及移除關聯性，新增和移除從實體`CourseAssignments`加入實體集。</span><span class="sxs-lookup"><span data-stu-id="14047-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="14047-201">核取方塊會啟用的課程講師指派給變更。</span><span class="sxs-lookup"><span data-stu-id="14047-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="14047-202">核取方塊會顯示資料庫中的每個課程。</span><span class="sxs-lookup"><span data-stu-id="14047-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="14047-203">會檢查講師指派給的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="14047-204">使用者可以選取或清除核取方塊，以變更課程作業。</span><span class="sxs-lookup"><span data-stu-id="14047-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="14047-205">如果更大的課程數目：</span><span class="sxs-lookup"><span data-stu-id="14047-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="14047-206">您可能會使用不同的使用者介面顯示課程。</span><span class="sxs-lookup"><span data-stu-id="14047-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="14047-207">操作來建立或刪除關聯性聯結實體的方法不會變更。</span><span class="sxs-lookup"><span data-stu-id="14047-207">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="14047-208">新增類別，來支援建立和編輯講師頁面</span><span class="sxs-lookup"><span data-stu-id="14047-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="14047-209">建立*SchoolViewModels/AssignedCourseData.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="14047-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="14047-210">`AssignedCourseData`類別包含資料，以建立講師所指派的課程核取方塊。</span><span class="sxs-lookup"><span data-stu-id="14047-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="14047-211">建立*Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*基底類別：</span><span class="sxs-lookup"><span data-stu-id="14047-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="14047-212">`InstructorCoursesPageModel`的基底類別，您將使用 [編輯]，並建立頁面模型。</span><span class="sxs-lookup"><span data-stu-id="14047-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="14047-213">`PopulateAssignedCourseData`讀取所有`Course`實體來擴展`AssignedCourseDataList`。</span><span class="sxs-lookup"><span data-stu-id="14047-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="14047-214">每個課程中，此程式碼設定`CourseID`，標題和講師指派給課程是否。</span><span class="sxs-lookup"><span data-stu-id="14047-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="14047-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1)用來建立有效查閱。</span><span class="sxs-lookup"><span data-stu-id="14047-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="14047-216">講師編輯頁面模型</span><span class="sxs-lookup"><span data-stu-id="14047-216">Instructors Edit page model</span></span>

<span data-ttu-id="14047-217">下列程式碼來更新講師編輯頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="14047-218">上述程式碼會處理 office 指派變更。</span><span class="sxs-lookup"><span data-stu-id="14047-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="14047-219">更新講師 Razor 檢視：</span><span class="sxs-lookup"><span data-stu-id="14047-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="14047-220">當您將 Visual Studio 中的程式碼時，分行符號會變更會破壞程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="14047-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="14047-221">按一次 Ctrl + Z 復原自動格式化。</span><span class="sxs-lookup"><span data-stu-id="14047-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="14047-222">Ctrl + Z 會修正換行，讓它們看起來像這裡所示。</span><span class="sxs-lookup"><span data-stu-id="14047-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="14047-223">縮排不一定要是完美，但是`@</tr><tr>`， `@:<td>`， `@:</td>`，和`@:</tr>`行必須是在單一行所示。</span><span class="sxs-lookup"><span data-stu-id="14047-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="14047-224">選取新的程式碼區塊時，按下 Tab 鍵三次線與現有的程式碼的新程式碼。</span><span class="sxs-lookup"><span data-stu-id="14047-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="14047-225">票選或檢閱這個 bug 狀態[與此連結](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)。</span><span class="sxs-lookup"><span data-stu-id="14047-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="14047-226">上述程式碼會建立具有三個資料行的 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="14047-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="14047-227">每個資料行的核取方塊以及內含的課程編號和標題的標題。</span><span class="sxs-lookup"><span data-stu-id="14047-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="14047-228">所有核取方塊具有相同的名稱 ("selectedCourses")。</span><span class="sxs-lookup"><span data-stu-id="14047-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="14047-229">使用相同的名稱會告知模型繫結器視為一個群組。</span><span class="sxs-lookup"><span data-stu-id="14047-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="14047-230">每個核取方塊的 value 屬性的值設定為`CourseID`。</span><span class="sxs-lookup"><span data-stu-id="14047-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="14047-231">模型繫結器頁面張貼時，將所組成的陣列傳遞`CourseID`值只會選取核取方塊。</span><span class="sxs-lookup"><span data-stu-id="14047-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="14047-232">一開始會呈現核取方塊，指派給講師課程已屬性。</span><span class="sxs-lookup"><span data-stu-id="14047-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="14047-233">執行應用程式，並測試更新的講師編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="14047-234">變更一些課程作業。</span><span class="sxs-lookup"><span data-stu-id="14047-234">Change some course assignments.</span></span> <span data-ttu-id="14047-235">變更會反映在索引頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="14047-236">注意： 您帶往這裡編輯講師課程資料的方法就可以使用也有限的數目的課程。</span><span class="sxs-lookup"><span data-stu-id="14047-236">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="14047-237">對於更大的集合，不同的 UI 和不同的更新方法將會是能更有效率。</span><span class="sxs-lookup"><span data-stu-id="14047-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="14047-238">更新講師建立頁面</span><span class="sxs-lookup"><span data-stu-id="14047-238">Update the instructors Create page</span></span>

<span data-ttu-id="14047-239">下列程式碼來更新講師建立頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="14047-240">上述程式碼很類似*Pages/Instructors/Edit.cshtml.cs*程式碼。</span><span class="sxs-lookup"><span data-stu-id="14047-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="14047-241">更新講師建立 Razor 頁面以下列標記：</span><span class="sxs-lookup"><span data-stu-id="14047-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="14047-242">測試講師建立頁面。</span><span class="sxs-lookup"><span data-stu-id="14047-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="14047-243">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="14047-243">Update the Delete page</span></span>

<span data-ttu-id="14047-244">下列程式碼更新刪除頁面模型：</span><span class="sxs-lookup"><span data-stu-id="14047-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="14047-245">上述程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="14047-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="14047-246">會使用積極式載入的`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14047-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="14047-247">`CourseAssignments`必須加入或刪除講師時不刪除它們。</span><span class="sxs-lookup"><span data-stu-id="14047-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="14047-248">若要避免需要讀取它們，請在資料庫中設定串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="14047-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="14047-249">如果要刪除講師被指派任何部門的系統管理員身分，移除這些部門講師指派。</span><span class="sxs-lookup"><span data-stu-id="14047-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="14047-250">[上一頁](xref:data/ef-rp/read-related-data)
[下一頁](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="14047-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
