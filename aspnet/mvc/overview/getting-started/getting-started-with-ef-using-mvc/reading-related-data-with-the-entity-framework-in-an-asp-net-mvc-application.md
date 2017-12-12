---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "讀取與 Entity Framework 中的 ASP.NET MVC 應用程式的相關的資料 |Microsoft 文件"
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f4912bb3113a8f9cdae4211e055a7e317ab2aff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>閱讀相關的 Entity Framework 的 ASP.NET MVC 應用程式中的資料
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一個教學課程中，您會完成學校資料模型。 在本教學課程，您將會讀取並顯示相關的資料，也就是，可將 Entity Framework 載入導覽屬性的資料。

下圖顯示的頁面，您將使用。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>延遲、 急切，和明確載入相關的資料

有數種方式，Entity Framework 可以將實體的導覽屬性載入相關的資料：

- *消極式載入*。 第一次讀取實體時，不被擷取相關的資料。 不過，第一次您嘗試存取的導覽屬性，該導覽屬性所需的資料自動擷取。 這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。 `DbContext`類別啟用預設消極式載入。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *積極式載入*。 實體讀取時，同時擷取的相關的資料。 這通常會導致單一聯結查詢以擷取所有所需的資料。 使用指定積極式載入`Include`方法。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明確式載入*。 這是類似於延遲載入，不同之處在於您明確地擷取相關的資料中的程式碼;當您存取導覽屬性時，它不會發生自動。 您載入相關的資料手動透過取得物件狀態管理員項目實體和呼叫[Collection.Load](https://msdn.microsoft.com/en-us/library/gg696220(v=vs.103).aspx)集合的方法或[Reference.Load](https://msdn.microsoft.com/en-us/library/gg679166(v=vs.103).aspx)保存的屬性方法單一實體。 (在下列範例中，如果您想要載入的系統管理員導覽屬性，您必須取代`Collection(x => x.Courses)`與`Reference(x => x.Administrator)`。)通常您會使用您已開啟消極式載入關閉時，才明確載入。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

因為它們不會立即擷取的屬性值，消極式載入和明確載入也都稱為*延後載入*。

### <a name="performance-considerations"></a>效能考量

如果您知道您需要針對擷取每個實體的相關的資料，積極式載入通常提供最佳效能，因為傳送至資料庫的單一查詢通常比針對擷取每個實體個別查詢更有效率。 例如，在上述範例中，假設每個部門各有十個相關的課程。 積極式載入範例會產生單一 （加入） 查詢和單一來回行程到資料庫。 消極式載入和明確載入範例會同時產生十一個查詢和 11 個往返資料庫。 延遲性很高時，特別是危害到效能額外反覆存取資料庫。

相反地，在某些情況下會更有效率消極式載入。 積極式載入可能會導致非常複雜聯結來產生，SQL Server 無法有效率地進行處理。 或者，如果您需要存取實體的導覽屬性，只會針對一組實體的子集正在處理，消極式載入可能會更好因為積極式載入會擷取比您所需的更多資料。 如果效能嚴重不足，所以最好先測試效能才能做出最好的選擇這兩種方式。

消極式載入，就能遮罩會造成效能問題的程式碼。 例如，程式碼不會指定立即或明確載入但處理高容量的實體，而且每個反覆項目中使用數個導覽屬性可能非常沒有效率 （因為許多反覆存取的資料庫）。 在開發使用在內部部署 SQL server 中執行的應用程式可能會發生效能問題時移至 Azure SQL Database，因為增加的延遲及消極式載入。 程式碼剖析實際的測試負載的資料庫查詢，可協助您判斷是否適用消極式載入。 如需詳細資訊，請參閱[Demystifying 實體架構策略： 載入相關資料](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx)和[使用 Entity Framework 來減少網路延遲到 SQL Azure](https://msdn.microsoft.com/en-us/magazine/gg309181.aspx)。

### <a name="disable-lazy-loading-before-serialization"></a>停用序列化之前的延遲載入

如果您離開消極式載入啟用序列化期間，您可以得到查詢比您預期多很多資料。 序列化通常運作所存取之型別的執行個體上的每個屬性。 屬性存取觸發記憶體消極式載入，且會序列化這些延遲載入的實體。 序列化程序會存取每一個屬性的延遲載入的實體，而可能造成更多的消極式載入和序列化。 若要避免 run-away 鏈結反應，請開啟消極式載入關閉之前序列化實體。

序列化也變得複雜由 Entity Framework 使用，proxy 類別中所述[進階案例的教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)。

一種方式以避免發生序列化問題為序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md)教學課程。

如果您不使用 DTOs，您可以停用消極式載入，並避免 proxy 問題，依[停用 proxy 建立](https://msdn.microsoft.com/en-US/data/jj592886.aspx)。

以下是一些其他[方式停用延遲載入](https://msdn.microsoft.com/en-US/data/jj574232):

- 對於特定的導覽屬性，請省略`virtual`關鍵字，當您宣告屬性。
- 對於所有導覽屬性，設定`LazyLoadingEnabled`至`false`，下列程式碼置於內容類別的建構函式： 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>建立課程頁面顯示部門名稱

`Course`實體包含導覽屬性，其中包含`Department`課程指派給部門的實體。 若要指派的部門名稱在清單中顯示的課程，您必須取得`Name`屬性從`Department`中的實體`Course.Department`導覽屬性。

建立名為控制器`CourseController`(不 CoursesController) 的`Course`實體類型，使用的相同選項**的 MVC 5 控制器與檢視，使用 Entity Framework** scaffolder 您稍早對`Student`控制站，如下圖所示：

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

開啟*Controllers\CourseController.cs*並查看`Index`方法：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動 scaffolding 已指定的積極式載入`Department`導覽屬性使用`Include`方法。

開啟*Views\Course\Index.cshtml*和範本程式碼取代為下列程式碼。 所做的變更會反白顯示：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

您已對 scaffold 的程式碼進行下列變更：

- 變更從標題**索引**至**課程**。
- 加入**數目**顯示的資料行`CourseID`屬性值。 根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。 不過，在此情況下的主索引鍵是有意義，而且您想要將其顯示。
- 移動**部門**至右側的資料行並變更其標題。 Scaffolder 正確選擇顯示`Name`屬性從`Department`實體，但應該是資料行標題的課程頁面在這裡**部門**而**名稱**。

請注意，[部門] 欄中，針對 scaffold 的程式碼會顯示`Name`屬性`Department`實體載入到`Department`導覽屬性：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

執行網頁 (選取**課程**Contoso 大學首頁上的索引標籤) 若要查看與部門名稱清單。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>建立講師頁面，其中顯示 Courses 以及註冊項目

本節中您會建立控制器和檢視`Instructor`實體，才能顯示講師頁面：

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

此頁面讀取，並會顯示相關的資料以下列方式：

- 講師清單會顯示相關的資料`OfficeAssignment`實體。 `Instructor`和`OfficeAssignment`實體是以 0 或-1 一個關聯性中。 您將使用的積極式載入`OfficeAssignment`實體。 如前所述，積極式載入時，通常更有效率主資料表的所有擷取的資料列所需的相關的資料。 在此情況下，您會想要顯示為顯示的所有講師 office 指派。
- 當使用者選取相關的講師`Course`實體便會顯示。 `Instructor`和`Course`實體是位於多對多關聯性。 您將使用的積極式載入`Course`實體和其相關`Department`實體。 在此情況下，消極式載入可能會更有效率因為您只會針對所選講師需要課程。 不過，此範例會示範如何使用積極式載入本身是導覽屬性中的實體內的導覽屬性。
- 當使用者選取的課程時，相關的資料`Enrollments`顯示實體集。 `Course`和`Enrollment`實體是位於一個對多關聯性。 您要加入的明確載入`Enrollment`實體和其相關`Student`實體。 （明確載入不必要的因為消極式載入已啟用，但這會顯示如何執行明確載入函式）。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>建立講師索引檢視的檢視模型

講師頁面會顯示三個不同的資料表。 因此，您將建立包含三個屬性，每個保留其中一個資料表的資料檢視模型。

在*ViewModels*資料夾中，建立*InstructorIndexData.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>建立講師控制器和檢視

建立`InstructorController`(不 InstructorsController) 控制器 EF 讀取/寫入動作，如下圖所示：

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

開啟*Controllers\InstructorController.cs*並加入`using`陳述式`ViewModels`命名空間：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Scaffold 中的程式碼`Index`方法會指定僅適用於的積極式載入`OfficeAssignment`導覽屬性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

取代`Index`下列的程式碼，以載入其他方法的相關資料，然後放入檢視模型：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

該方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，提供的識別碼值選取的講師和所選的課程，並將所有必要的資料傳遞至檢視。 參數所提供的**選取**頁面上的超連結。

程式碼首先會建立檢視模型的執行個體，並放在它的講師清單。 程式碼會指定的積極式載入`Instructor.OfficeAssignment`和`Instructor.Courses`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

第二個`Include`方法載入的課程，並載入每個課程會積極式載入`Course.Department`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

如先前所述，不需要積極式載入，但為了改善效能。 因為檢視一律需要`OfficeAssignment`實體，會更有效率，擷取在相同的查詢。 `Course`積極式載入優於延遲載入的頁面會顯示頻率課程，而不選取，才是在網頁上，選取講師時需要的實體。

如果選取講師識別碼，則選取的講師會擷取從講師中檢視模型的清單。 檢視模型`Courses`屬性接著會載入與`Course`實體從該講師`Courses`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where`方法會傳回集合，但在此情況下準則傳遞至該方法的結果中只有一個`Instructor`所傳回的實體。 `Single`方法會將集合轉換成單一`Instructor`實體，可讓您存取與該實體`Courses`屬性。

您使用[單一](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.single.aspx)集合，當您知道集合的方法將會有只有一個項目。 `Single`方法擲回例外狀況，如果是空的集合傳遞給它，或若有多個項目。 替代方式是[SingleOrDefault](https://msdn.microsoft.com/en-us/library/bb342451.aspx)，它會傳回預設值 (`null`在此情況下) 如果集合是空的。 不過，在此情況下，就仍然造成例外狀況 (從嘗試尋找`Courses`屬性`null`參考)，以及例外狀況訊息會較不清楚指出問題的原因。 當您呼叫`Single`方法，您也可以傳遞中`Where`條件，而不是呼叫`Where`方法分別：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

而非：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

接下來，如果已選取課程，選定的課程從擷取中檢視模型的課程的清單。 然後檢視模型的`Enrollments`屬性載入`Enrollment`實體從該課程`Enrollments`導覽屬性。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>修改講師索引檢視表

在*Views\Instructor\Index.cshtml*，範本程式碼取代為下列程式碼。 所做的變更會反白顯示：

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

您已對現有的程式碼進行下列變更：

- 變更模型類別`InstructorIndexData`。
- 變更頁面標題從**索引**至**講師**。
- 加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。 (因為這是以 0 或-1 一個關聯性，可能不會有相關`OfficeAssignment`實體。) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- 加入的程式碼將以動態方式新增`class="success"`至`tr`選取講師項目。 這會將選取的資料列使用的背景色彩[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)類別。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- 加入新`ActionLink`標示為**選取**之前中每個資料列的其他連結，讓所選的講師識別碼傳送至`Index`方法。

執行應用程式並選取**講師** 索引標籤。此頁面會顯示`Location`屬性的相關`OfficeAssignment`實體和空的資料表資料格時沒有相關`OfficeAssignment`實體。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

在*Views\Instructor\Index.cshtml*檔案，關閉後的`table`元素 （在檔案的結尾），加入下列程式碼。 此程式碼會顯示一份時選取講師講師相關的課程。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

此程式碼讀取`Courses`要顯示的課程清單的檢視模型的屬性。 它也提供`Select`超連結，將傳送至所選的課程識別碼`Index`動作方法。

執行網頁，然後選取 instructor。 現在您會看到一個方格，其中會顯示指派給所選講師，課程，每個課程中，您看到指派的部門名稱。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

之後您剛才加入的程式碼區塊，加入下列程式碼。 在課程中，選取該課程時，這會顯示一份學生註冊。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

此程式碼讀取`Enrollments`屬性的檢視模型，以顯示一份學生註冊課程。

執行網頁，然後選取 instructor。 然後，選取課程來查看已註冊的學生版和其等級清單。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>新增明確式載入

開啟*InstructorController.cs*並查看如何`Index`方法取得註冊項目所選課程的清單：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

當您擷取講師的清單時，您指定的積極式載入`Courses`導覽屬性以及`Department`屬性的每個課程。 然後您將`Courses`集合中檢視模型，並且現在正在存取`Enrollments`該集合中的某個實體的導覽屬性。 因為您沒有指定的積極式載入`Course.Enrollments`導覽屬性，該屬性的資料出現在頁面中，由於消極式載入。

如果您不需要變更任何其他方式中的程式碼停用延遲載入`Enrollments`屬性將會是 null，不論課程實際上有多少個註冊項目。 在此情況下，載入`Enrollments`屬性，您必須指定積極式載入或明確式載入。 您已經了解如何進行積極式載入。 若要查看明確載入的範例，取代`Index`方法，以下列程式碼，明確載入`Enrollments`屬性。 變更的程式碼會反白顯示。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

取得所選之後`Course`新的程式碼的實體明確載入該課程`Enrollments`導覽屬性：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

然後它會明確地載入每個`Enrollment`實體的相關`Student`實體：

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

請注意，您使用`Collection`方法以載入集合屬性，但為了保留一個實體的屬性，您使用`Reference`方法。

現在就執行講師索引頁，雖然您已變更資料擷取的方式，您會看到任何差異顯示在頁面上。

## <a name="summary"></a>總結

您現在使用所有三種方式 （延遲、 急切評估，以及明確） 相關的資料載入至導覽屬性。 在下一個教學課程中，您將學習如何更新相關的資料。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[下一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
