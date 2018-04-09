---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 更新相關的資料與 Entity Framework 中的 ASP.NET MVC 應用程式 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="72fff-103">Entity Framework 中的 ASP.NET MVC 應用程式以更新相關的資料</span><span class="sxs-lookup"><span data-stu-id="72fff-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="72fff-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="72fff-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="72fff-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="72fff-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="72fff-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72fff-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="72fff-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="72fff-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="72fff-108">您可以在上一個教學課程中顯示相關的資料。在本教學課程中，您要更新的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="72fff-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="72fff-109">大多數的關聯性，作法是藉由更新外部索引鍵欄位或導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-109">For most relationships, this can be done by updating either foreign key fields or navigation properties.</span></span> <span data-ttu-id="72fff-110">針對多對多關聯性，Entity Framework 不還對聯結資料表直接公開，讓您加入及移除實體與適當的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-110">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="72fff-111">下列圖例顯示了您將操作的一些頁面。</span><span class="sxs-lookup"><span data-stu-id="72fff-111">The following illustrations show some of the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![課程講師編輯](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="72fff-115">自訂 Courses 的 [建立] 和 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="72fff-115">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="72fff-116">當新的課程實體建立時，其必須要與現有的部門具有關聯性。</span><span class="sxs-lookup"><span data-stu-id="72fff-116">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="72fff-117">若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="72fff-117">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="72fff-118">下拉式清單設定`Course.DepartmentID`外部索引鍵屬性，而這就是 Entity Framework 必須以載入所有`Department`導覽屬性，以適當`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-118">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="72fff-119">您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="72fff-119">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="72fff-120">在*CourseController.cs*，刪除四個`Create`和`Edit`方法並取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="72fff-120">In *CourseController.cs*, delete the four `Create` and `Edit` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

