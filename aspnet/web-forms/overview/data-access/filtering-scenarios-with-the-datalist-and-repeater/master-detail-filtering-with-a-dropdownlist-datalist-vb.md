---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: 主版/詳細篩選使用 dropdownlist 進行 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們看到如何來顯示單一的 web 網頁，以顯示 要顯示的 'master' 的記錄和 DataList 使用 dropdownlist 進行主版/詳細報告...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e88a7459529e003b73ef5a42456de3501e3db461
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829317"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>主版/詳細篩選使用 dropdownlist 進行 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> 在本教學課程中，我們看到如何來顯示單一的 web 網頁，以顯示 「 主要 」 的記錄和 DataList，以顯示 [詳細資料] 中使用 dropdownlist 進行主版/詳細報告。


## <a name="introduction"></a>簡介

主版/詳細報告，其中我們第一次建立在先前使用 GridView[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教學課程中，一開始會顯示某些的 「 主要 」 的記錄集。 使用者可以再向下切入到其中一個主要記錄中，藉此檢視的主要記錄的 [詳細資料。] 主版/詳細報告是以視覺化方式呈現一對多關聯性，以及顯示 （亦即有許多資料行） 特別 「 寬 」 的資料表的詳細的資訊的理想選擇。 我們探討了如何實作在先前的教學課程使用 GridView 和 DetailsView 控制項的主版/詳細報告。 在本教學課程 和 下一步 的兩個，我們會檢查這些概念，但著重於使用 DataList 和 Repeater 控制項改。

在本教學課程中，我們將探討使用 dropdownlist 進行包含的 「 主要 」 記錄，[詳細資料] 中顯示的記錄資料清單。

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>步驟 1： 加入主版/詳細教學課程的 Web 網頁

在開始本教學課程之前，讓我們先花一點時間加入資料夾和我們需要針對本教學課程和因應使用 DataList 與重複項控制項的主版/詳細報告的下一步 兩個 ASP.NET 網頁。 藉由建立新的資料夾中名為專案啟動`DataListRepeaterFiltering`。 接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![建立 DataListRepeaterFiltering 資料夾，並新增這些教學課程的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**圖 1**： 建立`DataListRepeaterFiltering`資料夾並新增教學課程的 ASP.NET 頁面


接下來，開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。 此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，會列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


若要有項目符號清單顯示主版/詳細教學課程，我們將建立，我們要將它們新增至站台對應。 開啟`Web.sitemap`檔案，並 「 顯示的資料與 DataList 和 Repeater 」 站台對應節點標記之後新增下列標記：

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>步驟 2： 顯示在 DropDownList 中的類別

我們的主要/詳細資料報表會顯示選取的清單項目的產品清單的類別中的下拉式清單中，進一步在 DataList 的頁面。 超越我們，第一項工作，則將顯示在 DropDownList 中的類別。 首先開啟`FilterByDropDownList.aspx`頁面中`DataListRepeaterFiltering`資料夾，然後從 [工具箱] 拖曳至頁面的設計工具的 dropdownlist 進行拖曳。 接下來，設定 DropDownList`ID`屬性設`Categories`。 按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結，建立名為新 ObjectDataSource `CategoriesDataSource`。


[![新增名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**圖 4**： 新增新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


設定，它會叫用的新 ObjectDataSource`CategoriesBLL`類別的`GetCategories()`方法。 設定我們仍然需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 後其中一個應該為每個清單項目的值相關聯。 已`CategoryName`欄位做為顯示和`CategoryID`做為每個清單項目的值。


[![做為值的類別名稱 欄位和使用 CategoryID 有 DropDownList 顯示](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


現在我們有 DropDownList 控制項，其中會填入來自記錄`Categories`資料表 （全部在大約六秒內完成）。 圖 6 顯示我們進行到目前為止透過瀏覽器檢視時。


[![下拉式清單會列出目前的類別](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**圖 6**: 下拉式清單會列出目前的類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>步驟 2： 加入產品 DataList

我們的主要/詳細資料報表的最後一個步驟是列出與選取的類別相關聯的產品。 若要這麼做，DataList 新增至頁面，並建立名為新 ObjectDataSource `ProductsByCategoryDataSource`。 已`ProductsByCategoryDataSource`控制項擷取資料的來源`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 由於此主版/詳細報告是唯讀的選擇 （無） 選項在 INSERT、 UPDATE 和 DELETE 的索引標籤中。


[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**圖 7**： 選取 `GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


按一下 [下一步] 之後, ObjectDataSource 精靈會提示我們輸入之值的來源`GetProductsByCategoryID(categoryID)`方法的*`categoryID`* 參數。 若要使用選取的值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。


[![設定為值的分類 DropDownList categoryID 參數](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**圖 8**： 設定*`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


完成後設定資料來源精靈，Visual Studio 會自動產生`ItemTemplate`的 DataList 顯示的名稱和每個資料欄位的值。 讓我們來增強改用 DataList `ItemTemplate` ，顯示只是產品的名稱、 類別、 供應商、 每單位和價格以及數量`SeparatorTemplate`，會插入`<hr>`各個項目之間的項目。 我要使用`ItemTemplate`中的範例[使用 DataList 與重複項控制項顯示的資料](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md)教學課程中，但是可以隨意使用任何範本標記您找到最美觀。

進行這些變更之後，您的 DataList 和其 ObjectDataSource 的標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

花點時間查看我們的瀏覽器中的進度。 當第一次瀏覽的頁面，（如 圖 9 所示），會顯示屬於所選取的類別 （飲料） 這些產品，但變更 DropDownList 不會更新資料。 這是因為回傳會因發生更新 DataList。 若要這麼做我們可以設定 DropDownList`AutoPostBack`屬性設`true`或 Button Web 控制項加入頁面。 本教學課程中，我選擇設定 DropDownList`AutoPostBack`屬性設`true`。

圖 9 和 10 說明主要/詳細資料報表動作中。


[![當第一次瀏覽的頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**圖 9**： 當第一次瀏覽的頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![選取新的產品 （產生） 會自動造成回傳，更新 DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**圖 10**： 選取新的產品 （產生） 會自動造成回傳，更新 DataList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>新增"--選擇類別目錄-」 清單項目

第一次瀏覽時`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品 DataList 中選取的類別。 在 *主版/詳細篩選使用 DropDownList*教學課程中我們將"--選擇類別目錄-」 的選項加入，已選取預設情況下，選取時，顯示 DropDownList*所有*的在資料庫中的產品。 這種方式是可管理的列出一個 GridView 中的產品時，每個產品的資料列一職，少量的螢幕畫面。 具有 DataList，不過，每項產品的資訊會耗用較大區塊的畫面。 我們仍新增"--選擇類別目錄-」 的選項並讓它依預設選取，但而不是讓它顯示所有產品選取時，讓我們設定，讓它會顯示任何產品。

若要將新的清單項目新增至 DropDownList 中，移至 [屬性] 視窗，然後按一下省略符號，`Items`屬性。 加入新的清單項目具有`Text`"--選擇類別目錄-"和`Value` `0`。


![新增](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**圖 11**： 新增"--選擇類別目錄-」 的清單項目


或者，您也可以藉由將下列標記新增至 DropDownList 新增清單項目：

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

此外，我們需要設定 DropDownList 控制項`AppendDataBoundItems`要`true`因為如果設定為`false`（預設值），當類別繫結至 DropDownList 從 ObjectDataSource 它們將會覆寫任何手動加入的清單項目。


![AppendDataBoundItems 屬性設定為 True](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**圖 12**： 設定`AppendDataBoundItems`屬性設為 True


我們所選擇的值的原因`0`的 「-選擇類別目錄-」 的清單項目是因為值是系統中有無類別`0`，因此沒有產品記錄時，會傳回"--選擇類別目錄-」 清單項目已選取。 若要確認這一點，請花一點時間瀏覽透過瀏覽器頁面。 如 圖 13 所示，一開始檢視頁面的 「-選擇類別目錄-」 清單項目已選取，並不顯示任何產品時。


[![時](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**圖 13**： 選取 [-選擇類別目錄-] 清單項目時，會顯示沒有產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


而是會顯示如果*所有*的產品時選取 「-選擇類別目錄-」 的選項時，使用值`-1`改。 精明的讀者應該還記得該回溯*主版/詳細篩選使用 DropDownList*我們已更新的教學課程`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法，讓如果*`categoryID`* 值為`-1`傳入，傳回的記錄的所有產品。

## <a name="summary"></a>總結

顯示階層關聯的資料時，通常最好來呈現使用主版/詳細報告，使用者可以從中開始研究過的資料階層的頂端，並向下鑽研詳細資料的資料。 在本教學課程中，我們檢查建置簡單的主要/詳細資料報表，顯示所選取之目錄的產品。 這被透過使用 dropdownlist 進行分類和產品屬於所選分類的 DataList 的清單。

在下一個教學課程中，我們將探討跨兩個頁面分隔的主要和詳細資料記錄。 在第一個頁面中，「 主要 」 的記錄清單隨即出現，以檢視詳細資料的連結。 按一下連結，將 whisk 使用者第二個頁面上，將會顯示所選的主要記錄的詳細資料。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Randy Schmidt。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [下一頁](master-detail-filtering-acess-two-pages-datalist-vb.md)
