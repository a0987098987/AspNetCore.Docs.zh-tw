---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: "建置自訂資料庫驅動的站台地圖提供者 (C#) |Microsoft 文件"
author: rick-anderson
description: "在 ASP.NET 2.0 的預設站台對應提供者會從靜態的 XML 檔案擷取其資料。 雖然許多小型和中型 siz 適合以 XML 為基礎的提供者..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: cc0de856cb1ae2cf8e1f18a29ae29a3b226c12ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>建置自訂資料庫驅動的站台地圖提供者 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip)或[下載 PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> 在 ASP.NET 2.0 的預設站台對應提供者會從靜態的 XML 檔案擷取其資料。 以 XML 為基礎的提供者適合有許多小型和中型網站時，較大的 Web 應用程式需要更具動態網站地圖。 在此教學課程中，我們將建立自訂站台地圖提供者從商務邏輯層擷取其資料，這會從資料庫擷取資料。


## <a name="introduction"></a>簡介

ASP.NET 2.0 s 站台地圖 功能可讓網頁開發人員定義 web 應用程式的網站導覽某些永續性的媒介，例如，在 XML 檔案。 定義之後，可以透過程式設計方式存取網站導覽資料[`SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空間](https://msdn.microsoft.com/library/system.web.aspx)或透過不同的瀏覽 Web 控制項，例如SiteMapPath、 功能表和樹狀檢視控制項。 站台對應系統會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)使不同的站台對應序列化實作可建立及插入 web 應用程式。 ASP.NET 2.0 隨附的預設站台對應提供者保存網站導覽結構中的 XML 檔案。 回到[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中我們建立了名為`Web.sitemap`，包含此結構，並具有已更新其 XML 與每個新的教學課程 > 一節。

如果網站導覽的結構是靜態的例如針對這些教學課程的運作方式的預設 XML 為基礎的站台對應提供者。 在許多案例中，不過，更具動態網站地圖需要。 請考量圖 1，其中每個分類和產品會顯示為網站結構中的章節中的站台對應。 與此站台對應，對應至根節點的網頁瀏覽可能會列出所有類別，而瀏覽特定分類 s web 網頁會列出該分類產品，並檢視特定產品 s web 網頁會顯示該產品 s 詳細資料。


[![分類和產品架構的站台對應的結構](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**圖 1**: 分類和產品結構網站地圖的結構 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


雖然此類別目錄和產品基礎結構可能是硬式編碼成`Web.sitemap`檔案，檔案會想要每次一個類別目錄更新或產品已加入、 移除或重新命名。 因此，站台對應維護會大幅簡化如果其結構已擷取從資料庫或在理想情況下，從應用程式的架構的商務邏輯層。 這樣一來，當產品和分類已新增，請重新命名或刪除，站台地圖會自動更新以反映這些變更。

因為 ASP.NET 2.0 s 站台對應序列化已內建在提供者模型之上，我們可以建立自己 grabs 示範的替代資料存放區，例如資料庫或架構從其資料的自訂站台地圖提供者。 在本教學課程中，我們會建置 BLL 從擷取其資料的自訂提供者。 Let s 開始 ！

> [!NOTE]
> 在本教學課程中建立的自訂站台對應提供者會緊密結合的應用程式的架構和資料模型。 Jeff Prosise s[儲存在 SQL Server 中的站台對應](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)和[SQL 站台地圖提供者等候您已如](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)文章檢查將網站導覽資料儲存在 SQL Server 中的一般化的方法。


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>步驟 1： 建立自訂站台對應提供者 Web 網頁

我們可以開始建立自訂站台地圖提供者之前，可讓 s 先加入 ASP.NET 網頁，我們需要在此教學課程。 加入新的資料夾，名為啟動`SiteMapProvider`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

也新增`CustomProviders`子資料夾以`App_Code`資料夾。


![如需站台對應提供者相關教學課程中加入 ASP.NET 網頁](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**圖 2**： 加入 ASP.NET 網頁站台對應提供者相關的教學課程


因為沒有為此區段只有一個教學課程，我們不需要 t`Default.aspx`列出 s 區段教學課程。 相反地，`Default.aspx`將 GridView 控制項中顯示的類別。 我們將會處理這在步驟 2 中。

接下來，更新`Web.sitemap`參考`Default.aspx`頁面。 具體來說，在快取之後加入下列標記`<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 左側功能表現在包含唯一的站台對應提供者教學課程中的項目。


![站台對應提供者教學課程網站導覽現在包含的項目](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**圖 3**： 網站導覽現在包含一個項目，站台對應提供者教學課程


此教學課程 s 的主要重點是要說明建立自訂站台地圖提供者及設定 web 應用程式使用該提供者。 特別是，我們會建置的提供者，傳回包含根節點的節點代表每個分類和產品，以及站台對應，如圖 1 所示。 一般情況下，站台對應中的每個節點可能指定的 URL。 針對我們的站台對應，將會在根節點的 URL `~/SiteMapProvider/Default.aspx`，其中會列出所有資料庫中的類別。 站台對應中的每個類別目錄節點必須指向的 URL `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`，其中會列出所有指定的產品*categoryID*。 最後，將會為每個產品網站導覽節點`~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`，這會顯示特定產品 s 詳細資料。

若要開始建立所需的`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面。 這些頁面是在步驟 2、 3 和 4 分別完成。 因為介面的本教學課程的主要目的是在站台對應提供者，但因為過去的教學課程涵蓋了建立多頁主要/詳細資料這類報告，我們將會快步驟 2 到 4。 如果您需要建立跨越多個頁面的主要/詳細資料報表的重新整理程式，請參閱上一步[主要/詳細資料篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-cs.md)教學課程。

## <a name="step-2-displaying-a-list-of-categories"></a>步驟 2： 顯示類別目錄的清單

開啟`Default.aspx`頁面`SiteMapProvider`資料夾，然後拖曳 GridView 從工具箱拖曳至設計工具中，設定其`ID`至`Categories`。 從 GridView s 智慧標籤，將它繫結至名為新 ObjectDataSource`CategoriesDataSource`並加以設定，讓它會擷取其資料使用`CategoriesBLL`類別的`GetCategories`方法。 由於此 GridView 只會顯示類別，而且不會提供資料修改功能，設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![設定要傳回使用 GetCategories 方法的類別 ObjectDataSource](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**圖 4**： 設定要傳回的類別使用 ObjectDataSource`GetCategories`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**圖 5**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


完成設定資料來源精靈之後，Visual Studio 會加入為 BoundField `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath`。 編輯 GridView，使其只包含`CategoryName`和`Description`BoundFields 並更新`CategoryName`BoundField 的`HeaderText`類別目錄的屬性。

接下來，新增 HyperLinkField，並因此放在它 s 最左邊的欄位。 設定`DataNavigateUrlFields`屬性`CategoryID`和`DataNavigateUrlFormatString`屬性`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`。 設定`Text`檢視產品的屬性。


![加入類別 GridView HyperLinkField](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**圖 6**： 新增至 HyperLinkField `Categories` GridView


在之後建立 ObjectDataSource 和自訂 GridView 的欄位時，兩個控制項的宣告式標記看起來如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

圖 7 顯示`Default.aspx`透過瀏覽器檢視時。 按一下 類別檢視產品的連結會帶您前往`ProductsByCategory.aspx?CategoryID=categoryID`，我們將在步驟 3 中建置的。


[![每個類別目錄會列出沿著檢視產品連結](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**圖 7**： 每個類別是列出沿著檢視產品連結 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>步驟 3： 列出選取的類別的產品

開啟`ProductsByCategory.aspx`頁面上，並加入 GridView，其命名為`ProductsByCategory`。 從其智慧標籤，將繫結 GridView 至名為新 ObjectDataSource `ProductsByCategoryDataSource`。 設定用於 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`標籤中的 UPDATE、 INSERT 和 DELETE 方法並將下拉式清單列出為 （無）。


[![使用 ProductsBLL 類別的 GetProductsByCategoryID(categoryID) 方法](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**圖 8**： 使用`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


設定資料來源精靈的最後一個步驟會提示輸入參數的來源*categoryID*。 因為這項資訊透過查詢字串欄位傳遞`CategoryID`、 從下拉式清單中選取 QueryString 和 QueryStringField 文字方塊中輸入 CategoryID，如圖 9 所示。 按一下 [完成] 5d; 以完成精靈。


[![用於 categoryID 參數 CategoryID Querystring 欄位](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**圖 9**： 使用`CategoryID`Querystring 欄位針對*categoryID*參數 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


完成精靈之後，Visual Studio 將會加入對應 BoundFields 及其產品資料欄位的 GridView。 移除以外的所有`ProductName`， `UnitPrice`，和`SupplierName`BoundFields。 自訂這些三個 BoundFields`HeaderText`分別讀取產品、 價格和供應商的屬性。 格式`UnitPrice`BoundField 為貨幣。

接下來，新增 HyperLinkField，並將它移至最左邊的位置。 設定其`Text`屬性以檢視詳細資料，其`DataNavigateUrlFields`屬性`ProductID`，且其`DataNavigateUrlFormatString`屬性`~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`。


![加入指向 ProductDetails.aspx 檢視詳細資料 HyperLinkField](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**圖 10**： 加入 檢視詳細資料 HyperLinkField 指向`ProductDetails.aspx`


進行這些自訂後, GridView 和 ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

返回檢視`Default.aspx`透過瀏覽器，並按一下 檢視產品飲料的連結。 這會帶您到`ProductsByCategory.aspx?CategoryID=1`，顯示名稱、 價格以及產品的供應商屬於 「 飲料 」 分類的 Northwind 資料庫中 （請參閱圖 11）。 歡迎進一步加強此頁面來包含使用者返回類別目錄清單頁面的連結 (`Default.aspx`) 和顯示選取的類別 s 的名稱和描述的 DetailsView 或 FormView 控制項。


[![顯示飲料名稱、 價格以及供應商](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**圖 11**： 顯示飲料名稱、 價格以及供應商 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>步驟 4： 顯示產品 s 詳細資料

最後一頁， `ProductDetails.aspx`，顯示所選的產品詳細資料。 開啟`ProductDetails.aspx`拖曳 DetailsView 從 [工具箱] 拖曳至設計工具。 設定 DetailsView s`ID`屬性`ProductInfo`和清除其`Height`和`Width`屬性值。 從其智慧標籤，將繫結 DetailsView 至名為新 ObjectDataSource `ProductDataSource`，設定提取資料的來源 ObjectDataSource`ProductsBLL`類別的`GetProductByProductID(productID)`方法。 如同在步驟 2 和 3 中建立的上一個網頁，設定下拉式清單中更新、 插入和刪除索引標籤，以 （無）。


[![設定為使用 GetProductByProductID(productID) 方法 ObjectDataSource](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**圖 12**： 設定要使用 ObjectDataSource`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


設定資料來源精靈的最後一個步驟會提示輸入的來源*productID*參數。 因為這項資料是透過查詢字串欄位`ProductID`，設 QueryString 和以 ProductID QueryStringField 文字方塊中的下拉式清單。 最後，按一下 [完成] 按鈕以完成精靈。


[![設定產品識別碼 ProductID Querystring 欄位從提取其值的參數](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**圖 13**： 設定*productID*提取從其值的參數`ProductID`Querystring 欄位 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


完成設定資料來源精靈之後，Visual Studio 會建立對應的 BoundFields 及其 DetailsView 產品資料欄位中。 移除`ProductID`， `SupplierID`，和`CategoryID`BoundFields 並適當地設定其餘的欄位。 在少數幾個美觀的組態之後, 我 DetailsView 和 ObjectDataSource s 宣告式標記看起來如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

若要測試此頁面，返回`Default.aspx`，按一下 檢視產品上針對 「 飲料 」 分類。 飲料產品清單中，按一下 [Chai 茶杯的檢視詳細資料] 連結。 這會帶您到`ProductDetails.aspx?ProductID=1`，其中顯示 Chai 茶杯 s （請參閱圖 14） 的詳細資料。


[![顯示 Chai 茶杯 s 供應商，類別目錄、 價格和其他資訊](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**圖 14**: Chai 茶杯 s 供應商，類別目錄、 價格和其他資訊會顯示 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>步驟 5： 了解站台地圖提供者的內部運作方式

站台地圖以 web 伺服器記憶體中的集合`SiteMapNode`形成階層的執行個體。 必須有一個根，所有的非根節點必須剛好只有一個父節點，而且所有節點都可能都有任意數目的子系。 每個`SiteMapNode`物件表示的區段內的網站的結構; 這些章節通常會有對應的網頁。 因此， [ `SiteMapNode`類別](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)具有屬性，例如`Title`， `Url`，和`Description`，提供資訊區段`SiteMapNode`代表。 另外還有`Key`屬性可唯一識別每個`SiteMapNode`階層，以及用來建立此階層的屬性`ChildNodes`， `ParentNode`， `NextSibling`， `PreviousSibling`，依此類推。

圖 15 顯示一般網站導覽結構來自圖 1 中，但具有勾勒出詳細程度的實作詳細資料。


[![每個 SiteMapNode 具有屬性類似標題、 Url、 金鑰和等等](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**圖 15**： 每個`SiteMapNode`有屬性例如`Title`， `Url`，`Key`等等 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


站台對應是透過可存取[`SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空間](https://msdn.microsoft.com/library/system.web.aspx)。 這個類別 s`RootNode`屬性會傳回站台對應的根目錄`SiteMapNode`執行個體。`CurrentNode`傳回`SiteMapNode`其`Url`屬性會比對的目前要求的網頁 URL。 這個類別是由 ASP.NET 2.0 s 瀏覽 Web 控制項的內部使用。

當`SiteMap`類別的屬性存取，它必須將某些永續性的媒體中的站台對應結構序列化至記憶體。 不過，站台對應序列化邏輯不是硬式編碼至`SiteMap`類別。 相反地，在執行階段`SiteMap`類別會決定哪一個站台地圖*提供者*供序列化使用。 根據預設， [ `XmlSiteMapProvider`類別](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)使用時，從格式正確的 XML 檔案讀取站台對應的結構。 不過，利用最少的工作，我們可以建立自己的自訂站台地圖提供者。

所有站台對應提供者必須衍生自[`SiteMapProvider`類別](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx)、 包含重要的方法和屬性所需的站台對應提供者，但是省略許多實作詳細資料。 第二個類別[ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx)，擴充`SiteMapProvider`類別，並包含所需的功能較為複雜的實作。 就內部而言，`StaticSiteMapProvider`儲存`SiteMapNode`站台的執行個體對應在`Hashtable`，並提供等方法`AddNode(child, parent)`，`RemoveNode(siteMapNode),`和`Clear()`，加入和移除`SiteMapNode`s 至內部`Hashtable`。 `XmlSiteMapProvider` 衍生自 `StaticSiteMapProvider`。

當建立自訂站台地圖提供者擴充`StaticSiteMapProvider`，有兩種抽象方法，必須覆寫： [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx)和[ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx)。 `BuildSiteMap`正如其名，負責從永續性儲存體載入網站導覽結構並建構在記憶體中。 `GetRootNodeCore`傳回站台對應中的根節點。

之前的 web 應用程式可以使用站台對應的提供者，則必須在應用程式的組態中註冊。 根據預設，`XmlSiteMapProvider`名稱註冊類別`AspNetXmlSiteMapProvider`。 若要註冊額外的站台對應提供者，加入下列標記以`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*名稱*值將人類看得懂的名稱指派給提供者時*類型*指定站台地圖提供者的完整限定類型名稱。 我們將探討的具象值*名稱*和*類型*步驟 7 中的值之後，我們已建立我們的自訂站台地圖提供者。

站台對應提供者類別具現化第一次從存取`SiteMap`類別並維持在記憶體中的 web 應用程式的存留期。 因為只有一個執行個體可能會從叫用多個並行的網站訪客的站台對應提供者，請務必提供者的方法，是*安全執行緒*。

效能和延展性的原因，它 s 重要我們快取記憶體中的站台對應的結構，並傳回此快取結構，而非每次重新建立`BuildSiteMap`叫用方法。 `BuildSiteMap`可能會呼叫多次，每個頁面要求每位使用者，根據導覽控制項中使用的頁面及網站導覽結構的深度。 在任何情況下，如果我們不要快取中的站台對應結構`BuildSiteMap`然後我們會需要每次叫用時重新擷取的架構 （這會導致查詢資料庫） 的產品和分類資訊。 如我們所討論在先前的快取教學課程中，快取的資料可能會變成過時。 若要避免此問題，我們可以使用時間-或 SQL 快取相依性為基礎的 expiries。

> [!NOTE]
> 站台地圖提供者可能會選擇性地覆寫[`Initialize`方法](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx)。 `Initialize`當站台對應提供者會先具現化會傳遞給提供者中的任何自訂屬性時，會叫用`Web.config`中`<add>`類似的項目： `<add name="name" type="type" customAttribute="value" />`。 如果您想要允許網頁開發人員指定各種站台對應提供者相關設定，而不需要修改提供者 s 程式碼，就會很有用。 例如，如果我們已分類和產品資料直接從資料庫讀取與架構中，我們 d 可能想要讓網頁開發人員指定資料庫連接字串透過`Web.config`而不是使用硬式編碼提供者 s 程式碼中的值。 步驟 6 中，我們將建立的自訂站台對應提供者不會覆寫這`Initialize`方法。 如需使用`Initialize`方法，是指[Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s[儲存在 SQL Server 中的站台對應](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)發行項。


## <a name="step-6-creating-the-custom-site-map-provider"></a>步驟 6： 建立自訂站台地圖提供者

若要建立自訂站台地圖提供者可建立站台的對應的分類和產品 Northwind 資料庫中的，我們必須建立該類別可擴充`StaticSiteMapProvider`。 在步驟 1 中詢問您要加入`CustomProviders`資料夾中的`App_Code`資料夾將新類別加入名為此資料夾`NorthwindSiteMapProvider`。 將下列程式碼加入 `NorthwindSiteMapProvider` 類別：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

可讓開始瀏覽此類別 s 的 s`BuildSiteMap`方法，以開始[`lock`陳述式](https://msdn.microsoft.com/library/c5kehkcz.aspx)。 `lock`陳述式只允許一個執行緒每次輸入，藉以序列化其程式碼存取，並防止在另一個 s 腳趾算得出來逐步執行兩個並行執行緒。

類別層級`SiteMapNode`變數`root`用來快取網站導覽結構。 第一次，或第一次之後已修改基礎資料，建構網站導覽時`root`會`null`和網站導覽結構會在建構。 站台對應的根節點指派給`root`建構期間處理程序，以便在下一次這個方法呼叫時，`root`將不會`null`。 因此，只要`root`不`null`網站導覽結構將傳回給呼叫端，而不必重新建立它。

如果是根`null`，網站導覽結構從產品和分類的資訊建立。 藉由建立建置網站導覽`SiteMapNode`執行個體，然後再建構透過呼叫階層`StaticSiteMapProvider`類別的`AddNode`方法。 `AddNode`執行內部簿記，儲存各種`SiteMapNode`中執行個體`Hashtable`。 我們一開始建構階層之前，我們一開始呼叫`Clear`方法，從內部的項目可清除`Hashtable`。 下一步`ProductsBLL`類別 s`GetProducts`方法，並產生`ProductsDataTable`儲存在本機變數。

站台對應的建構開始建立根節點，並將它指派給`root`。 多載[ `SiteMapNode` s 建構函式](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx)使用此處，而這整個`BuildSiteMap`傳遞下列資訊：

- 站台對應提供者的參考 (`this`)。
- `SiteMapNode` s `Key`。 需要這個值必須是唯一的每個`SiteMapNode`。
- `SiteMapNode` s `Url`。 `Url`是選擇性的但如果提供，每個`SiteMapNode`s`Url`值必須是唯一。
- `SiteMapNode` s `Title`，這是必要。

`AddNode(root)`方法呼叫加入`SiteMapNode``root`做為根的站台對應。 接著，每個`ProductRow`中`ProductsDataTable`列舉。 如果已經有`SiteMapNode`目前產品 s 分類中，參考它。 否則，新`SiteMapNode`的類別目錄會建立並新增為子系`SiteMapNode``root`透過`AddNode(categoryNode, root)`方法呼叫。 在適當的類別之後`SiteMapNode`節點已找到或建立，`SiteMapNode`為目前的產品建立並加入做為子類別目錄的`SiteMapNode`透過`AddNode(productNode, categoryNode)`。 請注意，類別目錄`SiteMapNode`s`Url`屬性值是`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`而產品`SiteMapNode`s`Url`屬性會被指派`~/SiteMapNode/ProductDetails.aspx?ProductID=productID`。

> [!NOTE]
> 這些資料庫的產品`NULL`值及其`CategoryID`在類別目錄底下的分組`SiteMapNode`其`Title`屬性設定為 None，其`Url`屬性設定為空字串。 我決定設定`Url`設為空字串，因為`ProductBLL`類別 s`GetProductsByCategory(categoryID)`方法目前缺少傳回僅具有產品`NULL``CategoryID`值。 我想要示範導覽控制項的呈現方式的同時，`SiteMapNode`所缺少的值及其`Url`屬性。 建議您延伸本教學課程，讓 無`SiteMapNode`s`Url`屬性會指向`ProductsByCategory.aspx`，但只會顯示與產品`NULL``CategoryID`值。


任意的物件新增至使用上的 SQL 快取相依性的資料快取後建構站台對應，`Categories`和`Products`資料表透過`AggregateCacheDependency`物件。 我們已在先前的教學課程中，使用 SQL 快取相依性探索*使用 SQL 快取相依性*。 自訂站台地圖提供者，不過，會使用多載的資料快取的`Insert`方法我們尚未來瀏覽過。 這個多載會接受做為其最終的輸入參數，從快取中移除物件時呼叫委派。 具體來說，我們會傳送新[`CacheItemRemovedCallback`委派](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx)，它會指向`OnSiteMapChanged`方法定義中的進一步向下`NorthwindSiteMapProvider`類別。

> [!NOTE]
> 站台對應的記憶體中表示法會透過類別層級變數快取`root`。 因為只有一個執行個體的自訂站台對應提供者類別，因為該執行個體 web 應用程式中所有執行緒之間共用，此類別變數可做為快取中。 `BuildSiteMap`方法也會使用資料快取，但僅做為基礎資料庫中的資料時收到通知`Categories`或`Products`資料表的變更。 請注意，此值放入資料快取目前的日期和時間。 實際站台地圖資料是*不*放置於資料快取。


`BuildSiteMap`方法完成所傳回的站台地圖的根節點。

其餘的方法都相當直接明瞭。 `GetRootNodeCore`會負責傳回的根節點。 因為`BuildSiteMap`傳回根`GetRootNodeCore`只會傳回`BuildSiteMap`s 傳回值。 `OnSiteMapChanged`方法會設定`root`回到`null`移除快取項目時。 根設回`null`，下次`BuildSiteMap`是叫用，網站導覽結構將會重建。 最後，`CachedDate`如果存在於這類的值，屬性會傳回儲存在資料快取的日期和時間值。 這個屬性可以供網頁開發人員，以判斷當站台地圖資料上一次快取。

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>步驟 7： 註冊`NorthwindSiteMapProvider`

在我們的 web 應用程式，才能使用`NorthwindSiteMapProvider`步驟 6 中建立的站台對應提供者，我們需要註冊在`<siteMap>`區段`Web.config`。 具體來說，加入下列標記內`<system.web>`中的項目`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

這個標記會執行兩項工作： 首先，它會指出內建`AspNetXmlSiteMapProvider`為預設站台對應提供者; 第二，它會註冊以人類看得易記名稱 Northwind 步驟 6 中建立的自訂站台對應提供者。

> [!NOTE]
> 站台對應提供者位於應用程式 s`App_Code`資料夾、 值`type`屬性就是類別名稱。 或者，自訂站台地圖提供者可建立個別的類別庫專案中使用編譯的組件放在 web 應用程式的`/Bin`目錄。 在此情況下，`type`屬性值會是*命名空間*。*ClassName*， *AssemblyName* 。


在更新之後， `Web.config`，請花一點時間瀏覽器中檢視任何頁面上，從教學課程。 請注意，在左側瀏覽介面仍會顯示各節，教學課程中定義`Web.sitemap`。 這是因為我們保留`AspNetXmlSiteMapProvider`做為預設提供者。 若要建立使用的瀏覽使用者介面項目`NorthwindSiteMapProvider`，我們需要明確指定應 Northwind 站台地圖提供者。 我們會了解如何完成這項作業在步驟 8。

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>步驟 8： 顯示使用自訂站台地圖提供者的站台對應資訊

使用自訂站台地圖提供者建立並註冊在`Web.config`，我們要加入導覽控制項，以準備好`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`中的分頁`SiteMapProvider`資料夾。 先開啟`Default.aspx`頁面上，拖曳`SiteMapPath`從 [工具箱] 拖曳至設計工具。 SiteMapPath 控制項位於 [工具箱] 的 [導覽] 區段中。


[![SiteMapPath 加入 Default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**圖 16**： 新增至 SiteMapPath `Default.aspx` ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


SiteMapPath 控制項顯示階層連結列，指出目前的網頁的位置內站台對應。 我們加入主版頁面的頂端加入 SiteMapPath 回*主版頁面和站台瀏覽*教學課程。

請花一點時間來檢視此頁面，透過瀏覽器。 圖 16 加入 SiteMapPath 會使用預設站台對應提供者，提取資料的來源`Web.sitemap`。 因此，階層連結顯示首頁&gt;自訂站台對應，就像在右上角階層連結。


[![階層會使用預設站台地圖提供者](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**圖 17**： 階層會使用預設的站台地圖提供者 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


若要加入圖 16 SiteMapPath，使用您在步驟 6 中我們建立的自訂站台對應提供者，請設定其[`SiteMapProvider`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx)至 Northwind，我們指派給名稱`NorthwindSiteMapProvider`中`Web.config`。 不幸的是，設計工具會繼續使用預設站台對應提供者，但如果您可透過瀏覽器頁面瀏覽屬性變更之後您會看到階層連結現在會使用自訂站台地圖提供者。


[![階層連結現在會使用自訂站台對應提供者 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**圖 18**： 階層連結現在會使用自訂站台地圖提供者`NorthwindSiteMapProvider`([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


SiteMapPath 控制項顯示的其他功能的使用者介面中`ProductsByCategory.aspx`和`ProductDetails.aspx`頁面。 加入這些頁，設定 SiteMapPath`SiteMapProvider`中都為 Northwind 的屬性。 從`Default.aspx`按一下檢視詳細資料 連結以 Chai 茶杯的飲料，檢視產品連結，然後按一下。 階層圖 19 所示，包含目前的站台對應區段 （Chai 茶杯） 以及其上階： Beverages 和所有類別目錄。


[![階層連結現在會使用自訂站台對應提供者 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**圖 19**： 階層連結現在會使用自訂站台地圖提供者`NorthwindSiteMapProvider`([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


除了 SiteMapPath，例如功能表和 TreeView 控制項可使用其他瀏覽使用者介面項目。 `Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面下載此教學課程中，例如，在所有包含功能表控制項 （請參閱圖 20）。 請參閱[檢查 ASP.NET 2.0 的網站瀏覽功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)和[使用站台瀏覽控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx)區段[ASP.NET 2.0 快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/)的更深入探討導覽控制與 ASP.NET 2.0 中的站台對應系統。


[![此功能表控制項列出每個分類和產品](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**圖 20**: 功能表控制項列出每個分類和產品 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


如稍早在本教學課程中所述，可以透過程式設計方式存取網站導覽結構`SiteMap`類別。 下列程式碼會傳回根`SiteMapNode`的預設提供者：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

因為`AspNetXmlSiteMapProvider`是預設提供者應用程式中，如上述程式碼會傳回中所定義的根節點`Web.sitemap`。 若要參考非預設的站台地圖提供者，使用`SiteMap`類別 s [ `Providers`屬性](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx)就像這樣：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

其中*名稱*自訂站台地圖提供者 (Northwind，web 應用程式) 的名稱。

若要存取站台對應提供者專屬的成員，請使用`SiteMap.Providers["name"]`擷取提供者執行個體，並再將它轉換成適當的型別。 例如，若要顯示`NorthwindSiteMapProvider`s`CachedDate`屬性在 ASP.NET 網頁，可使用下列程式碼：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> 請務必測試 SQL 快取相依性功能。 之後造訪`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面，請移至其中一個編輯、 插入和刪除 > 一節中的教學課程，以及編輯分類或產品的名稱。 然後返回的其中一個在分頁`SiteMapProvider`資料夾。 假設要注意基礎資料庫變更的輪詢機制，如有充裕的時間，站台對應應該更新以顯示新的產品或分類名稱。


## <a name="summary"></a>總結

ASP.NET 2.0 s 站台地圖功能包括`SiteMap`類別的數字的內建瀏覽 Web 控制項，和預設站台地圖提供者預期的站台對應資訊保存為 XML 檔案。 若要使用來自其他來源這類資料庫中的站台對應資訊，將應用程式的架構或遠端的 Web 服務，我們必須建立自訂的站台對應提供者。 這項作業包括建立直接或間接衍生的類別從`SiteMapProvider`類別。

本教學課程中我們可了解如何建立自訂站台對應提供者基礎挑選從應用程式架構的產品和類別目錄資訊的站台對應。 我們提供者的擴充`StaticSiteMapProvider`類別並產生建立`BuildSiteMap`擷取資料的方法建構網站導覽階層架構，並快取所產生的結構，在類別層級變數。 我們要使其失效的快取使用的回呼函式與 SQL 快取相依性結構時的基本`Categories`或`Products`資料修改。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 SQL Server 中儲存站台對應](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)和[等您 ve SQL 站台地圖提供者](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [ASP.NET 2.0 看 s 提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [提供者的工具組](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [檢查 ASP.NET 2.0 的網站瀏覽功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Dave Gardner、 Zack Jones、 本文菲和 Bernadette Leigh。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一步](building-a-custom-database-driven-site-map-provider-vb.md)
