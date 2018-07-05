---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 使用 ASP.NET MVC 應用程式 (10 個 6) 中的 Entity Framework 更新相關的資料 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371667"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a><span data-ttu-id="19046-103">使用 Entity Framework 的 ASP.NET MVC 應用程式 (10 個 6) 中更新相關的資料</span><span class="sxs-lookup"><span data-stu-id="19046-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application (6 of 10)</span></span>
====================
<span data-ttu-id="19046-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="19046-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="19046-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="19046-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="19046-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="19046-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="19046-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="19046-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="19046-108">您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="19046-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="19046-109">如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。</span><span class="sxs-lookup"><span data-stu-id="19046-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="19046-110">您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="19046-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="19046-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="19046-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="19046-112">您在先前的教學課程中顯示相關的資料;在本教學課程中，您將更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="19046-112">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="19046-113">大部分的關聯性，做法是藉由更新適當的外部索引鍵欄位。</span><span class="sxs-lookup"><span data-stu-id="19046-113">For most relationships, this can be done by updating the appropriate foreign key fields.</span></span> <span data-ttu-id="19046-114">多對多關聯性，Entity Framework 不會聯結資料表直接公開，因此您必須明確地新增，然後移除適當的導覽屬性的實體。</span><span class="sxs-lookup"><span data-stu-id="19046-114">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you must explicitly add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="19046-115">下列圖例顯示了您將操作的頁面。</span><span class="sxs-lookup"><span data-stu-id="19046-115">The following illustrations show the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="19046-118">自訂 Courses 的 [建立] 和 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="19046-118">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="19046-119">當新的課程實體建立時，其必須要與現有的部門具有關聯性。</span><span class="sxs-lookup"><span data-stu-id="19046-119">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="19046-120">若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="19046-120">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="19046-121">下拉式清單會設定`Course.DepartmentID`外部索引鍵屬性，就是這麼 Entity Framework 必須以載入`Department`導覽屬性，以適當`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-121">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="19046-122">您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="19046-122">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="19046-123">在  *CourseController.cs*，刪除四個`Edit`和`Create`方法，並以下列程式碼取代：</span><span class="sxs-lookup"><span data-stu-id="19046-123">In *CourseController.cs*, delete the four `Edit` and `Create` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

