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
ms.openlocfilehash: 655fefc0f9d884300bea670795c39a7a9aa10bb8
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="0555a-104">更新相關的資料-EF Core 與 ASP.NET Core MVC 教學課程 (10-7)</span><span class="sxs-lookup"><span data-stu-id="0555a-104">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="0555a-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0555a-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0555a-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0555a-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0555a-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="0555a-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0555a-108">您可以在上一個教學課程中顯示相關的資料。在本教學課程中，您要更新相關的資料藉由更新外部索引鍵欄位，而且導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="0555a-109">下圖顯示一些您將使用的頁面。</span><span class="sxs-lookup"><span data-stu-id="0555a-109">The following illustrations show some of the pages that you'll work with.</span></span>

![課程編輯頁面](update-related-data/_static/course-edit.png)

![講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="0555a-112">建立和編輯網頁自訂課程</span><span class="sxs-lookup"><span data-stu-id="0555a-112">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="0555a-113">建立新的課程實體時，它必須有現有的部門關聯性。</span><span class="sxs-lookup"><span data-stu-id="0555a-113">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="0555a-114">若要達成此目的，scaffold 的程式碼包含控制器方法以及建立與編輯檢視，包含下拉式清單中選取部門。</span><span class="sxs-lookup"><span data-stu-id="0555a-114">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="0555a-115">下拉式清單設定`Course.DepartmentID`外部索引鍵屬性，而這就是 Entity Framework 必須以載入所有`Department`與適當的 Department 實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-115">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="0555a-116">您將使用 scaffold 的程式碼，但變更它稍微來加入錯誤處理和排序下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="0555a-116">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="0555a-117">在*CoursesController.cs*刪除四種建立與編輯方法，取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0555a-117">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

<span data-ttu-id="0555a-118">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]</span><span class="sxs-lookup"><span data-stu-id="0555a-118">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]</span></span>

<span data-ttu-id="0555a-119">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]</span><span class="sxs-lookup"><span data-stu-id="0555a-119">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]</span></span>

<span data-ttu-id="0555a-120">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]</span><span class="sxs-lookup"><span data-stu-id="0555a-120">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]</span></span>

<span data-ttu-id="0555a-121">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]</span><span class="sxs-lookup"><span data-stu-id="0555a-121">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]</span></span>

<span data-ttu-id="0555a-122">之後`Edit`HttpPost 方法建立新的方法，以載入部門資訊的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="0555a-122">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

<span data-ttu-id="0555a-123">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]</span><span class="sxs-lookup"><span data-stu-id="0555a-123">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]</span></span>

<span data-ttu-id="0555a-124">`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`集合下拉式清單中，並將集合傳遞給在檢視`ViewBag`。</span><span class="sxs-lookup"><span data-stu-id="0555a-124">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="0555a-125">該方法會接受選擇性`selectedDepartment`參數，可讓呼叫的程式碼，指定將會呈現下拉式清單時所選取的項目。</span><span class="sxs-lookup"><span data-stu-id="0555a-125">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="0555a-126">檢視會將"DepartmentID 」 的名稱傳遞給`<select>`標記協助程式，以及協助程式就會知道要查看`ViewBag`物件`SelectList`名為"DepartmentID"。</span><span class="sxs-lookup"><span data-stu-id="0555a-126">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="0555a-127">HttpGet`Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新的課程部門不會建立尚未方法：</span><span class="sxs-lookup"><span data-stu-id="0555a-127">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

<span data-ttu-id="0555a-128">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]</span><span class="sxs-lookup"><span data-stu-id="0555a-128">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]</span></span>

<span data-ttu-id="0555a-129">HttpGet`Edit`方法設定選取的項目，根據已指派給正在編輯的課程部門的識別碼：</span><span class="sxs-lookup"><span data-stu-id="0555a-129">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

<span data-ttu-id="0555a-130">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]</span><span class="sxs-lookup"><span data-stu-id="0555a-130">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]</span></span>

