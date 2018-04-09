---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: 主從式篩選跨兩個頁面 (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程，我們看看如何跨兩個的頁面分隔主要/詳細資料報表。 在 'master' 的頁面中，我們會使用中繼器控制項來呈現一份 categ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 2afc216de3b6894cfdd112787ab92d7483198ecc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>主從式篩選跨兩個頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe)或[下載 PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> 在本教學課程，我們看看如何跨兩個的頁面分隔主要/詳細資料報表。 在 「 主要 」 頁面中，我們會使用中繼器控制項呈現類別目錄的清單，按下時，會將使用者導向至 [詳細資料] 頁面其中兩欄資料清單會顯示屬於所選類別目錄的產品。


## <a name="introduction"></a>簡介

在[主要/詳細資料篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-vb.md)教學課程中，我們會檢查此系統中顯示所有供應商使用的 GridView 的模式。 此 GridView 包含 HyperLinkField，呈現為第二個頁面上，傳遞至連結`SupplierID`於 querystring 中。 第二頁會用來列出所選取的供應商提供的產品 GridView。

這類兩頁主要/詳細資料報表，可以使用一併 DataList 和中繼器控制項來完成。 唯一的差別，是在 DataList 和中繼器都不提供支援 HyperLinkField 控制項。 相反地，我們必須加入超連結 Web 控制項或 HTML 錨定項目 (`<a>`) 內控制項的`ItemTemplate`。 超連結的`NavigateUrl`屬性或錨點的`href`屬性然後可以開始自訂使用宣告式或程式設計方法。

在本教學課程中，我們將探討的範例會列出在使用中繼器控制項的單一頁面上的項目符號清單中的類別。 每個清單項目會包含類別目錄的名稱和描述，做為第二個頁面的連結會顯示類別目錄名稱。 按一下此連結將 whisk 到第二個頁面上，使用者 DataList 其中會顯示屬於所選類別目錄的產品。

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>步驟 1： 在項目符號清單中顯示類別目錄

建立任何主要/詳細資料報表的第一個步驟是開始所顯示的 「 主要 」 的記錄。 因此，我們第一項工作是在 「 主要 」 的頁面中顯示的類別。 開啟`CategoryListMaster.aspx`頁面`DataListRepeaterFiltering`資料夾，會將中繼器控制項中，加入，然後從智慧標籤中，選擇 加入新 ObjectDataSource。 設定新 ObjectDataSource，讓它存取其資料從`CategoriesBLL`類別的`GetCategories`方法 （請參閱圖 1）。


[![設定為使用 CategoriesBLL 類別 GetCategories 方法 ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**圖 1**： 設定要使用 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


接下來，定義中繼器範本，使它做為項目符號清單中的項目會顯示每個分類名稱和描述。 讓我們尚未擔心是否有每個類別目錄的詳細資料頁面的連結。 下圖顯示中繼器和 ObjectDataSource 宣告式標記：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

使用這個完整的標記，請花一點時間檢視進度透過瀏覽器。 如圖 2 所示，中繼器轉譯成活頁的項目符號清單會顯示每個類別目錄名稱和描述。


[![每個類別目錄會顯示為清單項目符號](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**圖 2**： 每個類別會顯示為項目符號清單項目 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>步驟 2： 將類別名稱轉換成詳細資料頁面的連結

若要讓使用者以顯示與指定的類別目錄的 [詳細資料] 資訊，我們需要加入至每個項目符號清單項目，按下時，會將使用者導向至第二個頁面的連結 (`ProductsForCategoryDetails.aspx`)。 此第二個頁面就會顯示所選取的類別目錄使用 DataList 產品。 若要判斷已按下其連結的類別目錄，我們要通過按過的類別目錄`CategoryID`透過一些機制的第二頁。 從一頁的純量資料傳送至另一個簡單且最直接的方式是透過查詢字串中，我們將在本教學課程中使用此選項。 特別是，`ProductsForCategoryDetails.aspx`頁面就會預期收到所選*`categoryID`*傳遞透過名為的 querystring 欄位值`CategoryID`。 例如，若要檢視 「 飲料 」 分類的產品具有`CategoryID`使用者會是 1，請瀏覽`ProductsForCategoryDetails.aspx?CategoryID=1`。

我們需要將超連結 Web 控制項或 HTML 錨定項目加入的中繼器中建立每個項目符號清單項目的超連結 (`<a>`) 至`ItemTemplate`。 中的情況下超連結可顯示每個資料列相同、 兩種方法就夠了。 中繼器，我願意使用錨定項目。 若要使用錨定項目，更新至中繼器 ItemTemplate:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

請注意，`CategoryID`可直接在錨定項目插入`href`屬性; 不過，若要執行因此一定要分隔`href`屬性的值與所有格符號 （附註引號） 後`Eval`方法內`href`屬性分隔的字串 (`"CategoryID"`) 加上引號。 或者，可以改為使用超連結 Web 控制項：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

請注意如何 URL 的靜態部分 — `ProductsForCategoryDetails.aspx?CategoryID` — 的結果會附加`Eval("CategoryID")`直接在資料繫結語法使用字串串連。

使用超連結控制項的其中一個優點是，可以在從中繼器以程式設計方式存取`ItemDataBound`事件處理常式，如有需要。 例如，您可以為文字而非與沒有相關聯的產品類別目錄的連結，顯示類別目錄名稱。 在無法以程式設計方式執行這類檢查`ItemDataBound`事件處理常式; 類別沒有相關聯的產品，超連結的`NavigateUrl`屬性無法設為空白字串，進而產生該特定類別名稱轉譯為純文字 （而非連結）。 回頭參考[格式化 DataList 和中繼器時資料](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md)教學課程，如需有關格式設定 DataList 和中繼器的內容會根據透過程式設計邏輯`ItemDataBound`事件處理常式。

如果您要遵照，歡迎頁面中使用錨定項目或超連結控制項的方法。 不論方法，當檢視透過每個類別目錄名稱應該轉譯為連結的瀏覽器頁面`ProductsForCategoryDetails.aspx`，並傳入之適用`CategoryID`值 （請參閱圖 3）。


[![類別目錄名稱現在連結到 ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**圖 3**: 類別目錄名稱現在連結到`ProductsForCategoryDetails.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>步驟 3： 列出屬於所選類別目錄的產品

與`CategoryListMaster.aspx`完成 頁面上，我們已經準備好開啟實作 詳細資料 頁面中，我們注意`ProductsForCategoryDetails.aspx`。 開啟此頁面，從 [工具箱] 拖曳至設計工具中，拖曳 DataList 和設定其`ID`屬性`ProductsInCategory`。 接下來，從 DataList 的智慧標籤選擇 [加入] 頁面上，其命名為新 ObjectDataSource `ProductsInCategoryDataSource`。 設定它，它會呼叫`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法; 下拉式清單會列出為 （無） 插入、 更新和刪除索引標籤中的設定。


[![設定為使用 ProductsBLL 類別 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


因為`GetProductsByCategoryID(categoryID)`方法會接受輸入的參數 (*`categoryID`*)，選擇資料來源精靈提供可讓我們有機會指定參數的來源。 將參數來源設定為使用 QueryStringField QueryString `CategoryID`。


[![Querystring 欄位 CategoryID 做為參數的來源](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**圖 5**： 使用查詢字串欄位`CategoryID`做為參數的來源 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Visual Studio 如我們所見在上一個教學課程中，選擇資料來源精靈中，完成後會自動建立`ItemTemplate`的資料清單，列出每個資料欄位名稱和值。 此範本取代另一個會列出只產品的名稱、 供應商和價格。 此外，設定 DataList `RepeatColumns` 2 的屬性。 這些變更之後，請您 DataList 和 ObjectDataSource 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

若要檢視這個頁面作用中，從啟動`CategoryListMaster.aspx`頁面上，接下來，請按一下 類別目錄項目符號清單中的連結。 這樣會帶您前往`ProductsForCategoryDetails.aspx`，並傳遞沿著`CategoryID`透過查詢字串。 `ProductsInCategoryDataSource`在 ObjectDataSource`ProductsForCategoryDetails.aspx`會取得指定的類別目錄僅擁有的產品，然後在 DataList，其會呈現這兩項產品每個資料列中顯示它們。 圖 6 顯示的螢幕擷取畫面`ProductsForCategoryDetails.aspx`檢視飲料時。


[![顯示的飲料，每個資料列的兩個](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**圖 6**： 顯示飲料，每個資料列的兩個 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>步驟 4： 上 ProductsForCategoryDetails.aspx 顯示類別目錄資訊

當使用者按一下中的類別上`CategoryListMaster.aspx`，它們會帶您到`ProductsForCategoryDetails.aspx`和顯示屬於所選類別目錄的產品。 不過，在`ProductsForCategoryDetails.aspx`有沒有視覺提示，並選取哪個類別。 使用者想要按一下飲料，但是不小心按下 「 調味品 」，便無法實現其錯誤一旦到達`ProductsForCategoryDetails.aspx`。 若要減輕此潛在的問題，我們可以顯示所選類別的相關資訊，其名稱和描述 — 頂端`ProductsForCategoryDetails.aspx`頁面。

若要完成此動作，新增中繼器控制項上方 FormView `ProductsForCategoryDetails.aspx`。 接下來，加入新 ObjectDataSource 頁面從名為在 FormView 的智慧標籤`CategoryDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`方法。


[![透過 CategoriesBLL 類別 GetCategoryByCategoryID(categoryID) 方法的類別目錄的存取資訊](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**圖 7**： 透過類別目錄的存取資訊`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


如同`ProductsInCategoryDataSource`ObjectDataSource 加入在步驟 3 中`CategoryDataSource`的設定資料來源精靈會提示輸入我們的來源`GetCategoryByCategoryID(categoryID)`方法的輸入參數。 使用相同的設定之前，查詢字串和 QueryStringField 值，以設定參數來源`CategoryID`（請參閱上一步圖 5）。

完成精靈之後，Visual Studio 會自動建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`在 FormView 的。 因為我們提供唯讀介面，則可以自由移除`EditItemTemplate`和`InsertItemTemplate`。 此外，您可以任意去自訂在 FormView 的`ItemTemplate`。 在移除多餘的範本，並自訂 ItemTemplate 之後, 您 FormView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

圖 8 顯示螢幕擷取畫面檢視此頁面，透過瀏覽器時。

> [!NOTE]
> 在 FormView 中，除了我新增了 FormView 將使用者引導回用類別目錄的清單上方的超連結控制項 (`CategoryListMaster.aspx`)。 隨意放置此連結的其他位置，或完全省略它。


[![類別目錄資訊是現在顯示在頁面頂端](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**圖 8**： 類別目錄資訊是現在顯示在頁面的頂端 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>步驟 5： 顯示訊息，如果沒有產品屬於所選類別目錄

`CategoryListMaster.aspx`頁面在系統中，列出所有類別目錄，不論是否有任何相關聯的產品。 如果使用者按一下與沒有相關聯的產品，在 DataList 類別`ProductsForCategoryDetails.aspx`不呈現，因為其資料來源沒有任何項目。 在 GridView 如同我們在過去的教學課程中，提供`EmptyDataText`可用來指定其資料來源中沒有的記錄時要顯示的文字訊息的屬性。 不幸的是，DataList 和中繼器都不會有這類屬性。

若要顯示訊息，通知使用者，確認選取的類別沒有相符的產品，我們需要加入標籤控制項至頁面`Text`屬性會被指定要顯示的沒有相符產品的訊息。 然後，我們需要以程式設計方式設定其`Visible`屬性根據資料清單是否包含任何項目。

若要完成此動作，啟動加在 DataList 下方的標籤。 設定其`ID`屬性`NoProductsMessage`及其`Text`"There are 任何產品選取的類別目錄..."的屬性 接下來，我們必須以程式設計方式設定這個標籤`Visible`屬性會根據是否任何資料已繫結至`ProductsInCategory`DataList。 已繫結至資料清單的資料之後，必須進行這項指派。 GridView、 DetailsView，以及在 FormView 中，我們無法建立控制項的事件處理常式`DataBound`資料繫結完成之後引發的事件。 不過，在 DataList 都中繼器有`DataBound`可用的事件。

針對此範例中，我們可以指派的標籤`Visible`屬性`Page_Load`事件處理常式，因為資料會被指派給頁面之前 DataList`Load`事件。 不過，這種方法不會執行在一般情況下，因為從 ObjectDataSource 資料可能會繫結至資料清單網頁的生命週期的更新版本中。 比方說，如果所顯示的資料根據另一個控制項中的值，例如顯示的主要/詳細資料報表是使用 DropDownList 以保存 「 主要 」 的記錄時，資料可能不重新繫結至資料的 Web 控制項，直到`PreRender`暫置中網頁的生命週期。

這適用於所有情況下的其中一種解決方案是將指派`Visible`屬性`False`在 DataList `ItemDataBound` (或`ItemCreated`) 事件處理常式繫結的項目類型時`Item`或`AlternatingItem`。 在這類情況下我們知道有至少一個資料資料來源中的項目，因此可以隱藏`NoProductsMessage`標籤。 除了這個事件處理常式中，我們還需要的事件處理常式 DataList`DataBinding`事件，我們用來初始化的標籤`Visible`屬性`True`。 因為`DataBinding`事件引發之前`ItemDataBound`事件，此標籤的`Visible`屬性一開始會設定為`True`; 如果沒有任何資料的項目，不過，它就會設定為`False`。 下列程式碼會實作此邏輯：

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

所有 Northwind 資料庫中的類別會產生關聯的一或多個產品。 若要測試這項功能，我已經手動調整 Northwind 資料庫在此教學課程中，重新指派與產生的類別目錄相關聯的所有產品 (`CategoryID` = 7) Seafood 類別目錄 (`CategoryID` = 8)。 達成這點可以從 [伺服器總管] 中選擇新的查詢，並使用下列`UPDATE`陳述式：

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

據以更新資料庫之後，返回`CategoryListMaster.aspx`頁面，然後按一下 [產生] 連結。 因為不再有任何屬於產生類別目錄的產品，您應該會看到"There are 任何產品選取的類別目錄..." 訊息，如圖 9 所示。


[![如果沒有產品屬於選取的類別，顯示一則訊息](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**圖 9**: No 產品屬於選取的類別目錄時，會顯示訊息 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>總結

雖然主要/詳細資料報表可以在單一頁面上會顯示主要和詳細記錄，在許多網站會分隔跨兩個網頁。 在此教學課程中我們討論了如何實作主要/詳細資料報表具有 「 主要 」 的網頁中使用中繼器的項目符號清單中所列的類別和相關聯的產品列在 [詳細資料] 頁面中。 主版網頁中的每個清單項目所包含的資料列所傳遞的詳細資料頁面的連結`CategoryID`值。

透過已完成的詳細資料頁面擷取指定的供應商的那些產品`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 *`categoryID`*使用以宣告方式指定參數值`CategoryID`querystring 值做為參數的來源。 我們也討論了如何使用程式設計的詳細資料頁面中顯示類別目錄詳細資料以及如何顯示一則訊息，如果沒有任何屬於所選類別目錄的產品。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones 和 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [下一頁](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