<span data-ttu-id="72fff-121">加入下列`using`檔案的開頭的陳述式：</span><span class="sxs-lookup"><span data-stu-id="72fff-121">Add the following `using` statement at the beginning of the file:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="72fff-122">`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`集合下拉式清單中，並將集合傳遞給在檢視`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-122">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="72fff-123">方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。</span><span class="sxs-lookup"><span data-stu-id="72fff-123">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="72fff-124">檢視會將名稱傳遞`DepartmentID`至[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)協助程式，以及協助程式就會知道要尋找`ViewBag`物件`SelectList`名為`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="72fff-124">The view will pass the name `DepartmentID` to the [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="72fff-125">`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新的課程部門不會建立尚未方法：</span><span class="sxs-lookup"><span data-stu-id="72fff-125">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="72fff-126">`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯的課程部門的識別碼：</span><span class="sxs-lookup"><span data-stu-id="72fff-126">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

<span data-ttu-id="72fff-127">`HttpPost`兩個方法`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：</span><span class="sxs-lookup"><span data-stu-id="72fff-127">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

<span data-ttu-id="72fff-128">此程式碼可確保，當頁面會重新顯示，以顯示錯誤訊息，已選取任何部門會持續選取。</span><span class="sxs-lookup"><span data-stu-id="72fff-128">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="72fff-129">課程檢視已為 scaffold [部門] 欄位中的下拉式清單但不希望 DepartmentID 標題這個欄位中，因此請下列反白顯示變更為*Views\Course\Create.cshtml*檔案變更標題。</span><span class="sxs-lookup"><span data-stu-id="72fff-129">The Course views are already scaffolded with drop-down lists for the department field, but you don't want the DepartmentID caption for this field, so make the following highlighted change to the *Views\Course\Create.cshtml* file to change the caption.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

<span data-ttu-id="72fff-130">進行中的相同變更*Views\Course\Edit.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="72fff-130">Make the same change in *Views\Course\Edit.cshtml*.</span></span>

<span data-ttu-id="72fff-131">通常 scaffolder 不會 scaffold 主索引鍵，因為索引鍵的值由資料庫產生和無法變更，而且不是顯示給使用者有意義的值。</span><span class="sxs-lookup"><span data-stu-id="72fff-131">Normally the scaffolder doesn't scaffold a primary key because the key value is generated by the database and can't be changed and isn't a meaningful value to be displayed to users.</span></span> <span data-ttu-id="72fff-132">課程實體 scaffolder 包含文字方塊`CourseID`欄位，因為其了解`DatabaseGeneratedOption.None`屬性表示使用者可以輸入主要金鑰的值。</span><span class="sxs-lookup"><span data-stu-id="72fff-132">For Course entities the scaffolder does include an text box for the `CourseID` field because it understands that the `DatabaseGeneratedOption.None` attribute means the user should be able enter the primary key value.</span></span> <span data-ttu-id="72fff-133">但它不了解的數目是有意義，因為您想要觀看它的其他檢視，因此您必須以手動方式新增。</span><span class="sxs-lookup"><span data-stu-id="72fff-133">But it doesn't understand that because the number is meaningful you want to see it in the other views, so you need to add it manually.</span></span>

<span data-ttu-id="72fff-134">在*Views\Course\Edit.cshtml*，加入課程數字欄位之前**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="72fff-134">In *Views\Course\Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="72fff-135">因為它是主索引鍵，它會顯示，但無法變更。</span><span class="sxs-lookup"><span data-stu-id="72fff-135">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

<span data-ttu-id="72fff-136">已隱藏的欄位 (`Html.HiddenFor`協助程式) 中編輯檢視課程編號。</span><span class="sxs-lookup"><span data-stu-id="72fff-136">There's already a hidden field (`Html.HiddenFor` helper) for the course number in the Edit view.</span></span> <span data-ttu-id="72fff-137">加入*Html.LabelFor* helper 不不再需要的隱藏欄位，因為它並不會造成要包含在張貼的資料，當使用者按一下的課程編號**儲存**上編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="72fff-137">Adding an *Html.LabelFor* helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the Edit page.</span></span>

<span data-ttu-id="72fff-138">在*Views\Course\Delete.cshtml*和*Views\Course\Details.cshtml*、 變更 「 部門 」 從 「 名稱 」 的部門名稱標題以及加入課程數字欄位之前**標題**欄位。</span><span class="sxs-lookup"><span data-stu-id="72fff-138">In *Views\Course\Delete.cshtml* and *Views\Course\Details.cshtml*, change the department name caption from "Name" to "Department" and add a course number field before the **Title** field.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

<span data-ttu-id="72fff-139">執行**建立**頁面 (顯示課程索引頁，然後按一下**新建**) 並輸入新的課程資料：</span><span class="sxs-lookup"><span data-stu-id="72fff-139">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="72fff-141">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="72fff-141">Click **Create**.</span></span> <span data-ttu-id="72fff-142">課程索引頁會顯示新的課程新增至清單。</span><span class="sxs-lookup"><span data-stu-id="72fff-142">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="72fff-143">[索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。</span><span class="sxs-lookup"><span data-stu-id="72fff-143">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="72fff-145">執行**編輯**頁面 (顯示課程索引頁，然後按一下**編輯**課程上)。</span><span class="sxs-lookup"><span data-stu-id="72fff-145">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="72fff-147">變更頁面上的資料，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="72fff-147">Change data on the page and click **Save**.</span></span> <span data-ttu-id="72fff-148">課程索引頁會顯示更新的課程資料。</span><span class="sxs-lookup"><span data-stu-id="72fff-148">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="72fff-149">加入對講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="72fff-149">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="72fff-150">當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="72fff-150">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="72fff-151">`Instructor`實體具有以零或-1 個關係`OfficeAssignment`實體，這表示您必須處理下列情況：</span><span class="sxs-lookup"><span data-stu-id="72fff-151">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="72fff-152">如果使用者清除 office 指派它最初的值，您必須移除並刪除`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-152">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="72fff-153">如果使用者輸入 office 指派值，而且它最初是空的您必須建立新`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-153">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="72fff-154">如果使用者變更 office 指派的值，您必須變更中的現有值`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-154">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="72fff-155">開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：</span><span class="sxs-lookup"><span data-stu-id="72fff-155">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="72fff-156">Scaffold 程式碼不是您所要的。</span><span class="sxs-lookup"><span data-stu-id="72fff-156">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="72fff-157">正在設定資料下拉式清單中，但是您需要什麼是文字方塊。</span><span class="sxs-lookup"><span data-stu-id="72fff-157">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="72fff-158">取代為下列程式碼中的這個方法：</span><span class="sxs-lookup"><span data-stu-id="72fff-158">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

<span data-ttu-id="72fff-159">此程式碼會卸除`ViewBag`陳述式，並將關聯的積極式載入`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-159">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="72fff-160">您無法執行使用積極式載入`Find`方法，所以`Where`和`Single`方法來選取講師來取代。</span><span class="sxs-lookup"><span data-stu-id="72fff-160">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="72fff-161">取代`HttpPost``Edit`方法取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="72fff-161">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="72fff-162">處理 office 指派更新：</span><span class="sxs-lookup"><span data-stu-id="72fff-162">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="72fff-163">若要參考`RetryLimitExceededException`需要`using`陳述式; 若要將它加入，請以滑鼠右鍵按一下`RetryLimitExceededException`，然後按一下 **解決** - **使用 System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="72fff-163">The reference to `RetryLimitExceededException` requires a `using` statement; to add it, right-click `RetryLimitExceededException`, and then click **Resolve** - **using System.Data.Entity.Infrastructure**.</span></span>

![解決為重試例外狀況](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="72fff-165">程式碼會執行下列操作：</span><span class="sxs-lookup"><span data-stu-id="72fff-165">The code does the following:</span></span>

- <span data-ttu-id="72fff-166">變更方法名稱，以`EditPost`因為簽章現在是相同`HttpGet`方法 (`ActionName`屬性會指定 /Edit/ URL 仍會使用)。</span><span class="sxs-lookup"><span data-stu-id="72fff-166">Changes the method name to `EditPost` because the signature is now the same as the `HttpGet` method (the `ActionName` attribute specifies that the /Edit/ URL is still used).</span></span>
- <span data-ttu-id="72fff-167">針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-167">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="72fff-168">這是您未相同`HttpGet``Edit`方法。</span><span class="sxs-lookup"><span data-stu-id="72fff-168">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="72fff-169">使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-169">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="72fff-170">[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*白名單*您想要包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-170">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="72fff-171">這可防止過度公佈時，所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="72fff-171">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- <span data-ttu-id="72fff-172">如果辦公室位置是空白，請設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表將會刪除。</span><span class="sxs-lookup"><span data-stu-id="72fff-172">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- <span data-ttu-id="72fff-173">將變更儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="72fff-173">Saves the changes to the database.</span></span>

<span data-ttu-id="72fff-174">在*Views\Instructor\Edit.cshtml*，之後`div`元素**雇用日期**欄位中，加入新欄位進行編輯的辦公室位置：</span><span class="sxs-lookup"><span data-stu-id="72fff-174">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

<span data-ttu-id="72fff-175">執行網頁 (選取**講師**索引標籤，然後按一下 **編輯**講師上)。</span><span class="sxs-lookup"><span data-stu-id="72fff-175">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="72fff-176">變更 [辦公室位置]，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="72fff-176">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="72fff-178">加入課程指派給講師編輯頁面</span><span class="sxs-lookup"><span data-stu-id="72fff-178">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="72fff-179">講師可教授任何數量的課程。</span><span class="sxs-lookup"><span data-stu-id="72fff-179">Instructors may teach any number of courses.</span></span> <span data-ttu-id="72fff-180">現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="72fff-180">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="72fff-182">之間的關聯性`Course`和`Instructor`實體是多對多，這表示您不需要直接存取還對聯結資料表中的外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-182">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the foreign key properties which are in the join table.</span></span> <span data-ttu-id="72fff-183">相反地，您加入和移除實體及與`Instructor.Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-183">Instead, you add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="72fff-184">可讓您變更講師指派之課程的 UI 為一組核取方塊。</span><span class="sxs-lookup"><span data-stu-id="72fff-184">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="72fff-185">資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。</span><span class="sxs-lookup"><span data-stu-id="72fff-185">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="72fff-186">使用者可選取或清除核取方塊來變更課程指派。</span><span class="sxs-lookup"><span data-stu-id="72fff-186">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="72fff-187">如果課程數目更大，可能要使用不同的檢視，來呈現資料的方法，但是您會使用相同操作才能建立或刪除關聯性的導覽屬性的方法。</span><span class="sxs-lookup"><span data-stu-id="72fff-187">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="72fff-188">若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="72fff-188">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="72fff-189">建立*AssignedCourseData.cs*中*ViewModels*資料夾並取代現有的程式碼取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="72fff-189">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="72fff-190">在*InstructorController.cs*，取代`HttpGet``Edit`方法取代下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="72fff-190">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="72fff-191">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="72fff-191">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

<span data-ttu-id="72fff-192">程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。</span><span class="sxs-lookup"><span data-stu-id="72fff-192">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="72fff-193">中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`實體以載入一份課程使用的檢視模型類別。</span><span class="sxs-lookup"><span data-stu-id="72fff-193">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="72fff-194">針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。</span><span class="sxs-lookup"><span data-stu-id="72fff-194">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="72fff-195">若要建立有效率查閱，檢查是否課程已指派給講師時，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。</span><span class="sxs-lookup"><span data-stu-id="72fff-195">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="72fff-196">`Assigned`屬性設定為`true`課程講師指派。</span><span class="sxs-lookup"><span data-stu-id="72fff-196">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="72fff-197">檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。</span><span class="sxs-lookup"><span data-stu-id="72fff-197">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="72fff-198">最後，清單會傳遞至檢視中`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-198">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="72fff-199">接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="72fff-199">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="72fff-200">取代`EditPost`方法會呼叫新的方法會更新為下列程式碼`Courses`導覽屬性`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-200">Replace the `EditPost` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="72fff-201">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="72fff-201">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

<span data-ttu-id="72fff-202">方法簽章現在是不同於`HttpGet``Edit`方法，以便從變更的方法名稱`EditPost`回到`Edit`。</span><span class="sxs-lookup"><span data-stu-id="72fff-202">The method signature is now different from the `HttpGet` `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="72fff-203">因為檢視沒有集合的`Course`實體模型繫結器無法自動會更新`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-203">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="72fff-204">而不是使用模型繫結器，來更新`Courses`導覽屬性，您將會執行，在新`UpdateInstructorCourses`方法。</span><span class="sxs-lookup"><span data-stu-id="72fff-204">Instead of using the model binder to update the `Courses` navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="72fff-205">因此您必須從模型繫結器中排除 `Courses` 屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-205">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="72fff-206">這並不需要呼叫的程式碼的任何變更[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您正在使用*允許清單*多載和`Courses`不在 include 清單中。</span><span class="sxs-lookup"><span data-stu-id="72fff-206">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="72fff-207">如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`導覽屬性使用空的集合：</span><span class="sxs-lookup"><span data-stu-id="72fff-207">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="72fff-208">程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。</span><span class="sxs-lookup"><span data-stu-id="72fff-208">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="72fff-209">為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。</span><span class="sxs-lookup"><span data-stu-id="72fff-209">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="72fff-210">若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。</span><span class="sxs-lookup"><span data-stu-id="72fff-210">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="72fff-211">若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。</span><span class="sxs-lookup"><span data-stu-id="72fff-211">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="72fff-212">在*Views\Instructor\Edit.cshtml*，新增**課程**欄位與陣列中加入下列的核取方塊的程式碼後立即`div`項目`OfficeAssignment`欄位和之前`div`元素**儲存**按鈕：</span><span class="sxs-lookup"><span data-stu-id="72fff-212">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the `OfficeAssignment` field and before the `div` element for the **Save** button:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

<span data-ttu-id="72fff-213">貼上之後的程式碼，如果換行和縮排不看起來像是他們所做的動作，手動修正所有項目，使它看起來像這裡所示。</span><span class="sxs-lookup"><span data-stu-id="72fff-213">After you paste the code, if line breaks and indentation don't look like they do here, manually fix everything so that it looks like what you see here.</span></span> <span data-ttu-id="72fff-214">縮排不一定要是完美的，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@</tr>` 必須要如顯示般各自在獨立的一行上，否則您會接收到執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="72fff-214">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span>

<span data-ttu-id="72fff-215">此程式碼會建立一個 HTML 表格，該表格中有三個資料行。</span><span class="sxs-lookup"><span data-stu-id="72fff-215">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="72fff-216">在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。</span><span class="sxs-lookup"><span data-stu-id="72fff-216">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="72fff-217">所有核取方塊具有相同名稱 ("selectedCourses 」) 會通知它們會被視為一個群組的模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="72fff-217">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="72fff-218">`value`的每個核取方塊的屬性設定的值為`CourseID.`當網頁回傳時，模型繫結器會將陣列傳遞至所組成的控制站`CourseID`只核取方塊已選取的值。</span><span class="sxs-lookup"><span data-stu-id="72fff-218">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="72fff-219">當一開始會呈現核取方塊時，為指派給講師課程的有`checked`選取 （顯示其核取） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="72fff-219">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="72fff-220">之後變更課程作業，您會想要能夠驗證所做的變更，當站台恢復時`Index`頁面。</span><span class="sxs-lookup"><span data-stu-id="72fff-220">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="72fff-221">因此，您需要將資料行加入至該頁面中的資料表。</span><span class="sxs-lookup"><span data-stu-id="72fff-221">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="72fff-222">在此情況下，您不必使用`ViewBag`物件，因為您想要顯示的資訊已在`Courses`導覽屬性`Instructor`您只要傳遞至頁面，為模型的實體。</span><span class="sxs-lookup"><span data-stu-id="72fff-222">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="72fff-223">在*Views\Instructor\Index.cshtml*，新增**課程**標題緊接**Office**標題之下，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="72fff-223">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

<span data-ttu-id="72fff-224">然後加入新的詳細資料資料格，緊接著辦公室位置的詳細資料格：</span><span class="sxs-lookup"><span data-stu-id="72fff-224">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

<span data-ttu-id="72fff-225">執行**講師索引**頁面，以查看指派給每個講師課程：</span><span class="sxs-lookup"><span data-stu-id="72fff-225">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="72fff-227">按一下**編輯**講師以查看 [編輯] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="72fff-227">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

<span data-ttu-id="72fff-229">變更一些課程作業，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="72fff-229">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="72fff-230">您所做的變更會反映在 [索引] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="72fff-230">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="72fff-231">注意： 您帶往這裡編輯講師課程資料的方法就可以使用也有限的數目的課程。</span><span class="sxs-lookup"><span data-stu-id="72fff-231">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="72fff-232">針對更大的集合，將需要不同的 UI 和不同的更新方法。</span><span class="sxs-lookup"><span data-stu-id="72fff-232">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-deleteconfirmed-method"></a><span data-ttu-id="72fff-233">Update DeleteConfirmed 方法</span><span class="sxs-lookup"><span data-stu-id="72fff-233">Update the DeleteConfirmed Method</span></span>

<span data-ttu-id="72fff-234">在*InstructorController.cs*，刪除`DeleteConfirmed`方法並插入下列程式碼會在其位置。</span><span class="sxs-lookup"><span data-stu-id="72fff-234">In *InstructorController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

<span data-ttu-id="72fff-235">此程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="72fff-235">This code makes the following change:</span></span>

- <span data-ttu-id="72fff-236">如果講師被指派任何部門的系統管理員身分，移除該部門講師指派。</span><span class="sxs-lookup"><span data-stu-id="72fff-236">If the instructor is assigned as administrator of any department, removes the instructor assignment from that department.</span></span> <span data-ttu-id="72fff-237">沒有這個程式碼中，您會取得參考完整性錯誤，如果您嘗試刪除講師已指派為部門的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="72fff-237">Without this code, you would get a referential integrity error if you tried to delete an instructor who was assigned as administrator for a department.</span></span>

<span data-ttu-id="72fff-238">此程式碼未處理的一個講師指派多個部門的系統管理員身分的案例。</span><span class="sxs-lookup"><span data-stu-id="72fff-238">This code doesn't handle the scenario of one instructor assigned as administrator for multiple departments.</span></span> <span data-ttu-id="72fff-239">最後一個教學課程中您要加入程式碼會防止發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="72fff-239">In the last tutorial you'll add code that prevents that scenario from happening.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="72fff-240">將辦公室位置和課程新增至 [新增] 頁面</span><span class="sxs-lookup"><span data-stu-id="72fff-240">Add office location and courses to the Create page</span></span>

<span data-ttu-id="72fff-241">在*InstructorController.cs*，刪除`HttpGet`和`HttpPost``Create`方法，然後在適當位置中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="72fff-241">In *InstructorController.cs*, delete the `HttpGet` and `HttpPost` `Create` methods, and then add the following code in their place:</span></span>


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="72fff-242">此程式碼很類似您所見的編輯方法的不同之處在於起初會選取任何課程。</span><span class="sxs-lookup"><span data-stu-id="72fff-242">This code is similar to what you saw for the Edit methods except that initially no courses are selected.</span></span> <span data-ttu-id="72fff-243">`HttpGet` `Create`方法呼叫`PopulateAssignedCourseData`方法不是因為可能有選取，但在課程，才能提供空的集合`foreach`（否則檢視程式碼會擲回 null 參考例外狀況的檢視中的迴圈).</span><span class="sxs-lookup"><span data-stu-id="72fff-243">The `HttpGet` `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="72fff-244">HttpPost Create 方法會將範本程式碼來檢查驗證錯誤，並將新講師加入至資料庫之前課程導覽屬性中的每個所選的課程。</span><span class="sxs-lookup"><span data-stu-id="72fff-244">The HttpPost Create method adds each selected course to the Courses navigation property before the template code that checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="72fff-245">即使模型錯誤加入課程，讓模型錯誤 （例如，做為索引鍵的日期不正確的使用者） 時所做的任何課程選取項目，以便當頁面會重新顯示，並出現錯誤訊息，會自動還原。</span><span class="sxs-lookup"><span data-stu-id="72fff-245">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date) so that when the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="72fff-246">請注意，為了要能夠將課程新增到 `Courses` 導覽屬性，您必須將屬性以空集合初始化：</span><span class="sxs-lookup"><span data-stu-id="72fff-246">Notice that in order to be able to add courses to the `Courses` navigation property you have to initialize the property as an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="72fff-247">作為在控制器程式碼中完成這項操作的替代方案，您可以在 Instructor 模型中藉由將屬性 getter 變更為在不存在時自動建立集合來完成，如以下範例所示：</span><span class="sxs-lookup"><span data-stu-id="72fff-247">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="72fff-248">若您使用這種方式修改了 `Courses` 屬性，您便可以移除控制器中的明確屬性初始化程式碼。</span><span class="sxs-lookup"><span data-stu-id="72fff-248">If you modify the `Courses` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="72fff-249">在*Views\Instructor\Create.cshtml*、 新增辦公室位置 文字方塊中，以及雇用日期欄位之後，在之前課程核取方塊**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="72fff-249">In *Views\Instructor\Create.cshtml*, add an office location text box and course check boxes after the hire date field and before the **Submit** button.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

<span data-ttu-id="72fff-250">您貼上程式碼之後，修正分行符號和縮排，如同先前的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="72fff-250">After you paste the code, fix line breaks and indentation as you did earlier for the Edit page.</span></span>

<span data-ttu-id="72fff-251">執行 [建立] 頁面，然後將講師。</span><span class="sxs-lookup"><span data-stu-id="72fff-251">Run the Create page and add an instructor.</span></span>

![講師建立的課程](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a><span data-ttu-id="72fff-253">處理交易</span><span class="sxs-lookup"><span data-stu-id="72fff-253">Handling Transactions</span></span>

<span data-ttu-id="72fff-254">中所述[基本 CRUD 功能教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)，根據預設 Entity Framework 會以隱含方式實作交易。</span><span class="sxs-lookup"><span data-stu-id="72fff-254">As explained in the [Basic CRUD Functionality tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), by default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="72fff-255">如案例，您需要更大的控制權-例如，如果您想要包含在交易-外面 Entity Framework 執行的作業，請參閱[使用交易](https://msdn.microsoft.com/data/dn456843)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="72fff-255">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Working with Transactions](https://msdn.microsoft.com/data/dn456843) on MSDN.</span></span>

## <a name="summary"></a><span data-ttu-id="72fff-256">總結</span><span class="sxs-lookup"><span data-stu-id="72fff-256">Summary</span></span>

<span data-ttu-id="72fff-257">您現在已完成此工作與相關資料的簡介。</span><span class="sxs-lookup"><span data-stu-id="72fff-257">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="72fff-258">到目前為止在這些教學課程中您使用過程式碼進行同步 I/O。</span><span class="sxs-lookup"><span data-stu-id="72fff-258">So far in these tutorials you've worked with code that does synchronous I/O.</span></span> <span data-ttu-id="72fff-259">您可以讓應用程式藉由實作非同步程式碼，更有效率地使用 web 伺服器資源，且您將會執行下一個教學課程中。</span><span class="sxs-lookup"><span data-stu-id="72fff-259">You can make the application use web server resources more efficiently by implementing asynchronous code, and that's what you'll do in the next tutorial.</span></span>

<span data-ttu-id="72fff-260">請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="72fff-260">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="72fff-261">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="72fff-261">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="72fff-262">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="72fff-262">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72fff-263">[上一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="72fff-263">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