<span data-ttu-id="0555a-131">兩者的 HttpPost 方法`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0555a-131">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="0555a-132">這可確保，當頁面會重新顯示，以顯示錯誤訊息，已選取任何部門會持續選取。</span><span class="sxs-lookup"><span data-stu-id="0555a-132">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="0555a-133">加入。AsNoTracking 詳細資料和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="0555a-133">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="0555a-134">若要最佳化效能的課程詳細資料及刪除頁，將`AsNoTracking`中呼叫`Details`和 HttpGet`Delete`方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-134">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

<span data-ttu-id="0555a-135">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]</span><span class="sxs-lookup"><span data-stu-id="0555a-135">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]</span></span>

<span data-ttu-id="0555a-136">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]</span><span class="sxs-lookup"><span data-stu-id="0555a-136">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]</span></span>

### <a name="modify-the-course-views"></a><span data-ttu-id="0555a-137">修改課程檢視</span><span class="sxs-lookup"><span data-stu-id="0555a-137">Modify the Course views</span></span>

<span data-ttu-id="0555a-138">在*Views/Courses/Create.cshtml*，新增 「 選取部門 」 選項，以**部門**下拉式清單中，變更從標題**DepartmentID**至**部門**，並新增驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="0555a-138">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

<span data-ttu-id="0555a-139">[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]</span><span class="sxs-lookup"><span data-stu-id="0555a-139">[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]</span></span>

<span data-ttu-id="0555a-140">在*Views/Courses/Edit.cshtml*，進行相同的變更，只要未在該部門欄位*Create.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0555a-140">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="0555a-141">此外，在*Views/Courses/Edit.cshtml*，加入課程數字欄位之前**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="0555a-141">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="0555a-142">課程數字是主索引鍵，因為它會顯示，但無法變更。</span><span class="sxs-lookup"><span data-stu-id="0555a-142">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

<span data-ttu-id="0555a-143">[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]</span><span class="sxs-lookup"><span data-stu-id="0555a-143">[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]</span></span>

<span data-ttu-id="0555a-144">已隱藏的欄位 (`<input type="hidden">`) 中編輯檢視課程編號。</span><span class="sxs-lookup"><span data-stu-id="0555a-144">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="0555a-145">加入`<label>`標記協助程式不會消除隱藏欄位的需要，因為它並不會造成要包含在張貼的資料，當使用者按一下的課程編號**儲存**上**編輯**頁面。</span><span class="sxs-lookup"><span data-stu-id="0555a-145">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="0555a-146">在*Views/Courses/Delete.cshtml*、 上方加入課程數字欄位，並將 department ID 變更為部門名稱。</span><span class="sxs-lookup"><span data-stu-id="0555a-146">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

<span data-ttu-id="0555a-147">[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]</span><span class="sxs-lookup"><span data-stu-id="0555a-147">[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]</span></span>

<span data-ttu-id="0555a-148">在*Views/Courses/Details.cshtml*，進行相同的變更，您只對*Delete.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0555a-148">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="0555a-149">測試課程頁面</span><span class="sxs-lookup"><span data-stu-id="0555a-149">Test the Course pages</span></span>

<span data-ttu-id="0555a-150">執行**建立**頁面 (顯示課程索引頁，然後按一下**新建**) 並輸入新的課程資料：</span><span class="sxs-lookup"><span data-stu-id="0555a-150">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![課程建立頁面](update-related-data/_static/course-create.png)

<span data-ttu-id="0555a-152">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="0555a-152">Click **Create**.</span></span> <span data-ttu-id="0555a-153">課程索引頁會顯示新的課程新增至清單。</span><span class="sxs-lookup"><span data-stu-id="0555a-153">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="0555a-154">在索引頁面的清單中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。</span><span class="sxs-lookup"><span data-stu-id="0555a-154">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="0555a-155">執行**編輯**頁面 (按一下**編輯**課程中的課程索引頁面上)。</span><span class="sxs-lookup"><span data-stu-id="0555a-155">Run the **Edit** page (click **Edit** on a course in the Course Index page ).</span></span>

![課程編輯頁面](update-related-data/_static/course-edit.png)

<span data-ttu-id="0555a-157">變更網頁上的資料，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0555a-157">Change data on the page and click **Save**.</span></span> <span data-ttu-id="0555a-158">課程索引頁會顯示更新的課程資料。</span><span class="sxs-lookup"><span data-stu-id="0555a-158">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="0555a-159">新增對講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="0555a-159">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="0555a-160">當您編輯講師記錄時，您想要能夠更新講師 office 指派。</span><span class="sxs-lookup"><span data-stu-id="0555a-160">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="0555a-161">[Instructor] 實體具有以零或-1 個關係 OfficeAssignment 實體，這表示您的程式碼必須處理在下列情況：</span><span class="sxs-lookup"><span data-stu-id="0555a-161">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="0555a-162">如果使用者清除 office 指派它最初的值，請刪除 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="0555a-162">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="0555a-163">如果使用者輸入 office 指派值，而且它最初是空的建立新的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="0555a-163">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="0555a-164">如果使用者變更 office 指派的值，變更現有 OfficeAssignment 實體中的值。</span><span class="sxs-lookup"><span data-stu-id="0555a-164">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0555a-165">更新講師控制站</span><span class="sxs-lookup"><span data-stu-id="0555a-165">Update the Instructors controller</span></span>

<span data-ttu-id="0555a-166">在*InstructorsController.cs*，HttpGet 在變更程式碼`Edit`方法，使它載入 [Instructor] 實體`OfficeAssignment`導覽屬性並呼叫`AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="0555a-166">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

