---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: 互動內容的頁面上，從主版頁面 (VB) |Microsoft 文件
author: rick-anderson
description: 會檢查呼叫方法、 從主版頁面中的程式碼中設定屬性的 [內容] 頁面等等的方式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 9274924b441cb21e33eb57de06ff374428fa036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889471"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>互動內容的頁面上，從主版頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> 會檢查呼叫方法、 從主版頁面中的程式碼中設定屬性的 [內容] 頁面等等的方式。


## <a name="introduction"></a>簡介

前述教學課程會檢查如何以程式設計方式互動其主版頁面的內容頁面。 我們已更新包括列出五個最近的 GridView 控制項的主版頁面重新叫用加入產品。 然後，我們會建立使用者無法加入新的產品內容頁面。 時加入新的產品，以指示主版頁面重新整理其 GridView，使它可以包含剛加入的產品所需內容頁面。 將公用方法加入至主版頁面，重新整理資料繫結至 GridView，，然後叫用該方法，從 [內容] 頁面，被完成這項功能。

最常見的內容和主版頁面互動形式來自內容頁面。 不過，主版頁面 rouse 目前的內容頁面，為動作，可能會而且可能需要這類功能，如果主版頁面包含讓使用者修改資料，也會顯示在 [內容] 頁面上的使用者介面項目。 請考慮內容頁面，顯示產品中的資訊 GridView 控制項和主版頁面，其中包含按鈕控制項，按下時，會將所有產品的標價加倍。 如同前述的教學課程中的範例，需要重新整理之後，使其顯示新的價格，按下按鈕的 double 價格 GridView 但在此案例中必須為動作 rouse 內容頁面的主版頁面。

本教學課程會探討如何叫用功能的內容頁面中定義的主版頁面。

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>透過事件和事件處理常式進行程式設計互動

叫用從主版頁面的內容頁面功能會比其他方式的更具有挑戰性。 因為內容頁面時進行程式設計的互動，舉凡內容頁面，具有單一主版頁面中，我們知道公用方法和屬性在我們處置時。 主版頁面中，不過，可以有許多不同內容頁面，每個都有它自己的屬性和方法集。 如何，然後可以我們中撰寫程式碼以其內容頁面中執行某些動作，當我們不知道哪些內容的頁面將會叫用，直到執行階段時的主版頁面？

請考慮的 ASP.NET Web 控制項，例如按鈕控制項。 按鈕控制項可出現在任意數目的 ASP.NET 網頁，且需要某種機制，它可以警示 頁面，已按一下。 這利用完成*事件*。 按鈕控制項特別的是，引發其`Click`事件按一下時其; 包含按鈕的 ASP.NET 網頁 （選擇性） 可回應透過該通知*事件處理常式*。

這個相同的模式可以用來在其內容頁的主版頁面的觸發程序功能：

1. 將事件加入至主版頁面。
2. 引發事件時的主版頁面需要其內容的頁面與通訊。 例如，如果主版頁面需要提醒其內容的頁面使用者加倍價格，其事件就會引發價格超過一倍之後，立即。
3. 建立事件處理常式中採取某些動作需要這些內容頁面。

本教學課程的這個其餘部分實作簡介; 中所述的範例也就是內容頁面，列出資料庫中的產品和包含按鈕的主版頁面控制價格增加兩倍。

## <a name="step-1-displaying-products-in-a-content-page"></a>步驟 1： 在內容頁面中顯示的產品

我們第一件事是建立列出的產品，從 Northwind 資料庫的內容頁面。 (我們在先前的教學課程中，加入至專案 Northwind 資料庫[*互動主版頁面內容的頁面從*](interacting-with-the-master-page-from-the-content-page-vb.md)。)藉由新增新的 ASP.NET 網頁的開始`~/Admin`資料夾名為`Products.aspx`，務必將它繫結讓`Site.master`主版頁面。 此頁面已加入至網站之後，圖 1 顯示 [方案總管]。


