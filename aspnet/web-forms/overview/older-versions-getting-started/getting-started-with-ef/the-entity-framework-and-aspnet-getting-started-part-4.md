---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 4 部分 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 5f8b1c15fbfd2d65b603013db3902b42faa40665
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836784"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form-第 4 部分
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>使用相關的資料

在您使用先前的教學課程中`EntityDataSource`控制項來篩選、 排序和分組資料。 在本教學課程中，您將會顯示與更新相關的資料。

您將建立 Instructors 頁面會顯示講師的清單。 當您選取講師時，您會看到一份該講師所教授的課程。 當您選取課程時，您會看到課程和學生的課程註冊清單的詳細資料。 您可以編輯講師姓名、 雇用日期和辦公室指派。 辦公室指派是您透過導覽屬性來存取個別的實體集。

您可以將主要資料連結標記或程式碼的詳細資料。 在這個部分的教學課程中，您將使用這兩種方法。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>顯示和更新 GridView 控制項中的相關的實體

建立名為的新網頁*Instructors.aspx*使用*Site.Master*主版頁面，並新增下列標記來`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

此標記會建立`EntityDataSource`選取講師，並可讓更新的控制項。 `div`項目會設定標記以呈現在左邊，以便您稍後可以新增資料行右側。

之間`EntityDataSource`標記和結尾`</div>`標記中加入下列標記會建立`GridView`控制項和`Label`控制項，您將使用錯誤訊息：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

這`GridView`控制項可讓資料列選取範圍、 反白顯示選取的資料列，以淺灰色背景色彩，和指定的處理常式 （這稍後將建立）`SelectedIndexChanged`和`Updating`事件。 它也會指定`PersonID`針對`DataKeyNames`屬性，讓您可選取的資料列的索引鍵值可以傳遞到另一個您稍後會加入的控制項。

最後一個資料行包含講師的辦公室指派，它會儲存在導覽屬性之`Person`實體因為它來自相關聯的實體。 請注意，`EditItemTemplate`項目會指定`Eval`而不是`Bind`，因為`GridView`控制項無法直接繫結至導覽屬性才能加以更新。 您將更新程式碼中的辦公室指派。 若要這樣做，您需要的參考`TextBox`控制項，而且您將取得，並儲存在`TextBox`控制項的`Init`事件。

遵循`GridView`控制項是`Label`用於錯誤訊息的控制項。 控制項的`Visible`屬性是`false`，且檢視狀態已關閉，以便標籤將出現只時程式碼會使它顯示在回應錯誤。

開啟*Instructors.aspx.cs*檔案，並新增下列`using`陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

加入私用類別欄位後面的部分類別名稱宣告，以存放辦公室指派的文字方塊中的參考。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

新增的虛設常式`SelectedIndexChanged`事件處理常式，您將新增程式碼供稍後使用。 也將新增辦公室指派的處理常式`TextBox`控制項的`Init`事件，所以您可以參考`TextBox`控制項。 您將使用此參考來取得使用者輸入才能更新實體的導覽屬性相關聯的值。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

您將使用`GridView`控制項的`Updating`事件來更新`Location`相關聯的屬性`OfficeAssignment`實體。 新增下列處理常式`Updating`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

當使用者按一下 執行這個程式碼**更新**在`GridView`資料列。 程式碼來擷取使用 LINQ to Entities`OfficeAssignment`目前相關聯的實體`Person`實體，使用`PersonID`所選取的資料列，從事件引數。

程式碼接著會採用其中一個下列動作中的值而定`InstructorOfficeTextBox`控制項：

- 如果文字方塊的值，而沒有任何`OfficeAssignment`實體來更新，它會建立一個。
- 如果文字方塊的值，而沒有`OfficeAssignment`它會更新實體，`Location`屬性值。
- 如果文字方塊為空白，而且`OfficeAssignment`實體存在，它會刪除實體。

在此之後，它會將變更儲存到資料庫。 如果發生例外狀況，它會顯示一則錯誤訊息。

執行網頁。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

按一下 **編輯**，並將文字方塊中的所有欄位。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

變更這些值，包括**辦公室指派**。 按一下 **更新**，您會看到清單中所反映的變更。

## <a name="displaying-related-entities-in-a-separate-control"></a>在不同的控制項中顯示相關的實體

每一個講師可以教授一或多個課程，讓您將新增`EntityDataSource`控制項和`GridView`控制項，以列出相關聯任何選取的講師的課程`GridView`控制項。 若要建立標題和`EntityDataSource`控制項的課程實體，新增下列標記之間的錯誤訊息`Label`控制項並在結尾`</div>`標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where`參數包含的值`PersonID`其資料列中選取的講師的`InstructorsGridView`控制項。 `Where`屬性包含可取得所有相關聯的子選擇命令`Person`實體`Course`實體的`People`導覽屬性並選取`Course`實體只有當其中一個相關聯`Person`實體包含所選`PersonID`值。

