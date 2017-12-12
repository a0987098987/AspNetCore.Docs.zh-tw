---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: "利用 ObjectDataSource (VB) 顯示資料 |Microsoft 文件"
author: rick-anderson
description: "本教學課程會使用這個控制項，您可以將擷取自 BLL havi 沒有先前的教學課程中建立資料繫結 ObjectDataSource 控制項查看..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: d575c8f597bcb5d2a5d2e27e1145d39110daabe1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>利用 ObjectDataSource (VB) 顯示資料
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe)或[下載 PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> 本教學課程會使用這個控制項，您可以將擷取自 BLL 而不需要撰寫一行程式碼在上一個教學課程中建立資料繫結 ObjectDataSource 控制項查看 ！


## <a name="introduction"></a>簡介

我們的應用程式架構和網站頁面版面配置完成，我們就可以開始探索如何完成各種不同的資料和報告相關的一般工作。 在先前的教學課程中，我們已經看到如何以程式設計方式從 DAL 和 BLL 的資料繫結至資料之 ASP.NET 網頁中的 Web 控制項。 此語法的資料 Web 控制項指派`DataSource`屬性顯示，然後再呼叫控制項的資料`DataBind()`方法是使用 ASP.NET 1.x 應用程式的模式，而且可以繼續在 2.0 應用程式中使用。 不過，ASP.NET 2.0 的新資料來源控制項提供宣告的方式，來處理資料。 使用這些控制項可以繫結從上一個教學課程中建立，而不需要撰寫一行程式碼 BLL 擷取的資料 ！

ASP.NET 2.0 隨附於五個內建資料來源控制項[SqlDataSource](https://msdn.microsoft.com/en-us/library/dz12d98w%28vs.80%29.aspx)， [AccessDataSource](https://msdn.microsoft.com/en-us/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/en-us/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/en-us/library/e8d8587a%28en-US,VS.80%29.aspx)，和[Treeview](https://msdn.microsoft.com/en-us/library/5ex9t96x%28en-US,VS.80%29.aspx)雖然您可以建立您自己[自訂資料來源控制項](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnvs05/html/DataSourceCon1.asp)有需要。 因為我們已經教學課程應用程式開發架構，我們將使用 ObjectDataSource 針對我們 BLL 的類別。


![ASP.NET 2.0 包含五個內建資料來源控制項](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**圖 1**: ASP.NET 2.0 包含五個內建資料來源控制項


ObjectDataSource 會做為其他物件所使用的 proxy。 若要設定 ObjectDataSource 我們指定這基礎物件和其方法如何對應至 ObjectDataSource `Select`， `Insert`， `Update`，和`Delete`方法。 一旦指定了這個基礎物件，其方法對應到 ObjectDataSource 我們可以再將繫結 ObjectDataSource Web 控制項的資料。 ASP.NET 隨附許多資料 Web 控制項，包括 GridView、 DetailsView、 RadioButtonList 和 DropDownList，和其他項目。 在頁面生命週期中，Web 控制項的資料可能需要存取的資料繫結，它將會完成藉由叫用其 ObjectDataSource`Select`方法; 如果資料 Web 控制項支援插入、 更新或刪除，呼叫可能會對其ObjectDataSource 的`Insert`， `Update`，或`Delete`方法。 這些呼叫適當的基礎物件的方法 objectdatasource 然後路由傳送，如下列圖表所示。


[![ObjectDataSource 做為 Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**圖 2**: ObjectDataSource 做為 Proxy ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


ObjectDataSource 可以用來叫用插入方法，更新或刪除資料，讓我們只著重於傳回的資料;未來的教學課程將會探索使用 ObjectDataSource 和資料修改資料的 Web 控制項。

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>步驟 1： 加入和設定 ObjectDataSource 控制項

先開啟`SimpleDisplay.aspx`頁面`BasicReporting`資料夾，切換至 [設計] 檢視中，然後再拖曳 ObjectDataSource 控制項從 [工具箱] 拖曳至頁面的設計介面。 ObjectDataSource 會顯示為設計介面上的灰色方塊，因為它不會產生任何標記。它只會叫用方法，從指定的物件存取的資料。 ObjectDataSource 所傳回的資料可以顯示資料的 Web 控制項，例如 GridView、 DetailsView、 FormView，等等。

> [!NOTE]
> 或者，您可能會先加入網頁中的 Web 控制項的資料，然後從其智慧標籤上，選擇&lt;新的資料來源&gt;從下拉式清單選項。


若要指定 ObjectDataSource 基礎物件和該物件的方法如何對應至 ObjectDataSource，按一下 ObjectDataSource 的智慧標籤的設定資料來源連結。


[![按一下 設定從智慧標籤的資料來源連結](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**圖 3**： 按一下從智慧標籤設定資料來源連結 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


這會開啟設定資料來源精靈。 首先，我們必須指定 ObjectDataSource 所使用的物件。 如果 「 僅顯示資料元件 」 核取方塊，下拉式清單，在這個畫面上的會列出只有在具有使用已裝飾的物件`DataObject`屬性。 目前我們清單會包含 Tableadapter 中具類型資料集和 BLL 類別上一個教學課程中建立。 如果您忘記新增`DataObject`屬性為商務邏輯層類別就看它們這份清單中。 在此情況下，取消 「 僅顯示資料元件 」 的核取方塊選取要檢視所有物件，其中應包含 BLL 類別 （以及其他類別型別資料集的 Datatable、 資料行，依此類推）。

從這個第一個畫面中，選擇`ProductsBLL`類別從下拉式清單，然後按一下 [下一步]。


[![指定要使用的物件與 ObjectDataSource 控制項](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**圖 4**: ObjectDataSource 控制項以指定要使用物件 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


在精靈中的下一個畫面會提示您選取 ObjectDataSource 應叫用哪一種方法。 下拉式清單會列出這些方法，從上一個畫面選取的物件中傳回的資料。 我們在這裡看到`GetProductByProductID`， `GetProducts`， `GetProductsByCategoryID`，和`GetProductsBySupplierID`。 選取`GetProducts`方法，從下拉式清單，然後按一下 [完成] (如果您加入`DataObjectMethodAttribute`至`ProductBLL`的預設會選取上一個教學課程中，此選項中所示的方法)。


[![選擇方法來傳回資料，從 [選取] 索引標籤](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**圖 5**： 選擇的方法傳回的資料，從 [選取] 索引標籤 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>手動設定 ObjectDataSource

ObjectDataSource 設定資料來源精靈提供快速的方式來指定它使用的物件，以及建立關聯物件的哪些方法會叫用。 不過，您就可以設定其屬性，透過 [屬性] 視窗，或是直接在宣告式標記中透過 ObjectDataSource。 只要設定`TypeName`屬性来使用的基礎物件的型別和`SelectMethod`來擷取資料時叫用的方法。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

即使您偏好設定資料來源精靈，可能是您需要手動設定 [ObjectDataSource]，因為精靈只列出開發人員建立的類別。 如果您想要這類將 ObjectDataSource 繫結至.NET Framework 中的類別[成員資格類別](https://msdn.microsoft.com/en-us/library/system.web.security.membership.aspx)、 存取使用者帳戶資訊，或[目錄類別](https://msdn.microsoft.com/en-us/library/system.io.directory.aspx)才能使用檔案系統資訊您必須手動設定 ObjectDataSource 的屬性。

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>步驟 2： 加入資料 Web 控制項並繫結至 ObjectDataSource

一旦 ObjectDataSource 已加入至頁面，並設定，我們將資料的 Web 控制項加入至頁面，即可顯示 ObjectDataSource 所傳回的資料準備`Select`方法。 可以繫結至 ObjectDataSource; 的任何資料的 Web 控制項讓我們看看 GridView、 DetailsView 和 FormView 中顯示 ObjectDataSource 的資料。

## <a name="binding-a-gridview-to-the-objectdatasource"></a>繫結至 ObjectDataSource 的 GridView

將 GridView 控制項從工具箱拖曳至`SimpleDisplay.aspx`的設計介面。 從 GridView 的智慧標籤上，選擇 ObjectDataSource 控制項，我們加入在步驟 1 中。 這會在每一個屬性傳回的資料從 ObjectDataSource GridView 中自動建立 BoundField`Select`方法 （也就是產品資料表所定義的內容）。


[![在 GridView 加入網頁和繫結至 ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**圖 6**: GridView 已加入至頁面，並繫結至 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


您可以再自訂、 重新排列，或移除 GridView BoundFields 按一下智慧標籤的 編輯資料行選項。


[![管理 GridView BoundFields 透過編輯資料行對話方塊](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**圖 7**： 管理 GridView BoundFields 透過編輯資料行對話方塊 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


請花一點時間修改 GridView BoundFields，移除`ProductID`， `SupplierID`， `CategoryID`， `QuantityPerUnit`， `UnitsInStock`， `UnitsOnOrder`，和`ReorderLevel`BoundFields。 直接選取 BoundField 左下角中的清單，並按一下 [刪除] 按鈕 (紅色 X) 將它們移除。 接下來，重新排列 BoundFields 以便`CategoryName`和`SupplierName`BoundFields 前面`UnitPrice`BoundField 選取這些 BoundFields 並按一下向上箭號。 設定`HeaderText`屬性至剩餘的 BoundFields `Products`， `Category`， `Supplier`，和`Price`分別。 接下來，讓`Price`BoundField 格式化為貨幣，藉由設定 BoundField`HtmlEncode`屬性設定為 False 且其`DataFormatString`屬性`{0:c}`。 最後，水平對齊`Price`右邊和`Discontinued`核取方塊，在透過中央`ItemStyle` / `HorizontalAlign`屬性。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![已自訂的 GridView BoundFields](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**圖 8**: GridView 已自訂 BoundFields ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>使用佈景主題的一致的外觀

這些教學課程盡可能移除的任何樣式控制層級設定，而非使用階層式樣式表定義在外部檔案盡可能。 `Styles.css`檔案包含`DataWebControlStyle`， `HeaderStyle`， `RowStyle`，和`AlternatingRowStyle`應該用來指定資料的外觀的 CSS 類別 Web 這些教學課程中使用的控制項。 若要達成此目的，我們無法設定 GridView`CssClass`屬性`DataWebControlStyle`，及其`HeaderStyle`， `RowStyle`，和`AlternatingRowStyle`屬性`CssClass`屬性據此。

如果我們將這些`CssClass`Web 控制項，我們需要明確設定這些屬性值，每個資料時，請記得在屬性 Web 控制項加入至教學課程。 更容易管理的方法是定義預設 CSS 相關屬性的 GridView，DetailsView，並在 FormView 可讓您控制使用佈景主題。 主題是控制層級屬性設定、 影像和強制執行通用的外觀及操作的站台可以套用到頁面的 CSS 類別的集合。

我們的佈景主題不會包含任何影像或 CSS 檔案 (我們將會保留在樣式表`Styles.css`為-web 應用程式的根資料夾中，定義)，但是會包含兩個面板。 面板是用以定義 Web 控制項的預設屬性的檔案。 具體來說，我們必須面板檔案的 GridView 和 DetailsView 控制項，預設值，指出`CssClass`-相關的屬性。

將新面板檔案新增至名為專案啟動`GridView.skin`方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇 加入新項目。


[![加入名為 GridView.skin 面板檔案](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**圖 9**： 新增面板檔案命名為`GridView.skin`([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


面板檔案必須放在佈景主題，其位於`App_Themes`資料夾。 因為我們還沒有這類資料夾，Visual Studio 會請提供建立一個讓我們加入我們的第一個面板時。 按一下 [是] 建立`App_Theme`資料夾，並將新`GridView.skin`那里檔案。


[![讓 Visual Studio 建立 App_Theme 資料夾](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**圖 10**： 可讓 Visual Studio 建立`App_Theme`資料夾 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


這會建立新的佈景主題中`App_Themes`命名 GridView 面板檔案與資料夾`GridView.skin`。


![GridView 佈景主題到 App_Theme 資料夾具有 已加入](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**圖 11**: GridView 佈景主題可讓您有尚未加入`App_Theme`資料夾


重新命名為 DataWebControls 的 GridView 佈景主題 (以滑鼠右鍵按一下 GridView 資料夾中`App_Theme`資料夾，然後選擇 重新命名)。 接下來，輸入下列標記成`GridView.skin`檔案：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

這會定義的預設屬性`CssClass`-相關的任何 GridView DataWebControls 佈景主題會使用任何網頁中的屬性。 讓我們加入另一個面板為 DetailsView，我們將稍後使用的 Web 控制項的資料。 將新的面板加入至名為 DataWebControls 主題`DetailsView.skin`並加入下列標記：


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

我們已定義的佈景主題，最後一個步驟是將佈景主題套用至我們的 ASP.NET 頁面。 根據頁面的頁面或網站中的所有頁面，可以套用佈景主題。 讓我們來使用此主題中網站的所有頁面。 若要完成這項作業，加入下列標記以`Web.config`的`<system.web>`> 一節：


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

這就是這麼簡單 ！ `styleSheetTheme`設定表示佈景主題中所指定的屬性應該*不*覆寫控制層級上指定的屬性。 主題設定應該優先控制設定，請使用`theme`屬性取代`styleSheetTheme`; 不幸的是，Visual Studio 設計檢視中看不到主題設定。 請參閱[ASP.NET 佈景主題和面板概觀](https://msdn.microsoft.com/en-us/library/ykzx33wh.aspx)和[伺服器端樣式使用佈景主題](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)佈景主題和面板; 上的相關資訊請參閱[How To： 適用於 ASP.NET 主題](https://msdn.microsoft.com/en-us/library/0yy5hxdk%28VS.80%29.aspx)如需詳細資訊設定頁面，即可使用佈景主題。


[![在 GridView 會顯示產品的名稱、 類別、 供應商、 價格和已停止的資訊](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**圖 12**: GridView 會顯示產品的名稱、 類別、 供應商、 價格和已停止的資訊 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>在 DetailsView 中一次顯示一筆記錄

在 GridView 會顯示資料來源控制項所繫結所傳回的每一筆記錄的一個資料列。 但有些的時候，不過，當我們可能會想要一次顯示唯一的記錄或只有一項記錄。 [DetailsView 控制項](https://msdn.microsoft.com/en-us/library/s3w1w7t4.aspx)提供這項功能，轉譯為 HTML`<table>`具有兩個資料行和一個資料列，每個資料行或屬性繫結至控制項。 您可以將 DetailsView 視為單一記錄旋轉 90 度的 GridView。

開始加入的 DetailsView 控制項*上方*中的 GridView `SimpleDisplay.aspx`。 接下來，將它繫結到相同 ObjectDataSource 控制項為 GridView。 Like 與 GridView BoundField 就會加入至 ObjectDataSource 所傳回的物件中每一個屬性 DetailsView`Select`方法。 唯一的差別在於 DetailsView 的 BoundFields 配置時水平而不是垂直。


[![在 DetailsView 加入頁面和繫結到 ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**圖 13**: DetailsView 加入頁面和繫結到 ObjectDataSource ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


GridView，像是在 DetailsView 的 BoundFields 可強制提供更多自訂的顯示 ObjectDataSource 所傳回的資料。 圖 14 顯示 DetailsView 之後其 BoundFields 和`CssClass`屬性已設定，使其外觀類似於 GridView 範例。


[![在 DetailsView 顯示單一資料錄](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**圖 14**: DetailsView 顯示單一記錄 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


在 DetailsView 只會顯示其資料來源傳回的第一個記錄的注意事項。 若要讓使用者逐步執行的所有記錄，一次，我們必須啟用 DetailsView 的分頁。 若要這樣做，請返回 Visual Studio 並檢查 DetailsView 的智慧標籤的 啟用分頁核取方塊。


[![啟用 DetailsView 控制項中的分頁](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**圖 15**: DetailsView 控制項中啟用分頁 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![啟用分頁，DetailsView 可讓使用者檢視的任何產品](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**圖 16**: DetailsView 分頁已啟用，允許使用者檢視的任何產品 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


我們會詳細討論分頁未來教學課程。

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>一次顯示一筆記錄為更有彈性的版面配置

在 DetailsView 是相當固定在 ObjectDataSource 所傳回的每一筆記錄的顯示方式。 我們可能會想更有彈性的資料檢視。 比方說，而不是個別的資料列上顯示的產品名稱、 類別、 供應商、 價格和已停止的資訊，我們可能想要顯示產品名稱，並在價格`<h4>`標題時，出現的類別目錄] 和 [供應商資訊以下的名稱和價格較小的字型大小。 和我們可能不想要顯示值旁邊的屬性名稱 （產品、 分類等等）。

[FormView 控制項](https://msdn.microsoft.com/en-US/library/fyf1dk77.aspx)提供這個層級自訂。 而不是使用欄位 （例如 GridView 和 DetailsView 做到的），在 FormView 會使用範本，可允許混合的 Web 控制項，靜態的 HTML 和[資料繫結語法](http://www.15seconds.com/issue/040630.htm)。 如果您已熟悉 ASP.NET 從中繼器控制項 1.x，您可以想像在 formview 中繼器顯示單一記錄。

將 FormView 控制項加入`SimpleDisplay.aspx`頁面的設計介面。 一開始在 FormView 會顯示為灰色區塊，通知我們，我們需要最小值，提供控制項的`ItemTemplate`。


[![在 FormView 必須包含 ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**圖 17**: FormView 必須包含`ItemTemplate`([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


您可以直接繫結在 FormView 透過在 FormView 的智慧標籤，將會建立預設的資料來源控制項`ItemTemplate`自動 (連同`EditItemTemplate`和`InsertItemTemplate`，如果 ObjectDatatSource 控制項`InsertMethod`和`UpdateMethod`屬性會設定)。 不過，此範例中我們將資料繫結至 FormView 並指定其`ItemTemplate`手動。 開始，以設定在 FormView 的`DataSourceID`屬性`ID`ObjectDataSource 控制項的`ObjectDataSource1`。 接下來，建立`ItemTemplate`，使其顯示產品的名稱和價格`<h4>`項目和類別目錄和託運商名稱下方，以較小的字型大小。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![第一次的產品 (Chai) 會顯示在自訂格式](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**圖 18**: 第一次產品 (Chai) 會顯示在自訂格式 ([按一下以檢視完整大小的影像](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>`是資料繫結的語法。 `Eval`方法會傳回目前物件所繫結至 FormView 控制項的指定屬性值。 請參閱 Alex 荷馬文章 <<c0> [ 簡化並擴充資料繫結語法在 ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)如需資料繫結的好處。

DetailsView，像是 FormView 只會顯示從 ObjectDataSource 傳回第一筆記錄。 您可以啟用要允許訪客逐步執行的一項產品，一次在 FormView 中的分頁。

## <a name="summary"></a>總結

存取和顯示資料的商務邏輯層就可完成而不需要撰寫一行程式碼，這點受惠 ASP.NET 2.0 ObjectDataSource 控制項。 ObjectDataSource 叫用指定之類別的方法，並傳回結果。 這些結果可以顯示在資料繫結至 ObjectDataSource Web 控制項。 在此教學課程中我們會探討 GridView、 DetailsView 和在 FormView 的控制項繫結至 ObjectDataSource。

到目前為止我們只了解如何使用 ObjectDataSource 叫用無參數方法，但如果我們要叫用的方法，必須要有輸入參數，例如`ProductBLL`類別的`GetProductsByCategoryID(categoryID)`嗎？ 若要呼叫的方法必須要有一個或多個參數，我們必須設定 ObjectDataSource 來指定這些參數的值。 我們會看到如何完成在我們[下一個教學課程](declarative-parameters-vb.md)。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建立您自己的資料來源控制項](https://msdn.microsoft.com/en-us/library/ms364049.aspx)
- [ASP.NET 2.0 的 GridView 範例](https://msdn.microsoft.com/en-us/library/aa479339.aspx)
- [簡化並擴充資料繫結在 ASP.NET 2.0 的語法](http://www.15seconds.com/issue/040630.htm)
- [在 ASP.NET 2.0 的佈景主題](http://www.odetocode.com/Articles/423.aspx)
- [使用佈景主題的伺服器端樣式](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [如何： 以程式設計方式套用 ASP.NET 主題](https://msdn.microsoft.com/en-us/library/tx35bd89.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
[下一頁](declarative-parameters-vb.md)
