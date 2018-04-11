---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 更新相關的資料與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-6) |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>更新與 ASP.NET MVC 應用程式 (10-6) 中的 Entity Framework 的相關的資料
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


您可以在上一個教學課程中顯示相關的資料。在本教學課程中，您要更新的相關的資料。 大多數的關聯性，作法是藉由更新適當的外部索引鍵欄位。 針對多對多關聯性，Entity Framework 不還對聯結資料表直接公開，因此您必須明確地加入及移除實體與適當的導覽屬性。

下列圖例顯示了您將操作的頁面。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>自訂 Courses 的 [建立] 和 [編輯] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。 下拉式清單設定`Course.DepartmentID`外部索引鍵屬性，而這就是 Entity Framework 必須以載入所有`Department`導覽屬性，以適當`Department`實體。 您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。

在*CourseController.cs*，刪除四個`Edit`和`Create`方法並取代為下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`集合下拉式清單中，並將集合傳遞給在檢視`ViewBag`屬性。 方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。 檢視會將名稱傳遞`DepartmentID`來[ `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)，並協助專家就會知道要查看`ViewBag`物件`SelectList`名為`DepartmentID`。

`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新的課程部門不會建立尚未方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯的課程部門的識別碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost`兩個方法`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

此程式碼可確保，當頁面會重新顯示，以顯示錯誤訊息，已選取任何部門會持續選取。

在*Views\Course\Create.cshtml*，加入反白顯示的程式碼，以建立新的課程數字欄位之前**標題**欄位。 根據預設，如同先前的教學課程中所說明，不 scaffold 主索引鍵欄位，但此主索引鍵是有意義，因此您想讓使用者能夠輸入的索引鍵值。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

在*Views\Course\Edit.cshtml*， *Views\Course\Delete.cshtml*，和*Views\Course\Details.cshtml*，加入課程數字欄位之前**標題**欄位。 因為它是主索引鍵，它會顯示，但無法變更。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

執行**建立**頁面 (顯示課程索引頁，然後按一下**新建**) 並輸入新的課程資料：

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

按一下 [建立] 。 課程索引頁會顯示新的課程新增至清單。 [索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

執行**編輯**頁面 (顯示課程索引頁，然後按一下**編輯**課程上)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

變更頁面上的資料，然後按一下 [儲存]。 課程索引頁會顯示更新的課程資料。

## <a name="adding-an-edit-page-for-instructors"></a>加入對講師編輯頁面

當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。 `Instructor`實體具有以零或-1 個關係`OfficeAssignment`實體，這表示您必須處理下列情況：

- 如果使用者清除 office 指派它最初的值，您必須移除並刪除`OfficeAssignment`實體。
- 如果使用者輸入 office 指派值，而且它最初是空的您必須建立新`OfficeAssignment`實體。
- 如果使用者變更 office 指派的值，您必須變更中的現有值`OfficeAssignment`實體。

開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Scaffold 程式碼不是您所要的。 正在設定資料下拉式清單中，但是您需要什麼是文字方塊。 取代為下列程式碼中的這個方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

此程式碼會卸除`ViewBag`陳述式，並將關聯的積極式載入`OfficeAssignment`實體。 您無法執行使用積極式載入`Find`方法，所以`Where`和`Single`方法來選取講師來取代。

取代`HttpPost``Edit`方法取代下列程式碼。 處理 office 指派更新：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

程式碼會執行下列操作：

- 針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。 這是您未相同`HttpGet``Edit`方法。
- 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*允許清單*您想要包含的屬性。 這可防止過度公佈時，所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- 如果辦公室位置是空白，請設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表將會刪除。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- 將變更儲存到資料庫。

在*Views\Instructor\Edit.cshtml*，之後`div`元素**雇用日期**欄位中，加入新欄位進行編輯的辦公室位置：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

執行網頁 (選取**講師**索引標籤，然後按一下 **編輯**講師上)。 變更 [辦公室位置]，然後按一下 [儲存]。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>加入課程指派給講師編輯頁面

講師可教授任何數量的課程。 現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

之間的關聯性`Course`和`Instructor`實體是多對多，這表示您不需要直接存取還對聯結資料表。 相反地，您將加入並移除實體及與`Instructor.Courses`導覽屬性。

可讓您變更講師指派之課程的 UI 為一組核取方塊。 資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。 使用者可選取或清除核取方塊來變更課程指派。 如果課程數目更大，可能要使用不同的檢視，來呈現資料的方法，但是您會使用相同操作才能建立或刪除關聯性的導覽屬性的方法。

若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。 建立*AssignedCourseData.cs*中*ViewModels*資料夾並取代現有的程式碼取代下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

在*InstructorController.cs*，取代`HttpGet``Edit`方法取代下列程式碼。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。

中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`實體以載入一份課程使用的檢視模型類別。 針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。 若要建立有效率查閱，檢查是否課程已指派給講師時，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。 `Assigned`屬性設定為`true`課程講師指派。 檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。 最後，清單會傳遞至檢視中`ViewBag`屬性。

