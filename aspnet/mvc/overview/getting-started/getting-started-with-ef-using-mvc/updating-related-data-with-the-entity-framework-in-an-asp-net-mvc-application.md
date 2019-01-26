---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：ASP.NET MVC 應用程式中，使用 EF 更新相關的資料
description: 在本教學課程中，您將更新相關的資料。 大部分的關聯性，做法是藉由更新外部索引鍵欄位或導覽屬性。
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: daefbcea39dc341c6fc72eb344b05eb2dbf0c3ba
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889843"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="1ec59-104">教學課程：ASP.NET MVC 應用程式中，使用 EF 更新相關的資料</span><span class="sxs-lookup"><span data-stu-id="1ec59-104">Tutorial: Update related data with EF in an ASP.NET MVC app</span></span>

<span data-ttu-id="1ec59-105">在上一個教學課程中，您會顯示相關的資料。</span><span class="sxs-lookup"><span data-stu-id="1ec59-105">In the previous tutorial you displayed related data.</span></span> <span data-ttu-id="1ec59-106">在本教學課程中，您將更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="1ec59-106">In this tutorial you'll update related data.</span></span> <span data-ttu-id="1ec59-107">大部分的關聯性，做法是藉由更新外部索引鍵欄位或導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-107">For most relationships, this can be done by updating either foreign key fields or navigation properties.</span></span> <span data-ttu-id="1ec59-108">多對多關聯性，Entity Framework 不會聯結資料表直接公開，讓您新增和移除適當的導覽屬性的實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-108">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="1ec59-109">下列圖例顯示了您將操作的一些頁面。</span><span class="sxs-lookup"><span data-stu-id="1ec59-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![編輯講師課程](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="1ec59-113">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="1ec59-113">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1ec59-114">自訂 [課程] 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-114">Customize courses pages</span></span>
> * <span data-ttu-id="1ec59-115">將 office 新增至講師頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-115">Add office to instructors page</span></span>
> * <span data-ttu-id="1ec59-116">將課程新增至講師頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-116">Add courses to instructors page</span></span>
> * <span data-ttu-id="1ec59-117">更新 DeleteConfirmed</span><span class="sxs-lookup"><span data-stu-id="1ec59-117">Update DeleteConfirmed</span></span>
> * <span data-ttu-id="1ec59-118">將辦公室位置和課程新增至 [新增] 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-118">Add office location and courses to the Create page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ec59-119">必要條件</span><span class="sxs-lookup"><span data-stu-id="1ec59-119">Prerequisites</span></span>

