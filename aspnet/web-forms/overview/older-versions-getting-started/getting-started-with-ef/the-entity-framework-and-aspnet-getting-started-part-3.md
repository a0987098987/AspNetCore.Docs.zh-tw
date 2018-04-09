---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: 開始使用 Entity Framework 4.0 資料庫中第一次與 ASP.NET 4 Web Form-第 3 部分 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 的 ASP.NET Web Form 應用程式。 範例應用程式是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>開始使用 Entity Framework 4.0 資料庫中第一次和 ASP.NET 4 Web Form 第 3 部分
====================
由[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>篩選、 排序和分組資料

在上一個教學課程，您使用`EntityDataSource`控制項以顯示和編輯資料。 在本教學課程中，您會篩選、 排序和分組資料。 當您執行此動作所設定的屬性，`EntityDataSource`控制語法是不同於其他資料來源控制項。 如您所見，不過，您可以使用`QueryExtender`控制這些差異降到最低。

您要變更*Students.aspx*頁面，即可篩選的學生，依名稱，並搜尋名稱排序。 您也要變更*Courses.aspx*頁面，即可依名稱顯示所選的部門 」 和 「 搜尋課程的課程。 最後，您會加入學生的統計資料*about.aspx 的網頁*頁面。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>使用 EntityDataSource"Where"屬性來篩選資料

開啟*Students.aspx*您在上一個教學課程中建立的頁面。 以目前設定，`GridView`頁面中的控制項會顯示來自所有名稱`People`實體集。 不過，您想要顯示只有學生，您可以找到選取`Person`具有非 null 註冊日期的實體。

切換至**設計**檢視和選取`EntityDataSource`控制項。 在**屬性**視窗中，將`Where`屬性`it.EnrollmentDate is not null`。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

在您使用的語法`Where`屬性`EntityDataSource`控制項是 Entity SQL。 Entity SQL 與 TRANSACT-SQL，類似，但自訂用於實體，而不是資料庫物件。 在運算式中`it.EnrollmentDate is not null`，word`it`表示查詢所傳回之實體的參考。 因此，`it.EnrollmentDate`指`EnrollmentDate`屬性`Person`實體，`EntityDataSource`控制傳回。

執行網頁。 學生清單現在包含只學生。 (其中顯示沒有資料列沒有註冊日期。)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>使用順序資料 EntityDataSource"OrderBy"屬性

您也想要名稱順序中第一次出現時的這份清單。 與*Students.aspx*仍在中開啟頁面**設計** 檢視中，與`EntityDataSource`仍然選取控制項，在**屬性**視窗中，將**OrderBy**屬性`it.LastName`。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

執行網頁。 學生清單現在為按照姓氏的順序。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>"Where"屬性設為使用控制參數

因為與其他資料來源控制項，您可以將參數值來傳遞`Where`屬性。 在*Courses.aspx*頁面上，您在第 2 部分的教學課程中建立，您可以使用這個方法以顯示使用者從下拉式清單中選取部門相關聯的課程。

開啟*Courses.aspx*並切換至**設計**檢視。 新增第二個`EntityDataSource`控制項加入頁面上，並將其命名`CoursesEntityDataSource`。 其連接至`SchoolEntities`模型，然後選取`Courses`為**EntitySetName**值。

在**屬性**視窗中，按一下省略符號在**其中**屬性方塊中。 (請確定`CoursesEntityDataSource`之前使用仍選取控制項**屬性**視窗。)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**運算式編輯器**對話方塊隨即出現。 在此對話方塊中，選取**自動產生 Where 運算式會根據提供的參數**，然後按一下 **加入參數**。 將參數`DepartmentID`，選取**控制項**為**參數來源**值，並選取**DepartmentsDropDownList**為**ControlID**值。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

按一下**顯示進階屬性**，然後在**屬性**視窗**運算式編輯器**對話方塊中，變更`Type`屬性`Int32`。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

當您完成時，按一下**確定**。

下方下拉式清單中，加入`GridView`控制項加入網頁並將其命名`CoursesGridView`。 其連接至`CoursesEntityDataSource`資料來源控制項，按一下 **重新整理結構描述**，按一下 **編輯資料行**，並移除`DepartmentID`資料行。 `GridView`控制項標記類似下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

當使用者變更選取的部門在下拉式清單中時，您會想自動變更相關聯的課程的清單。 為了讓這個動作，請選取下拉式清單，然後在**屬性**視窗中，將`AutoPostBack`屬性`True`。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

既然您完成使用設計工具時，切換至**來源**檢視，並取代`ConnectionString`和`DefaultContainer`命名的屬性`CoursesEntityDataSource`用來控制`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`屬性。 當您完成時，控制項的標記會看起來像下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

執行網頁，並使用下拉式清單選取不同的部門。 選取部門所提供的課程會顯示在`GridView`控制項。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>使用 EntityDataSource"GroupBy"屬性來群組資料

假設 Contoso 大學想要在其相關的頁面上放置一些學生主體統計資料。 具體來說，它想要它們註冊的日期顯示學生人數的細目。

開啟*about.aspx 的網頁*，然後在**來源**檢視中，取代現有的內容`BodyContent`控制 「 學生的主體統計資料 」 之間`h2`標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

在標題，後面加上`EntityDataSource`控制項並將其命名`StudentStatisticsEntityDataSource`。 其連接至`SchoolEntities`，選取`People`實體集，並保留**選取**方塊中未變更的精靈。 在中設定下列屬性**屬性**視窗：

- 若要篩選只學生版，請設定`Where`屬性`it.EnrollmentDate is not null`。
- 若要將結果分組註冊日期，設定`GroupBy`屬性`it.EnrollmentDate`。
- 若要選取註冊日期和學生數目，設定`Select`屬性`it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`。
- 若要註冊日期排序結果，設定`OrderBy`屬性`it.EnrollmentDate`。

在**來源**檢視中，取代`ConnectionString`和`DefaultContainer`名稱與屬性`ContextTypeName`屬性。 `EntityDataSource`控制項標記現在會類似下列的範例。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

語法`Select`， `GroupBy`，和`Where`屬性類似於 TRANSACT-SQL 除了`it`關鍵字會指定目前的實體。

加入下列標記建立`GridView`控制項來顯示資料。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

執行頁面，以查看顯示的學生總數一樣依註冊日期的清單。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>用於篩選和排序 QueryExtender 控制項

`QueryExtender`控制項提供方法來指定篩選和排序標記中。 語法與您正在使用資料庫管理系統 (DBMS) 無關。 它也是通常獨立的 Entity Framework 中，與您用於導覽屬性的語法是 Entity Framework 唯一的例外狀況。

在本教學課程的這個部分，您將使用`QueryExtender`控制篩選及排序資料，以及其中一個 [排序依據] 欄位將會瀏覽屬性。

(如果您想要使用程式碼而非標記，來擴充透過自動產生的查詢`EntityDataSource`控制項，您可以執行此動作處理`QueryCreated`事件。 這是如何`QueryExtender`控制項延伸`EntityDataSource`也控制查詢。)

開啟*Courses.aspx*頁面，然後以下您先前加入的標記，插入下列標記以建立標題時，文字方塊中輸入搜尋字串，[搜尋] 按鈕和`EntityDataSource`來繫結控制項`Courses`實體集。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

請注意，`EntityDataSource`控制項的`Include`屬性設定為`Department`。 在資料庫中，`Course`資料表不包含部門名稱，會包含`DepartmentID`外部索引鍵資料行。 如果您已取得的部門名稱，連同課程資料查詢直接的資料庫，您必須加入`Course`和`Department`資料表。 藉由設定`Include`屬性`Department`，您可以指定 Entity Framework 應有取得相關的工作`Department`實體時，它會取得`Course`實體。 `Department`實體會儲存在`Department`導覽屬性`Course`實體。 (根據預設，`SchoolEntities`資料模型設計工具所產生的類別會擷取相關的資料，當需要它，且您已將資料來源控制項繫結至該類別，因此設定`Include`屬性不是必要。 不過，將其設定，改善效能 頁面上，因為 Entity Framework 會將個別呼叫資料庫擷取資料`Course`實體以及相關`Department`實體。)

之後`EntityDataSource`控制您剛才建立的插入下列標記，以建立`QueryExtender`繫結的控制項`EntityDataSource`控制項。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression`項目會指定您想要選取的課程標題符合在文字方塊中輸入的值。 只會在文字方塊中輸入的字元數目的比較，因為`SearchType`屬性會指定`StartsWith`。

`OrderByExpression`項目可讓您指定的排序結果集是依部門名稱中的課程標題。 請注意指定部門名稱的方式： `Department.Name`。 因為之間的關聯`Course`實體和`Department`實體是一對一，`Department`導覽屬性包含`Department`實體。 （如果這是一對多關聯性，此屬性會包含集合）。若要取得的部門名稱，您必須指定`Name`屬性`Department`實體。

最後，會加入`GridView`控制項來顯示課程的清單：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

第一個資料行是顯示部門名稱的範本欄位。 資料繫結運算式會指定`Department.Name`，就像您在中看到`QueryExtender`控制項。

執行網頁。 初始的顯示順序顯示所有課程清單，依部門和課程標題。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

輸入"m"，然後按一下**搜尋**以查看其標題開頭為"m"（搜尋是不大小寫區分） 的所有課程。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>使用 [讚] 運算子來篩選資料

您可以達到的效果類似於`QueryExtender`控制項的`StartsWith`， `Contains`，和`EndsWith`搜尋類型使用`Like`中的運算子`EntityDataSource`控制項的`Where`屬性。 在本教學課程的這個部分，您會看到如何使用`Like`運算子來依名稱搜尋學生。

開啟*Students.aspx*中**來源**檢視。 之後`GridView`控制項，加入下列標記：

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

此標記是類似於所稍早除了`Where`屬性值。 第二個部分`Where`運算式定義的子字串搜尋 (`LIKE %FirstMidName% or LIKE %LastName%`)，不論在文字方塊中輸入的第一個和最後一個名稱中搜尋。

執行網頁。 一開始您看到所有的學生的預設值，因為`StudentName`參數是"%"。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

在文字方塊中輸入的字母"g"，然後按一下**搜尋**。 您會看到一份在第一個或最後一個名稱有"g"的學生。

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

您已現在顯示、 更新、 篩選、 排序和分組資料，從個別的資料表。 在下一個教學課程中開始，您將使用相關資料 （主版詳細資料案例）。

> [!div class="step-by-step"]
> [上一頁](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [下一頁](the-entity-framework-and-aspnet-getting-started-part-4.md)
