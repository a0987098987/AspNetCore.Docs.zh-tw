---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: 主版頁面 (VB) 中的 Url |Microsoft Docs
author: rick-anderson
description: 解決方式中的主版頁面的 Url 可能會中斷由於在不同於 [內容] 頁面相對目錄中的主版頁面檔案。 查看重定基底...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 881debeeaa98a7f2be7ccadb501c019e698b22f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831760"
---
<a name="urls-in-master-pages-vb"></a>主版頁面 (VB) 中的 Url
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> 解決方式中的主版頁面的 Url 可能會中斷由於在不同於 [內容] 頁面相對目錄中的主版頁面檔案。 查看 重定基底 Url，透過 ~ 的宣告式語法和以程式設計方式使用 ResolveUrl ResolveClientUrl 中。 （也請看


## <a name="introduction"></a>簡介

在所有範例中我們已了解到目前為止，主版頁面和內容頁面必須位於相同的資料夾 （網站的根資料夾）。 但是，沒有的理由為何主版頁面和內容頁面必須位於相同的資料夾。 您當然可以建立子資料夾中的內容頁面。 同樣地，您可以在其中建立`~/MasterPages/`您放置您的網站主版頁面的資料夾。

一個可能的問題，以將主要和內容頁面放入不同的資料夾包含中斷的 Url。 如果主版頁面中包含超連結、 影像或其他項目中的相對 Url，連結將會無效之位於不同的資料夾中的內容頁面。 在本教學課程中，我們檢查此問題，以及因應措施的來源。

## <a name="the-problem-with-relative-urls"></a>使用相對 Url 的問題

在網頁上的 URL 即為*相對的 URL*如果它所指向之資源的位置是相對於網站的資料夾結構中的 web 網頁的位置。 任何不是以正斜線開頭的 URL (`/`) 或通訊協定 (例如`http://`) 是相對的因為它的解決方式根據位置包含 URL 的網頁瀏覽器。

比方說，我們的網站有`~/Images/`資料夾中的，使用單一影像檔， `PoweredByASPNET.gif`。 主版頁面檔案`Site.master`已經`<img>`中的項目`footerContent`區域，以下列標記：


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

`src`屬性中的值`<img>`項目是相對的 URL，因為它不是以開頭`/`或`http://`。 簡單來說，`src`屬性值會告知瀏覽器中呈現的外觀`Images`子資料夾，名為的檔案`PoweredByASPNET.gif`。

瀏覽時內容的頁面，上述標記會直接傳送至瀏覽器。 請花一點時間瀏覽`About.aspx`以及檢視傳送至瀏覽器的 HTML 原始檔。 您會發現主版頁面中完全相同的標記已傳送至瀏覽器。


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

如果是根資料夾中的 [內容] 頁面 (現狀`About.aspx`) 一切都會如預期般運作，因為沒有`Images`相對於根資料夾的子資料夾。 不過，項目細分為 [內容] 頁面是否在主版頁面不同的資料夾。 為了說明這點，建立名為`Admin`。 接下來，新增名為的內容頁面`Default.aspx`要`Admin`資料夾，並確認其繫結至新的頁面`Site.master`主版頁面。

