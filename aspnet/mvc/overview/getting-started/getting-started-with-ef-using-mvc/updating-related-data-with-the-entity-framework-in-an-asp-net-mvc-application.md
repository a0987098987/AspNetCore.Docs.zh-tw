---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 使用 Entity Framework 的 ASP.NET MVC 應用程式中更新相關的資料 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 05b2f92155a4c3cac7ec8edd36b8ac6724b21888
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370927"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>使用 Entity Framework 的 ASP.NET MVC 應用程式中更新相關的資料
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


您在先前的教學課程中顯示相關的資料;在本教學課程中，您將更新相關的資料。 大部分的關聯性，做法是藉由更新外部索引鍵欄位或導覽屬性。 多對多關聯性，Entity Framework 不會聯結資料表直接公開，讓您新增和移除適當的導覽屬性的實體。

下列圖例顯示了您將操作的一些頁面。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![編輯講師課程](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>自訂 Courses 的 [建立] 和 [編輯] 頁面

當新的課程實體建立時，其必須要與現有的部門具有關聯性。 若要達成此目的，Scaffold 程式碼包含了控制器方法和 [建立] 和 [編輯] 檢視，當中包含了一個可選取部門的下拉式清單。 下拉式清單會設定`Course.DepartmentID`外部索引鍵屬性，就是這麼 Entity Framework 必須以載入`Department`導覽屬性，以適當`Department`實體。 您將使用 Scaffold 程式碼，但會稍微對其進行一些變更以新增錯誤處理及排序下拉式清單。

在  *CourseController.cs*，刪除四個`Create`和`Edit`方法，並以下列程式碼取代：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

新增下列`using`檔案的開頭的陳述式：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList`方法會取得一份依名稱排序的所有部門、 建立`SelectList`下拉式清單中，集合，並將集合傳遞至檢視中`ViewBag`屬性。 方法接受選擇性的 `selectedDepartment` 參數，可允許呼叫程式碼在呈現下拉式清單時指定選取的項目。 檢視會將名稱傳遞`DepartmentID`要[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)協助程式，並協助程式就會知道要尋找`ViewBag`物件`SelectList`名為`DepartmentID`。

`HttpGet` `Create`方法呼叫`PopulateDepartmentsDropDownList`但不會將選取的項目，因為新課程的部門還沒有建立的方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit`方法設定選取的項目，根據已指派給正在編輯之課程的部門識別碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost`方法都`Create`和`Edit`也包括設定選取的項目，當他們在發生錯誤之後重新顯示頁面的程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

此程式碼可確保，當頁面重新顯示以顯示錯誤訊息，已選取的任何部門保持選取。

下拉式清單中 [部門] 欄位中，已經 scaffold Course 檢視但不想 DepartmentID 標題此欄位中，以便進行以下反白顯示變更為*Views\Course\Create.cshtml*檔案變更標題。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

請在相同的變更*Views\Course\Edit.cshtml*。

通常框架不會 scaffold 主索引鍵，因為索引鍵的值由資料庫產生和無法變更，而且不會向使用者顯示有意義的值。 針對 Course 實體框架包含文字方塊，讓`CourseID`欄位，因為它了解`DatabaseGeneratedOption.None`屬性表示的使用者應該可以輸入主索引鍵值。 但它並不了解有意義的數字，是因為您想要看到它在其他檢視中，因此您必須手動將它加入。

在  *Views\Course\Edit.cshtml*，新增一個課程號碼欄位，再**標題**欄位。 因為它是主索引鍵時，它會顯示，但無法變更。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

已經有隱藏的欄位 (`Html.HiddenFor`協助程式) 在 [編輯] 檢視中將課程編號。 新增*Html.LabelFor*協助程式無法消除隱藏欄位的需求，因為它無法讓課程號碼包含在張貼的資料，當使用者按一下**儲存**編輯 頁面上。

在  *Views\Course\Delete.cshtml*並*Views\Course\Details.cshtml*、 變更 「 部門 」 從 「 名稱 」 的部門名稱標題以及加入前的一個課程號碼欄位**標題**欄位。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

執行**Create**網頁 (顯示課程索引頁面，然後按一下**建立新**) 並輸入新的課程資料：

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

按一下 [建立] 。 課程索引 頁面會顯示新的課程新增至清單。 [索引] 頁面中的部門名稱來自於導覽屬性，顯示關聯性已正確建立。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

執行**編輯**網頁 (顯示課程索引頁面，然後按一下**編輯**課程)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

變更頁面上的資料，然後按一下 [儲存]。 課程索引 頁面會顯示更新的課程資料。

## <a name="adding-an-edit-page-for-instructors"></a>新增講師 [編輯] 的頁面

當您編輯講師記錄時，您可能會想要更新講師的辦公室指派。 `Instructor`實體具有一對零-或-一關係`OfficeAssignment`實體，這表示您必須處理下列情況：

- 如果使用者清除辦公室指派，而且它原先擁有值，您必須移除並刪除`OfficeAssignment`實體。
- 如果使用者輸入辦公室指派值，而且它原先是空白，您必須建立新`OfficeAssignment`實體。
- 如果使用者變更辦公室指派的值時，您必須變更中的現有值`OfficeAssignment`實體。

開啟*InstructorController.cs*並查看`HttpGet``Edit`方法：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Scaffold 的程式碼不是您所要的。 設定資料的下拉式清單中，但您需要的是文字方塊。 這個方法取代為下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

此程式碼會卸除`ViewBag`陳述式，並將相關聯的積極式載入`OfficeAssignment`實體。 您無法執行使用積極式載入`Find`方法，因此`Where`和`Single`方法將改為用來選取講師。

取代`HttpPost``Edit`為下列程式碼的方法。 處理辦公室指派更新：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

若要參考`RetryLimitExceededException`需要`using`陳述式; 若要將它加入，請以滑鼠右鍵按一下`RetryLimitExceededException`，然後按一下**解決** - **使用 System.Data.Entity.Infrastructure**.

![解析重試例外狀況](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

程式碼會執行下列操作：

- 將方法名稱變更為`EditPost`因為簽章現在相同`HttpGet`方法 (`ActionName`屬性會指定 /Edit/ URL 仍在使用)。
- 針對 `OfficeAssignment` 導覽屬性使用積極式載入從資料庫中取得目前的 `Instructor` 實體。 這是您未相同`HttpGet``Edit`方法。
- 使用從模型繫結器取得的值更新擷取的 `Instructor` 實體。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用多載可讓您*允許清單*您想要包含的屬性。 這可防止大量指派，如所述[第二個教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- 如果辦公室位置為空白，設定`Instructor.OfficeAssignment`屬性設為 null，讓相關的資料列中`OfficeAssignment`資料表會被刪除。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- 將變更儲存到資料庫。

在  *Views\Instructor\Edit.cshtml*之後，`div`項目**雇用日期**欄位中，新增用於編輯辦公室位置的欄位：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

執行網頁 (選取**講師**索引標籤，然後按一下**編輯**講師上)。 變更 [辦公室位置]，然後按一下 [儲存]。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>將課程指派給講師的編輯頁面

講師可教授任何數量的課程。 現在您將藉由使用核取方塊群組，新增變更課程指派的能力來強化 Instructor [編輯] 頁面，如以下螢幕擷取畫面所示：

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

之間的關聯性`Course`和`Instructor`實體是多對多，這表示您沒有直接存取中的聯結資料表的外部索引鍵屬性。 相反地，您新增和移除實體，來回`Instructor.Courses`導覽屬性。

可讓您變更講師指派之課程的 UI 為一組核取方塊。 資料庫中每個課程的核取方塊都會顯示，而該名講師目前受指派的課程會已選取狀態顯示。 使用者可選取或清除核取方塊來變更課程指派。 如果課程數目大許多，您可能想要使用不同的方法，可在檢視中，顯示資料，但您會使用相同的方法的操作才能建立或刪除關聯性的導覽屬性。

若要針對核取方塊清單提供資料給檢視，您必須使用一個檢視模型類別。 建立*Schoolviewmodels*中*Viewmodel*資料夾，並將現有的程式碼，以下列程式碼：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

在  *InstructorController.cs*，取代`HttpGet``Edit`為下列程式碼的方法。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

程式碼會為 `Courses` 導覽屬性新增積極式載入，然後使用 `AssignedCourseData` 檢視模型類別來呼叫新的 `PopulateAssignedCourseData` 方法以提供資訊給核取方塊陣列。

中的程式碼`PopulateAssignedCourseData`方法會讀取所有`Course`為了載入一份使用檢視的課程實體模型類別。 針對每個課程，程式碼會檢查課程是否存在於講師的 `Courses` 導覽屬性中。 若要檢查課程是否已指派給講師時，請建立有效率的查閱，指派給講師的課程會放入[HashSet](https://msdn.microsoft.com/library/bb359438.aspx)集合。 `Assigned`屬性設定為`true`指派的講師的課程。 檢視會使用這個屬性，來判斷哪一個核取方塊必須顯示為已選取。 最後，清單會傳遞至檢視中`ViewBag`屬性。

接下來，新增當使用者按一下 [儲存] 時要執行的程式碼。 取代`EditPost`方法會呼叫新方法以更新為下列程式碼`Courses`導覽屬性`Instructor`實體。 所做的變更已醒目提示。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

方法簽章現在是不同於`HttpGet``Edit`方法，所以方法名稱會從`EditPost`回到`Edit`。

由於檢視沒有一堆`Course`自動更新實體，模型繫結無法`Courses`導覽屬性。 而不是使用更新的模型繫結`Courses`導覽屬性，您將執行，在新`UpdateInstructorCourses`方法。 因此您必須從模型繫結器中排除 `Courses` 屬性。 這並不需要進行任何變更，要呼叫的程式碼[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)因為您使用*列入白名單*多載和`Courses`不在 include 清單中。

如果沒有核取方塊已選取中的程式碼`UpdateInstructorCourses`初始化`Courses`具有空的集合導覽屬性：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

程式碼會執行迴圈，尋訪資料庫中所有的課程，並檢查每個已指派給講師的課程，以及在檢視中選取的課程。 為了協助達成有效率的搜尋，後者的兩個集合會儲存在 `HashSet` 物件中。

若課程的核取方塊已被選取，但課程並未位於 `Instructor.Courses` 導覽屬性中，則課程便會新增至導覽屬性的集合中。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

若課程的核取方塊未被選取，但課程卻位於 `Instructor.Courses` 導覽屬性中，則課程便會從導覽屬性的集合中移除。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

中*Views\Instructor\Edit.cshtml*，新增**課程**欄位陣列的核取方塊，新增下列程式碼之後立即`div`項目`OfficeAssignment`欄位和再`div`項目**儲存**按鈕：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

貼上之後的程式碼，如果分行符號和縮排看起來不太一樣這裡，手動修正所有項目，使它看起來像這裡所示。 縮排不一定要是完美的，但 `@</tr><tr>`、`@:<td>`、`@:</td>` 和 `@</tr>` 必須要如顯示般各自在獨立的一行上，否則您會接收到執行階段錯誤。

此程式碼會建立一個 HTML 表格，該表格中有三個資料行。 在每個資料行中，核取方塊的後方會是由課程號碼和標題組成的標題。 核取方塊都具有相同名稱 ("selectedCourses") 會告知模型繫結，它們會被視為一個群組。 `value`的值設定屬性的每個核取方塊`CourseID.`當頁面回傳時，模型繫結會將陣列傳遞至控制器所組成`CourseID`只核取方塊已選取的值。

當一開始呈現核取方塊時，即指派給講師的課程有`checked`選取 （顯示它們已核取） 的屬性。

變更課程指派之後, 您會想要能夠驗證所做的變更，當站台恢復`Index`頁面。 因此，您需要將資料行新增至該頁面中的資料表。 在此情況下，您不需要使用`ViewBag`物件，因為已在您想要顯示的資訊`Courses`導覽屬性`Instructor`您要將它傳遞給頁面做為模型的實體。

在  *Views\Instructor\Index.cshtml*，新增**課程**標題的正後方**Office**標題之下，如下列範例所示：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

然後，新增新的詳細資料格，緊接著辦公室位置的詳細資料格：

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

執行**Instructor 索引**頁面，以查看指派給每位講師的課程：

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

按一下 **編輯**講師，以查看 編輯 頁面上。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

變更一些課程指派，然後按一下**儲存**。 您所做的變更會反映在 [索引] 頁面上。

 注意： 這裡的方法來編輯講師課程資料時運作相當良好有限的數目的課程。 針對更大的集合，將需要不同的 UI 和不同的更新方法。  
 

## <a name="update-the-deleteconfirmed-method"></a>更新 DeleteConfirmed 方法

在  *InstructorController.cs*，刪除`DeleteConfirmed`方法，然後插入下列程式碼加以取代。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

此程式碼會進行下列變更：

- 如果講師指派任何部門的系統管理員身分，請從該部門中移除講師的指派。 不需要此程式碼中，您會取得參考完整性錯誤，如果您嘗試刪除某部門的系統管理員身分指派的講師。

此程式碼不會處理一位講師指派多個部門的系統管理員身分的案例。 在最後一個教學課程中，您將新增程式碼，可防止發生這種情況。

## <a name="add-office-location-and-courses-to-the-create-page"></a>將辦公室位置和課程新增至 [新增] 頁面

在  *InstructorController.cs*，刪除`HttpGet`並`HttpPost``Create`方法，然後在適當位置中新增下列程式碼：


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

此程式碼很類似您所見的 Edit 方法的不同之處在於一開始會選取任何課程。 `HttpGet` `Create`方法呼叫`PopulateAssignedCourseData`方法不是因為可能有選取，但在課程是為了提供空集合給`foreach`（否則檢視程式碼會擲回 null 參考例外狀況的檢視中的迴圈).

HttpPost Create 方法會將之前的範本程式碼來檢查驗證錯誤，並在資料庫中加入新的講師的課程導覽屬性中的每個所選的課程。 即使沒有模型錯誤，會加入課程，讓模型錯誤 （例如使用者鍵入了無效的日期） 時所做的任何課程選取項目，因此當頁面重新顯示並出現錯誤訊息，都會自動還原。

請注意，為了要能夠將課程新增到 `Courses` 導覽屬性，您必須將屬性以空集合初始化：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

作為在控制器程式碼中完成這項操作的替代方案，您可以在 Instructor 模型中藉由將屬性 getter 變更為在不存在時自動建立集合來完成，如以下範例所示：

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

若您使用這種方式修改了 `Courses` 屬性，您便可以移除控制器中的明確屬性初始化程式碼。

在  *Views\Instructor\Create.cshtml*、 新增辦公室位置文字方塊及課程核取方塊，雇用日期 欄位之後，以及之前**送出** 按鈕。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

貼上程式碼之後，修正換行和縮排，如先前所做的編輯頁面。

執行 [建立] 頁面，並新增一名講師。

![講師的課程所建立](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>處理交易

中所述[基本的 CRUD 功能教學課程](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)，預設的 Entity Framework 隱含實作了交易。 如案例，您需要更多控制，例如，如果您想要包含在交易-Entity Framework 之外完成的作業，請參閱[使用交易](https://msdn.microsoft.com/data/dn456843)MSDN 上。

## <a name="summary"></a>總結

您現在已完成此操作相關資料的簡介。 到目前為止在這些教學課程中，您先前曾經使用執行同步 I/O 程式碼。 您可以讓應用程式藉由實作非同步程式碼，更有效率地使用 web 伺服器資源，這是您將在下一個教學課程中執行。

您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
