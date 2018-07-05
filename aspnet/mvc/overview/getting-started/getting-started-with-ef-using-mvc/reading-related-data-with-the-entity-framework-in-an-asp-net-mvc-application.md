---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 讀取相關的資料，使用 ASP.NET MVC 應用程式中的 Entity Framework |Microsoft Docs
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 24ee4cd1de4b2868af15513439a88e5363ee11e9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401272"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>讀取相關資料，使用 Entity Framework 中的 ASP.NET MVC 應用程式
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一個教學課程，您已完成 School 資料模型的內容。 在本教學課程中，您將讀取並顯示相關的資料-也就是 Entity Framework 載入到導覽屬性的資料。

下列圖例顯示了您將操作的頁面。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>延遲，積極式，明確載入相關資料

有數種方式，Entity Framework，可將相關的資料載入實體的導覽屬性：

- *消極式載入*。 第一次讀取實體時，不會擷取相關資料。 不過，第一次嘗試存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。 這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。 `DbContext`類別預設會啟用消極式載入。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *積極式載入*。 讀取實體時，將會同時擷取其相關資料。 這通常會導致單一聯結查詢，其可擷取所有需要的資料。 使用指定積極式載入`Include`方法。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明確式載入*。 這是類似於消極式載入，不同之處在於您明確地擷取相關的資料，在程式碼當您存取導覽屬性時，它不會發生自動。 您載入相關的資料手動藉由取得物件狀態管理員項目實體和呼叫[Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx)集合的方法或[Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx)保存屬性的方法單一實體。 (在下列範例中，如果您想要載入 [管理員] 瀏覽屬性中，您會取代`Collection(x => x.Courses)`與`Reference(x => x.Administrator)`。)通常您會使用您已開啟消極式載入關閉時，只明確載入。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

因為它們不立即擷取的屬性值，消極式載入和明確式載入同時所謂*延後載入*。

### <a name="performance-considerations"></a>效能考量

如果您知道擷取的每個實體需要相關資料，積極式載入通常可以提供最佳效能，因為傳送至資料庫的單一查詢通常比所擷取每個實體的個別查詢更有效率。 例如，在上述範例中，假設每個部門有十個相關的課程。 積極式載入範例都只是單一 （聯結） 查詢和單一來回行程到資料庫。 消極式載入和明確式載入範例會同時產生十一個查詢和十一個來回行程到資料庫。 當延遲很高時，資料庫的額外來回行程對效能特別不利。

相反地，在某些情況下會更有效率消極式載入。 積極式載入可能會導致非常複雜的聯結產生的 SQL Server 無法有效率地處理。 或者如果您需要存取實體的導覽屬性，只會針對一組實體的子集正在處理，消極式載入可能會比較好，因為積極式載入會擷取比您所需的更多資料。 如果效能嚴重不足，最好先測試這兩種方式的效能，才能做出最好的選擇。

消極式載入可加上遮罩會造成效能問題的程式碼。 例如，未指定積極式或明確載入，但處理大量的實體，而且每個反覆項目中使用數個導覽屬性的程式碼可能會非常沒有效率 （因為許多往返資料庫）。 在開發使用在內部部署 SQL server 中正常執行的應用程式可能會遇到效能問題移到 Azure SQL Database，因為增加的延遲和消極式載入時。 分析實際的測試負載的資料庫查詢，可協助您判斷是否適當消極式載入。 如需詳細資訊，請參閱[揭密 Entity Framework 策略： 載入相關的資料](https://msdn.microsoft.com/magazine/hh205756.aspx)並[使用 Entity Framework 對 SQL Azure 以減少網路延遲](https://msdn.microsoft.com/magazine/gg309181.aspx)。

### <a name="disable-lazy-loading-before-serialization"></a>停用消極式載入，序列化之前

如果您離開消極式載入已啟用在序列化期間，您可以得到查詢比您想要更多的資料。 序列化通常適用於藉由存取類型的執行個體上的每個屬性。 屬性存取就會觸發消極式載入，並會序列化這些消極式載入的實體。 序列化程序之後即可存取的消極式載入的實體，而可能造成更多的消極式載入及序列化的每個屬性。 若要避免 run-away 鏈結反應，請開啟消極式載入關閉之前序列化實體。

序列化也變得複雜的 Entity Framework 使用時，proxy 類別中所述[進階案例的教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)。

若要避免發生序列化問題的一個方式是序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md)教學課程。