<span data-ttu-id="19046-124">`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`下拉式清單中，集合，並將集合傳遞至檢視中`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-124">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="19046-125">方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。</span><span class="sxs-lookup"><span data-stu-id="19046-125">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="19046-126">檢視會將名稱傳遞`DepartmentID`來[ `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)，並協助程式就會知道要尋找`ViewBag`物件`SelectList`名為`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="19046-126">The view will pass the name `DepartmentID` to [the `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="19046-127">`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新課程的部門還沒有建立的方法：</span><span class="sxs-lookup"><span data-stu-id="19046-127">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="19046-128">`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯之課程的部門識別碼：</span><span class="sxs-lookup"><span data-stu-id="19046-128">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="19046-129">`HttpPost`方法都`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：</span><span class="sxs-lookup"><span data-stu-id="19046-129">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="19046-130">此程式碼可確保，當頁面重新顯示以顯示錯誤訊息，已選取的任何部門保持選取。</span><span class="sxs-lookup"><span data-stu-id="19046-130">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="19046-131">在  *Views\Course\Create.cshtml*，新增反白顯示的程式碼，以建立新的課程號碼欄位之前**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="19046-131">In *Views\Course\Create.cshtml*, add the highlighted code to create a new course number field before the **Title** field.</span></span> <span data-ttu-id="19046-132">先前的教學課程所述，主索引鍵欄位不會進行 scaffold 根據預設，但此主索引鍵是有意義，因此您想讓使用者能夠輸入的索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="19046-132">As explained in an earlier tutorial, primary key fields aren't scaffolded by default, but this primary key is meaningful, so you want the user to be able to enter the key value.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

<span data-ttu-id="19046-133">在  *Views\Course\Edit.cshtml*， *Views\Course\Delete.cshtml*，並*Views\Course\Details.cshtml*，新增一個課程號碼欄位，再**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="19046-133">In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, and *Views\Course\Details.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="19046-134">因為它是主索引鍵時，它會顯示，但無法變更。</span><span class="sxs-lookup"><span data-stu-id="19046-134">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="19046-135">執行**Create**網頁 (顯示課程索引頁面，然後按一下**建立新**) 並輸入新的課程資料：</span><span class="sxs-lookup"><span data-stu-id="19046-135">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="19046-137">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="19046-137">Click **Create**.</span></span> <span data-ttu-id="19046-138">課程索引 頁面會顯示新的課程新增至清單。</span><span class="sxs-lookup"><span data-stu-id="19046-138">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="19046-139">[索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。</span><span class="sxs-lookup"><span data-stu-id="19046-139">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="19046-141">執行**編輯**網頁 (顯示課程索引頁面，然後按一下**編輯**課程)。</span><span class="sxs-lookup"><span data-stu-id="19046-141">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="19046-143">變更頁面上的資料，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="19046-143">Change data on the page and click **Save**.</span></span> <span data-ttu-id="19046-144">課程索引 頁面會顯示更新的課程資料。</span><span class="sxs-lookup"><span data-stu-id="19046-144">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="19046-145">新增講師 [編輯] 的頁面</span><span class="sxs-lookup"><span data-stu-id="19046-145">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="19046-146">當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="19046-146">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="19046-147">`Instructor`實體具有一對零-或-一關係`OfficeAssignment`實體，這表示您必須處理下列情況：</span><span class="sxs-lookup"><span data-stu-id="19046-147">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="19046-148">如果使用者清除辦公室指派，而且它原先擁有值，您必須移除並刪除`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-148">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="19046-149">如果使用者輸入辦公室指派值，而且它原先是空白，您必須建立新`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-149">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="19046-150">如果使用者變更辦公室指派的值時，您必須變更中的現有值`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-150">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="19046-151">開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：</span><span class="sxs-lookup"><span data-stu-id="19046-151">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="19046-152">Scaffold 的程式碼不是您所要的。</span><span class="sxs-lookup"><span data-stu-id="19046-152">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="19046-153">設定資料的下拉式清單中，但您需要的是文字方塊。</span><span class="sxs-lookup"><span data-stu-id="19046-153">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="19046-154">這個方法取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="19046-154">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="19046-155">此程式碼會卸除`ViewBag`陳述式，並將相關聯的積極式載入`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-155">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="19046-156">您無法執行使用積極式載入`Find`方法，因此`Where`和`Single`方法將改為用來選取講師。</span><span class="sxs-lookup"><span data-stu-id="19046-156">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="19046-157">取代`HttpPost``Edit`為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="19046-157">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="19046-158">處理辦公室指派更新：</span><span class="sxs-lookup"><span data-stu-id="19046-158">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="19046-159">程式碼會執行下列操作：</span><span class="sxs-lookup"><span data-stu-id="19046-159">The code does the following:</span></span>

