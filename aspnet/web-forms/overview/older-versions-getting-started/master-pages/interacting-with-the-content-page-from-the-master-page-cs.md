---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: 內容從與頁面互動主版頁面 (C#) |Microsoft Docs
author: rick-anderson
description: 檢驗如何呼叫方法時，由主版頁面的程式碼中設定屬性的 [內容] 頁面等等。
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 293e4dab6142393c9d57836a2f04244388e54cec
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808362"
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>內容從與頁面互動主版頁面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> 檢驗如何呼叫方法時，由主版頁面的程式碼中設定屬性的 [內容] 頁面等等。


## <a name="introduction"></a>簡介

前述教學課程檢驗如何讓 [內容] 頁面，以程式設計方式與其主版頁面互動。 我們已更新以包含列出五個最近的 GridView 控制項的主版頁面的重新叫用加入產品。 然後，我們會建立內容頁面的使用者可以加入新的產品。 在新的產品，[內容] 頁面所需以指示主版頁面重新整理其 GridView，使它會包含剛加入的產品。 這項功能被透過將公用方法新增至主版頁面，重新整理資料繫結至 GridView，，然後叫用該方法，從 [內容] 頁面。

最常見的內容和主版頁面互動形式源自於 [內容] 頁面。 不過，可能成可採取行動，rouse 目前的內容頁面的主版頁面，而且可能需要這類功能，如果主版頁面包含使用者介面項目，讓使用者能夠修改也會顯示在 [內容] 頁面的資料。 請考慮內容頁面，顯示產品資訊，在 GridView 控制項和主版頁面，其中包含一個按鈕控制項，按下時，所有產品的價格將增加一倍。 如同先前的教學課程的範例，GridView 需要重新整理 double 的價格，以顯示新的價格，按下按鈕之後，但在此案例中，它是主版頁面需要 rouse 成可採取行動的 [內容] 頁面。

本教學課程將探討如何叫用功能，在 [內容] 頁面中定義的主版頁面。

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>透過事件和事件處理常式進行程式設計互動

叫用的主版頁面的內容頁面功能是比反過來要好更具挑戰性。 因為內容頁面時進行程式設計互動，從 [內容] 頁面，具有單一的主版頁面中，我們知道公用方法與屬性，供我們運用。 不過，主版頁面，可以有許多不同內容頁面，各有自己一組屬性和方法。 我們要如何，然後撰寫程式碼主版頁面，以在其內容頁面中執行某些動作，當我們不知道哪些內容的頁面將會叫用，直到執行階段中？

請考慮為 ASP.NET Web 控制項，例如按鈕控制項。 按鈕控制項可以顯示在任意數目的 ASP.NET 網頁，而且需要用它可以警示頁面，它已按下的機制。 這利用完成*事件*。 特別是，按鈕控制項會引發其`Click`事件按一下; 時，包含按鈕的 ASP.NET 網頁 （選擇性） 可以回應透過該通知*事件處理常式*。

這個相同的模式可用來在其內容頁面的主版頁面觸發程序功能：

1. 加入主版頁面中的事件。
2. 引發事件，每當需要其內容的頁面與通訊的主版頁面。 比方說，如果主版頁面需要提醒其內容的頁面使用者加倍價格，其事件就會引發緊接著價格超過一倍。
3. 在需要採取某些動作的內容頁面中的事件處理常式。

本教學課程的其餘部分會實作簡介; 中所述的範例也就是列出資料庫中的產品內容頁面和主版頁面，其中包含一個按鈕控制價格的兩倍。

## <a name="step-1-displaying-products-in-a-content-page"></a>步驟 1： 在內容上顯示的產品

我們第一要務是建立內容的頁面，其中列出 Northwind 資料庫中的產品。 (我們在先前的教學課程中，加入至專案的 Northwind 資料庫[*與主版頁面，從內容頁互動*](interacting-with-the-master-page-from-the-content-page-cs.md)。)藉由新增新的 ASP.NET 頁面，以啟動`~/Admin`名為資料夾`Products.aspx`，並確定將它繫結`Site.master`主版頁面。 此頁面已加入至網站之後，圖 1 顯示 方案總管。


[![將新的 ASP.NET 網頁新增至 [Admin] 資料夾](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**圖 01**： 加入新的 ASP.NET 頁面，以便`Admin`資料夾 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


請注意，在[*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為自訂的基底頁面類別`BasePage`如果不是產生頁面的標題明確地設定。 移至`Products.aspx`頁面的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案，以包含這一課中的項目。 加入下列標記下方`<siteMapNode>`針對至主版頁面互動課程內容：


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

這個加法`<siteMapNode>`項目會反映在課程清單 （請參閱 [圖 5]）。

返回`Products.aspx`。 在適用於內容的控制項`MainContent`，將 GridView 控制項並命名它`ProductsGrid`。 繫結至新的 SqlDataSource 控制項，名為的 GridView `ProductsDataSource`。


[![繫結至新的 SqlDataSource 控制項的 GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**圖 02**： 將 GridView 繫結至新的 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


因此，它會使用 Northwind 資料庫，請設定精靈。 如果您已完成上一個教學課程，則您應該已經有名稱為的連接字串`NorthwindConnectionString`在`Web.config`。 圖 3 所示，請從下拉式清單中，選擇此連接字串。


[![設定為使用 Northwind 資料庫 SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**圖 03**： 設定為使用 Northwind 資料庫 SqlDataSource ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


接下來，指定資料來源控制項的`SELECT`陳述式，從下拉式清單中選擇 產品 資料表，並傳回`ProductName`和`UnitPrice`（請參閱 圖 4） 的資料行。 按一下 下一步，然後完成 以完成設定資料來源精靈


[![傳回從 Products 資料表的 [ProductName] 和 UnitPrice 欄位](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**圖 04**： 傳回`ProductName`並`UnitPrice`欄位從`Products`資料表 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


這樣就完成了 ！ 完成精靈之後 Visual Studio 會將兩個 BoundFields 加入至 GridView，以鏡像 SqlDataSource 控制項所傳回的兩個欄位。 GridView 和 SqlDataSource 控制項的標記會遵循。 [圖 5] 顯示透過瀏覽器檢視時的結果。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![每個產品而其價格則列於 GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**圖 05**： 每個產品而其價格則列於 GridView ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> 請放心清除 GridView 的外觀。 一些建議包括格式化為貨幣顯示的 UnitPrice 值和使用背景色彩和字型來改善格線的外觀。 如需有關顯示和設定在 ASP.NET 中的資料格式的詳細資訊，請參閱我[使用資料的教學課程系列](../../data-access/index.md)。


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>步驟 2： 將 Double 的價格按鈕新增至主版頁面

下一步是將按鈕 Web 控制項至主要頁面上，按一下時，將兩倍的資料庫中的所有產品的價格。 開啟`Site.master`主版頁面，並從工具箱拖曳至設計工具中，放置下方拖曳一個按鈕`RecentProductsDataSource`我們在上一個教學課程中新增的 SqlDataSource 控制項。 將按鈕的`ID`屬性，以`DoublePrice`及其`Text`"Double 產品價格 」 的屬性。

接下來，將 SqlDataSource 控制項新增至主版頁面，並將它命名為`DoublePricesDataSource`。 將用來執行此 SqlDataSource`UPDATE`所有價格的兩倍的陳述式。 具體來說，我們要設定其`ConnectionString`並`UpdateCommand`屬性，以適當的連接字串和`UPDATE`陳述式。 然後我們要呼叫此 SqlDataSource 控制項的`Update`方法時`DoublePrice`按一下按鈕時。 若要設定`ConnectionString`和`UpdateCommand`屬性，選取 SqlDataSource 控制項，然後移至 [屬性] 視窗。 `ConnectionString`屬性會列出已儲存在這些連接字串`Web.config`在下拉式清單中，選擇`NorthwindConnectionString`選項，如 [圖 6] 所示。


[![設定要使用 NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**圖 06**： 設定要使用 SqlDataSource `NorthwindConnectionString` ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


若要設定`UpdateCommand`屬性，在 [屬性] 視窗中找出 UpdateQuery 選項。 選取時，這個屬性會顯示具有省略符號; 的按鈕按一下此按鈕即可顯示 [圖 7] 所示的命令及參數編輯器對話方塊。 輸入下列命令`UPDATE`陳述式，在對話方塊的文字方塊：


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

此陳述式，在執行時，將會加倍`UnitPrice`值中的每一筆記錄`Products`資料表。


[![設定 SqlDataSource 的 UpdateCommand 屬性](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**圖 07**： 設定 SqlDataSource`UpdateCommand`屬性 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


設定這些屬性之後, 您按鈕和 SqlDataSource 控制項的宣告式標記看起來應該如下所示：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

是呼叫其`Update`方法時`DoublePrice`按一下按鈕時。 建立`Click`事件處理常式`DoublePrice` 按鈕，並新增下列程式碼：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

若要測試這項功能，請瀏覽`~/Admin/Products.aspx`我們在步驟 1 中建立，然後按一下 「 雙重產品價格 」 按鈕的頁面。 按一下此按鈕會導致回傳，執行`DoublePrice`按鈕的`Click`事件處理常式，使所有產品的價格加倍。 然後重新轉譯頁面的標記會傳回並重新顯示在瀏覽器中。 GridView，在 [內容] 頁面中，不過，會列出相同的價格為"Double 的產品價格 」 之前所按的按鈕。 這是因為一開始載入 GridView 內的資料必須儲存在檢視狀態，因此除非另有指示否則，它不會在回傳時載入其狀態。 如果您瀏覽不同的頁面，然後再傳回給`~/Admin/Products.aspx`頁面，您會看到更新的價格。

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>步驟 3： 引發事件時的價格會增加一倍

因為在 GridView`~/Admin/Products.aspx`頁面不會立即反映價格加倍，使用者可以理解的是認為，它們沒有按一下 「 雙重產品價格 」 按鈕，或無法運作。 它們可能嘗試按一下按鈕多次，一再使價格加倍的幾個。 若要修正此我們要在內容中的方格頁面會顯示新的價格，它們會增加一倍之後，立即。

如稍早在本教學課程中所述，我們需要引發事件，主版頁面中的，每當使用者按一下`DoublePrice` 按鈕。 事件是一個類別 （事件發行者） 的方式通知另一組有趣的東西發生其他類別 （事件訂閱者）。 在此範例中，主版頁面是 「 事件發行者 」;這些內容頁面時關心`DoublePrice` 按鈕便是訂閱者。

藉由建立訂閱事件的類別*事件處理常式*，這是執行以回應所引發的事件的方法。 發行者會定義他藉由定義所引發的事件*事件委派*。 事件委派指定的事件處理常式必須接受輸入的參數。 在.NET Framework 中，事件委派執行不傳回任何值，並接受兩個輸入的參數：

- `Object`，這會識別事件來源，以及
- 衍生自的類別 `System.EventArgs`

傳遞給事件處理常式的第二個參數可以包含事件相關的其他資訊。 雖然基底`EventArgs`類別不傳遞任何資訊、.NET Framework 包含數個擴充的類別`EventArgs`並包含其他屬性。 例如，`CommandEventArgs`執行個體傳遞至回應的事件處理常式`Command`事件，並包含兩個參考的屬性：`CommandArgument`和`CommandName`。

> [!NOTE]
> 如需有關如何建立的詳細資訊，提高，以及處理事件，請參閱[事件與委派](https://msdn.microsoft.com/library/17sde2xt.aspx)並[簡單 english 的事件委派](http://www.codeproject.com/KB/cs/eventdelegates.aspx)。


若要定義事件，請使用下列語法：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

因為我們只需要時使用者按下 [警示內容] 頁面`DoublePrice`按鈕並不需要傳遞任何額外的資訊，我們可以使用事件委派`EventHandler`，其為第二個定義可接受的事件處理常式參數型別的物件`System.EventArgs`。 若要建立主版頁面中的事件，請將下列程式碼行加入主版頁面的程式碼後置類別：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

上述程式碼會將名為的主版頁面中的公用事件`PricesDoubled`。 我們現在需要之後會引發這個事件價格超過一倍。 若要引發事件會使用下列語法：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

何處*寄件者*並*eventArgs*是您想要傳遞給訂閱者的事件處理常式的值。

更新`DoublePrice``Click`為下列程式碼的事件處理常式：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

同樣地，`Click`事件處理常式會啟動，藉由呼叫`DoublePricesDataSource`SqlDataSource 控制項`Update`方法的所有產品價格的兩倍。 之後，有兩個事件處理常式新增項目。 首先， `RecentProducts` GridView 的資料重新整理。 此 GridView 已新增至主版頁面前述教學課程中，並顯示最新加入的五種產品。 我們需要重新整理此方格，使它顯示這些五種產品的只是兩個價格。 接下來，`PricesDoubled`就會引發事件。 主版頁面本身的參考 (`this`) 會傳送至事件處理常式，為事件來源，並為空`EventArgs`物件傳送做為事件引數。

## <a name="step-4-handling-the-event-in-the-content-page"></a>步驟 4： 處理的 [內容] 頁面中的事件

目前主版頁面會引發其`PricesDoubled`事件時`DoublePrice`按一下按鈕控制項。 不過，這是只有一半-我們仍必須處理 「 訂閱者 」 中的事件。 這牽涉到兩個步驟： 建立事件處理常式，並加入事件連接程式碼，以便在引發事件時執行的事件處理常式。

建立名為事件處理常式著手`Master_PricesDoubled`。 因為我們所定義的方式`PricesDoubled`主版頁面中的事件的事件處理常式的兩個輸入的參數必須是類型`Object`和`EventArgs`分別。 在 事件處理常式呼叫`ProductsGrid`GridView 的`DataBind`重新繫結至方格的資料的方法。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

事件處理常式的程式碼已完成，但我們至今還連線的主版頁面`PricesDoubled`這個事件處理常式的事件。 訂閱者將事件的事件處理常式，透過下列語法：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*發行者*會提供事件物件的參考*eventName*，以及*methodName*是訂閱者端有對應的簽章中所定義的事件處理常式的名稱*eventDelegate*。 也就是說，事件委派是否`EventHandler`，然後*methodName*必須是 「 訂閱者 」 不會傳回值，並接受兩個輸入參數的型別中的方法名稱`Object`和`EventArgs`，分別。

此事件的連接程式碼必須執行的第一個頁面瀏覽和後續回傳時，以及應該在前面可能會引發事件時的頁面週期中的某一點。 加入事件連接程式碼的好時機是在 PreInit 階段中，在網頁生命週期非常早期，就會發生。

開啟`~/Admin/Products.aspx`並建立`Page_PreInit`事件處理常式：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

若要完成此連接程式碼中，我們需要從 [內容] 頁面的主版頁面的程式設計參考。 如先前的教學課程中所述，有兩種方式可以執行這項操作：

- 轉換鬆散型別`Page.Master`屬性，適當的主版頁面的型別，或
- 藉由新增`@MasterType`指示詞`.aspx`頁面，然後使用 強型別`Master`屬性。

讓我們使用第二種方法。 新增下列`@MasterType`指示詞加入頁面的宣告式標記的頂端：


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

然後加入下列事件連接程式碼，在`Page_PreInit`事件處理常式：


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

GridView 內容頁面中的重新整理與此程式碼就緒之後，每當`DoublePrice`按一下按鈕時。

圖 8 和 9 說明這項行為。 圖 8 顯示當第一次瀏覽的頁面。 請注意，在價格值`RecentProducts`（在主版頁面的左側資料行） 的 GridView 和`ProductsGrid`GridView （在 [內容] 頁面中）。 圖 9 顯示相同畫面之後立即`DoublePrice`按下按鈕。 如您所見，這兩個 Gridview 會立即會反映新的價格。


[![初始的價格值](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**圖 08**: 初始的價格值 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Just-Doubled 價格會顯示在 Gridview](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**圖 09**: The Just-Doubled 價格會顯示在 Gridview 中 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>總結

在理想情況下，主版頁面和其內容的頁面是完全獨立的另一個，並需要互動的任何層級。 不過，如果您有主版頁面或內容頁面，以顯示可從主版頁面或內容頁面中修改的資料，則您可能需要具有警示內容頁面 （或相反的） 主版頁面的資料修改時，讓您可以更新顯示。 在先前的教學課程中，我們看到如何以程式設計的方式互動其主版頁面; 的內容頁面在本教學課程中，我們討論過如何互動的主版頁面起始。

雖然程式設計內容和主版頁面之間的互動可能來自的內容 」 或 「 主版頁面，使用互動模式取決於原始。 差異是因為，內容頁面具有單一主要頁面，但主版頁面可以有許多不同的內容頁面。 而不需要直接互動內容頁面的主版頁面，更好的方法是能夠引發事件，以表示某些動作都已經發生的主版頁面。 這些動作所關心的內容頁面可以建立事件處理常式。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新在 ASP.NET 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [事件與委派](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [內容與主版頁面之間傳遞資訊](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教學課程中使用的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Suchi Banerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](interacting-with-the-master-page-from-the-content-page-cs.md)
> [下一頁](master-pages-and-asp-net-ajax-cs.md)
