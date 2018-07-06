---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: 主版/詳細篩選跨兩個頁面 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中我們將使用 GridView，若要列出的供應商的資料庫來實作此模式。 在 gridview 裡的每個供應商資料列會包含 Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 48102f64b1ec832774d9b41258503937dae4f4d5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373116"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>主版/詳細篩選跨兩個頁面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe)或[下載 PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> 在本教學課程中我們將使用 GridView，若要列出的供應商的資料庫來實作此模式。 在 gridview 裡的每個供應商資料列會包含檢視產品連結，當按下時，會將使用者帶到另一個頁面，列出這些產品的所選的供應商。


## <a name="introduction"></a>簡介

在先前的兩個教學課程，我們看到如何[顯示單一的 web 網頁使用 dropdownlist 進行主版/詳細報告](master-detail-filtering-with-a-dropdownlist-cs.md)要[顯示的 「 主要 」 的記錄和 GridView 或 DetailsView 控制項](master-detail-filtering-with-two-dropdownlists-cs.md)顯示"詳細資料。 」 用於主版/詳細資料報表的另一個常見模式是將上一個網頁並在另一個顯示的詳細資料的主要記錄。 論壇網站，例如[ASP.NET 論壇](https://forums.asp.net/)，是這種模式在實務上的絕佳範例。 ASP.NET 論壇會組成各種不同的論壇開始使用 Web Form 資料簡報控制項，依此類推。 每一個論壇由多個執行緒所組成，每個執行緒組成的文章數。 ASP.NET 論壇在首頁上，會列出論壇。 在論壇上按一下即可您`ShowForum.aspx`網頁，當中列出該論壇的執行緒。 同樣地，按一下 上一個執行緒會帶您前往`ShowPost.aspx`，其中顯示已按下之執行緒的文章。

在本教學課程中我們將使用 GridView，若要列出的供應商的資料庫來實作此模式。 在 gridview 裡的每個供應商資料列會包含檢視產品連結，當按下時，會將使用者帶到另一個頁面，列出這些產品的所選的供應商。

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>步驟 1： 新增`SupplierListMaster.aspx`並`ProductsForSupplierDetails.aspx`頁面`Filtering`資料夾

第三個教學課程中定義頁面配置時我們會新增 「 起始 」 中的分頁數目`BasicReporting`， `Filtering`，和`CustomFormatting`資料夾。 不過，我們不未加入本教學課程入門頁面，在該時間，因此請花一點時間加入兩個新的頁面，來`Filtering`資料夾：`SupplierListMaster.aspx`和`ProductsForSupplierDetails.aspx`。 `SupplierListMaster.aspx` 會列出時的 「 主要 」 記錄 （供應商）`ProductsForSupplierDetails.aspx`將選取的供應商顯示產品。

當建立這兩個新的頁面一定要將它們與關聯`Site.master`主版頁面。


