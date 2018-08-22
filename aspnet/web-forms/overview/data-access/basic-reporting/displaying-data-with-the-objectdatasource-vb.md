---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: 顯示與 ObjectDataSource (VB) 的資料 |Microsoft Docs
author: rick-anderson
description: 本教學課程會探討使用這個控制項，您可以將擷取自 BLL havi 沒有先前的教學課程中建立資料繫結的 ObjectDataSource 控制項...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: f49cbf19b090917c170b025c269f825cf486c31a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824760"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>顯示資料與 ObjectDataSource (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe)或[下載 PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> 本教學課程會探討 ObjectDataSource 控制項使用這個控制項，您可以繫結從先前的教學課程中建立，而不需要撰寫一行程式碼，BLL 擷取的資料 ！


## <a name="introduction"></a>簡介

使用我們的應用程式架構和網站頁面版面配置完成，我們已準備好開始探索如何完成各種常見的資料和報告相關的工作項目。 在先前的教學課程中，我們已了解如何以程式設計方式從 BLL 和 DAL 的資料繫結至 ASP.NET 網頁中的 Web 控制項的資料。 此指派資料 Web 控制項的語法`DataSource`屬性設為資料顯示，並接著呼叫控制項的`DataBind()`方法是在 ASP.NET 1.x 應用程式所使用的模式，而且可以繼續在 2.0 的應用程式。 不過，ASP.NET 2.0 的新資料來源控制項提供宣告的方式來處理資料。 使用這些控制項可以繫結從先前的教學課程中建立，而不需要撰寫一行程式碼，BLL 擷取的資料 ！

ASP.NET 2.0 隨附於五個內建的資料來源控制項[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)， [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)，並[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)雖然可以建立自己[自訂資料來源控制項](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)，如有需要。 因為我們已在我們的教學課程應用程式開發架構，我們將針對我們的 BLL 類別使用 ObjectDataSource。


