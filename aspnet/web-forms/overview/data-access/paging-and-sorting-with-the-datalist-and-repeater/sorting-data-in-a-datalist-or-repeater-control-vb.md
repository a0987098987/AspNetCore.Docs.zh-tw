---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: 排序 DataList 或 Repeater 控制項 (VB) 中的資料 |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將檢驗如何納入排序 DataList 和 Repeater 中, 支援，以及如何建構資料可以 DataList 或 Repeater...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fcbc1f83a00621ce0031cdcb775537992e3cb843
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828879"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>DataList 或 Repeater 控制項 (VB) 中的排序資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe)或[下載 PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> 在本教學課程中，我們將檢驗如何納入排序 DataList 和 Repeater 中, 支援，以及如何建構 DataList 或 Repeater 可以分頁和排序資料。


## <a name="introduction"></a>簡介

在 [先前的教學課程](paging-report-data-in-a-datalist-or-repeater-control-vb.md)我們檢查如何將分頁支援新增至 DataList。 我們建立中的新方法`ProductsBLL`類別 (`GetProductsAsPagedDataSource`) 傳回`PagedDataSource`物件。 當繫結至 DataList 或 Repeater、 DataList 或 Repeater 會顯示只是要求的頁面顯示的資料。 這個技術非常類似於用在內部 GridView、 DetailsView 和 FormView 控制項來提供他們的內建的預設分頁功能。

除了提供分頁支援，GridView 也包含內建排序支援。 DataList 或 Repeater 都不會提供內建排序功能;不過，排序功能可以使用新增的程式碼。 在本教學課程中，我們將檢驗如何納入排序 DataList 和 Repeater 中, 支援，以及如何建構 DataList 或 Repeater 可以分頁和排序資料。

## <a name="a-review-of-sorting"></a>檢閱排序

如我們在中所見[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-vb.md)教學課程中，提供現成的排序支援的 GridView 控制項。 每個 GridView 欄位可以有相關聯`SortExpression`，表示資料欄位，用來排序資料。 當 GridView s`AllowSorting`屬性設定為`true`，有每個 GridView 欄位`SortExpression`屬性值已轉譯為 LinkButton 標頭。 當使用者按一下特定的 GridView 欄位 s 標頭時，就會發生回傳和資料根據按下 [s] 欄位排序`SortExpression`。

GridView 控制項具有`SortExpression`屬性，它會儲存`SortExpression`[GridView] 欄位的資料會依照。 此外，`SortDirection`屬性會指出資料是否以遞增或遞減順序 （如果使用者按一下，就會切換為特定 GridView 的欄位標頭中的連結兩次連續，排序順序） 排序。

在其資料來源控制項繫結 GridView，它將其`SortExpression`和`SortDirection`屬性的資料來源控制項。 資料來源控制項擷取資料，並加以根據所提供排序`SortExpression`和`SortDirection`屬性。 完成排序之後的資料，資料來源控制項它傳回至 GridView。

若要複寫的 DataList 或 Repeater 控制項的這項功能，我們必須：

- 建立排序的介面
- 請記住要排序的資料欄位，以及是否要以遞增或遞減順序排序
- 指示要依特定資料欄位排序資料的 ObjectDataSource

我們將會處理這些步驟 3 和 4 中的三個工作。 接下來，我們將檢驗如何包含分頁和排序中 DataList 或 Repeater 的支援。

## <a name="step-2-displaying-the-products-in-a-repeater"></a>步驟 2： 顯示產品中

我們會擔心實作任何排序相關的功能之前，讓開始要先列出的產品在中繼器控制項中的 s。 首先開啟`Sorting.aspx`頁面中`PagingSortingDataListRepeater`資料夾。 將重複項控制項新增至網頁上，設定其`ID`屬性設`SortableProducts`。 從 Repeater s 智慧標籤，建立名為新 ObjectDataSource`ProductsDataSource`並將它設定為從中擷取資料`ProductsBLL`類別的`GetProducts()`方法。 選取 （無） 選項從下拉式清單中，在 INSERT、 UPDATE 和 DELETE 的索引標籤中。


