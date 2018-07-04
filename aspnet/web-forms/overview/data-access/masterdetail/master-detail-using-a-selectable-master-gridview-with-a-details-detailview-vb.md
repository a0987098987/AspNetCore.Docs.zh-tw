---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: 使用具有 (VB) 的詳細資料 detailview 之可選取主要 GridView 的主要/詳細資料 |Microsoft Docs
author: rick-anderson
description: 本教學課程中會有的 GridView，其資料列包含名稱和每個產品價格以及 [選取] 按鈕。 按一下 [選取] 按鈕的 particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: a97323700e20aed12ee29674952f1ffe9144133c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366037"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>使用具有 (VB) 的詳細資料 detailview 之可選取主要 GridView 的主要/詳細資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe)或[下載 PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> 本教學課程中會有的 GridView，其資料列包含名稱和每個產品價格以及 [選取] 按鈕。 按一下特定產品的 [選取] 按鈕將會導致在相同網頁的 DetailsView 控制項中顯示其完整詳細資料。


## <a name="introduction"></a>簡介

在 [先前的教學課程](master-detail-filtering-across-two-pages-vb.md)我們了解如何建立使用兩個網頁的主版/詳細報告: 「 主要 」 的網頁上，我們用來顯示供應商; 清單，列出所選提供這些產品的 [詳細資料] 網頁供應商。 這兩個頁面的報表格式可以壓縮成一頁中。 本教學課程中會有的 GridView，其資料列包含名稱和每個產品價格以及 [選取] 按鈕。 按一下特定產品的 [選取] 按鈕將會導致在相同網頁的 DetailsView 控制項中顯示其完整詳細資料。