<span data-ttu-id="0555a-167">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]</span><span class="sxs-lookup"><span data-stu-id="0555a-167">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]</span></span>

<span data-ttu-id="0555a-168">取代 HttpPost`Edit`方法來處理 office 指派更新為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0555a-168">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

<span data-ttu-id="0555a-169">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]</span><span class="sxs-lookup"><span data-stu-id="0555a-169">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]</span></span>

<span data-ttu-id="0555a-170">程式碼會執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="0555a-170">The code does the following:</span></span>

-  <span data-ttu-id="0555a-171">變更方法名稱，以`EditPost`因為簽章現在是 HttpGet 相同`Edit`方法 (`ActionName`屬性指定`/Edit/`仍會使用 URL)。</span><span class="sxs-lookup"><span data-stu-id="0555a-171">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="0555a-172">取得資料庫使用的目前 [Instructor] 實體積極式載入`OfficeAssignment`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-172">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="0555a-173">這是您未在 HttpGet 相同`Edit`方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-173">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="0555a-174">更新擷取 [Instructor] 實體中的模型繫結器的值。</span><span class="sxs-lookup"><span data-stu-id="0555a-174">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="0555a-175">`TryUpdateModel`多載可讓您將白名單您想要包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-175">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="0555a-176">這可防止過度公佈時，所述[第二個教學課程](crud.md)。</span><span class="sxs-lookup"><span data-stu-id="0555a-176">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="0555a-177">如果辦公室位置為空白，將 Instructor.OfficeAssignment 屬性設定為 null，如此就會刪除 OfficeAssignment 資料表中相關的資料列。</span><span class="sxs-lookup"><span data-stu-id="0555a-177">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="0555a-178">將變更儲存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="0555a-178">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="0555a-179">更新 Instructor 編輯檢視</span><span class="sxs-lookup"><span data-stu-id="0555a-179">Update the Instructor Edit view</span></span>

<span data-ttu-id="0555a-180">在*Views/Instructors/Edit.cshtml*，加入新欄位來編輯辦公室位置，在結束之前**儲存**按鈕：</span><span class="sxs-lookup"><span data-stu-id="0555a-180">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

<span data-ttu-id="0555a-181">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]</span><span class="sxs-lookup"><span data-stu-id="0555a-181">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]</span></span>

<span data-ttu-id="0555a-182">執行網頁 (選取**講師**索引標籤，然後按一下 **編輯**講師上)。</span><span class="sxs-lookup"><span data-stu-id="0555a-182">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="0555a-183">變更**辦公室位置**按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0555a-183">Change the **Office Location** and click **Save**.</span></span>

![講師編輯頁面](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="0555a-185">加入課程指派至講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="0555a-185">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="0555a-186">講師可能教導任意數目的課程。</span><span class="sxs-lookup"><span data-stu-id="0555a-186">Instructors may teach any number of courses.</span></span> <span data-ttu-id="0555a-187">現在您將會增強講師編輯網頁所加入的能力變更課程作業使用的群組核取方塊，下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="0555a-187">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0555a-189">「 課程 」 和 「 講師 」 實體之間的關聯性是多對多。</span><span class="sxs-lookup"><span data-stu-id="0555a-189">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="0555a-190">若要加入及移除關聯性，您可以加入和移除 CourseAssignments 聯結實體集的實體。</span><span class="sxs-lookup"><span data-stu-id="0555a-190">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="0555a-191">可讓您變更哪些課程 UI 講師指派給已核取方塊的群組。</span><span class="sxs-lookup"><span data-stu-id="0555a-191">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="0555a-192">顯示每個課程資料庫中的核取方塊，並選取講師目前指派給項目。</span><span class="sxs-lookup"><span data-stu-id="0555a-192">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="0555a-193">使用者可以選取或清除核取方塊，以變更課程作業。</span><span class="sxs-lookup"><span data-stu-id="0555a-193">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="0555a-194">如果課程數目大很多，您可能會想要使用其他方法將資料呈現在檢視中，但您要用於建立或刪除關聯性的處理聯結實體相同的方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-194">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="0555a-195">更新講師控制站</span><span class="sxs-lookup"><span data-stu-id="0555a-195">Update the Instructors controller</span></span>

<span data-ttu-id="0555a-196">若要提供的核取方塊清單檢視資料，您將使用的檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="0555a-196">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="0555a-197">建立*AssignedCourseData.cs*中*SchoolViewModels*資料夾並取代現有的程式碼取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0555a-197">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

<span data-ttu-id="0555a-198">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]</span><span class="sxs-lookup"><span data-stu-id="0555a-198">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]</span></span>

