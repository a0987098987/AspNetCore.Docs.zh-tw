---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: "主版頁面 (VB) 中指定的標題、 Meta 標記，以及其他 HTML 標頭 |Microsoft 文件"
author: rick-anderson
description: "查看定義各種不同的技術&lt;head&gt;主版頁面，從 [內容] 頁面中的項目。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bbc2efc67d2d828dd0a5c1fcfe95145e8ffb2cb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>指定標題、 Meta 標記和其他 HTML 標頭在主版頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> 查看定義各種不同的技術&lt;head&gt;主版頁面，從 [內容] 頁面中的項目。


## <a name="introduction"></a>簡介

在 Visual Studio 2008 中建立新的主版頁面有，根據預設，兩個 ContentPlaceHolder 控制項： 一個名為`head`，位於`<head>`項目，以及一個名為`ContentPlaceHolder1`、 Web 表單中放置。 目的`ContentPlaceHolder1`是可自訂的頁面為基礎的 Web 表單中定義的區域。 `head` ContentPlaceHolder 讓頁面以新增自訂內容`<head>`> 一節。 （當然，這些兩個 ContentPlaceHolders 可以修改或移除，而其他 ContentPlaceHolder 可能會加入至主版頁面。 我們的主版頁面`Site.master`，目前有四個 ContentPlaceHolder 控制項。)

HTML`<head>`項目做為儲存機制不是文件本身的一部分網頁文件的相關資訊。 這包括資訊，例如，網頁的標題、 使用搜尋引擎或內部尋檢程式，並連結至外部資源，例如 RSS 摘要、 JavaScript 和 CSS 檔案的中繼資訊。 此資訊的某些部分可能相關之網站中的所有頁面。 例如，您可以全域匯入相同的 CSS 規則和每個 ASP.NET 網頁的 JavaScript 檔案。 不過，有部分`<head>`頁面特定的項目。 頁面標題是最明顯的例子。

在本教學課程中，我們檢驗如何定義全域和頁面的特定`<head>`區段標記和其內容頁中的主版頁面。

## <a name="examining-the-master-pagesheadsection"></a>檢查主版頁面`<head>`區段

Visual Studio 2008 所建立之預設主版網頁檔案包含下列標記中的其`<head>`> 一節：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

請注意，`<head>`元素包含`runat="server"`屬性，表示伺服器控制項 （而非靜態 HTML）。 所有的 ASP.NET 網頁衍生自[`Page`類別](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx)，位於`System.Web.UI`命名空間。 這個類別包含[`Header`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.page.header.aspx)可提供存取網頁的`<head>`區域。 使用`Header`屬性，我們可以設定 ASP.NET 網頁的標題，或將額外的標記新增至呈現`<head>`> 一節。 它是可行的然後自訂內容頁面的`<head>`元素在頁面中撰寫許多程式碼的`Page_Load`事件處理常式。 我們檢驗如何以程式設計方式在步驟 1 中設定頁面的標題。

中所顯示的標記`<head>`上述項目也包含名為的 ContentPlaceHolder 控制項`head`。 當內容頁面可以新增自訂內容，並非必要，這個 ContentPlaceHolder 控制項`<head>`項目以程式設計的方式。 會很有用，不過，在內容頁面要加入至靜態標記的情況下`<head>`對應的內容控制項而不是以程式設計方式為靜態標記的項目也可以以宣告方式新增。

除了`<title>`項目和`head`ContentPlaceHolder，主版頁面的`<head>`項目應該包含任何`<head>`-層級通用於所有頁面的標記。 在我們的網站上所有網頁都使用中定義的 CSS 規則`Styles.css`檔案。 因此，我們已更新`<head>`中的項目[*使用主版頁面建立全站台的配置*](creating-a-site-wide-layout-using-master-pages-vb.md)包含相對應的教學課程`<link>`項目。 我們`Site.master`主版頁面的目前`<head>`標記如下所示。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>步驟 1： 設定內容頁面的標題

網頁的標題會透過指定`<title>`項目。 請務必設定為適當值的每一頁的標題。 當造訪的網頁，其標題會顯示在瀏覽器的標題列中。 此外，建立書籤的頁面上，瀏覽器就會使用頁面的標題做為書籤的建議名稱。 此外，許多搜尋引擎時顯示搜尋結果顯示網頁的標題。