* [<span data-ttu-id="1ec59-120">讀取相關資料</span><span class="sxs-lookup"><span data-stu-id="1ec59-120">Reading Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a><span data-ttu-id="1ec59-121">自訂 [課程] 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-121">Customize courses pages</span></span>

<span data-ttu-id="1ec59-122">當新的課程實體建立時，其必須要與現有的部門具有關聯性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="1ec59-123">若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="1ec59-123">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="1ec59-124">下拉式清單會設定`Course.DepartmentID`外部索引鍵屬性，就是這麼 Entity Framework 必須以載入`Department`導覽屬性，以適當`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-124">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="1ec59-125">您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="1ec59-125">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="1ec59-126">在  *CourseController.cs*，刪除四個`Create`和`Edit`方法，並以下列程式碼取代：</span><span class="sxs-lookup"><span data-stu-id="1ec59-126">In *CourseController.cs*, delete the four `Create` and `Edit` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

<span data-ttu-id="1ec59-127">新增下列`using`檔案的開頭的陳述式：</span><span class="sxs-lookup"><span data-stu-id="1ec59-127">Add the following `using` statement at the beginning of the file:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="1ec59-128">`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`下拉式清單中，集合，並將集合傳遞至檢視中`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-128">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="1ec59-129">方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。</span><span class="sxs-lookup"><span data-stu-id="1ec59-129">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="1ec59-130">檢視會將名稱傳遞`DepartmentID`要[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)協助程式，並協助程式就會知道要尋找`ViewBag`物件`SelectList`名為`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="1ec59-130">The view will pass the name `DepartmentID` to the [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="1ec59-131">`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新課程的部門還沒有建立的方法：</span><span class="sxs-lookup"><span data-stu-id="1ec59-131">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="1ec59-132">`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯之課程的部門識別碼：</span><span class="sxs-lookup"><span data-stu-id="1ec59-132">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

<span data-ttu-id="1ec59-133">`HttpPost`方法都`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：</span><span class="sxs-lookup"><span data-stu-id="1ec59-133">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

<span data-ttu-id="1ec59-134">此程式碼可確保，當頁面重新顯示以顯示錯誤訊息，已選取的任何部門保持選取。</span><span class="sxs-lookup"><span data-stu-id="1ec59-134">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="1ec59-135">下拉式清單中 [部門] 欄位中，已經 scaffold Course 檢視但不想 DepartmentID 標題此欄位中，以便進行以下反白顯示變更為*Views\Course\Create.cshtml*檔案變更標題。</span><span class="sxs-lookup"><span data-stu-id="1ec59-135">The Course views are already scaffolded with drop-down lists for the department field, but you don't want the DepartmentID caption for this field, so make the following highlighted change to the *Views\Course\Create.cshtml* file to change the caption.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

<span data-ttu-id="1ec59-136">請在相同的變更*Views\Course\Edit.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1ec59-136">Make the same change in *Views\Course\Edit.cshtml*.</span></span>

<span data-ttu-id="1ec59-137">通常框架不會 scaffold 主索引鍵，因為索引鍵的值由資料庫產生和無法變更，而且不會向使用者顯示有意義的值。</span><span class="sxs-lookup"><span data-stu-id="1ec59-137">Normally the scaffolder doesn't scaffold a primary key because the key value is generated by the database and can't be changed and isn't a meaningful value to be displayed to users.</span></span> <span data-ttu-id="1ec59-138">針對 Course 實體框架包含文字方塊，讓`CourseID`欄位，因為它了解`DatabaseGeneratedOption.None`屬性表示的使用者應該可以輸入主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="1ec59-138">For Course entities the scaffolder does include an text box for the `CourseID` field because it understands that the `DatabaseGeneratedOption.None` attribute means the user should be able enter the primary key value.</span></span> <span data-ttu-id="1ec59-139">但它並不了解有意義的數字，是因為您想要看到它在其他檢視中，因此您必須手動將它加入。</span><span class="sxs-lookup"><span data-stu-id="1ec59-139">But it doesn't understand that because the number is meaningful you want to see it in the other views, so you need to add it manually.</span></span>

<span data-ttu-id="1ec59-140">在  *Views\Course\Edit.cshtml*，新增一個課程號碼欄位，再**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="1ec59-140">In *Views\Course\Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="1ec59-141">因為它是主索引鍵時，它會顯示，但無法變更。</span><span class="sxs-lookup"><span data-stu-id="1ec59-141">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

<span data-ttu-id="1ec59-142">已經有隱藏的欄位 (`Html.HiddenFor`協助程式) 在 [編輯] 檢視中將課程編號。</span><span class="sxs-lookup"><span data-stu-id="1ec59-142">There's already a hidden field (`Html.HiddenFor` helper) for the course number in the Edit view.</span></span> <span data-ttu-id="1ec59-143">新增*Html.LabelFor*協助程式無法消除隱藏欄位的需求，因為它無法讓課程號碼包含在張貼的資料，當使用者按一下**儲存**編輯 頁面上。</span><span class="sxs-lookup"><span data-stu-id="1ec59-143">Adding an *Html.LabelFor* helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the Edit page.</span></span>

<span data-ttu-id="1ec59-144">在  *Views\Course\Delete.cshtml*並*Views\Course\Details.cshtml*、 變更 「 部門 」 從 「 名稱 」 的部門名稱標題以及加入前的一個課程號碼欄位**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="1ec59-144">In *Views\Course\Delete.cshtml* and *Views\Course\Details.cshtml*, change the department name caption from "Name" to "Department" and add a course number field before the **Title** field.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

<span data-ttu-id="1ec59-145">執行**Create**網頁 (顯示課程索引頁面，然後按一下**建立新**) 並輸入新的課程資料：</span><span class="sxs-lookup"><span data-stu-id="1ec59-145">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

| <span data-ttu-id="1ec59-146">值</span><span class="sxs-lookup"><span data-stu-id="1ec59-146">Value</span></span> | <span data-ttu-id="1ec59-147">設定</span><span class="sxs-lookup"><span data-stu-id="1ec59-147">Setting</span></span> |
| ----- | ------- |
| <span data-ttu-id="1ec59-148">number</span><span class="sxs-lookup"><span data-stu-id="1ec59-148">Number</span></span> | <span data-ttu-id="1ec59-149">請輸入*1000年*。</span><span class="sxs-lookup"><span data-stu-id="1ec59-149">Enter *1000*.</span></span> |
| <span data-ttu-id="1ec59-150">標題</span><span class="sxs-lookup"><span data-stu-id="1ec59-150">Title</span></span> | <span data-ttu-id="1ec59-151">請輸入*代數*。</span><span class="sxs-lookup"><span data-stu-id="1ec59-151">Enter *Algebra*.</span></span> |
| <span data-ttu-id="1ec59-152">參與名單</span><span class="sxs-lookup"><span data-stu-id="1ec59-152">Credits</span></span> | <span data-ttu-id="1ec59-153">請輸入*4*。</span><span class="sxs-lookup"><span data-stu-id="1ec59-153">Enter *4*.</span></span> |
|<span data-ttu-id="1ec59-154">部門</span><span class="sxs-lookup"><span data-stu-id="1ec59-154">Department</span></span> | <span data-ttu-id="1ec59-155">選取 **數學**。</span><span class="sxs-lookup"><span data-stu-id="1ec59-155">Select **Mathematics**.</span></span> |

<span data-ttu-id="1ec59-156">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="1ec59-156">Click **Create**.</span></span> <span data-ttu-id="1ec59-157">課程索引 頁面會顯示新的課程新增至清單。</span><span class="sxs-lookup"><span data-stu-id="1ec59-157">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="1ec59-158">[索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。</span><span class="sxs-lookup"><span data-stu-id="1ec59-158">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="1ec59-159">執行**編輯**網頁 (顯示課程索引頁面，然後按一下**編輯**課程)。</span><span class="sxs-lookup"><span data-stu-id="1ec59-159">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

<span data-ttu-id="1ec59-160">變更頁面上的資料，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="1ec59-160">Change data on the page and click **Save**.</span></span> <span data-ttu-id="1ec59-161">課程索引 頁面會顯示更新的課程資料。</span><span class="sxs-lookup"><span data-stu-id="1ec59-161">The Course Index page is displayed with the updated course data.</span></span>

## <a name="add-office-to-instructors-page"></a><span data-ttu-id="1ec59-162">將 office 新增至講師頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-162">Add office to instructors page</span></span>

<span data-ttu-id="1ec59-163">當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="1ec59-163">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="1ec59-164">`Instructor`實體具有一對零-或-一關係`OfficeAssignment`實體，這表示您必須處理下列情況：</span><span class="sxs-lookup"><span data-stu-id="1ec59-164">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="1ec59-165">如果使用者清除辦公室指派，而且它原先擁有值，您必須移除並刪除`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-165">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="1ec59-166">如果使用者輸入辦公室指派值，而且它原先是空白，您必須建立新`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-166">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="1ec59-167">如果使用者變更辦公室指派的值時，您必須變更中的現有值`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-167">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="1ec59-168">開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：</span><span class="sxs-lookup"><span data-stu-id="1ec59-168">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="1ec59-169">Scaffold 的程式碼不是您所要的。</span><span class="sxs-lookup"><span data-stu-id="1ec59-169">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="1ec59-170">設定資料的下拉式清單中，但您需要的是文字方塊。</span><span class="sxs-lookup"><span data-stu-id="1ec59-170">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="1ec59-171">這個方法取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="1ec59-171">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

<span data-ttu-id="1ec59-172">此程式碼會卸除`ViewBag`陳述式，並將相關聯的積極式載入`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-172">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="1ec59-173">您無法執行使用積極式載入`Find`方法，因此`Where`和`Single`方法將改為用來選取講師。</span><span class="sxs-lookup"><span data-stu-id="1ec59-173">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="1ec59-174">取代`HttpPost``Edit`為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="1ec59-174">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="1ec59-175">處理辦公室指派更新：</span><span class="sxs-lookup"><span data-stu-id="1ec59-175">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="1ec59-176">若要參考`RetryLimitExceededException`需要`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="1ec59-176">The reference to `RetryLimitExceededException` requires a `using` statement.</span></span> <span data-ttu-id="1ec59-177">若要將它加入，停留`RetryLimitExceededException`。</span><span class="sxs-lookup"><span data-stu-id="1ec59-177">To add it, hover over `RetryLimitExceededException`.</span></span> <span data-ttu-id="1ec59-178">此時會出現此問題的說明。</span><span class="sxs-lookup"><span data-stu-id="1ec59-178">An explanation of the issue appears.</span></span> <span data-ttu-id="1ec59-179">選取 **顯示可能的修正**，然後按一下**使用 System.Data.Entity.Infrastructure;**。</span><span class="sxs-lookup"><span data-stu-id="1ec59-179">Select **Show potential fixes** and then click **using System.Data.Entity.Infrastructure;**.</span></span>

![解析重試例外狀況](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="1ec59-181">程式碼會執行下列操作：</span><span class="sxs-lookup"><span data-stu-id="1ec59-181">The code does the following:</span></span>

- <span data-ttu-id="1ec59-182">將方法名稱變更為`EditPost`因為簽章現在相同`HttpGet`方法 (`ActionName`屬性會指定 /Edit/ URL 仍在使用)。</span><span class="sxs-lookup"><span data-stu-id="1ec59-182">Changes the method name to `EditPost` because the signature is now the same as the `HttpGet` method (the `ActionName` attribute specifies that the /Edit/ URL is still used).</span></span>
- <span data-ttu-id="1ec59-183">針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-183">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="1ec59-184">這是您未相同`HttpGet``Edit`方法。</span><span class="sxs-lookup"><span data-stu-id="1ec59-184">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="1ec59-185">使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="1ec59-186">[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*允許清單*您想要包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-186">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="1ec59-187">這可防止大量指派，如所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="1ec59-187">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- <span data-ttu-id="1ec59-188">如果辦公室位置為空白，設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表會被刪除。</span><span class="sxs-lookup"><span data-stu-id="1ec59-188">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- <span data-ttu-id="1ec59-189">將變更儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="1ec59-189">Saves the changes to the database.</span></span>

<span data-ttu-id="1ec59-190">在  *Views\Instructor\Edit.cshtml*之後，`div`項目**雇用日期**欄位中，新增用於編輯辦公室位置的欄位：</span><span class="sxs-lookup"><span data-stu-id="1ec59-190">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

<span data-ttu-id="1ec59-191">執行網頁 (選取**講師**索引標籤，然後按一下**編輯**講師上)。</span><span class="sxs-lookup"><span data-stu-id="1ec59-191">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="1ec59-192">變更 [辦公室位置]，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="1ec59-192">Change the **Office Location** and click **Save**.</span></span>

## <a name="add-courses-to-instructors-page"></a><span data-ttu-id="1ec59-193">將課程新增至講師頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-193">Add courses to instructors page</span></span>

<span data-ttu-id="1ec59-194">講師可教授任何數量的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-194">Instructors may teach any number of courses.</span></span> <span data-ttu-id="1ec59-195">現在您將會增強 Instructor [編輯] 頁面，藉由新增變更課程指派使用核取方塊群組的能力。</span><span class="sxs-lookup"><span data-stu-id="1ec59-195">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes.</span></span>

<span data-ttu-id="1ec59-196">之間的關聯性`Course`和`Instructor`實體是多對多，這表示您沒有直接存取中的聯結資料表的外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-196">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the foreign key properties which are in the join table.</span></span> <span data-ttu-id="1ec59-197">相反地，您新增和移除實體，來回`Instructor.Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-197">Instead, you add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="1ec59-198">可讓您變更講師指派之課程的 UI 為一組核取方塊。</span><span class="sxs-lookup"><span data-stu-id="1ec59-198">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="1ec59-199">資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。</span><span class="sxs-lookup"><span data-stu-id="1ec59-199">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="1ec59-200">使用者可選取或清除核取方塊來變更課程指派。</span><span class="sxs-lookup"><span data-stu-id="1ec59-200">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="1ec59-201">如果課程數目大許多，您可能想要使用不同的方法，可在檢視中，顯示資料，但您會使用相同的方法的操作才能建立或刪除關聯性的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-201">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="1ec59-202">若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="1ec59-202">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="1ec59-203">建立*Schoolviewmodels*中*Viewmodel*資料夾，並將現有的程式碼，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="1ec59-203">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="1ec59-204">在  *InstructorController.cs*，取代`HttpGet``Edit`為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="1ec59-204">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="1ec59-205">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="1ec59-205">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

<span data-ttu-id="1ec59-206">程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。</span><span class="sxs-lookup"><span data-stu-id="1ec59-206">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="1ec59-207">中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`為了載入一份使用檢視的課程實體模型類別。</span><span class="sxs-lookup"><span data-stu-id="1ec59-207">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="1ec59-208">針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。</span><span class="sxs-lookup"><span data-stu-id="1ec59-208">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="1ec59-209">若要檢查課程是否已指派給講師時，請建立有效率的查閱，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。</span><span class="sxs-lookup"><span data-stu-id="1ec59-209">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="1ec59-210">`Assigned`屬性設定為`true`指派的講師的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-210">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="1ec59-211">檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。</span><span class="sxs-lookup"><span data-stu-id="1ec59-211">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="1ec59-212">最後，清單會傳遞至檢視中`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-212">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="1ec59-213">接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1ec59-213">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="1ec59-214">取代`EditPost`方法會呼叫新方法以更新為下列程式碼`Courses`導覽屬性`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-214">Replace the `EditPost` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="1ec59-215">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="1ec59-215">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

<span data-ttu-id="1ec59-216">方法簽章現在是不同於`HttpGet``Edit`方法，所以方法名稱會從`EditPost`回到`Edit`。</span><span class="sxs-lookup"><span data-stu-id="1ec59-216">The method signature is now different from the `HttpGet` `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="1ec59-217">由於檢視沒有一堆`Course`自動更新實體，模型繫結無法`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-217">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="1ec59-218">而不是使用更新的模型繫結`Courses`導覽屬性，您將執行，在新`UpdateInstructorCourses`方法。</span><span class="sxs-lookup"><span data-stu-id="1ec59-218">Instead of using the model binder to update the `Courses` navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="1ec59-219">因此您必須從模型繫結器中排除 `Courses` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-219">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="1ec59-220">這並不需要進行任何變更，要呼叫的程式碼[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您使用*列入允許清單*多載和`Courses`不在 include 清單中。</span><span class="sxs-lookup"><span data-stu-id="1ec59-220">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="1ec59-221">如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`具有空的集合導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="1ec59-221">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="1ec59-222">程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-222">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="1ec59-223">為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。</span><span class="sxs-lookup"><span data-stu-id="1ec59-223">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="1ec59-224">若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。</span><span class="sxs-lookup"><span data-stu-id="1ec59-224">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="1ec59-225">若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。</span><span class="sxs-lookup"><span data-stu-id="1ec59-225">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="1ec59-226">中*Views\Instructor\Edit.cshtml*，新增**課程**欄位陣列的核取方塊，新增下列程式碼之後立即`div`項目`OfficeAssignment`欄位和再`div`項目**儲存**按鈕：</span><span class="sxs-lookup"><span data-stu-id="1ec59-226">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the `OfficeAssignment` field and before the `div` element for the **Save** button:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

<span data-ttu-id="1ec59-227">貼上之後的程式碼，如果分行符號和縮排看起來不太一樣這裡，手動修正所有項目，使它看起來像這裡所示。</span><span class="sxs-lookup"><span data-stu-id="1ec59-227">After you paste the code, if line breaks and indentation don't look like they do here, manually fix everything so that it looks like what you see here.</span></span> <span data-ttu-id="1ec59-228">縮排不一定要是完美的，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@</tr>` 必須要如顯示般各自在獨立的一行上，否則您會接收到執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="1ec59-228">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span>

<span data-ttu-id="1ec59-229">此程式碼會建立一個 HTML 表格，該表格中有三個資料行。</span><span class="sxs-lookup"><span data-stu-id="1ec59-229">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="1ec59-230">在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。</span><span class="sxs-lookup"><span data-stu-id="1ec59-230">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="1ec59-231">核取方塊都具有相同名稱 ("selectedCourses") 會告知模型繫結，它們會被視為一個群組。</span><span class="sxs-lookup"><span data-stu-id="1ec59-231">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="1ec59-232">`value`的值設定屬性的每個核取方塊`CourseID.`當頁面回傳時，模型繫結會將陣列傳遞至控制器所組成`CourseID`只核取方塊已選取的值。</span><span class="sxs-lookup"><span data-stu-id="1ec59-232">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="1ec59-233">當一開始呈現核取方塊時，即指派給講師的課程有`checked`選取 （顯示它們已核取） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="1ec59-233">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="1ec59-234">變更課程指派之後, 您會想要能夠驗證所做的變更，當站台恢復`Index`頁面。</span><span class="sxs-lookup"><span data-stu-id="1ec59-234">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="1ec59-235">因此，您需要將資料行新增至該頁面中的資料表。</span><span class="sxs-lookup"><span data-stu-id="1ec59-235">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="1ec59-236">在此情況下，您不需要使用`ViewBag`物件，因為已在您想要顯示的資訊`Courses`導覽屬性`Instructor`您要將它傳遞給頁面做為模型的實體。</span><span class="sxs-lookup"><span data-stu-id="1ec59-236">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="1ec59-237">在  *Views\Instructor\Index.cshtml*，新增**課程**標題的正後方**Office**標題之下，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="1ec59-237">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

<span data-ttu-id="1ec59-238">然後，新增新的詳細資料格，緊接著辦公室位置的詳細資料格：</span><span class="sxs-lookup"><span data-stu-id="1ec59-238">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

<span data-ttu-id="1ec59-239">執行**Instructor 索引**頁面，以查看指派給每位講師的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-239">Run the **Instructor Index** page to see the courses assigned to each instructor.</span></span>

<span data-ttu-id="1ec59-240">按一下 **編輯**講師，以查看 編輯 頁面上。</span><span class="sxs-lookup"><span data-stu-id="1ec59-240">Click **Edit** on an instructor to see the Edit page.</span></span>

<span data-ttu-id="1ec59-241">變更一些課程指派，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="1ec59-241">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="1ec59-242">您所做的變更會反映在 [索引] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="1ec59-242">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="1ec59-243">注意:這裡的方法來編輯講師課程資料時運作相當良好有限的數目的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-243">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="1ec59-244">針對更大的集合，將需要不同的 UI 和不同的更新方法。</span><span class="sxs-lookup"><span data-stu-id="1ec59-244">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-deleteconfirmed"></a><span data-ttu-id="1ec59-245">更新 DeleteConfirmed</span><span class="sxs-lookup"><span data-stu-id="1ec59-245">Update DeleteConfirmed</span></span>

<span data-ttu-id="1ec59-246">在  *InstructorController.cs*，刪除`DeleteConfirmed`方法，然後插入下列程式碼加以取代。</span><span class="sxs-lookup"><span data-stu-id="1ec59-246">In *InstructorController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

<span data-ttu-id="1ec59-247">此程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="1ec59-247">This code makes the following change:</span></span>

- <span data-ttu-id="1ec59-248">如果講師指派任何部門的系統管理員身分，請從該部門中移除講師的指派。</span><span class="sxs-lookup"><span data-stu-id="1ec59-248">If the instructor is assigned as administrator of any department, removes the instructor assignment from that department.</span></span> <span data-ttu-id="1ec59-249">不需要此程式碼中，您會取得參考完整性錯誤，如果您嘗試刪除某部門的系統管理員身分指派的講師。</span><span class="sxs-lookup"><span data-stu-id="1ec59-249">Without this code, you would get a referential integrity error if you tried to delete an instructor who was assigned as administrator for a department.</span></span>

<span data-ttu-id="1ec59-250">此程式碼不會處理一位講師指派多個部門的系統管理員身分的案例。</span><span class="sxs-lookup"><span data-stu-id="1ec59-250">This code doesn't handle the scenario of one instructor assigned as administrator for multiple departments.</span></span> <span data-ttu-id="1ec59-251">在最後一個教學課程中，您將新增程式碼，可防止發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="1ec59-251">In the last tutorial you'll add code that prevents that scenario from happening.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="1ec59-252">將辦公室位置和課程新增至 [新增] 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-252">Add office location and courses to the Create page</span></span>

<span data-ttu-id="1ec59-253">在  *InstructorController.cs*，刪除`HttpGet`並`HttpPost``Create`方法，然後在適當位置中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="1ec59-253">In *InstructorController.cs*, delete the `HttpGet` and `HttpPost` `Create` methods, and then add the following code in their place:</span></span>


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="1ec59-254">此程式碼很類似您所見的 Edit 方法的不同之處在於一開始會選取任何課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-254">This code is similar to what you saw for the Edit methods except that initially no courses are selected.</span></span> <span data-ttu-id="1ec59-255">`HttpGet` `Create`方法呼叫`PopulateAssignedCourseData`方法不是因為可能有選取，但在課程是為了提供空集合給`foreach`（否則檢視程式碼會擲回 null 參考例外狀況的檢視中的迴圈).</span><span class="sxs-lookup"><span data-stu-id="1ec59-255">The `HttpGet` `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="1ec59-256">HttpPost Create 方法會將之前的範本程式碼來檢查驗證錯誤，並在資料庫中加入新的講師的課程導覽屬性中的每個所選的課程。</span><span class="sxs-lookup"><span data-stu-id="1ec59-256">The HttpPost Create method adds each selected course to the Courses navigation property before the template code that checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="1ec59-257">即使沒有模型錯誤，會加入課程，讓模型錯誤 （例如使用者鍵入了無效的日期） 時所做的任何課程選取項目，因此當頁面重新顯示並出現錯誤訊息，都會自動還原。</span><span class="sxs-lookup"><span data-stu-id="1ec59-257">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date) so that when the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="1ec59-258">請注意，為了要能夠將課程新增到 `Courses` 導覽屬性，您必須將屬性以空集合初始化：</span><span class="sxs-lookup"><span data-stu-id="1ec59-258">Notice that in order to be able to add courses to the `Courses` navigation property you have to initialize the property as an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="1ec59-259">作為在控制器程式碼中完成這項操作的替代方案，您可以在 Instructor 模型中藉由將屬性 getter 變更為在不存在時自動建立集合來完成，如以下範例所示：</span><span class="sxs-lookup"><span data-stu-id="1ec59-259">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="1ec59-260">若您使用這種方式修改了 `Courses` 屬性，您便可以移除控制器中的明確屬性初始化程式碼。</span><span class="sxs-lookup"><span data-stu-id="1ec59-260">If you modify the `Courses` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="1ec59-261">在  *Views\Instructor\Create.cshtml*、 新增辦公室位置文字方塊及課程核取方塊，雇用日期 欄位之後，以及之前**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1ec59-261">In *Views\Instructor\Create.cshtml*, add an office location text box and course check boxes after the hire date field and before the **Submit** button.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

<span data-ttu-id="1ec59-262">貼上程式碼之後，修正換行和縮排，如先前所做的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="1ec59-262">After you paste the code, fix line breaks and indentation as you did earlier for the Edit page.</span></span>

<span data-ttu-id="1ec59-263">執行 [建立] 頁面，並新增一名講師。</span><span class="sxs-lookup"><span data-stu-id="1ec59-263">Run the Create page and add an instructor.</span></span>

<a id="transactions"></a>

## <a name="handling-transactions"></a><span data-ttu-id="1ec59-264">處理交易</span><span class="sxs-lookup"><span data-stu-id="1ec59-264">Handling transactions</span></span>

<span data-ttu-id="1ec59-265">中所述[基本的 CRUD 功能教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)，預設的 Entity Framework 隱含實作了交易。</span><span class="sxs-lookup"><span data-stu-id="1ec59-265">As explained in the [Basic CRUD Functionality tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), by default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="1ec59-266">如案例，您需要更多控制，例如，如果您想要包含在交易-Entity Framework 之外完成的作業，請參閱[使用交易](https://msdn.microsoft.com/data/dn456843)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="1ec59-266">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Working with Transactions](https://msdn.microsoft.com/data/dn456843) on MSDN.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="1ec59-267">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="1ec59-267">Get the code</span></span>

[<span data-ttu-id="1ec59-268">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="1ec59-268">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="1ec59-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="1ec59-269">Additional resources</span></span>

<span data-ttu-id="1ec59-270">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="1ec59-270">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="1ec59-271">後續步驟</span><span class="sxs-lookup"><span data-stu-id="1ec59-271">Next step</span></span>

<span data-ttu-id="1ec59-272">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="1ec59-272">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1ec59-273">自訂的課程 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-273">Customized courses pages</span></span>
> * <span data-ttu-id="1ec59-274">新增的辦公室 instructors 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-274">Added office to instructors page</span></span>
> * <span data-ttu-id="1ec59-275">已新增至講師頁面的課程</span><span class="sxs-lookup"><span data-stu-id="1ec59-275">Added courses to instructors page</span></span>
> * <span data-ttu-id="1ec59-276">更新的 DeleteConfirmed</span><span class="sxs-lookup"><span data-stu-id="1ec59-276">Updated DeleteConfirmed</span></span>
> * <span data-ttu-id="1ec59-277">新增的辦公室位置和課程，[建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="1ec59-277">Added office location and courses to the Create page</span></span>

<span data-ttu-id="1ec59-278">請前往下一篇文章，以了解如何實作非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="1ec59-278">Advance to the next article to learn how to implement an asynchronous programming model.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="1ec59-279">非同步程式設計模型</span><span class="sxs-lookup"><span data-stu-id="1ec59-279">Asynchronous programming model</span></span>](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
