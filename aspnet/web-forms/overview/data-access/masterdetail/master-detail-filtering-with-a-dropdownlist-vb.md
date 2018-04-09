---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: 使用 DropDownList (VB) 進行篩選的主要/詳細資料 |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們會看到如何 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料中顯示的主要記錄。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1ae660ddbc6c8e2874190ade6f3deddeebe820
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>使用 DropDownList (VB) 進行篩選的主要/詳細資料
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> 在本教學課程中，我們會看到如何 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料中顯示的主要記錄。


## <a name="introduction"></a>簡介

一種常見的報表是*主要/詳細資料報表*中的報表一開始會顯示某些的 「 主要 」 的記錄集。 使用者可以再向下鑽研到其中一個主要記錄中，藉此檢視主要記錄的 「 詳細資料。 」 主要/詳細資料報告會視覺化為一對多關聯性，例如報告的理想選擇顯示所有類別，然後由使用者選取特定的類別，並顯示其相關聯的產品。 此外，主要/詳細資料報表是適用於顯示詳細的資訊，特別是 「 寬的 」 的資料表 （是有很多資料行）。 例如，「 主要 」 的層級的主要/詳細資料報表可能會在資料庫中，顯示只的產品名稱和單位價格的產品和向下切入特定產品，則會顯示其他產品欄位 (類別、 供應商、 每個單位的數量和等等）。

有許多方法可以用實作主要/詳細資料報表。 透過這和接下來三個教學課程介紹各種不同的主要/詳細資料報表。 在本教學課程中，我們會看到如何顯示中的主要記錄[DropDownList 控制項](https://msdn.microsoft.com/library/dtx91y0z.aspx)和 GridView 中選取的清單項目的詳細資料。 尤其，本教學課程的主要/詳細資料報表會列出分類和產品資訊。

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>步驟 1: DropDownList 以顯示類別目錄

我們的主要/詳細資料報表會列出與選取的清單項目的產品顯示的 DropDownList 中的類別，進一步向下 GridView 中的頁面。 晚於我們在第一個工作，則具有 dropdownlist 顯示類別目錄。 開啟`FilterByDropDownList.aspx`頁面`Filtering`資料夾中，拖曳 DropDownList 上從 [工具箱] 拖曳至頁面的設計工具，並設定其`ID`屬性`Categories`。 接下來，按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結。 這會顯示資料來源組態精靈。


[![指定的資料來源的 DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**圖 1**： 指定 DropDownList 的資料來源 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


選擇加入名為新 ObjectDataSource`CategoriesDataSource`會叫用`CategoriesBLL`類別的`GetCategories()`方法。


[![加入名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**圖 2**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![選擇使用 CategoriesBLL 類別](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**圖 3**： 選擇使用`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![設定為使用 GetCategories() 方法 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


設定我們仍需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 之後其中一個應該做為清單項目值相關聯。 具有`CategoryName`欄位顯示器和`CategoryID`做為每個清單項目值。


[![此類別名稱欄位，然後使用 CategoryID 有 DropDownList 顯示做為值](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


現在我們有從記錄中會填入的 DropDownList 控制項`Categories`資料表 （全部在六秒內完成）。 圖 6 為止顯示進度，透過瀏覽器檢視時。


[![下拉式清單會列出目前的分類](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**圖 6**: 下拉式清單會列出目前的分類 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>步驟 2： 加入產品 GridView

在我們的主要/詳細資料報表中的最後一個步驟是列出與選取的類別相關聯的產品。 若要達成此目的，GridView 加入頁面並建立名為新 ObjectDataSource `productsDataSource`。 具有`productsDataSource`控制項選出從其資料`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。


[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**圖 7**： 選取`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


選擇這個方法之後，ObjectDataSource 精靈會提示輸入我們方法的值*`categoryID`*參數。 若要使用的所選值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。


[![CategoryID 參數值設定為類別 DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**圖 8**： 設定*`categoryID`*參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


請花一點時間簽出的瀏覽器中的進度。 當第一次造訪的頁面時，這些產品屬於所選類別目錄 （如圖 9 所示），會顯示 （飲料），但變更 DropDownList 不會更新資料。 這是因為更新 GridView 會因發生回傳。 若要完成這項作業中，我們有兩個選項 （其中都不需要撰寫任何程式碼）：

- **設定類別 DropDownList**[AutoPostBack 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**為 True。** （您可以完成此作業藉由檢查 DropDownList 的智慧標籤的 [啟用 AutoPostBack] 選項。）如此將會觸發回傳，只要 DropDownList 的選取項目由使用者變更。 因此，當使用者選取新的類別從 DropDownList 將發生回傳，而且會以新選取的分類的產品更新 GridView。 （這是已在本教學課程使用的方法）。
- **加入按鈕 Web 控制項旁 DropDownList。** 設定其`Text`屬性來重新整理或類似的項目。 使用此方法時，使用者必須選取新的類別，然後按一下 [] 按鈕。 按一下按鈕，將會導致回傳，並更新以列出所選類別的那些產品 GridView。

數字 9 和 10 說明主要/詳細資料中的報表動作。


[![當第一次瀏覽頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**圖 9**： 當第一次瀏覽頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![選取新的產品 （產生） 會自動導致回傳，更新 GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**圖 10**： 選取新的產品 （產生） 會自動導致回傳，更新 GridView ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>可以加入 「-選擇分類-」 清單項目

當第一次造訪`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品在 GridView 中選取的類別。 而非顯示第一個類別目錄的產品，我們可能會想要改為在 DropDownList 項目選取，顯示類似，「-選擇分類-」。

DropDownList 中加入新的清單項目，請前往 [屬性] 視窗，按一下省略符號在`Items`屬性。 加入新的清單項目與`Text`"-選擇一個類別目錄-"和`Value` `-1`。


[![新增-選擇類別目錄--清單項目](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**圖 11**： 新增為--選擇類別目錄--清單項目 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


或者，您可以將下列標記加入至 DropDownList 新增清單項目：


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

此外，我們需要將 DropDownList 控制項的`AppendDataBoundItems`設為 True，因為類別繫結至 DropDownList 從 ObjectDataSource 時將會被覆寫的任何手動新增清單項目如果`AppendDataBoundItems`不是 True。


![AppendDataBoundItems 屬性設為 True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**圖 12**： 設定`AppendDataBoundItems`屬性設定為 True


這些變更之後，請先瀏覽頁面的 「-選擇分類-」 選項已選取，並不顯示任何產品時。


[![初始載入的頁面會不顯示任何產品](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**圖 13**： 上初始頁面載入不會顯示產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


因為 「-選擇分類-」 清單項目被選取時顯示任何產品的原因是因為它的值是`-1`和含有資料庫中沒有產品`CategoryID`的`-1`。 如果這是您想在此時完成時的行為 ！ 如果您想要顯示的但是*所有*一個類別目錄選取 「-選擇分類-」 清單項目時，返回`ProductsBLL`類別和自訂`GetProductsByCategoryID(categoryID)`方法，使它會叫用`GetProducts()`方法如果傳入中*`categoryID`*參數小於零：


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

這裡使用的技巧是類似的方法，我們用來顯示所有供應商回[宣告式參數](../basic-reporting/declarative-parameters-cs.md)教學課程，雖然此範例中，我們會使用值為`-1`表示應該所有記錄擷取與`Nothing`。 這是因為*`categoryID`*參數`GetProductsByCategoryID(categoryID)`方法應為整數值傳入，而宣告式參數教學課程中我們所傳入的字串輸入參數。

圖 14 顯示的螢幕擷取畫面`FilterByDropDownList.aspx`如果選取 「-選擇分類-」 的選項。 在這裡，根據預設，會顯示所有產品，使用者可以選擇特定的類別目錄來縮小顯示。


[![所有的產品是現在所列的預設值](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**圖 14**： 所有產品會立即列出預設 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>總結

顯示階層關聯的資料時，這通常有助於呈現的資料，使用主要/詳細資料報表，使用者可以從中啟動研究的資料階層的頂端，並向下鑽研到詳細資料。 在此教學課程中我們會檢查建置簡單的主要/詳細資料報表，顯示所選類別目錄的產品。 這是透過 DropDownList 的分類和產品屬於所選類別目錄的 GridView 的清單以完成。

在[下一個教學課程](master-detail-filtering-with-two-dropdownlists-vb.md)我們 DropDownList 介面的一個步驟，使用兩個 DropDownLists。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [下一頁](master-detail-filtering-with-two-dropdownlists-vb.md)
