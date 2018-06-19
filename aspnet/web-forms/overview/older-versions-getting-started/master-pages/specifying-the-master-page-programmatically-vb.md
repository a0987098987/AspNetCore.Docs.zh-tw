---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: 以程式設計方式指定主版頁面 (VB) |Microsoft 文件
author: rick-anderson
description: 會查看設定內容頁面的主版頁面，以程式設計方式透過 PreInit 事件處理常式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ba2981e627199da89a25b0b59840f66521f2e78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890264"
---
<a name="specifying-the-master-page-programmatically-vb"></a>以程式設計方式指定主版頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip)或[下載 PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> 會查看設定內容頁面的主版頁面，以程式設計方式透過 PreInit 事件處理常式。


## <a name="introduction"></a>簡介

因為我的範例中[*建立全站台的版面配置使用主版頁面*](creating-a-site-wide-layout-using-master-pages-vb.md)，所有內容的頁面參考以宣告方式透過其主版頁面`MasterPageFile`中屬性`@Page`指示詞。 例如，下列`@Page`指示詞連結至主版頁面的 [內容] 頁面`Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page`類別](https://msdn.microsoft.com/library/system.web.ui.page.aspx)中`System.Web.UI`命名空間包含[`MasterPageFile`屬性](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx)，傳回內容頁面的主版頁面的路徑，則可以由設定這個屬性`@Page`指示詞。 這個屬性也可用來以程式設計方式指定內容頁面的主版頁面。 這個方法很有用，如果您想要以動態方式指派主版頁面根據外部因素，例如瀏覽頁面的使用者。

本教學課程中我們會將第二個主版頁面加入至我們的網站，以動態方式決定要在執行階段使用的主版頁面。

## <a name="step-1-a-look-at-the-page-lifecycle"></a>步驟 1： 看頁面生命週期

ASP.NET 引擎時要求到達網頁伺服器內容頁面 ASP.NET 網頁時，必須將網頁的內容控制項加入主版頁面中對應 ContentPlaceHolder 的控制項。 此 fusion 會建立單一控制項階層架構，可以繼續透過典型網頁生命週期。

圖 1 說明這個融合。 步驟 1 中圖 1 顯示的初始內容和主版頁面控制項階層架構。 PreInit 階段內容的結尾結束頁面中的控制項加入至對應的 ContentPlaceHolders 主版頁面 (步驟 2) 中。 之後此融合主版頁面當做合成的控制項階層架構的根。 這 fused 控制項階層架構接著會加入至頁面，會產生最終的控制項階層架構 (步驟 3)。 最後結果就是頁面控制項階層架構包含合成的控制項階層架構。


[![主版頁面和內容頁面控制項階層會一起 Fused PreInit 階段](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**圖 01**: 主版頁面和內容頁面控制項階層是一起 Fused PreInit 階段 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>步驟 2： 設定`MasterPageFile`從程式碼的屬性

在此融合 partakes 哪些主版頁面的值而定`Page`物件的`MasterPageFile`屬性。 設定`MasterPageFile`屬性`@Page`指示詞有指派的淨影響`Page`的`MasterPageFile`在初始化階段，也就是網頁的生命週期的第一個階段的屬性。 我們也可以程式設計方式設定這個屬性。 不過，請務必在圖 1 融合之前設定這個屬性。

在 PreInit 階段開始`Page`物件引發其[`PreInit`事件](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx)並呼叫其[`OnPreInit`方法](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)。 若要以程式設計方式設定主版頁面，然後，我們可以建立事件處理常式`PreInit`事件或覆寫`OnPreInit`方法。 讓我們看看這兩種方法。

先開啟`Default.aspx.vb`，我們的網站首頁上的程式碼後置類別檔。 加入頁面的事件處理常式`PreInit`輸入下列程式碼中的事件：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

我們可以從這裡設定`MasterPageFile`屬性。 更新程式碼，它會將值指派"~ / Site.master 」 至`MasterPageFile`屬性。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

如果您設定中斷點並開始偵錯您會看到每當`Default.aspx`瀏覽頁面時，或每當回傳至這個頁面上，`Page_PreInit`事件處理常式都會執行和`MasterPageFile`屬性指派給"~ / Site.master"。

或者，您可以覆寫`Page`類別的`OnPreInit`方法並將`MasterPageFile`那里屬性。 此範例中，我們沒有設定主版頁面中的特定頁面，而是從`BasePage`。 前文提過，我們會建立自訂的基底頁類別 (`BasePage`) 回[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程。 目前`BasePage`會覆寫`Page`類別的`OnLoadComplete`方法，它用來設定網頁的`Title`屬性根據網站地圖資料。 讓我們更新`BasePage`也覆寫`OnPreInit`方法，以程式設計方式指定主版頁面。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

因為我們所有的內容頁衍生自`BasePage`，全部都現在有以程式設計的方式指派其主版頁面。 此時`PreInit`中的事件處理常式`Default.aspx.vb`是多餘; 隨意移除它。

### <a name="what-about-thepagedirective"></a>呢`@Page`指示詞？

可能有點混淆內容的頁面是`MasterPageFile`屬性現已被指定在兩個地方： 以程式設計方式在`BasePage`類別的`OnPreInit`方法也透過`MasterPageFile`中每個內容頁面的屬性`@Page`指示詞。

在頁面生命週期中的第一個階段是初始化階段。 在這個階段`Page`物件的`MasterPageFile`的值指派給屬性`MasterPageFile`屬性`@Page`指示詞 （如果有提供）。 PreInit 階段遵循初始化階段，並就在這裡，我們以程式設計方式設定`Page`物件的`MasterPageFile`屬性，藉此覆寫從指派的值`@Page`指示詞。 由於我們將`Page`物件的`MasterPageFile`屬性以程式設計方式，我們無法移除`MasterPageFile`屬性從`@Page`指示詞不會影響使用者的體驗。 若要說服自行這個，請繼續進行，並移除`MasterPageFile`屬性從`@Page`指示詞`Default.aspx`，然後瀏覽透過瀏覽器頁面。 如您所預期，輸出會是相同之前移除的屬性。

是否`MasterPageFile`屬性設定透過`@Page`指示詞或以程式設計的方式並不重要終端使用者的體驗。 不過，`MasterPageFile`屬性`@Page`指示詞可由 Visual Studio 在設計階段期間來產生所見即所得的設計工具中檢視。 如果您返回`Default.aspx`Visual Studio 中，瀏覽至設計工具中，您會看到訊息，"主版頁面的錯誤： 分頁具有需要主版頁面參考的控制項，但未指定"（請參閱圖 2）。

簡單地說，您需要讓`MasterPageFile`屬性`@Page`享受豐富的設計階段經驗，在 Visual Studio 中的指示詞。


[![Visual Studio 會使用@Page指示詞的 MasterPageFile 屬性來呈現 [設計] 檢視](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**圖 02**: Visual Studio 會使用`@Page`指示詞的`MasterPageFile`要轉譯屬性 [設計] 檢視 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>步驟 3： 建立替代主版頁面

因為可以在執行階段可以動態載入特定的主版頁面，根據某些外部準則以程式設計方式設定內容頁面的主版頁面。 這項功能可以在其中站台的配置必須不同根據使用者的情況下很有用。 比方說，部落格引擎 web 應用程式可能會讓其使用者選擇他們的部落格的版面配置其中每個配置都與不同的主版頁面。 在執行階段，當訪客正在檢視使用者的部落格、 web 應用程式需要決定部落格的版面配置，並動態將對應的主版頁面關聯的內容頁面。

讓我們來討論如何動態載入某個外部的準則為基礎執行階段主版頁面。 我們的網站目前包含一個主版頁面 (`Site.master`)。 我們需要其他說明選擇在執行階段主版頁面的主版頁面。 此步驟著重於建立及設定新的主版頁面。 步驟 4 會查看決定哪些在執行階段使用的主版頁面。

名為的根資料夾中建立新的主版頁面`Alternate.master`。 也將新的樣式表加入至名為網站`AlternateStyles.css`。


[![加入另一個網站的主版頁面和 CSS 檔案](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**圖 03**： 加入另一個主版頁面和 CSS 檔案的網站 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image9.png))


我已設計`Alternate.master`主版頁面頂端的頁面上，置中對齊和海軍藍的背景上顯示的標題。 我已在分配的左側資料行，並移動該內容下方`MainContent`ContentPlaceHolder 控制，現在會橫跨整個頁面的寬度。 此外，我 nixed 未排序的課程清單，並取代上述水平清單`MainContent`。 我也已更新的字型和色彩及所使用的主版頁面 （，延伸模組，其內容頁）。 圖 4 顯示`Default.aspx`時使用`Alternate.master`主版頁面。

> [!NOTE]
> ASP.NET 包括了可定義*佈景主題*。 主題是影像、 CSS 檔案和樣式相關網頁的控制項屬性設定可以套用至在執行階段頁面的集合。 主題是如果您的網站配置不同只在顯示的映像和其 CSS 規則的方式。 如果配置不同更本質上，例如使用不同的 Web 控制項，或擁有完全不同的配置，則您必須使用不同的主版頁面。 在本教學課程，如需有關主題的結尾，請參閱進一步閱讀章節。


[![我們的內容頁面現在可以使用新的外觀與風格](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**圖 04**： 我們的內容頁面現在可以使用新的外觀及操作 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image12.png))


當主要和內容頁面的標記 fused 時，`MasterPage`類別會檢查以確定每個內容控制項的內容頁面參考 ContentPlaceHolder 主版頁面中的。 如果找不到參考不存在 ContentPlaceHolder 內容控制項，則會擲回例外狀況。 換句話說，務必要指派給內容頁面的主版頁面有 ContentPlaceHolder，每個內容控制項的內容頁面。

`Site.master`主版頁面包含四個 ContentPlaceHolder 控制項：

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

在我們的網站內容的頁面包括只有一個或兩個內容控制項。其他每個可用 ContentPlaceHolders 包含內容控制項。 如果我們新的主版頁面 (`Alternate.master`) 可能不會指派給內容控制項的所有 ContentPlaceHolders 中那些內容頁面`Site.master`則很重要，`Alternate.master`也包含與相同ContentPlaceHolder控制項`Site.master`.

若要取得您`Alternate.master`主版頁面看起來很相似，鑽研 （請參閱圖 4），請啟動藉由定義中的主版頁面的樣式`AlternateStyles.css`樣式表。 新增下列規則到`AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

接下來，加入下列宣告式標記`Alternate.master`。 如您所見，`Alternate.master`包含具有相同的四個 ContentPlaceHolder 控制項`ID`值當作 ContentPlaceHolder 控制項`Site.master`。 此外，它包含一個 ScriptManager 控制項，在我們的網站中使用 ASP.NET AJAX 架構的那些頁面才需要。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>測試新的主版頁面

若要測試這個新的主版頁面更新`BasePage`類別的`OnPreInit`方法以便`MasterPageFile`將值指派給屬性`"~/Alternate.maser"`，然後瀏覽的網站。 每個頁面應該不會發生錯誤，除了兩個函式：`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`。 在 detailsview 中新增一項產品`~/Admin/AddProduct.aspx`導致`NullReferenceException`從嘗試設定主版頁面的程式碼行`GridMessageText`屬性。 造訪時`~/Admin/Products.aspx``InvalidCastException`與訊息的頁面載入時擲回: 「 無法將型別的物件轉換 'ASP.alternate\_master' 輸入' ASP.site\_master'。 」

這些錯誤的發生原因`Site.master`程式碼後置類別包含公用事件、 屬性和方法中未定義`Alternate.master`。 這兩個分頁的標記部分有`@MasterType`指示詞參考`Site.master`主版頁面。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

此外，在 DetailsView 的`ItemInserted`中的事件處理常式`~/Admin/AddProduct.aspx`包含程式碼會轉換為鬆散型別`Page.Master`屬性型別的物件`Site`。 `@MasterType` （使用這種方式） 的指示詞和轉換`ItemInserted`緊密結合的事件處理常式`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`頁面`Site.master`主版頁面。

若要中斷這個緊密結合，我們可以有`Site.master`和`Alternate.master`衍生自一般基底類別，其中包含定義的公用成員。 接下來，我們可以更新`@MasterType`指示詞，以參考這個通用基底型別。

### <a name="creating-a-custom-base-master-page-class"></a>建立自訂的基底主版頁面類別

加入新的類別檔案`App_Code`資料夾名為`BaseMasterPage.vb`，並讓它衍生自`System.Web.UI.MasterPage`。 我們必須定義`RefreshRecentProductsGrid`方法和`GridMessageText`屬性`BaseMasterPage`，但我們無法只移動它們那里從`Site.master`因為這些成員使用特定的 Web 控制項`Site.master`主版頁面 ( `RecentProducts`GridView 和`GridMessage`標籤)。

我們需要如何做為設定`BaseMasterPage`方式定義，這些成員，但實際上由實作`BaseMasterPage`的衍生類別 (`Site.master`和`Alternate.master`)。 這種類型的繼承可標示為類別`MustInherit`和做為其成員`MustOverride`。 簡單地說，這些關鍵字加入類別和其兩個成員宣告的`BaseMasterPage`尚未實作`RefreshRecentProductsGrid`和`GridMessageText`，但該將其衍生的類別。

我們也需要定義`PricesDoubled`中的事件`BaseMasterPage`衍生的類別，以引發此事件來提供一種方法。 這種行為，以便使用.NET Framework 中的模式是基底類別中建立公用事件，並新增名為的受保護的、 可覆寫方法`OnEventName`。 在衍生的類別接著可以呼叫這個方法以引發此事件，或可以覆寫它立即之前或之後就會引發事件時，才執行程式碼。

更新您`BaseMasterPage`類別，使其包含下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

接下來，請移至`Site.master`程式碼後置類別，並讓它衍生自`BaseMasterPage`。 因為`BaseMasterPage`包含標示的成員`MustOverride`我們需要覆寫中的成員才能這裡`Site.master`。 新增`Overrides`方法與屬性定義的關鍵字。 也請更新程式碼引發`PricesDoubled`中的事件`DoublePrice`按鈕的`Click`事件處理常式的基底類別呼叫`OnPricesDoubled`方法。

這些修改之後`Site.master`程式碼後置類別應包含下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

我們也需要更新`Alternate.master`的程式碼後置類別衍生自`BaseMasterPage`並覆寫這兩個`MustOverride`成員。 但是由於`Alternate.master`不包含的 GridView，最新的產品，也不會顯示一個訊息之後的新產品的標籤加入至資料庫的清單，這些方法不需要採取任何動作。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>參考基底主版頁面類別

既然我們已經完成`BaseMasterPage`類別而我們將它擴充的兩個主要頁面，最後一個步驟是，更新`~/Admin/AddProduct.aspx`和`~/Admin/Products.aspx`分頁來參考這個通用的類型。 藉由變更啟動`@MasterType`指示詞從兩個頁面：


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

收件者:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

而不是參考檔案路徑，`@MasterType`屬性現在已參考的基底類型 (`BaseMasterPage`)。 因此，強型別`Master`現在是在這兩個頁面的程式碼後置類別中使用的屬性型別的`BaseMasterPage`(而不是類型`Site`)。 透過這項變更就地重新瀏覽`~/Admin/Products.aspx`。 先前，這會導致轉換錯誤則由於頁面設定成使用`Alternate.master`主版頁面，但`@MasterType`指示詞參考`Site.master`檔案。 但是，頁面現在會呈現不會發生錯誤。 這是因為`Alternate.master`主版頁面都可以轉換成的型別物件`BaseMasterPage`（因為它會擴充它）。

必須設為中的一個小變更是`~/Admin/AddProduct.aspx`。 在 DetailsView 控制項的`ItemInserted`事件處理常式會使用這兩個的強型別`Master`屬性，與鬆散型別`Page.Master`屬性。 當我們更新時，我們會修正的強型別參考`@MasterType`指示詞，但我們仍需要更新的鬆散型別參考。 取代下列程式碼行：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

使用下列程式碼，它會轉換`Page.Master`的基底類型：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>步驟 4： 決定哪些繫結至內容頁面的主版頁面

我們`BasePage`類別目前設定的所有內容頁面的`MasterPageFile`硬式編碼值頁面生命週期的 PreInit 階段中的屬性。 我們可以更新這個程式碼以根據一些外部因素的主版頁面。 可能是主版頁面載入取決於目前登入使用者的喜好設定。 在此情況下，我們必須撰寫程式碼`OnPreInit`方法中的`BasePage`查閱目前正在瀏覽使用者的主版頁面的喜好設定。

讓我們來建立網頁，可讓使用者選擇的主版頁面，即可使用-`Site.master`或`Alternate.master`-工作階段變數中儲存這項選擇。 藉由名為的根目錄中建立新的網頁啟動`ChooseMasterPage.aspx`。 此頁面 （或任何其他內容頁面從此以後） 建立時不需要繫結至主版頁面中以程式設計方式設定主版頁面因為`BasePage`。 不過，如果您請不要繫結之新網頁主版頁面然後在新頁面的預設宣告式標記包含 Web Form 和主版頁面所提供的其他內容。 您必須手動將適當的內容控制項取代這個標記。 因此，我找到您更輕鬆地將新的 ASP.NET 網頁繫結至主版頁面。

> [!NOTE]
> 因為`Site.master`和`Alternate.master`有相同的 ContentPlaceHolder 控制項集合並不重要時建立新的內容頁面，您選擇何種主版頁面。 為了保持一致性，建議使用`Site.master`。


[![將新的內容頁面新增至網站](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**圖 05**： 網站加入新的內容頁面 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image15.png))


更新`Web.sitemap`檔案至這一課中加入一個項目。 加入下列標記下方`<siteMapNode>`主版頁面和 ASP.NET AJAX 一課中：


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

然後再加入任何內容發佈至`ChooseMasterPage.aspx`頁面花一點時間來更新網頁的程式碼後置類別，好讓它衍生自`BasePage`(而非`System.Web.UI.Page`)。 接下來，將 DropDownList 控制項加入至頁面、 設定其`ID`屬性`MasterPageChoice`，並加入兩個具有 ListItems`Text`值"~ / Site.master"和"~ / Alternate.master"。

按鈕 Web 控制項加入網頁，並設定其`ID`和`Text`屬性`SaveLayout`」 儲存配置的選擇 」，並分別。 此時網頁的宣告式標記看起來應該如下所示：


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

當第一次瀏覽網頁時我們需要顯示使用者的目前選取的主版頁面選擇。 建立`Page_Load`事件處理常式並加入下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

上述程式碼執行只有第一個頁面瀏覽 （和不在後續回傳時）。 它會先檢查以查看工作階段變數`MyMasterPage`存在。 若是如此，它會嘗試尋找相符的 ListItem 中`MasterPageChoice`DropDownList。 如果找到相符的清單項目，其`Selected`屬性設定為`True`。

我們還需要儲存到使用者的選擇的程式碼`MyMasterPage`工作階段變數。 建立事件處理常式`SaveLayout`按鈕的`Click`事件並加入下列程式碼：


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> 依時間`Click`在回傳事件處理常式都會執行、 已選取的主版頁面。 因此，使用者的下拉式清單選取項目不是作用中直到下一個頁面瀏覽。 `Response.Redirect`強制瀏覽器以重新要求`ChooseMasterPage.aspx`。


與`ChooseMasterPage.aspx`完成 頁面上，我們最後一項工作是讓`BasePage`指派`MasterPageFile`屬性根據值`MyMasterPage`工作階段變數。 如果未設定的工作階段變數具有`BasePage`預設為`Site.master`。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> 我移動指派的程式碼`Page`物件的`MasterPageFile`屬性超出`OnPreInit`事件處理常式，並置於兩個不同的方法。 第一個方法， `SetMasterPageFile`，指派`MasterPageFile`屬性設為第二個方法中，所傳回的值`GetMasterPageFileFromSession`。 標示`SetMasterPageFile`方法`Overridable`，使未來的類別延伸`BasePage`可以選擇性地覆寫該實作自訂邏輯，如有需要。 我們會看到的覆寫範例`BasePage`的`SetMasterPageFile`下一個教學課程中的屬性。


此位置的程式碼，請瀏覽`ChooseMasterPage.aspx`頁面。 一開始，`Site.master`主版頁面已選取 （請參閱圖 6），但使用者可以選擇不同的主版頁面，從下拉式清單。


[![內容頁面會顯示使用 Site.master 主版頁面](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**圖 06**： 內容頁面會顯示使用`Site.master`主版頁面 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![內容頁面現在會顯示使用 Alternate.master 主版頁面](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**圖 07**： 內容頁面會立即顯示使用`Alternate.master`主版頁面 ([按一下以檢視完整大小的影像](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>總結

當瀏覽內容的頁面時，其內容的控制項被 fused 使用其主版頁面的 ContentPlaceHolder 控制項。 內容頁面的主版頁面由表示`Page`類別的`MasterPageFile`屬性，指派給`@Page`指示詞的`MasterPageFile`初始化階段時的屬性。 本教學課程中已顯示，我們可以指派到的值為`MasterPageFile`只要我們 PreInit 階段結束之前執行此動作的屬性。 能夠以程式設計方式指定主版頁面開啟更進階的案例，例如動態地繫結至外部因素，包括主版頁面的內容頁面的媒體櫃門。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網頁生命週期圖表](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET 網頁生命週期概觀](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET 佈景主題和面板概觀](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [主版頁面： 秘訣、 竅門與設陷](http://www.odetocode.com/articles/450.aspx)
- [在 ASP.NET 中的主題](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Suchi Banerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](master-pages-and-asp-net-ajax-vb.md)
> [下一頁](nested-master-pages-vb.md)
