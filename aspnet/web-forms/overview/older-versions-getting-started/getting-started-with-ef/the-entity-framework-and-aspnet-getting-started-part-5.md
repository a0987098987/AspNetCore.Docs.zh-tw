---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 5 部分 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7f351718123e881e832f4ac95af506ed601d3337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form-第 5 部分
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>使用相關資料，繼續

在上一個教學課程中您開始使用`EntityDataSource`控制項使用相關資料。 顯示多個階層層級，並編輯導覽屬性中的資料。 在本教學課程，您將會繼續使用相關的資料，加入和刪除關聯性，並可加入新的實體有現有的實體關聯性。

您將建立一個頁面，將會指派給部門的課程。 部門尚未存在，且當您建立新的課程，同時會建立它與現有的部門之間的關聯性。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

您也會建立搭配講師交給課程 （新增您所選取的兩個實體之間的關聯性） 的多對多關聯性的頁面或從課程移除講師 (移除兩個實體之間的關聯性，您選取）。 在資料庫中，加入一位講師和課程之間的關聯性會導致新的資料列加入至`CourseInstructor`關聯資料表，移除關聯性包含刪除資料列從`CourseInstructor`關聯資料表。 不過，您可以 Entity Framework 中設定導覽屬性，而不會參考`CourseInstructor`明確資料表。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>加入至現有的實體的實體與關聯性

建立名為的新網頁*CoursesAdd.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

這個標記會建立`EntityDataSource`控制項選取課程，可插入，並指定的處理常式`Inserting`事件。 您將使用此處理常式來更新`Department`導覽屬性時新`Course`建立實體。

標記也會建立`DetailsView`用於加入新控制項`Course`實體。 使用的繫結的欄位，標記`Course`實體屬性。 您必須輸入`CourseID`值，因為這不是系統產生的識別碼欄位。 相反地，它是時，必須指定手動課程建立的課程數字。

您使用的範本欄位`Department`導覽屬性因為導覽屬性不能與`BoundField`控制項。 [範本] 欄位提供下拉式清單選取部門。 下拉式清單繫結至`Departments`實體集使用`Eval`而不是`Bind`、 再次因為您無法直接將繫結導覽屬性才能更新它們。 指定的處理常式`DropDownList`控制項的`Init`事件，讓您可以儲存更新的程式碼使用的控制項的參考`DepartmentID`外部索引鍵。

在*CoursesAdd.aspx.cs*只在部分類別宣告之後，加入 類別欄位，以保存至`DepartmentsDropDownList`控制項：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

加入的處理常式`DepartmentsDropDownList`控制項的`Init`事件，讓您可以儲存至控制項的參考。 這可讓您取得使用者所輸入的值，並用來更新`DepartmentID`值`Course`實體。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

加入的處理常式`DetailsView`控制項的`Inserting`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

當使用者按一下`Insert`、`Inserting`插入新的記錄之前，就會引發事件。 此處理常式中的程式碼取得`DepartmentID`從`DropDownList`控制，並使用它來設定值，將會用於`DepartmentID`屬性`Course`實體。

Entity Framework 會負責將新增到本課程的`Courses`相關聯的導覽屬性`Department`實體。 它也會加入至部門`Department`導覽屬性`Course`實體。

執行網頁。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

輸入的識別碼、 標題、 數目信用額度，然後選取部門，然後按一下**插入**。

執行*Courses.aspx*頁面上，並選取相同的部門，以查看新的課程。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>使用 多對多關聯性

之間的關聯性`Courses`實體集和`People`實體集是多對多關聯性。 A`Course`實體具有名為的導覽屬性`People`可以包含零個、 一個或多個相關`Person`（代表指派給教導該課程講師） 的實體。 和`Person`實體具有名為的導覽屬性`Courses`可以包含零個、 一個或多個相關`Course`實體 (代表課程教導指派該講師)。 一個講師可能教導多個課程中，並可能由多個講師教一個課程。 本節的逐步解說中，您會加入及移除之間的關聯性`Person`和`Course`藉由更新相關實體的導覽屬性的實體。

建立名為的新網頁*InstructorsCourses.aspx*使用*Site.Master*主版頁面，並加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

這個標記會建立`EntityDataSource`擷取名稱的控制項和`PersonID`的`Person`講師的實體。 A`DropDrownList`控制項繫結至`EntityDataSource`控制項。 `DropDownList`控制項指定的處理常式`DataBound`事件。 您將使用此處理常式，以進行資料繫結這兩個下拉式清單中顯示的課程。

標記也會建立下列群組的指派給所選講師課程使用的控制項：

- A`DropDownList`控制項用於選取要指派的課程。 此控制項將會填入目前未指派給所選講師的課程。
- A`Button`起始作業的控制項。
- A`Label`控制項來顯示錯誤訊息，如果指派會失敗。

最後，標記也會建立一組用於課程移除所選講師控制項。

在*InstructorsCourses.aspx.cs*，加入 using 陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

加入填入顯示課程的兩個下拉式清單的方法：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

這個程式碼取得從所有課程`Courses`實體設定和都取得從課程`Courses`導覽屬性`Person`選取講師的實體。 它會判斷哪些課程指派給該講師並據以填入下拉式清單。

加入的處理常式`Assign`按鈕的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

這個程式碼取得`Person`選取講師，實體取得`Course`實體所選的課程中，並加入至所選的課程`Courses`講師的導覽屬性`Person`實體。 然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。

加入的處理常式`Remove`按鈕的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

這個程式碼取得`Person`選取講師，實體取得`Course`實體所選的課程中，並移除從所選的課程`Person`實體的`Courses`導覽屬性。 然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。

將程式碼加入`Page_Load`方法，以確保錯誤訊息不會顯示任何錯誤報告，並加入處理常式時`DataBound`和`SelectedIndexChanged`講師下拉式清單來擴展課程下拉式清單的事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

執行網頁。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

選取 [instructor]。 <strong>指派課程</strong>下拉式清單會顯示不會教導講師，課程和<strong>移除課程</strong>下拉式清單會顯示已指派給講師的課程。 在<strong>指派課程</strong>區段中，選取課程，然後按一下<strong>指派</strong>。 本課程會移至<strong>移除課程</strong>下拉式清單。 選取在課程<strong>移除課程</strong>區段，然後按一下<strong>移除</strong><em>。</em> 本課程會移至<strong>指派課程</strong>下拉式清單。

您現在有一些更多的方法來處理相關的資料。 在下列的教學課程中，您將學習如何使用資料模型中的繼承，以改善您的應用程式的可維護性。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)
