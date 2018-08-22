---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 5 部分 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827220"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form-第 5 部分
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>使用相關的資料，繼續執行

您一開始在上一個教學課程中使用`EntityDataSource`相關的資料搭配使用的控制項。 顯示多個階層層級，並編輯導覽屬性中的資料。 在本教學課程中，您將繼續使用相關的資料，加入和刪除關聯性，並可加入新的實體，有現有的實體關聯性。

您將建立的頁面，將會指派給部門的課程。 部門已經存在，以及當您建立新的課程，同時您將建立它與現有的部門之間的關聯性。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

您也會建立多對多關聯性與運作方式，是將講師指派給課程 （新增您所選取的兩個實體之間的關聯性） 頁面中或移除講師的課程 (移除兩個實體之間的關聯性，您選取）。 在資料庫中，加入 instructor 與 course 之間的關聯性會導致新的資料列新增至`CourseInstructor`關聯資料表，移除關聯性包含刪除資料列從`CourseInstructor`關聯資料表。 不過，您可以在 Entity Framework 設定導覽屬性，而不會參考`CourseInstructor`明確資料表。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>將具有關聯性的實體加入至現有的實體

建立名為的新網頁*CoursesAdd.aspx*使用*Site.Master*主版頁面，並新增下列標記來`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

此標記會建立`EntityDataSource`控制項，可選取課程，可讓您插入，以及指定的處理常式`Inserting`事件。 您將使用的處理常式來更新`Department`導覽屬性，當新`Course`建立實體。

標記也會建立`DetailsView`用於加入新控制項`Course`實體。 標記會使用繫結的欄位`Course`實體屬性。 您必須輸入`CourseID`值，因為這不是系統產生的識別碼欄位。 相反地，就必須以手動方式建立時，指定課程是課程編號。

您使用的範本欄位`Department`導覽屬性因為導覽屬性不能搭配`BoundField`控制項。 樣板欄位提供下拉式清單選取部門。 下拉式清單繫結至`Departments`實體集利用`Eval`而非`Bind`，一次因為您無法直接瀏覽將屬性繫結才能加以更新。 您指定的處理常式`DropDownList`控制項的`Init`事件，所以您可以使用控制項的參考更新的程式碼所`DepartmentID`外部索引鍵。

在  *CoursesAdd.aspx.cs*只在部分類別宣告之後，將 類別欄位加入保存的參考`DepartmentsDropDownList`控制項：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

加入的處理常式`DepartmentsDropDownList`控制項的`Init`事件，所以您可以儲存控制項的參考。 這可讓您取得使用者所輸入的值，並使用它來更新`DepartmentID`的值`Course`實體。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

加入的處理常式`DetailsView`控制項的`Inserting`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

當使用者按一下`Insert`，則`Inserting`之前插入新記錄時，就會引發事件。 在處理常式的程式碼取得`DepartmentID`從`DropDownList`控制項，並使用它來設定值，將會用於`DepartmentID`屬性`Course`實體。

Entity Framework 會負責將加入此課程來`Courses`相關聯的導覽屬性`Department`實體。 它也會新增到 department`Department`導覽屬性`Course`實體。

執行網頁。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

輸入識別碼、 標題、 數字的信用額度，並選取某個部門，然後按一下 **插入**。

執行*Courses.aspx*頁面，然後選取相同的部門，以查看新的課程。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>使用 多對多關聯性

之間的關聯性`Courses`實體集和`People`實體集是多對多關聯性。 A`Course`實體具有一個名為的導覽屬性`People`可包含零個、 一個或多個相關`Person`實體 （代表講師指派給教授的課程）。 並`Person`實體具有一個名為的導覽屬性`Courses`可包含零個、 一個或多個相關`Course`實體 (代表課程指派給教導該講師的)。 一位講師可能會讓多個課程，是一門課程可能由多個講師進行教授， 在本節的逐步解說中，將會加入和移除之間的關聯性`Person`和`Course`藉由更新相關實體的導覽屬性的實體。

建立名為的新網頁*InstructorsCourses.aspx*使用*Site.Master*主版頁面，並新增下列標記來`Content`控制項，名為`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

此標記會建立`EntityDataSource`擷取名稱的控制項和`PersonID`的`Person`講師的實體。 A`DropDrownList`控制項繫結至`EntityDataSource`控制項。 `DropDownList`控制項中指定的處理常式`DataBound`事件。 您將使用此處理常式，以顯示課程的資料繫結兩個下拉式清單。

標記也會建立下列群組的控制項，以使用指派給所選取講師的課程：

- A`DropDownList`選取課程，以指派控制項。 這個控制項將會填入目前未指派給所選取講師的課程。
- A`Button`起始指派的控制項。
- A`Label`控制項來顯示錯誤訊息，如果指派會失敗。

最後，標記也會建立一組控制項用於移除所選講師的課程。

在  *InstructorsCourses.aspx.cs*，加上 using 陳述式：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

新增方法以填入顯示課程的兩個下拉式清單：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

此程式碼會取得所有課程，從`Courses`實體集，並都取得的課程`Courses`導覽屬性`Person`所選取講師的實體。 然後它會決定要指派給該講師的課程，並據以填入下拉式清單。

加入的處理常式`Assign`按鈕的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

這個程式碼取得`Person`實體所選取的講師，取得`Course`實體所選的課程，並將所選的課程，以`Courses`講師的導覽屬性`Person`實體。 然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。

加入的處理常式`Remove`按鈕的`Click`事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

這個程式碼取得`Person`實體所選取的講師，取得`Course`實體所選的課程，並移除所選的課程，從`Person`實體的`Courses`導覽屬性。 然後，它會將變更儲存到資料庫並重新填入下拉式清單，因此可以立即看到結果。

將程式碼加入`Page_Load`方法，以確保錯誤訊息不會顯示當沒有任何錯誤報告，並加入處理常式`DataBound`和`SelectedIndexChanged`講師下拉式清單來填入 [課程] 下拉式清單的事件：

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

執行網頁。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

選取講師。 <strong>指派課程</strong>下拉式清單會顯示講師不教授的課程而<strong>移除課程</strong>下拉式清單會顯示已指派給講師的課程。 在 <strong>指派課程</strong>區段中，選取課程，然後按一下<strong>指派</strong>。 課程移至<strong>移除課程</strong>下拉式清單。 選取的課程<strong>移除課程</strong>區段，然後按一下<strong>移除</strong><em>。</em> 課程移至<strong>指派課程</strong>下拉式清單。

您現在已看到一些更多相關的資料搭配使用的方法。 在下列教學課程中，您將了解如何使用資料模型中的繼承，以改善您的應用程式的可維護性。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-6.md)
