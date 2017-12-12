---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: "在 DataList 或中繼器控制項 (VB) 中排序資料 |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們將檢驗如何納入排序 DataList 和中繼器中的支援，以及如何建構資料可以 DataList 或中繼器..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e3f505e525fd5e701bb40dc3e6467b880bf75447
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>DataList 或中繼器控制項 (VB) 中的排序資料
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe)或[下載 PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> 在本教學課程中，我們將檢驗如何納入排序 DataList 和中繼器中的支援，以及如何建構 DataList 或中繼器的可以分頁和排序資料。


## <a name="introduction"></a>簡介

在[上一個教學課程](paging-report-data-in-a-datalist-or-repeater-control-vb.md)我們檢驗如何將資料清單中的分頁支援。 我們建立了新的方法中`ProductsBLL`類別 (`GetProductsAsPagedDataSource`) 傳回`PagedDataSource`物件。 當繫結至 DataList 或中繼器，DataList 或中繼器會顯示只是要求的資料頁。 這個技術非常類似於用途內部 GridView、 DetailsView 和 FormView 控制項所提供的內建的預設分頁功能。

除了提供分頁支援，GridView 也包含現成排序支援。 DataList 都中繼器提供內建的排序功能。不過，排序功能可以使用新增許多程式碼。 在本教學課程中，我們將檢驗如何納入排序 DataList 和中繼器中的支援，以及如何建構 DataList 或中繼器的可以分頁和排序資料。

## <a name="a-review-of-sorting"></a>檢閱排序

如我們所見中[分頁和排序報表資料](../paging-and-sorting/paging-and-sorting-report-data-vb.md)教學課程中，排序支援現成提供的 GridView 控制項。 在 GridView 中的每個欄位可以有相關聯`SortExpression`，表示用來排序資料的資料欄位。 當 GridView s`AllowSorting`屬性設定為`true`，具有每個 GridView 欄位`SortExpression`屬性值已轉譯為 LinkButton 標頭。 當使用者按一下特定的 GridView 欄位 s 標頭時，就會發生回傳和資料根據按下欄位 s 排序`SortExpression`。

GridView 控制項有`SortExpression`屬性，其中儲存`SortExpression`GridView 欄位的資料會依照。 此外，`SortDirection`屬性會指出是否要以遞增或遞減順序 （如果使用者按一下，就會切換為特定 GridView 的欄位標頭中的連結兩次連續，排序順序） 排序資料。

在 GridView 繫結至其資料來源控制項，其傳遞其`SortExpression`和`SortDirection`屬性的資料來源控制項。 資料來源控制項擷取資料，然後將它排序依據提供`SortExpression`和`SortDirection`屬性。 完成排序之後的資料，資料來源控制項它傳回至 GridView。

若要複寫這項功能與 DataList 或中繼器控制項中，我們必須：

- 建立排序的介面
- 請記住要排序的資料欄位，以及是否要以遞增或遞減順序排序
- 指示排序依據特定資料欄位資料 ObjectDataSource

我們將會處理這些步驟 3 和 4 中的三個工作。 接下來，我們將檢驗如何同時包含分頁和排序 DataList 或中繼器中的支援。

## <a name="step-2-displaying-the-products-in-a-repeater"></a>步驟 2： 在中繼器中顯示的產品

我們會擔心實作的任何排序相關功能之前，可讓 s 列出中繼器控制項中的產品。 先開啟`Sorting.aspx`頁面`PagingSortingDataListRepeater`資料夾。 中繼器控制項加入網頁上，設定其`ID`屬性`SortableProducts`。 從中繼器 s 智慧標籤，建立名為新 ObjectDataSource`ProductsDataSource`並將它設定為從中擷取資料`ProductsBLL`類別的`GetProducts()`方法。 選取 （無） 選項從下拉式清單中，插入、 更新和刪除索引標籤中。


[![建立 ObjectDataSource，並將它設定為使用 GetProductsAsPagedDataSource() 方法](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**圖 1**： 建立 ObjectDataSource，並將它設定為使用`GetProductsAsPagedDataSource()`方法 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![設定下拉式清單中更新、 插入和刪除索引標籤為 （無）](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**圖 2**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


不同於與 DataList，Visual Studio 不會自動建立`ItemTemplate`中繼器控制項繫結至資料來源之後。 此外，我們必須加入`ItemTemplate`以宣告方式，因為中繼器控制項 s 智慧標籤缺少 DataList s 中找到的 [編輯樣板] 選項。 可讓 s 使用的相同`ItemTemplate`，從上一個教學課程中，顯示產品的名稱、 供應商和類別目錄。

在新增之後`ItemTemplate`，中繼器和 ObjectDataSource s 宣告式標記看起來應該類似下列：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

圖 3 顯示透過瀏覽器檢視時，此頁面。


[![會顯示每個產品 s 的名稱，供應商，以及類別目錄](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**圖 3**： 會顯示每個產品的名稱、 供應商和類別目錄 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>步驟 3： 指示排序資料 ObjectDataSource

若要排序中繼器中顯示的資料，我們需要通知的排序運算式的資料應該排序 ObjectDataSource。 ObjectDataSource 擷取其資料之前，先引發其[`Selecting`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)，這樣會提供機會，讓我們來指定排序運算式。 `Selecting`事件處理常式會傳遞類型的物件[ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)，其具有內容，名為[ `Arguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx)型別的[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx)。 `DataSourceSelectArguments`類別為了將相關的資料要求資料的取用者從傳遞至資料來源控制項，並包含[`SortExpression`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)。

若要從 ASP.NET 頁面 ObjectDataSource 傳遞排序資訊，請建立事件處理常式`Selecting`事件並使用下列程式碼：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression*来排序的資料 （例如產品名稱） 的資料欄位的名稱應該指派值。 沒有排序方向相關的屬性，因此如果您想要排序的資料，以遞減順序，將字串附加 DESC 至*sortExpression*值 （例如 ProductName DESC)。

請繼續並再試一次的某些不同硬式編碼值*sortExpression*和瀏覽器中測試的結果。 如圖 4 所示，使用為 ProductName DESC 時*sortExpression*，產品會依其名稱，以反向字母順序排序。


[![產品名稱來排序其以反向字母順序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**圖 4**: 產品會依照其名稱以反向字母順序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>步驟 4： 建立排序的介面，並記住的排序運算式和方向

開啟排序 GridView 中的支援會將每一個 s 可排序的欄位標頭文字轉換成 LinkButton，按一下時，排序資料據此。 這類排序的介面有意義的 GridView，其中資料整齊地配置資料行中。 DataList 和中繼器控制項，不過，不同的排序介面需要。 排序的通用介面，取得一份資料 （而不是資料方格），是提供資料可以排序的欄位的下拉式清單。 可讓 s 實作本教學課程的這類介面。

將上述 DropDownList Web 控制項`SortableProducts`中繼器並設定其`ID`屬性`SortBy`。 從 [屬性] 視窗中，按一下省略符號，`Items`屬性，以顯示清單項目集合編輯器。 新增`ListItem`s 排序的資料`ProductName`， `CategoryName`，和`SupplierName`欄位。 也新增`ListItem`由其名稱以反向字母順序排序的產品。

`ListItem` `Text`屬性可以設定為任何值 （例如名稱），但`Value`屬性必須設定為資料欄位的名稱 （例如產品名稱）。 若要排序的結果以遞減順序，將附加至像是 ProductName DESC 的資料欄位名稱的字串 DESC。


![新增清單項目，每個可排序資料欄位](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**圖 5**： 新增`ListItem`每個可排序資料欄位


最後，在右邊的 DropDownList 加入按鈕 Web 控制項。 設定其`ID`至`RefreshRepeater`及其`Text`屬性，以重新整理。

在建立之後`ListItem`s 和新增 [重新整理] 按鈕，DropDownList 和按鈕 s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

完成排序 DropDownList，與我們接下來必須更新 ObjectDataSource s`Selecting`事件處理常式，因此它會使用所選`SortBy``ListItem`s`Value`屬性而不是硬式編碼的排序運算式。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

這個時候先瀏覽頁面時產品會一開始會依照`ProductName`資料欄位，因為它 s `SortBy` `ListItem`預設選取 （請參閱圖 6）。 選取不同的排序選項，例如類別和按一下 重新整理將會導致回傳，並重新排序資料的類別名稱，如圖 7 所示。


[![產品為一開始會依其名稱](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**圖 6**: 產品會一開始會依其名稱 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![產品為現在會依類別排序](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**圖 7**: 產品會立即依類別排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> 按一下 [重新整理] 按鈕會導致資料自動重新排序中繼器的檢視狀態已停用，因而導致中繼器重新繫結至其在每次回傳的資料來源。 如果您已保留中繼器的檢視狀態啟用，變更排序下拉式清單贏了 t 對任何會影響排序次序。 若要補救這種情況，建立 [重新整理] 按鈕 s 的事件處理常式`Click`事件和重新繫結至其資料來源中繼器 (藉由呼叫中繼器的`DataBind()`方法)。


## <a name="remembering-the-sort-expression-and-direction"></a>記住的排序運算式和方向

建立非排序與可能會發生回傳，在頁面上的可排序的 DataList 或中繼器時它 s 命令式的排序運算式和方向會記住在回傳。 例如，假設我們已中繼器更新在本教學課程，包括與每個產品的 [刪除] 按鈕。 當使用者按一下 [刪除] 按鈕 d 我們會執行一些程式碼來刪除選取的產品，然後重新繫結至中繼器的資料。 如果排序詳細資料不會保存跨回傳，螢幕上顯示的資料將會還原為原始的排序次序。

本教學課程，DropDownList 以隱含方式將儲存排序運算式和方向其檢視狀態為我們。 如果我們使用不同的排序介面其中一個具有所提供的各種不同的排序選項的說，LinkButtons d 我們需要謹慎地在回傳記住的排序次序。 這可藉由來包含排序參數在查詢字串，或透過其他狀態持續性方法，將排序參數儲存在頁面的檢視狀態。

未來的範例，在本教學課程中，瀏覽如何保存網頁的檢視狀態中的排序詳細資料。

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>步驟 5： 加入要使用預設分頁 DataList 排序支援

在[前述教學課程](paging-report-data-in-a-datalist-or-repeater-control-vb.md)我們檢查如何實作與 DataList 預設分頁。 可讓 s 擴充此先前的範例包括能夠排序分頁的資料。 先開啟`SortingWithDefaultPaging.aspx`和`Paging.aspx`中的分頁`PagingSortingDataListRepeater`資料夾。 從`Paging.aspx`頁面上，按一下 [來源] 按鈕來檢視網頁 s 宣告式標記上。 複製選取的文字 （請參閱圖 8） 並將它貼到的宣告式標記`SortingWithDefaultPaging.aspx`之間`<asp:Content>`標記。


[![複寫中的宣告式標記&lt;asp: Content&gt; Paging.aspx SortingWithDefaultPaging.aspx 的標記](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**圖 8**： 複寫中的宣告式標記`<asp:Content>`標記從`Paging.aspx`至`SortingWithDefaultPaging.aspx`([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


複製的宣告式標記之後, 將複製的方法和屬性在`Paging.aspx`頁面 s 程式碼後置類別的程式碼後置類別`SortingWithDefaultPaging.aspx`。 接下來，請花一點時間檢視`SortingWithDefaultPaging.aspx`瀏覽器中的。 它應該在相同的功能和外觀與展示`Paging.aspx`。

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>增強 ProductsBLL 来包含預設分頁和排序方法

在上一個教學課程中我們建立`GetProductsAsPagedDataSource(pageIndex, pageSize)`方法中的`ProductsBLL`類別傳回`PagedDataSource`物件。 這`PagedDataSource`物件已填入*所有*的產品 (經由 BLL s`GetProducts()`方法)，當繫結至 DataList 對應至指定的記錄，但*pageIndex*和*pageSize*顯示輸入的參數。

稍早在本教學課程中加入排序支援藉由指定的排序運算式，從 ObjectDataSource 的`Selecting`事件處理常式。 這就非常適合 ObjectDataSource 傳回物件可以進行排序，例如`ProductsDataTable`傳回`GetProducts()`方法。 不過，`PagedDataSource`所傳回物件`GetProductsAsPagedDataSource`方法不支援其內部資料來源的排序。 相反地，我們需要排序傳回之結果`GetProducts()`方法*之前*我們將其放在`PagedDataSource`。

若要達成此目的，建立新的方法中`ProductsBLL`類別`GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`。 若要排序`ProductsDataTable`傳回`GetProducts()`方法，指定`Sort`屬性的預設值`DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource`方法只會稍微不同從`GetProductsAsPagedDataSource`在上一個教學課程中建立的方法。 特別是，`GetProductsSortedAsPagedDataSource`接受其他的輸入的參數`sortExpression`並指派此值設為`Sort`屬性`ProductDataTable`s `DefaultView`。 只要幾行程式碼之後，`PagedDataSource`物件 s 資料來源指派`ProductDataTable`s `DefaultView`。

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>呼叫 GetProductsSortedAsPagedDataSource 方法，並指定 SortExpression 輸入參數的值

與`GetProductsSortedAsPagedDataSource`方法完成下, 一個步驟是為此參數提供值。 在 ObjectDataSource`SortingWithDefaultPaging.aspx`目前設定為呼叫`GetProductsAsPagedDataSource`方法，並透過兩個的兩個輸入參數中傳遞`QueryStringParameters`，其指定在`SelectParameters`集合。 這兩個`QueryStringParameters`指示的來源`GetProductsAsPagedDataSource`方法 s *pageIndex*和*pageSize*參數來自 querystring 欄位`pageIndex`和`pageSize`。

更新 ObjectDataSource s`SelectMethod`屬性，它會叫用的新好`GetProductsSortedAsPagedDataSource`方法。 然後，將 新`QueryStringParameter`以便*sortExpression*輸入的參數從查詢字串欄位存取`sortExpression`。 設定`QueryStringParameter`s`DefaultValue`以產品名稱。

這些變更之後，請 ObjectDataSource s 宣告式標記看起來應該像：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

此時，`SortingWithDefaultPaging.aspx`頁面會依字母順序排序其結果，依產品名稱 （請參閱圖 9）。 這是因為根據預設，值的產品名稱傳遞為`GetProductsSortedAsPagedDataSource`方法 s *sortExpression*參數。


[![根據預設，結果會依照產品名稱](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**圖 9**： 根據預設，結果會依照`ProductName`([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


如果您手動新增`sortExpression`querystring 欄位，例如`SortingWithDefaultPaging.aspx?sortExpression=CategoryName`排序結果所指定`sortExpression`。 不過，這`sortExpression`移動至其他頁面的資料時，於 querystring 中不包含參數。 事實上，按一下下一步] 或 [最後一個頁面上的按鈕會讓我們回到`Paging.aspx`！ 此外，有 s 目前未排序的介面。 使用者可以變更分頁的資料的排序順序的唯一方法是藉由直接操作的 querystring。

## <a name="creating-the-sorting-interface"></a>建立排序的介面

我們必須先更新`RedirectUser`方法，以便傳送使用者`SortingWithDefaultPaging.aspx`(而不是`Paging.aspx`)，以及包含`sortExpression`於 querystring 中的值。 我們也應該新增唯讀的頁面層級名為`SortExpression`屬性。 此屬性，類似於`PageIndex`和`PageSize`先前的教學課程中建立屬性傳回的值`sortExpression`querystring 欄位，如果存在，而且預設值 (ProductName) 否則。

目前`RedirectUser`方法接受只有一個輸入的參數來顯示頁面的索引。 不過，可能是我們想要將使用者重新導向至特定的頁面上，使用查詢字串中指定哪些 s 以外的排序運算式的資料。 稍後我們將建立此頁面上，其中包含一系列的排序資料所指定之資料行的按鈕 Web 控制項的排序介面。 按一下其中一個這些按鈕時，我們想要傳入適當的排序運算式的值將使用者重新導向。 若要提供這項功能，建立兩個版本`RedirectUser`方法。 第一個應該接受頁面索引來顯示，而第二個可接受的頁面索引和排序運算式。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

在本教學課程中第一個範例中，我們會建立使用 DropDownList 排序介面。 此範例中，o d e s 使用上述其中一個在 DataList 置於所排序的三個按鈕 Web 控制項`ProductName`，下列其中一個針對`CategoryName`，，另一個用於`SupplierName`。 將三個按鈕 Web 控制項，設定其`ID`和`Text`屬性適當地：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

接下來，建立`Click`每個事件處理常式。 事件處理常式應該呼叫`RedirectUser`方法，傳回使用者第一頁使用適當的排序運算式。


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

當第一次造訪的頁面時，資料會依產品名稱依字母順序排序 （請參閱上一步圖 9）。 按一下 下一步 按鈕前進到第二個資料頁，然後按一下類別按鈕的 排序。 這會傳回我們第一頁的資料，依照類別目錄名稱 （請參閱圖 10）。 同樣地，由供應商 按鈕按一下排序排序從資料的第一頁開始的供應商資料。 透過資料形式，會記住排序選項。 圖 11 顯示依分類排序後再跳至資料的第十三個頁面的頁面。


[![會依類別排序產品](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**圖 10**: 產品會依類別排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![排序運算式會記住分頁透過資料時](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**圖 11**: 排序運算式會記住分頁透過資料時 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>透過記錄在中繼器中的步驟 6： 自訂分頁

DataList 範例會檢查在步驟 5 的頁面，透過使用沒有效率的預設分頁技術其資料。 若充分大量的資料進行分頁，務必使用自訂分頁。 回到[有效率地透過大型量的資料分頁](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)和[排序自訂分頁資料](../paging-and-sorting/sorting-custom-paged-data-vb.md)教學課程中，我們會檢查為 BLL 中預設和自訂分頁以及建立的方法之間的差異使用自訂分頁和排序自訂分頁的資料。 特別是，這些兩個先前的教學課程中我們加入下列三種方法`ProductsBLL`類別：

- `GetProductsPaged(startRowIndex, maximumRows)`傳回開始記錄的特定子集*startRowIndex*和不超過*maximumRows*。
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`傳回記錄依照指定的特定子集*sortExpression*輸入的參數。
- `TotalNumberOfProducts()`提供的記錄總數`Products`資料庫資料表。

這些方法可用來有效率地頁面上，並使用 DataList 或中繼器控制項中的資料進行排序。 為了說明這點，可讓啟動藉由使用自訂分頁支援; 建立中繼器控制項中的 s然後，我們要加入排序功能。

開啟`SortingWithCustomPaging.aspx`頁面`PagingSortingDataListRepeater`資料夾，並將重複項加入至頁面上，設定其`ID`屬性`Products`。 從中繼器 s 智慧標籤，建立名為新 ObjectDataSource `ProductsDataSource`。 將它設定為選取其資料從`ProductsBLL`類別的`GetProductsPaged`方法。


[![設定為使用 ProductsBLL 類別的 GetProductsPaged 方法 ObjectDataSource](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**圖 12**： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProductsPaged`方法 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


設定下拉式清單中更新、 插入並刪除索引標籤為 （無），然後按一下 下一步 按鈕。 設定資料來源精靈現在會提示輸入的來源`GetProductsPaged`方法 s *startRowIndex*和*maximumRows*輸入參數。 事實上，會忽略這些輸入的參數。 相反地， *startRowIndex*和*maximumRows*值將會在傳遞給`Arguments`ObjectDataSource s 中的屬性`Selecting`事件處理常式，如同我們的指定方式*sortExpression*在此教學課程 s 第一個示範。 因此，將保留參數來源 none 設定精靈中的下拉式清單。


[![保留參數來源設定為 None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**圖 13**： 保留參數來源設定為 None ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> 請勿*不*組 ObjectDataSource s`EnablePaging`屬性`true`。 這會自動包含它自己 ObjectDataSource *startRowIndex*和*maximumRows*參數`SelectMethod`s 現有的參數清單。 `EnablePaging`屬性時，繫結自訂分頁至 GridView、 DetailsView 或在 FormView 控制項的資料，因為這些控制項預期從 ObjectDataSource s 特定行為時才使用`EnablePaging`屬性是`true`。 由於我們有手動新增 DataList 和中繼器的分頁支援，讓這個屬性設定為`false`（預設值），因為我們將直接在我們的 ASP.NET 頁面內試吃中需要的功能。


最後，定義中繼器的`ItemTemplate`以顯示產品的名稱、 類別和供應商。 這些變更之後，請中繼器和 ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

請花一點時間瀏覽的頁面，透過瀏覽器，並記下會傳回任何記錄。 這是因為我們尚未以指定 ve *startRowIndex*和*maximumRows*參數的值; 因此，值 0 會傳遞兩個。 若要指定這些值，建立事件處理常式 ObjectDataSource s`Selecting`事件及設定這些參數值以程式設計方式硬式編碼值 0 到 5，分別：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

透過這項變更，頁面上，當透過瀏覽器中，檢視會顯示前五個產品。


[![會顯示第一個五筆記錄](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**圖 14**： 會顯示第一個五筆記錄 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> 若要依產品名稱排序，因為發生圖 14 中列出的產品`GetProductsPaged`執行有效率的自訂分頁查詢的預存程序排序的結果`ProductName`。


為了讓使用者逐步進行頁面中，我們需要追蹤的起始資料列索引和最大資料列，並在回傳記住這些值。 預設分頁範例中使用查詢字串欄位保存這些值。這個示範中，可讓 s 保存網頁的檢視狀態的資訊。 建立下列兩個屬性：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

接下來，更新選取的事件處理常式的程式碼，使其使用`StartRowIndex`和`MaximumRows`屬性而非 0 到 5 的硬式編碼值：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

此時我們的頁面仍然會顯示前五個的記錄。 不過，在使用這些屬性的位置，我們準備好建立我們分頁的介面。

## <a name="adding-the-paging-interface"></a>新增分頁介面

可讓的會使用相同的第一個、 上一步，接下來，最後分頁介面用於預設分頁範例中，包含標籤 Web 檢視會顯示哪些資料頁的控制項和存在多少的總頁數。 加入四個按鈕 Web 控制項和中繼器下方的標籤。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

接下來，建立`Click`四個按鈕的事件處理常式。 按一下其中一個按鈕時，我們需要更新`StartRowIndex`重新繫結至中繼器的資料。 名字、 上一步和 下一步 按鈕的程式碼相當簡單，但最後一個按鈕執行我們如何判斷資料的最後一頁的開始資料列索引嗎？ 若要計算此索引，以及判斷是否應該啟用下一步] 和 [最後一個按鈕我們需要知道總計中的多少筆記錄需透過分頁處理能力。 我們可以呼叫來判斷這`ProductsBLL`類別的`TotalNumberOfProducts()`方法。 可讓 s 建立名為的唯讀、 頁面層級屬性`TotalRowCount`所傳回的結果`TotalNumberOfProducts()`方法：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

這個屬性與我們現在可以判斷最後一頁開始資料列的索引。 具體來說，它 s 整數結果的`TotalRowCount`減 1 除以`MaximumRows`乘以`MaximumRows`。 現在我們可以撰寫`Click`四個分頁介面按鈕的事件處理常式：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

最後，我們需要檢視最後一頁時，檢視資料和下一步 和 最後一個按鈕的第一頁時，停用分頁介面中的第一個 和 上一頁 按鈕。 若要完成這項作業，加入下列程式碼 ObjectDataSource 的`Selecting`事件處理常式：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

新增這些後`Click`瀏覽器中測試網頁，事件處理常式和程式碼以啟用或停用根據目前的起始資料列索引的分頁介面項目。 圖 15 所示，當第一次瀏覽頁面第一個和上一個 按鈕將會停用。 按一下 [下一步] 顯示的資料，第二個頁面，按一下最後一個則會顯示最後一頁 （請參閱數據 16 及 17）。 檢視資料的最後一頁時，會停用 下一步 和 最後一個按鈕。


[![上一個和最後一個 按鈕就會停用檢視第一個頁面的產品](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**圖 15**: 上一個和最後一個按鈕就會停用檢視第一個頁面的產品 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![第二個頁面的產品為 Dispalyed](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**圖 16**: 第二個頁面的產品是 Dispalyed ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![按一下最後一個顯示資料的最後一頁](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**圖 17**： 按一下最後一個顯示的最後一個頁面的資料 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>步驟 7： 包括排序支援，其中包含自訂分頁中繼器

既然已實作自訂分頁，我們要包含排序準備好支援。 `ProductsBLL`類別 s`GetProductsPagedAndSorted`方法具有相同的*startRowIndex*和*maximumRows*輸入做為參數`GetProductsPaged`，但允許其他*sortExpression*輸入的參數。 若要使用`GetProductsPagedAndSorted`方法從`SortingWithCustomPaging.aspx`，我們需要執行下列步驟：

1. 變更 ObjectDataSource s`SelectMethod`屬性從`GetProductsPaged`至`GetProductsPagedAndSorted`。
2. 新增*sortExpression* `Parameter` ObjectDataSource s 物件`SelectParameters`集合。
3. 建立私用，頁面層級`SortExpression`在透過 [s] 頁面檢視狀態的回傳保存其值的屬性。
4. 更新 ObjectDataSource s`Selecting`事件處理常式來指派 ObjectDataSource s *sortExpression*參數的頁面層級值`SortExpression`屬性。
5. 建立排序的介面。

藉由更新 ObjectDataSource s 啟動`SelectMethod`屬性和加入*sortExpression* `Parameter`。 請確定*sortExpression* `Parameter` s`Type`屬性設定為`String`。 完成這些前兩個工作之後，ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

接下來，我們需要頁面層級`SortExpression`屬性其值會序列化至檢視狀態。 如果沒有排序運算式的值已設定，使用 ProductName 做為預設值：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

ObjectDataSource 叫用之前`GetProductsPagedAndSorted`方法，所以我們需要將*sortExpression* `Parameter`值`SortExpression`屬性。 在`Selecting`事件處理常式，加入下列程式碼行：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

剩下的就是實作排序的介面。 如我們所做的最後一個範例中，可讓 s 已排序的介面實作使用三個按鈕 Web 控制項，可讓使用者排序的結果依產品名稱、 類別或供應商。


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

建立`Click`這三個按鈕控制項的事件處理常式。 在事件處理常式中，重設`StartRowIndex`設成 0，設定`SortExpression`適當的值，並重新繫結至中繼器資料：


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

S 都是這麼簡單 ！ 儘管有幾個步驟來取得自訂分頁和排序實作的步驟所需要的預設分頁非常類似。 檢視資料時依分類排序的最後一頁時，圖 18 顯示的產品。


[![接著會顯示最後一頁的資料、 排序依據類別、](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**圖 18**： 會顯示最後一頁的資料、 依類別、 排序 ([按一下以檢視完整大小的影像](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> 在上一個範例中，請依供應商供應商名稱當做排序運算式排序時。 不過，對於自訂分頁實作中，我們需要使用供應商。 這是因為預存程序負責以實作自訂分頁`GetProductsPagedAndSorted`會傳遞至排序運算式`ROW_NUMBER()`關鍵字，`ROW_NUMBER()`關鍵字需要實際的資料行名稱，而不是別名。 因此，我們必須使用`CompanyName`(中的資料行名稱`Suppliers`資料表) 而不是使用中的別名`SELECT`查詢 (`SupplierName`) 的排序運算式。


## <a name="summary"></a>總結

既不 DataList 也中繼器提供內建排序支援，但可以新增程式碼和自訂排序介面的位元，這類功能。 當實作排序，但未分頁時，可以透過指定的排序運算式`DataSourceSelectArguments`物件傳遞到 ObjectDataSource 的`Select`方法。 這`DataSourceSelectArguments`物件 s`SortExpression`屬性可以指派在 ObjectDataSource 的`Selecting`事件處理常式。

若要加入排序功能 DataList 或已經提供分頁支援的中繼器，最簡單的方法是自訂商務邏輯層，以納入方法可接受排序運算式。 這項資訊可以接著傳遞中透過 ObjectDataSource s 中的參數`SelectParameters`。

本教學課程完成我們檢驗分頁和排序與 DataList 和中繼器控制項。 我們的下一個和最後一個教學課程將會檢驗如何 DataList 和中繼器範本，s 新增按鈕 Web 控制項，以提供每個項目為基礎的一些自訂的使用者啟動的功能。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 David Suru。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一步](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