<span data-ttu-id="0555a-199">在*InstructorsController.cs*，取代 HttpGet`Edit`方法取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="0555a-199">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="0555a-200">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="0555a-200">The changes are highlighted.</span></span>

<span data-ttu-id="0555a-201">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]</span><span class="sxs-lookup"><span data-stu-id="0555a-201">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]</span></span>

<span data-ttu-id="0555a-202">加入的程式碼的積極式載入`Courses`導覽屬性，並呼叫新`PopulateAssignedCourseData`方法以提供的核取方塊陣列使用資訊`AssignedCourseData`檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="0555a-202">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="0555a-203">中的程式碼`PopulateAssignedCourseData`方法會讀取所有課程實體透過以載入課程使用的檢視模型類別的清單。</span><span class="sxs-lookup"><span data-stu-id="0555a-203">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="0555a-204">每個課程中，程式碼會檢查課程是否存在於講師`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-204">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="0555a-205">若要建立有效率查閱，檢查是否課程已指派給講師時，指派給講師的課程會放入`HashSet`集合。</span><span class="sxs-lookup"><span data-stu-id="0555a-205">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="0555a-206">`Assigned`屬性設定為 true 的課程講師指派給。</span><span class="sxs-lookup"><span data-stu-id="0555a-206">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="0555a-207">檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為選取。</span><span class="sxs-lookup"><span data-stu-id="0555a-207">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="0555a-208">最後，清單會傳遞至檢視中`ViewData`。</span><span class="sxs-lookup"><span data-stu-id="0555a-208">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="0555a-209">接下來，新增使用者時所執行的程式碼**儲存**。</span><span class="sxs-lookup"><span data-stu-id="0555a-209">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="0555a-210">取代`EditPost`方法，以下列程式碼，並增加新方法，以更新`Courses`[Instructor] 實體導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-210">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

<span data-ttu-id="0555a-211">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]</span><span class="sxs-lookup"><span data-stu-id="0555a-211">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]</span></span>

<span data-ttu-id="0555a-212">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]</span><span class="sxs-lookup"><span data-stu-id="0555a-212">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]</span></span>

<span data-ttu-id="0555a-213">方法簽章現在是不同於 HttpGet`Edit`方法，以便從變更的方法名稱`EditPost`回到`Edit`。</span><span class="sxs-lookup"><span data-stu-id="0555a-213">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="0555a-214">由於檢視不會有課程實體的集合，所以無法自動更新模型繫結器`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-214">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="0555a-215">而不是使用模型繫結器，來更新`CourseAssignments`導覽屬性，在中的新執行`UpdateInstructorCourses`方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-215">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="0555a-216">因此您必須排除`CourseAssignments`屬性從模型繫結。</span><span class="sxs-lookup"><span data-stu-id="0555a-216">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="0555a-217">這並不需要呼叫的程式碼的任何變更`TryUpdateModel`因為您正在使用的允許清單的多載和`CourseAssignments`不在 include 清單中。</span><span class="sxs-lookup"><span data-stu-id="0555a-217">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="0555a-218">如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`CourseAssignments`空集合，並傳回具有瀏覽屬性：</span><span class="sxs-lookup"><span data-stu-id="0555a-218">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

<span data-ttu-id="0555a-219">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]</span><span class="sxs-lookup"><span data-stu-id="0555a-219">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]</span></span>

<span data-ttu-id="0555a-220">程式碼執行迴圈，在資料庫中的所有課程，然後檢查每個課程，針對目前已指派給講師對照檢視中所選取的項目。</span><span class="sxs-lookup"><span data-stu-id="0555a-220">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="0555a-221">為了有效率查閱，後者的兩個集合會儲存在`HashSet`物件。</span><span class="sxs-lookup"><span data-stu-id="0555a-221">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="0555a-222">如果課程核取方塊已選取，但過程中沒有`Instructor.CourseAssignments`導覽屬性，過程會加入至集合中的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-222">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

