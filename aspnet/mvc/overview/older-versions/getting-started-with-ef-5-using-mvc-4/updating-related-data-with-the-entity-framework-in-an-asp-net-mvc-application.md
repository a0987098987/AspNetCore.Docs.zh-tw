---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 使用 ASP.NET MVC 應用程式 (10 個 6) 中的 Entity Framework 更新相關的資料 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e85162f58ed9826132db8bd854914a14709f709d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809429"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>使用 Entity Framework 的 ASP.NET MVC 應用程式 (10 個 6) 中更新相關的資料
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


您在先前的教學課程中顯示相關的資料;在本教學課程中，您將更新相關的資料。 大部分的關聯性，做法是藉由更新適當的外部索引鍵欄位。 多對多關聯性，Entity Framework 不會聯結資料表直接公開，因此您必須明確地新增，然後移除適當的導覽屬性的實體。

下列圖例顯示了您將操作的頁面。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>自訂 Courses 的 [建立] 和 [編輯] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。 下拉式清單會設定`Course.DepartmentID`外部索引鍵屬性，就是這麼 Entity Framework 必須以載入`Department`導覽屬性，以適當`Department`實體。 您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。

在  *CourseController.cs*，刪除四個`Edit`和`Create`方法，並以下列程式碼取代：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`下拉式清單中，集合，並將集合傳遞至檢視中`ViewBag`屬性。 方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。 檢視會將名稱傳遞`DepartmentID`來[ `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)，並協助程式就會知道要尋找`ViewBag`物件`SelectList`名為`DepartmentID`。

`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新課程的部門還沒有建立的方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯之課程的部門識別碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost`方法都`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

此程式碼可確保，當頁面重新顯示以顯示錯誤訊息，已選取的任何部門保持選取。

在  *Views\Course\Create.cshtml*，新增反白顯示的程式碼，以建立新的課程號碼欄位之前**標題**欄位。 先前的教學課程所述，主索引鍵欄位不會進行 scaffold 根據預設，但此主索引鍵是有意義，因此您想讓使用者能夠輸入的索引鍵值。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

在  *Views\Course\Edit.cshtml*， *Views\Course\Delete.cshtml*，並*Views\Course\Details.cshtml*，新增一個課程號碼欄位，再**標題**欄位。 因為它是主索引鍵時，它會顯示，但無法變更。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

執行**Create**網頁 (顯示課程索引頁面，然後按一下**建立新**) 並輸入新的課程資料：

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

按一下 [建立] 。 課程索引 頁面會顯示新的課程新增至清單。 [索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

執行**編輯**網頁 (顯示課程索引頁面，然後按一下**編輯**課程)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

變更頁面上的資料，然後按一下 [儲存]。 課程索引 頁面會顯示更新的課程資料。

## <a name="adding-an-edit-page-for-instructors"></a>新增講師 [編輯] 的頁面

當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。 `Instructor`實體具有一對零-或-一關係`OfficeAssignment`實體，這表示您必須處理下列情況：

- 如果使用者清除辦公室指派，而且它原先擁有值，您必須移除並刪除`OfficeAssignment`實體。
- 如果使用者輸入辦公室指派值，而且它原先是空白，您必須建立新`OfficeAssignment`實體。
- 如果使用者變更辦公室指派的值時，您必須變更中的現有值`OfficeAssignment`實體。

開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Scaffold 的程式碼不是您所要的。 設定資料的下拉式清單中，但您需要的是文字方塊。 這個方法取代為下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

此程式碼會卸除`ViewBag`陳述式，並將相關聯的積極式載入`OfficeAssignment`實體。 您無法執行使用積極式載入`Find`方法，因此`Where`和`Single`方法將改為用來選取講師。

取代`HttpPost``Edit`為下列程式碼的方法。 處理辦公室指派更新：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

程式碼會執行下列操作：

- 針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。 這是您未相同`HttpGet``Edit`方法。
- 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*允許清單*您想要包含的屬性。 這可防止大量指派，如所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- 如果辦公室位置為空白，設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表會被刪除。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- 將變更儲存到資料庫。

在  *Views\Instructor\Edit.cshtml*之後，`div`項目**雇用日期**欄位中，新增用於編輯辦公室位置的欄位：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

執行網頁 (選取**講師**索引標籤，然後按一下**編輯**講師上)。 變更 [辦公室位置]，然後按一下 [儲存]。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>將課程指派給講師的編輯頁面

講師可教授任何數量的課程。 現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

之間的關聯性`Course`和`Instructor`實體是多對多，這表示您沒有直接對聯結資料表的存取。 相反地，您將在其中新增和移除實體，來回`Instructor.Courses`導覽屬性。

可讓您變更講師指派之課程的 UI 為一組核取方塊。 資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。 使用者可選取或清除核取方塊來變更課程指派。 如果課程數目大許多，您可能想要使用不同的方法，可在檢視中，顯示資料，但您會使用相同的方法的操作才能建立或刪除關聯性的導覽屬性。

若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。 建立*Schoolviewmodels*中*Viewmodel*資料夾，並將現有的程式碼，以下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

在  *InstructorController.cs*，取代`HttpGet``Edit`為下列程式碼的方法。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。

中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`為了載入一份使用檢視的課程實體模型類別。 針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。 若要檢查課程是否已指派給講師時，請建立有效率的查閱，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。 `Assigned`屬性設定為`true`指派的講師的課程。 檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。 最後，清單會傳遞至檢視中`ViewBag`屬性。