[![按一下 [選取] 按鈕會顯示產品的詳細資料](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**圖 1**： 按一下 [選取] 按鈕會顯示產品的詳細資料 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>步驟 1： 建立可選取的 GridView

回想一下，在兩個頁面主版/詳細報告每個主要的記錄包含超連結，按一下時，傳送給使用者傳遞的已按下 資料列的詳細資料頁面`SupplierID`於 querystring 中的值。 這類的超連結加入至每個使用 HyperLinkField 的 GridView 資料列。 單一頁面主版/詳細資料報表中，我們需要一個按鈕的每個 GridView 資料列，按一下時，會顯示詳細資料。 GridView 控制項可以設定為包含每個資料列，造成回傳，並將該資料列標示為 GridView 的 [選取] 按鈕[SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)。

著手將 GridView 控制項加入`DetailsBySelecting.aspx`頁面中`Filtering`資料夾中，設定其`ID`屬性設`ProductsGrid`。 接下來，新增名為新 ObjectDataSource`AllProductsDataSource`叫用`ProductsBLL`類別的`GetProducts()`方法。


[![建立名為 AllProductsDataSource ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**圖 2**： 建立名為 ObjectDataSource `AllProductsDataSource` ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![使用 ProductsBLL 類別](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**圖 3**： 使用`ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![設定要叫用 GetProducts() 方法的 ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**圖 4**： 設定要叫用 ObjectDataSource`GetProducts()`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


編輯移除的 GridView 的欄位以外的所有`ProductName`和`UnitPrice`BoundFields。 此外，您可以自訂這些 BoundFields 如有需要格式化等`UnitPrice`成貨幣 BoundField 和變更`HeaderText`BoundFields 的屬性。 按一下 [編輯資料行] 連結，從 GridView 的智慧標籤，或透過手動設定的宣告式語法，可以以圖形方式，完成這些步驟。


[![移除的產品名稱和單價 BoundFields 以外的所有](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**圖 5**： 移除以外的所有`ProductName`並`UnitPrice`BoundFields ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


GridView 的最後一個標記是：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

接下來，我們需要將標示為選取，將會新增至每個資料列的 [選取] 按鈕的 GridView。 若要達成此目的，只是檢查 GridView 的智慧標籤中啟用選取核取方塊。


[![使 GridView 的資料列選取](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**圖 6**： 讓選取的 GridView 資料列 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


檢查 [啟用選取範圍] 選項新增至 CommandField `ProductsGrid` GridView 與其`ShowSelectButton`屬性設為 True。 這會導致 選取 按鈕的每個資料列的 GridView，如 圖 6 所示。 根據預設，[選取] 按鈕會呈現為 Linkbutton，但是您可以使用按鈕或 ImageButtons 改為透過 CommandField`ButtonType`屬性。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

按下 GridView 資料列的 [選取] 按鈕時回傳接踵而來和 GridView`SelectedRow`屬性更新。 除了`SelectedRow`提供的屬性，GridView [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx)， [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)，和[SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx)屬性。 `SelectedIndex`屬性會傳回選取的資料列的索引，而`SelectedValue`並`SelectedDataKey`屬性會傳回值根據 GridView [DataKeyNames 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)。

`DataKeyNames`屬性用來建立關聯的其中一個或多個資料欄位值的每一列，並且通常用來從基礎資料與每個 GridView 資料列的唯一識別資訊的屬性。 `SelectedValue`屬性會傳回值的第一個`DataKeyNames`選取的資料列的資料欄位做為 where`SelectedDataKey`屬性會傳回選取的資料列`DataKey`物件，其中包含所有指定的資料索引鍵欄位的值該資料列。

`DataKeyNames`資料來源繫結至 GridView、 DetailsView 或 FormView 透過設計工具時自動屬性設定為用來唯一識別的資料欄位。 雖然設定此屬性已為我們自動在先前的教學課程中，範例會曾不`DataKeyNames`指定的屬性。 不過，在本教學課程中，可選取的 GridView 以及未來的教學課程中我們將會檢查插入、 更新和刪除`DataKeyNames`屬性必須正確設定。 請花一點時間確認您的 GridView`DataKeyNames`屬性設定為`ProductID`。

讓我們來檢視透過瀏覽器到目前為止我們進度。 請注意，GridView 會列出所有選取的 LinkButton 以及產品的價格與名稱。 按一下 [選取] 按鈕時，會導致回傳。 在步驟 2 中，我們會看到如何讓此回傳 DetailsView 回應會顯示所選產品的詳細資料。


[![每個產品的資料列包含選取的 LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**圖 7**： 每個產品的資料列包含選取的 LinkButton ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>反白顯示選取的資料列

`ProductsGrid` GridView 有`SelectedRowStyle`可以用來指定選取的資料列的視覺化樣式的屬性。 使用方式正確，這可以改善使用者經驗的詳細清楚地顯示目前選取的 GridView 資料列。 本教學課程中，讓我們以黃色背景反白顯示選取的資料列。

如同我們先前的教學課程，讓我們致力於將定義為 CSS 類別的美觀相關設定。 因此，建立新的 CSS 類別中`Styles.css`名為`SelectedRowStyle`。


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

要套用這個 CSS 類別，以便`SelectedRowStyle`屬性*所有*Gridview，在我們的教學課程系列中，編輯`GridView.skin`面板中`DataWebControls`佈景主題，以包含`SelectedRowStyle`設定，如下所示：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

此步驟中，選取的 GridView 資料列現在反白黃色背景色彩。


[![自訂使用 GridView 的 SelectedRowStyle 屬性選取的資料列的外觀](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**圖 8**： 選取資料列的外觀使用自訂的 GridView`SelectedRowStyle`屬性 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>步驟 2： 在 DetailsView 中顯示選取之的產品的詳細資料

使用`ProductsGrid`完成 GridView，所有剩下就是新增顯示所選擇的特定產品的相關資訊的 DetailsView。 將上述 GridView DetailsView 控制項，並建立名為新 ObjectDataSource `ProductDetailsDataSource`。 因為我們希望此 DetailsView 來顯示有關所選產品的特定資訊，請設定`ProductDetailsDataSource`若要使用`ProductsBLL`類別的`GetProductByProductID(productID)`方法。


[![叫用 ProductsBLL 類別的 GetProductByProductID(productID) 方法](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**圖 9**： 叫用`ProductsBLL`類別的`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


已*`productID`* 參數的值取自 GridView 控制項`SelectedValue`屬性。 我們稍早所述，GridView 的`SelectedValue`屬性會傳回第一個資料機碼的所選資料列的值。 因此，務必，GridView`DataKeyNames`屬性設定為`ProductID`，以便選取的資料列`ProductID`所傳回值`SelectedValue`。


[![將 [productID 參數] 設定 GridView 的 SelectedValue 屬性](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**圖 10**： 設定*`productID`* GridView 的參數`SelectedValue`屬性 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


一次`productDetailsDataSource`ObjectDataSource 已正確設定和繫結至 DetailsView，本教學課程已完成 ！ 當第一次瀏覽的頁面已選取任何資料列，因此 GridView 的`SelectedValue`屬性會傳回`Nothing`。 因為沒有與產品`NULL``ProductID`的值，由，會傳回任何記錄`GetProductByProductID(productID)`方法，這表示未顯示 DetailsView （請參閱 圖 11）。 按下 GridView 資料列的 [選取] 按鈕時回傳是兩邊彼此乾瞪眼並 DetailsView 重新整理為止。 這次的 GridView`SelectedValue`屬性會傳回`ProductID`所選取的資料列，`GetProductByProductID(productID)`方法會傳回`ProductsDataTable`該特定的產品和 DetailsView 的相關資訊會顯示這些詳細資料 （請參閱 圖 12）。


[![當第一次瀏覽的只有 GridView 顯示](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**圖 11**： 當第一次瀏覽，只 GridView 會顯示 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![在選取的資料列，會顯示產品的詳細資料](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**圖 12**： 在選取的資料列，會顯示產品的詳細資料 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>總結

這和上述的三個教學課程中，我們已了解一些技巧可以顯示主版/詳細報告。 在本教學課程中，我們檢查使用可選取的 GridView 存放主要記錄和 DetailsView 相同的頁面上顯示所選的主要記錄的相關詳細資料。 在先前的教學課程中，我們討論過如何顯示主版/詳細資料報表使用 dropdownlist 進行與上一個網頁及詳細的記錄，另一個顯示主要的記錄。

本教學課程結束時，我們檢查主版/詳細報告。 我們將開始下一個教學課程來開始探索的自訂格式設定與 GridView、 DetailsView 和 FormView。 我們會看到如何自訂這些繫結到這些資料為基礎的控制項的外觀、 如何在 GridView 的頁尾中的資料摘述和如何使用範本，以取得更高的版面配置上的控制項。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](master-detail-filtering-across-two-pages-vb.md)