- <span data-ttu-id="19046-160">針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="19046-160">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="19046-161">這是您未相同`HttpGet``Edit`方法。</span><span class="sxs-lookup"><span data-stu-id="19046-161">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="19046-162">使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="19046-162">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="19046-163">[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*允許清單*您想要包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-163">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="19046-164">這可防止大量指派，如所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="19046-164">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- <span data-ttu-id="19046-165">如果辦公室位置為空白，設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表會被刪除。</span><span class="sxs-lookup"><span data-stu-id="19046-165">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- <span data-ttu-id="19046-166">將變更儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="19046-166">Saves the changes to the database.</span></span>

<span data-ttu-id="19046-167">在  *Views\Instructor\Edit.cshtml*之後，`div`項目**雇用日期**欄位中，新增用於編輯辦公室位置的欄位：</span><span class="sxs-lookup"><span data-stu-id="19046-167">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

<span data-ttu-id="19046-168">執行網頁 (選取**講師**索引標籤，然後按一下**編輯**講師上)。</span><span class="sxs-lookup"><span data-stu-id="19046-168">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="19046-169">變更 [辦公室位置]，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="19046-169">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="19046-171">將課程指派給講師的編輯頁面</span><span class="sxs-lookup"><span data-stu-id="19046-171">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="19046-172">講師可教授任何數量的課程。</span><span class="sxs-lookup"><span data-stu-id="19046-172">Instructors may teach any number of courses.</span></span> <span data-ttu-id="19046-173">現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="19046-173">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="19046-175">之間的關聯性`Course`和`Instructor`實體是多對多，這表示您沒有直接對聯結資料表的存取。</span><span class="sxs-lookup"><span data-stu-id="19046-175">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the join table.</span></span> <span data-ttu-id="19046-176">相反地，您將在其中新增和移除實體，來回`Instructor.Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-176">Instead, you will add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="19046-177">可讓您變更講師指派之課程的 UI 為一組核取方塊。</span><span class="sxs-lookup"><span data-stu-id="19046-177">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="19046-178">資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。</span><span class="sxs-lookup"><span data-stu-id="19046-178">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="19046-179">使用者可選取或清除核取方塊來變更課程指派。</span><span class="sxs-lookup"><span data-stu-id="19046-179">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="19046-180">如果課程數目大許多，您可能想要使用不同的方法，可在檢視中，顯示資料，但您會使用相同的方法的操作才能建立或刪除關聯性的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-180">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="19046-181">若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="19046-181">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="19046-182">建立*Schoolviewmodels*中*Viewmodel*資料夾，並將現有的程式碼，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="19046-182">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="19046-183">在  *InstructorController.cs*，取代`HttpGet``Edit`為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="19046-183">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="19046-184">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="19046-184">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

<span data-ttu-id="19046-185">程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。</span><span class="sxs-lookup"><span data-stu-id="19046-185">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="19046-186">中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`為了載入一份使用檢視的課程實體模型類別。</span><span class="sxs-lookup"><span data-stu-id="19046-186">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="19046-187">針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。</span><span class="sxs-lookup"><span data-stu-id="19046-187">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="19046-188">若要檢查課程是否已指派給講師時，請建立有效率的查閱，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。</span><span class="sxs-lookup"><span data-stu-id="19046-188">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="19046-189">`Assigned`屬性設定為`true`指派的講師的課程。</span><span class="sxs-lookup"><span data-stu-id="19046-189">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="19046-190">檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。</span><span class="sxs-lookup"><span data-stu-id="19046-190">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="19046-191">最後，清單會傳遞至檢視中`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-191">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="19046-192">接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="19046-192">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="19046-193">取代`HttpPost``Edit`方法會呼叫新方法以更新為下列程式碼`Courses`導覽屬性`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="19046-193">Replace the `HttpPost` `Edit` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="19046-194">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="19046-194">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

<span data-ttu-id="19046-195">由於檢視沒有一堆`Course`自動更新實體，模型繫結無法`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-195">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="19046-196">而不是使用模型繫結來更新 Course 導覽屬性，您將執行在新的`UpdateInstructorCourses`方法。</span><span class="sxs-lookup"><span data-stu-id="19046-196">Instead of using the model binder to update the Courses navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="19046-197">因此您必須從模型繫結器中排除 `Courses` 屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-197">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="19046-198">這並不需要進行任何變更，要呼叫的程式碼[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您使用*列入白名單*多載和`Courses`不在 include 清單中。</span><span class="sxs-lookup"><span data-stu-id="19046-198">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="19046-199">如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`具有空的集合導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="19046-199">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="19046-200">程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。</span><span class="sxs-lookup"><span data-stu-id="19046-200">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="19046-201">為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。</span><span class="sxs-lookup"><span data-stu-id="19046-201">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="19046-202">若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。</span><span class="sxs-lookup"><span data-stu-id="19046-202">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="19046-203">若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。</span><span class="sxs-lookup"><span data-stu-id="19046-203">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="19046-204">在*Views\Instructor\Edit.cshtml*，新增**課程**欄位具有陣列中加入下列反白顯示的核取方塊的程式碼之後立即`div`項目`OfficeAssignment`欄位：</span><span class="sxs-lookup"><span data-stu-id="19046-204">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following highlighted code immediately after the `div` elements for the `OfficeAssignment` field:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

<span data-ttu-id="19046-205">此程式碼會建立一個 HTML 表格，該表格中有三個資料行。</span><span class="sxs-lookup"><span data-stu-id="19046-205">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="19046-206">在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。</span><span class="sxs-lookup"><span data-stu-id="19046-206">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="19046-207">核取方塊都具有相同名稱 ("selectedCourses") 會告知模型繫結，它們會被視為一個群組。</span><span class="sxs-lookup"><span data-stu-id="19046-207">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="19046-208">`value`的值設定屬性的每個核取方塊`CourseID.`當頁面回傳時，模型繫結會將陣列傳遞至控制器所組成`CourseID`只核取方塊已選取的值。</span><span class="sxs-lookup"><span data-stu-id="19046-208">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="19046-209">當一開始呈現核取方塊時，即指派給講師的課程有`checked`選取 （顯示它們已核取） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="19046-209">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="19046-210">變更課程指派之後, 您會想要能夠驗證所做的變更，當站台恢復`Index`頁面。</span><span class="sxs-lookup"><span data-stu-id="19046-210">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="19046-211">因此，您需要將資料行新增至該頁面中的資料表。</span><span class="sxs-lookup"><span data-stu-id="19046-211">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="19046-212">在此情況下，您不需要使用`ViewBag`物件，因為已在您想要顯示的資訊`Courses`導覽屬性`Instructor`您要將它傳遞給頁面做為模型的實體。</span><span class="sxs-lookup"><span data-stu-id="19046-212">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="19046-213">在  *Views\Instructor\Index.cshtml*，新增**課程**標題的正後方**Office**標題之下，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="19046-213">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

<span data-ttu-id="19046-214">然後，新增新的詳細資料格，緊接著辦公室位置的詳細資料格：</span><span class="sxs-lookup"><span data-stu-id="19046-214">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

<span data-ttu-id="19046-215">執行**Instructor 索引**頁面，以查看指派給每位講師的課程：</span><span class="sxs-lookup"><span data-stu-id="19046-215">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="19046-217">按一下 **編輯**講師，以查看 編輯 頁面上。</span><span class="sxs-lookup"><span data-stu-id="19046-217">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="19046-219">變更一些課程指派，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="19046-219">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="19046-220">您所做的變更會反映在 [索引] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="19046-220">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="19046-221">注意： 來編輯講師課程資料的方法時運作相當良好有限的數目的課程。</span><span class="sxs-lookup"><span data-stu-id="19046-221">Note: The approach taken to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="19046-222">針對更大的集合，將需要不同的 UI 和不同的更新方法。</span><span class="sxs-lookup"><span data-stu-id="19046-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-delete-method"></a><span data-ttu-id="19046-223">更新的 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="19046-223">Update the Delete Method</span></span>

<span data-ttu-id="19046-224">因此辦公室指派記錄 （如果有的話） 刪除時刪除講師時，請變更則 HttpPost Delete 方法中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="19046-224">Change the code in the HttpPost Delete method so the office assignment record (if any) is deleted when the instructor is deleted:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


<span data-ttu-id="19046-225">如果您嘗試刪除講師角色，獲指派給某個部門系統管理員身分，您會收到參考完整性錯誤。</span><span class="sxs-lookup"><span data-stu-id="19046-225">If you try to delete an instructor who is assigned to a department as administrator, you'll get a referential integrity error.</span></span> <span data-ttu-id="19046-226">請參閱[目前的版本，本教學課程的](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)的額外程式碼會自動移除講師指派為系統管理員的其中任何部門的講師。</span><span class="sxs-lookup"><span data-stu-id="19046-226">See [the current version of this tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) for additional code that will automatically remove the instructor from any department where the instructor is assigned as an administrator.</span></span>

## <a name="summary"></a><span data-ttu-id="19046-227">總結</span><span class="sxs-lookup"><span data-stu-id="19046-227">Summary</span></span>

<span data-ttu-id="19046-228">您現在已完成此操作相關資料的簡介。</span><span class="sxs-lookup"><span data-stu-id="19046-228">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="19046-229">到目前為止在這些教學課程中，您已完成完整的 CRUD 作業，但您還沒有處理的並行處理問題。</span><span class="sxs-lookup"><span data-stu-id="19046-229">So far in these tutorials you've done a full range of CRUD operations, but you haven't dealt with concurrency issues.</span></span> <span data-ttu-id="19046-230">下一個教學課程會介紹並行存取的主題、 說明選項來處理它，並新增您已撰寫一個實體類型的 CRUD 程式碼處理的並行存取。</span><span class="sxs-lookup"><span data-stu-id="19046-230">The next tutorial will introduce the topic of concurrency, explain options for handling it, and add concurrency handling to the CRUD code you've already written for one entity type.</span></span>

<span data-ttu-id="19046-231">其他 Entity Framework 資源的連結可以找到在結尾[這個系列的最後一個教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。</span><span class="sxs-lookup"><span data-stu-id="19046-231">Links to other Entity Framework resources, can be found at the end of [the last tutorial in this series](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="19046-232">[上一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="19046-232">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
