---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 4.0 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378906"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 3 部分： 排序和篩選
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您實作儲存機制模式中使用 Entity Framework 的多層式架構 web 應用程式和`ObjectDataSource`控制項。 本教學課程會示範如何進行排序和篩選，以及處理主從式案例。 您將新增的下列增強功能*Departments.aspx*頁面：

- 若要允許使用者依據名稱選取部門的文字方塊。
- 在方格中會顯示每個部門的課程清單。
- 能夠按一下資料行標題來排序。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>將功能新增至排序 GridView 資料行

開啟*Departments.aspx*頁面，然後新增`SortParameterName="sortExpression"`屬性設定為`ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`。 (稍後您將建立`GetDepartments`方法接受名為參數`sortExpression`。)控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

新增`AllowSorting="true"`屬性的開頭標記`GridView`控制項。 控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

在  *Departments.aspx.cs*，藉由呼叫設定的預設排序順序`GridView`控制項的`Sort`方法，從`Page_Load`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

您可以加入排序或篩選的程式碼中的商務邏輯的類別或儲存機制類別。 如果您這麼做，商務邏輯類別中，排序或篩選的工作時，必須從資料庫擷取資料之後因為商務邏輯類別使用`IEnumerable`存放庫所傳回的物件。 如果您將新增排序和篩選存放庫類別中的程式碼和 LINQ 運算式之前執行或已經轉換成物件查詢`IEnumerable`物件時，您的命令將會傳遞至資料庫以進行處理，通常更有效率。 在本教學課程中，您將實作排序和篩選會導致資料庫完成處理的方式 — 也就是儲存機制中。

若要加入排序功能，您必須加入的新方法的儲存機制介面和儲存機制類別以及有關商務邏輯類別。 在  *ISchoolRepository.cs*檔案中，加入新`GetDepartments`採用的方法`sortExpression`將用來排序所傳回的部門清單的參數：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression`參數會指定要排序的資料行和排序方向。

將新方法的程式碼加入*SchoolRepository.cs*檔案：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

變更現有無參數`GetDepartments`方法來呼叫新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

在測試專案中，新增下列新方法*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

如果您要建立任何依存於此方法傳回的已排序的清單的單元測試，您會需要排序的清單，再加以傳回。 您不會建立測試類似在本教學課程中，因此這個方法只會傳回部門的未排序的清單。

在  *SchoolBL.cs*檔案中，將下列新方法加入商務邏輯類別：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

此程式碼會將排序參數傳遞至儲存機制方法。

執行*Departments.aspx*頁面。

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

您現在可以按一下任何欄標題，依該資料行排序。 如果已排序資料行，按一下標題反轉的排序方向。

## <a name="adding-a-search-box"></a>新增搜尋方塊

在本節中，您將新增搜尋文字方塊中，將它連結到`ObjectDataSource`控制項使用一個控制項參數，並將方法加入商務邏輯類別，若要支援篩選。

開啟*Departments.aspx*頁面上，並新增下列標題與第一個標記`ObjectDataSource`控制項：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

在 `ObjectDataSource`控制項，名為`DepartmentsObjectDataSource`，執行下列動作：

- 新增`SelectParameters`名為參數的項目`nameSearchString`會取得與輸入中的值`SearchTextBox`控制項。
- 變更`SelectMethod`屬性值來`GetDepartmentsByName`。 （您稍後將建立此方法。）

標記`ObjectDataSource`控制項現在會類似下列範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

在  *ISchoolRepository.cs*，新增`GetDepartmentsByName`方法，同時`sortExpression`和`nameSearchString`參數：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

在  *SchoolRepository.cs*，新增下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

此程式碼使用`Where`方法，以選取包含搜尋字串的項目。 如果搜尋字串是空的則會選取所有記錄。 請注意，當您指定方法會在一個如下的陳述式一起呼叫 (`Include`，然後`OrderBy`，然後`Where`)，則`Where`方法必須一律為最後一個。

變更現有`GetDepartments`採用方法`sortExpression`參數來呼叫新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

在  *MockSchoolRepository.cs*在測試專案中，新增下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

在  *SchoolBL.cs*，新增下列新方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

執行*Departments.aspx*頁面，然後輸入搜尋字串，以確保選取邏輯運作正常。 將文字方塊保留空白，並嘗試以確定會傳回所有記錄搜尋。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>每個格線資料列加入詳細資料的資料行

接下來，您會想要查看所有顯示在右側的資料格方格的每個部門的課程。 若要這樣做，您將使用巢狀`GridView`控制和資料繫結它將資料從`Courses`導覽屬性`Department`實體。

開啟*Departments.aspx*和中的標記`GridView`控制項，指定處理常式的`RowDataBound`事件。 控制項的開頭標記的標記現在會類似下列的範例。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

加入新`TemplateField`之後的項目`Administrator`樣板欄位：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

此標記會建立巢狀`GridView`控制項可顯示課程編號和標題的講師的課程。 它不會指定資料來源，因為您將資料繫結中的程式碼在`RowDataBound`處理常式。

開啟*Departments.aspx.cs* ，並新增下列處理常式，如`RowDataBound`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

這個程式碼取得`Department`實體的事件引數，將轉換`Courses`導覽屬性來`List`集合，並將巢狀的`GridView`至集合。

開啟*SchoolRepository.cs*檔案，並指定積極式載入`Courses`導覽屬性，藉由呼叫`Include`方法，在您在建立物件查詢`GetDepartmentsByName`方法。 `return`中的陳述式`GetDepartmentsByName`方法現在會類似下列的範例。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

執行網頁。 排序和篩選您稍早新增的功能，除了 GridView 控制項現在會顯示每個部門巢狀的 course 詳細資料。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

如此即完成排序、 篩選，和主版詳細資料的案例簡介。 在下一個教學課程中，您會看到如何處理並行存取。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [下一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
