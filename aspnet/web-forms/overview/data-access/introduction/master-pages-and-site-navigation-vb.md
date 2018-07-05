---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: 主版頁面與網站導覽 (VB) |Microsoft Docs
author: rick-anderson
description: 方便使用的網站的一個共同的特點在於它們都擁有一致、 全站台的頁面配置和巡覽配置。 本教學課程會探討如何 y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: c823421a6b934951dd08656b48801c3d26247c5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372040"
---
<a name="master-pages-and-site-navigation-vb"></a>主版頁面與網站導覽 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe)或[下載 PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> 方便使用的網站的一個共同的特點在於它們都擁有一致、 全站台的頁面配置和巡覽配置。 本教學課程會探討如何建立一致的外觀及操作，可以輕鬆地更新的所有頁面。


## <a name="introduction"></a>簡介

方便使用的網站的一個共同的特點在於它們都擁有一致、 全站台的頁面配置和巡覽配置。 ASP.NET 2.0 引進兩項新功能可大幅簡化實作整個網站的頁面配置和巡覽配置： 主版頁面與網站瀏覽。 主版頁面可讓開發人員可以使用指定的可編輯區域建立全網站範本。 此範本接著可以套用至網站中的 ASP.NET 頁面。 主版頁面的指定可編輯區域，這類的 ASP.NET 頁面只需要提供內容的主版頁面中的所有其他標記等同使用主版頁面的所有 ASP.NET 頁面。 此模型可讓開發人員定義，並集中管理整個網站的頁面配置，藉此讓您更輕鬆地建立一致的外觀及操作，可以輕鬆地更新的所有頁面。

[網站導覽系統](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)提供一種機制，來定義站台對應的網頁開發人員並以程式設計方式查詢該網站對應的 API。 新的瀏覽 Web 控制項的功能表、 樹狀檢視中和 SiteMapPath 輕鬆地呈現全部或一部分網站導覽中常見的巡覽使用者介面項目。 我們將使用預設網站瀏覽提供者，這表示我們的網站對應將會定義 XML 格式檔案中。

若要說明這些概念，並讓我們的教學課程的網站更便於，讓我們花這一課定義整個網站的頁面配置、 實作站台對應，並將新增的巡覽 UI。 本教學課程結束時，我們必須建置我們的教學課程網頁的外觀精美的網站設計。


[![本教學課程的最終結果](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**圖 1**: 結束結果的本教學課程 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>步驟 1： 建立主版頁面

第一個步驟是建立網站的主版頁面。 現在我們的網站包含只將輸入資料集 (`Northwind.xsd`，請在`App_Code`資料夾)，BLL 類別 (`ProductsBLL.vb`， `CategoriesBLL.vb`，依此類推，所有`App_Code`資料夾)，資料庫 (`NORTHWND.MDF`中`App_Data`資料夾），組態檔 (`Web.config`)，和 CSS 樣式表檔案 (`Styles.css`)。 我清除這些頁面和 BLL 和 DAL 使用的前兩個教學課程，因為我們將會再次檢驗詳細說明這些範例在未來的教學課程所示範的檔案。


![我們的專案中的檔案](master-pages-and-site-navigation-vb/_static/image4.png)

**圖 2**： 我們的專案中的檔案


若要建立主版頁面，以滑鼠右鍵按一下 方案總管 中的專案名稱，並選擇 加入新項目。 然後從範本清單中選取主版頁面類型並將它命名`Site.master`。


[![將新的主版頁面新增至網站](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**圖 3**： 將新的主版頁面新增至網站 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image7.png))


主版頁面中定義的整個網站的頁面配置。 您可以使用 [設計] 檢視，並新增任何版面配置] 或 [Web 控制項，您需要或您可以手動在原始碼檢視中手動新增標記。 在我使用我的主版頁面中[階層式樣式表](http://www.w3schools.com/css/default.asp)用來定位和樣式的外部檔案中定義的 CSS 設定`Style.css`。 雖然您無法分辨從下方所顯示的標記，定義的 CSS 規則，瀏覽`<div>`的內容絕對位置，使其出現在左邊，並已在固定的寬度為 200 像素。

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

主版頁面定義靜態頁面配置和使用主版頁面的 ASP.NET 頁面都可以編輯的區域。 這些內容的可編輯區域會以您所見的內容中的 ContentPlaceHolder 控制項`<div>`。 我們的主版頁面具有單一 ContentPlaceHolder (`MainContent`)，但是主版頁面可能會有多個 ContentPlaceHolders。

使用上面輸入的標記，切換至 [設計] 檢視會顯示主版頁面的版面配置。 使用此主版頁面的任何 ASP.NET 頁面會有這個統一的版面配置，讓您指定的標記有`MainContent`區域。


[![主版頁面，檢視透過 [設計] 檢視時](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**圖 4**: 主版頁面中，當檢視透過 設計檢視 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>步驟 2： 加入至網站的首頁

定義主版頁面的情況下，我們準備好加入網站的 ASP.NET 頁面。 讓我們開始新增`Default.aspx`，我們的網站首頁。 在 方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇 加入新項目。 挑選檔案的名稱與範本 清單中的 Web Form 選項`Default.aspx`。 此外，選取 [選取主版頁面] 核取方塊。


[![加入新的 Web 表單，檢查選取的主版頁面的核取方塊](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**圖 5**： 加入新的 Web 表單，檢查選取的主版頁面的核取方塊 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image13.png))


按一下 [確定] 按鈕後，我們就必須選擇這個新的 ASP.NET 網頁應該使用何種主版頁面。 雖然您可以擁有多個主版頁面在您的專案中，我們有一個。


[![選擇應該使用這個 ASP.NET 網頁的主版頁面](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**圖 6**： 選擇此 ASP.NET 頁面應該使用的主版頁面 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image16.png))


挑選後的主版頁面，新的 ASP.NET 網頁會包含下列標記：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

在 `@Page`指示詞有會使用主版頁面檔案的參考 (`MasterPageFile="~/Site.master"`)，及 ASP.NET 網頁的標記包含每個 ContentPlaceHolder 控制項與控制項的主版頁面中定義的內容控制項`ContentPlaceHolderID`特定 ContentPlaceHolder 對應內容控制項。 內容控制項是您放置的標記您想要出現在對應的 ContentPlaceHolder。 設定`@Page`指示詞的`Title`屬性設定為首頁，並將一些親切的內容新增至內容控制項：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

`Title`屬性中`@Page`指示詞可讓我們設定頁面的標題，從 ASP.NET 頁面中，即使`<title>`項目會定義主版頁面中。 我們也可以設定標題以程式設計的方式，使用`Page.Title`。 也請注意，樣式表的主版頁面的參考 (例如`Style.css`) 會自動更新，讓它們能夠在任何 ASP.NET 頁面中，不論 ASP.NET 頁面是 相對於主版頁面的 在哪些目錄。

切換至 [設計] 檢視，我們可以看到我們的網頁瀏覽器中所呈現的樣子。 請注意，在設計檢視的 ASP.NET 頁面中，只有內容的可編輯區域都可以讓您編輯主版頁面中定義的非 ContentPlaceHolder 標記會呈現灰色。


[![ASP.NET 頁面的 [設計] 檢視會顯示可編輯和不可編輯的區域](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**圖 7**： 為 ASP.NET 頁面會顯示兩個可編輯 [設計] 檢視和非可編輯區域 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image19.png))


當`Default.aspx`瀏覽器瀏覽頁面、 ASP.NET 引擎自動合併頁面的主版頁面內容和 ASP。NET 的內容，並將合併的內容轉譯成最終會傳送給要求的瀏覽器的 HTML。 主版頁面的內容更新時，所有使用此主版頁面的 ASP.NET 頁面就會有與新的主版頁面內容 remerged 的下次要求其內容。 簡單地說，主版頁面模型可讓單一頁面的版面配置範本，可定義 (頁面 master page) 的變更會立即反映在整個網站。

## <a name="adding-additional-aspnet-pages-to-the-website"></a>將額外的 ASP.NET 頁面加入至網站

讓我們花一點時間加入其他 ASP.NET 頁面虛設常式到最後保存的各種報告示範網站。 會有多個非總計 35 示範因此而不是讓我們建立的所有虛設常式頁面只是建立第一個的幾個。 因為也會示範許多種，更妥善管理，示範會新增一個類別目錄的資料夾。 現在，新增下列三個資料夾：

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

最後，新增新的檔案，如 圖 8 中的 方案總管 中所示。 時新增的每個檔案，請務必選取 [選取主版頁面] 核取方塊。


![加入下列檔案](master-pages-and-site-navigation-vb/_static/image20.png)

**圖 8**： 新增下列檔案


## <a name="step-2-creating-a-site-map"></a>步驟 2： 建立站台對應

管理多個少數幾個頁面所組成的網站的挑戰之一提供一個簡單的方式，來瀏覽網站的訪客。 一開始，您必須定義網站的導覽結構。 接下來，此結構必須轉譯成可巡覽使用者介面項目，例如功能表或階層連結。 最後，這整個程序必須維護和更新為新的頁面會加入至網站並移除現有的。 ASP.NET 2.0 中，開發人員之前，為自己建立的網站巡覽結構，維護它，並將它轉譯成可巡覽使用者介面項目。 使用 ASP.NET 2.0，不過，開發人員可以利用非常大的彈性內建的網站導覽系統。

ASP.NET 2.0 的網站導覽系統會提供方法，讓開發人員定義站台對應，然後透過程式設計的 API 來存取這項資訊。 ASP.NET 隨附網站導覽提供者預期要在特定的方式進行格式化的 XML 檔案中儲存網站地圖資料。 但由於的網站導覽系統為基礎，所以[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)它可以擴充，以支援的替代方式來序列化網站地圖資訊。 Jeff Prosise 文章[SQL 站台對應提供者您 've Been Waiting For](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)示範如何建立網站導覽提供者會儲存在 SQL Server 資料庫站台對應，另一個選項是建立[網站導覽提供者根據檔案系統結構](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)。

本教學課程中，不過，我們會使用預設網站導覽提供者隨附具有 ASP.NET 2.0。 若要建立站台對應，只要以滑鼠右鍵按一下 方案總管 中的專案名稱、 選擇 加入新項目，並選擇站台對應選項。 保留的名稱為`Web.sitemap`，按一下 [新增] 按鈕。


[![新增站台對應到您的專案](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**圖 9**： 將站台地圖新增至您的專案 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image23.png))


站台對應檔案是 XML 檔案。 請注意，Visual Studio 會提供 IntelliSense 的網站地圖結構。 站台對應檔案必須具有`<siteMap>`節點作為其根節點，必須包含一個精確`<siteMapNode>`子項目。 先`<siteMapNode>`項目則可以包含任意數目的子系`<siteMapNode>`項目。

定義站台對應，來模擬檔案系統結構。 亦即，加入`<siteMapNode>`三個資料夾和子系的每個項目`<siteMapNode>`項目，每個 ASP.NET 網頁，在這些資料夾中，就像這樣：

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

站台對應定義網站的導覽結構，也就是描述站台的不同區段的階層。 每個`<siteMapNode>`中的項目`Web.sitemap`表示網站巡覽結構中的區段。


[![站台對應都代表階層式導覽結構](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**圖 10**: 站台對應都代表階層式導覽結構 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET 會公開.NET Framework 的透過網站導覽的結構[SiteMap 類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)。 這個類別具有`CurrentNode`屬性，會傳回使用者目前正在造訪; 區段的詳細資訊`RootNode`屬性會傳回網站地圖的根 （家中，在我們的網站對應）。 同時`CurrentNode`並`RootNode`屬性傳回[SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)執行個體，具有屬性等`ParentNode`， `ChildNodes`， `NextSibling`， `PreviousSibling`，依此類推，可讓站台對應若要逐步的階層。

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>步驟 3： 顯示功能表，根據站台對應

在 ASP.NET 2.0 中存取資料可以是以程式設計方式完成，如同在 ASP.NET 1.x 中，或以宣告方式，透過新[資料來源控制項](https://msdn.microsoft.com/library/ms227679.aspx)。 有數個內建的資料來源控制項，例如 SqlDataSource 控制項，用於存取關聯式資料庫資料、 ObjectDataSource 控制項，從類別和其他人存取資料。 您甚至可以建立您自己[自訂資料來源控制項](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)。

資料來源控制項做為您的 ASP.NET 網頁和基礎資料之間的 proxy。 若要顯示的資料來源控制項擷取的資料，我們通常會加入至頁面的另一個 Web 控制項和繫結至資料來源控制項。 若要將 Web 控制項繫結至資料來源控制項，只要設定 Web 控制項的`DataSourceID`屬性設為值的資料來源控制項的`ID`屬性。

為了協助使用站台對應的資料，ASP.NET 會包含 SiteMapDataSource 控制項，可讓我們將針對我們的網站的站台地圖的 Web 控制項繫結。 兩個 Web 控制項的 TreeView 和 Menu 常用來提供巡覽使用者介面中。 若要將網站導覽資料繫結至這兩個控制項的其中一個，只要新增至 TreeView 以及頁面的 SiteMapDataSource 或功能表控制項`DataSourceID`會據以設定屬性。 比方說，我們可以將功能表控制項加入主版頁面使用下列標記：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

更精細的程度的控制發出的 HTML，我們可以將控制項繫結 SiteMapDataSource 至 Repeater 控制項，就像這樣：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

SiteMapDataSource 控制項傳回的站台對應一個階層層級一次從根網站導覽節點 （家中，在我們的網站對應），然後下一個層級 （基本報告、 篩選報表和自訂格式設定），依此類推。 當繫結，SiteMapDataSource 設有重複項，它會列舉第一個層級傳回並具現化`ItemTemplate`針對每個`SiteMapNode`第一個層級的執行個體。 若要存取特定的屬性`SiteMapNode`，我們可以使用`Eval(propertyName)`，這是我們要如何取得每個`SiteMapNode`的`Url`和`Title`超連結控制項的屬性。

Repeater 上述範例中會轉譯下列標記：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

這些網站導覽節點 （基本報告、 篩選報表，與自訂格式化） 組成*第二個*所呈現並非第一個站台對應的層級。 這是因為，SiteMapDataSource`ShowStartingNode`屬性設為 False，導致略過根網站導覽節點，並改為藉由傳回站台對應階層中的第二個層級開始，SiteMapDataSource。

針對基本報告、 篩選報表，以及自訂格式化顯示的子系`SiteMapNode`s，我們可以加入初始的重複項中的另一個 Repeater `ItemTemplate`。 將繫結至這個第二個 Repeater`SiteMapNode`執行個體的`ChildNodes`屬性，就像這樣：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

在下列標記中 （為了簡潔起見已移除某個標記） 會產生這些兩個重複項：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

使用 CSS 樣式所選從[Rachel Andrew](http://www.rachelandrew.co.uk/)的書籍[CSS Anthology: 101 不可或缺的秘訣，秘訣&amp;Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance)，則`<ul>`和`<li>`元素的樣式設定，標記會產生下列視覺化輸出：


![由兩個重複項及一些 CSS 功能表](master-pages-and-site-navigation-vb/_static/image27.png)

**圖 11**： 由兩個重複項及一些 CSS 所組成的功能表


這個功能表中的主版頁面，並繫結至網站導覽中定義`Web.sitemap`，這表示網站導覽的任何變更就會立即反映在所有頁面使用`Site.master`主版頁面。

## <a name="disabling-viewstate"></a>停用檢視狀態

所有的 ASP.NET 控制項可以選擇性地將其狀態保存[檢視狀態](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)，這會序列化為隱藏的表單欄位中轉譯的 HTML。 檢視狀態使用控制項来記住它們以程式設計方式變更的狀態之間的回傳中，例如資料繫結至資料 Web 控制項。 雖然檢視狀態允許在回傳之間能記住的資訊時，它會增加必須傳送至用戶端，可能會導致嚴重的頁面膨脹如果未嚴密的監控的標記的大小。 資料 Web 控制項特別 GridView 會特別糟的情況將數十個額外的 kb 為單位的標記加入至網頁。 雖然此類增加可能是微不足道的寬頻或內部網路的使用者，檢視狀態可以加入撥接使用者往返的幾秒鐘的時間。

若要查看的影響，檢視狀態、 瀏覽的網頁瀏覽器中，然後檢視網頁所傳送的來源 （在 Internet Explorer 中，移至 [檢視] 功能表然後選擇 [來源] 選項）。 您也可以開啟[網頁追蹤](https://msdn.microsoft.com/library/sfbfw58f.aspx)以查看每個頁面上的控制項使用的檢視狀態配置。 檢視狀態資訊會以名為隱藏的表單欄位序列化`__VIEWSTATE`，位於`<div>`項目開頭之後立即`<form>`標記。 正在使用 Web 表單時，只會保存檢視狀態如果您的 ASP.NET 網頁不包含`<form runat="server">`不會有宣告式語法中`__VIEWSTATE`隱藏的表單欄位中呈現的標記。

`__VIEWSTATE`主版頁面所產生的表單欄位會加入頁面產生的標記中的大約 1,800 個位元組。 這個額外的膨脹主要是因為 Repeater 控制項、 SiteMapDataSource 控制項的內容會保存至檢視狀態。 雖然什麼時有許多欄位與記錄使用 GridView 令人雀躍，似乎不到額外的 1,800 個位元組，檢視狀態可以輕鬆地 swell 10 或更多倍。

檢視狀態可以在停用的頁面或控制項的層級設定`EnableViewState`屬性設`False`，藉此減少呈現的標記的大小。 資料 Web 控制項將資料保存於在回傳之間的繫結至資料 Web 控制項的檢視狀態，因為當停用檢視狀態資料 Web 控制項的資料必須繫結在每個回傳。 在 ASP.NET 版本 1.x 這個責任是乏人在頁面開發人員; 的肩膀利用 ASP.NET 2.0，不過，資料 Web 控制項將會重新繫結至其在每次回傳的資料來源控制項如有需要。

若要減少頁面的檢視狀態，讓我們將設定 Repeater 控制項的`EnableViewState`屬性設`False`。 這可以透過設計工具中，或以宣告方式在原始碼檢視中的 [屬性] 視窗來完成。 進行這項變更後重複項的宣告式標記應該看起來像：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

這項變更，頁面的轉譯後的檢視狀態大小已壓縮成只有之後 52 個位元組，省下更是 97%會在檢視狀態大小 ！ 在此系列教學課程中我們將停用檢視狀態資料 Web 控制項的預設以減少呈現的標記的大小。 在大部分的範例`EnableViewState`屬性會設定為`False`和這麼做不值得一提。 狀態將會討論的檢視資料，必須已啟用的案例中是唯一的時候 Web 控制權傳輸至提供其預期的功能。

## <a name="step-4-adding-breadcrumb-navigation"></a>步驟 4： 加入階層連結巡覽

若要完成的主版頁面，讓我們新增至每個頁面的階層連結巡覽 UI 項目。 階層連結可快速地顯示使用者站台階層中其目前的位置。 新增 ASP.NET 2.0 中的階層連結只簡單 SiteMapPath 控制項加入頁面;不需要任何程式碼。

針對我們的網站，將這個控制項標頭`<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

階層連結會顯示目前頁面一直到根使用者造訪網站對應階層架構，以及該站台網站導覽節點的 「 上階，"（家中，在我們的網站對應）。


![階層連結會顯示目前頁面和網站中的其祖系對應階層架構](master-pages-and-site-navigation-vb/_static/image28.png)

**圖 12**： 軌跡會顯示目前頁面和網站中的其祖系對應階層架構


## <a name="step-5-adding-the-default-page-for-each-section"></a>步驟 5： 加入每個區段的預設頁面

我們的網站中的教學課程會細分成不同的類別基本報表功能，篩選、 自訂設定格式與針對每個類別目錄和對應的教學課程，為該資料夾中的 ASP.NET 網頁的資料夾，依此類推。 此外，每個資料夾包含`Default.aspx`頁面。 此預設頁面中，我們顯示所有目前這一節的教學課程。 也就是針對`Default.aspx`中`BasicReporting`資料夾，我們會有連結`SimpleDisplay.aspx`， `DeclarativeParams.aspx`，和`ProgrammaticParams.aspx`。 在這裡，同樣地，我們可以使用`SiteMap`中定義的類別和資料 Web 控制項來顯示這項資訊根據網站地圖`Web.sitemap`。

讓我們來顯示一次，但這次我們會顯示標題和描述的教學課程使用 Repeater 的未排序的清單。 因為標記和程式碼來完成這將會需要針對每個重複`Default.aspx` 頁面上，我們可以封裝在此 UI 邏輯[使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)。 在呼叫網站上建立資料夾`UserControls`並將類型名為 Web 使用者控制項的新項目加入至該`SectionLevelTutorialListing.ascx`，並新增下列標記：


[![將新的 Web 使用者控制項新增至 Usercontrol 資料夾](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**圖 13**： 加入新的 Web 使用者控制項來`UserControls`資料夾 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

在上述範例中，重複項我們繫結`SiteMap`Repeater 的資料以宣告方式;`SectionLevelTutorialListing`使用者控制項，不過，這麼做以程式設計的方式。 在 `Page_Load`事件處理常式，以確保此頁面 s URL 對應至站台模式中的節點，會進行檢查。 如果沒有任何對應的頁面中使用這個使用者控制項`<siteMapNode>`項目，`SiteMap.CurrentNode`會傳回`Nothing`和任何資料將會繫結至 Repeater。 假設我們有`CurrentNode`，我們會繫結其`ChildNodes`Repeater 的集合。 因為我們的網站對應設定使`Default.aspx`頁面每個區段中的所有教學課程，該區段中的父節點，此程式碼會顯示一節的教學課程中，所有的描述和連結，如以下螢幕擷取畫面所示。

一旦建立這個 Repeater，開啟`Default.aspx`頁面中的每個資料夾，請移至 [設計] 檢視中，並只將使用者控制項拖曳到設計介面上的 [方案總管] 從想要顯示的教學課程清單。


[![使用者控制項可讓您有已加入至 Default.aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**圖 14**: 使用者控制項可讓您有已加入至`Default.aspx`([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image34.png))


[![基本報表教學課程將列](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**圖 15**： 報告的基本教學課程將列 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>總結

在定義網站地圖和完成主版頁面中，我們現在有一致的頁面配置和巡覽配置為我們資料相關的教學課程。 不論我們加入我們的網站多少頁面，更新整個網站的頁面配置或網站瀏覽資訊是快速而簡單的程序，因為這項資訊在集中式。 具體來說，主版頁面中定義的頁面配置資訊`Site.master`對應中的站台和`Web.sitemap`。 我們並不需要撰寫*任何*程式碼，以達到這整個網站的頁面配置和巡覽機制，以及我們會保留完整的 WYSIWYG 設計工具的支援，在 Visual Studio 中。

資料存取層和商務邏輯層完成，而且有定義的一致性頁面配置和站台導覽，我們準備好要開始探索常見的報告模式。 在接下來三個教學課程中我們將探討基本的報告工作，顯示從 BLL GridView、 DetailsView 和 FormView 控制項中擷取的資料。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 主版頁面概觀](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [在 ASP.NET 2.0 主版頁面](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 的設計範本](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET 網站巡覽概觀](https://msdn.microsoft.com/library/e468hxky.aspx)
- [檢查 ASP.NET 2.0 的網站導覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 網站瀏覽功能](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [了解 ASP.NET 檢視狀態](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [如何： 啟用 ASP.NET 網頁的追蹤](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET 使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Liz Shulok、 Dennis Patterson 和 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](creating-a-business-logic-layer-vb.md)
