---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: 篩選跨兩個頁面 (C#) 的主要/詳細資料 |Microsoft 文件
author: rick-anderson
description: 本教學課程中我們將會列出供應商的資料庫使用 GridView 實作此模式。 在 GridView 中的每個供應商資料列將包含 Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-c"></a>主從式篩選跨兩個頁面 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe)或[下載 PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> 本教學課程中我們將會列出供應商的資料庫使用 GridView 實作此模式。 在 GridView 中的每個供應商資料列會包含，按下時，會將使用者導向至個別頁面，列出這些產品選取供應商檢視產品連結。


## <a name="introduction"></a>簡介

在先前的兩個教學課程中我們了解如何[使用 DropDownLists 單一網頁中顯示主要/詳細資料報告](master-detail-filtering-with-a-dropdownlist-cs.md)來[顯示 「 主要 」 的記錄和 GridView 或 DetailsView 控制項](master-detail-filtering-with-two-dropdownlists-cs.md)顯示"詳細資料。 」 用於主要/詳細資料報表的另一個常見模式是同時擁有上一個網頁，並顯示在另一台的詳細資料的主要記錄。 論壇網站、 like [ASP.NET 論壇](https://forums.asp.net/)，是絕佳的範例，此模式實際上。 ASP.NET 論壇是組成各種不同的論壇開始使用 Web Form，資料呈現控制項，並以此類推。 每個論壇撰寫多執行緒，而且每個執行緒組成的文章的數字。 在 ASP.NET 論壇首頁上，會列出論壇。 在論壇上按一下 whisks 您`ShowForum.aspx`頁面上，列出該論壇的執行緒。 同樣地，按一下 上一個執行緒會帶您前往`ShowPost.aspx`，其中顯示之執行緒的已按下的文章。

本教學課程中我們將會列出供應商的資料庫使用 GridView 實作此模式。 在 GridView 中的每個供應商資料列會包含，按下時，會將使用者導向至個別頁面，列出這些產品選取供應商檢視產品連結。

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>步驟 1： 加入`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`頁面`Filtering`資料夾

我們在第三個教學課程中定義的頁面配置時加入 「 起始 」 中的頁面數目`BasicReporting`， `Filtering`，和`CustomFormatting`資料夾。 不過，我們並未新增起始頁面在此教學課程在該時間，因此請花一點時間加入至兩個新的頁面`Filtering`資料夾：`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`。 `SupplierListMaster.aspx` 會列出時的 「 主要 」 記錄 （供應商）`ProductsForSupplierDetails.aspx`將顯示所選的供應商的產品。

當建立這兩個新的分頁會使它們與特定`Site.master`主版頁面。


![加入篩選的資料夾中的 [SupplierListMaster.aspx 和 ProductsForSupplierDetails.aspx] 頁面](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**圖 1**： 新增`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`頁面`Filtering`資料夾


此外，將新頁面加入至專案，請務必更新站台對應檔案`Web.sitemap`分別。 在此教學課程只是加入`SupplierListMaster.aspx`頁面，即可使用下列 XML 內容，做為篩選的報表的子站台地圖`<siteMapNode>`項目：


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> 您可以協助自動更新站台對應檔案時加入新的 ASP.NET 頁面使用的程序[K.Scott Allen](http://odetocode.com/Blogs/scott/)的免費 Visual Studio[站台對應巨集](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)。


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>步驟 2： 顯示供應商清單中`SupplierListMaster.aspx`

與`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`建立頁面下, 一步是建立供應商的 GridView `SupplierListMaster.aspx`。 將 GridView 加入頁面和繫結到新 ObjectDataSource。 應該使用這個 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法來傳回所有供應商。


[![選取 SuppliersBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**圖 2**： 選取`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![設定為使用 GetSuppliers() 方法 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**圖 3**： 設定要使用 ObjectDataSource`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image7.png))


我們要包含的連結中標題為檢視產品每個 GridView 資料列，按一下時，將使用者帶至`ProductsForSupplierDetails.aspx`中選取的資料列傳遞`SupplierID`透過查詢字串值。 例如，如果使用者按一下東京 Traders 供應商檢視產品連結 (其具有`SupplierID`4 的值)，應傳送至`ProductsForSupplierDetails.aspx?SupplierID=4`。

若要達成此目的，將[HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx)至 GridView，這樣會將超連結加入每個 GridView 資料列。 按一下 編輯資料行中的連結 GridView 的智慧標籤來啟動。 接下來，HyperLinkField 從清單中選取左上方，然後按一下 [加入] 加入 HyperLinkField GridView 的欄位清單中。


[![在 GridView 中加入 HyperLinkField](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**圖 4**: HyperLinkField 加入 GridView ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image10.png))


HyperLinkField 可以設定為使用相同的文字或 URL 值中每個 GridView 資料列中，連結可以將這些值根據繫結至每個特定的資料列的資料值。 若要指定靜態值跨所有資料列使用 HyperLinkField`Text`或`NavigateUrl`屬性。 由於我們想要使用相同的所有資料列的連結文字時，將設定 HyperLinkField`Text`檢視產品的屬性。


[![將 HyperLinkField 文字屬性設定為檢視產品](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**圖 5**： 設定 HyperLinkField`Text`屬性來檢視產品 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image13.png))


若要設定的文字或 URL 是依據基礎資料繫結至 GridView 資料列的值，指定資料欄位文字或 URL 值應提取從`DataTextField`或`DataNavigateUrlFields`屬性。 `DataTextField` 只可以設定為單一資料欄位。`DataNavigateUrlFields`，不過，可以設定為以逗號分隔的資料欄位清單。 我們經常需要基底的文字或目前的資料列資料欄位值和某些 static 標記的組合上的 URL。 在本教學課程中，比方說，我們想要 HyperLinkField 連結的 URL `ProductsForSupplierDetails.aspx?SupplierID=supplierID`，其中*`supplierID`*是每個 GridView 資料列`SupplierID`值。 請注意，我們需要這兩個靜態資料導向此處值：`ProductsForSupplierDetails.aspx?SupplierID=`連結 URL 的部分是靜態的而*`supplierID`*部分是資料導向因為其值是每個資料列的自己`SupplierID`值。

若要表示的靜態和資料驅動的值組合，使用`DataTextFormatString`和`DataNavigateUrlFormatString`屬性。 這些內容中輸入所需的 static 標記，然後使用 標記`{0}`想中指定之欄位的值`DataTextField`或`DataNavigateUrlFields`出現的屬性。 如果`DataNavigateUrlFields`屬性有多個欄位指定的使用`{0}`其中您要插入的第一個欄位值`{1}`的第二個欄位的值，依此類推。

將此套用至我們的教學課程中，所以我們需要將`DataNavigateUrlFields`屬性`SupplierID`，因為這是資料欄位我們需要自訂針對每個資料列，其值和`DataNavigateUrlFormatString`屬性`ProductsForSupplierDetails.aspx?SupplierID={0}`。


[![設定要包含適當的連結 URL 根據 SupplierID HyperLinkField](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**圖 6**： 設定要包含適當連結 URL 基礎時 HyperLinkField `SupplierID` ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image16.png))


在新增之後 HyperLinkField，隨意自訂和 GridView 的欄位重新排列。 下列標記顯示 GridView 之後我進行的某些次要欄位層級自訂。


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

請花一點時間檢視`SupplierListMaster.aspx`透過瀏覽器的頁面。 如圖 7 所示，頁面目前會列出所有供應商包括檢視產品連結。 按一下 檢視產品連結將您帶到`ProductsForSupplierDetails.aspx`，並沿著供應商的傳遞`SupplierID`於 querystring 中。


[![每個供應商資料列都包含檢視產品連結](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**圖 7**： 每個供應商資料列都包含檢視產品連結 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>步驟 3： 列出中的供應商的產品`ProductsForSupplierDetails.aspx`

此時`SupplierListMaster.aspx`頁面傳送使用者`ProductsForSupplierDetails.aspx`，傳遞所選的供應商`SupplierID`於 querystring 中。 本教學課程的最後一個步驟是在 GridView 中顯示的產品`ProductsForSupplierDetails.aspx`其`SupplierID`等於`SupplierID`傳遞查詢字串。 若要將新增至的 GridView 來完成這個入門`ProductsForSupplierDetails.aspx`頁面上，使用新的 ObjectDataSource 控制項，名為`ProductsBySupplierDataSource`會叫用`GetProductsBySupplierID(supplierID)`方法從`ProductsBLL`類別。


[![加入名為 ProductsBySupplierDataSource 新 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**圖 8**： 加入新的 ObjectDataSource 名為`ProductsBySupplierDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![選取 ProductsBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**圖 9**： 選取`ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![已叫用 GetProductsBySupplierID(supplierID) 方法 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**圖 10**： 有 ObjectDataSource 叫用`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image28.png))


設定資料來源精靈的最後一個步驟會要求我們提供的來源`GetProductsBySupplierID(supplierID)`方法的*`supplierID`*參數。 若要使用的查詢字串值，設 QueryString 參數來源，然後輸入要用於 QueryStringField 文字方塊 querystring 值的名稱 (`SupplierID`)。


[![填入 supplierID SupplierID Querystring 值的參數值](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**圖 11**： 填入*`supplierID`*參數值從`SupplierID`Querystring 值 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image31.png))


這就是這麼簡單 ！ 圖 12 顯示`ProductsForSupplierDetails.aspx`頁面上，當從東京 Traders 連結，即可瀏覽`SupplierListMaster.aspx`。


[![東京 Traders 所提供的產品會顯示](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**圖 12**: 產品所提供的東京 Traders 會顯示 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>顯示中的供應商資訊`ProductsForSupplierDetails.aspx`

如圖 12 所示，`ProductsForSupplierDetails.aspx`頁面只會列出所提供的產品`SupplierID`於 querystring 中指定。 有人直接傳送至這個頁面上，不過，就不知道圖 12 顯示東京 Traders' 產品。 若要補救這個我們可以在這個頁面也顯示供應商資訊。

將上述產品 GridView FormView 來開始。 建立新的 ObjectDataSource 控制項，名為`SuppliersDataSource`會叫用`SuppliersBLL`類別的`GetSupplierBySupplierID(supplierID)`方法。


[![選取 SuppliersBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**圖 13**： 選取`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![已叫用 GetSupplierBySupplierID(supplierID) 方法 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**圖 14**： 有 ObjectDataSource 叫用`GetSupplierBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image40.png))


如同`ProductsBySupplierDataSource`，有*`supplierID`*參數的值指派給`SupplierID`querystring 值。


[![填入 supplierID SupplierID Querystring 值的參數值](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**圖 15**： 填入*`supplierID`*參數值從`SupplierID`Querystring 值 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image43.png))


當繫結 [設計] 檢視中 ObjectDataSource FormView，Visual Studio 會自動建立在 FormView 的`ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`與每個傳回的資料欄位的標籤和文字方塊中的 Web 控制項ObjectDataSource。 因為我們只是想要顯示供應商資訊逕行移除`InsertItemTemplate`和`EditItemTemplate`。 接下來，使其顯示中的供應商的公司名稱，編輯 ItemTemplate`<h3>`項目、 地址、 縣 （市）、 國家/地區，以及公司名稱下方的電話號碼。 或者，您可以手動設定在 FormView 的`DataSourceID`並建立`ItemTemplate`標記，如同回"[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)」 教學課程。

這些編輯之後在 FormView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

圖 16 顯示的螢幕擷取畫面`ProductsForSupplierDetails.aspx`頁面之後以上詳述的供應商資訊已包含在內。


[![產品的清單包含有關供應商的摘要](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**圖 16**： 產品的清單包含摘要的相關供應商 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>套用最終碰觸的`ProductsForSupplierDetails.aspx`UI

若要改善使用者體驗有這份報表都是幾個新增項目，我們應該要對`ProductsForSupplierDetails.aspx`頁面。 目前的使用者可以前往從的唯一方式`ProductsForSupplierDetails.aspx`頁面回傳至供應商的清單，是按一下其瀏覽器的上一頁按鈕。 讓我們將超連結控制項加入`ProductsForSupplierDetails.aspx`頁面連結回`SupplierListMaster.aspx`，提供使用者返回主清單的另一種方法。


[![將超連結控制項加入使用者拿到 SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**圖 17**： 將超連結控制項加入使用者回到採取`SupplierListMaster.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image49.png))


如果使用者在沒有任何產品，供應商的檢視產品連結`ProductsBySupplierDataSource`在 ObjectDataSource`ProductsForSupplierDetails.aspx`不會傳回任何結果。 繫結至 ObjectDataSource GridView 不會呈現任何使用者的瀏覽器中的頁面上的空白區域中所產生的標記。 若要更清楚地與使用者通訊有任何選取的供應商相關聯的產品中，我們可以設定 GridView 的`EmptyDataText`我們想要在這種情況時所顯示的訊息屬性。 我已設定此屬性為"There are 沒有該供應商所提供的產品"

根據預設，Northwinds 資料庫中的所有供應商提供至少一個產品。 不過，在此教學課程我手動修改了`Products`資料表，以便讓供應商 Escargots Nouveaux 已不再與任何產品相關聯。 圖 18 在進行這項變更之後，會顯示 Escargots Nouveaux 的詳細資料頁面。


[![使用者會接到通知供應商不會提供任何產品](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**圖 18**： 供應商不會提供任何產品通知使用者 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>總結

雖然主要/詳細資料報表可以在單一頁面上會顯示主要和詳細記錄，在許多網站會分隔跨兩個網頁。 在此教學課程中我們討論了如何實作主要/詳細資料報表需要 「 主要 」 的網頁中的 GridView 中列出供應商 和 詳細資料 頁面中所列的相關聯的產品。 主版網頁中的每個供應商資料列所包含的資料列所傳遞的詳細資料頁面的連結`SupplierID`值。 這類特定資料列的連結可以輕鬆地加入使用 GridView HyperLinkField。

藉由叫用已完成的詳細資料頁面擷取指定的供應商的那些產品`ProductsBLL`類別的`GetProductsBySupplierID(supplierID)`方法。 *`supplierID`*做為參數的來源使用查詢字串以宣告方式指定參數值。 我們也討論了如何使用程式設計的詳細資料頁面中顯示的供應商的詳細資料。

我們[下一個教學課程](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)是主要/詳細資料報表中的最後一個。 我們會探討如何在每個資料列之選取按鈕的 GridView 中顯示的產品清單。 按一下 [選取] 按鈕會在相同頁面上 DetailsView 控制項中顯示該產品的詳細資料。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-filtering-with-two-dropdownlists-cs.md)
> [下一頁](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
