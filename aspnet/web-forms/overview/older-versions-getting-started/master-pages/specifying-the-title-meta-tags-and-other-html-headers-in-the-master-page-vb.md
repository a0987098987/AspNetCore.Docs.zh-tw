---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: 在主版頁面 (VB) 中指定的標題、 中繼標籤及其他 HTML 標頭 |Microsoft Docs
author: rick-anderson
description: 查看定義各種不同的技術&lt;head&gt;主版頁面，從 [內容] 頁面中的項目。
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a34b11f5ec8836cfdffc3a08c9cae847232dfcd8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833426"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>指定標題、 中繼標籤及其他 HTML 標頭主版頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> 查看定義各種不同的技術&lt;head&gt;主版頁面，從 [內容] 頁面中的項目。


## <a name="introduction"></a>簡介

已在 Visual Studio 2008 中建立新的主版頁面，根據預設，兩個 ContentPlaceHolder 控制項： 一個名為`head`，位於`<head>`項目，以及一個名為`ContentPlaceHolder1`、 放置 Web 表單中。 目的`ContentPlaceHolder1`是可以自訂頁面的頁面為基礎的 Web Form 中定義的區域。 `head` ContentPlaceHolder 可讓頁面以新增自訂內容`<head>`一節。 （當然，這些兩個 ContentPlaceHolders 可以修改或移除，而其他 ContentPlaceHolder 可能會加入主版頁面。 我們的主版頁面， `Site.master`，目前有四個 ContentPlaceHolder 控制項。)

HTML`<head>`項目做為網頁文件不包含在文件本身的相關資訊的存放庫。 這包括資訊，例如 web 頁面的標題、 使用搜尋引擎或內部的編目程式，並連結至外部資源，例如 RSS 摘要、 JavaScript 和 CSS 檔案的中繼資訊。 這項資訊的一些可能相關之網站中的所有頁面。 例如，您可能要全域匯入相同的 CSS 規則和每個 ASP.NET 網頁的 JavaScript 檔案。 不過，有部分`<head>`頁面特定的項目。 頁面標題是最明顯的例子。

在本教學課程中我們將探討如何定義全域和頁面特定`<head>`區段標記中的主版頁面和其內容頁中。

## <a name="examining-the-master-pagesheadsection"></a>檢查主版頁面的`<head>`區段

建立 Visual Studio 2008 的預設主版頁面檔案包含下列標記中的其`<head>`區段：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

