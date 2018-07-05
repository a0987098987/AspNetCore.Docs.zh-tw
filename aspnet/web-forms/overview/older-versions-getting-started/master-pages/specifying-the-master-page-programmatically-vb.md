---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: 以程式設計方式指定主版頁面 (VB) |Microsoft Docs
author: rick-anderson
description: 查看設定內容頁面的主版頁面，以程式設計方式透過 PreInit 事件處理常式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: d9962004d0bcd816d7fccaac1db5a3e8d3b8e2b9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369466"
---
<a name="specifying-the-master-page-programmatically-vb"></a>以程式設計方式指定主版頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip)或[下載 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> 查看設定內容頁面的主版頁面，以程式設計方式透過 PreInit 事件處理常式。


## <a name="introduction"></a>簡介

因為在我的範例[*建立全網站的版面配置使用主版頁面*](creating-a-site-wide-layout-using-master-pages-vb.md)所有內容頁面已參考以宣告方式透過其主版頁面、 `MasterPageFile` 中的屬性`@Page`指示詞。 例如，下列`@Page`指示詞連結至主版頁面的 [內容] 頁面`Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page`類別](https://msdn.microsoft.com/library/system.web.ui.page.aspx)中`System.Web.UI`命名空間包含[`MasterPageFile`屬性](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx)，傳回 [內容] 頁面的主版頁面的路徑; 它是由設定這個屬性`@Page`指示詞。 這個屬性也可用來以程式設計方式指定 [內容] 頁面的主版頁面。 這個方法很有用，如果您想要以動態方式指派的外部因素，例如瀏覽頁面的使用者為基礎的主版頁面。

在本教學課程中我們將第二個主版頁面新增至我們的網站，並以動態方式決定要在執行階段使用的主版頁面。

## <a name="step-1-a-look-at-the-page-lifecycle"></a>步驟 1： 了解網頁生命週期

當要求抵達 ASP.NET 網頁的內容頁面的 web 伺服器時，ASP.NET 引擎必須 fuse 網頁的內容控制項的主版頁面載入的對應 ContentPlaceHolder 控制項。 此 fusion 會建立單一的控制項階層架構，可以繼續透過一般的網頁生命週期。

[圖 1] 說明此融合。 步驟 1 中圖 1 顯示的初始內容和主版頁面控制項階層架構。 結尾結尾的 PreInit 階段內容頁面中會新增至主版頁面 (步驟 2) 中對應的 ContentPlaceHolders。 之後此 fusion，主版頁面可做為合成的控制項階層的根。 這融合控制項階層架構接著會新增至頁面以產生最終的控制項階層架構 (步驟 3)。 最後結果就是網頁的控制項階層架構包含積的控制項階層架構。


[![主版頁面和內容頁面的控制項階層是融合在一起的 PreInit 階段](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**圖 01**: 主版頁面和內容頁面的控制項階層架構會融合在一起的 PreInit 階段 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>步驟 2： 設定`MasterPageFile`從程式碼的屬性

主版頁面 partakes 此 fusion 中的值而定`Page`物件的`MasterPageFile`屬性。 設定`MasterPageFile`屬性中`@Page`指示詞有指派的淨影響`Page`的`MasterPageFile`在初始化階段，也就是網頁的生命週期的第一階段的屬性。 我們也可以以程式設計方式設定此屬性。 不過，務必在圖 1 中融合發生之前，設定這個屬性。

PreInit 階段開頭`Page`物件引發其[`PreInit`事件](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx)，並呼叫其[`OnPreInit`方法](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)。 若要以程式設計方式設定主版頁面，然後，我們可以建立的事件處理常式`PreInit`事件或覆寫`OnPreInit`方法。 讓我們看看這兩種方法。

首先開啟`Default.aspx.vb`，我們的網站首頁的程式碼後置類別檔案。 新增頁面的事件處理常式`PreInit`輸入下列程式碼中的事件：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

我們可以從這裡設定`MasterPageFile`屬性。 更新程式碼，以便將值指派"~ / Site.master 」 到`MasterPageFile`屬性。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

如果您設定中斷點並開始偵錯您會看到每當`Default.aspx`瀏覽的頁面，或每當至此頁面，請回傳`Page_PreInit`事件處理常式執行和`MasterPageFile`屬性指派給"~ / Site.master"。

或者，您可以覆寫`Page`類別的`OnPreInit`方法，並設定`MasterPageFile`那里屬性。 針對此範例中，讓我們未設定主版頁面在特定的頁面中，而從`BasePage`。 您應該記得，我們會建立自訂的基底頁面類別 (`BasePage`) 回到[*主版頁面中指定的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程。 目前`BasePage`會覆寫`Page`類別的`OnLoadComplete`方法，它用來設定頁面的`Title`基礎網站導覽資料的屬性。 讓我們更新`BasePage`也會覆寫`OnPreInit`方法，以程式設計方式指定主版頁面。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

因為我們所有的內容頁面衍生自`BasePage`，所有人都現在必須以程式設計的方式指派給其主版頁面。 此時`PreInit`中的事件處理常式`Default.aspx.vb`多餘，歡迎您將它移除。

### <a name="what-about-thepagedirective"></a>什麼是`@Page`指示詞？

哪些可能會造成一些混淆是內容的網頁`MasterPageFile`屬性現已在兩個位置所指定： 以程式設計方式在`BasePage`類別的`OnPreInit`方法以及透過`MasterPageFile`中每個內容頁面的屬性`@Page`指示詞。

在頁面生命週期中的第一個階段是初始化階段。 在這個階段期間`Page`物件的`MasterPageFile`的值指派給屬性`MasterPageFile`屬性中`@Page`指示詞 （如果有提供）。 PreInit 階段遵循初始化階段，並就在這裡，我們以程式設計方式設定`Page`物件的`MasterPageFile`屬性，藉此覆寫從指派的值`@Page`指示詞。 因為我們要設定`Page`物件的`MasterPageFile`屬性以程式設計的方式，我們可以移除`MasterPageFile`屬性從`@Page`指示詞不會影響使用者的體驗。 若要說服自己這個問題，請繼續進行，並移除`MasterPageFile`屬性從`@Page`指示詞`Default.aspx`，然後瀏覽透過瀏覽器頁面。 如您所預期，輸出會是相同之前移除的屬性。

是否`MasterPageFile`屬性設定透過`@Page`指示詞或以程式設計方式並不重要終端使用者的體驗。 不過，`MasterPageFile`屬性中`@Page`指示詞由 Visual Studio 在設計階段期間產生的 WYSIWYG 設計工具檢視中的。 如果您返回`Default.aspx`Visual Studio 中，並瀏覽至設計工具中，您會看到訊息，「 主版頁面錯誤： 分頁具有需要主版頁面參考的控制項，但未指定"（請參閱 圖 2）。

簡單地說，您必須保持`MasterPageFile`屬性中`@Page`享受豐富的設計階段經驗，在 Visual Studio 中的指示詞。


[![Visual Studio 會使用@Page呈現 [設計] 檢視的指示詞的 MasterPageFile 屬性](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**圖 02**: Visual Studio 會使用`@Page`指示詞的`MasterPageFile`屬性轉譯到 [設計] 檢視 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>步驟 3： 建立替代主版頁面

因為可以以程式設計方式設定內容頁面的主版頁面，在執行階段就能夠以動態方式載入特定的主版頁面，根據某些外部的準則。 這項功能可以在其中的網站配置需要更改根據使用者的情況下很有用。 比方說，部落格引擎 web 應用程式可能會允許其使用者選擇他們的部落格的版面配置其中每一個版面配置是不同的主版頁面與相關聯。 在執行階段，當訪客正在檢視使用者的部落格、 web 應用程式必須判斷部落格的版面配置，並以動態方式將對應的主版頁面與 [內容] 頁面產生關聯。

我們看看如何動態載入主版頁面，在執行階段根據一些外部的準則。 我們的網站目前包含一個主版頁面 (`Site.master`)。 我們需要另一個說明選擇在執行階段主版頁面的主版頁面。 此步驟中，著重於建立和設定新的主版頁面。 步驟 4 會查看判斷哪些主版頁面，即可在執行階段使用。

名為根資料夾中建立新的主版頁面`Alternate.master`。 也加入至名為網站的 新的樣式表`AlternateStyles.css`。


[![新增另一個網站的主版頁面和 CSS 檔案](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**圖 03**： 加入另一個的主版頁面和 CSS 檔案至網站 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image9.png))


我已設計`Alternate.master`主版頁面上方的頁面上，置中對齊和海軍藍的背景上顯示的標題。 我鈔票的左側資料行，移動該內容下方`MainContent`ContentPlaceHolder 控制項，現在會橫跨整個頁面的寬度。 此外，我 nixed 未排序的課程清單，並取代上述的水平清單`MainContent`。 此外，我也會更新的字型和色彩和所使用的主版頁面 （，延伸模組，其內容頁）。 [圖 4] 所示`Default.aspx`時使用`Alternate.master`主版頁面。

> [!NOTE]
> ASP.NET 包含定義的能力*佈景主題*。 主題是影像、 CSS 檔案和與樣式相關 Web 控制項屬性設定可套用至頁面，以在執行階段的集合。 佈景主題是最好的選擇，如果您的網站配置不同，只顯示之影像和其 CSS 規則。 如果配置不同更本質上，例如使用不同的 Web 控制項，或有完全不同的版面配置，則您必須使用個別的主版頁面。 佈景主題的更多有關本教學課程結尾處，請參閱進一步閱讀 > 一節。


[![內容頁面現在可以使用新的外觀與風格](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**圖 04**： 內容頁面現在可以使用新的外觀與風格 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image12.png))


當主要和內容頁面的標記會融合時，`MasterPage`類別會確認每個內容控制項，在 [內容] 頁面中的參考 ContentPlaceHolder 主版頁面中的。 如果找不到參考不存在 ContentPlaceHolder 內容控制項，則會擲回例外狀況。 換句話說，務必要指派給 [內容] 頁面的主版頁面，有每個 ContentPlaceHolder 內容在 [內容] 頁面中的控制項。

`Site.master`主版頁面包含四個 ContentPlaceHolder 控制項：

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

在我們的網站內容的頁面部分只是一個或兩個內容控制項;其他還包括每個可用的 ContentPlaceHolders 內容控制項。 如果我們新的主版頁面 (`Alternate.master`) 可能曾經會指派給所有在 ContentPlaceHolders 有內容控制項的內容頁面`Site.master`則是不可或缺的`Alternate.master`也包含相同的 ContentPlaceHolder 控制項，作為`Site.master`.

若要取得您`Alternate.master`看起來會類似我的 （請參閱 圖 4），開始藉由定義中的主版頁面的樣式的主版頁面`AlternateStyles.css`樣式表。 新增下列規則到`AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

接下來，新增下列宣告式標記`Alternate.master`。 如您所見，`Alternate.master`包含具有相同的四個 ContentPlaceHolder 控制項`ID`ContentPlaceHolder 控制項中的值`Site.master`。 此外，它包含 ScriptManager 控制項，也就是我們的網站使用 ASP.NET AJAX 架構的這些頁面所需。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>測試新的主版頁面

若要測試這個新的主版頁面更新`BasePage`類別的`OnPreInit`方法，讓`MasterPageFile`將值指派給屬性`"~/Alternate.maser"`，然後瀏覽網站。 每一頁應該不會發生錯誤，除了兩個函式：`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`。 將產品加入至在 DetailsView`~/Admin/AddProduct.aspx`會導致`NullReferenceException`嘗試設定主版頁面的程式碼行從`GridMessageText`屬性。 瀏覽時`~/Admin/Products.aspx``InvalidCastException`在頁面載入，訊息就會擲回: 「 無法將型別的物件轉換 'ASP.alternate\_主要' 輸入 ' ASP.site\_master'。 」

這些錯誤發生的原因`Site.master`程式碼後置類別包含公用事件、 屬性和方法中未定義`Alternate.master`。 這兩個頁面的標記部分有`@MasterType`指示詞參考`Site.master`主版頁面。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

此外，DetailsView 的`ItemInserted`中的事件處理常式`~/Admin/AddProduct.aspx`包含程式碼轉換鬆散型別`Page.Master`型別的物件的屬性`Site`。 `@MasterType` （使用這種方式） 的指示詞和轉換中的運算`ItemInserted`事件處理常式緊密結合兩者`~/Admin/AddProduct.aspx`並`~/Admin/Products.aspx`頁面`Site.master`主版頁面。

若要中斷這個緊密結合，我們可以有`Site.master`和`Alternate.master`衍生自一般基底類別包含定義的公用成員。 接下來，我們可以更新`@MasterType`指示詞，以參考這個通用基底型別。

### <a name="creating-a-custom-base-master-page-class"></a>建立自訂基底主版頁面類別

加入新的類別檔案，來`App_Code`名為資料夾`BaseMasterPage.vb`，並讓它衍生自`System.Web.UI.MasterPage`。 我們必須定義`RefreshRecentProductsGrid`方法和`GridMessageText`中的屬性`BaseMasterPage`，但我們無法只移動它們那里從`Site.master`因為這些成員使用特定的 Web 控制項`Site.master`主版頁面 ( `RecentProducts`GridView 和`GridMessage`標籤)。

我們要做為設定`BaseMasterPage`方式定義，這些成員，但實際上由`BaseMasterPage`的衍生類別 (`Site.master`和`Alternate.master`)。 這種類型的繼承可將標示為類別`MustInherit`和做為其成員`MustOverride`。 簡單地說，這些關鍵字加入類別和其兩個成員發表，`BaseMasterPage`未實作`RefreshRecentProductsGrid`和`GridMessageText`，但將其衍生的類別。

我們也需要定義`PricesDoubled`中的事件`BaseMasterPage`和引發事件的衍生類別中提供的方法。 此行為，以便使用.NET Framework 中的模式是基底類別中建立公用事件，並新增受保護且可覆寫的方法，名為`OnEventName`。 在衍生的類別接著可以呼叫此方法以引發事件，或可以覆寫它立即之前或之後引發事件時，才執行程式碼。

更新您`BaseMasterPage`類別，使其包含下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

接下來，移至`Site.master`程式碼後置類別，並讓它衍生自`BaseMasterPage`。 因為`BaseMasterPage`包含標示的成員`MustOverride`我們需要覆寫中的成員才能這裡`Site.master`。 新增`Overrides`方法與屬性定義的關鍵字。 更新程式碼引發，也`PricesDoubled`中的事件`DoublePrice` 按鈕的`Click`事件處理常式的基底類別呼叫`OnPricesDoubled`方法。

這些修改之後`Site.master`程式碼後置類別應該包含下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

我們也需要更新`Alternate.master`的程式碼後置類別變成衍生自`BaseMasterPage`，並覆寫這兩個`MustOverride`成員。 但是因為`Alternate.master`不包含 GridView，最新的產品或新的產品之後，會顯示一則訊息的標籤加入至資料庫的清單，這些方法不需要採取任何動作。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>參考基底 Master Page 類別

現在，我們已完成`BaseMasterPage`類別，並且已經將它擴充我們兩個主版頁面，最後一個步驟是，更新`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`頁面，以參考這個常見的類型。 開始先變更`@MasterType`從這兩個頁面指示詞：


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

收件者:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

而不是參考檔案路徑，`@MasterType`屬性現在已參考的基底類型 (`BaseMasterPage`)。 因此，強型別`Master`現已在這兩個頁面的程式碼後置類別中使用的屬性型別的`BaseMasterPage`(而不是類型`Site`)。 使用此項變更之後再次造訪`~/Admin/Products.aspx`。 先前，這會導致轉換錯誤因為頁面設定為使用`Alternate.master`主版頁面，但`@MasterType`指示詞參考`Site.master`檔案。 但現在頁面會呈現不會發生錯誤。 這是因為`Alternate.master`型別的物件可以轉型主版頁面`BaseMasterPage`（因為它會擴充它）。

沒有需要進行中的一項小變更`~/Admin/AddProduct.aspx`。 在 DetailsView 控制項`ItemInserted`事件處理常式會使用這兩個強型別`Master`屬性，與鬆散型別`Page.Master`屬性。 我們已修正的強型別參考，當我們更新了`@MasterType`指示詞，但我們仍需要更新的鬆散型別參考。 取代下列程式碼行：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

使用下列程式碼，它會轉換`Page.Master`基底類型：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>步驟 4： 決定哪些繫結到內容頁面的主版頁面

我們`BasePage`類別目前設定的所有內容頁面的`MasterPageFile`屬性，以在網頁生命週期的 PreInit 階段中的硬式編碼值。 我們可以更新此程式碼以根據某些外部因素的主版頁面。 可能將主版頁面目前登入使用者的喜好設定而定。 在此情況下，我們必須撰寫程式碼`OnPreInit`方法中的`BasePage`查閱目前正在瀏覽使用者的主版頁面的喜好設定。

讓我們建立可讓使用者選擇要使用哪一個主版頁面的網頁`Site.master`或`Alternate.master`-工作階段變數中儲存這項選擇。 藉由名為的根目錄中建立新的 web 網頁啟動`ChooseMasterPage.aspx`。 建立此頁面 （或任何其他內容頁面通） 時您不需要將它繫結至主版頁面由於主版頁面以程式設計方式設定`BasePage`。 不過，如果您不會繫結的新頁面主版頁面則新頁面的預設宣告式標記包含 Web Form 和主版頁面所提供的其他內容。 您必須以手動方式使用適當的內容控制項來取代此標記。 基於這個理由，我發現將新的 ASP.NET 網頁繫結至主版頁面的工作變得更容易。

> [!NOTE]
> 因為`Site.master`和`Alternate.master`有相同一組 ContentPlaceHolder 控制項並不重要時建立新的內容頁面，您選擇何種主版頁面。 為求一致，我會建議使用`Site.master`。


[![將新的內容頁面新增至網站](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**圖 05**： 將新的 [內容] 頁面新增至網站 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image15.png))


更新`Web.sitemap`檔案，以包含這一課中的項目。 加入下列標記下方`<siteMapNode>`主版頁面和 ASP.NET AJAX 一課中：


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

然後再加入任何內容`ChooseMasterPage.aspx`頁面上花一點時間來更新頁面的程式碼後置類別，使其衍生自`BasePage`(而非`System.Web.UI.Page`)。 接下來，將 DropDownList 控制項加入頁面，設定其`ID`屬性，以`MasterPageChoice`，並新增具有兩個 ListItems`Text`值"~ / Site.master"和"~ / Alternate.master"。

將 Button Web 控制項新增至頁面並設定其`ID`並`Text`屬性，以`SaveLayout`和 「 儲存的版面配置選項 」，分別。 此時您頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

當第一次造訪網頁時我們要顯示使用者的目前選取的主版頁面選擇。 建立`Page_Load`事件處理常式，並新增下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

上述程式碼執行，只在第一個頁面瀏覽 （和不在後續回傳時）。 它會先檢查以查看工作階段變數`MyMasterPage`存在。 若是如此，它會嘗試尋找在比對的 ListItem `MasterPageChoice` DropDownList。 如果找到相符的清單項目，其`Selected`屬性設定為`True`。

我們也需要將儲存到使用者選擇的程式碼`MyMasterPage`工作階段變數。 建立事件處理常式`SaveLayout`按鈕的`Click`事件，並新增下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> 依時間`Click`事件處理常式執行回傳時，已選取的主版頁面。 因此，使用者的下拉式清單選取項目不是作用中直到下一個頁面瀏覽。 `Response.Redirect`強制瀏覽器，以重新要求`ChooseMasterPage.aspx`。


具有`ChooseMasterPage.aspx`完整的頁面上，最後一項工作是將`BasePage`指派`MasterPageFile`屬性的值，根據`MyMasterPage`工作階段變數。 如果未設定工作階段變數`BasePage`預設為`Site.master`。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> 我可以移動指派的程式碼`Page`物件的`MasterPageFile`共屬性`OnPreInit`事件處理常式和分成兩個不同的方法。 此第一個方法中， `SetMasterPageFile`，指派`MasterPageFile`屬性設為第二個方法中，所傳回的值`GetMasterPageFileFromSession`。 我標示`SetMasterPageFile`方法`Overridable`，使未來的類別延伸`BasePage`可以選擇性地覆寫以實作自訂邏輯，如有需要。 稍後將會覆寫`BasePage`的`SetMasterPageFile`下一個教學課程中的屬性。


使用此程式碼就緒之後，請瀏覽`ChooseMasterPage.aspx`頁面。 一開始，`Site.master`主版頁面已選取 （請參閱 圖 6），但使用者可以選擇不同的主版頁面，從下拉式清單。


[![內容頁面會顯示使用 Site.master 主版頁面](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**圖 06**： 內容頁面會顯示使用`Site.master`主版頁面 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![內容頁面現在會顯示使用 Alternate.master 主版頁面](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**圖 07**： 內容頁面會顯示使用現在`Alternate.master`主版頁面 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>總結

當瀏覽內容的頁面時，其內容的控制項被融合具有其主版頁面 ContentPlaceHolder 控制項。 [內容] 頁面的主版頁面由表示`Page`類別的`MasterPageFile`屬性，指派給`@Page`指示詞的`MasterPageFile`初始化階段的屬性。 本教學課程中示範了，我們可以將值指派給為`MasterPageFile`只要我們這麼做之前 PreInit 階段結束時的屬性。 能夠以程式設計方式指定主版頁面會開啟更進階的案例，例如動態繫結至外部因素，包括主版頁面的內容頁面的媒體櫃門。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網頁生命週期圖表](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET 網頁生命週期概觀](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET 佈景主題和面板概觀](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [主版頁面： 秘訣、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [在 ASP.NET 中的佈景主題](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Suchi Banerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-pages-and-asp-net-ajax-vb.md)
> [下一頁](nested-master-pages-vb.md)