[![將新的 ASP.NET 網頁新增至 [Admin] 資料夾](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**圖 01**： 加入新的 ASP.NET 網頁的`Admin`資料夾 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


請記得，在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`如果不是產生頁面的標題明確地設定。 移至`Products.aspx`網頁的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案至這一課中加入一個項目。 加入下列標記下方`<siteMapNode>`主版頁面互動課程內容：


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

這個加法`<siteMapNode>`項目會反映在課程清單 （請參閱圖 5）。

返回`Products.aspx`。 在內容控制項中`MainContent`、 新增 GridView 控制項並將其命名`ProductsGrid`。 將 GridView 繫結至新的 SqlDataSource 控制項，名為`ProductsDataSource`。


[![將 GridView 繫結至新的 SqlDataSource 控制項](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**圖 02**： 將 GridView 繫結至新的 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


設定精靈，使其使用 Northwind 資料庫。 如果您已完成上一個教學課程，則您應該已經有名稱為連接字串`NorthwindConnectionString`中`Web.config`。 圖 3 所示，從下拉式清單中，選擇此連接字串。


[![設定為使用 Northwind 資料庫 SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**圖 03**： 設定為使用 Northwind 資料庫 SqlDataSource ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


接下來，指定資料來源控制項的`SELECT`陳述式，藉由從下拉式清單中選擇 [產品] 資料表，並傳回`ProductName`和`UnitPrice`（請參閱圖 4） 的資料行。 按一下 [下一步]，然後 「 完成 」 以完成設定資料來源精靈。


[![傳回從 Products 資料表的 [ProductName] 和 UnitPrice 欄位](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**圖 04**： 傳回`ProductName`和`UnitPrice`欄位從`Products`資料表 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


這就是這麼簡單 ！ 完成精靈之後 Visual Studio 會將兩個 BoundFields 加入 GridView 鏡像 SqlDataSource 控制項所傳回的兩個欄位。 遵循 GridView 和 SqlDataSource 控制項的標記。 圖 5 顯示透過瀏覽器檢視時的結果。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![每個產品而其價格則列於 GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**圖 05**: GridView 列出每個產品及其價格 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> 請隨意清除 GridView 的外觀。 某些建議包括格式化為貨幣顯示的 UnitPrice 值和使用背景色彩和字型，以改善格線的外觀。 如需有關顯示和格式化資料在 ASP.NET 中的詳細資訊，請參閱我[使用資料的教學課程系列](../../data-access/index.md)。


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>步驟 2： 將雙重價格按鈕加入至主版頁面

下一步是要加入按鈕 Web 控制項至主要頁面上，按一下時，將雙資料庫中的所有產品的價格。 開啟`Site.master`主版頁面，並從工具箱拖曳至設計工具中，將它下方拖曳一個按鈕`RecentProductsDataSource`SqlDataSource 控制項，我們在上一個教學課程中加入。 設定按鈕的`ID`屬性`DoublePrice`及其`Text`"雙引號產品價格 」 的屬性。

接下來，將 SqlDataSource 控制項加入主版頁面上，其命名為`DoublePricesDataSource`。 將用來執行此 SqlDataSource`UPDATE`按兩下所有價格的陳述式。 具體來說，所以我們需要將其`ConnectionString`和`UpdateCommand`屬性，以適當的連接字串和`UPDATE`陳述式。 然後我們需要呼叫此 SqlDataSource 控制項`Update`方法時`DoublePrice`按鈕。 若要設定`ConnectionString`和`UpdateCommand`屬性，選取 SqlDataSource 控制項，然後移至 [屬性] 視窗。 `ConnectionString`屬性會列出已儲存在這些連接字串`Web.config`在下拉式清單中，選擇`NorthwindConnectionString`選項圖 6 所示。


[![設定要使用 NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**圖 06**： 設定要使用 SqlDataSource `NorthwindConnectionString` ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


若要設定`UpdateCommand`屬性，在 屬性 視窗中，尋找 UpdateQuery 選項。 選取時，這個屬性會顯示省略符號; 的按鈕按一下此按鈕即可顯示命令及參數編輯器對話方塊圖 7 所示。 輸入下列命令`UPDATE`到對話方塊的文字方塊中的陳述式：


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

此陳述式，在執行時，將會加倍`UnitPrice`值中的每一筆記錄`Products`資料表。


[![設定 SqlDataSource 的 UpdateCommand 屬性](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**圖 07**： 設定 SqlDataSource`UpdateCommand`屬性 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


之後設定這些屬性，您按鈕和 SqlDataSource 控制項的宣告式標記看起來應該如下所示：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

剩下的只有呼叫其`Update`方法時`DoublePrice`按鈕。 建立`Click`事件處理常式`DoublePrice`按鈕並加入下列程式碼：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

若要測試這項功能，請瀏覽`~/Admin/Products.aspx`頁面，我們在步驟 1 中建立，並按一下 「 雙重產品價格 」 按鈕。 按一下按鈕導致回傳，並執行`DoublePrice`按鈕的`Click`事件處理常式，使所有產品的價格加倍。 然後重新轉譯頁面標記會傳回並重新顯示在瀏覽器中。 「 雙引號產品價格 」 之前 button 已按下 GridView 在內容頁面中，不過，列出相同的價格。 這是因為一開始載入在 GridView 的資料儲存在檢視狀態，因此除非另有指示否則，它不會在回傳時載入其狀態。 如果您瀏覽不同的頁面，然後返回`~/Admin/Products.aspx`頁面，您會看到更新的價格。

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>引發事件時的價格被重複步驟 3:

因為在 GridView`~/Admin/Products.aspx`頁面不會立即反映價格加倍，使用者可能會理解認為他們沒有按一下"雙引號產品價格 」 按鈕，或無法運作。 它們可能嘗試按一下按鈕更多時間，使價格加倍，一再重複。 若要修正此我們必須將內容中的方格頁面後，顯示新價格立即它們加倍。

如稍早在本教學課程中所討論，我們要引發事件主版頁面中的，每當使用者按一下`DoublePrice` 按鈕。 事件是一個類別 （事件發行者） 的方式通知另一組有趣的東西會發生其他類別 （事件訂閱者）。 在此範例中，主版頁面是 「 事件發行者 」。這些內容頁面關心時`DoublePrice`按鈕是 「 訂閱者 」。

藉由建立事件訂閱類別*事件處理常式*，這是執行以回應所引發的事件的方法。 「 發行者 」 定義他藉由定義所引發的事件*事件委派*。 事件委派指定的事件處理常式必須接受輸入的參數。 在.NET Framework 中，事件委派執行不傳回任何值，並接受兩個輸入的參數：

- `Object`，其可識別事件來源，並
- 衍生自的類別 `System.EventArgs`

傳遞至事件處理常式的第二個參數可以包含其他事件的相關資訊。 雖然基底`EventArgs`沿著任何資訊未通過類別，.NET Framework 包含一些類別會擴充`EventArgs`並包含額外的屬性。 例如，`CommandEventArgs`執行個體傳遞至回應的事件處理常式`Command`事件，並包含兩個參考的屬性：`CommandArgument`和`CommandName`。

> [!NOTE]
> 如需有關建立的詳細資訊，引發，和處理事件，請參閱[事件和委派](https://msdn.microsoft.com/library/17sde2xt.aspx)和[簡單 english 事件委派](http://www.codeproject.com/KB/cs/eventdelegates.aspx)。


若要定義事件，請使用下列語法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

因為我們只需要時使用者已按一下 [警示內容] 頁面`DoublePrice`按鈕並不需要任何其他資訊一起傳遞，我們可以使用事件委派`EventHandler`，其為第二個定義可接受的事件處理常式參數型別的物件`System.EventArgs`。 若要建立事件主版頁面中，將下列程式碼行加入主版頁面的程式碼後置類別：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

上述程式碼會將公用事件加入至名為的主版頁面`PricesDoubled`。 我們現在需要價格超過一倍之後引發此事件。 若要引發事件會使用下列語法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

其中*寄件者*和*eventArgs*是您想要傳遞給訂閱者的事件處理常式的值。

更新`DoublePrice``Click`事件處理常式，以下列程式碼：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

如往常一般，`Click`事件處理常式會呼叫方式啟動`DoublePricesDataSource`SqlDataSource 控制項`Update`方法的所有產品的價格增加兩倍。 之後，有兩個事件處理常式加入項目。 首先， `RecentProducts` GridView 的資料重新整理。 此 GridView 新增至主版頁面在先前的教學課程中，以及顯示的最新加入的五個產品。 我們需要重新整理此方格，使它顯示這些五項產品只兩倍的價格。 接下來，`PricesDoubled`就會引發事件。 主版頁面本身的參考 (`Me`) 會傳送至事件處理常式的事件來源和空白做`EventArgs`物件傳送做為事件引數。

## <a name="step-4-handling-the-event-in-the-content-page"></a>步驟 4： 處理事件的內容頁面

主版頁面此時引發其`PricesDoubled`事件每當`DoublePrice`按一下按鈕控制項。 不過，這是只有一半-我們仍需要處理 「 訂閱者 」 中的事件。 這牽涉到兩個步驟： 建立事件處理常式，並加入事件配線程式碼，以便在引發事件時執行的事件處理常式。

藉由建立名為事件處理常式開始`Master_PricesDoubled`。 因為我們的定義方式`PricesDoubled`主版頁面中的事件的事件處理常式的兩個輸入的參數必須為類型`Object`和`EventArgs`分別。 在事件處理常式呼叫`ProductsGrid`GridView 的`DataBind`重新繫結至資料格資料的方法。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

此事件處理常式的程式碼已完成，但我們尚未至網路的主版頁面`PricesDoubled`事件，此事件處理常式。 「 訂閱者 」 纏繞事件之事件處理常式，透過下列語法：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*發行者*提供事件的物件參考*eventName*，和*methodName*是在 「 訂閱者 」 中定義的事件處理常式的名稱。

此事件配線程式碼必須執行上的第一個頁面瀏覽和後續回傳時，而且應該發生在之前可能會引發事件時的頁面生命週期中的點。 加入事件配線程式碼的好時機是在 PreInit 階段中，就會發生頁面生命週期中及早。

開啟`~/Admin/Products.aspx`並建立`Page_PreInit`事件處理常式：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

若要完成此連接程式碼中，我們需要從內容頁面以程式設計方式參考主版頁面。 如先前的教學課程中所述，有兩種方式可以執行這項操作：

- 透過將轉型鬆散型別`Page.Master`屬性適當的主版頁面的型別，或
- 藉由新增`@MasterType`指示詞`.aspx`頁面，然後使用 強型別`Master`屬性。

我們將使用第二種方法。 加入下列`@MasterType`指示詞，以便在頁面的宣告式標記的頂端：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

然後加入下列事件配線程式碼中的`Page_PreInit`事件處理常式：


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

GridView 內容頁面中的重新整理此位置的程式碼，每當`DoublePrice`按鈕。

數字 8 和 9 說明這項行為。 圖 8 顯示當第一次瀏覽的頁面。 請注意，在價格值`RecentProducts`GridView （中的主版頁面的左側資料行） 和`ProductsGrid`GridView （在 [內容] 頁面）。 圖 9 顯示相同畫面之後立即`DoublePrice`按下按鈕。 如您所見，這兩個 GridViews 會立即反映新的價格。


[![初始的價格值](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**圖 08**: 價格初始值 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Just-Doubled 價格會顯示在 GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**圖 09**: The Just-Doubled 價格會顯示在 GridViews ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>總結

在理想情況下，主版頁面和其內容的頁面是完全獨立的另一個，而且需要互動的任何層級。 不過，如果您的主版頁面或顯示的資料，可以進行修改主版頁面或內容頁面的內容頁面，則您可能需要具有警示內容頁面 （或相反的） 主版頁面的資料修改時，這樣可以更新顯示。 在前述教學課程中我們了解如何以程式設計方式互動其主版頁面; 內容頁面在此教學課程中我們討論了如何擁有主版頁面起始互動。

以程式設計方式之間的內容和主版頁面的互動可以來自於內容或主版頁面中，而使用互動模式取決於原始。 這些差異是因為，內容頁面具有單一主版頁面中，但主版頁面都可以有許多不同的內容頁面。 而不是主版頁面直接與內容的頁面互動，較好的做法是讓引發事件來表示某些動作都已經發生的主版頁面。 這些動作有興趣的內容頁面可以建立事件處理常式。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新在 ASP.NET 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [事件和委派](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [主版頁面內容之間傳遞資訊](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教學課程中使用的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Suchi Banerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](interacting-with-the-master-page-from-the-content-page-vb.md)
> [下一頁](master-pages-and-asp-net-ajax-vb.md)