> [!NOTE]
> 根據預設，Visual Studio 會將設定`<title>`未命名頁面 」 的主版頁面中的項目。 同樣地，新的 ASP.NET 頁面都有其`<title>`太設為 「 未命名的頁面 」。 因為很容易會忘記若要設定為適當值的網頁的標題，許多頁面上沒有標題為 「 未命名的頁 」 網際網路。 搜尋具有此標題的網頁 Google 傳回大約 2,460,000 結果。 即使 Microsoft 很容易發行網頁標題為 「 未命名頁面 」。 在撰寫本文時，Google 搜尋會報告這類 236 Microsoft.com 網域的 web 網頁。


ASP.NET 網頁可以下列方式之一，指定其標題：

- 將值直接內`<title>`項目
- 使用`Title`屬性`<%@ Page %>`指示詞
- 以程式設計方式設定頁面的`Title`屬性類似的程式碼`Page.Title="title"`或`Page.Header.Title="title"`。

內容頁面沒有`<title>`元素，因為它主版頁面中定義。 因此，若要設定內容頁面的標題您可以使用`<%@ Page %>`指示詞的`Title`屬性，或以程式設計方式設定它。

### <a name="setting-the-pages-title-declaratively"></a>以宣告方式設定頁面的標題

以宣告方式透過設定內容頁面的標題`Title`屬性[`<%@ Page %>`指示詞](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx)。 這個屬性可以設定直接修改`<%@ Page %>`指示詞，或透過 [屬性] 視窗。 讓我們看看這兩種方法。

