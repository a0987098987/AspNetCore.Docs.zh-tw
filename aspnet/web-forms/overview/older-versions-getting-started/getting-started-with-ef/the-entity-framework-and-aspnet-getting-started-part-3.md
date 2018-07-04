---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: 開始使用 Entity Framework 4.0 Database 中第一次與 ASP.NET 4 Web Form-第 3 部分 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 的 ASP.NET Web Forms 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 51964f86e66e21d63105f1043641c0096d1d41b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391143"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>開始使用 Entity Framework 4.0 Database 中第一次和 ASP.NET 4 Web Form-第 3 部分
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>篩選、 排序和分組資料

在您使用先前的教學課程中`EntityDataSource`控制項來顯示和編輯資料。 在本教學課程中，您將篩選、 排序和分組資料。 當您執行此動作所設定的屬性，`EntityDataSource`控制項，語法是不同於其他資料來源控制項。 如您所見，不過，您可以使用`QueryExtender`控制這些差異降到最低。

您將會變更*Students.aspx*頁面，即可篩選適用於學生，依名稱，並搜尋名稱排序。 您也會變更*Courses.aspx*頁面，即可依名稱顯示所選的科系與課程搜尋的課程。 最後，您將新增至學生統計資料*about.aspx 的網頁*頁面。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>使用 EntityDataSource"Where"屬性來篩選資料

開啟*Students.aspx*您在上一個教學課程中建立的頁面。 以目前設定，`GridView`頁面中的控制項顯示的所有名稱`People`實體集。 不過，您想要顯示只有學生才，您可以找到選取`Person`具有非 null 的註冊日期的實體。

若要切換**設計**檢視，並選取`EntityDataSource`控制項。 在 **屬性**視窗中，將`Where`屬性設`it.EnrollmentDate is not null`。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

您在中使用的語法`Where`屬性`EntityDataSource`控制項是 Entity SQL。 Entity SQL 是類似於 TRANSACT-SQL，但會在自訂實體，而不是資料庫物件的使用。 在運算式中`it.EnrollmentDate is not null`，這個字`it`表示查詢所傳回之實體的參考。 因此，`it.EnrollmentDate`是指`EnrollmentDate`屬性`Person`實體，`EntityDataSource`控制傳回。

執行網頁。 學生清單現在包含只學生。 (其中顯示沒有資料列沒有註冊日期。)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>使用順序資料 EntityDataSource"OrderBy 」 屬性

您也想要第一次顯示時，會以姓名順序排列此清單。 與*Students.aspx*仍在中開啟頁面**設計** 檢視中，且`EntityDataSource`仍已選取控制項，在**屬性** 視窗中設定**OrderBy**屬性設`it.LastName`。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

執行網頁。 學生清單現在是依據姓氏的順序。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>若要設定的"Where"屬性中使用控制項參數

因為與其他資料來源控制項，您可以傳遞參數值來`Where`屬性。 在  *Courses.aspx*頁面上，您在本教學課程第 2 部分中建立，您可以使用這個方法，以顯示相關聯的使用者從下拉式清單中選取部門的課程。

開啟*Courses.aspx* ，並切換至**設計**檢視。 新增第二`EntityDataSource`控制項加入頁面，並將它命名`CoursesEntityDataSource`。 將其連接到`SchoolEntities`模型，然後選取`Courses`作為**EntitySetName**值。

在 [**屬性**] 視窗中，按一下中的省略符號**其中**屬性方塊中。 (請確定`CoursesEntityDataSource`仍會使用之前為已選取控制項**屬性**視窗。)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**運算式編輯器**對話方塊隨即出現。 在此對話方塊中，選取**自動產生 Where 運算式會根據提供的參數**，然後按一下**加入參數**。 將參數命名`DepartmentID`，選取**控制**作為**參數來源**值，然後選取**DepartmentsDropDownList**為**ControlID**值。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

按一下 [**顯示進階屬性**，然後在**屬性**視窗**運算式編輯器**] 對話方塊中，變更`Type`屬性設`Int32`。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

當您完成時，按一下**確定**。

下拉式清單底下，新增`GridView`至頁面控制項並命名它`CoursesGridView`。 將其連接到`CoursesEntityDataSource`資料來源控制項，按一下**重新整理結構描述**，按一下 **編輯資料行**，並移除`DepartmentID`資料行。 `GridView`控制項標記類似下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

當使用者變更下拉式清單中的所選取的部門時，您會想自動變更的相關課程的清單。 若要將此動作，請選取下拉式清單中，然後在**屬性** 視窗中設定`AutoPostBack`屬性設`True`。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