請注意，`<head>`項目包含`runat="server"`屬性，以指出它是伺服器控制項 （而非靜態的 HTML）。 所有的 ASP.NET 頁面衍生自[`Page`類別](https://msdn.microsoft.com/library/system.web.ui.page.aspx)，位於`System.Web.UI`命名空間。 這個類別包含[`Header`屬性](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx)可存取網頁的`<head>`區域。 使用`Header`屬性，我們可以設定 ASP.NET 網頁的標題，或將額外標記新增至呈現`<head>`一節。 它是可行的然後自訂內容頁面`<head>`撰寫的程式碼，在頁面中的項目`Page_Load`事件處理常式。 我們將探討如何以程式設計方式設定在步驟 1 中的 頁面的標題。

中所顯示的標記`<head>`上述的項目也包含名為的 ContentPlaceHolder 控制項`head`。 當內容頁面可以新增自訂內容，並非必要，這個 ContentPlaceHolder 控制項`<head>`項目以程式設計的方式。 很有用，不過，在何處靜態將標記新增至內容頁面的情況下`<head>`來對應內容控制項，而非以程式設計方式為靜態標記的項目也可以以宣告方式新增。

除了`<title>`項目和`head`ContentPlaceHolder，主版頁面的`<head>`項目應該包含任何`<head>`-通用於所有頁面的層級標記。 在我們的網站所有頁面會都使用中定義的 CSS 規則`Styles.css`檔案。 因此，我們已更新`<head>`中的項目[*使用主版頁面建立全網站的版面配置*](creating-a-site-wide-layout-using-master-pages-vb.md)教學課程，以包含對應`<link>`項目。 我們`Site.master`主版頁面的目前`<head>`標記如下所示。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>步驟 1： 設定內容頁面的標題

網頁的標題，並透過指定`<title>`項目。 請務必設定為適當值的每個頁面的標題。 當瀏覽的頁面，其標題會顯示在瀏覽器的標題列中。 此外，書籤的頁面上，瀏覽器就會使用頁面的標題做為書籤的建議名稱。 此外，許多搜尋引擎時顯示搜尋結果顯示頁面的標題。

> [!NOTE]
> 根據預設，Visual Studio 設定`<title>`"Untitled Page"的主版頁面中的項目。 同樣地，新的 ASP.NET 網頁有其`<title>`太設為"Untitled Page"。 有，因為它可能很容易忘了設定頁面的標題設為適當值，會將許多網頁位於數個標題為"Untitled Page"網際網路。 具有此標題的網頁搜尋 Google 傳回大約 2,460,000 的結果。 Microsoft 也很容易發行的網頁標題為"Untitled Page"。 在撰寫本文時，Google 搜尋會報告這類 236 Microsoft.com 網域的 web 網頁。


ASP.NET 頁面可以指定其標題中的下列方法之一：

- 將值直接內`<title>`項目
- 使用`Title`屬性中`<%@ Page %>`指示詞
- 以程式設計方式設定頁面`Title`類似的程式碼的屬性`Page.Title="title"`或`Page.Header.Title="title"`。

內容頁面沒有`<title>`元素，因為它主版頁面中定義。 因此，若要設定內容頁面的標題您可以使用`<%@ Page %>`指示詞的`Title`屬性，或以程式設計方式設定它。

### <a name="setting-the-pages-title-declaratively"></a>以宣告方式設定頁面的標題

透過以宣告方式設定內容頁面的標題`Title`的屬性[`<%@ Page %>`指示詞](https://msdn.microsoft.com/library/ydy4x04a.aspx)。 會設定這個屬性，請直接修改`<%@ Page %>`指示詞，或透過 [屬性] 視窗。 讓我們看看這兩種方法。

從原始碼 檢視中，找出`<%@ Page %>`指示詞，也就是在頁面的宣告式標記的頂端。 `<%@ Page %>`指示詞`Default.aspx`遵循：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>`指示詞會指定 ASP.NET 引擎剖析和編譯的頁面時所使用的網頁專用屬性。 這包括其主版頁面檔案、 其程式碼檔案，以及其標題，以及其他資訊的位置。

根據預設，當建立新的內容頁面，Visual Studio 會將`Title`屬性設定為"Untitled Page"。 變更`Default.aspx`的`Title`"Untitled page"屬性設定為 「 主版頁面教學課程 」，然後再檢視透過瀏覽器頁面。 [圖 1] 顯示瀏覽器的標題列，會反映新的頁面標題。


![瀏覽器的標題列現在會顯示&quot;主版頁面教學課程&quot;而不是&quot;未命名的頁面&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**圖 01**： 瀏覽器的標題列現在會顯示 「 主版頁面教學課程 」 而不是"Untitled Page"


從 [屬性] 視窗，可能也會設定頁面的標題。 從 [屬性] 視窗中，選取文件從下拉式清單中載入頁面層級屬性，其中包括`Title`屬性。 [圖 2] 顯示之後的 [屬性] 視窗`Title`已設為 「 主版頁面教學課程 」。


![您也可以設定從 [屬性] 視窗的標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**圖 02**： 您也可以設定從 屬性 視窗的標題


### <a name="setting-the-pages-title-programmatically"></a>以程式設計方式設定頁面的標題

主版頁面`<head runat="server">`標記會轉譯成[`HtmlHead`類別](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx)ASP.NET 引擎所呈現的頁面時，執行個體。 `HtmlHead`類別具有[`Title`屬性](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx)其值會反映在轉譯`<title>`項目。 這個屬性是從 ASP.NET 頁面的程式碼後置類別，透過存取`Page.Header.Title`; 此相同屬性也可以透過存取`Page.Title`。

若要練習以程式設計方式設定頁面的標題，請瀏覽至`About.aspx`頁面的程式碼後置類別，並建立頁面的事件處理常式`Load`事件。 接下來，設定頁面的標題為 「 主版頁面教學課程:: 相關::*日期*"，其中*日期*是目前的日期。 新增此程式碼後您`Page_Load`事件處理常式看起來應該如下所示：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

圖 3 顯示瀏覽器的標題列，瀏覽時`About.aspx`頁面。


![頁面的標題是以程式設計方式設定，並包含目前的日期](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**圖 03**: 頁面的標題是以程式設計方式設定，並包含目前的日期


## <a name="step-2-automatically-assigning-a-page-title"></a>步驟 2： 自動指派的頁面標題

如我們在步驟 1 中所見的就可以設定頁面的標題以宣告方式或以程式設計的方式。 如果您忘記要明確變更為更具描述性的標題，不過，您的頁面會有預設的標題，"Untitled Page"。 在理想情況下，頁面的標題會自動設為我們，我們未明確指定其值。 比方說，如果在執行階段頁面的標題會是"Untitled Page"，我們可能會想要自動更新，而 ASP.NET 網頁的檔案名稱相同的標題。 好消息是，利用一小段的事前工作，就可以將自動指派的標題。

所有的 ASP.NET web pages 是衍生自`Page`System.Web.UI 命名空間中的類別。 `Page`類別定義 ASP.NET 網頁所需的最小功能，而且會公開基本的屬性，例如`IsPostBack`， `IsValid`， `Request`，和`Response`，還有其他更多。 通常，web 應用程式中的每一頁所需要的其他功能。 提供這樣的常見方式是建立自訂的基底頁面類別。 自訂的基底頁面類別是的類別建立衍生自`Page`類別，並包含其他功能。 一旦建立此基底類別，您可以讓您從它衍生的 ASP.NET 頁面 (而非`Page`類別)，藉此供應項目至您的 ASP.NET 頁面的擴充的功能。

在此步驟中，我們會建立一個基底的網頁，如果標題不否則已明確設定，就會自動設定 ASP.NET 網頁的檔名的頁面的標題。 步驟 3 會尋找設定頁面的標題，根據站台對應。

> [!NOTE]
> 建立及使用自訂基底的頁面類別的徹底的檢查已超出本教學課程系列的範圍。 如需詳細資訊，請參閱[使用自訂的基底類別，您的 ASP.NET 網頁的程式碼後置類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)。


### <a name="creating-the-base-page-class"></a>建立 Page 基底類別

我們的第一個工作是建立基底的頁面類別，也就是該類別可擴充`Page`類別。 加上啟動`App_Code`至 [方案總管] 中的專案名稱上按一下滑鼠右鍵、 選擇加入 ASP.NET 資料夾，然後選取您的專案資料夾`App_Code`。 接下來，以滑鼠右鍵按一下`App_Code`資料夾，並加入新類別，名為`BasePage.vb`。 圖 4 顯示 方案總管，在後`App_Code`資料夾和`BasePage.vb`類別已新增。


![加入 App_Code 資料夾和名為 BasePage 類別](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**圖 04**： 新增`App_Code`資料夾和名為的類別 `BasePage`


> [!NOTE]
> Visual Studio 支援兩種專案管理模式： 網站專案和 Web 應用程式專案。 `App_Code`資料夾設計來搭配網站專案模型。 如果您使用 Web 應用程式專案模型，放`BasePage.vb`有別於資料夾中的類別`App_Code`，例如`Classes`。 如需有關本主題的詳細資訊，請參閱[移轉至 Web 應用程式專案的網站專案](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx)。


自訂的基底頁面做為 ASP.NET 網頁的程式碼後置類別的基底類別，因為它需要擴充`Page`類別。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

每當 ASP.NET 網頁要求時便會繼續透過一連串的階段，已累積可要求轉譯成 HTML 的頁面。 我們可以藉由覆寫到階段點選`Page`類別的`OnEvent`方法。 我們的基底 頁面中讓我們自動設定的標題，如果它尚未明確指定所`LoadComplete`階段 (其中，您可能已經猜到，就會發生之後`Load`階段)。

若要達成此目的，覆寫`OnLoadComplete`方法並輸入下列程式碼：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete`方法會判斷如果`Title`屬性具有尚未明確設定。 如果`Title`屬性是`Nothing`，空字串，或具有值"Untitled Page"，將它指派給要求的 ASP.NET 網頁的檔案名稱。 要求的 ASP.NET 頁面-的實體路徑`C:\MySites\Tutorial03\Login.aspx`，例如-就是可透過存取`Request.PhysicalPath`屬性。 `Path.GetFileNameWithoutExtension`取出 filename 而已，使用方法，而且這個檔案名稱接著會指派給`Page.Title`屬性。

> [!NOTE]
> 歡迎您強化此邏輯，可改善的標題格式。 比方說，如果網頁的檔名為`Company-Products.aspx`，上述程式碼會產生的標題為 「 公司-產品 」，但在理想情況下會取代虛線加上空格，如 「 公司產品 」 所示。 此外，請考慮變更大小寫時，新增空格。 也就是，請考慮將轉換的檔案名稱的程式碼加入`OurBusinessHours.aspx`標題的 「 我們營業時間 」。


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>讓內容頁面繼承 Page 基底類別

我們現在需要更新我們的網站，從自訂的基底頁面衍生中的 ASP.NET 網頁 (`BasePage`) 而不是`Page`類別。 若要完成此移至每個程式碼後置類別，並從類別宣告變更：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

收件者:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

完成後，請瀏覽的網站，透過瀏覽器。 如果您瀏覽的網頁，其標題會明確設定，例如`Default.aspx`或`About.aspx`，會使用明確指定的標題。 如果，不過，您瀏覽的網頁，沒有預設值 ("Untitled Page") 變更其標題，基底的頁面類別就會設定至頁面的檔案名稱的標題。

[圖 5] 顯示`MultipleContentPlaceHolders.aspx`頁面上透過瀏覽器檢視時。 請注意，標題是精確 （較不擴充功能），頁面的檔案名稱"MultipleContentPlaceHolders 」。


[![如果未明確指定標題，網頁的檔案名稱是自動使用](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**圖 05**： 如果未明確指定標題，網頁的檔案名稱是自動使用 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>步驟 3： 基礎網站導覽中的頁面標題

ASP.NET 提供穩固的站台對應架構，可讓網頁程式開發人員定義外部資源 （例如 XML 檔案或資料庫資料表） 以及 Web 控制項來顯示資訊 （例如 SiteMapPath，網站導覽中的階層式網站導覽功能表和 TreeView 控制項）。

網站導覽結構也可以從 ASP.NET 頁面的程式碼後置類別存取以程式設計的方式。 以這種方式中，我們會自動可以網站導覽中，以設定頁面的標題，其對應的節點的標題。 讓我們來增強`BasePage`，讓它提供這項功能，在步驟 2 中建立的類別。 但是，我們必須先建立我們的網站的站台對應。

> [!NOTE]
> 本教學課程假設讀者已熟悉使用 ASP。NET 的網站對應功能。 如需有關使用站台對應的詳細資訊，請參閱我的多部分的文章系列，[檢查 ASP。NET 的網站瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)。


### <a name="creating-the-site-map"></a>建立站台對應

站台對應系統建置之上[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，其中以減少站台對應 API 從序列化網站地圖資訊記憶體和持續性存放區之間的邏輯。 .NET Framework 隨附[`XmlSiteMapProvider`類別](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)，這是預設網站導覽提供者。 正如其名， `XmlSiteMapProvider` XML 檔案做為其站台對應存放區。 讓我們使用此提供者用於定義我們的站台對應。

建立名為網站的根資料夾中的網站地圖檔案著手`Web.sitemap`。 若要這麼做，以滑鼠右鍵按一下方案總管] 中的網站名稱、 選擇 [加入新項目並選取站台對應範本。 請確定檔案會命名為`Web.sitemap`並按一下 [新增]。


[![新增名為網站的根資料夾的 Web.sitemap 檔案](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**圖 06**： 將檔案命名為`Web.sitemap`網站的根資料夾 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


新增下列 XML 來`Web.sitemap`檔案：


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

這個 XML 會定義階層式網站導覽結構 [圖 7] 所示。


![站台對應是目前包含的三個網站導覽節點](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**圖 07**: 站台地圖是目前包含的三個網站導覽節點


當我們加入新的範例，我們將在未來的教學課程中更新的網站導覽結構。

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>更新包括瀏覽 Web 控制項的主版頁面

既然我們已經定義站台對應，讓我們更新包括瀏覽 Web 控制項的主版頁面。 具體來說，讓我們新增的 ListView 控制項中呈現在網站導覽中定義的每個節點的清單項目使用的未排序的清單的 [課程] 區段的左資料行。

> [!NOTE]
> ListView 控制項是 asp.net 3.5 版的新功能。 如果您使用舊版 ASP.NET，請改為使用重複項控制項。 如需有關在 ListView 控制項的詳細資訊，請參閱[使用 ASP.NET 3.5 的 ListView 和 DataPager 控制項](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


啟動 [課程] 區段中移除現有的未排序的清單標記。 接下來，將 ListView 控制項從 [工具箱] 拖放下方課程標題。 ListView 位於 [工具箱] 中的檢視控制項旁的 [資料] 區段中： GridView、 DetailsView 和 FormView。 設定 ListView`ID`屬性設`LessonsList`。

從資料來源組態精靈 中，選擇要繫結至名為的新 SiteMapDataSource 控制項的 ListView `LessonsDataSource`。 SiteMapDataSource 控制項從站台對應系統傳回階層式結構。


[![將 SiteMapDataSource 控制項繫結至 LessonsList ListView 控制項](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**圖 08**： 將 SiteMapDataSource 控制項繫結至 LessonsList ListView 控制項 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


建立之後 SiteMapDataSource 控制項，我們需要定義 ListView 的範本，讓它呈現 SiteMapDataSource 控制項所傳回的每個節點的清單項目使用的未排序的清單。 這可以使用下列的範本標記來完成：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate`產生的未排序清單的標記 (`<ul>...</ul>`) 時`ItemTemplate`呈現為清單項目，SiteMapDataSource 所傳回的每個項目 (`<li>`)，其中包含特定的課程的連結。

之後設定 ListView 的範本，請瀏覽網站。 如 [圖 9] 所示，[課程] 區段會包含單一項目符號項目，首頁。 其中是關於 」 和 「 使用多個 ContentPlaceHolder 控制項課程？ SiteMapDataSource 設計為傳回一組階層式資料，但 ListView 控制項只能顯示單一階層的層級。 因此，會顯示網站導覽節點，SiteMapDataSource 所傳回的第一個層級。


[![[課程] 區段包含單一的清單項目](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**圖 09**: [課程] 區段包含單一的清單項目 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


若要顯示多個層級能以巢狀內的多個 Listview `ItemTemplate`。 這項技術有密切關係中[*主版頁面與網站導覽*教學課程](../../data-access/introduction/master-pages-and-site-navigation-vb.md)的我[處理資料的教學課程系列](../../data-access/index.md)。 不過，本教學課程系列我們的網站對應中會包含只是兩個層級： 首頁 （最上層）;與每個課程做為子系的首頁。 製作巢狀的 ListView，我們可以改用指示不會返回開始節點設定 SiteMapDataSource 及其[`ShowStartingNode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)至`False`。 實質效果是，SiteMapDataSource 一開始會傳回第二層的網站導覽節點。

透過這項變更，ListView 會顯示關於項目符號項目，以及使用多個 ContentPlaceHolder 控制項經驗，但省略的項目符號項目家用。 若要解決此問題，我們可以明確加入的項目符號項目中的家用`LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

藉由設定 略過開始節點 SiteMapDataSource 並明確地將首頁的項目符號項目，課程 區段現在會顯示預期的輸出。


[![[課程] 區段包含的項目符號項目首頁和每個子節點](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**圖 10**: [課程] 區段的首頁和每一個子節點包含項目符號項目 ([按一下以檢視完整大小的影像](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>設定標題根據站台對應

使用網站中的對應位置，我們可以更新我們`BasePage`類別，以使用站台對應所指定的標題。 如同我們在步驟 2 中，我們只想要使用網站導覽節點的標題，如果頁面的標題尚未明確設定由頁面開發人員。 如果所要求的頁面並沒有明確設定頁面標題，然後中找不到站台對應，然後我們將改為使用要求的頁面檔名 （較不擴充功能），如同我們在步驟 2。 [圖 11] 說明此決策程序。


![如果沒有明確設定頁面標題，會使用對應的網站導覽節點的標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**圖 11**： 如果沒有明確設定頁面標題，會使用對應的網站導覽節點的標題


更新`BasePage`類別的`OnLoadComplete`方法以包含下列程式碼：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

同樣地，`OnLoadComplete`方法會判斷是否已明確設定頁面的標題。 如果`Page.Title`已`Nothing`、 空字串，或被指派"Untitled Page"的值，然後將程式碼會自動將值指派給`Page.Title`。

若要判斷使用的標題，程式碼會開始藉由參考[`SiteMap`類別](https://msdn.microsoft.com/library/system.web.sitemap.aspx)的[`CurrentNode`屬性](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)。 `CurrentNode` 會傳回[ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)對應至目前要求的網頁的網站導覽中的執行個體。 假設目前要求的頁面中站台對應中找到`SiteMapNode`的`Title`屬性指派給頁面的標題。 如果目前要求的頁面不是在站台模式中，`CurrentNode`傳回`Nothing`和要求的頁面檔名做為標題 （如已在步驟 2 中所執行）。

[圖 12] 顯示`MultipleContentPlaceHolders.aspx`頁面上透過瀏覽器檢視時。 因為沒有明確設定此頁面的標題，會改為使用其對應的網站導覽節點的標題。


![從網站導覽提取 MultipleContentPlaceHolders.aspx 頁面的標題](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**圖 12**: MultipleContentPlaceHolders.aspx 頁面的標題取自網站導覽


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>步驟 4： 加入至其他頁面特定標記`<head>`區段

步驟 1、 2 和 3 探討了自訂`<title>`頁面的頁面為基礎的項目。 除了`<title>`，則`<head>`區段可能包含`<meta>`項目和`<link>`項目。 如稍早在本教學課程中所述`Site.master`的`<head>`一節包含`<link>`項目`Styles.css`。 因為這`<link>`項目會定義內的主版頁面、 它包含在`<head>`的所有內容頁面的區段。 但我們輕鬆地將`<meta>`和`<link>`頁面的頁面為基礎的項目嗎？

最簡單的方式，將頁面特定內容`<head>`區段是藉由建立 ContentPlaceHolder 控制項中的主版頁面。 我們已經有這類 ContentPlaceHolder (名為`head`)。 因此，若要新增自訂`<head>`標記中，建立對應內容頁面中的控制項，並將標記放至該處。

為了說明新增自訂`<head>`標記頁面，請包含`<meta>`到我們的內容頁面的目前集合的 description 項目。 `<meta>` Description 項目提供網頁的簡短說明; 顯示搜尋結果時，大部分的搜尋引擎會納入某種形式的這項資訊。

A `<meta>` description 項目具有下列格式：


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

若要將此標記新增至內容頁面中，將上述的文字新增至內容控制項對應至主版頁面的`head`ContentPlaceHolder。 例如，若要定義`<meta>`描述項目`Default.aspx`，加入下列標記：


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

因為`head`ContentPlaceHolder 不在 HTML 頁面的主體內，新增至內容控制項的標記不會顯示在 [設計] 檢視。 若要查看`<meta>`描述項目，請造訪`Default.aspx`透過瀏覽器。 在頁面載入之後，檢視原始檔，並注意`<head>`區段包含內容控制項中所指定的標記。

請花一點時間加入`<meta>`描述項目`About.aspx`， `MultipleContentPlaceHolders.aspx`，和`Login.aspx`。

### <a name="programmatically-adding-markup-to-theheadregion"></a>以程式設計方式將標記`<head>`區域

`head` ContentPlaceHolder 可讓我們以宣告方式將自訂標記新增至主版頁面的`<head>`區域。 自訂標記可能也會以程式設計方式加入。 請記得，`Page`類別的`Header`屬性會傳回`HtmlHead`主版頁面中定義的執行個體 ( `<head runat="server">`)。

能夠以程式設計方式將內容加入至`<head>`區域時，要加入的內容是動態的。 可能是它根據使用者瀏覽頁面;也許它是從資料庫提取。 不論原因，您可以將內容加入至`HtmlHead`藉由將控制項加入其`Controls`集合就像這樣：


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

上述程式碼會新增`<meta>`keywords 項目至`<head>`提供以逗號分隔的關鍵字清單，描述頁面的區域。 請注意，新增`<meta>`您所建立的標記[ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx)執行個體、 將其`Name`並`Content`屬性，然後將它新增到`Header`的`Controls`集合。 同樣地，若要以程式設計方式新增`<link>`項目，建立[ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx)物件，設定其屬性，然後再將它加入`Header`的`Controls`集合。

> [!NOTE]
> 若要新增任意的標記，建立[ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx)執行個體、 將其`Text`屬性，然後將它新增到`Header`的`Controls`集合。


## <a name="summary"></a>總結

在本教學課程中我們探討了以各種方式將`<head>`頁面的頁面為基礎的區域標記。 主版頁面應該包含`HtmlHead`執行個體 (`<head runat="server">`) 與 ContentPlaceHolder。 `HtmlHead`執行個體允許以程式設計方式存取的內容頁面`<head>`區域，以宣告方式或以程式設計方式設定的頁面標題; ContentPlaceHolder 控制項可讓自訂標記新增至`<head>`以宣告方式是透過內容控制項的一節。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [以動態方式設定在 ASP.NET 中的 頁面的標題](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [正在檢查 ASP。NET 的網站巡覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [如何使用 HTML 中繼標籤](http://searchenginewatch.com/showPage.html?page=2167931)
- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/articles/419.aspx)
- [使用 ASP.NET 3.5 的 ListView 和 DataPager 控制項](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [使用自訂的基底類別的 ASP.NET 頁面的程式碼後置類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones 和 Suchi Banerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](multiple-contentplaceholders-and-default-content-vb.md)
> [下一頁](urls-in-master-pages-vb.md)
