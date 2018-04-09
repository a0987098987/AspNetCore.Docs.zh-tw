---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 4 部分 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 第 4 部分
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>使用相關的資料

在上一個教學課程，您使用`EntityDataSource`控制項來篩選、 排序和分組資料。 在此教學課程中，將顯示和更新的相關的資料。

您將建立顯示一份講師講師頁面。 當您選取講師時，您會看到一份由該講師教導的課程。 當您選取的課程時，您會看到程序過程以及一份學生課程中註冊的詳細資料。 您可以編輯講師姓名、 雇用日期，以及 office 指派。 Office 指派是您透過導覽屬性來存取個別的實體集。

您可以連結主要資料標記或程式碼的詳細資料。 在本教學課程的這個部分，您將使用這兩種方法。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>顯示和更新 GridView 控制項中的相關的實體

建立名為的新網頁*Instructors.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

這個標記會建立`EntityDataSource`講師和啟用更新控制項。 `div`項目會呈現在左邊，您可以將資料行稍後新增右邊的標記。

之間`EntityDataSource`標記和結束`</div>`標記中，加入下列標記建立`GridView`控制項和`Label`控制項，您將使用的錯誤訊息：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

這`GridView`控制項可讓資料列選取、 反白顯示選取的資料列以淺灰色背景色彩，並指定處理常式 （您將在稍後建立） 的`SelectedIndexChanged`和`Updating`事件。 它也會指定`PersonID`如`DataKeyNames`屬性，以便在所選取的資料列索引鍵的值可以傳遞至另一個您稍後會加入的控制項。

最後一個資料行包含講師 office 指派會儲存在瀏覽屬性的`Person`實體因為它來自相關聯的實體。 請注意，`EditItemTemplate`項目會指定`Eval`而不是`Bind`，因為`GridView`控制項不能直接繫結至導覽屬性才能更新它們。 您要更新程式碼中的 office 指派。 若要這樣做，您必須參考`TextBox`控制項，而且您會取得並儲存於`TextBox`控制項的`Init`事件。

遵循`GridView`控制項是`Label`錯誤訊息所使用的控制項。 控制項的`Visible`屬性是`false`，及檢視狀態已關閉，使僅當程式碼會顯示錯誤回應中出現的標籤。

開啟*Instructors.aspx.cs*檔案，然後加入下列`using`陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

部分類別名稱宣告，以保存 office 指派文字方塊中的參考之後，立即將私用類別欄位。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

新增虛設常式`SelectedIndexChanged`您要稍後加入程式碼的事件處理常式。 也新增 office 指派的處理常式`TextBox`控制項的`Init`事件，讓您可以儲存的參考`TextBox`控制項。 您將使用這個參考來取得使用者輸入才能更新實體的導覽屬性相關聯的值。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

您將使用`GridView`控制項的`Updating`事件來更新`Location`屬性相關聯的`OfficeAssignment`實體。 加入下列處理常式`Updating`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

當使用者按一下 執行這個程式碼**更新**中`GridView`資料列。 程式碼使用 LINQ to Entities 擷取`OfficeAssignment`目前與相關聯的實體`Person`實體，使用`PersonID`之選取的資料列的事件引數。

程式碼接著會使用其中一個動作中的值而定`InstructorOfficeTextBox`控制項：

- 在文字方塊中的值而且沒有任何`OfficeAssignment`實體，若要更新，它會建立一個。
- 在文字方塊中的值而且沒有`OfficeAssignment`便會更新實體，`Location`屬性值。
- 如果是空的文字方塊和`OfficeAssignment`實體存在，它會刪除實體。

在此之後，它會將所做的變更儲存到資料庫。 如果發生例外狀況，它會顯示錯誤訊息。

執行網頁。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

按一下**編輯**並在文字方塊中，變更所有欄位。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

變更任何這些值，包括**Office 指派**。 按一下**更新**您將會看到反映清單中的變更。

## <a name="displaying-related-entities-in-a-separate-control"></a>在個別控制項中顯示相關的實體

