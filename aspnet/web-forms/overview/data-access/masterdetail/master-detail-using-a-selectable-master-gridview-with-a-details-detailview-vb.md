---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: "詳細資料 DetailView (VB) 搭配使用可選取的主要 GridView 的主要/詳細資料 |Microsoft 文件"
author: rick-anderson
description: "本教學課程中會有的 GridView 的資料列會包含名稱與選取的按鈕以及每個產品的價格。 按一下 選取的按鈕，如 particu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: eae9c07eff7780aab18346815ca410d687789d17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>詳細資料 DetailView (VB) 搭配使用可選取的主要 GridView 的主要/詳細資料
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe)或[下載 PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> 本教學課程中會有的 GridView 的資料列會包含名稱與選取的按鈕以及每個產品的價格。 按一下特定產品的 [選取] 按鈕將會導致在相同頁面上的 DetailsView 控制項中顯示其完整詳細資料。


## <a name="introduction"></a>簡介

在[上一個教學課程](master-detail-filtering-across-two-pages-vb.md)我們可了解如何建立主要/詳細資料報表，使用兩個網頁: 「 主要 」 顯示的網頁，從中我們的供應商; 清單，[詳細資料] 網頁，列出所選所提供的產品供應商。 這兩個頁面的報表格式可以壓縮成一頁中。 本教學課程中會有的 GridView 的資料列會包含名稱與選取的按鈕以及每個產品的價格。 按一下特定產品的 [選取] 按鈕將會導致在相同頁面上的 DetailsView 控制項中顯示其完整詳細資料。


[![按一下 [選取] 按鈕會顯示產品的詳細資料](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**圖 1**： 按一下 [選取] 按鈕會顯示產品的詳細資料 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>步驟 1： 建立可選取的 GridView

提醒您，在兩個頁面主要/詳細資料中每個主要的記錄包含超連結的報表，按一下時，會傳送使用者傳遞的按下資料列的詳細資料頁面`SupplierID`於 querystring 中的值。 每個使用 HyperLinkField 的 GridView 資料列加入超連結。 單一頁面的主要/詳細資料報表，我們需要按鈕的每個 GridView 資料列，按一下時，會顯示詳細資料。 GridView 控制項可以設定為包含每個資料列，導致回傳，並將該資料列標示為 GridView 選取按鈕[SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)。

開始將 GridView 控制項加入`DetailsBySelecting.aspx`頁面`Filtering`資料夾中，設定其`ID`屬性`ProductsGrid`。 接下來，加入名為新 ObjectDataSource`AllProductsDataSource`會叫用`ProductsBLL`類別的`GetProducts()`方法。


[![建立名為 AllProductsDataSource ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**圖 2**： 建立 ObjectDataSource 名為`AllProductsDataSource`([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![使用 ProductsBLL 類別](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**圖 3**： 使用`ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![設定要叫用 GetProducts() 方法 ObjectDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**圖 4**： 設定要叫用 ObjectDataSource`GetProducts()`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


編輯 GridView 的欄位移除以外的所有`ProductName`和`UnitPrice`BoundFields。 此外，請隨意自訂這些 BoundFields，如有需要例如格式化`UnitPrice`BoundField 為貨幣，並將變更`HeaderText`BoundFields 的屬性。 可以以圖形方式，完成這些步驟，即可編輯資料行中的連結 GridView 的智慧標籤，或手動設定的宣告式語法。


[![全部移除的產品名稱和單價 BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**圖 5**： 移除以外的所有`ProductName`和`UnitPrice`BoundFields ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


在 GridView 的最後一個標記是：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

接下來，我們需要將標示為可選取、 GridView 這會將選取的按鈕加入至每個資料列。 若要達成此目的，只會檢查 GridView 的智慧標籤中啟用選取核取方塊。


[![讓 GridView 的資料列選取](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**圖 6**： 請 GridView 的資料列可選取 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


檢查 [啟用選取範圍] 選項加入至 CommandField `ProductsGrid` GridView 其`ShowSelectButton`屬性設定為 True。 這會導致選取的按鈕每個資料列的 GridView，如圖 6 所示。 根據預設，[選取] 按鈕會呈現為 LinkButtons，但是您可以使用按鈕或 ImageButtons 改為透過 CommandField`ButtonType`屬性。


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

在按下 GridView 資料列選取按鈕時回傳展示和 GridView`SelectedRow`更新屬性。 除了`SelectedRow`屬性 GridView 提供[SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx)， [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)，和[SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx)屬性。 `SelectedIndex`屬性會傳回選取的資料列的索引，而`SelectedValue`和`SelectedDataKey`屬性會傳回值為基礎的 GridView [DataKeyNames 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)。

`DataKeyNames`屬性用來將一個或多個資料欄位值與每個資料列，並且通常用來從基礎資料的每個 GridView 資料列的唯一識別資訊的屬性。 `SelectedValue`屬性會傳回第一個值`DataKeyNames`選取的資料列的資料欄位做為 where`SelectedDataKey`屬性會傳回選取的資料列`DataKey`物件，其中包含所有的指定的資料索引鍵欄位的值該資料列。

`DataKeyNames`時資料來源繫結至 GridView、 DetailsView 或透過設計工具在 FormView 自動屬性設定為用來唯一識別資料欄。 當這個屬性已設定為我們自動前述教學課程中時，範例會曾不含`DataKeyNames`指定屬性。 不過，在本教學課程，可選取的 GridView 以及未來教學課程中我們將會檢查插入、 更新和刪除`DataKeyNames`必須正確設定屬性。 請花一點時間，確定您的 GridView`DataKeyNames`屬性設定為`ProductID`。

讓我們來檢視進度透過瀏覽器為止。 請注意 GridView 會列出名稱和所有選取的 LinkButton 以及產品的價格。 按一下 [選取] 按鈕會導致回傳。 在步驟 2 中，我們會看到如何讓此回傳 DetailsView 回應，藉由顯示所選產品的詳細資料。


[![每個產品的資料列包含選取的 LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**圖 7**： 每個產品的資料列包含選取的 LinkButton ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>反白顯示選取的資料列

`ProductsGrid` GridView 具有`SelectedRowStyle`可用來指示選取的資料列的視覺樣式的屬性。 不當使用，這可以改善使用者經驗所需清楚顯示目前選取的 GridView 的資料列。 本教學課程，讓我們有會以黃色背景反白顯示選取的資料列。

如同我們先前的教學課程，讓我們盡量保留定義 CSS 類別為美觀相關的設定。 因此，建立新的 CSS 類別中`Styles.css`名為`SelectedRowStyle`。


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

若要套用至這個 CSS 類別`SelectedRowStyle`屬性*所有*GridViews 在教學課程系列中，編輯`GridView.skin`面板中`DataWebControls`佈景主題以包含`SelectedRowStyle`設定如下所示：


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

此新增功能，以選取的 GridView 資料列現在會以黃色背景的色彩反白。


[![自訂使用 GridView SelectedRowStyle 屬性選取的資料列的外觀](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**圖 8**： 選取資料列的外觀使用自訂的 GridView`SelectedRowStyle`屬性 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>步驟 2： 在 DetailsView 中顯示所選的產品的詳細資料

與`ProductsGrid`GridView 完成時，會維持為只顯示所選擇的特定產品的相關資訊的 DetailsView 加入。 將上述 GridView DetailsView 控制項，並建立名為新 ObjectDataSource `ProductDetailsDataSource`。 因為我們想要這個 DetailsView 以顯示所選產品的特定資訊，請設定`ProductDetailsDataSource`使用`ProductsBLL`類別的`GetProductByProductID(productID)`方法。


[![叫用 ProductsBLL 類別 GetProductByProductID(productID) 方法](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**圖 9**： 叫用`ProductsBLL`類別的`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


具有 *`productID`* 參數的值取自 GridView 控制項`SelectedValue`屬性。 如前所述，GridView`SelectedValue`屬性會傳回第一個資料索引鍵所選取的資料列的值。 因此，務必的 GridView 的`DataKeyNames`屬性設定為`ProductID`，好讓選取的資料列`ProductID`值由`SelectedValue`。


[![ProductID 參數設 GridView 的 SelectedValue 屬性](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**圖 10**： 設定 *`productID`* 參數 GridView`SelectedValue`屬性 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


一次`productDetailsDataSource`已經正確地設定和繫結至 DetailsView ObjectDataSource，本教學課程已完成 ！ 當第一次瀏覽網頁時選取任何資料列，所以 GridView 的`SelectedValue`屬性會傳回`Nothing`。 因為沒有與產品`NULL``ProductID`沒有記錄所傳回的值，`GetProductByProductID(productID)`方法，這表示未顯示在 DetailsView （請參閱圖 11）。 在 GridView 資料列選取按鈕時回傳展示並 DetailsView 重新整理為止。 這次 GridView`SelectedValue`屬性會傳回`ProductID`所選取的資料列，`GetProductByProductID(productID)`方法會傳回`ProductsDataTable`該特定產品，並在 DetailsView 的相關資訊會顯示這些詳細資料 （請參閱圖 12）。


[![當第一次瀏覽的 GridView 只會顯示](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**圖 11**： 當第一次瀏覽，只在 GridView 會顯示 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![在選取的資料列，會顯示產品的詳細資料](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**圖 12**： 時選取一個資料列，會顯示產品的詳細資料 ([按一下以檢視完整大小的影像](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>總結

在此範例與上述三個教學課程中，我們已看到一些技術來顯示主要/詳細資料報表。 在本教學課程中我們檢查使用的可選取的 GridView 來存放主要記錄和顯示相同頁面上選取的主要記錄詳細的 DetailsView。 在先前的教學課程中我們討論了如何顯示主要/詳細資料報表使用 DropDownLists 並顯示上一個網頁，並在另一台的詳細記錄的主要記錄。

本教學課程結束時，我們的主要/詳細資料報表的檢查。 從下一個教學課程中我們一開始我們瀏覽具有 GridView、 DetailsView 和在 FormView 的自訂格式。 我們會看到如何自訂這些繫結到這些資料為基礎的控制項的外觀、 如何彙總 GridView 的頁尾中的資料以及如何使用範本來取得更大的版面配置上的控制項。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一步](master-detail-filtering-across-two-pages-vb.md)