若要建立`GridView`控制項。 加入下列標記的正後方`CoursesEntityDataSource`控制項 (在關閉前`</div>`標記):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

因為如果沒有選取，會不顯示任何課程`EmptyDataTemplate`就會包含項目。

執行網頁。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

選取一或多個課程指派的人員講師和課程清單中出現。 (注意： 雖然資料庫結構描述允許多個課程，提供與資料庫的測試資料中沒有講師確實具有一個以上的課程。 您可以加入課程資料庫自行使用**伺服器總管** 視窗或*CoursesAdd.aspx*頁面上，您將在稍後的教學課程新增。)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView`控制項會顯示只有少數的課程欄位。 若要顯示課程的所有詳細資料，您將使用`DetailsView`使用者選取課程的控制項。 在*Instructors.aspx*之後則會在結尾, 新增下列標記`</div>`標記 (請確定您將此標記**之後**結尾 div 標記，非之前):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

此標記會建立`EntityDataSource`繫結至控制項`Courses`實體集。 `Where`屬性可讓您選取課程，使用`CourseID`課程中所選取的資料列的值`GridView`控制項。 標記指定的處理常式`Selected`事件，這稍後會用來顯示學生成績，這是另一個階層中較低的層級。

在  *Instructors.aspx.cs*，建立下列虛設常式`CourseDetailsEntityDataSource_Selected`方法。 （您將填寫此虛設常式，稍後在本教學課程中，現在，您需要它，讓頁面會編譯並執行）。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

執行網頁。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

一開始沒有 course 詳細資料因為不選取任何課程。 選取講師課程指派的人員，然後選取 課程，以查看詳細資料。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>使用 EntityDataSource [選取] 以顯示相關的資料的事件

最後，您會想要顯示的所有已註冊的學生和其年級的所選的課程。 若要這樣做，您將使用`Selected`事件的`EntityDataSource`控制項繫結至課程`DetailsView`。

在  *Instructors.aspx*，將下列標記之後新增`DetailsView`控制項：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

此標記會建立`ListView`顯示學生和其所選課程的年級的清單控制項。 因為您將資料繫結程式碼中的控制項不指定任何資料來源。 `EmptyDataTemplate`項目會提供選取任何課程時所顯示的訊息，在此情況下，會顯示任何學生。 `LayoutTemplate`項目會建立 HTML 資料表，以顯示清單中，而`ItemTemplate`指定要顯示的資料行。 學生識別碼以及學生的成績等級是來自`StudentGrade`實體和學生名稱取自`Person`中的 Entity Framework 提供的實體`Person`導覽屬性`StudentGrade`實體。

在  *Instructors.aspx.cs*，取代虛設常式外`CourseDetailsEntityDataSource_Selected`為下列程式碼的方法：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

此事件的事件引數提供的集合，其中將包含零個項目，如果選取任何項目或一個項目表單中選取的資料如果`Course`選取實體。 如果`Course`實體已選取，此程式碼使用`First`方法，將集合轉換為單一物件。 它接著會取得`StudentGrade`實體的導覽屬性，將它們轉換成集合，並將繫結`GradesListView`控制項加入集合。

這是足夠顯示等級，但您想要確定空的資料範本中的訊息會顯示頁面會顯示第一次，而且未選取課程時。 若要這樣做，請建立下列方法，您將呼叫的兩個位置：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

呼叫此新方法，從`Page_Load`方法，以顯示頁面會顯示空的資料範本的第一個階段。 並從呼叫`InstructorsGridView_SelectedIndexChanged`方法選取講師時，會引發該事件，因為這表示新的課程會載入課程`GridView`控制項，而不尚未選取。 以下是在兩次呼叫：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

執行網頁。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

選取講師的課程指派，，然後選取 課程。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

您現在已看到一些相關的資料搭配使用。 在下列教學課程中，您將了解如何新增現有的實體之間的關聯性如何移除關聯性，以及如何新增現有的實體關聯性的實體。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-5.md)
