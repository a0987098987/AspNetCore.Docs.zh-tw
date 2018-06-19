---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: 使用 DropDownList (C#) 進行篩選的主要/詳細資料 |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們看到如何使用 DropDownLists 顯示 'master' 的記錄和資料清單來顯示單一網頁中顯示主要/詳細資料報表...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880670"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>主從式篩選搭配 DropDownList (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> 本教學課程中我們會了解如何在單一網頁使用 DropDownLists 顯示 「 主要 」 的記錄和資料以顯示 [詳細資料] 清單中顯示主要/詳細資料報表。


## <a name="introduction"></a>簡介

主要/詳細資料報表，我們先建立在先前使用 GridView[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程中，一開始會顯示某些的 「 主要 」 的記錄集。 使用者可以再向下鑽研到其中一個主要記錄中，藉此檢視主要記錄的 「 詳細資料。 」 主要/詳細資料報表是視覺化一對多關聯性並顯示 （是有很多資料行） 特別 「 寬 」 的資料表的詳細的資訊的理想選擇。 我們已探索如何實作在上一個教學課程中使用的 GridView 和 DetailsView 控制項的主要/詳細資料報表。 在本教學課程和兩個 下一步，我們會重新檢查這些概念，但是著重於使用 DataList 和中繼器控制項改為。

在本教學課程中，我們將探討使用 DropDownList 包含的 「 主要 」 記錄，[詳細資料] 中顯示的記錄資料清單。

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>步驟 1： 加入主從式教學課程 Web 網頁

開始本教學課程之前，讓我們先花一點時間，將資料夾加入我們需要本教學課程和下一步使用 DataList 和中繼器控制項的主要/詳細資料報表處理的兩個 ASP.NET 網頁。 藉由建立新的資料夾中名為專案啟動`DataListRepeaterFiltering`。 接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![建立 DataListRepeaterFiltering 資料夾，以及新增的教學課程的 ASP.NET 頁面](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**圖 1**： 建立`DataListRepeaterFiltering`資料夾並加入的教學課程的 ASP.NET 網頁


接下來，開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。 此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


若要有項目符號清單顯示主從式教學課程，我們將建立，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，然後加入下列標記 」 顯示的資料與 DataList 和中繼器 「 站台對應節點標記之後：

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**圖 3**： 更新包含新的 ASP.NET 網頁站台對應


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>步驟 2: DropDownList 以顯示類別目錄

我們的主要/詳細資料報表會列出 DropDownList 中的類別，與選取的清單項目的產品顯示進一步的頁面中的資料清單。 晚於我們在第一個工作，則具有 dropdownlist 顯示類別目錄。 先開啟`FilterByDropDownList.aspx`頁面`DataListRepeaterFiltering`資料夾，然後從 [工具箱] 拖曳至頁面的設計工具拖曳 DropDownList。 接下來，設定 DropDownList`ID`屬性`Categories`。 按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結，建立名為新 ObjectDataSource `CategoriesDataSource`。


[![加入名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**圖 4**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


設定新 ObjectDataSource，它會叫用`CategoriesBLL`類別的`GetCategories()`方法。 設定我們仍需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 之後其中一個應該為每個清單項目的值相關聯。 具有`CategoryName`欄位顯示器和`CategoryID`做為每個清單項目值。


[![此類別名稱欄位，然後使用 CategoryID 有 DropDownList 顯示做為值](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


現在我們有從記錄中會填入的 DropDownList 控制項`Categories`資料表 （全部在六秒內完成）。 圖 6 為止顯示進度，透過瀏覽器檢視時。


[![下拉式清單會列出目前的分類](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**圖 6**: 下拉式清單會列出目前的分類 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>步驟 2： 加入產品 DataList

我們的主要/詳細資料報表的最後一個步驟是列出與選取的類別相關聯的產品。 若要達成此目的，DataList 加入頁面並建立名為新 ObjectDataSource `ProductsByCategoryDataSource`。 具有`ProductsByCategoryDataSource`控制項擷取資料的來源`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 由於此主要/詳細資料報表是唯讀的選擇 （無） 選項在 INSERT、 UPDATE 和 DELETE 的索引標籤中。


[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**圖 7**： 選取`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


按一下 下一步之後，ObjectDataSource 精靈會提示我們之值的來源`GetProductsByCategoryID(categoryID)`方法的*`categoryID`* 參數。 若要使用的所選值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。


[![CategoryID 參數值設定為類別 DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**圖 8**： 設定*`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


完成設定資料來源精靈，Visual Studio 會自動產生`ItemTemplate`如 DataList 顯示的名稱和每個資料欄位的值。 讓我們來增強改用 DataList`ItemTemplate`顯示只要產品的名稱、 類別、 供應商，每個單位和價格以及數量`SeparatorTemplate`，插入`<hr>`每個項目之間的項目。 我要使用`ItemTemplate`範例從[顯示的資料，以在 DataList 和中繼器控制項](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)教學課程中，但可自由使用任何範本標記您找到最視覺上吸引人。

進行這些變更之後，DataList 和其 ObjectDataSource 標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

請花一點時間簽出的瀏覽器中的進度。 當第一次造訪的頁面時，屬於所選類別目錄 （飲料） 這些產品會顯示 （如圖 9 所示），但變更 DropDownList 不會更新資料。 這是因為更新在 DataList 一定會發生回傳。 若要完成此我們可以設定 DropDownList`AutoPostBack`屬性`true`或按鈕 Web 控制項加入至網頁。 此教學課程中，我選擇設定 DropDownList`AutoPostBack`屬性`true`。

數字 9 和 10 說明主要/詳細資料中的報表動作。


[![當第一次瀏覽頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**圖 9**： 當第一次瀏覽頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![選取新的產品 （產生） 會自動導致回傳，更新資料清單](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**圖 10**： 選取新的產品 （產生） 會自動導致回傳，更新在 DataList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>可以加入 「-選擇分類-」 清單項目

當第一次造訪`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品資料清單中選取的類別。 在*主要/詳細資料篩選與 DropDownList*教學課程中我們加入 「-選擇分類-」 的選項，已預設選取，如果選取，顯示 DropDownList*所有*的在資料庫中的產品。 這種方法時可管理以列出產品 GridView，為每個產品的資料列花費少量的實際螢幕面積組成。 不過，DataList，與每個產品資訊耗用較大區塊的螢幕。 我們仍加入 「-選擇分類-」 選項，依預設選取，但而不是讓它顯示所有產品選取時，我們將它設定為讓它會顯示任何產品。

DropDownList 中加入新的清單項目，請前往 [屬性] 視窗，按一下省略符號在`Items`屬性。 加入新的清單項目與`Text`"-選擇一個類別目錄-"和`Value` `0`。


![新增](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**圖 11**： 加入 「-選擇分類-」 清單項目


或者，您可以將下列標記加入至 DropDownList 新增清單項目：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

此外，我們需要將 DropDownList 控制項的`AppendDataBoundItems`至`true`因為如果設定為`false`（預設值），分類繫結至 DropDownList 從 ObjectDataSource 時，它們會覆寫任何手動加入清單項目。


![AppendDataBoundItems 屬性設為 True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**圖 12**： 設定`AppendDataBoundItems`屬性設定為 True


因此我們選擇值`0`「-選擇分類-」 清單項目是因為值是系統中有無類別`0`，因此沒有產品記錄時，會傳回 「-選擇分類-」 清單項目已選取。 若要確認這一點，請花一點時間瀏覽的頁面，透過瀏覽器。 如圖 13 所示，一開始檢視頁面 「-選擇分類-」 清單中選取項目，並不顯示任何產品時。


[![當](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**圖 13**： 選取 「-選擇分類-」 清單項目時，會顯示沒有產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


而是會顯示如果*所有*產品的 「-選擇分類-」 選取選項時，使用值`-1`改為。 精明讀取器將會回收該回*主要/詳細資料篩選與 DropDownList*我們已更新的教學課程`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法以便如果*`categoryID`* 值`-1`未傳回記錄的所有產品中傳遞。

## <a name="summary"></a>總結

顯示階層關聯的資料時，這通常有助於呈現的資料，使用主要/詳細資料報表，使用者可以從中啟動研究的資料階層的頂端，並向下鑽研到詳細資料。 在此教學課程中我們會檢查建置簡單的主要/詳細資料報表，顯示所選類別目錄的產品。 這是使用 DropDownList DataList 屬於所選類別目錄的產品類別目錄清單以完成。

在下一個教學課程中，我們將探討跨兩個的頁面分隔 master 和詳細資料記錄。 在第一個頁面中，「 主要 」 記錄的清單隨即出現，以檢視詳細資料的連結。 按一下連結，將 whisk 使用者第二個頁面上，將會顯示選取的主要記錄的詳細資料。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已袁 Schmidt。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](master-detail-filtering-acess-two-pages-datalist-cs.md)
