---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: 建置自訂資料庫驅動網站導覽提供者 (C#) |Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 預設網站導覽提供者會將其資料擷取是靜態的 XML 檔案。 雖然以 XML 為基礎的提供者是適合許多小型和中型 siz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: 1160711911d53dcf889ced1a8422c2dfcf72b240
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368692"
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>建置自訂資料庫驅動網站導覽提供者 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip)或[下載 PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> ASP.NET 2.0 預設網站導覽提供者會將其資料擷取是靜態的 XML 檔案。 適用於許多小型和中型網站以 XML 為基礎的提供者時，較大的 Web 應用程式會需要更動態的站台對應。 在本教學課程，我們將建置自訂網站地圖提供者，擷取其資料與商務邏輯層，其中依序從資料庫擷取資料。


## <a name="introduction"></a>簡介

ASP.NET 2.0 s 站台導覽功能可讓網頁開發人員定義 web 應用程式的網站地圖，某些永續性的媒體，例如，在 XML 檔案。 定義之後，可以透過程式設計方式存取網站導覽資料[`SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空間](https://msdn.microsoft.com/library/system.web.aspx)或透過不同的瀏覽 Web 控制項，例如SiteMapPath、 功能表和 TreeView 控制項。 站台對應系統會使用[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，讓不同的站台對應序列化實作可以建立並插入 web 應用程式。 預設網站導覽提供者隨附於 ASP.NET 2.0 保存網站導覽結構中的 XML 檔案。 回到[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中我們建立名為`Web.sitemap`，包含此結構，並有已更新其 XML 與每個新的教學課程區段。

預設值 XML 為基礎的站台的網站導覽提供者適用於這些教學課程中是相當靜態的例如網站地圖的結構。 在許多情況下，不過，更動態的站台對應需要。 圖 1，其中每個類別目錄和產品會顯示為網站的結構中的各節所示的站台對應，請考慮。 與此站台對應中，瀏覽對應至根節點的網頁可能會列出所有類別，而瀏覽特定類別的網頁會列出該類別目錄產品，並檢視特定產品的網頁會顯示該產品 s 詳細資料。


[![分類和產品結構 s 網站導覽結構](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**圖 1**: 分類和產品結構 s 的網站導覽結構 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


雖然此類別目錄和產品基礎結構可能是硬式編碼至`Web.sitemap`檔案，檔案會需要更新的類別每次或產品已新增、 移除或重新命名。 因此，站台對應維護所能大幅簡化如果從資料庫或在理想情況下，從商業邏輯層的應用程式的架構擷取其結構。 如此一來，已新增的產品和分類，重新命名或刪除，站台地圖會自動更新以反映這些變更。

因為在提供者模型之上建置 ASP.NET 2.0 s 站台對應序列化時，我們可以建立自己自訂網站地圖提供者，會擷取其資料，從替代資料存放區，例如資料庫或架構。 在本教學課程中，我們將建置 BLL 中擷取其資料的自訂提供者。 讓 s 開始 ！

> [!NOTE]
> 在本教學課程中建立的自訂站台網站導覽提供者會緊密結合到應用程式的架構和資料模型。 Jeff Prosise s [SQL Server 中儲存網站地圖](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)並[SQL 網站導覽提供者您 ve 一直在等待](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)文章檢查以通用的方法，來在 SQL Server 中儲存網站地圖資料。


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>步驟 1： 建立自訂的網站對應提供者的 Web 網頁

我們開始建立自訂網站地圖提供者之前，讓第一次加入我們在本教學課程需要 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`SiteMapProvider`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

也加入`CustomProviders`的子資料夾`App_Code`資料夾。


![如需站台對應提供者相關教學課程加入 ASP.NET 網頁](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**圖 2**： 加入 ASP.NET 網頁，站台對應提供者相關的教學課程


因為只有一個此章節的教學課程時，我們不需要 t`Default.aspx`列出區段 s 教學課程。 相反地，`Default.aspx`將 GridView 控制項中顯示類別目錄。 我們將在步驟 2 中解決這個問題。

接下來，更新`Web.sitemap`若要包含參考`Default.aspx`頁面。 具體來說，快取之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含項目，唯一的站台對應提供者教學課程。


![網站導覽現在包含一個項目，站台對應提供者教學課程](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**圖 3**： 網站地圖現在包含一個項目，站台對應提供者教學課程


本教學課程的 s 的重點是要說明建立自訂網站地圖提供者和設定 web 應用程式使用該提供者。 特別是，我們將建置的提供者，會傳回包含根節點的節點，針對每個類別目錄和產品，以及站台對應，如 圖 1 所述。 一般情況下，每個節點在網站導覽中的可能指定的 URL。 根節點的 URL 會是我們的站台對應， `~/SiteMapProvider/Default.aspx`，其中會列出所有資料庫中的類別。 網站導覽中的每個類別目錄 節點會有 URL 指向`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`，其中會列出所有與指定的產品*categoryID*。 最後，每個產品網站導覽節點將會指到`~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`，這會顯示特定產品 s 詳細資料。

若要啟動我們需要建立`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面。 這些頁面會在步驟 2、 3 和 4，分別完成。 由於本教學課程的主要目的是在網站導覽提供者，而因為過去的教學課程已涵蓋建立這種多頁主版/詳細報告，我們會趕快到步驟 2 到 4。 如果您需要複習一下建立跨越多個頁面的主版/詳細報告，請參閱上一步[主版/詳細篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-cs.md)教學課程。

## <a name="step-2-displaying-a-list-of-categories"></a>步驟 2： 顯示一份類別

開啟`Default.aspx`頁面中`SiteMapProvider`資料夾，然後拖曳的 GridView，從 [工具箱] 拖曳至設計工具，設定其`ID`至`Categories`。 從 GridView s 智慧標籤，將它繫結至名為新 ObjectDataSource`CategoriesDataSource`並將其設定，因此它會擷取其資料使用`CategoriesBLL`類別的`GetCategories`方法。 由於此 GridView 只會顯示類別，而且不會提供資料修改功能，設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。


[![設定要傳回類別使用 GetCategories 方法的 ObjectDataSource](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**圖 4**： 設定要傳回類別使用 ObjectDataSource`GetCategories`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**圖 5**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


完成設定資料來源精靈之後，Visual Studio 會新增為 BoundField `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath`。 編輯 GridView，使其只包含`CategoryName`並`Description`BoundFields 並更新`CategoryName`BoundField 的`HeaderText`類別目錄的屬性。

接下來，新增 HyperLinkField，並將其放置到因此它 s 最左邊的欄位。 設定`DataNavigateUrlFields`屬性，以`CategoryID`並`DataNavigateUrlFormatString`屬性設`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`。 設定`Text`檢視產品的屬性。


![加入類別 GridView HyperLinkField](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**圖 6**： 新增至 HyperLinkField `Categories` GridView


建立 ObjectDataSource 和自訂之後 GridView 的欄位，兩個控制項宣告式標記看起來如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

[圖 7] 顯示`Default.aspx`透過瀏覽器檢視時。 按一下 類別目錄的 View Products 連結會帶您前往`ProductsByCategory.aspx?CategoryID=categoryID`，我們會建置在步驟 3 中。


[![每個類別目錄會列出沿著檢視產品連結](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**圖 7**： 每個類別目錄會列出沿著檢視產品連結 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>步驟 3： 列出選取的類別目錄產品

開啟`ProductsByCategory.aspx`頁面上，並新增一個 GridView，將它命名為`ProductsByCategory`。 從它的智慧標籤，將繫結 GridView 至名為新 ObjectDataSource `ProductsByCategoryDataSource`。 設定要使用 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法，並設定下拉式清單為 (None) 會列出與更新、 插入和刪除的索引標籤中。


[![使用 ProductsBLL 類別的 GetProductsByCategoryID(categoryID) 方法](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**圖 8**： 使用`ProductsBLL`類別 s`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


設定資料來源精靈的最後一個步驟會提示輸入參數的來源*categoryID*。 因為這項資訊透過查詢字串欄位傳遞`CategoryID`、 從下拉式清單中選取 查詢字串和 QueryStringField 文字方塊中輸入 CategoryID，如 圖 9 所示。 按一下 完成 以完成精靈。


[![[CategoryID] 查詢字串欄位用於 categoryID 參數](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**圖 9**： 使用`CategoryID`Querystring 欄位*categoryID*參數 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


完成精靈之後，Visual Studio 會將對應的 BoundFields 及其產品資料欄位的 GridView。 移除以外的所有`ProductName`， `UnitPrice`，和`SupplierName`BoundFields。 自訂這些三個 BoundFields`HeaderText`分別讀取產品、 價格和供應商的屬性。 格式`UnitPrice`BoundField 的貨幣。

接下來，新增 HyperLinkField，並將它移到最左邊位置。 設定其`Text`屬性，以檢視詳細資料，其`DataNavigateUrlFields`屬性設`ProductID`，並將其`DataNavigateUrlFormatString`屬性設`~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`。


![加入指向 ProductDetails.aspx 檢視詳細資料 HyperLinkField](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**圖 10**： 新增檢視詳細資料，指向 HyperLinkField `ProductDetails.aspx`


完成之後這些自訂，GridView 和 ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

傳回檢視`Default.aspx`透過瀏覽器，並按一下 View Products 飲料的連結。 這會帶您前往`ProductsByCategory.aspx?CategoryID=1`，顯示屬於 「 飲料 」 分類的 Northwind 資料庫中的名稱、 價格和產品的供應商 （請參閱 圖 11）。 歡迎進一步加強此頁面包含使用者返回類別目錄清單頁面的連結 (`Default.aspx`) 和 DetailsView 或 FormView 控制項，顯示選取的類別 s 的名稱和描述。


[![顯示飲料名稱、 價格和供應商](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**圖 11**： 顯示飲料名稱、 價格和供應商 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>步驟 4： 顯示產品 s 詳細資料

最後一頁， `ProductDetails.aspx`，顯示所選的產品詳細資料。 開啟`ProductDetails.aspx`並從 [工具箱] 拖曳至設計工具拖曳 DetailsView。 設定 DetailsView s`ID`屬性，以`ProductInfo`並清除其`Height`和`Width`屬性值。 從它的智慧標籤，將繫結 DetailsView 至名為新 ObjectDataSource `ProductDataSource`，設定提取其資料從 ObjectDataSource`ProductsBLL`類別的`GetProductByProductID(productID)`方法。 如同步驟 2 和 3 中建立的上一個網頁，設定下拉式清單中更新、 插入和刪除 （無） 索引標籤。


[![設定為使用 GetProductByProductID(productID) 方法的 ObjectDataSource](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**圖 12**： 設定要使用 ObjectDataSource`GetProductByProductID(productID)`方法 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


設定資料來源精靈的最後一個步驟會提示輸入的來源*productID*參數。 由於這項資料是透過查詢字串欄位`ProductID`，下拉式清單設 QueryString 和 ProductID QueryStringField 文字方塊。 最後，按一下 [完成] 按鈕，以完成精靈。


[![設定產品識別碼 ProductID 的 Querystring 欄位從提取其值的參數](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**圖 13**： 設定*productID*提取其值的參數`ProductID`Querystring 欄位 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


完成設定資料來源精靈之後，Visual Studio 會建立對應的 BoundFields 及其在 DetailsView 產品資料欄位中。 移除`ProductID`， `SupplierID`，和`CategoryID`BoundFields 並適當地設定其餘的欄位。 在少數幾個美觀的組態之後, 我 DetailsView 與 ObjectDataSource s 宣告式標記看起來如下所示：


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

若要測試此頁面，請返回`Default.aspx`飲料類別目錄中按一下 檢視產品。 從清單中的飲料產品，請按一下 Chai 茶的 [檢視詳細資料] 連結。 這會帶您前往`ProductDetails.aspx?ProductID=1`，其中顯示 Chai 茶 s （請參閱 圖 14） 的詳細資料。


[![在顯示 Chai 茶 s 供應商、 類別、 價格和其他資訊](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**圖 14**： 顯示 Chai 茶 s 供應商、 類別、 價格和其他資訊 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>步驟 5： 了解網站導覽提供者的內部運作方式

站台對應的集合時，都會在 web 伺服器記憶體`SiteMapNode`形成階層架構的執行個體。 必須有一個根、 非根的所有節點必須都有一個父節點，和所有節點都可能都都有任意數目的子系。 每個`SiteMapNode`物件表示網站的結構中的區段，這些區段通常會有對應的網頁。 因此， [ `SiteMapNode`類別](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)具有屬性，例如`Title`， `Url`，和`Description`，提供資訊區段`SiteMapNode`表示。 另外還有`Key`屬性可唯一識別每個`SiteMapNode`階層，以及用來建立此階層的屬性`ChildNodes`， `ParentNode`， `NextSibling`， `PreviousSibling`，依此類推。

圖 15 會顯示一般的網站導覽結構，從 圖 1，但勾勒出更詳細的實作詳細資料。


[![每個 SiteMapNode 具有屬性，例如標題、 Url、 金鑰和等等](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**圖 15**： 每個`SiteMapNode`具有屬性類似`Title`， `Url`，`Key`等等 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


站台對應是可透過存取[`SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)中[`System.Web`命名空間](https://msdn.microsoft.com/library/system.web.aspx)。 此類別 s`RootNode`屬性會傳回站台對應的根`SiteMapNode`執行個體;`CurrentNode`會傳回`SiteMapNode`其`Url`屬性符合目前所要求之網頁的 URL。 這個類別會在內部使用 ASP.NET 2.0 s 瀏覽 Web 控制項。

當`SiteMap`存取 s 類別屬性，它必須將某些永續性媒體中的網站導覽結構序列化至記憶體。 不過，站台對應序列化邏輯不是硬式編碼成`SiteMap`類別。 相反地，在執行階段`SiteMap`類別會決定哪一個站台地圖*提供者*供序列化使用。 根據預設， [ `XmlSiteMapProvider`類別](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)使用時，它會在站台對應的結構讀取從格式正確之 XML 檔案。 不過，利用最少的工作，我們可以建立自己自訂網站地圖提供者。

所有網站導覽提供者必須都衍生自[`SiteMapProvider`類別](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx)，其中包含重要的方法以及站台所需的內容網站地圖提供者，但省略了許多實作詳細資料。 第二個類別[ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx)，擴充`SiteMapProvider`類別，而且包含的所需的功能較為複雜的實作。 就內部而言，`StaticSiteMapProvider`儲存`SiteMapNode`中對應的站台的執行個體`Hashtable`，並提供方法讓`AddNode(child, parent)`，`RemoveNode(siteMapNode),`並`Clear()`新增和移除`SiteMapNode`以內部`Hashtable`。 `XmlSiteMapProvider` 衍生自 `StaticSiteMapProvider`。

當建立自訂網站地圖提供者擴充`StaticSiteMapProvider`，有兩個抽象方法必須覆寫： [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx)並[ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx)。 `BuildSiteMap`正如其名，會負責從永續性儲存體載入網站導覽結構，並建構在記憶體中。 `GetRootNodeCore` 網站導覽中傳回的根節點。

之前的 web 應用程式可以使用站台對應的提供者，則必須註冊在應用程式的組態。 根據預設，`XmlSiteMapProvider`使用的名稱來註冊類別`AspNetXmlSiteMapProvider`。 若要註冊其他網站導覽提供者，加入下列標記來`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*名稱*值指派的人類看得懂的名稱時，提供者*型別*指定網站導覽提供者的完整類型名稱。 我們將探討的具體值*名稱*並*型別*步驟 7 中的值之後，我們已建立我們自訂網站地圖提供者。

站台對應提供者類別具現化第一次從存取`SiteMap`類別並且在記憶體中的 web 應用程式的存留期的持續狀態。 因為只有一個執行個體的網站導覽提供者可能會從多個並行的網站訪客中叫用時，請務必提供者的方法，是*具備執行緒安全*。

效能和延展性的原因，它對應結構重要，我們會快取於記憶體中的站台，並傳回這個快取的結構，而不是每次重新建立`BuildSiteMap`叫用方法。 `BuildSiteMap` 可能被呼叫多次，每個頁面要求每位使用者，根據使用中的導覽控制項，頁面與網站導覽結構的深度。 在任何情況下，如果我們不要快取中的網站導覽結構`BuildSiteMap`，則每次叫用時我們會需要重新擷取的產品和分類的資訊 （這會導致查詢資料庫） 的架構。 如我們所討論先前快取的教學課程中，快取的資料可能會變成過時。 若要避免此問題，我們可以使用時間-或 SQL 快取相依性為基礎的 expiries。

> [!NOTE]
> 網站導覽提供者可能會選擇性地覆寫[`Initialize`方法](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx)。 `Initialize` 網站導覽提供者會先具現化，並傳遞給提供者中指定任何自訂屬性時，會叫用`Web.config`中`<add>`項目，例如： `<add name="name" type="type" customAttribute="value" />`。 如果您想要允許網頁開發人員指定各種網站地圖提供者相關設定值，而不需要修改提供者 s 程式碼，它很有用。 例如，如果我們已分類和產品的資料直接從資料庫讀取而不透過架構中，我們的 d 可能想要讓網頁開發人員指定資料庫連接字串透過`Web.config`而不是使用硬式編碼提供者 s 程式碼中的值。 步驟 6 中，我們將建立的自訂站台網站導覽提供者不會覆寫這`Initialize`方法。 如需使用的範例`Initialize`方法，請參閱[Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [SQL Server 中儲存網站地圖](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)文章。


## <a name="step-6-creating-the-custom-site-map-provider"></a>步驟 6： 建立自訂網站地圖提供者

若要建立自訂網站地圖提供者，建立站台從對應的類別和 Northwind 資料庫中的產品，我們需要建立該類別可擴充`StaticSiteMapProvider`。 我要求您加入在步驟 1 中`CustomProviders`中的資料夾`App_Code`資料夾-將新類別加入名為此資料夾`NorthwindSiteMapProvider`。 將下列程式碼加入 `NorthwindSiteMapProvider` 類別：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

可讓開始探索這個類別 s s`BuildSiteMap`方法，其開頭[`lock`陳述式](https://msdn.microsoft.com/library/c5kehkcz.aspx)。 `lock`陳述式只允許一個執行緒一次輸入時，藉此序列化程式碼存取，並避免兩個並行執行緒上另一個 s 腳趾逐步執行。

類別層級`SiteMapNode`變數`root`來快取的網站導覽結構。 第一次，或第一次之後已修改基礎資料，建構網站地圖時`root`會`null`而將建構的網站導覽結構。 網站導覽的根節點指派給`root`在建構程序，以便在下一次這個方法呼叫時，`root`不會`null`。 因此，只要`root`不是`null`網站導覽結構會傳回給呼叫端，而不必重新建立它。

如果根目錄是`null`，網站導覽結構會建立從 product 和 category 的資訊。 藉由建立建置網站地圖`SiteMapNode`執行個體，然後再建構透過呼叫階層`StaticSiteMapProvider`類別的`AddNode`方法。 `AddNode` 執行內部簿記，儲存各種`SiteMapNode`執行個體在`Hashtable`。 在開始建構階層之前，我們會先呼叫`Clear`方法，從內部的項目可清除`Hashtable`。 下一步`ProductsBLL`類別 s`GetProducts`方法，並產生`ProductsDataTable`存放於區域變數。

站台對應的建構一開始建立根節點，並將它指派給`root`。 多載[ `SiteMapNode` s 建構函式](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx)使用這裡和整份`BuildSiteMap`傳遞下列資訊：

- 網站導覽提供者的參考 (`this`)。
- `SiteMapNode` s `Key`。 需要這個值必須是唯一的每個`SiteMapNode`。
- `SiteMapNode` s `Url`。 `Url` 是選擇性的但是，如果提供，每個`SiteMapNode`s`Url`值必須是唯一。
- `SiteMapNode` s `Title`，這是必要項。

`AddNode(root)`方法呼叫加入`SiteMapNode``root`以 root 身分執行的站台對應。 接下來，每個`ProductRow`在`ProductsDataTable`列舉。 如果已經有`SiteMapNode`對於目前的產品的類別，它參考。 否則，新`SiteMapNode`是建立類別目錄，並且將其新增為子系`SiteMapNode``root`透過`AddNode(categoryNode, root)`方法呼叫。 在適當的類別之後`SiteMapNode`已找到或建立，節點`SiteMapNode`建立目前產品和分類的子元素加入`SiteMapNode`透過`AddNode(productNode, categoryNode)`。 請注意，類別目錄`SiteMapNode`s`Url`屬性值是`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`而產品`SiteMapNode`s`Url`屬性會被指派`~/SiteMapNode/ProductDetails.aspx?ProductID=productID`。

> [!NOTE]
> 這些資料庫的產品`NULL`值其`CategoryID`類別底下的分組`SiteMapNode`其`Title`屬性設定為 None，且其`Url`屬性設定為空字串。 我決定設`Url`設為空字串，因為`ProductBLL`類別 s`GetProductsByCategory(categoryID)`方法目前缺少要傳回的產品與功能`NULL``CategoryID`值。 此外，我要示範如何瀏覽控制項轉譯`SiteMapNode`缺少的值及其`Url`屬性。 建議您擴充本教學課程，讓無`SiteMapNode`s`Url`屬性會指向`ProductsByCategory.aspx`，還只會顯示與產品`NULL``CategoryID`值。


任意物件新增至資料快取上使用 SQL 快取相依性後建構網站地圖，`Categories`並`Products`透過資料表`AggregateCacheDependency`物件。 我們已在先前的教學課程中，使用 SQL 快取相依性探索*使用 SQL 快取相依性*。 自訂網站地圖提供者，不過，會使用多載的資料快取的`Insert`方法我們 ve 尚未來瀏覽。 這個多載會接受做為其最終的輸入參數物件從快取中移除時呼叫的委派。 具體來說，我們會傳入新[`CacheItemRemovedCallback`委派](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx)，指向`OnSiteMapChanged`方法定義中的進一步向`NorthwindSiteMapProvider`類別。

> [!NOTE]
> 站台對應的記憶體中表示已快取透過類別層級變數`root`。 因為只有一個執行個體的自訂站台對應提供者類別，而且因為所有執行緒之間共用該執行個體中的 web 應用程式時，此類別變數可做為快取中。 `BuildSiteMap`方法也使用的資料快取中，但只能做為基礎資料庫中的資料時收到通知`Categories`或`Products`資料表的變更。 請注意放入資料快取的值是目前的日期和時間。 實際站台對應資料可*不*放在資料快取。


`BuildSiteMap`方法完成所傳回網站地圖的根節點。

其餘的方法都很直接明瞭。 `GetRootNodeCore` 負責傳回的根節點。 由於`BuildSiteMap`傳回的根項目`GetRootNodeCore`只會傳回`BuildSiteMap`s 傳回值。 `OnSiteMapChanged`方法會設定`root`回到`null`快取項目中移除時。 具有根設回`null`、 下一次`BuildSiteMap`是叫用，網站導覽結構將會重建。 最後，`CachedDate`如果值存在，屬性會傳回儲存在資料快取的日期和時間值。 這個屬性可以由頁面開發人員用來決定當網站導覽資料上一次快取。

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>步驟 7： 註冊`NorthwindSiteMapProvider`

在我們的 web 應用程式，才能使用`NorthwindSiteMapProvider`網站導覽提供者在步驟 6 中建立，我們需要向它`<siteMap>`一節`Web.config`。 具體來說，新增下列標記內`<system.web>`中的項目`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

此標記會執行兩項工作： 首先，它會指出內建`AspNetXmlSiteMapProvider`是預設網站導覽提供者; 第二，它會註冊自訂網站地圖提供者以人類易名稱 Northwind 在步驟 6 中建立。

> [!NOTE]
> 網站導覽提供者中之應用程式位於`App_Code`資料夾中的值`type`屬性就是類別名稱。 或者，自訂網站地圖提供者可建立個別的類別庫專案中編譯的組件放在 web 應用程式與`/Bin`目錄。 在此情況下，`type`屬性值會是*命名空間*。*ClassName*， *AssemblyName* 。


在更新之後`Web.config`，花一點時間瀏覽器中檢視任何頁面上，從教學課程。 請注意，在左側瀏覽介面仍然顯示的區段，而且教學課程中所定義`Web.sitemap`。 這是因為我們保留`AspNetXmlSiteMapProvider`做為預設提供者。 若要建立會使用瀏覽使用者介面項目`NorthwindSiteMapProvider`，我們需要明確指定，應該使用 Northwind 網站導覽提供者。 我們會看到如何完成這項作業在步驟 8 中。

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>步驟 8： 顯示使用自訂網站地圖提供者的站台對應資訊

使用自訂網站的網站導覽提供者建立並登錄中`Web.config`，我們要加入導覽控制項，以準備好`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`中的分頁`SiteMapProvider`資料夾。 首先開啟`Default.aspx`頁面上，並將拖曳`SiteMapPath`從 [工具箱] 拖曳至設計工具。 SiteMapPath 控制項位於 [工具箱] 的 [導覽] 區段中。


[![新增 SiteMapPath default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**圖 16**： 新增至 SiteMapPath `Default.aspx` ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


SiteMapPath 控制項顯示階層連結列，指出目前的網頁的位置內的站台對應。 我們新增至主版頁面頂端的 SiteMapPath 年代*主版頁面與網站導覽*教學課程。

請花一點時間才能檢視此頁面，透過瀏覽器。 圖 16 中新增 SiteMapPath 會使用預設網站導覽提供者，將從其資料提取`Web.sitemap`。 因此，階層連結顯示首頁&gt;自訂站台對應，就像在右上角階層連結。


[![階層連結會使用預設網站導覽提供者](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**圖 17**： 階層連結會使用預設的網站導覽提供者 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


若要使用我們在步驟 6 中建立的自訂站台網站導覽提供者 SiteMapPath 加入 [圖 16] 中，設定其[`SiteMapProvider`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx)至 Northwind，名稱我們指派給`NorthwindSiteMapProvider`在`Web.config`。 不幸的是，設計工具會繼續使用預設網站導覽提供者，但如果請瀏覽透過瀏覽器頁面進行這項屬性變更後您會看到階層連結現在會使用自訂網站地圖提供者。


[![階層連結現在會使用自訂站台對應提供者 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**圖 18**： 階層連結現在會使用自訂網站地圖提供者`NorthwindSiteMapProvider`([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


SiteMapPath 控制項顯示在功能較多的使用者介面`ProductsByCategory.aspx`和`ProductDetails.aspx`頁面。 這些頁面，設定加入 SiteMapPath`SiteMapProvider`中兩者設定為 [Northwind] 屬性。 從`Default.aspx`按一下飲料，檢視產品連結，然後 Chai 茶的 [檢視詳細資料] 連結。 階層連結如圖 19 所示，包含目前的站台對應區段 （Chai 茶） 和其上階： Beverages 和所有類別目錄。


[![階層連結現在會使用自訂站台對應提供者 NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**圖 19**： 階層連結現在會使用自訂網站地圖提供者`NorthwindSiteMapProvider`([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


除了 SiteMapPath，例如功能表與 TreeView 控制項可使用其他瀏覽使用者介面項目。 `Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面下載此教學課程中，例如，在所有包含功能表控制項 （請參閱圖 20）。 請參閱[檢查 ASP.NET 2.0 s 站台導覽功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)並[使用的網站巡覽控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx)一節[ASP.NET 2.0 快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/)的更深入的了解導覽控制項和 ASP.NET 2.0 中的站台對應系統。


[![功能表控制項列出每個類別和產品](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**圖 20**: 功能表控制項列出每個類別目錄和產品的 ([按一下以檢視完整大小的影像](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


如稍早在本教學課程中所述，可以透過程式設計的方式存取網站導覽結構`SiteMap`類別。 下列程式碼會傳回根`SiteMapNode`預設提供者：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

由於`AspNetXmlSiteMapProvider`是預設提供者的應用程式中，上述程式碼會傳回中所定義的根節點`Web.sitemap`。 若要參考網站導覽提供者的預設值以外，使用`SiteMap`類別 s [ `Providers`屬性](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx)就像這樣：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

何處*名稱*自訂網站地圖提供者 (Northwind，我們的 web 應用程式) 的名稱。

若要存取的網站導覽提供者的特定成員，請使用`SiteMap.Providers["name"]`擷取提供者執行個體，然後再將它轉換成適當的類型。 例如，若要顯示`NorthwindSiteMapProvider`s`CachedDate`屬性在 ASP.NET 頁面中，使用下列程式碼：


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> 請務必測試 SQL 快取相依性功能。 之後造訪`Default.aspx`， `ProductsByCategory.aspx`，和`ProductDetails.aspx`頁面，請移至其中一個編輯、 插入及刪除一節的教學課程，以及編輯分類或產品的名稱。 然後返回其中一個在頁面`SiteMapProvider`資料夾。 假設要注意到基礎資料庫變更的輪詢機制有充裕的時間，站台對應應該更新以顯示新的產品或分類名稱。


## <a name="summary"></a>總結

ASP.NET 2.0 s 站台對應功能包括`SiteMap`類別，幾個內建的瀏覽網頁的控制項，與預設網站導覽提供者預期網站導覽資訊保存到 XML 檔案。 若要使用來自其他來源例如資料庫、 應用程式的架構，或遠端的 Web 服務，我們需要建立自訂網站地圖提供者的網站地圖資訊。 這牽涉到建立直接或間接衍生的類別從`SiteMapProvider`類別。

在本教學課程中，我們了解如何建立自訂網站地圖提供者，根據從應用程式架構的產品和分類資訊中的站台對應的內容。 我們提供者的擴充`StaticSiteMapProvider`類別，並產生建立`BuildSiteMap`方法擷取資料，建構網站導覽階層架構，並快取所產生的結構，在類別層級變數。 我們要使其失效的快取使用的回呼函式的 SQL 快取相依性結構時的基礎`Categories`或`Products`資料修改。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 SQL Server 中儲存網站地圖](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)和[您 ve 已久 SQL 網站導覽提供者](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [了解 ASP.NET 2.0 s 提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [提供者工具組](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [檢查 ASP.NET 2.0 的網站瀏覽功能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dave Gardner、 Zack Jones、 Teresa Murphy 和 Bernadette Leigh。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](building-a-custom-database-driven-site-map-provider-vb.md)
