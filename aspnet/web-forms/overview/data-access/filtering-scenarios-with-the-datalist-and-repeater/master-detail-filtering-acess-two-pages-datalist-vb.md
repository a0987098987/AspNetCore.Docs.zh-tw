---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: 主版/詳細篩選跨兩個頁面 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們看看如何跨兩個頁面中個別的主要/詳細資料報表。 在 'master' 的頁面中，我們會使用重複項控制項來呈現一份 categ...
ms.author: aspnetcontent
ms.date: 10/30/2010
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cfbd685344bdd223f8d07f8bad5a54b63735839
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813685"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>主版/詳細篩選跨兩個頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe)或[下載 PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> 在本教學課程，我們看看如何跨兩個頁面中個別的主要/詳細資料報表。 在 「 主要 」 的頁面中，我們會使用重複項控制項呈現類別清單，當按下時，會將使用者帶到 [詳細資料] 頁面其中兩個資料行的資料清單會顯示屬於所選分類的產品。


## <a name="introduction"></a>簡介

在 [主版/詳細篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-vb.md)教學課程中，我們會檢查使用 GridView 顯示所有的供應商在系統中此模式。 此 GridView 包含 HyperLinkField，轉譯為連結至第二個的頁面，並傳遞`SupplierID`於 querystring 中。 第二個頁面，列出所選取的供應商提供這些產品使用 GridView。

使用 DataList 與重複項控制項時也可以完成這類的兩個頁面主版/詳細報告。 唯一的差別是 DataList 和 Repeater 都不提供支援 HyperLinkField 控制項。 相反地，我們必須加入超連結 Web 控制項或錨點 HTML 元素 (`<a>`) 在控制項內`ItemTemplate`。 HyperLink`NavigateUrl`屬性或錨點的`href`屬性然後可以開始自訂使用宣告式或程式設計的方式。

在本教學課程中，我們將探討的範例會列出在使用重複項控制項的單一頁面上的項目符號清單中的類別。 每個清單項目會包含類別目錄的名稱和描述，做為第二個頁面的連結會顯示類別目錄名稱。 按一下此連結會 whisk 到第二個頁面上，使用者所在的 DataList 會顯示屬於所選分類的產品。

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>步驟 1： 在項目符號清單中顯示類別目錄

建立任何主要/詳細資料報表的第一個步驟是先從顯示的 「 主要 」 的記錄開始。 因此，我們的第一個工作是在 「 主要 」 的網頁中顯示的類別。 開啟`CategoryListMaster.aspx`頁面中`DataListRepeaterFiltering`資料夾中，會新增重複項控制項，，和從智慧標籤中，選擇 加入新的 ObjectDataSource。 設定新的 ObjectDataSource，以便存取其資料從`CategoriesBLL`類別的`GetCategories`方法 （請參閱 圖 1）。