[![建立 ObjectDataSource，並將它設定為使用 GetProductsAsPagedDataSource() 方法](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**圖 1**： 建立 ObjectDataSource，並將它設定為使用`GetProductsAsPagedDataSource()`方法 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![設定下拉式清單中更新、 插入和刪除索引標籤為 （無）](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**圖 2**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


不同於與，Visual Studio 不會自動建立`ItemTemplate`Repeater 控制項繫結至資料來源之後。 此外，我們必須將此新增`ItemTemplate`以宣告方式，如 Repeater 控制項 s 智慧標籤缺少 DataList s 中找到的 [編輯範本] 選項。 可讓 s 使用相同`ItemTemplate`從上一個教學課程中，其中顯示 s 產品名稱、 供應商和類別目錄。

在新增之後`ItemTemplate`，Repeater 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

圖 3 顯示此頁面上，當透過瀏覽器檢視。


[![會顯示每項產品 s 的名稱，供應商和類別目錄](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**圖 3**： 顯示每個產品的名稱、 供應商和類別目錄 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>步驟 3： 指示來排序資料的 ObjectDataSource

若要排序顯示中繼器中的資料，我們必須通知 ObjectDataSource 的資料應該排序的排序運算式。 ObjectDataSource 擷取其資料之前，它首先會引發其[`Selecting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)，可讓我們指定的排序運算式。 `Selecting`事件處理常式傳遞類型的物件[ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)，其中包含一個名為屬性[ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx)型別的[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)。 `DataSourceSelectArguments`類別設計來將資料相關的要求資料的取用者從傳遞至資料來源控制項，並包含[`SortExpression`屬性](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)。

若要從 ASP.NET 網頁的排序資訊傳遞到 ObjectDataSource 中，建立 事件處理常式`Selecting`事件並使用下列程式碼：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression*應將值指派要排序的資料 （例如產品名稱） 的資料欄位的名稱。 沒有任何排序方向相關屬性，因此如果您想要排序的資料，以遞減順序，將字串附加 DESC 來*sortExpression*值 （例如 ProductName DESC)。

請繼續並嘗試一些不同硬式編碼的值為*sortExpression*和瀏覽器中測試的結果。 如 圖 4 所示，使用做為 ProductName DESC 時*sortExpression*，產品會依其名稱以反向字母順序排序。


[![產品會依其名稱以反向字母順序排序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**圖 4**: 產品會依其名稱以反向字母順序排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>步驟 4： 建立排序的介面，並記住的排序運算式和方向

開啟排序 GridView 裡的支援會將每個排序欄位 s 標頭文字轉換成 LinkButton，按一下時，排序的資料依此。 這類排序的介面的合理 gridview，其中資料整齊的版面配置資料行。 DataList 與重複項控制項中，不過，不同的排序介面需要。 排序的通用介面，取得一份資料 （而不是資料方格的），是提供資料可以排序依據的欄位的下拉式清單。 可讓 s 本教學課程中實作這類介面。

新增上述的 DropDownList Web 控制項`SortableProducts`Repeater 並將其`ID`屬性設`SortBy`。 從 [屬性] 視窗中，按一下省略符號，`Items`顯示 ListItem 集合編輯器的屬性。 新增`ListItem`來排序資料所`ProductName`， `CategoryName`，和`SupplierName`欄位。 也將新增`ListItem`依其名稱以反向字母順序排序的產品。

`ListItem` `Text`屬性可以設定為任何值 （例如名稱），但`Value`屬性必須設定為資料欄位的名稱 （例如產品名稱）。 若要排序的結果以遞減順序，將字串附加 DESC 的資料欄位名稱，例如 ProductName DESC。


![每個可排序的資料欄位中新增清單項目](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**圖 5**： 新增`ListItem`每個可排序的資料欄位


最後，新增按鈕 Web 控制項右邊的 DropDownList。 設定其`ID`要`RefreshRepeater`及其`Text`屬性，以重新整理。

在建立之後`ListItem`s，新增 [重新整理] 按鈕的 DropDownList 和按鈕 s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

使用排序下拉式清單中完成，我們接下來需要更新 ObjectDataSource s`Selecting`事件處理常式，因此它會使用所選`SortBy``ListItem`s`Value`屬性，而不是硬式編碼的排序運算式。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

在第一次造訪網頁時，此點產品會一開始依`ProductName`資料欄位，因為它 s `SortBy` `ListItem`預設選取 （請參閱 圖 6）。 選取不同的排序選項，例如類別和按一下 重新整理將會導致回傳，並重新排序資料分類名稱，如 圖 7 所示。


[![產品現已依其名稱一開始排序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**圖 6**: 產品會依其名稱一開始排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![現在依類別排序的產品現已](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**圖 7**: 產品會立即依類別排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> 按一下 [重新整理] 按鈕會導致資料自動重新排序因為 Repeater 的檢視狀態已停用，因而導致重新繫結至它在每次回傳的資料來源的重複項。 如果您已保留重複項的檢視狀態已啟用，變更排序下拉式清單贏得 t 的清單會有任何影響的排序次序。 若要解決此問題，建立 [重新整理] 按鈕 s 的事件處理常式`Click`事件，並重新繫結至其資料來源的重複項 (藉由呼叫 Repeater 的`DataBind()`方法)。


## <a name="remembering-the-sort-expression-and-direction"></a>記住的排序運算式和方向

建立非排序相關，可能會發生回傳時，在頁面上的可排序的 DataList 或 Repeater 時它 s 命令式的排序運算式和方向會記住在回傳之間。 例如，假設我們在本教學課程，以包含每種產品的 [刪除] 按鈕，更新 Repeater。 當使用者按一下 [刪除] 按鈕 d 我們執行一些程式碼來刪除選取的產品，然後重新繫結之資料的重複項。 如果排序詳細資料不會保存在回傳中，螢幕上顯示的資料將會還原成原始的排序順序。

本教學課程中，DropDownList 以隱含方式儲存排序運算式和方向其檢視狀態中我們。 如果我們使用不同的排序介面其中一個使用所提供的各種不同的排序選項的說，Linkbutton d 我們要請務必記得在回傳之間的排序順序。 這可藉由在頁面的檢視狀態中，儲存排序的參數，藉由在查詢字串，或透過一些其他的狀態持續性技術，包括排序參數。

未來的範例，在本教學課程會探討如何保存頁面的檢視狀態中的排序詳細資料。

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>步驟 5： 新增要使用預設的分頁 DataList 排序支援

在 [前述教學課程](paging-report-data-in-a-datalist-or-repeater-control-vb.md)我們檢查如何實作具有 DataList 的預設分頁。 可讓 s 延伸此先前的範例，以包含要排序的分頁的資料的能力。 首先開啟`SortingWithDefaultPaging.aspx`並`Paging.aspx`中的分頁`PagingSortingDataListRepeater`資料夾。 從`Paging.aspx`頁面上，按一下 [來源] 按鈕，以檢視 [s] 頁面的宣告式標記。 複製選取的文字 （請參閱 圖 8） 並將它貼到的宣告式標記`SortingWithDefaultPaging.aspx`之間`<asp:Content>`標記。


[![複寫中的宣告式標記&lt;asp: Content&gt; Paging.aspx SortingWithDefaultPaging.aspx 的標記](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**圖 8**： 複寫中的宣告式標記`<asp:Content>`標籤`Paging.aspx`要`SortingWithDefaultPaging.aspx`([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


複製之後宣告式標記，請將複製的方法和屬性中的`Paging.aspx`頁面上的程式碼後置類別 s 程式碼後置類別`SortingWithDefaultPaging.aspx`。 接下來，請花一點時間檢視`SortingWithDefaultPaging.aspx`瀏覽器中的頁面。 它應該會表現相同的功能和外觀`Paging.aspx`。

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>增強 ProductsBLL 包含預設的分頁和排序方法

我們在上一個教學課程中建立`GetProductsAsPagedDataSource(pageIndex, pageSize)`方法中的`ProductsBLL`類別傳回`PagedDataSource`物件。 這`PagedDataSource`物件已填入*所有*的產品 (經由 BLL s`GetProducts()`方法)，但是當繫結至 DataList 對應到指定的記錄*pageIndex*並*pageSize*顯示輸入的參數。

稍早在本教學課程中加入排序支援藉由指定的排序運算式，從 ObjectDataSource 的`Selecting`事件處理常式。 這非常適合 ObjectDataSource 時傳回的物件，可以進行排序，例如`ProductsDataTable`所傳回`GetProducts()`方法。 不過，`PagedDataSource`所傳回的物件`GetProductsAsPagedDataSource`方法不支援其內部資料來源的排序。 相反地，我們需要從傳回的結果進行排序`GetProducts()`方法*之前*我們將它放`PagedDataSource`。

若要達成此目的，建立新的方法，在`ProductsBLL`類別， `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`。 若要排序`ProductsDataTable`所傳回`GetProducts()`方法，指定`Sort`屬性，其預設的`DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource`方法只會稍微不同從`GetProductsAsPagedDataSource`在上一個教學課程中建立的方法。 特別是，`GetProductsSortedAsPagedDataSource`接受額外的輸入的參數`sortExpression`，並將指派此值可`Sort`屬性`ProductDataTable`s `DefaultView`。 幾行程式碼更新版本中，`PagedDataSource`指派給物件 s 資料來源`ProductDataTable`s `DefaultView`。

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>呼叫 GetProductsSortedAsPagedDataSource 方法並指定 SortExpression 輸入參數的值

使用`GetProductsSortedAsPagedDataSource`方法完成下, 一個步驟是為此參數提供值。 內的 ObjectDataSource`SortingWithDefaultPaging.aspx`目前設定為呼叫`GetProductsAsPagedDataSource`方法，並透過其兩個的兩個輸入參數中傳遞`QueryStringParameters`，指定在`SelectParameters`集合。 這兩個`QueryStringParameters`表示的來源`GetProductsAsPagedDataSource`方法 s *pageIndex*並*pageSize*參數來自的 querystring 欄位`pageIndex`和`pageSize`。

更新 ObjectDataSource s`SelectMethod`屬性，因此它會叫用新`GetProductsSortedAsPagedDataSource`方法。 然後，新增`QueryStringParameter`如此*sortExpression*輸入的參數從查詢字串欄位存取`sortExpression`。 設定`QueryStringParameter`s`DefaultValue`到 ProductName。

這些變更之後，請 ObjectDataSource s 宣告式標記看起來應該類似：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

此時，`SortingWithDefaultPaging.aspx`頁面會依字母順序排序其結果，依產品名稱 （請參閱 圖 9）。 這是因為根據預設，產品名稱的值會當做傳入`GetProductsSortedAsPagedDataSource`方法 s *sortExpression*參數。


[![根據預設，結果會按照產品名稱](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**圖 9**： 根據預設，結果會依照`ProductName`([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


如果您手動新增`sortExpression`這類的 querystring 欄位`SortingWithDefaultPaging.aspx?sortExpression=CategoryName`將排序結果所指定`sortExpression`。 不過，這`sortExpression`移至不同的資料頁時，查詢字串中不包含參數。 事實上，按一下下一步] 或 [最後一個頁面上的按鈕會讓我們回到`Paging.aspx`！ 此外，有 s 目前未排序的介面。 使用者可以變更分頁的資料的排序次序的唯一方法是藉由直接操作查詢字串。

## <a name="creating-the-sorting-interface"></a>建立排序的介面

我們必須先更新`RedirectUser`方法，將傳送至使用者`SortingWithDefaultPaging.aspx`(而非`Paging.aspx`)，以及包含`sortExpression`於 querystring 中的值。 我們也應該新增唯讀、 頁面層級名為`SortExpression`屬性。 此屬性，類似於`PageIndex`並`PageSize`先前的教學課程中建立的屬性傳回的值`sortExpression`查詢字串欄位，如果存在，而且預設值 (ProductName) 否則。

目前`RedirectUser`方法會接受只有一個輸入的參數來顯示頁面的索引。 不過，可能是我們想要將使用者重新導向到特定的頁面，使用查詢字串中指定何種 s 以外的排序運算式的資料。 稍後我們將建立此頁面上，其中包含一系列按鈕 Web 控制項所指定的資料行排序資料的排序介面。 按一下其中一個這些按鈕時，我們想要傳入適當的排序運算式的值將使用者重新導向。 若要提供這項功能，建立兩個版本`RedirectUser`方法。 第一個應該接受頁面索引顯示，而第二個可接受的頁面索引和排序運算式。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

在本教學課程的第一個範例，我們會建立使用 dropdownlist 進行排序介面。 此範例中，讓 s 使用上述其中一個 DataList 定位排序的三個按鈕 Web 控制項`ProductName`，下列其中一個用於`CategoryName`，，另一個用於`SupplierName`。 新增三個按鈕 Web 控制項、 設定其`ID`和`Text`屬性適當地：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

接下來，建立`Click`每個事件處理常式。 事件處理常式應該呼叫`RedirectUser`方法，以使用適當的排序運算式的第一頁已註冊的使用者。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

當第一次瀏覽的頁面，資料會依產品名稱依字母順序排序 （請參閱上一步 圖 9）。 按一下 下一步 按鈕，前進到第二個資料頁，然後按一下 依類別目錄按鈕的 排序。 這會傳回我們的資料，依照類別目錄名稱的第一頁 （請參閱 圖 10）。 同樣地，由供應商 按鈕按一下排序會排序從資料的第一頁的供應商提供的資料。 因為資料透過分頁，會記住排序選擇。 圖 11 顯示後依分類排序，然後逐步引導到第十三個資料頁的頁面。


[![產品會依類別排序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**圖 10**: 產品會依類別排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![排序運算式會記住分頁透過資料的時間](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**圖 11**: 排序運算式會記住分頁透過資料的時間 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>步驟 6： 自訂分頁中的記錄

DataList 範例會檢查在步驟 5 的頁面，透過使用效率不佳的預設分頁技術及其資料。 當夠大量的資料進行分頁，務必使用自訂分頁。 回到[有效率地透過大型數量的資料分頁](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)並[排序自訂的分頁資料](../paging-and-sorting/sorting-custom-paged-data-vb.md)教學課程中，我們會檢查的 BLL 預設和自訂分頁與建立的方法之間的差異使用自訂的分頁和排序自訂分頁的資料。 特別是，這些兩個先前的教學課程中我們加入下列三種方法`ProductsBLL`類別：

- `GetProductsPaged(startRowIndex, maximumRows)` 傳回開始記錄的特定子集*startRowIndex*並不超過*maximumRows*。
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` 傳回排序所指定的記錄的特定子集*sortExpression*輸入的參數。
- `TotalNumberOfProducts()` 提供在中的記錄總數`Products`資料庫資料表。

這些方法可用來有效率地頁面上，並使用 DataList 或 Repeater 控制項的資料進行排序。 為了說明這點，讓開始使用自訂的分頁支援; 建立 Repeater 控制項的 s然後，我們將新增排序功能。

開啟`SortingWithCustomPaging.aspx`頁面中`PagingSortingDataListRepeater`資料夾，並將重複項新增至頁面上，設定其`ID`屬性設`Products`。 從 Repeater s 智慧標籤，建立名為新 ObjectDataSource `ProductsDataSource`。 設定以選取資料的來源`ProductsBLL`類別的`GetProductsPaged`方法。


[![設定為使用 ProductsBLL 類別的 GetProductsPaged 方法的 ObjectDataSource](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**圖 12**： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProductsPaged`方法 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


設定下拉式清單中更新、 插入和刪除為 [（無）] 索引標籤，然後按 [下一步] 按鈕。 設定資料來源精靈現在會提示輸入的來源`GetProductsPaged`方法 s *startRowIndex*並*maximumRows*輸入參數。 實際上，會忽略這些輸入的參數。 相反地， *startRowIndex*並*maximumRows*的值將會在傳遞給`Arguments`中之 ObjectDataSource 的屬性`Selecting`事件處理常式，就像我們所指定的方式*sortExpression*在此教學課程 s 第一個示範。 因此，保留參數來源下拉式清單，在精靈中設定為 None。


[![保留參數的 [來源] 設定為 None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**圖 13**： 保留參數來源設定為 None ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> 請勿*未*設定 ObjectDataSource s`EnablePaging`屬性設`true`。 這會自動包含它自己的 ObjectDataSource *startRowIndex*並*maximumRows*參數`SelectMethod`s 現有的參數清單。 `EnablePaging`屬性時，繫結自訂分頁至 GridView、 DetailsView 或 FormView 控制項的資料，因為這些控制項預期從 ObjectDataSource s 特定行為僅適用於當`EnablePaging`屬性是`true`。 因為我們必須以手動方式新增 DataList 與重複項分頁支援，保留這個屬性設定為`false`（預設值），因為我們將直接在我們的 ASP.NET 頁面內試吃中所需的功能。


最後，定義 Repeater 的`ItemTemplate`以便顯示 s 產品名稱、 類別和供應商。 這些變更之後，請重複項與 ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

請花一點時間瀏覽的頁面，透過瀏覽器，並記下會傳回任何記錄。 這是因為我們尚未以指定 ve *startRowIndex*並*maximumRows*參數值; 因此，值為 0 會傳入兩個。 若要指定這些值，建立事件處理常式 ObjectDataSource s`Selecting`事件及設定這些參數值以程式設計方式將值硬式編碼的 0 到 5，分別：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

透過這項變更，頁面上，當透過瀏覽器中，檢視會顯示前五個產品。


[![會顯示前五筆記錄](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**圖 14**： 會顯示前五筆記錄 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> [圖 14] 中所列出之產品剛好排序依產品名稱，因為`GetProductsPaged`會執行有效率的自訂分頁查詢的預存程序來排序結果所`ProductName`。


若要讓使用者逐步進行頁面，我們要追蹤的起始資料列索引和最大的資料列，並記得在回傳之間的這些值。 預設分頁範例中使用查詢字串欄位來保存這些值;此示範中，讓保存這項資訊頁面的檢視狀態中的 s。 建立下列兩個屬性：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

接下來，更新選取的事件處理常式中的程式碼，使其使用`StartRowIndex`和`MaximumRows`屬性，而不是 0 到 5 的硬式編碼值：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

此時我們網頁仍會顯示前五個的記錄。 不過，具有下列屬性的位置，我們準備好建立我們的分頁介面。

## <a name="adding-the-paging-interface"></a>新增分頁介面

可讓的使用相同的第一個、 上一步，接下來，最後一個分頁介面用於預設分頁範例中，包括在檢視會顯示哪些資料頁的控制項的標籤 Web 和多少的總頁數存在。 新增四個按鈕 Web 控制項和 Repeater 下方的標籤。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

接下來，建立`Click`的四個按鈕的事件處理常式。 按一下其中一個按鈕時，我們需要更新`StartRowIndex`和重新繫結之資料的重複項。 [名字]、 [上一個] 和 [下一步] 按鈕的程式碼相當簡單，但最後一個按鈕執行我們如何判斷資料的最後一頁開始的資料列索引嗎？ 若要計算此索引，以及能夠判斷是否應該啟用下一步] 和 [最後一個按鈕我們需要知道總共的多少筆記錄會透過已分頁。 我們可以呼叫來判斷這`ProductsBLL`類別的`TotalNumberOfProducts()`方法。 建立名為唯讀、 頁面層級屬性，可讓 s`TotalRowCount`傳回的結果`TotalNumberOfProducts()`方法：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

與這個屬性，我們現在可以判斷的最後一個頁面的起始資料列索引。 具體來說，它 s 整數結果的`TotalRowCount`減 1 除以`MaximumRows`乘以`MaximumRows`。 現在我們可以撰寫`Click`四個分頁介面按鈕的事件處理常式：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

最後，我們需要時檢視資料 和 下一頁 和 最後一個 按鈕的第一頁，檢視的最後一頁時，停用分頁介面中的第一個 和 上一頁 按鈕。 若要這麼做，請將下列程式碼新增至 ObjectDataSource 的`Selecting`事件處理常式：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

新增這些之後`Click`事件處理常式和程式碼，以啟用或停用介面分頁的項目已根據目前的起始資料列索引，測試網頁瀏覽器中。 如 [圖 15 所示，當第一次瀏覽頁面的第一個和上一步] 按鈕將會停用。 按一下 [下一步] 顯示的資料，第二個頁面，而按一下最後一個會顯示最後一頁 （請參閱圖 16，17）。 檢視資料的最後一頁時就會停用 下一步 和 最後一個按鈕。


[![[上一步] 和 最後一個按鈕就會停用檢視第一個頁面的產品](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**圖 15**： 檢視第一個頁面的產品時停用前一個和最後一個按鈕 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![第二個頁面的產品現已 Dispalyed](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**圖 16**: 第二個頁面的產品是 Dispalyed ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![按一下最後一個顯示資料的最後一頁](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**圖 17**： 按一下最後一個會顯示最後的頁面上的資料 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>步驟 7： 包括排序支援，以自訂分頁 Repeater

既然已實作自訂分頁，我們準備好要包含排序支援。 `ProductsBLL`類別 s`GetProductsPagedAndSorted`方法具有相同*startRowIndex*並*maximumRows*輸入做為參數`GetProductsPaged`，但允許其他*sortExpression*輸入的參數。 若要使用`GetProductsPagedAndSorted`方法從`SortingWithCustomPaging.aspx`，我們需要執行下列步驟：

1. 變更 ObjectDataSource s`SelectMethod`屬性從`GetProductsPaged`至`GetProductsPagedAndSorted`。
2. 新增*sortExpression* `Parameter`物件的 ObjectDataSource s`SelectParameters`集合。
3. 建立私用的頁面層級`SortExpression`在透過 [s] 頁面檢視狀態的回傳之間保存其值的屬性。
4. 更新 ObjectDataSource s`Selecting`事件處理常式來指派 ObjectDataSource s *sortExpression*參數的值的頁面層級`SortExpression`屬性。
5. 建立排序的介面。

藉由更新 ObjectDataSource s 開始`SelectMethod`屬性，並新增*sortExpression* `Parameter`。 請確定*sortExpression* `Parameter` s`Type`屬性設定為`String`。 完成前兩項工作之後, ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

接下來，我們需要的頁面層級`SortExpression`其值序列化至檢視狀態的屬性。 如果尚未設定任何排序運算式的值，使用產品名稱做為預設值：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

ObjectDataSource 會叫用之前`GetProductsPagedAndSorted`方法，我們需要設定*sortExpression* `Parameter`的值`SortExpression`屬性。 在 `Selecting`事件處理常式，加入下列程式碼行：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

所有剩下就是實作排序的介面。 如同我們在上一個範例，可讓 s 已實作 依產品名稱、 類別或供應商的 使用三個按鈕 Web 控制項可讓使用者排序結果的排序介面。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

建立`Click`這三個按鈕控制項的事件處理常式。 在事件處理常式中，重設`StartRowIndex`設成 0，設定`SortExpression`適當的值，並重新繫結至 Repeater 資料：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

S 就是這麼簡單 ！ 儘管有幾個步驟，以取得自訂的分頁和排序實作，步驟都非常類似於所需的預設分頁。 圖 18.顯示產品，檢視依類別排序的資料的最後一頁時。


[![顯示資料的最後一個頁面上，依類別排序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**圖 18**： 顯示 最後一頁的資料、 依類別排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> 在上一個範例中，請依供應商供應商名稱當做排序運算式排序時。 不過，自訂的分頁實作中，我們需要使用公司名稱。 這是因為預存程序負責實作自訂分頁`GetProductsPagedAndSorted`傳遞到排序運算式`ROW_NUMBER()`關鍵字，`ROW_NUMBER()`關鍵字需要實際的資料行名稱，而不是別名。 因此，我們必須使用`CompanyName`(在資料行名稱`Suppliers`資料表) 而不是使用中的別名`SELECT`查詢 (`SupplierName`) 的排序運算式。


## <a name="summary"></a>總結

既不 DataList 或 Repeater 提供內建的排序支援，但可加入一些程式碼和自訂的排序介面，這類功能。 在實作排序，但不是會進行分頁時，可以透過指定的排序運算式`DataSourceSelectArguments`物件傳遞到 ObjectDataSource 的`Select`方法。 這`DataSourceSelectArguments`物件 s`SortExpression`屬性中之 ObjectDataSource 指派`Selecting`事件處理常式。

若要將排序功能新增至 DataList 或 Repeater 已提供分頁支援，最簡單的方法是自訂商務邏輯層，以納入方法可接受的排序運算式。 這項資訊可以傳入透過 ObjectDataSource s 中的參數`SelectParameters`。

本教學課程中完成我們的分頁和排序的 DataList 與重複項控制項的檢查。 我們的下一個和最後一個教學課程將探討如何將按鈕 Web 控制項新增至 DataList 與重複項的 「 s 」 範本，以提供每個項目為基礎的一些自訂的使用者啟動的功能。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者為 David Suru。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