![加入篩選的資料夾中的 [SupplierListMaster.aspx 和 ProductsForSupplierDetails.aspx] 頁面](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**圖 1**： 新增`SupplierListMaster.aspx`並`ProductsForSupplierDetails.aspx`頁面`Filtering`資料夾


此外，將新頁面加入至專案中，務必更新站台對應檔案`Web.sitemap`分別。 本教學課程只是新增`SupplierListMaster.aspx`頁面，即可使用下列 XML 內容篩選的報表之子節點的網站導覽`<siteMapNode>`項目：


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> 您可以將更新的站台對應檔案時加入新的 ASP.NET 頁面使用的程序自動化，協助[K.Scott Allen](http://odetocode.com/Blogs/scott/)的免費 Visual Studio[站台對應巨集](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)。


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>步驟 2： 顯示供應商清單中`SupplierListMaster.aspx`

具有`SupplierListMaster.aspx`並`ProductsForSupplierDetails.aspx`建立的頁面下, 一步是建立的供應商附於 GridView `SupplierListMaster.aspx`。 加入至頁面的 GridView 和繫結至新的 ObjectDataSource。 應該使用這個 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法，以傳回所有的供應商。


[![選取 SuppliersBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**圖 2**： 選取 `SuppliersBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![設定為使用 GetSuppliers() 方法的 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**圖 3**： 設定要使用 ObjectDataSource`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image7.png))


我們要包含的連結中標題為 View Products 每個 GridView 資料列，按一下時，將使用者帶到`ProductsForSupplierDetails.aspx`在選取的資料列中傳遞`SupplierID`透過查詢字串的值。 比方說，如果使用者按一下 [View Products] 連結，東京 Traders 供應商 (其具有`SupplierID`值為 4)，它們應該會傳送至`ProductsForSupplierDetails.aspx?SupplierID=4`。

若要達成此目的，新增[HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx)至 GridView，加入超連結至每個 GridView 資料列。 按一下 GridView 的智慧標籤中的 編輯資料行 連結啟動。 接下來，從左上角的清單中選取 HyperLinkField，並按一下 [新增]，在 GridView 的欄位清單中包含 HyperLinkField。


[![新增 HyperLinkField 至 GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**圖 4**： 加入 GridView HyperLinkField ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image10.png))


HyperLinkField 可以設定為使用相同的文字或 URL 值中每個 GridView 資料列的連結，或可以將這些值根據繫結至每個特定的資料列的資料值。 若要指定靜態值跨所有資料列使用 HyperLinkField`Text`或`NavigateUrl`屬性。 由於我們想要針對所有資料列相同的連結文字時，設定 HyperLinkField`Text`檢視產品的屬性。


[![HyperLinkField 的 Text 屬性設定為檢視產品](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**圖 5**： 設定 HyperLinkField`Text`屬性，以檢視產品 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image13.png))


若要設定的文字或 URL 是根據基礎資料繫結至 GridView 資料列的值，指定資料欄位的文字或 URL 值應從提取`DataTextField`或`DataNavigateUrlFields`屬性。 `DataTextField` 只可以設定為單一資料欄位;`DataNavigateUrlFields`，不過，可以設定以逗號分隔清單的資料欄位。 我們經常需要基底的文字或在目前資料列的資料欄位值和一些靜態標記的組合上的 URL。 在本教學課程中，比方說，我們想要成為 HyperLinkField 連結的 URL `ProductsForSupplierDetails.aspx?SupplierID=supplierID`，其中*`supplierID`* 是每個 GridView 資料列的`SupplierID`值。 請注意，我們需要這兩個靜態資料導向這裡值：`ProductsForSupplierDetails.aspx?SupplierID=`連結的 URL 部分是靜態的而*`supplierID`* 部分是資料驅動因為其值是每個資料列的自己`SupplierID`值。

若要表示的靜態和資料驅動的值組合，請使用`DataTextFormatString`和`DataNavigateUrlFormatString`屬性。 在這些屬性輸入所需的靜態標記，然後使用 標記`{0}`您想要在指定之欄位的值`DataTextField`或`DataNavigateUrlFields`出現的屬性。 如果`DataNavigateUrlFields`屬性有多個欄位的指定的用法`{0}`在您想要插入的第一個欄位值`{1}`的第二個欄位的值，依此類推。

將此套用到本教學課程中，我們需要設定`DataNavigateUrlFields`屬性設`SupplierID`，因為這是我們要自訂針對每個資料列，其值的 [資料] 欄位並`DataNavigateUrlFormatString`屬性設`ProductsForSupplierDetails.aspx?SupplierID={0}`。


[![設定包含適當的連結 URL 根據 SupplierID HyperLinkField](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**圖 6**： 設定適當連結 URL 架構時包含 HyperLinkField `SupplierID` ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image16.png))


在新增之後 HyperLinkField，放心地自訂和重新排列 GridView 的欄位。 下列標記顯示 GridView 之後我做了一些最少的欄位層級自訂。


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

請花一點時間檢視`SupplierListMaster.aspx`透過瀏覽器的頁面。 如 [圖 7] 所示，頁面目前列出所有包含檢視產品連結的供應商。 按一下 View Products 連結會帶您前往`ProductsForSupplierDetails.aspx`，並傳遞沿著供應商的`SupplierID`於 querystring 中。


[![每個供應商資料列包含檢視產品連結](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**圖 7**： 每個供應商資料列包含檢視產品連結 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>步驟 3： 列出供應商的產品`ProductsForSupplierDetails.aspx`

此時`SupplierListMaster.aspx`頁面會傳送使用者`ProductsForSupplierDetails.aspx`，將選取的供應商`SupplierID`於 querystring 中。 教學課程中的最後一個步驟是在 GridView 中顯示的產品`ProductsForSupplierDetails.aspx`其`SupplierID`等於`SupplierID`傳遞查詢字串。 新增 GridView，以完成本入門`ProductsForSupplierDetails.aspx`頁面上，使用名為的新 ObjectDataSource 控制項`ProductsBySupplierDataSource`叫用`GetProductsBySupplierID(supplierID)`方法從`ProductsBLL`類別。


[![新增名為 ProductsBySupplierDataSource 新 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**圖 8**： 新增新的 ObjectDataSource 名為`ProductsBySupplierDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![選取 ProductsBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**圖 9**： 選取 `ProductsBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![已叫用 GetProductsBySupplierID(supplierID) 方法的 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**圖 10**： 有 ObjectDataSource 叫用`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image28.png))


設定資料來源精靈的最後一個步驟會要求我們提供的來源`GetProductsBySupplierID(supplierID)`方法的*`supplierID`* 參數。 若要使用的查詢字串值，設定參數來源至查詢字串，然後輸入在 QueryStringField 文字方塊中使用查詢字串值的名稱 (`SupplierID`)。


[![填入 supplierID SupplierID Querystring 值的參數值](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**圖 11**： 填入*`supplierID`* 參數值，從`SupplierID`查詢字串值 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image31.png))


這樣就完成了 ！ [圖 12] 顯示`ProductsForSupplierDetails.aspx`頁面上，當從東京 Traders 連結，即可瀏覽`SupplierListMaster.aspx`。


[![會顯示東京 Traders 所提供的產品](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**圖 12**： 會顯示產品所提供的東京 Traders ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>顯示中的供應商資訊`ProductsForSupplierDetails.aspx`

如 [圖 12 所示`ProductsForSupplierDetails.aspx`] 頁面只會列出所提供的產品`SupplierID`於 querystring 中指定。 有人直接傳送到此頁面上，不過，就不知道，圖 12 顯示東京 Traders 產品。 若要解決這個問題我們可以在此頁面也顯示供應商資訊。

新增 GridView 的產品上面 FormView 開始。 建立新的 ObjectDataSource 控制項，名為`SuppliersDataSource`叫用`SuppliersBLL`類別的`GetSupplierBySupplierID(supplierID)`方法。


[![選取 SuppliersBLL 類別](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**圖 13**： 選取 `SuppliersBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![已叫用 GetSupplierBySupplierID(supplierID) 方法的 ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**圖 14**： 有 ObjectDataSource 叫用`GetSupplierBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image40.png))


如同`ProductsBySupplierDataSource`，具有*`supplierID`* 參數的值指派給`SupplierID`查詢字串值。


[![填入 supplierID SupplierID Querystring 值的參數值](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**圖 15**： 填入*`supplierID`* 參數值，從`SupplierID`查詢字串值 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image43.png))


當繫結 [設計] 檢視內的 ObjectDataSource FormView，Visual Studio 會自動建立 FormView `ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`與每個傳回的資料欄位的標籤和文字方塊 Web 控制項ObjectDataSource。 因為我們只想要顯示供應商資訊自由移除`InsertItemTemplate`和`EditItemTemplate`。 接著，編輯 ItemTemplate，使其顯示中的供應商的公司名稱`<h3>`項目和地址、 縣 （市）、 國家/地區、 和公司名稱下方的電話號碼。 或者，您可以手動設定 FormView 的`DataSourceID`並建立`ItemTemplate`標記，如同我們在上一步 」[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)」 教學課程。

這些編輯之後 FormView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

[圖 16] 顯示的螢幕擷取畫面`ProductsForSupplierDetails.aspx`頁面之後以上詳述的供應商資訊已包含在內。


[![產品的清單包含有關供應商摘要](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**圖 16**： 產品的清單包含摘要的相關供應商 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>套用最後的接觸`ProductsForSupplierDetails.aspx`UI

若要改善使用者體驗有這份報告是幾個新增項目，我們應該要對`ProductsForSupplierDetails.aspx`頁面。 目前的使用者即可從的唯一方式`ProductsForSupplierDetails.aspx`頁面回傳至供應商的清單，是按一下其瀏覽器的 [上一頁] 按鈕。 讓我們將超連結控制項加入`ProductsForSupplierDetails.aspx`網頁連結回`SupplierListMaster.aspx`，提供另一種方式，讓使用者返回主要清單。


[![加入超連結控制項以將使用者帶回 SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**圖 17**： 將超連結控制項以讓使用者返回至`SupplierListMaster.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image49.png))


如果使用者按一下 [View Products] 連結，沒有任何產品，供應商`ProductsBySupplierDataSource`內的 ObjectDataSource`ProductsForSupplierDetails.aspx`不會傳回任何結果。 繫結至 ObjectDataSource GridView 不會呈現任何使用者的瀏覽器頁面上的空白區域中所產生的標記。 若要更清楚地告知使用者有沒有選取的供應商相關聯的產品中，我們可以設定 GridView 的`EmptyDataText`我們想要在這種情況發生時所顯示的訊息的屬性。 我已設定此屬性為"There are 沒有此供應商所提供的產品 」

根據預設，Northwinds 資料庫中的所有供應商提供至少一項產品。 不過，本教學課程中有以手動方式修改`Products`資料表，以便讓供應商 Escargots Nouveaux 不再與任何產品相關聯。 圖 18.在進行這項變更之後，會顯示 Escargots Nouveaux 的詳細資料頁面。


[![使用者會收到通知供應商不提供任何產品](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**圖 18**： 使用者會收到通知供應商不提供任何產品 ([按一下以檢視完整大小的影像](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>總結

雖然主版/詳細報告可以顯示主要和詳細的記錄，在單一頁面上，在許多網站它們會被分到兩個網頁。 在本教學課程中我們探討了如何實作這類主版/詳細報告，「 主要 」 的網頁上的 GridView 中列出的供應商和 [詳細資料] 頁面中所列的相關聯的產品。 在主版頁面中的每個供應商資料列所包含的資料列一起傳遞的詳細資料頁面的連結`SupplierID`值。 這類特定資料列的連結可以輕易地新增使用 GridView 的 HyperLinkField。

藉由叫用已完成的詳細資料頁面擷取這些產品指定的供應商`ProductsBLL`類別的`GetProductsBySupplierID(supplierID)`方法。 *`supplierID`* 做為參數的來源使用查詢字串以宣告方式指定參數的值。 我們也會探討如何使用 FormView 的詳細資料頁面中顯示的供應商的詳細資料。

我們[下一個教學課程](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)是主版/詳細報告中的最後一個。 我們將探討如何在 GridView，其中每個資料列已選取 按鈕顯示的產品清單。 按一下 [選取] 按鈕，會在相同網頁的 DetailsView 控制項中顯示之產品的詳細資料。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-detail-filtering-with-two-dropdownlists-cs.md)
> [下一頁](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