[![設定要使用 CategoriesBLL 類別的 GetCategories 方法的 ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**圖 1**： 設定要使用 ObjectDataSource`CategoriesBLL`類別的`GetCategories`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


接下來，定義 Repeater 的範本，使它為項目符號清單中的項目會顯示每個類別目錄名稱和描述。 讓我們尚不擔心是否有每個類別目錄詳細資料頁面連結。 下面顯示的 Repeater 和 ObjectDataSource 宣告式標記：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

使用這個完整的標記，請花一點時間檢閱我們透過瀏覽器的進度。 如 [圖 2] 所示，Repeater 會呈現為項目符號清單顯示每個類別目錄的名稱和描述。


[![每個類別目錄會顯示為項目符號清單項目](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**圖 2**： 每個類別會顯示為項目符號清單項目 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>步驟 2： 將類別目錄名稱轉換成詳細資料頁面的連結

若要允許的使用者以顯示指定的類別目錄的 [詳細資料] 資訊，我們需要加入至每個項目符號清單項目，當按下時，會將使用者帶到第二個頁面的連結 (`ProductsForCategoryDetails.aspx`)。 此第二個的頁面就會顯示選取之類別目錄使用 DataList 的產品。 若要判斷其連結已按下的分類，我們要傳遞已按下 類別目錄的`CategoryID`至第二個頁面，透過一些機制。 從一頁的純量的資料傳輸至另一個簡單且最直接的方式是透過查詢字串中，我們將在本教學課程使用的選項。 特別是， `ProductsForCategoryDetails.aspx`  頁面就會預期收到所選*`categoryID`* 通過一個名為的 querystring 欄位的值`CategoryID`。 例如，若要檢視產品的飲料類別目錄，其中包含`CategoryID`為 1，使用者會瀏覽`ProductsForCategoryDetails.aspx?CategoryID=1`。

我們要將超連結 Web 控制項或 HTML 錨定項目加入的中繼器中建立每個項目符號清單項目的超連結 (`<a>`) 以`ItemTemplate`。 中的情況下超連結會顯示相同的每個資料列中，兩種方法就夠了。 對於重複項，我想使用錨定項目。 若要使用錨定項目，更新 Repeater 的 ItemTemplate:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

請注意，`CategoryID`可以直接在錨定項目插入`href`屬性; 不過，若要執行因此一定要分隔`href`屬性的值與單引號 （附註引號） 自`Eval`方法內`href`屬性會分隔其字串 (`"CategoryID"`) 加上引號。 或者，可以改為使用超連結 Web 控制項：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

請注意如何 URL 的靜態部分 — `ProductsForCategoryDetails.aspx?CategoryID` — 會附加至結果`Eval("CategoryID")`中使用字串串連的資料繫結語法直接。

使用超連結控制項的其中一個優點是，可以在從 Repeater 以程式設計方式存取`ItemDataBound`事件處理常式，如有需要。 例如，您可能要為文字而不是與沒有相關聯的產品類別目錄的連結，顯示該類別名稱。 在無法以程式設計方式執行這類檢查`ItemDataBound`事件處理常式; 類別沒有相關聯的產品，超連結的`NavigateUrl`屬性無法設為空白的字串，進而導致該特定類別名稱轉譯為純文字 （而非連結）。 回頭[格式化 DataList 和 Repeater 基礎時的資料](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md)透過程式設計邏輯為基礎的教學課程，如需有關格式化 DataList 和 Repeater 的內容`ItemDataBound`事件處理常式。

如果您要遵照，歡迎使用頁面中的錨定項目或超連結控制項的方法。 無論種方法，檢視透過每個類別目錄名稱應該轉譯為連結的瀏覽器頁面時`ProductsForCategoryDetails.aspx`，並傳入適用`CategoryID`值 （請參閱 [圖 3]）。


[![類別目錄名稱現在是連結到 ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**圖 3**: 類別目錄名稱現在連結到`ProductsForCategoryDetails.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>步驟 3： 列出屬於所選分類的產品

具有`CategoryListMaster.aspx`完整的頁面上，我們已經準備好把焦點轉到實作 [詳細資料] 頁面上， `ProductsForCategoryDetails.aspx`。 開啟此頁面，從 [工具箱] 拖曳至設計工具中，拖曳 DataList 和設定其`ID`屬性設`ProductsInCategory`。 接下來，從 DataList 的智慧標籤選擇 加入新的 ObjectDataSource 頁面上，將它命名為`ProductsInCategoryDataSource`。 設定它，它會呼叫`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法; 下拉式清單會列出為 （無） 與插入、 更新和刪除索引標籤中的設定。


[![設定要使用 ProductsBLL 類別的 GetProductsByCategoryID(categoryID) 方法的 ObjectDataSource](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


由於`GetProductsByCategoryID(categoryID)`方法會接受輸入的參數 (*`categoryID`*)，[選擇資料來源精靈] 可讓我們得以指定參數的來源。 設定參數來源至查詢字串使用 QueryStringField `CategoryID`。


[![Querystring 欄位 CategoryID 做為參數的來源](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**圖 5**： 使用查詢字串欄位`CategoryID`做為參數的來源 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Visual Studio 如我們所見先前的教學課程中，選擇資料來源精靈中，完成後會自動建立`ItemTemplate`的 DataList，其中列出每個資料欄位名稱和值。 此範本取代另一個會列出只有產品的名稱、 供應商和價格。 此外，設定 DataList 的`RepeatColumns`屬性設為 2。 在這些變更之後, 您 DataList 與 ObjectDataSource 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

若要檢視這個頁面作用中，從啟動`CategoryListMaster.aspx`分頁; 接下來，按一下 類別目錄項目符號清單中的連結。 這樣會帶您前往`ProductsForCategoryDetails.aspx`，並傳遞沿著`CategoryID`透過查詢字串。 `ProductsInCategoryDataSource`內的 ObjectDataSource`ProductsForCategoryDetails.aspx`接著取得指定分類的產品並顯示它們 DataList，呈現每個資料列的兩個產品中。 [圖 6] 顯示的螢幕擷取畫面`ProductsForCategoryDetails.aspx`檢視飲料時。


[![顯示的飲料，每個資料列的兩個](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**圖 6**： 顯示的飲料，每個資料列的兩個 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>步驟 4: ProductsForCategoryDetails.aspx 上顯示類別目錄資訊

當使用者按一下中的類別上`CategoryListMaster.aspx`，便會進入`ProductsForCategoryDetails.aspx`並顯示屬於所選分類的產品。 不過，在`ProductsForCategoryDetails.aspx`有關於哪個類別已選取任何視覺提示。 想要按一下飲料，但是不小心按下 「 調味品 」，使用者有沒有辦法實現其錯誤，一旦達到`ProductsForCategoryDetails.aspx`。 若要減少這種潛在問題，我們可以顯示選取的類別目錄的相關資訊，其名稱和描述，頂端的`ProductsForCategoryDetails.aspx`頁面。

若要達成此目的，將新增 Repeater 控制項上方 FormView `ProductsForCategoryDetails.aspx`。 從名為的 FormView 的智慧標籤接下來，新增至頁面的新 ObjectDataSource`CategoryDataSource`並將它設定為使用`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`方法。


[![透過 CategoriesBLL 類別的 GetCategoryByCategoryID(categoryID) 方法的類別目錄的存取資訊](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**圖 7**： 透過類別目錄的存取資訊`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


如同`ProductsInCategoryDataSource`ObjectDataSource 加入在步驟 3 中，`CategoryDataSource`的設定資料來源精靈會提示我們輸入的來源`GetCategoryByCategoryID(categoryID)`方法的輸入參數。 使用完全相同的設定之前，查詢字串和 QueryStringField 值，以設定參數來源`CategoryID`（請參閱上一步 圖 5）。

完成精靈之後，Visual Studio 會自動建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`FormView 的。 因為我們提供唯讀介面，放心地移除`EditItemTemplate`和`InsertItemTemplate`。 此外，您可以自訂 FormView 的`ItemTemplate`。 移除多餘的範本和自訂 ItemTemplate 之後, 您 FormView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

[圖 8] 顯示螢幕擷取畫面，檢視此頁面，透過瀏覽器時。

> [!NOTE]
> 除了 FormView，我在裡面加入超連結控制項上面，會讓使用者回到的類別清單 FormView (`CategoryListMaster.aspx`)。 請隨意將此連結放在其他位置或完全省略它。


[![類別資訊則是現在顯示在頁面頂端](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**圖 8**： 現在顯示在頁面頂端的類別目錄資訊 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>步驟 5： 顯示訊息，如果沒有產品屬於所選取的類別

`CategoryListMaster.aspx`頁面會列出所有類別目錄的系統，不論是否有任何相關聯的產品。 如果使用者按一下分類沒有相關聯的產品，在 DataList`ProductsForCategoryDetails.aspx`就不會呈現出來，因為其資料來源沒有任何項目。 我們在過去的教學課程中所見，GridView 提供`EmptyDataText`可用來指定其資料來源中沒有任何記錄時所要顯示的文字訊息的屬性。 不幸的是，DataList 和 Repeater 都不會有這類屬性。

若要顯示訊息，通知使用者，選取的類別沒有相符的產品，我們需要新增標籤控制項頁面`Text`屬性會被指定要顯示，沒有相符產品的訊息。 我們就需要以程式設計方式設定其`Visible`屬性，根據 DataList 是否包含任何項目。

若要達成此目的，先新增 DataList 下方的標籤。 設定其`ID`屬性，以`NoProductsMessage`及其`Text`"There are 沒有選取的類別目錄 的產品 」 的屬性接下來，我們需要以程式設計方式設定此標籤`Visible`屬性，根據任何資料是否繫結至`ProductsInCategory`DataList。 已繫結至 DataList 的資料之後，就必須進行這項指派。 如 GridView、 DetailsView 和 FormView，我們可以建立控制項的事件處理常式`DataBound`完成資料繫結之後所引發的事件。 不過，DataList 和 Repeater 都不具有`DataBound`可用的事件。

針對此範例中，我們可以指派的標籤`Visible`中的屬性`Page_Load`事件處理常式，因為資料會被指派給之前頁面的 DataList`Load`事件。 不過，這種方法會不適用於一般的情況下，因為從 ObjectDataSource 資料可能會繫結至 DataList 稍後網頁的生命週期。 比方說，如果顯示的資料根據另一個控制項中的值，像是 it's 顯示來保存 「 主要 」 的記錄使用 dropdownlist 進行主要/詳細資料報表時，資料可能不重新繫結至資料 Web 控制項，直到`PreRender`暫置中在頁面生命週期。

這將適用於所有情況下的其中一個解決方案是將指派`Visible`屬性，以`False`在 DataList `ItemDataBound` (或`ItemCreated`) 事件處理常式繫結的項目類型時`Item`或`AlternatingItem`。 在此情況下我們知道有至少一個資料資料來源中項目，然後因此可以隱藏`NoProductsMessage`標籤。 除了這個事件處理常式中，我們還需要的事件處理常式的 DataList`DataBinding`事件，我們用來初始化標籤的`Visible`屬性設`True`。 由於`DataBinding`事件引發之前`ItemDataBound`事件，標籤的`Visible`屬性一開始會設定為`True`; 如果有任何資料的項目，不過，它就會設定為`False`。 下列程式碼會實作此邏輯：

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

所有的 Northwind 資料庫中的類別都是相關聯與一或多個產品。 若要測試這項功能，我手動調整 Northwind 資料庫在此教學課程中，重新產生類別目錄相關聯的所有產品的都指派 (`CategoryID` = 7) Seafood 類別目錄 (`CategoryID` = 8)。 這可透過從 [伺服器總管] 中選擇新的查詢，並使用下列`UPDATE`陳述式：

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

據以更新資料庫之後, 返回`CategoryListMaster.aspx`頁面，然後按一下 [產生] 連結。 由於不會再有任何屬於產生類別目錄的產品，因此您應該會看到"There are 沒有選取的類別目錄] 的產品 」 訊息，如 [圖 9 所示。


[![如果沒有產品屬於選取的類別，顯示一則訊息](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**圖 9**： 如果有沒有產品屬於選取的類別，會顯示訊息 ([按一下以檢視完整大小的影像](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>總結

雖然主版/詳細報告可以顯示主要和詳細的記錄，在單一頁面上，在許多網站它們會被分到兩個網頁。 在本教學課程中我們探討了如何實作這類主版/詳細報告，「 主要 」 的網頁上使用重複項的項目符號清單中所列的類別和 [詳細資料] 頁面中所列的相關聯的產品。 在主版頁面中的每個清單項目所包含的資料列一起傳遞的詳細資料頁面的連結`CategoryID`值。

透過已完成的詳細資料頁面擷取這些產品指定的供應商`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。 *`categoryID`* 使用以宣告方式指定參數值`CategoryID`做為參數來源的查詢字串值。 我們也會探討如何使用 FormView 的詳細資料頁面中顯示類別目錄詳細資料，以及如何顯示一則訊息，如果沒有任何屬於所選分類的產品。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones 和 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [下一頁](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
