---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中您會實作儲存機制中的模式使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。 本教學課程會示範如何執行排序和篩選，以及處理主從式案例。 您會加入下列的增強功能*Departments.aspx*頁面：

- 若要允許使用者依名稱選取部門的文字方塊。
- 顯示在方格中每個部門的課程的清單。
- 按一下資料行標題來排序功能。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>將功能加入至排序 GridView 的資料行

開啟*Departments.aspx*頁面上，並加入`SortParameterName="sortExpression"`屬性`ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`。 (您將在稍後建立`GetDepartments`方法參數的名稱為`sortExpression`。)控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

新增`AllowSorting="true"`屬性的開頭標記`GridView`控制項。 控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

在*Departments.aspx.cs*，藉由呼叫設定預設的排序次序`GridView`控制項的`Sort`方法從`Page_Load`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

您可以加入程式碼來排序或篩選在商務邏輯類別或儲存機制類別。 如果您執行商務邏輯類別中，排序或篩選的工作會執行從資料庫擷取資料之後因為商務邏輯類別使用的`IEnumerable`儲存機制所傳回的物件。 如果您加入排序和篩選程式碼中的儲存機制類別和 LINQ 運算式之前執行物件查詢已經轉換成`IEnumerable`物件，您的命令將會傳遞到資料庫以進行處理，通常更有效率。 在本教學課程，您將實作的排序和篩選會使要由資料庫執行的處理方式 — 也就是在儲存機制中。

若要加入排序功能，您必須將新方法加入至儲存機制介面和儲存機制類別以及有關商務邏輯類別。 在*ISchoolRepository.cs*檔案中，加入新`GetDepartments`採用方法`sortExpression`會用來排序，則會傳回清單中的部門的參數：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression`參數會指定要排序的資料行和排序方向。

加入新的方法的程式碼*SchoolRepository.cs*檔案：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

變更現有無參數`GetDepartments`方法來呼叫新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

在測試專案中，新增下列新方法加入*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

如果您要建立任何單元測試，依存於這個方法傳回已排序的清單，您需要排序清單，再加以傳回。 您將不會建立測試像那樣在本教學課程，讓方法可以只傳回未排序的清單的部門。

在*SchoolBL.cs*檔案中，將下列新方法加入商務邏輯類別：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

此程式碼會將排序參數傳遞至儲存機制方法。

執行*Departments.aspx*頁面。

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

您現在可以按一下任何欄標題，排序依據該資料行。 如果已排序的資料行，按一下標題會反轉排序方向。

## <a name="adding-a-search-box"></a>新增搜尋方塊

本節中，您會加入 [搜尋] 文字方塊中，將其連結到`ObjectDataSource`控制使用控制參數，並將方法加入商務邏輯類別，若要支援篩選。

開啟*Departments.aspx*頁面上，加入下列標記之間的標題和第一個`ObjectDataSource`控制項：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

在`ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`，執行下列動作：

- 新增`SelectParameters`名為的參數項目`nameSearchString`來取得在輸入的值`SearchTextBox`控制項。
- 變更`SelectMethod`屬性值，以`GetDepartmentsByName`。 （您會建立這個方法之後）。

標記`ObjectDataSource`控制項現在會類似下列的範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

在*ISchoolRepository.cs*，新增`GetDepartmentsByName`採用兩者方法`sortExpression`和`nameSearchString`參數：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

在*SchoolRepository.cs*，加入下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

此程式碼使用`Where`方法，以選取包含搜尋字串的項目。 如果搜尋字串是空的則會選取所有記錄。 請注意，當您指定方法呼叫一起在如下的單一陳述式 (`Include`，然後`OrderBy`，然後`Where`)、`Where`方法必須永遠是最後一個。

變更現有`GetDepartments`採用方法`sortExpression`參數來呼叫新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

在*MockSchoolRepository.cs*在測試專案中，將下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

在*SchoolBL.cs*，加入下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

執行*Departments.aspx*頁面上，並輸入搜尋字串，並確定選取邏輯運作。 將文字方塊保留空白，然後再次嘗試搜尋，請確定傳回的所有記錄。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>每個格線資料列加入詳細資料的資料行

接下來，您會想要查看所有顯示在右邊的資料格方格的每個部門的課程。 若要這樣做，您將使用巢狀`GridView`控制項和資料繫結它將資料從`Courses`導覽屬性`Department`實體。

開啟*Departments.aspx*和中的標記`GridView`控制，請指定處理常式的`RowDataBound`事件。 控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

加入新`TemplateField`之後的項目`Administrator`範本欄位：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

這個標記會建立巢狀`GridView`控制項，顯示的課程編號和標題的課程的清單。 其並未指定資料來源，因為您將資料繫結中的程式碼在`RowDataBound`處理常式。

開啟*Departments.aspx.cs*並加入下列處理常式`RowDataBound`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

這個程式碼取得`Department`事件引數，從實體轉換`Courses`導覽屬性來`List`集合，並將巢狀的`GridView`至集合。

開啟*SchoolRepository.cs*檔案，並指定的積極式載入`Courses`導覽屬性，藉由呼叫`Include`方法中建立的物件查詢中`GetDepartmentsByName`方法。 `return`陳述式中的`GetDepartmentsByName`方法現在會類似下列的範例。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

執行網頁。 排序和篩選您先前加入的功能，除了 GridView 控制項現在會顯示每個部門的巢狀的課程詳細資料。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

如此即完成排序、 篩選，和主版詳細資料案例簡介。 在下一個教學課程中，您會看到如何處理並行存取。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
