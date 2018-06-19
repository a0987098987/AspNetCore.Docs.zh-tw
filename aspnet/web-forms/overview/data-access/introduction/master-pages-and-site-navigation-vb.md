---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: 主版頁面和站台瀏覽 (VB) |Microsoft 文件
author: rick-anderson
description: 使用者易記的一個共同的特性是網站的他們有一致且全站台的頁面配置和導覽配置。 本教學課程會查看如何 y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 45fdbf70f7981c0faefef2603d21f913022e2a8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887557"
---
<a name="master-pages-and-site-navigation-vb"></a>主版頁面和站台瀏覽 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe)或[下載 PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> 使用者易記的一個共同的特性是網站的他們有一致且全站台的頁面配置和導覽配置。 本教學課程會查看如何建立一致的外觀及操作，可以輕鬆地更新的所有頁面。


## <a name="introduction"></a>簡介

使用者易記的一個共同的特性是網站的他們有一致且全站台的頁面配置和導覽配置。 ASP.NET 2.0 導入了兩個新的功能，大幅簡化實作全站台的頁面配置和巡覽配置： 主版頁面與網站瀏覽。 主版頁面可讓開發人員使用指定的可編輯區域建立全站台的範本。 然後可以套用這個範本加入 ASP.NET 網頁站台中。 主版頁面的指定可編輯區域，這類的 ASP.NET 網頁只需要提供內容使用主版頁面的所有 ASP.NET 頁面都是相同的主版頁面中的所有其他標記。 此模型可讓開發人員定義並集中管理整個網站頁面版面配置，藉此讓您更輕鬆地建立一致的外觀及操作，可以輕鬆地更新的所有頁面。

[站台瀏覽系統](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)提供兩者的機制，來定義站台對應的網頁開發人員以及要以程式設計方式查詢該站台對應的 API。 新的瀏覽 Web 控制項功能表、 樹狀檢視，以及 SiteMapPath 讓您輕鬆轉譯全部或一部分網站導覽中常見的巡覽使用者介面項目。 我們將使用預設的網站瀏覽提供者，這表示我們的站台對應，將在 XML 格式檔案中定義。

說明這些概念，讓我們教學課程的網站更為有用，讓我們花在這一課定義全站台的頁面配置，實作站台對應，並將新增的導覽 UI。 本教學課程結束時，我們必須建置我們教學課程的 web 網頁的外觀精美的網站設計。


[![本教學課程的最終結果](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**圖 1**: 結束結果的此教學課程 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>步驟 1： 建立主版頁面

第一個步驟是建立網站的主版頁面。 現在我們的網站包含只輸入資料集，以滑鼠右鍵 (`Northwind.xsd`，請在`App_Code`資料夾)，BLL 類別 (`ProductsBLL.vb`， `CategoriesBLL.vb`，依此類推，所有的`App_Code`資料夾)，資料庫 (`NORTHWND.MDF`，請在`App_Data`資料夾） 的組態檔 (`Web.config`)，和 CSS 樣式表檔案 (`Styles.css`)。 我清除的頁面而示範 DAL 和 BLL 使用前兩個教學課程中，因為我們將會再次檢驗這些範例中會更詳細地在未來教學課程中的檔案。


![我們的受測專案檔案](master-pages-and-site-navigation-vb/_static/image4.png)

**圖 2**： 我們的受測專案的檔案


若要建立主版頁面，以滑鼠右鍵按一下 方案總管 中的專案名稱，並選擇 加入新項目。 然後從範本清單中選取主版頁面類型並將其命名`Site.master`。


[![將新的主版頁面新增至網站](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**圖 3**： 將新的主版頁面新增至網站 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image7.png))


主版頁面中定義的全站台的頁面配置。 您可以使用 [設計] 檢視，並將在需要時，任何配置或 Web 控制項，或您可以手動在來源檢視中手動新增標記。 我使用我的主版頁面中[階層式樣式表](http://www.w3schools.com/css/default.asp)用來定位和外部檔案中定義的 CSS 設定樣式`Style.css`。 雖然您無法分辨從標記如下所示，定義的 CSS 規則，瀏覽`<div>`的內容絕對置放出現在左邊，使其具有固定的寬度為 200 像素。

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

主版頁面定義靜態頁面配置和使用主版頁面的 ASP.NET 頁面可編輯的區域。 這些內容可編輯區域以 ContentPlaceHolder 控制項內容中，可以看到`<div>`。 我們的主版頁面都有單一 ContentPlaceHolder (`MainContent`)，但是主版頁面可能會有多個 ContentPlaceHolders。

上面輸入的標記，切換至 [設計] 檢視會顯示主版頁面的版面配置。 任何使用此主版頁面的 ASP.NET 網頁有這個統一的配置，讓您指定的標記與`MainContent`區域。


[![主版頁面，檢視透過 [設計] 檢視時](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**圖 4**: 主版頁面中，當檢視透過 設計檢視 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>步驟 2： 加入至網站的首頁

定義的主版頁面，我們就可以新增網站的 ASP.NET 頁面。 讓我們開始加`Default.aspx`，我們的網站的首頁。 在 方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇 加入新項目。 挑選檔案的名稱與範本] 清單中的 [Web Form 選項`Default.aspx`。 另請檢查 「 選取的主版頁面 」 核取方塊。


[![加入新的 Web 表單，檢查選取的主版頁面的核取方塊](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**圖 5**： 加入新的 Web 表單，檢查選取的主版頁面的核取方塊 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image13.png))


按一下 [確定] 按鈕後，我們就必須選擇這個新的 ASP.NET 網頁應該使用何種主版頁面。 雖然您可以在專案中有多個主版頁面，我們只能有一個。


[![選擇此 ASP.NET 網頁應該使用主版頁面](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**圖 6**： 選擇此 ASP.NET 網頁應該使用主版頁面 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image16.png))


之後挑選主版頁面，新的 ASP.NET 網頁將會包含下列標記：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

在`@Page`有指示詞時使用的主版頁面檔案的參考 (`MasterPageFile="~/Site.master"`)，而 ASP.NET 網頁的標記包含每個定義在主版頁面中，使用控制項的 ContentPlaceHolder 控制項的內容控制項`ContentPlaceHolderID`對應的內容控制項加入特定的 ContentPlaceHolder。 內容控制項是您放置標記您想要出現在對應 ContentPlaceHolder。 設定`@Page`指示詞的`Title`首頁屬性，並加入一些歡迎內容至內容控制項：

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

`Title`屬性`@Page`指示詞可讓我們設定頁面的標題，從 ASP.NET 頁面中，即使`<title>`主版頁面定義元素。 我們也可以設定標題以程式設計的方式，使用`Page.Title`。 也請注意，樣式表的主版頁面的參考 (例如`Style.css`) 會自動更新，讓它們能夠在任何 ASP.NET 網頁，不論 ASP.NET 頁面位於主版頁面的相對目錄中。

切換至設計檢視，我們可以看到我們的網頁瀏覽器中所呈現的樣子。 請注意，在設計檢視只有內容的可編輯區域是可編輯的 ASP.NET 網頁的主版頁面中定義的非 ContentPlaceHolder 標記會變成灰色。


[![ASP.NET 網頁的 [設計] 檢視會顯示可編輯與不可編輯的區域](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**圖 7**： 針對 ASP.NET 頁面會顯示同時編輯 [設計] 檢視和非編輯區域 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image19.png))


當`Default.aspx`瀏覽器瀏覽頁面時，ASP.NET 引擎自動合併頁面的主版頁面內容和 ASP。網路的內容，並將合併的內容轉譯成最終會向下傳送給要求瀏覽器的 HTML。 當主版頁面的內容更新時，所有使用此主版頁面的 ASP.NET 頁面會有與新的主版頁面內容 remerged 下一次要求其內容。 簡單地說，主版頁面模型可讓單一頁面的版面配置範本，可定義 (頁面 master page) 的變更會立即反映在整個網站。

## <a name="adding-additional-aspnet-pages-to-the-website"></a>網站中加入其他 ASP.NET 網頁

讓我們花點時間到最終會保留各種報告示範站台加入其他 ASP.NET 頁面虛設常式。 會有多個不總共 35 示範因此而不是我們建立的所有虛設常式頁面只建立第一個部分。 也會有許多類別的示範，因為若要管理更容易示範會加入類別目錄的資料夾。 現在，加入下列三個資料夾：

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

最後，將新檔案，如圖 8 中的 [方案總管] 中所示。 當加入每個檔案，請記得選取 「 選取的主版頁面 」 核取方塊。


![加入下列的檔案](master-pages-and-site-navigation-vb/_static/image20.png)

**圖 8**： 加入下列的檔案


## <a name="step-2-creating-a-site-map"></a>步驟 2： 建立站台地圖

管理網站超過少數幾個頁面所組成的挑戰之一就提供簡單的方式讓瀏覽網站的訪客。 一開始您必須定義站台的瀏覽結構。 接下來，此結構必須轉譯成可瀏覽的使用者介面項目，例如功能表或階層連結列。 最後，這整個程序需要維護以及當新的頁面會加入至網站並移除現有的更新。 在 ASP.NET 2.0 之前開發人員已為自己建立站台的瀏覽結構，維護它，並將它轉譯成可瀏覽的使用者介面項目。 透過 ASP.NET 2.0，不過，開發人員可以利用非常大的彈性內建的站台瀏覽系統。

ASP.NET 2.0 網站巡覽系統會提供方法，讓開發人員定義站台對應，然後透過程式設計的 API 存取這項資訊。 ASP.NET 隨附於站台對應提供者預期網站導覽資料儲存在 XML 檔案以特定方式格式化。 但是，因為站台瀏覽系統已內建[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)其可以擴充以支援序列化此站台對應資訊的替代方式。 Jeff Prosise 文章[SQL 站台對應提供者您 've 已等待](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)示範如何建立站台對應提供者會儲存在 SQL Server 資料庫網站導覽; 另一個選項是建立[站台對應提供者為基礎檔案系統結構](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)。

此教學課程中，不過，我們使用隨附的預設站台對應提供者與 ASP.NET 2.0。 若要建立站台對應，只要以滑鼠右鍵按一下 方案總管 中的專案名稱、 選擇 加入新項目，然後選擇網站地圖選項。 保留相同的名稱。 `Web.sitemap` ，按一下 [新增] 按鈕。


[![新增站台對應到您的專案](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**圖 9**： 新增站台對應到您的專案 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image23.png))


站台對應檔案是 XML 檔案。 請注意，Visual Studio 會提供 IntelliSense 網站導覽結構。 站台對應檔案必須具有`<siteMap>`節點作為其根節點，必須包含一個精確`<siteMapNode>`子項目。 該第一個`<siteMapNode>`項目可以再包含任意數目的子系`<siteMapNode>`項目。

定義要模擬的檔案系統結構的站台對應。 亦即，加入`<siteMapNode>`每三個資料夾和子系的項目`<siteMapNode>`項目，每個 ASP.NET 網頁，在這些資料夾中，就像這樣：

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

站台對應定義網站的導覽結構，也就是描述的不同區段的站台的階層。 每個`<siteMapNode>`中的項目`Web.sitemap`代表站台的巡覽式結構中的區段。


[![站台地圖代表階層式導覽結構](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**圖 10**： 網站導覽代表階層式導覽結構 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET 公開透過.NET Framework 的網站地圖結構[SiteMap 類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)。 這個類別具有`CurrentNode`屬性，它會傳回使用者目前正在造訪; 區段的詳細資訊`RootNode`屬性會傳回站台對應的根目錄 （家中，我們的站台對應中）。 同時`CurrentNode`和`RootNode`屬性傳回[SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)執行個體，且有屬性類似`ParentNode`， `ChildNodes`， `NextSibling`， `PreviousSibling`，依此類推，可讓站台地圖若要逐步階層。

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>步驟 3： 顯示功能表，根據站台對應

在 ASP.NET 2.0 中存取資料可以是以程式設計方式來達成，如同在 ASP.NET 1.x，或以宣告方式，透過新[資料來源控制項](https://msdn.microsoft.com/library/ms227679.aspx)。 有數個內建資料來源控制項，例如 SqlDataSource 控制項，用於存取關聯式資料庫資料、 ObjectDataSource 控制項，從類別和其他人存取資料。 您甚至可以建立您自己[自訂資料來源控制項](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)。

資料來源控制項做為您的 ASP.NET 頁面與基礎資料之間的 proxy。 為了顯示資料來源控制項的擷取的資料，我們通常會將另一個 Web 控制項加入頁面和繫結到資料來源控制項。 若要將 Web 控制項繫結至資料來源控制項，只要設定 Web 控制項`DataSourceID`屬性設為值的資料來源控制項的`ID`屬性。

為協助使用站台對應的資料，ASP.NET 會包含 Treeview 控制項，可讓我們對我們的網站的站台地圖 Web 控制項繫結。 兩個 Web 控制項的樹狀檢視和功能表通常用於提供巡覽使用者介面。 若要將網站導覽資料繫結這兩個控制項的其中一個，只要將加入 TreeView 以及頁面的 Treeview 或功能表控制項`DataSourceID`屬性會隨之設定。 例如，我們可以使用下列標記的主版頁面加入功能表控制項：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

針對更充分掌控發出的 HTML，我們可以將控制項繫結 Treeview 中繼器控制項中，就像這樣：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

Treeview 控制項傳回的站台對應一個階層層級一次，開頭為根網站對應節點 （組織，在我們的站台對應），然後下一個層級 （基本報表功能、 篩選報表和自訂格式），依此類推。 當繫結 Treeview 設有重複項，會列舉第一個層級傳回並具現化`ItemTemplate`每個`SiteMapNode`第一個層級的執行個體。 若要存取的特定屬性`SiteMapNode`，我們可以使用`Eval(propertyName)`，這是我們要如何取得每個`SiteMapNode`的`Url`和`Title`超連結控制項的屬性。

中繼器上述範例中會呈現下列標記：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

這些站台對應節點 （基本報表功能、 篩選報表，以及自訂格式化） 組成*第二個*呈現，並非第一個站台對應的層級。 這是因為 Treeview`ShowStartingNode`屬性設為 False，導致略過根網站對應節點，並改為從開始傳回站台對應階層中的第二個層級 Treeview。

若要顯示基本報表功能、 篩選報表和自訂格式的子系`SiteMapNode`s，我們可以將另一個中繼器加入初始的中繼器`ItemTemplate`。 將繫結至這個第二個中繼器`SiteMapNode`執行個體的`ChildNodes`屬性，就像這樣：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

這些兩個重複項會導致下列標記 （為畫面簡潔起見，某些標記有已移除）：


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

使用 CSS 樣式所選從[Rachel Andrew](http://www.rachelandrew.co.uk/)的書籍[CSS Anthology: 101 重要提示、 秘訣&amp;缺失](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance)、`<ul>`和`<li>`項目已有的樣式，標記會產生下列 visual 輸出：


![由兩個中繼器和某些 CSS 功能表](master-pages-and-site-navigation-vb/_static/image27.png)

**圖 11**： 功能表由兩個中繼器和某些 CSS


這個功能表中的主版頁面和繫結至網站導覽中定義`Web.sitemap`，這表示任何站台對應的變更就會立即反映在所有頁面，使用`Site.master`主版頁面。

## <a name="disabling-viewstate"></a>停用檢視狀態

所有 ASP.NET 控制項可以選擇性地都保存其狀態[檢視狀態](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)，這會序列化為隱藏的表單欄位中呈現的 HTML。 檢視狀態是控制項用來記住它們以程式設計方式變更的狀態，在回傳，例如資料繫結至資料的 Web 控制項。 雖然檢視狀態允許在回傳記住的資訊，它會增加必須傳送至用戶端可能會導致嚴重的頁面膨脹不可和密切監視的標記的大小。 Web 控制項的資料特別 GridView 會特別昭彰數十個額外的 kb 數標記加入頁面的。 雖然這種增加可能是微不足道寬頻或內部網路使用者，檢視狀態可以新增撥號使用者，來回行程期間幾秒鐘的時間。

若要查看的影響，檢視狀態、 造訪的網頁瀏覽器中，然後檢視網頁所傳送的來源 （在 Internet Explorer 中，移至 [檢視] 功能表並選擇來源選項）。 您也可以開啟[網頁追蹤](https://msdn.microsoft.com/library/sfbfw58f.aspx)以查看每個頁面上的控制項使用的檢視狀態配置。 檢視狀態資訊會以名為隱藏的表單欄位序列化`__VIEWSTATE`，位於`<div>`緊接之後開啟項目`<form>`標記。 正在使用 Web 表單時，只保存檢視狀態如果您的 ASP.NET 頁面不包括`<form runat="server">`宣告式語法中將不會在`__VIEWSTATE`呈現標記中的隱藏的欄位。

`__VIEWSTATE`主版頁面所產生的表單欄位將大致 1800 位元組加入至網頁的產生的標記。 此額外膨脹是主要是因為在中繼器控制項中，為 Treeview 控制項的內容會保存至檢視狀態。 雖然額外的 1800 位元組不看似很感興趣，使用 GridView 有許多欄位與記錄時，檢視狀態可以輕鬆地 swell 10 倍或更多的因數。

檢視狀態可能會停用頁面或控制層級設定`EnableViewState`屬性`False`，藉此減少所呈現標記的大小。 Web 控制項繫結至 Web 控制項的資料，在回傳資料的保存資料的檢視狀態，因為當停用檢視狀態資料的 Web 控制項的資料必須繫結在每個回傳。 在 ASP.NET 版本 1.x 此的責任丟給您的網頁開發人員; 過程ASP.NET 2.0，不過，Web 控制項的資料會重新繫結至其資料來源控制項，在每次回傳視。

若要減少網頁的檢視狀態，讓我們將設定中繼器控制項`EnableViewState`屬性`False`。 這可以透過設計工具中，或以宣告方式在原始碼檢視中的 [屬性] 視窗。 進行這項變更後中繼器的宣告式標記看起來應該像：


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

這項變更，頁面的轉譯檢視狀態的大小已壓縮成只之後 52 位元組為單位的 97%節省才會在檢視狀態大小 ！ 在此數列的教學課程中我們會檢視狀態的 Web 控制項的資料依預設停用為了減少呈現標記的大小。 在大部分的範例`EnableViewState`屬性將設定為`False`和執行此步驟未提及。 Web 控制項提供預期的功能狀態將會討論的檢視中資料的順序，必須已啟用的案例中是唯一的時間。

## <a name="step-4-adding-breadcrumb-navigation"></a>步驟 4： 加入階層連結巡覽

若要完成的主版頁面，讓我們加入至每個頁面的階層連結巡覽 UI 項目。 階層連結可快速顯示使用者站台階層中其目前的位置。 新增 ASP.NET 2.0 中的階層連結列只簡單 SiteMapPath 控制項加入頁面。程式碼不需要。

針對我們的站台，將此控制項加入至標頭`<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

階層連結可顯示目前頁面使用者已在站台對應階層，以及該站台對應節點 」 上階，「 瀏覽一路到根 （家中，我們的站台對應中）。


![階層連結列會顯示目前網頁和網站中的其祖系階層架構對應](master-pages-and-site-navigation-vb/_static/image28.png)

**圖 12**: 階層連結列會顯示目前網頁和網站中的其祖系階層架構對應


## <a name="step-5-adding-the-default-page-for-each-section"></a>步驟 5： 加入每個區段的預設頁面

我們的網站中的教學課程會分成不同類別基本報表功能，篩選、 的自訂格式，與每個類別目錄和對應的教學課程為該資料夾中的 ASP.NET 網頁的資料夾，依此類推。 此外，每個資料夾包含`Default.aspx`頁面。 此預設頁面上，讓我們來顯示所有目前區段的教學課程。 也就是針對`Default.aspx`中`BasicReporting`資料夾，我們會有連結`SimpleDisplay.aspx`， `DeclarativeParams.aspx`，和`ProgrammaticParams.aspx`。 在這裡，同樣地，我們可以使用`SiteMap`中定義的類別和 Web 控制項來顯示這項資訊根據網站導覽資料`Web.sitemap`。

讓我們來顯示一次，但這次我們將會顯示標題和描述的教學課程使用中繼器的未排序的清單。 因為標記和程式碼以完成這將會針對每個重複`Default.aspx` 頁面上，我們可以將封裝中的此 UI 邏輯[使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)。 在呼叫網站上建立資料夾`UserControls`並將類型名為 Web 使用者控制項的新項目加入至該`SectionLevelTutorialListing.ascx`，並加入下列標記：


[![將新的 Web 使用者控制項加入至 Usercontrol 資料夾](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**圖 13**： 將新的 Web 使用者控制項加入`UserControls`資料夾 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

在上述範例中，中繼器我們繫結`SiteMap`中繼器的資料以宣告方式;`SectionLevelTutorialListing`使用者控制項，不過，這樣做，以程式設計的方式。 在`Page_Load`事件處理常式，會進行檢查以確定此頁面 s URL 會對應至站台地圖中的節點。 如果沒有相對應的頁面中會使用這個使用者控制項`<siteMapNode>`項目，`SiteMap.CurrentNode`會傳回`Nothing`以及將繫結至中繼器的任何資料。 假設我們有`CurrentNode`，無法繫結其`ChildNodes`中繼器的集合。 因為我們網站地圖設定使`Default.aspx`頁面的每個區段是所有的教學課程，該區段中的父節點，此程式碼會顯示所有區段的教學課程的描述和連結，如下列螢幕擷取畫面所示。

一旦建立此中繼器之後，開啟`Default.aspx`頁面中每個資料夾，移至 [設計] 檢視中，並只需將使用者控制項拖曳到設計介面上的 [方案總管] 從您想要顯示教學課程的清單。


[![使用者控制項已加入至 Default.aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**圖 14**： 使用者控制項已加入至`Default.aspx`([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image34.png))


[![會列出基本報表教學課程](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**圖 15**： 列出報告的基本教學課程 ([按一下以檢視完整大小的影像](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>總結

定義站台對應和完整的主版頁面中，我們現在有一致的頁面配置和巡覽配置為我們的資料相關的教學課程。 不論多少的頁面，我們將加入至我們的網站，更新全站台的頁面配置或站台瀏覽資訊是快速且簡單的程序，因為這項資訊的集中式。 具體而言，主版頁面中定義的頁面配置資訊`Site.master`和對應中的站台`Web.sitemap`。 我們不需要撰寫*任何*來達成這項全站台的頁面配置和瀏覽機制，程式碼，我們保留在 Visual Studio 中的完整 WYSIWYG 設計工具支援。

已完成的資料存取層與商務邏輯層和具有定義一致的頁面配置和站台瀏覽，我們就可以開始探索報告的常見模式。 接下來三個教學課程中我們將探討顯示資料之 GridView、 DetailsView 和 FormView 控制項 BLL 從擷取的基本報告工作。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 主版頁面概觀](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [在 ASP.NET 2.0 的主版頁面](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 的設計範本](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET 網站巡覽概觀](https://msdn.microsoft.com/library/e468hxky.aspx)
- [檢查 ASP.NET 2.0 的站台瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 的網站瀏覽功能](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [了解 ASP.NET 檢視狀態](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [如何： 啟用 ASP.NET 網頁的追蹤](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET 使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Liz Shulok、 Dennis Patterson 和 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](creating-a-business-logic-layer-vb.md)