從原始碼檢視中，找出`<%@ Page %>`指示詞，也就是在網頁的宣告式標記的頂端。 `<%@ Page %>`指示詞`Default.aspx`遵循：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>`指示詞會指定 ASP.NET 引擎剖析和編譯的頁面時所使用的網頁專用屬性。 這包括其主版頁面檔案、 其程式碼檔案，且其標題，以及其他資訊的位置。

根據預設，當建立新的內容頁面，設定 Visual Studio`Title`屬性設定為 「 未命名頁面 」。 變更`Default.aspx`的`Title`"主版頁面教學課程 」 從 「 未命名的頁 」 屬性，然後再檢視透過瀏覽器頁面。 圖 1 顯示在瀏覽器標題列，會反映新的頁面標題。


![在瀏覽器標題列現在會顯示&quot;主版頁面教學課程&quot;而不是&quot;未命名的頁面&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**圖 01**： 瀏覽器的標題列現在會顯示"主版頁面教學課程 」 而不是 「 未命名的頁 」


從 [屬性] 視窗時，可能也設定頁面的標題。 從 [屬性] 視窗中，選取文件從下拉式清單載入頁面層級屬性，其中包括`Title`屬性。 圖 2 顯示 [屬性] 視窗之後`Title`已設定為"主版頁面教學課程 」。


![您可能會太設定從 [屬性] 視窗標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**圖 02**： 您可能會太設定從 [屬性] 視窗標題


### <a name="setting-the-pages-title-programmatically"></a>以程式設計方式設定頁面的標題

主版頁面的`<head runat="server">`標記會轉譯成[`HtmlHead`類別](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.aspx)ASP.NET 引擎所呈現的頁面時執行個體。 `HtmlHead`類別具有[`Title`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.title.aspx)其值會反映在轉譯`<title>`項目。 這個屬性是可從 ASP.NET 網頁的程式碼後置類別透過存取`Page.Header.Title`; 此相同屬性也可以透過存取`Page.Title`。

若要練習以程式設計方式設定頁面的標題，請瀏覽至`About.aspx`網頁的程式碼後置類別，並建立網頁的事件處理常式`Load`事件。 接下來，設定頁面的標題為 「 主版頁面教學課程:: 有關::*日期*"，其中*日期*是目前日期。 加入此程式碼之後您`Page_Load`事件處理常式看起來應該類似下列：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

圖 3 顯示瀏覽器的標題列，瀏覽時`About.aspx`頁面。


![在頁面的標題是以程式設計方式設定，並且包含目前的日期](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**圖 03**: 頁面標題是以程式設計方式設定，並且包含目前的日期


## <a name="step-2-automatically-assigning-a-page-title"></a>步驟 2： 自動指派的頁面標題

如我們所見在步驟 1 中，以宣告方式或以程式設計方式可以設定頁面的標題。 如果您忘記明確地將標題變更為更具描述性，不過，您的頁面將具有預設的標題，未命名頁面 」。 在理想情況下，網頁的標題會自動設定為我們，我們沒有明確地指定其值。 例如，如果在執行階段頁面的標題是 「 未命名頁面 」，我們可能需要自動更新為與 ASP.NET 網頁的檔案名稱相同的標題。 好消息是工作的，使用一些很可能已自動指派給項目的前方。

所有的 ASP.NET web pages 是衍生自`Page`System.Web.UI 命名空間中的類別。 `Page`類別定義 ASP.NET 網頁所需的基本功能，而且會公開必要的屬性，例如`IsPostBack`， `IsValid`， `Request`，和`Response`，還有許多其他。 有時候，web 應用程式中的每一頁會需要額外的功能。 提供此為常見方式是建立自訂的基底頁類別。 自訂的基底頁面類別是的類別建立衍生自`Page`類別，並包含額外的功能。 一旦建立此基底類別，您可以讓您從它衍生的 ASP.NET 網頁 (而非`Page`類別)，藉此供應項目加入 ASP.NET 網頁的擴充的功能。

在此步驟中，我們會建立基底的頁面，如果標題不否則已明確設定，自動設定 ASP.NET 網頁的檔名的網頁的標題。 步驟 3 會查看設定頁面的標題，根據站台對應。

> [!NOTE]
> 建立和使用自訂的基底頁類別的詳盡超出此教學課程系列的範圍。 如需詳細資訊，請參閱[使用自訂基底類別來代表您的 ASP.NET 網頁的程式碼後置類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)。


### <a name="creating-the-base-page-class"></a>建立基底類別

我們第一個工作是建立頁面基底類別，也就是該類別可擴充`Page`類別。 啟動新增`App_Code`至 方案總管 中的專案名稱上按一下滑鼠右鍵，選擇 加入 ASP.NET 資料夾，然後選取您的專案資料夾`App_Code`。 接下來，以滑鼠右鍵按一下`App_Code`資料夾並加入新類別，名為`BasePage.vb`。 圖 4 顯示在 [方案總管] 中之後`App_Code`資料夾和`BasePage.vb`類別已新增。


![新增 App_Code 資料夾，名為 BasePage 類別](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**圖 04**： 新增`App_Code`資料夾和類別，名為`BasePage`


> [!NOTE]
> Visual Studio 支援的專案管理的兩種模式： 網站專案和 Web 應用程式專案。 `App_Code`資料夾設計來搭配網站專案模型。 如果您使用 Web 應用程式專案模型，放入`BasePage.vb`有別於資料夾中的類別`App_Code`，例如`Classes`。 如需有關本主題的詳細資訊，請參閱[移轉至 Web 應用程式專案的網站專案](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx)。


自訂的基底網頁做為 ASP.NET 網頁程式碼後置類別的基底類別，因為它需要擴充`Page`類別。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

每當 ASP.NET 網頁要求時便會繼續執行一連串的階段，要求的頁面被轉譯為 HTML 中累積。 我們可以藉由覆寫到階段點選`Page`類別的`OnEvent`方法。 我們的基底 頁面中讓我們來自動設定的標題，如果它尚未明確指定所`LoadComplete`階段 (其中，您可能會有猜到，就會發生之後`Load`階段)。

若要完成這項作業，覆寫`OnLoadComplete`方法並輸入下列程式碼：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete`方法會判斷是否啟動`Title`屬性具有尚未明確設定。 如果`Title`屬性是`Nothing`，空字串，或它的值未命名頁面 」，將它指派給要求的 ASP.NET 網頁的檔名。 要求的 ASP.NET 頁面-的實體路徑`C:\MySites\Tutorial03\Login.aspx`，例如-則可透過`Request.PhysicalPath`屬性。 `Path.GetFileNameWithoutExtension`方法用來拉出剛才的檔名部分，並接著這個檔案名稱會指派給`Page.Title`屬性。

> [!NOTE]
> 邀請您強化這個邏輯，可改善標題的格式。 例如，如果頁面的檔案名稱是`Company-Products.aspx`，上述程式碼會產生標題為 「 公司-產品 」，但在理想情況下繪製虛線會變成一個空格，如 「 公司產品 」 所示。 此外，請考慮加入空格，每當大小變更時。 也就是說，請考慮將轉換檔案名稱的程式碼加入`OurBusinessHours.aspx`標題的 「 我們上班時間 」。


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>將繼承基底類別的內容頁面