![ASP.NET 2.0 包含五個內建的資料來源控制項](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**圖 1**: ASP.NET 2.0 包含五個內建的資料來源控制項


ObjectDataSource 會做為其他物件所使用的 proxy。 若要設定 ObjectDataSource 我們指定此基礎物件和其方法如何對應至的 ObjectDataSource `Select`， `Insert`， `Update`，和`Delete`方法。 一旦已指定此基礎物件，其方法對應到 ObjectDataSource 的我們可以為資料 Web 控制項的方式來然後繫結 ObjectDataSource。 ASP.NET 隨附了許多資料 Web 控制項，包括 GridView、 DetailsView、 RadioButtonList 和下拉式清單中，其他項目。 在頁面生命週期中，資料 Web 控制項可能需要存取的資料繫結，它將會完成藉由叫用其 ObjectDataSource`Select`方法; 如果資料 Web 控制項支援插入、 更新或刪除，可能會呼叫其ObjectDataSource 的`Insert`， `Update`，或`Delete`方法。 這些呼叫路由傳送至適當的基礎物件的方法的 ObjectDataSource 的下列圖所示。


[![ObjectDataSource 做為 Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**圖 2**: ObjectDataSource 做為 Proxy ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


ObjectDataSource 可以用來叫用方法來插入、 更新或刪除資料，讓我們把重點放在傳回的資料;未來的教學課程將探討使用 ObjectDataSource 和資料修改資料的 Web 控制項。

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>步驟 1： 加入和設定 ObjectDataSource 控制項

首先開啟`SimpleDisplay.aspx`頁面中`BasicReporting`資料夾，切換至 [設計] 檢視中，然後再從 [工具箱] 拖曳至頁面的設計介面拖曳 ObjectDataSource 控制項。 ObjectDataSource 會顯示為設計介面上的灰色方塊，因為它不會產生任何標記，它只會透過叫用方法，以從指定的物件存取資料。 ObjectDataSource 所傳回的資料可以顯示的資料 Web 控制項，例如 GridView、 DetailsView、 FormView 和等等。

> [!NOTE]
> 或者，您可能會先將資料 Web 控制項新增至頁面並再從它的智慧標籤，選擇 &lt;新的資料來源&gt;從下拉式清單中的選項。


若要指定 ObjectDataSource 的基礎物件和該物件的方法如何對應到 ObjectDataSource 的按一下 從 ObjectDataSource 的智慧標籤的 設定資料來源 連結。


[![按一下 設定從智慧標籤的資料來源連結](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**圖 3**： 按一下從智慧標籤設定資料來源連結 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


這會顯示 [設定資料來源精靈]。 首先，我們必須指定要使用 ObjectDataSource 的物件。 如果勾選 [顯示僅將資料的元件] 核取方塊，則下拉式清單中的，在此畫面上只會列出這些物件，已附有`DataObject`屬性。 目前我們的清單會包含 Tableadapter 中具類型資料集和我們在上一個教學課程中建立的 BLL 類別。 如果您忘記新增`DataObject`屬性商業邏輯層類別您不會看到這份清單中它們。 若要檢視所有物件，其中應包含 （以及其他類別中具類型資料集的 Datatable、 Datarow，等等） 的 BLL 類別，在此情況下，取消選取的 [顯示僅將資料的元件] 核取方塊。

在此第一個畫面中選擇`ProductsBLL`類別從下拉式清單中，按一下 [下一步]。


[![使用 ObjectDataSource 控制項來指定要使用的物件](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**圖 4**： 指定要使用的物件與 ObjectDataSource 控制項 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


在精靈中的下一個畫面會提示您選取 ObjectDataSource 應叫用哪一種方法。 下拉式清單會列出這些方法，從上一個畫面中選取之物件中傳回的資料。 我們在這裡看到`GetProductByProductID`， `GetProducts`， `GetProductsByCategoryID`，和`GetProductsBySupplierID`。 選取 `GetProducts`方法，從下拉式清單按一下 完成 (如果您已新增`DataObjectMethodAttribute`到`ProductBLL`的預設會選取 上一個教學課程中，此選項中所示的方法)。


[![選擇方法來傳回資料，從 [選取] 索引標籤](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**圖 5**： 選擇的方法傳回的資料，從 [選取] 索引標籤 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>以手動方式設定 ObjectDataSource

ObjectDataSource 的設定資料來源精靈 提供快速的方法來指定它所使用的物件，以及建立關聯物件的哪些方法會叫用。 不過，您就可以設定其屬性，透過 [屬性] 視窗，或是直接在宣告式標記中透過 ObjectDataSource。 只要設定`TypeName`屬性為基礎的物件，要使用的型別和`SelectMethod`擷取資料時要叫用方法。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

即使您偏好設定資料來源精靈可能會有您需要以手動方式設定 ObjectDataSource 精靈只列出開發人員建立的類別。 如果您想要這類將 ObjectDataSource 繫結至.NET Framework 中的類別[Membership 類別](https://msdn.microsoft.com/library/system.web.security.membership.aspx)，以存取使用者帳戶資訊，或有[Directory 類別](https://msdn.microsoft.com/library/system.io.directory.aspx)才能使用檔案系統資訊您必須以手動方式設定 ObjectDataSource 的屬性。

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>步驟 2： 加入資料 Web 控制項及繫結至 ObjectDataSource

一旦已新增至頁面並設定 ObjectDataSource，我們已經準備好資料 Web 控制項加入頁面，即可顯示 ObjectDataSource 的所傳回的資料`Select`方法。 任何資料 Web 控制項可以繫結至 ObjectDataSource;讓我們看看在 GridView、 DetailsView 和 FormView 中顯示 ObjectDataSource 的資料。

## <a name="binding-a-gridview-to-the-objectdatasource"></a>繫結至 ObjectDataSource 的 GridView

新增 GridView 控制項從工具箱拖曳至`SimpleDisplay.aspx`的設計介面。 從 GridView 的智慧標籤上，選擇我們在步驟 1 中新增的 ObjectDataSource 控制項。 這會自動在每個屬性傳回的資料從 ObjectDataSource 的 gridview 裡建立 BoundField`Select`方法 （也就是產品 DataTable 所定義的屬性）。


[![已新增至頁面的 GridView 和繫結到 ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**圖 6**: GridView 已新增至頁面，並繫結至的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


您可以再自訂、 重新排列，或移除 GridView 的 BoundFields，即可從智慧標籤的 編輯資料行 選項。


[![管理透過 [編輯資料行] 對話方塊中的 GridView 的 BoundFields](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**圖 7**： 管理 GridView 的 BoundFields 透過編輯資料行對話方塊 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


請花一點時間修改 GridView 的 BoundFields，移除`ProductID`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields。 只要 BoundField 從清單中選取左下角，然後按一下 [刪除] 按鈕 (紅色 X) 將它們移除。 接下來，重新排列 BoundFields 以便`CategoryName`並`SupplierName`BoundFields 前面`UnitPrice`BoundField 選取這些 BoundFields，然後按一下向上箭號。 設定`HeaderText`屬性到其餘的 BoundFields `Products`， `Category`， `Supplier`，和`Price`分別。 接下來，讓`Price`BoundField 格式化為貨幣，藉由設定 BoundField`HtmlEncode`屬性設定為 False 而且其`DataFormatString`屬性設`{0:c}`。 最後，水平對齊`Price`右邊的`Discontinued`核取方塊，在透過管理中心`ItemStyle` / `HorizontalAlign`屬性。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![GridView 的 BoundFields 已經自訂](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**圖 8**: GridView 已經自訂 BoundFields ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>使用一致的外觀的佈景主題

這些教學課程盡量以移除任何控制層級樣式設定，改為使用階層式樣式表盡可能與外部檔案中定義。 `Styles.css`檔案包含`DataWebControlStyle`， `HeaderStyle`， `RowStyle`，和`AlternatingRowStyle`應該用來決定資料的外觀的 CSS 類別 Web 這些教學課程中使用的控制項。 若要這麼做，我們可以設定 GridView 的`CssClass`屬性，以`DataWebControlStyle`，並將其`HeaderStyle`， `RowStyle`，並`AlternatingRowStyle`屬性`CssClass`屬性據以。

如果我們將這些`CssClass`Web 控制項，我們需要明確地設定這些屬性值，每個資料，請記得在屬性 Web 控制項新增至我們的教學課程。 更容易管理的方法是定義的預設 CSS 相關屬性，如 GridView、 DetailsView 和 FormView 控制項使用的佈景主題。 主題是控制層級屬性設定、 影像和跨站台以強制執行常見的外觀及操作可以套用到頁面的 CSS 類別的集合。

我們的主題將不會包含任何影像或 CSS 檔案 (我們要將樣式表`Styles.css`作為-web 應用程式的根資料夾中，定義)，但會包括兩個面板。 面板是一個檔案，定義 Web 控制項的預設屬性。 具體來說，我們必須面板檔案的 GridView 和 DetailsView 控制項中，預設值，指出`CssClass`-相關的屬性。

將新的面板檔案新增至名為您專案啟動`GridView.skin`方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇 加入新項目。


[![新增名為 GridView.skin 面板檔案](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**圖 9**： 新增面板檔案命名為`GridView.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


面板檔案必須放在佈景主題，其位於`App_Themes`資料夾。 因為我們還沒有這類資料夾，Visual Studio 將請提供建立一個讓我們加入我們的第一個面板時。 按一下 [是] 以建立`App_Theme`資料夾，並將新`GridView.skin`那里檔案。


[![讓 Visual Studio 建立 App_Theme 資料夾](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**圖 10**： 讓 Visual Studio 建立`App_Theme`資料夾 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


這會建立新的佈景主題中`App_Themes`名 GridView 為面板檔案與資料夾`GridView.skin`。


![GridView 佈景主題 App_Theme 資料夾具有 已新增](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**圖 11**: GridView 佈景主題可讓您有已加入至`App_Theme`資料夾


重新命名為 DataWebControls 的 GridView 佈景主題 (以滑鼠右鍵按一下 GridView 資料夾中`App_Theme`資料夾，然後選擇 重新命名)。 接下來，輸入下列標記成`GridView.skin`檔案：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

這會定義的預設屬性`CssClass`-相關的任何頁面，使用 DataWebControls 佈景主題中的任何 GridView 的屬性。 DetailsView，我們會短時間內使用的 Web 控制項的資料，讓我們新增另一個面板。 將新的面板加入至名為 DataWebControls 主題`DetailsView.skin`並新增下列標記：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

使用我們定義的佈景主題，最後一個步驟是將佈景主題套用至我們的 ASP.NET 頁面。 頁面的頁面為基礎或網站中的所有頁面，可以套用佈景主題。 在網站中的所有頁面，讓我們使用此佈景主題。 若要這麼做，加入下列標記，即可`Web.config`的`<system.web>`區段：


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

這樣就完成了 ！ `styleSheetTheme`值表示指定佈景主題中的屬性應該*不*覆寫控制層級上指定的屬性。 若要指定佈景主題設定應該也能造就控制設定，請使用`theme`屬性取代`styleSheetTheme`; 不幸的是，在 Visual Studio 設計檢視中看不到主題設定。 請參閱[ASP.NET 佈景主題和面板概觀](https://msdn.microsoft.com/library/ykzx33wh.aspx)並[伺服器端樣式使用佈景主題](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)如需主題和面板; 詳細資訊，請參閱[How To： 適用於 ASP.NET 佈景主題](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx)如需詳細資訊設定要使用佈景主題的頁面。


[![GridView 會顯示產品的名稱、 類別、 供應商、 價格及已停用的資訊](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**圖 12**: GridView 會顯示產品的名稱、 類別、 供應商、 價格和已停止的資訊 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>在 DetailsView 中一次顯示一筆記錄

GridView 會顯示傳回的資料來源控制項所繫結每一筆記錄的一個資料列。 有些的時候，不過，當我們可能會想要一次顯示唯一的記錄或只是一筆記錄。 [DetailsView 控制項](https://msdn.microsoft.com/library/s3w1w7t4.aspx)提供這項功能，轉譯為 HTML`<table>`兩個資料行與每個資料行或屬性繫結至控制項的一個資料列。 您可以想像的 DetailsView GridView，以使用單一記錄旋轉 90 度。

開始加入的 DetailsView 控制項*上面*在 GridView `SimpleDisplay.aspx`。 接下來，繫結至 GridView 為相同的 ObjectDataSource 控制項。 使用 GridView，BoundField 會新增到 ObjectDataSource 的所傳回的物件中每一個屬性 DetailsView 像`Select`方法。 唯一的差別在於，DetailsView BoundFields 配置是水平方式而不是以垂直方式。


[![加入至網頁的 DetailsView，並將它繫結到 ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**圖 13**： 加入至網頁的 DetailsView 和繫結至的 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


例如 GridView、 DetailsView BoundFields 可以調整才能挑提供更客製化的 ObjectDataSource 所傳回的資料顯示。 [圖 14] 顯示 DetailsView 之後其 BoundFields 和`CssClass`屬性已設定，使其外觀類似於 GridView 範例。


[![DetailsView 顯示單一記錄](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**圖 14**: DetailsView 顯示單一記錄 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


請注意，DetailsView 只會顯示其資料來源傳回的第一筆記錄。 若要讓使用者逐步執行的所有記錄，一次，我們必須啟用 DetailsView 的分頁。 若要這樣做，請返回 Visual Studio 並檢查 DetailsView 的智慧標籤的 啟用分頁核取方塊。


[![啟用在 DetailsView 控制項中的分頁](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**圖 15**： 在 DetailsView 控制項中啟用分頁 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![啟用分頁，DetailsView 可讓使用者檢視的任何產品](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**圖 16**: DetailsView 分頁已啟用，允許使用者檢視的任何產品 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


我們會討論更多關於分頁在未來的教學課程。

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>一次顯示一筆記錄為更有彈性的版面配置

DetailsView 是很固定在傳回的 ObjectDataSource 每一筆記錄的顯示方式。 我們可能會想要更有彈性的資料檢視。 比方說，而不是在個別的資料列上顯示的產品名稱、 類別、 供應商、 價格及已停用的資訊，我們可能會想要顯示產品名稱，並在價格`<h4>`標題時，出現的 category 與 supplier 資訊以下的名稱和價格中較小的字型。 此外，我們不會注意到顯示的值旁的屬性名稱 （產品類別目錄等等）。

[FormView 控制項](https://msdn.microsoft.com/library/fyf1dk77.aspx)提供此層級的自訂。 而不是使用欄位 （例如 GridView 和 DetailsView 執行），還會使用 FormView 使用範本，可讓 Web 控制項，靜態 HTML，混用及[進行資料繫結語法](http://www.15seconds.com/issue/040630.htm)。 如果您熟悉 Repeater 控制項從 ASP.NET 1.x 中，您可以將 FormView 視為重複項顯示單一記錄。

加入 FormView 控制項來`SimpleDisplay.aspx`頁面的設計介面。 一開始 FormView 會顯示為灰色區塊，通知我們，我們需要最小值，提供控制項的`ItemTemplate`。


[![FormView 必須包含一個 ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**圖 17**： 必須包含 FormView `ItemTemplate` ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


您可以直接以透過 FormView 的智慧標籤，這將會建立預設值的資料來源控制項繫結 FormView`ItemTemplate`自動 (連同`EditItemTemplate`並`InsertItemTemplate`，如果 ObjectDatatSource 控制項`InsertMethod`和`UpdateMethod`屬性會設定)。 不過，此範例中我們將資料繫結至 FormView，並指定其`ItemTemplate`以手動方式。 先是設定 FormView`DataSourceID`屬性，以`ID`的 ObjectDataSource 控制項， `ObjectDataSource1`。 接下來，建立`ItemTemplate`，以顯示產品的名稱和價格`<h4>`項目和類別目錄] 和 [託運商名稱下方，以較小的字型大小。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![第一項產品 (Chai) 會顯示在自訂格式](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**圖 18**: 第一項產品 (Chai) 會顯示在自訂格式 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>`是資料繫結語法。 `Eval`方法會傳回目前繫結至 FormView 控制項的物件所指定屬性的值。 請參閱 Alex Homer 文[簡體和擴充資料繫結語法在 ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)如需資料繫結的優缺點。

DetailsView，像 FormView 只會顯示從 ObjectDataSource 傳回第一筆記錄。 您可以啟用以允許逐步產品一次的訪客 FormView 中的分頁。

## <a name="summary"></a>總結

而不需要撰寫一行程式碼，多虧 ASP.NET 2.0 的 ObjectDataSource 控制項即可存取和顯示資料的商務邏輯層。 ObjectDataSource 會叫用類別的指定的方法，並傳回結果。 這些結果可以顯示在資料繫結至 ObjectDataSource 的 Web 控制項。 在本教學課程中我們探討了 GridView、 DetailsView 和 FormView 控制項繫結到 ObjectDataSource。

到目前為止我們只看到如何使用 ObjectDataSource 叫用的無參數方法，但如果我們要叫用方法預期的輸入參數，例如`ProductBLL`類別的`GetProductsByCategoryID(categoryID)`嗎？ 若要呼叫的方法，必須要有一或多個參數中，我們必須設定 ObjectDataSource 來指定這些參數的值。 我們會看到如何完成這在我們[下一個教學課程](declarative-parameters-vb.md)。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建立您自己的資料來源控制項](https://msdn.microsoft.com/library/ms364049.aspx)
- [Asp.net 2.0 GridView 範例](https://msdn.microsoft.com/library/aa479339.aspx)
- [簡化並擴充繫結語法，在 ASP.NET 2.0 中的資料](http://www.15seconds.com/issue/040630.htm)
- [ASP.NET 2.0 中的佈景主題](http://www.odetocode.com/Articles/423.aspx)
- [伺服器端樣式使用佈景主題](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [如何： 以程式設計方式套用 ASP.NET 佈景主題](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [下一頁](declarative-parameters-vb.md)