接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。 取代`HttpPost``Edit`方法會呼叫新方法以更新為下列程式碼`Courses`導覽屬性`Instructor`實體。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

由於檢視沒有一堆`Course`自動更新實體，模型繫結無法`Courses`導覽屬性。 而不是使用模型繫結來更新 Course 導覽屬性，您將執行在新的`UpdateInstructorCourses`方法。 因此您必須從模型繫結器中排除 `Courses` 屬性。 這並不需要進行任何變更，要呼叫的程式碼[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您使用*列入白名單*多載和`Courses`不在 include 清單中。

如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`具有空的集合導覽屬性：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。 為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。

若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

在*Views\Instructor\Edit.cshtml*，新增**課程**欄位具有陣列中加入下列反白顯示的核取方塊的程式碼之後立即`div`項目`OfficeAssignment`欄位：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

此程式碼會建立一個 HTML 表格，該表格中有三個資料行。 在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。 核取方塊都具有相同名稱 ("selectedCourses") 會告知模型繫結，它們會被視為一個群組。 `value`的值設定屬性的每個核取方塊`CourseID.`當頁面回傳時，模型繫結會將陣列傳遞至控制器所組成`CourseID`只核取方塊已選取的值。

當一開始呈現核取方塊時，即指派給講師的課程有`checked`選取 （顯示它們已核取） 的屬性。

變更課程指派之後, 您會想要能夠驗證所做的變更，當站台恢復`Index`頁面。 因此，您需要將資料行新增至該頁面中的資料表。 在此情況下，您不需要使用`ViewBag`物件，因為已在您想要顯示的資訊`Courses`導覽屬性`Instructor`您要將它傳遞給頁面做為模型的實體。

在  *Views\Instructor\Index.cshtml*，新增**課程**標題的正後方**Office**標題之下，如下列範例所示：

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

然後，新增新的詳細資料格，緊接著辦公室位置的詳細資料格：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

執行**Instructor 索引**頁面，以查看指派給每位講師的課程：

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

按一下 **編輯**講師，以查看 編輯 頁面上。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

變更一些課程指派，然後按一下**儲存**。 您所做的變更會反映在 [索引] 頁面上。

 注意： 來編輯講師課程資料的方法時運作相當良好有限的數目的課程。 針對更大的集合，將需要不同的 UI 和不同的更新方法。  
 

## <a name="update-the-delete-method"></a>更新的 Delete 方法

因此辦公室指派記錄 （如果有的話） 刪除時刪除講師時，請變更則 HttpPost Delete 方法中的程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


如果您嘗試刪除講師角色，獲指派給某個部門系統管理員身分，您會收到參考完整性錯誤。 請參閱[目前的版本，本教學課程的](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)的額外程式碼會自動移除講師指派為系統管理員的其中任何部門的講師。

## <a name="summary"></a>總結

您現在已完成此操作相關資料的簡介。 到目前為止在這些教學課程中，您已完成完整的 CRUD 作業，但您還沒有處理的並行處理問題。 下一個教學課程會介紹並行存取的主題、 說明選項來處理它，並新增您已撰寫一個實體類型的 CRUD 程式碼處理的並行存取。

其他 Entity Framework 資源的連結可以找到在結尾[這個系列的最後一個教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。

> [!div class="step-by-step"]
> [上一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