我們現在需要更新我們的網站是衍生自自訂基礎頁面中的 ASP.NET 網頁 (`BasePage`) 而不是`Page`類別。 若要完成請移至每個程式碼後置類別並將變更從類別宣告：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

收件者:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

之後，請造訪網站透過瀏覽器。 如果您瀏覽的網頁的標題會明確設定，例如`Default.aspx`或`About.aspx`，會使用明確指定的標題。 如果，不過，您瀏覽其標題尚未變更預設值 （未命名頁面 」） 的頁面，頁面基底類別會設定要在頁面的檔案名稱的標題。

圖 5 顯示`MultipleContentPlaceHolders.aspx`頁面上透過瀏覽器檢視時。 請注意，標題精確 （較不擴充功能），頁面的檔案名稱"MultipleContentPlaceHolders"。


[![如果未明確指定標題，在頁面的檔案名稱是自動使用](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**圖 05**： 如果未明確指定標題，在頁面的檔案名稱是自動使用 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>步驟 3： 基礎網站導覽中的頁面標題

ASP.NET 提供了強固的站台對應架構，可讓網頁開發人員在外部資源 （例如 XML 檔案或資料庫資料表） 以及 Web 控制項，顯示資訊 （例如 SiteMapPath 網站導覽中定義階層式網站地圖功能表和 TreeView 控制項）。

從 ASP.NET 網頁的程式碼後置類別也存取以程式設計方式網站導覽結構。 以這種方式我們可以自動將其對應的節點標題網頁的標題設定網站導覽中。 讓我們來增強`BasePage`，讓它提供這項功能，在步驟 2 中建立的類別。 但是，我們必須先建立站台的站台對應。

> [!NOTE]
> 本教學課程假設讀者已熟悉 ASP。對應網路的站台的功能。 如需使用站台對應的詳細資訊，請參閱我多部分文章系列，[檢查 ASP。網路的站台瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)。


### <a name="creating-the-site-map"></a>建立站台對應

站台對應系統建置在之上[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，其中以減少站台對應應用程式開發介面會序列化記憶體和持續性存放區之間的站台對應資訊的邏輯。 .NET Framework 隨附[`XmlSiteMapProvider`類別](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx)，這是預設站台地圖提供者。 正如其名，`XmlSiteMapProvider`做為其站台對應存放區使用的 XML 檔案。 讓我們使用此提供者來定義我們網站地圖。

藉由建立名為網站的根資料夾中的站台對應檔案啟動`Web.sitemap`。 若要完成此動作，以滑鼠右鍵按一下方案總管] 中的網站名稱，選擇 [加入新項目並選取網站地圖範本。 請確定檔案命名為`Web.sitemap`並按一下 [新增]。


[![加入名為 Web.sitemap 到網站的根資料夾的檔案](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**圖 06**： 將檔案命名為`Web.sitemap`到網站的根資料夾 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


加入下列 XML 以`Web.sitemap`檔案：


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

這段 XML 會定義階層式網站導覽結構圖 7 所示。


![站台對應是目前組成的三個站台對應節點](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**圖 07**: 網站地圖是目前組成的三個站台對應節點


當我們新增了新的範例，我們將在未來教學課程中更新網站導覽結構。

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>更新包括瀏覽 Web 控制項的主版頁面

現在，我們已經定義站台對應，讓我們來更新包括瀏覽 Web 控制項的主版頁面。 具體來說，讓我們加入 ListView 控制項中呈現網站導覽中定義的每個節點的清單項目使用的未排序的清單的課程區段的左邊資料行。

> [!NOTE]
> ListView 控制項的新 asp.net 3.5 版。 如果您使用舊版 ASP.NET，請改為使用在中繼器控制項。 如需有關 ListView 控制項的詳細資訊，請參閱[使用 ASP.NET 3.5 的 ListView 和 DataPager 控制項](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


開始課程 > 一節，移除現有的未排序的清單標記。 接下來，將 ListView 控制項從 [工具箱] 拖放課程下方標題。 ListView 位於 [工具箱] 旁邊的檢視控制項的資料區段中： GridView、 DetailsView 和 FormView。 設定 ListView 的`ID`屬性`LessonsList`。

從資料來源組態精靈選擇繫結至新的 Treeview 控制項，名為的 ListView `LessonsDataSource`。 Treeview 控制項站台對應系統從傳回階層式結構。


[![Treeview 控制項繫結至 LessonsList ListView 控制項](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**圖 08**: Treeview 控制項繫結至 LessonsList ListView 控制項 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


在建立之後 Treeview 控制項，我們需要定義 ListView 的範本，讓它呈現 Treeview 控制項所傳回的每個節點的清單項目使用的未排序的清單。 這可以使用下列範本標記完成：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate`產生的未排序清單的標記 (`<ul>...</ul>`) 時`ItemTemplate`呈現 Treeview 做為清單項目所傳回的每個項目 (`<li>`)，其中包含特定課程的連結。

設定 ListView 的範本之後, 造訪的網站。 如圖 9 所示，這個課程區段包含單一項目符號項目，首頁。 關於和使用多個 ContentPlaceHolder 控制項課程哪裡？ Treeview 設計為傳回一組階層式資料，但 ListView 控制項只能顯示單一階層的層級。 因此，會顯示傳回的 Treeview 的站台對應節點中的第一個層級。


[![課程 > 一節包含單一清單項目](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**圖 09**： 課程 > 一節包含單一的清單項目 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


若要顯示多個層級能以巢狀內的多個 Listview `ItemTemplate`。 這項技術有密切關係中[*主版頁面和站台瀏覽*教學課程](../../data-access/introduction/master-pages-and-site-navigation-vb.md)的我[使用資料的教學課程系列](../../data-access/index.md)。 不過，此教學課程系列我們網站地圖將包含只兩個層級： 首頁 （最上層）;及首頁的子系為每個課程。 製作巢狀的 ListView，我們可以改用指示來設定傳回開始節點 Treeview 其[`ShowStartingNode`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)至`False`。 最後的結果會是 Treeview 開始透過傳回第二層的站台對應的節點。

這項變更，清單檢視會顯示關於項目符號項目並使用多個 ContentPlaceHolder 控制項課程，但省略 Home 的項目。 若要補救這種情況，我們可以明確加入項目中的家用`LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

藉由設定 Treeview 省略開始節點，並明確地加入首頁項目符號項目、 課程區段現在會顯示預期的輸出。


[![課程章節包含的項目符號項目首頁及每一個子節點](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**圖 10**: 課程章節包含的項目符號項目首頁和每一個子節點 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>設定站台地圖為基礎的標題

與就地站台對應，我們可以更新我們`BasePage`類別，以使用站台對應中指定的標題。 如同在步驟 2 中，我們只想要使用的站台對應節點標題，如果頁面標題尚未明確設定，網頁開發人員。 如果所要求的頁面並沒有明確設定頁面上的標題和找不到站台對應，則我們會改為使用要求的網頁檔名 （較不擴充功能），在步驟 2 中一樣。 圖 11 說明此決策程序。


![如果沒有明確設定頁面標題，會使用對應站台對應的節點標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**圖 11**: 明確地設定網頁標題不存在，會使用對應站台對應的節點標題


更新`BasePage`類別的`OnLoadComplete`方法，將包含下列程式碼：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

如往常一般，`OnLoadComplete`方法會啟動，判斷是否已明確設定頁面的標題。 如果`Page.Title`是`Nothing`，空字串，或被指派的值未命名頁面 」，則程式碼會自動指派值到`Page.Title`。

若要判斷使用的標題，程式碼會啟動藉由參考[`SiteMap`類別](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)的[`CurrentNode`屬性](https://msdn.microsoft.com/en-us/library/system.web.sitemap.currentnode.aspx)。 `CurrentNode`傳回[ `SiteMapNode` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx)站台對應，對應至目前要求的頁面中的執行個體。 假設目前要求的頁面內站台對應，發現`SiteMapNode`的`Title`屬性指派給頁面的標題。 如果目前要求的頁面不是站台對應，`CurrentNode`傳回`Nothing`和要求的網頁檔名做為標題 （為 「 已完成步驟 2 中）。

圖 12 顯示`MultipleContentPlaceHolders.aspx`頁面上透過瀏覽器檢視時。 因為未明確設定此頁面的標題，會改為使用其對應站台對應節點的標題。


![從站台地圖提取 MultipleContentPlaceHolders.aspx 網頁的標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**圖 12**： 從站台地圖提取 MultipleContentPlaceHolders.aspx 網頁的標題


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>步驟 4： 加入至其他頁面的特定標記`<head>`區段

步驟 1、 2 和 3 探討了自訂`<title>`頁面的頁面為基礎的項目。 除了`<title>`、`<head>`區段可能包含`<meta>`項目和`<link>`項目。 如同稍早在本教學課程中所註明`Site.master`的`<head>`區段包含`<link>`元素`Styles.css`。 因為這`<link>`項目已定義的主版頁面中，隨附於`<head>`所有內容頁的區段。 但我們移關於新增`<meta>`和`<link>`頁面的頁面為基礎的項目嗎？

最簡單的方式指定頁面將內容加入到`<head>`區段是藉由建立 ContentPlaceHolder 控制項中的主版頁面。 我們已經有這類 ContentPlaceHolder (名為`head`)。 因此，若要加入自訂`<head>`標記中，建立對應內容頁面中的控制項，並將標記放在該處。

為了說明新增自訂`<head>`標記頁面時，讓我們來包含`<meta>`我們目前資料集的內容頁面的描述項目。 `<meta>` Description 項目提供網頁的簡短說明，大部分的搜尋引擎時顯示搜尋結果，合併某種形式的資訊。

A `<meta>` description 項目具有下列格式：


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

若要將這個標記加入至內容的頁面，加入上述文字對應至主版頁面的內容控制項`head`ContentPlaceHolder。 例如，若要定義`<meta>`描述項目`Default.aspx`，加入下列標記：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

因為`head`ContentPlaceHolder 不在 HTML 網頁主體中，加入至內容控制項的標記不會顯示在 [設計] 檢視。 若要查看`<meta>`描述項目，請造訪`Default.aspx`透過瀏覽器。 在頁面載入之後，檢視原始檔，請注意，`<head>`章節包含內容控制項中指定的標記。

請花一點時間加入`<meta>`描述項目`About.aspx`， `MultipleContentPlaceHolders.aspx`，和`Login.aspx`。

### <a name="programmatically-adding-markup-to-theheadregion"></a>以程式設計方式將標記`<head>`區域

`head` ContentPlaceHolder 可讓我們以宣告方式將自訂標記加入至主版頁面的`<head>`區域。 自訂標記可能也會以程式設計方式加入。 請記得，`Page`類別的`Header`屬性會傳回`HtmlHead`主版頁面中定義的執行個體 ( `<head runat="server">`)。

無法以程式設計方式將內容加入到`<head>`區域時，要加入的內容是動態的。 可能根據使用者瀏覽頁面;可能它是從資料庫提取。 不論原因，您可以將內容加入到`HtmlHead`所加入控制項，以其`Controls`集合就像這樣：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

上述程式碼會加入`<meta>`keywords 項目至`<head>`區域，而提供的關鍵字，描述頁面以逗號分隔清單。 請注意，若要加入`<meta>`您所建立的標記[ `HtmlMeta` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlmeta.aspx)執行個體、 將其`Name`和`Content`屬性，然後將它新增到`Header`的`Controls`集合。 同樣地，若要以程式設計方式加入`<link>`項目，建立[ `HtmlLink` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmllink.aspx)物件、 設定其屬性，並再將它加入`Header`的`Controls`集合。

> [!NOTE]
> 若要新增任意的標記，請建立[ `LiteralControl` ](https://msdn.microsoft.com/en-us/library/system.web.ui.literalcontrol.aspx)執行個體、 將其`Text`屬性，然後將它新增到`Header`的`Controls`集合。


## <a name="summary"></a>總結

在此教學課程中我們探討了各種方法來新增`<head>`區域 由頁面為基礎的標記。 主版頁面應包含`HtmlHead`執行個體 (`<head runat="server">`) 與 ContentPlaceHolder。 `HtmlHead`執行個體允許以程式設計方式存取的內容頁面`<head>`區域和宣告方式和以程式設計方式設定網頁的標題，; ContentPlaceHolder 控制項可讓自訂標記加入至`<head>`以宣告方式透過內容控制項的區段。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [動態設定 ASP.NET 網頁的標題](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [正在檢查 ASP。網路的站台瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [如何使用 HTML Meta 標記](http://searchenginewatch.com/showPage.html?page=2167931)
- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/articles/419.aspx)
- [使用 ASP.NET 3.5 的 ListView 和 DataPager 控制項](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET 網頁的程式碼後置類別中使用自訂的基底類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones 和 Suchi Banerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一頁](multiple-contentplaceholders-and-default-content-vb.md)
[下一頁](urls-in-master-pages-vb.md)