每個講師可以教導一或多個課程，讓您將新增`EntityDataSource`控制項和`GridView`控制項清單與任何講師講師中選取相關聯的課程`GridView`控制項。 若要建立的標題和`EntityDataSource`課程實體控制項，加入下列標記之間的錯誤訊息`Label`控制項和關閉`</div>`標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where`參數包含的值`PersonID`講師中選取的資料列的`InstructorsGridView`控制項。 `Where`屬性包含可取得所有相關聯的子選擇命令`Person`實體`Course`實體的`People`導覽屬性，並選取`Course`實體只有當其中一個相關聯的`Person`實體包含所選`PersonID`值。

若要建立`GridView`控制項。 加入下列標記緊接`CoursesEntityDataSource`控制項 (在關閉前`</div>`標記):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

因為如果沒有講師選取時，就會顯示沒有課程`EmptyDataTemplate`就會包含項目。

執行網頁。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

選取 講師具有指派，一個或多個課程，課程出現在清單中。 (注意： 雖然資料庫結構描述允許多個課程，提供與資料庫的測試資料中沒有講師確實具有一個以上的課程。 您可以加入課程資料庫自行使用**伺服器總管**視窗或*CoursesAdd.aspx*頁面上，您將新增更新的教學課程中。)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView`控制項會顯示只有幾個課程欄位。 若要顯示的課程的所有詳細資料，您將使用`DetailsView`課程使用者選取的控制項。 在*Instructors.aspx*，結尾後面加入下列標記`</div>`標記 (請確定您將這個標記**之後**右 div 標記中，非之前):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

這個標記會建立`EntityDataSource`繫結至控制項`Courses`實體集。 `Where`屬性選取課程使用`CourseID`課程中所選取的資料列的值`GridView`控制項。 標記指定的處理常式`Selected`事件，會顯示學生成績稍後使用，這是另一個階層中較低的層級。

在*Instructors.aspx.cs*，建立下列虛設常式`CourseDetailsEntityDataSource_Selected`方法。 （您可以填寫此虛設常式，稍後在本教學課程中，現在，您需要它，頁面將會編譯及執行）。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

執行網頁。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

一開始會沒有課程詳細資料，因為沒有課程已選取。 選取具有指派，課程講師，然後選取 課程來查看詳細資料。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>使用 EntityDataSource 「 選取 」 事件，以顯示相關的資料

最後，您會想要顯示所有已註冊的學生版和其等級所選的課程。 若要這樣做，您將使用`Selected`事件`EntityDataSource`控制項繫結至課程`DetailsView`。

在*Instructors.aspx*，加入下列標記之後`DetailsView`控制項：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

這個標記會建立`ListView`顯示一份其等級選取課程的學生的控制項。 沒有指定資料來源是因為您將資料繫結程式碼中的控制項。 `EmptyDataTemplate`項目會提供任何課程已不選取時顯示訊息，在此情況下，沒有任何要顯示的學生。 `LayoutTemplate`項目會建立 HTML 表格顯示清單中，而`ItemTemplate`指定要顯示的資料行。 學生識別碼 」 和 「 學生成績都是來自`StudentGrade`實體和學生名稱是來自`Person`實體，Entity Framework 可讓在`Person`導覽屬性`StudentGrade`實體。

在*Instructors.aspx.cs*，取代附加虛設常式超出`CourseDetailsEntityDataSource_Selected`方法取代下列程式碼：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

此事件的事件引數提供的集合，其中將包含零個項目，如果選取任何項目或一個項目表單中選取的資料如果`Course`選取實體。 如果`Course`選取實體時，程式碼會使用`First`方法，將集合轉換為單一物件。 接著會取得`StudentGrade`實體的導覽屬性，將它們轉換成一個集合，並繫結`GradesListView`控制項至集合。

這樣已足以顯示，但是您想要確定空的資料範本中的訊息會顯示第一次顯示網頁時，每當課程未選取。 若要這樣做，請建立下列方法，您會從兩個地方呼叫：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

呼叫此新方法，從`Page_Load`方法，以顯示頁面會顯示空的資料範本的第一個時間。 並從呼叫`InstructorsGridView_SelectedIndexChanged`方法講師選取時，會引發該事件，因為這表示新的課程會載入課程`GridView`尚未選取控制項和 none。 以下是兩個呼叫：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

執行網頁。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

選取已指派，課程講師，然後選取 課程。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

您現在有幾個方法來處理相關的資料。 在下列的教學課程中，您將學習如何加入現有的實體之間的關聯性如何移除關聯性，以及如何加入新的實體有現有的實體關聯性。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-5.md)