既然您完成使用設計工具時，切換至**來源**檢視，並取代`ConnectionString`並`DefaultContainer`的屬性名稱`CoursesEntityDataSource`用來控制`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 當您完成時，控制項的標記看起來如下列範例所示。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

執行網頁，並使用下拉式清單選取不同的部門。 所選科系所提供的課程會顯示在`GridView`控制項。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>使用 [EntityDataSource]"GroupBy"屬性，來分組資料

假設 Contoso 大學想要將一些學生主體統計資料放在其 [關於] 頁面上。 具體來說，它想要顯示學生人數的細目，他們必須已註冊的日期。

開啟*about.aspx 的網頁*，然後在**來源**檢視中，取代現有的內容`BodyContent`之間使用 「 學生主體統計資料 」 來控制`h2`標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

在標題中加`EntityDataSource`控制項並命名它`StudentStatisticsEntityDataSource`。 將其連接到`SchoolEntities`，選取`People`實體集，並保留**選取**方塊在精靈中保持不變。 在 設定下列屬性**屬性**視窗：

- 若要篩選只學生版，請設定`Where`屬性設`it.EnrollmentDate is not null`。
- 若要依註冊日期將結果分組，將`GroupBy`屬性設`it.EnrollmentDate`。
- 若要選取的註冊日期和學生數目，設定`Select`屬性設`it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`。
- 若要依註冊日期排序結果，將`OrderBy`屬性設`it.EnrollmentDate`。

在**來源**檢視中，取代`ConnectionString`並`DefaultContainer`使用的屬性名稱`ContextTypeName`屬性。 `EntityDataSource`控制項標記現在類似下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

語法`Select`， `GroupBy`，並`Where`屬性，類似於 TRANSACT-SQL 除了`it`關鍵字，指定目前的實體。

加入下列標記建立`GridView`控制項來顯示資料。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

執行頁面，以查看顯示依註冊日期的學生數目的清單。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>QueryExtender 控制項用於篩選和排序

`QueryExtender`控制項可用來指定篩選和排序在標記中。 語法與您正在使用資料庫管理系統 (DBMS) 無關。 它也是通常獨立的 Entity Framework，與您用於導覽屬性的語法是 Entity Framework 唯一的例外狀況。

在本教學課程的這個部分您將使用`QueryExtender`控制項以篩選和訂單資料，以及其中一個排序依據欄位將會瀏覽屬性。

(如果您想要使用的程式碼而非標記，來擴充透過自動產生的查詢`EntityDataSource`控制項，您可以這麼做處理`QueryCreated`事件。 這是如何`QueryExtender`控制項延伸`EntityDataSource`也控制查詢。)

開啟*Courses.aspx*頁面上，與您先前新增的標記，下方插入下列標記來建立標題，文字方塊中輸入搜尋字串、 一個 [搜尋] 按鈕和`EntityDataSource`來繫結控制項`Courses`實體集。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

請注意，`EntityDataSource`控制項的`Include`屬性設定為`Department`。 在資料庫中，`Course`資料表不包含部門名稱，它包含`DepartmentID`外部索引鍵資料行。 如果您已取得部門名稱，連同課程資料查詢，直接的資料庫，您必須加入`Course`和`Department`資料表。 藉由設定`Include`屬性，以`Department`，指定 Entity Framework 怎麼取得相關的工作`Department`實體，當它取得`Course`實體。 `Department`接著會儲存在實體`Department`導覽屬性`Course`實體。 (根據預設，`SchoolEntities`類別所產生的資料模型設計工具會擷取相關的資料，當有需要以及您已資料來源控制項繫結至該類別，所以設定`Include`屬性不是必要。 不過，將它設定可以改善效能的頁面上，因為 Entity Framework 會將個別呼叫擷取資料的資料庫`Course`實體和相關`Department`實體。)

在後`EntityDataSource`控制您剛剛建立，插入下列標記來建立`QueryExtender`繫結的控制項`EntityDataSource`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression`項目會指定您想要選取的課程標題符合在文字方塊中輸入的值。 只會在文字方塊中輸入的字元數目的比較，因為`SearchType`屬性會指定`StartsWith`。

`OrderByExpression`項目會指定結果集，會依據部門名稱內的課程標題進行排序。 請注意指定部門名稱的方式： `Department.Name`。 因為之間的關聯`Course`實體和`Department`實體是一對一，`Department`導覽屬性會包含`Department`實體。 （如果這是一對多關聯性時，此屬性會包含集合）。若要取得的部門名稱，您必須指定`Name`屬性`Department`實體。

最後，新增`GridView`控制項來顯示課程清單：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

第一個資料行是範本欄位顯示部門名稱。 資料繫結運算式會指定`Department.Name`如同您在中所見，`QueryExtender`控制項。

執行網頁。 初始顯示一份所有課程為了依部門和課程標題。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

輸入"m"，然後按一下**搜尋**來查看所有課程標題開頭為"m"（搜尋是不區分大小寫）。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>使用"Like"運算子來篩選資料

您可以達到的效果類似於`QueryExtender`控制項的`StartsWith`， `Contains`，和`EndsWith`搜尋使用的型別`Like`中的運算子`EntityDataSource`控制項的`Where`屬性。 在本教學課程的這個部分，您會看到如何使用`Like`依名稱搜尋適用於學生的運算子。

開啟*Students.aspx*中**來源**檢視。 之後`GridView`控制項，加入下列標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

此標記是類似於什麼您稍早已見過除了`Where`屬性值。 第二部分`Where`運算式定義的子字串搜尋 (`LIKE %FirstMidName% or LIKE %LastName%`)，無論在文字方塊中輸入的第一個和最後一個名稱中搜尋。

執行網頁。 一開始您看到的所有學生的預設值，因為`StudentName`參數是"%"。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

在文字方塊中輸入字母"g"，然後按一下**搜尋**。 您看到的第一個或最後一個名稱中有"g"的學生的清單。

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

您已經顯示、 更新、 篩選、 排序，並分組資料，從個別的資料表。 在下一個教學課程中，您會開始處理相關的資料 （一對多案例）。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-4.md)
