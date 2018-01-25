---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: "主版頁面 (C#) 中的 Url |Microsoft 文件"
author: rick-anderson
description: "解決方式中的主版頁面的 Url 可能會中斷由於是在不同的相對目錄的內容頁面之外的主版頁面檔案。 查看重定基底..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b01f0ac780121c4e0941df6016220a1cb1ed2d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-c"></a>主版頁面 (C#) 中的 Url
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> 解決方式中的主版頁面的 Url 可能會中斷由於是在不同的相對目錄的內容頁面之外的主版頁面檔案。 查看重定基底 Url，透過 ~ 宣告式語法和以程式設計方式使用 ResolveUrl ResolveClientUrl 中。 （另請查看


## <a name="introduction"></a>簡介

在所有範例中我們看到了到目前為止，主要和內容頁面具有位於相同的資料夾 （網站的根資料夾）。 但是，沒有的理由為何 master 和內容頁必須是相同的資料夾中。 您當然可以在子資料夾中建立內容頁面。 同樣地，您可能會建立`~/MasterPages/`放置站台的主版頁面的資料夾。

一個可能的問題，將主要和內容頁面放在不同的資料夾包括中斷的 Url。 如果主版頁面包含超連結、 影像或其他項目中的相對 Url，連結將會位於不同的資料夾中的內容頁面的無效。 在本教學課程，我們會檢查此問題，以及因應措施的來源。

## <a name="the-problem-with-relative-urls"></a>使用相對 Url 的問題

在網頁上的 URL 即為*相對 URL*如果它所指向的資源的位置是相對於網站的資料夾結構中的網頁上的位置。 任何不是以正斜線開頭的 URL (`/`) 或通訊協定 (例如`http://`) 是相對的因為它會解析瀏覽器的網頁，其中包含 URL 的位置。

例如，我們的網站有`~/Images/`單一映像檔案後，資料夾`PoweredByASPNET.gif`。 主版頁面檔案`Site.master`具有`<img>`中的項目`footerContent`區域以下列標記：


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

`src`屬性值中的`<img>`項目為相對 URL，因為它不是以開頭`/`或`http://`。 簡單地說，`src`屬性值會告知瀏覽器中呈現的外觀`Images`子資料夾名為的檔案`PoweredByASPNET.gif`。

當造訪內容頁面上，上述的標記是直接傳送至瀏覽器。 請花一點時間瀏覽`About.aspx`並檢視傳送到瀏覽器的 HTML 原始檔。 您會發現主版頁面中的完全相同標記已傳送至瀏覽器。


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

如果 [內容] 頁面的根資料夾中 (因為`About.aspx`) 的所有項目如預期般運作，所以`Images`相對於根資料夾的子資料夾。 不過，事情細分如果內容的頁面是在不同的主版頁面的資料夾。 為了說明這點，建立名稱為的子`Admin`。 接下來，加入名為的內容頁面`Default.aspx`至`Admin`資料夾，並確認新的頁面，即可將繫結`Site.master`主版頁面。

> [!NOTE]
> 在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`，自動設定內容頁面的標題 (如果它未明確指派）。 別忘了讓新建立的網頁程式碼後置類別衍生自`BasePage`，讓它可以利用這項功能。


建立此內容的頁面之後，您的方案總管 中看起來應該類似於圖 1。


![新的資料夾和 ASP.NET 頁面加入至專案](urls-in-master-pages-cs/_static/image1.png)

**圖 01**： 新的資料夾和 ASP.NET 頁面加入至專案


接下來，更新`Web.sitemap`檔案以納入新`<siteMapNode>`這一課中的項目。 下列 XML 顯示完整`Web.sitemap`標記，現在包含加入的第三個`<siteMapNode>`項目。


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

新建立`Default.aspx`頁面應該會有四個內容控制項對應中的四個 ContentPlaceHolders `Site.master`。 將一些文字加入至內容控制項參考`MainContent`ContentPlaceHolder，然後瀏覽透過瀏覽器頁面。 如圖 2 所示，在瀏覽器找不到`PoweredByASPNET.gif`映像檔。 什麼呢？

`~/Admin/Default.aspx`內容頁面會將相同的 HTML 傳送給`footerContent`區域為`About.aspx`頁面：


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

因為`<img>`項目的`src`屬性為相對 URL，在瀏覽器會嘗試尋找`Images`相對於網頁的資料夾位置的資料夾。 換句話說，在瀏覽器正在尋找映像檔`Admin/Images/PoweredByASPNET.gif`。


[![找不到 PoweredByASPNET.gif 映像檔](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**圖 02**:`PoweredByASPNET.gif`映像找不到檔案 ([按一下以檢視完整大小的影像](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>相對的 Url 取代為絕對 Url

相對 URL 的情況則相反*絕對 URL*，這是它的開頭是正斜線 (`/`) 或通訊協定，例如`http://`。 絕對 URL 指定位置的已知的固定端點中的資源，因為相同的絕對 URL 無效，在任何網頁上，不論網站的資料夾結構中的網頁上的位置。

若要補救不完整的影像圖 2 所示，我們需要更新`<img>`項目的`src`屬性，讓它使用而不是相對的絕對 URL。 若要判斷正確的絕對 URL，請瀏覽您的網站中的網頁的其中一個並檢查 [網址] 列。 如圖 2 中的網址列所示，web 應用程式的完整的路徑是`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`。 因此，我們無法更新`<img>`項目的`src`屬性設定為下列兩個絕對 url:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

請花一點時間更新`<img>`項目的`src`屬性設定為使用其中一種形式，如上所示的絕對 URL，然後瀏覽`~/Admin/Default.aspx`透過瀏覽器的頁面。 目前瀏覽器會正確地尋找並顯示`PoweredByASPNET.gif`映像檔案 （請參閱圖 3）。


[![PoweredByASPNET.gif 影像是現在顯示](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**圖 03**:`PoweredByASPNET.gif`影像是現在顯示 ([按一下以檢視完整大小的影像](urls-in-master-pages-cs/_static/image7.png))


硬式編碼的絕對 URL 中的運作方式，雖然緊密涉入 HTML 網站的伺服器和資料夾位置，可能會變更。 使用表單的絕對 URL`http://localhost:3908/...`很容易損毀因為先前的連接埠號碼`localhost`會自動選取每次啟動 Visual Studio 內建 ASP.NET 開發 Web 伺服器時。 同樣地，`http://localhost`時在本機測試組件才有效。 程式碼部署到實際執行伺服器之後, URL 基底會變更為其他項目，例如`http://www.yourserver.com`。 在表單中的絕對 URL`/ASPNET_MasterPages_Tutorial_04_CS/...`也受到相同損毀，因為此應用程式路徑有時候開發和生產伺服器有不同。

好消息是 ASP.NET 提供方法，以產生在執行階段的有效相對 URL。

## <a name="usingandresolveclienturl"></a>使用`~`和`ResolveClientUrl`

而是硬式編碼的絕對 URL 位址，比 ASP.NET 可讓網頁開發人員可以使用波狀符號 (`~`) 來表示 web 應用程式的根目錄。 例如，如稍早在本教學課程我可以使用標記法`~/Admin/Default.aspx`中的文字來參考`Default.aspx`頁面`Admin`資料夾。 `~`表示`Admin`資料夾是 web 應用程式根目錄的子資料夾。

`Control`類別的[`ResolveClientUrl`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)採用 URL 並加以修改以適當的控制項所在的網頁的相對 URL。 例如，呼叫`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`從`About.aspx`傳回`Images/PoweredByASPNET.gif`。 呼叫從`~/Admin/Default.aspx`，不過，會傳回`../Images/PoweredByASPNET.gif`。

> [!NOTE]
> 因為所有 ASP.NET 伺服器控制項都衍生自`Control`類別，所有的伺服器控制項有存取權`ResolveClientUrl`方法。 即使`Page`類別衍生自`Control`類別，這表示您可以使用這個方法，直接從您的 ASP.NET 網頁程式碼後置類別。


### <a name="usingin-the-declarative-markup"></a>使用`~`宣告式標記中

數個的 ASP.NET Web 控制項包含 URL 相關的屬性： 超連結控制項有`NavigateUrl`屬性; 影像控制項有`ImageUrl`屬性; 等等。 進行轉譯時，這些控制項傳遞至其 URL 相關的屬性值`ResolveClientUrl`。 因此，如果這些屬性包含`~`表示 web 應用程式的根目錄，URL 將會修改為有效的相對 URL。

請注意，ASP.NET 伺服器控制項轉換`~`URL 相關屬性中。 如果`~`會顯示在靜態的 HTML 標記中，像是`<img src="~/Images/PoweredByASPNET.gif" />`，ASP.NET 引擎會將傳送`~`HTML 內容的其餘部分一起瀏覽器。 瀏覽器假設`~`是 URL 的一部分。 例如，如果瀏覽器收到標記`<img src="~/Images/PoweredByASPNET.gif" />`假設有一個名為子`~`子資料夾`Images`包含映像檔`PoweredByASPNET.gif`。

若要修正的映像標記`Site.master`，取代現有`<img>`的 ASP.NET 影像 Web 控制項的項目。 將映像 Web 控制項的`ID`至`PoweredByImage`、 其`ImageUrl`屬性`~/Images/PoweredByASPNET.gif`，且其`AlternateText`"提供 asp.net ！"的屬性


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

變更主版頁面之後，再次瀏覽`~/Admin/Default.aspx`頁面上一次。 這次`PoweredByASPNET.gif`頁面上會出現映像檔案 （請參閱圖 3）。 控制項是映像網頁呈現時它會使用`ResolveClientUrl`方法來解決其`ImageUrl`屬性值。 在`~/Admin/Default.aspx``ImageUrl`轉換成適當的相對 URL，做為 HTML 來源顯示下列程式碼片段：


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> 除了在 URL 為基礎的 Web 控制項的屬性，在使用`~`也可以用時呼叫`Response.Redirect`和`Server.MapPath`方法，和其他項目。 此外，`ResolveClientUrl`方法可能會直接從叫用 ASP.NET 或主版頁面的宣告式標記，如有需要請參閱 < [Fritz 洋蔥](https://www.pluralsight.com/blogs/fritz/)的部落格項目[使用`ResolveClientUrl`標記中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)。


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>修正主版頁面的剩餘相對的 Url

除了`<img>`中的項目`footerContent`我們修正，、 主版頁面包含需要注意我們的一個更相對 URL。 `topContent`區域包含連結 」 主版頁面教學課程中，「 指向`Default.aspx`。


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

此 URL 是相對的因為它會傳送使用者`Default.aspx`內容頁面他們造訪的資料夾中的頁面。 指向此連結`Default.aspx`我們需要更換的根資料夾中`<a>`項目與超連結 Web 控制項，以便我們可以使用`~`標記法。

移除`<a>`元素標記並將超連結控制項加入其所在位置。 設定超連結的`ID`至`lnkHome`、 其`NavigateUrl`屬性`~/Default.aspx`，且其`Text`屬性為"主版頁面教學課程 」。


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

就這麼容易！ 此時，所有主版頁面和內容頁面，無論何種資料夾的內容頁轉譯時，我們主版頁面中的 Url 會正確基礎位於中。

### <a name="automatic-url-resolution-in-theheadsection"></a>中的自動 URL 解析`<head>`區段

在[*建立全站台的版面配置使用主版頁面*](creating-a-site-wide-layout-using-master-pages-cs.md)教學課程中，我們加入`<link>`至`Styles.css`檔案`<head>`區域：


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

雖然`<link>`項目的`href`屬性是相對，它會自動轉換為適當的路徑，在執行階段。 如我們所述[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中，`<head>`區域是實際的伺服器端控制項，讓它能夠修改其內部的控制項呈現時的內容。

若要確認這種情況，請再次造訪`~/Admin/Default.aspx`頁面，並檢視傳送到瀏覽器的 HTML 原始檔。 如以下程式碼片段所示，`<link>`項目的`href`屬性會自動修改以適當的相對 URL， `../Styles.css`。


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>總結

主版頁面通常包括連結、 影像和其他外部資源，必須指定透過 URL。 因為主版頁面和內容頁面可能不存在於相同的資料夾中，務必 abstain 使用相對的 Url。 雖然可以使用硬式編碼的絕對 Url，因此緊密結合的絕對 URL 的 web 應用程式。 如果變更的絕對 URL-通常會移動或 web 應用程式的部署時，您必須記得要返回並更新絕對 Url。

理想的方法是使用波狀符號 (`~`) 來表示應用程式根目錄。 ASP.NET Web 控制項包含 URL 相關的屬性對應`~`在執行階段應用程式根目錄。 就內部而言，Web 控制項使用`Control`類別的`ResolveClientUrl`方法來產生有效的相對 URL。 這個方法是公用的而且可從每個伺服器控制項 (包括`Page`類別)，因此您可以使用它以程式設計方式從您的程式碼後置類別，如有需要。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/Articles/419.aspx)
- [在主版頁面的 URL 重定基底](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [使用`ResolveClientUrl`標記中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一頁](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
[下一頁](control-id-naming-in-content-pages-cs.md)