> [!NOTE]
> 在  [*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中我們建立名為自訂的基底頁面類別`BasePage`，自動設定 [內容] 頁面的標題 (如果它未明確指派給）。 別忘了有新建立的網頁程式碼後置類別衍生自`BasePage`以便讓它能夠利用這項功能。


您已建立此內容的頁面之後，您的方案總管] 中看起來應該類似於 [圖 1。


![新的資料夾和 ASP.NET 網頁加入至專案](urls-in-master-pages-vb/_static/image1.png)

**圖 01**： 新的資料夾和 ASP.NET 網頁加入至專案


接下來，更新`Web.sitemap`檔案，以包含新`<siteMapNode>`這一課中的項目。 下列 XML 會顯示完整`Web.sitemap`標記，現在包含加入的第三個`<siteMapNode>`項目。


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

新建`Default.aspx`頁面上應該有四個內容控制項，對應至在四個 ContentPlaceHolders `Site.master`。 將一些文字新增至內容控制項參考`MainContent`ContentPlaceHolder，然後瀏覽透過瀏覽器頁面。 如 [圖 2] 所示，在瀏覽器找不到`PoweredByASPNET.gif`映像檔。 什麼呢？

`~/Admin/Default.aspx`內容頁面會傳送相同的 HTML`footerContent`區域一樣`About.aspx`頁面：


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

因為`<img>`項目的`src`屬性為相對 URL，瀏覽器會嘗試尋找`Images`相對於網頁的資料夾位置的資料夾。 換句話說，在瀏覽器正在尋找映像檔`Admin/Images/PoweredByASPNET.gif`。


[![找不到 PoweredByASPNET.gif 映像檔](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**圖 02**:`PoweredByASPNET.gif`映像找不到檔案 ([按一下以檢視完整大小的影像](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>相對的 Url 取代為絕對 Url

相對 URL 的情況則相反*絕對 URL*，這是它的開頭是正斜線 (`/`) 或這類通訊協定`http://`。 絕對 URL 指定的資源，以從已知的固定點位置，因為相同的絕對 URL 無效，任何網頁，不論網站的資料夾結構中的網頁所在的位置中。

若要修復損毀的映像 [圖 2] 所示，我們需要更新`<img>`項目的`src`屬性，讓它使用而不是相對的絕對 URL。 若要判斷正確的絕對 URL，瀏覽的網頁，您的網站中的其中一個，並檢查 [網址] 列。 如圖 2 中的 網址 列所示，web 應用程式的完整的路徑是`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`。 因此，我們無法更新`<img>`項目的`src`屬性設定為下列兩個絕對的 url:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

請花一點時間更新`<img>`項目的`src`屬性設定為使用其中一種形式，如上所示的絕對 URL，然後瀏覽`~/Admin/Default.aspx`透過瀏覽器的頁面。 目前瀏覽器會正確地尋找並顯示`PoweredByASPNET.gif`映像檔案 （請參閱 [圖 3]）。


[![PoweredByASPNET.gif 映像會現在顯示](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**圖 03**:`PoweredByASPNET.gif`影像是顯示 ([按一下以檢視完整大小的影像](urls-in-master-pages-vb/_static/image7.png))


硬式編碼絕對的 URL 中的運作方式，而它緊密結合您的 HTML 網站的伺服器和資料夾位置，可能會變更。 使用格式的絕對 URL`http://localhost:3908/...`是很容易損毀，因為前面 localhost 的連接埠號碼就會自動選取每次啟動 Visual Studio 的內建 ASP.NET 開發 Web 伺服器時。 同樣地，`http://localhost`本機測試時，才有效組件。 程式碼部署至實際執行伺服器之後, 的 URL 基底會變更為其他項目，例如`http://www.yourserver.com`。 在表單中的絕對 URL`/ASPNET_MasterPages_Tutorial_04_VB/...`因為此應用程式路徑通常與開發和生產環境的伺服器之間也有相同的脆弱度。

好消息是 ASP.NET 提供方法，以產生在執行階段的有效相對 URL。

## <a name="usingandresolveclienturl"></a>使用`~`和`ResolveClientUrl`

而非硬式編碼絕對 URL，ASP.NET 可讓網頁程式開發人員使用波狀符號 (`~`) 表示的 web 應用程式的根目錄。 例如，如稍早在本教學課程中我可以使用標記法`~/Admin/Default.aspx`中的文字，請參閱`Default.aspx`頁面中`Admin`資料夾。 `~`表示`Admin`資料夾是 web 應用程式的根的子資料夾。

`Control`類別的[`ResolveClientUrl`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)採用 URL，並修改它適用於控制項所在的網頁的相對 url。 例如，呼叫`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`從`About.aspx`傳回`Images/PoweredByASPNET.gif`。 從呼叫`~/Admin/Default.aspx`，不過，會傳回`../Images/PoweredByASPNET.gif`。

> [!NOTE]
> 因為所有的 ASP.NET 伺服器控制項是衍生自`Control`類別，所有伺服器控制項都可以存取`ResolveClientUrl`方法。 甚至`Page`類別衍生自`Control`類別，這表示您可以使用這個方法，直接從您的 ASP.NET 網頁的程式碼後置類別。


### <a name="usingin-the-declarative-markup"></a>使用`~`宣告式標記中

數個 ASP.NET Web 控制項，包含 URL 相關的屬性： 超連結控制項具有`NavigateUrl`屬性，且控制項的映像`ImageUrl`屬性; 等等。 這些控制項進行轉譯時，傳遞至其 URL 相關的屬性值`ResolveClientUrl`。 因此，如果這些屬性包含`~`表示 web 應用程式的根目錄，URL 將會修改以有效的相對 URL。

請記住，ASP.NET 伺服器控制項轉換`~`在其 URL 相關的屬性。 如果`~`會顯示在靜態的 HTML 標記中，例如`<img src="~/Images/PoweredByASPNET.gif" />`，ASP.NET 引擎會將傳送`~`以及剩餘的 HTML 內容的瀏覽器。 瀏覽器假設`~`是 URL 的一部分。 例如，如果瀏覽器會接收標記`<img src="~/Images/PoweredByASPNET.gif" />`它會假設有一個名為子`~`子資料夾`Images`，其中包含映像檔案`PoweredByASPNET.gif`。

若要修正的映像標記`Site.master`，取代現有`<img>`與 ASP.NET 映像的 Web 控制項的項目。 將映像的 Web 控制項的`ID`要`PoweredByImage`、 其`ImageUrl`屬性設`~/Images/PoweredByASPNET.gif`，並將其`AlternateText`」 提供的 ASP.NET ！"的屬性


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

這項變更的主版頁面後，重新瀏覽`~/Admin/Default.aspx`頁面上一次。 這次`PoweredByASPNET.gif`映像檔案會出現在頁面 （請參閱 [圖 3]）。 映像的 Web 控制項呈現時它會使用`ResolveClientUrl`方法來解析其`ImageUrl`屬性值。 在  `~/Admin/Default.aspx` `ImageUrl`會轉換成適當的相對 URL，為下列程式碼片段的 HTML 原始檔所示：


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> 除了以 URL 為基礎的 Web 控制項的屬性，用`~`也可以用時呼叫`Response.Redirect`和`Server.MapPath`方法，其他項目。 此外，`ResolveClientUrl`可能會叫用方法直接從 ASP.NET 或主版頁面的宣告式標記，如有需要請參閱[Fritz Onion](https://www.pluralsight.com/blogs/fritz/)的部落格文章[Using`ResolveClientUrl`標記中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)。


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>修正主版頁面的剩餘的相對 Url

除了`<img>`中的項目`footerContent`，我們只是修正，主版頁面包含一個更相對的 URL 需要我們的注意。 `topContent`區域包含連結 「 主版頁面教學課程，」 指向`Default.aspx`。


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

此 URL 是相對的因為它會傳送使用者`Default.aspx`資料夾中的 [內容] 頁面，他們造訪的頁面。 若要將一律指向此連結`Default.aspx`我們要取代的根資料夾中`<a>`項目與超連結 Web 控制項，讓我們可以使用`~`標記法。

移除`<a>`項目標記，並在其位置新增超連結控制項。 設定超連結`ID`要`lnkHome`、 其`NavigateUrl`屬性設`~/Default.aspx`，並將其`Text`屬性，以 「 主版頁面教學課程 」。


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

就這麼容易！ 此時，所有主版頁面和內容頁面，不論資料夾的內容頁面呈現時正確地以我們的主版頁面中的 Url 位於中。

### <a name="automatic-url-resolution-in-theheadsection"></a>中的自動 URL 解析`<head>`區段

在[*建立全網站的版面配置使用主版頁面*](creating-a-site-wide-layout-using-master-pages-vb.md)教學課程，我們已新增`<link>`來`Styles.css`中的檔案`<head>`區域：


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

雖然`<link>`項目的`href`屬性相對的它會自動轉換為適當的路徑，在執行階段。 如我們所述[*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中，`<head>`區域是實際的伺服器端控制項，讓它能夠修改其內部的控制項呈現時的內容。

若要確認，請再次造訪`~/Admin/Default.aspx`頁面，並檢視傳送至瀏覽器的 HTML 原始檔。 下列程式碼片段所示，`<link>`項目的`href`屬性會自動修改適當的相對 url， `../Styles.css`。


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>總結

主版頁面通常包括連結、 影像和其他外部資源，必須指定透過 URL。 因為主版頁面和內容頁面可能不存在於相同的資料夾中，務必 abstain 使用相對 Url。 雖然您可以使用硬式編碼的絕對 Url，因此嚴格結合的 web 應用程式的絕對 URL。 如果絕對的 URL 變更為它經常這樣做時移動，或部署 web 應用程式-您必須記得要返回並更新絕對 Url。

理想的方法是使用波狀符號 (`~`) 來表示應用程式根目錄。 ASP.NET Web 控制項的包含 URL 相關的屬性對應`~`在執行階段應用程式根目錄。 就內部而言，Web 控制項使用`Control`類別的`ResolveClientUrl`方法以產生有效的相對 URL。 這個方法是公用的而且可從每個伺服器控制項 (包括`Page`類別)，因此您可以使用它以程式設計方式從您的程式碼後置類別，如有需要。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/Articles/419.aspx)
- [URL 重訂基底中主版頁面](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [使用`ResolveClientUrl`標記中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [下一頁](control-id-naming-in-content-pages-vb.md)