如果您未使用 Dto，您可以停用消極式載入，並避免 proxy 問題，依[停用 proxy 建立](https://msdn.microsoft.com/data/jj592886.aspx)。

以下是一些其他[如何停用消極式載入](https://msdn.microsoft.com/data/jj574232):

- 針對特定的導覽屬性，請省略`virtual`關鍵字宣告的屬性。
- 對於所有導覽屬性，來設定`LazyLoadingEnabled`至`false`，置於您的內容類別的建構函式中的下列程式碼： 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>建立顯示部門名稱的 Courses 頁面

`Course`實體包含導覽屬性，其中包含`Department`指派課程的部門實體。 若要在課程清單中顯示所指派部門的名稱，您需要取得`Name`屬性從`Department`中的實體`Course.Department`導覽屬性。

建立的控制器命名為`CourseController`(不 CoursesController) 的`Course`實體類型，使用的相同選項**使用 MVC 5 控制器與檢視，Entity Framework**您先前為的框架`Student`控制器，如下圖所示：

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

開啟*Controllers\CourseController.cs*並查看`Index`方法：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動 Scaffolding 已使用 `Include` 方法，針對 `Department` 導覽屬性指定積極式載入。

開啟*Views\Course\Index.cshtml*並以下列程式碼取代範本程式碼。 所做的變更已醒目提示：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

您已對包含 Scaffold 的程式碼進行下列變更：

- 變更從標題**Index**要**課程**。
- 新增顯示 `CourseID` 屬性值的 [編號] 資料行。 根據預設，主索引鍵不會進行 scaffold 因為它們通常是使用者沒有任何意義。 不過，在此情況下主索引鍵有意義，因此您想要顯示它。
- 移動**部門**右側的資料行並變更其標題。 正確選擇要顯示的框架`Name`屬性從`Department`實體，但這裡的 [課程] 頁面的資料行標題應該**部門**而非**名稱**。

請注意，[部門] 欄中，針對包含 scaffold 的程式碼會顯示`Name`的屬性`Department`載入的實體`Department`導覽屬性：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

執行網頁 (選取**課程**的 Contoso 大學首頁上的索引標籤) 以查看含有部門名稱的清單。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立顯示課程 」 和 「 註冊 」 的 Instructors 頁面

在本節中，您會建立控制器並檢視`Instructor`實體以顯示 Instructors 頁面：

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

此頁面將以下列方式讀取和顯示相關資料：

- 講師清單會顯示相關的資料，從`OfficeAssignment`實體。 `Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。 您將使用積極式載入`OfficeAssignment`實體。 如上所述，當您需要主要資料表中所有擷取資料列的相關資料時，積極式載入通常更有效率。 在此情況下，您可能想要顯示所有已呈現講師的辦公室指派。
- 當使用者選取講師，相關`Course`實體便會顯示。 `Instructor` 與 `Course` 實體具有多對多關聯性。 您將使用積極式載入`Course`實體和其相關`Department`實體。 在此情況下，消極式載入可能會更有效率因為您需要只會針對所選講師的課程。 不過，這個範例會示範如何在本身處於導覽屬性內的實體中，針對導覽屬性使用積極式載入。
- 當使用者選取課程時，相關資料`Enrollments`顯示實體集。 `Course` 與 `Enrollment` 實體具有一對多關聯性。 您將新增的明確式載入`Enrollment`實體和其相關`Student`實體。 （明確式載入不必要的因為已啟用消極式載入，但這會顯示如何執行明確式載入函式）。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立 Instructor [索引] 檢視的檢視模型

Instructors 頁面會顯示三個不同的資料表。 因此，您將建立包含三個屬性的檢視模型，每個保留其中一個資料表的資料。

在  *Viewmodel*資料夾中，建立*InstructorIndexData.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>建立 Instructor 控制器和檢視

建立`InstructorController`(不 InstructorsController) 控制站使用 EF 讀取/寫入動作，如下圖所示：

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

開啟*Controllers\InstructorController.cs*並加入`using`陳述式`ViewModels`命名空間：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

中的 scaffold 程式碼`Index`方法指定積極式載入，僅適用於`OfficeAssignment`導覽屬性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

取代`Index`方法具有下列的程式碼，以載入其他相關資料，並將它放在檢視模型中：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，提供所選取的講師和所選的課程的識別碼值，並將所有必要的資料傳遞至檢視。 這些參數由頁面上的**選取**超連結提供。

此程式碼是從建立檢視模型的執行個體，並將其置於講師清單開始。 程式碼會指定積極式載入`Instructor.OfficeAssignment`而`Instructor.Courses`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

第二個`Include`方法載入課程，並針對每個載入的課程會積極式載入`Course.Department`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

如先前所述，不需要積極式載入，但是為了改善效能。 由於檢視一律需要`OfficeAssignment`實體，它是擷取該相同的查詢更有效率。 `Course` 因此積極式載入是優於消極式載入，而不需要選取比更常顯示頁面時，才在網頁上，選取講師時，所需的實體。

如果選取的講師識別碼，所選取的講師會從檢視模型中的講師清單中。 檢視模型`Courses`屬性然後充滿`Course`實體從該講師`Courses`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where`方法會傳回集合，但準則在此情況下傳遞至該方法的結果中只有一個`Instructor`所傳回的實體。 `Single`方法會將集合轉換成單一`Instructor`可讓您存取與該實體的實體`Courses`屬性。

您使用[單一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)集合，當您知道集合上的方法必須只有一個項目。 `Single`方法擲回例外狀況，如果是空的集合傳遞給它，或是否有多個項目。 替代方法是[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)，它會傳回預設值 (`null`在此情況下) 如果集合是空的。 不過，在此情況下，仍然會造成例外狀況 (由於嘗試尋找`Courses`屬性上的`null`參考)，和例外狀況訊息會不太清楚地指出問題的原因。 當您呼叫`Single`方法中，您也可以傳入`Where`條件，而不是呼叫`Where`方法分開：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

而非：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

接下來，如果已選取課程，則會從檢視模型的課程清單中擷取選取的課程。 然後，檢視模型的`Enrollments`屬性已載入`Enrollment`實體從該課程`Enrollments`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>修改 Instructor [索引] 檢視

在  *Views\Instructor\Index.cshtml*，以下列程式碼取代範本程式碼。 所做的變更已醒目提示：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

您已對現有程式碼進行下列變更：

- 已將模型類別變更為 `InstructorIndexData`。
- 已將頁面標題從**索引**變更為**講師**。
- 新增**辦公室**會顯示的資料行`item.OfficeAssignment.Location`只有當`item.OfficeAssignment`不是 null。 (因為這是一對零-或-一關聯性時，可能不會有相關`OfficeAssignment`實體。) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- 新增程式碼，將會以動態方式新增`class="success"`至`tr`所選取講師的項目。 這會將選取的資料列使用的背景色彩[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)類別。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- 已新增`ActionLink`標示**選取**正前方中每個資料列的其他連結，這會讓選取的講師識別碼傳送至`Index`方法。

執行應用程式，然後選取**講師** 索引標籤。此頁面會顯示`Location`屬性的相關`OfficeAssignment`實體和空的資料表資料格時有無相關`OfficeAssignment`實體。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

在  *Views\Instructor\Index.cshtml*檔案之後則會在結尾,`table`項目 （在檔案結尾），新增下列程式碼。 當選取講師時，此程式碼會顯示與講師相關的課程。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

此程式碼會讀取檢視模型的 `Courses` 屬性以顯示課程清單。 它也會提供`Select`超連結，會傳送至所選取課程的識別碼`Index`動作方法。

執行網頁，然後選取講師。 現在您會看到一個方格，其中顯示指派給所選取講師的課程，而且在每個課程中，您可以看到指派的部門名稱。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

在您剛才新增的程式碼區塊之後，新增下列程式碼。 這會在選取課程時，顯示已註冊該課程的學生清單。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

此程式碼會讀取`Enrollments`屬性以顯示學生的清單檢視模型的註冊過程中。

執行網頁，然後選取講師。 接著選取課程，以查看已註冊學生和其年級的清單。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>新增明確式載入

開啟*InstructorController.cs*並查看如何`Index`方法會取得所選取課程的註冊清單：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

當您擷取講師清單時，您會指定積極式載入`Courses`導覽屬性和`Department`屬性的每個課程。 然後您將放`Courses`在檢視模型中，而您正在存取的集合`Enrollments`從該集合中的某個實體的導覽屬性。 因為您沒有指定積極式載入`Course.Enrollments`導覽屬性，該屬性的資料出現在頁面中，由於消極式載入。

如果您不需要變更任何其他方式中的程式碼停用消極式載入`Enrollments`屬性將會是 null，不論課程實際上有多少的註冊項目。 在此情況下，載入`Enrollments`屬性，您必須指定積極式載入或明確式載入。 您已經看到如何進行積極式載入。 若要查看的明確式載入範例，取代`Index`方法，明確地載入為下列程式碼`Enrollments`屬性。 變更的程式碼會反白顯示。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

取得所選之後`Course`實體，新的程式碼，明確地載入該課程`Enrollments`導覽屬性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

然後它會明確地載入每個`Enrollment`實體的相關`Student`實體：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

請注意，您使用`Collection`方法來載入集合屬性，但會保留一個實體的屬性，您使用`Reference`方法。

立即執行 Instructor 索引 頁面，您會看到任何差異，在頁面上，顯示的內容，雖然您已變更資料擷取的方式。

## <a name="summary"></a>總結

您現在已使用三種方式 （lazy，積極式，和明確） 載入到導覽屬性的相關的資料。 在下一個教學課程中，您將了解如何更新相關資料。

您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [下一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