<span data-ttu-id="0555a-223">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]</span><span class="sxs-lookup"><span data-stu-id="0555a-223">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]</span></span>

<span data-ttu-id="0555a-224">如果未選取課程核取方塊，但是課程在`Instructor.CourseAssignments`導覽屬性，課程移除導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-224">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

<span data-ttu-id="0555a-225">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]</span><span class="sxs-lookup"><span data-stu-id="0555a-225">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]</span></span>

### <a name="update-the-instructor-views"></a><span data-ttu-id="0555a-226">更新講師檢視</span><span class="sxs-lookup"><span data-stu-id="0555a-226">Update the Instructor views</span></span>

<span data-ttu-id="0555a-227">在*Views/Instructors/Edit.cshtml*，新增**課程**欄位與陣列中加入下列的核取方塊的程式碼後立即`div`項目**Office**欄位，以及之前`div`元素**儲存** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0555a-227">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="0555a-228">當您將程式碼貼在 Visual Studio 中時，插入換行符號將會破壞程式碼的方式。</span><span class="sxs-lookup"><span data-stu-id="0555a-228">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="0555a-229">按一次 Ctrl + Z 復原自動格式化。</span><span class="sxs-lookup"><span data-stu-id="0555a-229">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="0555a-230">這會修正換行，讓它們看起來像這裡所示。</span><span class="sxs-lookup"><span data-stu-id="0555a-230">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="0555a-231">縮排不一定要是完美，但是`@</tr><tr>`， `@:<td>`， `@:</td>`，和`@:</tr>`行必須是在單一行所示，否則將會執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="0555a-231">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="0555a-232">選取新的程式碼區塊時，按下 Tab 鍵三次線與現有的程式碼的新程式碼。</span><span class="sxs-lookup"><span data-stu-id="0555a-232">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span>

<span data-ttu-id="0555a-233">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]</span><span class="sxs-lookup"><span data-stu-id="0555a-233">[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]</span></span>

<span data-ttu-id="0555a-234">此程式碼會建立具有三個資料行的 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="0555a-234">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="0555a-235">在每個資料行是核取方塊，後面的課程編號和標題所組成的標題。</span><span class="sxs-lookup"><span data-stu-id="0555a-235">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="0555a-236">所有核取方塊具有相同名稱 ("selectedCourses 」) 會通知它們會被視為一個群組的模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="0555a-236">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="0555a-237">每個核取方塊的 value 屬性的值設定為值`CourseID`。</span><span class="sxs-lookup"><span data-stu-id="0555a-237">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="0555a-238">當網頁回傳時，模型繫結器會將陣列傳遞至所組成的控制站`CourseID`只核取方塊已選取的值。</span><span class="sxs-lookup"><span data-stu-id="0555a-238">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="0555a-239">當一開始會呈現核取方塊時，為指派給講師課程的已簽屬性，以選取它們 （顯示其核取）。</span><span class="sxs-lookup"><span data-stu-id="0555a-239">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="0555a-240">執行 Instructor 索引頁面上，然後按一下 **編輯**上以查看講師**編輯**頁面。</span><span class="sxs-lookup"><span data-stu-id="0555a-240">Run the Instructor Index page, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![課程講師編輯頁面](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="0555a-242">變更一些課程作業，並按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="0555a-242">Change some course assignments and click Save.</span></span> <span data-ttu-id="0555a-243">所做的變更會反映在索引頁面。</span><span class="sxs-lookup"><span data-stu-id="0555a-243">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="0555a-244">沒有有限的數目的課程時，就會運作帶往這裡編輯講師課程資料的方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-244">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="0555a-245">對於更大的集合，不同的 UI 和不同的更新方法將會是必要。</span><span class="sxs-lookup"><span data-stu-id="0555a-245">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="0555a-246">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="0555a-246">Update the Delete page</span></span>

<span data-ttu-id="0555a-247">在*InstructorsController.cs*，刪除`DeleteConfirmed`方法並插入下列程式碼會在其位置。</span><span class="sxs-lookup"><span data-stu-id="0555a-247">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

<span data-ttu-id="0555a-248">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]</span><span class="sxs-lookup"><span data-stu-id="0555a-248">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]</span></span>

<span data-ttu-id="0555a-249">此程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="0555a-249">This code makes the following changes:</span></span>