接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。 取代`HttpPost``Edit`方法會呼叫新的方法會更新為下列程式碼`Courses`導覽屬性`Instructor`實體。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

因為檢視沒有集合的`Course`實體模型繫結器無法自動會更新`Courses`導覽屬性。 而不是使用模型繫結器，來更新課程導覽屬性，您將會執行在新的`UpdateInstructorCourses`方法。 因此您必須從模型繫結器中排除 `Courses` 屬性。 這並不需要呼叫的程式碼的任何變更[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您正在使用*允許清單*多載和`Courses`不在 include 清單中。

如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`導覽屬性使用空的集合：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。 為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。

若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

在*Views\Instructor\Edit.cshtml*，新增**課程**欄位與陣列中加入下列反白顯示的核取方塊的程式碼後立即`div`之項目的`OfficeAssignment`欄位：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

此程式碼會建立一個 HTML 表格，該表格中有三個資料行。 在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。 所有核取方塊具有相同名稱 ("selectedCourses 」) 會通知它們會被視為一個群組的模型繫結器。 `value`的每個核取方塊的屬性設定的值為`CourseID.`當網頁回傳時，模型繫結器會將陣列傳遞至所組成的控制站`CourseID`只核取方塊已選取的值。

當一開始會呈現核取方塊時，為指派給講師課程的有`checked`選取 （顯示其核取） 的屬性。

之後變更課程作業，您會想要能夠驗證所做的變更，當站台恢復時`Index`頁面。 因此，您需要將資料行加入至該頁面中的資料表。 在此情況下，您不必使用`ViewBag`物件，因為您想要顯示的資訊已在`Courses`導覽屬性`Instructor`您只要傳遞至頁面，為模型的實體。

在*Views\Instructor\Index.cshtml*，新增**課程**標題緊接**Office**標題之下，如下列範例所示：

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

然後加入新的詳細資料資料格，緊接著辦公室位置的詳細資料格：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

執行**講師索引**頁面，以查看指派給每個講師課程：

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

按一下**編輯**講師以查看 [編輯] 頁面上。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

變更一些課程作業，然後按一下**儲存**。 您所做的變更會反映在 [索引] 頁面上。

 附註： 若要編輯講師課程資料採用的方法就可以使用也有限的數目的課程。 針對更大的集合，將需要不同的 UI 和不同的更新方法。  
 

## <a name="update-the-delete-method"></a>更新 Delete 方法

變更 HttpPost Delete 方法中的程式碼，因此刪除講師時刪除該 office 指派記錄 （如果有的話）：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


如果您嘗試刪除講師獲指派給系統管理員身分的部門，您會取得參考完整性錯誤。 請參閱[本教學課程中的目前版本](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)會自動移除講師講師指派為系統管理員的其中任何部門的其他程式碼。

## <a name="summary"></a>總結

您現在已完成此工作與相關資料的簡介。 到目前為止在這些教學課程已完成完整的 CRUD 作業，但尚未處理並行的問題。 下一個教學課程將介紹並行的主題、 說明選項來處理，並新增您已寫入一個實體類型的 CRUD 程式碼處理的並行存取。

連結其他實體架構的資源，可以找到結尾[這一系列中的最後一個教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。

> [!div class="step-by-step"]
> [上一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