* <span data-ttu-id="0555a-250">載入未 eager`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-250">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="0555a-251">EF 不了解相關或您必須加入此`CourseAssignment`實體並不會刪除它們。</span><span class="sxs-lookup"><span data-stu-id="0555a-251">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="0555a-252">若要避免需要讀取他們這裡您可以設定串聯刪除資料庫中。</span><span class="sxs-lookup"><span data-stu-id="0555a-252">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="0555a-253">如果要刪除講師被指派任何部門的系統管理員身分，移除這些部門講師指派。</span><span class="sxs-lookup"><span data-stu-id="0555a-253">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="0555a-254">將辦公室位置和課程加入至 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="0555a-254">Add office location and courses to the Create page</span></span>

<span data-ttu-id="0555a-255">在*InstructorsController.cs*，刪除 HttpGet 和 HttpPost`Create`方法，然後在適當位置中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="0555a-255">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

<span data-ttu-id="0555a-256">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]</span><span class="sxs-lookup"><span data-stu-id="0555a-256">[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]</span></span>

<span data-ttu-id="0555a-257">此程式碼是類似您為已看到`Edit`選取最初沒有課程除外的方法。</span><span class="sxs-lookup"><span data-stu-id="0555a-257">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="0555a-258">HttpGet`Create`方法呼叫`PopulateAssignedCourseData`方法不是因為可能有選取，但在課程，才能提供空的集合`foreach`（否則檢視程式碼會擲回 null 參考例外狀況） 的檢視中的迴圈。</span><span class="sxs-lookup"><span data-stu-id="0555a-258">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="0555a-259">HttpPost`Create`方法會加入至每個所選的課程`CourseAssignments`之前它會檢查是否有驗證錯誤，並在資料庫中加入新講師的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0555a-259">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="0555a-260">即使有模型錯誤，以便於當模型錯誤 （例如，做為索引鍵的日期不正確的使用者），而頁面會重新顯示一則錯誤訊息，會自動還原所做的任何課程選取項目，就會加入課程。</span><span class="sxs-lookup"><span data-stu-id="0555a-260">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="0555a-261">請注意，為了能夠新增至課程`CourseAssignments`導覽屬性，您必須初始化為空集合屬性：</span><span class="sxs-lookup"><span data-stu-id="0555a-261">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="0555a-262">替代執行此動作控制器的程式碼中，您無法操作講師模型中變更屬性 getter，來自動建立集合如果它不存在，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0555a-262">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

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

<span data-ttu-id="0555a-263">如果您修改`CourseAssignments`屬性如此一來，您可以移除控制器中的明確的屬性初始化程式碼。</span><span class="sxs-lookup"><span data-stu-id="0555a-263">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="0555a-264">在*Views/Instructor/Create.cshtml*、 新增辦公室位置 文字方塊和核取方塊之前送出按鈕的課程。</span><span class="sxs-lookup"><span data-stu-id="0555a-264">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="0555a-265">如果是 編輯頁面上，[修正格式時將它貼入 Visual Studio 重新格式化程式碼如果](#notepad)。</span><span class="sxs-lookup"><span data-stu-id="0555a-265">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

<span data-ttu-id="0555a-266">[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]</span><span class="sxs-lookup"><span data-stu-id="0555a-266">[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]</span></span>

<span data-ttu-id="0555a-267">藉由執行測試**建立**頁並加入講師。</span><span class="sxs-lookup"><span data-stu-id="0555a-267">Test by running the **Create** page and adding an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="0555a-268">處理交易</span><span class="sxs-lookup"><span data-stu-id="0555a-268">Handling Transactions</span></span>

<span data-ttu-id="0555a-269">中所述[CRUD 教學課程](crud.md)，Entity Framework 會實作隱含交易。</span><span class="sxs-lookup"><span data-stu-id="0555a-269">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="0555a-270">如案例，您需要更大的控制權-例如，如果您想要包含在交易-外面 Entity Framework 執行的作業，請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。</span><span class="sxs-lookup"><span data-stu-id="0555a-270">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="0555a-271">總結</span><span class="sxs-lookup"><span data-stu-id="0555a-271">Summary</span></span>

<span data-ttu-id="0555a-272">您現在已完成處理的相關資料的簡介。</span><span class="sxs-lookup"><span data-stu-id="0555a-272">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="0555a-273">在下一個教學課程中，您會看到如何處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="0555a-273">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0555a-274">[上一頁](read-related-data.md)
[下一頁](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="0555a-274">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
