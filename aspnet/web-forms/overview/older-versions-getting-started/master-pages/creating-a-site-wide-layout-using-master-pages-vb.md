---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: 建立全網站的版面配置使用主版頁面 (VB) |Microsoft Docs
author: rick-anderson
description: 本教學課程中會顯示主版頁面的基本概念。 也就是為何主版頁面是怎麼一建立主版頁面、 什麼是內容的預留位置，是怎麼一 cr...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: feb04c19092101bb019883c8b72b40ceb9afc015
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833853"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>建立全網站的版面配置使用主版頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> 本教學課程中會顯示主版頁面的基本概念。 也就是為何主版頁面，其中一個要如何建立主版頁面，什麼是內容的預留位置，要如何一個建立的 ASP.NET 網頁，會使用主版頁面，如何修改主版頁面會自動反映在其相關聯的內容頁面，依此類推。


## <a name="introduction"></a>簡介

設計良好的一個屬性是網站的一致的整個網站的頁面配置。 www.asp.net 網站後，為例。 在撰寫本文時，每一頁會有相同的內容，在頂端和底部的頁面。 如 [圖 1] 所示，每個頁面最上方會顯示一份 Microsoft 社群的灰色列。 下方也就是為網站標誌、 所在的站台已轉譯，語言和核心區段的清單： 首頁、 開始、 深入了解、 下載及其他等等。 同樣地，頁面底部包含 www.asp.net、 的著作權陳述式，以及隱私權聲明連結廣告的相關資訊。


[![Www.asp.net 網站所有頁面採用一致的外觀與風格](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>圖 01</strong>: www.asp.net 網站採用一致的查詢和覺得跨所有的網頁 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


設計良好的站台的另一個屬性就是可以變更站台的外觀的容易。 [圖 1] 顯示 www.asp.net 首頁，自 2008 年 3 月起，但現在開始到本教學課程中的發行集，可能執行的動作會變更外觀與風格。 可能是沿著頂端的功能表項目會展開包括 MVC 架構的新區段。 或者，也許是嶄新的設計，含有不同的色彩、 字型和配置即將揭開。 將這類變更套用至整個網站應該快速及簡單的程序不需要修改的數千個組成該網站的網頁。

在 ASP.NET 中建立全網站的頁面範本可使用*主版頁面*。 簡單的說，主版頁面是一種特殊類型的定義中所有常見的標記的 ASP.NET 網頁*內容頁*以及自訂內容 頁面的內容頁面為基礎的區域。 （內容頁面是 ASP.NET 網頁繫結至主版頁面）。每當變更主版頁面的版面配置或格式化時，所有其內容頁的輸出會同樣地立即更新，這可讓您套用整個網站的外觀會變更，只要更新和部署單一檔案 （也就是，主版頁面）。

這是一系列的教學課程會探索使用主版頁面的第一個教學課程。 本教學課程系列的課程我們：

- 檢查建立主版頁面和其相關聯的內容頁面
- 討論各種不同的秘訣、 技巧和陷阱，
- 找出常見的主版頁面錯誤，並瀏覽因應措施，
- 了解如何從內容頁面和一個-反之亦然，存取主版頁面
- 了解如何指定在執行階段的內容頁面的主版頁面和
- 其他進階的主版頁面主題。

這些教學課程都被為了很簡潔，並提供足夠的螢幕擷取畫面的逐步指示，可逐步引導您完成此程序以視覺化方式而定。 每個教學課程適用於 C# 和 Visual Basic 版本，而且包含的下載時，使用完整的程式碼。

本教學課程中首開始了解主版頁面的基本概念。 我們會討論主版頁面的運作方式，看看建立主版頁面與相關聯的內容頁面，使用 Visual Web Developer 中，並請參閱如何主版頁面的變更會立即反映在其內容的頁面。 讓我們開始吧 ！

## <a name="understanding-how-master-pages-work"></a>了解主版頁面的運作方式

建置具有一致的全站台的頁面配置的網站需要每個網頁上發出通用格式的標記，除了其自訂的內容。 例如，雖然在 www.asp.net 上的每個教學課程或論壇貼文都有自己唯一的內容，這些頁面每一也會呈現一系列的常見`<div>`顯示最上層區段連結的項目： 首頁、 開始、 學習、 等等。

有各種技術，用於建立具有一致的外觀及操作的 web 網頁。 貝氏方法，就是只要複製，並將常見的版面配置標記貼到 所有網頁，但這種方法有一些缺點。 簡單來說，每次建立新的頁面時，您必須記得複製並貼到頁面的 共用的內容。 這類複製並貼上作業會敞開錯誤，因為您可能不小心將只是共用標記的子集複製到新頁面。 並不夠好，這種方法可以取代現有的全站台的外觀新即時痛，因為必須編輯每個站台中的單一頁面，才能使用新的外觀與風格。

在 ASP.NET 2.0 版、 之前頁面開發人員通常會放在常見的標記[使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)然後新增至每個頁面的 這些使用者控制項。 這種方法需要頁面開發人員手動將使用者控制項新增至每個新的頁面，請記住，但允許進行更輕鬆的全站台修改，因為使用者控制項更新常見的標記時才能進行修改。 不幸的是，Visual Studio.NET 2002年和 2003-版本的 Visual Studio 用來建立 ASP.NET 1.x 應用程式-在 [設計] 檢視中呈現使用者控制項，會以灰色方塊。 因此，使用這種方法的網頁開發人員不喜歡的 WYSIWYG 設計階段環境。

使用使用者控制項的缺點已解決在 ASP.NET 2.0 版和 Visual Studio 2005 中引進*主版頁面*。 主版頁面是一種特殊類型的 ASP.NET 頁面，定義全站台的標記和*地區*位置相關聯*內容頁*定義其自訂的標記。 如同我們在步驟 1 所見，這些區域 ContentPlaceHolder 控制項所定義。 ContentPlaceHolder 控制項只是表示主版頁面的控制階層架構中，插入自訂內容，由內容頁面的位置。

> [!NOTE]
> ASP.NET 2.0 版之後並未變更的核心概念和主版頁面的功能。 不過，Visual Studio 2008 提供設計階段支援巢狀主版頁面，在 Visual Studio 2005 中缺乏的功能。 我們將探討在未來的教學課程中使用巢狀主版頁面。


[圖 2] 顯示 www.asp.net 主版頁面的外觀。 請注意，主版頁面定義常用的全網站的版面配置-頂端、 底端，和每個頁面的右邊標記-以及 ContentPlaceHolder 左方，每個個別的網頁的唯一內容所在的位置中。


![主版頁面定義的全站台的配置，以及可編輯區域內容的頁面依內容頁面為基礎](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**圖 02**： 主版頁面定義的全站台的配置，以及可編輯區域內容的頁面依內容頁面為基礎


一旦已經定義了主版頁面可以繫結至新的 ASP.NET 頁面，透過核取方塊的刻度。 -呼叫內容頁面-這些 ASP.NET 頁面包含每個主版頁面的 ContentPlaceHolder 控制項內容的控制項。 當透過瀏覽器瀏覽 [內容] 頁面時 ASP.NET 引擎會建立主版頁面的控制項階層架構，並將 [內容] 頁面的控制階層架構插入到適當的位置。 轉譯此結合的控制階層架構，產生的 HTML 會傳回給使用者的瀏覽器。 因此，[內容] 頁面就會發出其 ContentPlaceHolder 控制項之外的主版頁面中定義的常見標記和其本身的內容控制項內定義的頁面特定標記。 圖 3 闡明此概念。


[![要求之網頁的標記被融合到主版頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**圖 03**: 要求網頁的標記融合到主版頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


既然我們已經討論過主版頁面的運作方式，讓我們看看建立主版頁面與相關聯的內容頁面，使用 Visual Web Developer。

> [!NOTE]
> 以達到最多可能觀眾，我們在此教學課程系列建置 ASP.NET 網站會建立與 Microsoft 的免費版本的 Visual Studio 2008 中，使用 ASP.NET 3.5 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 如果您還沒有升級至 ASP.NET 3.5 中，別擔心-在這些教學課程工作中所討論的概念也能用於 ASP.NET 2.0 和 Visual Studio 2005。 不過，有些示範應用程式可以使用新增至.NET Framework 3.5; 版的功能當使用 3.5 特有的功能時，包含附註，討論如何在 2.0 版中實作類似的功能。 不要記住，可用的示範應用程式從每個教學課程的目標.NET Framework 3.5 版，會導致下載`Web.config`檔案，其中包含特定 3.5 的組態項目。 未先移除 3.5 特定標記的來源，將無法運作長話短說，如果您有尚未則可下載的 web 應用程式的電腦上安裝.NET 3.5 `Web.config`。 請參閱[剖析 ASP.NET 3.5 版的`Web.config`檔案](http://www.4guysfromrolla.com/articles/121207-1.aspx)如需本主題的詳細資訊。


## <a name="step-1-creating-a-master-page"></a>步驟 1： 建立主版頁面

我們可以建立和使用主要和內容頁面瀏覽之前，我們首先需要 ASP.NET 網站。 開始建立新檔案系統架構 ASP.NET 網站。 若要達成此目的，啟動 Visual Web Developer 然後移至 [檔案] 功能表並選擇新的網站上，顯示新的網站] 對話方塊 （請參閱 [圖 4）。 選擇 ASP.NET 網站範本、 到檔案系統設定 位置 下拉式清單、 選擇的資料夾，以存放 web 站台和 Visual basic 中設定的語言。 這會建立新的網站與`Default.aspx`ASP.NET 網頁`App_Data`資料夾，和`Web.config`檔案。

> [!NOTE]
> Visual Studio 支援兩種專案管理模式： 網站專案和 Web 應用程式專案。 網站專案沒有專案檔，而 Web 應用程式專案模擬專案架構在 Visual Studio.NET 2002年/2003年-它們包含在專案檔和專案的原始程式碼編譯成單一組件，位於`/bin`資料夾。 Visual Studio 2005 一開始只支援的網站專案，雖然 Web 應用程式專案模型已重新引入含 Service Pack 1;Visual Studio 2008 提供了這兩個專案模型。 Visual Web Developer 2005 和 2008年版本，不過，僅支援網站專案。 我在本教學課程系列中，我示範使用網站專案模型。 如果您正在使用非 Express edition，而且想要使用[Web 應用程式專案模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)相反地，請隨意這樣做，但您看到您的畫面和必須與所採取的步驟之間可能會有些不一致螢幕擷取畫面所示，在這些教學課程中提供的指示。


[![建立新檔案系統為基礎的網站](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**圖 04**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


接下來，加入主版頁面的根目錄中的站台的專案名稱上按一下滑鼠右鍵，選擇 加入新項目並選取主版頁面範本。 請注意，主版頁面最後副檔名`.master`。 這個新的主版頁面命名為`Site.master`並按一下 [新增]。


[![加入主版頁面名稱為 Site.master 網站](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**圖 05**： 加入主版頁面具名`Site.master`網站 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


加入新的主版頁面檔案透過 Visual Web Developer，則會使用下列的宣告式標記建立主版頁面：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

宣告式標記中的第一行會[`@Master`指示詞](https://msdn.microsoft.com/library/ms228176.aspx)。 `@Master`指示詞大致[`@Page`指示詞](https://msdn.microsoft.com/library/ydy4x04a.aspx)ASP.NET 網頁中顯示。 它會定義的伺服器端語言 (VB) 和資訊的位置和主版頁面的程式碼後置類別的繼承。

`DOCTYPE`和頁面的宣告式標記下方`@Master`指示詞。 此頁面包含四個伺服器端控制項以及靜態 HTML:

- **Web Form ( `<form runat="server">`)** -因為所有的 ASP.NET 網頁通常有一個 Web 表單-，因為主版頁面可能包含的 Web 控制項，必須出現在 Web Form-務必要將 Web Form 加入至您的主版頁面 （而非 Web Form 加入電子除此之外，每個內容頁面）。
- **ContentPlaceHolder 控制項，名為`ContentPlaceHolder1`**  -這個 ContentPlaceHolder 控制項中的 Web 表單隨即出現，並做為 [內容] 頁面的使用者介面的區域。
- **伺服器端`<head>`項目**-`<head>`項目具有`runat="server"`屬性，所以可透過伺服器端程式碼存取。 `<head>`項目實作這種方式，讓頁面的標題和其他`<head>`-相關的標記可能會新增，或以程式設計方式調整。 例如，設定 ASP.NET 網頁`Title`屬性變更`<title>`所呈現的項目`<head>`伺服器控制項。
- **ContentPlaceHolder 控制項，名為`head`**  -ContentPlaceHolder 會出現這個控制項內`<head>`伺服器控制項，並可用來以宣告方式將內容加入至`<head>`項目。

此預設主版頁面的宣告式標記做為起點來設計您自己的主版頁面。 請隨意編輯 HTML 還是要新增額外的 Web 控制項或 ContentPlaceHolders 主版頁面。

> [!NOTE]
> 當設計主版頁面，確定主版頁面包含 Web 表單，並至少一個 ContentPlaceHolder 控制出現在此 Web Form 內。


### <a name="creating-a-simple-site-layout"></a>建立簡單的站台版面配置

讓我們進一步探討`Site.master`的預設宣告式標記，以建立共用的所有頁面的其中一種網站配置： 常見的標頭; 左側的資料行，以瀏覽、 新聞與其他的全站台內容; 會顯示 「 提供的 Microsoft ASP.NET 」 圖示的頁尾。 圖 6 顯示主版頁面的最終結果，其內容頁面的其中一個透過瀏覽器檢視時。 圖 6 中的紅色圈選的區域是所造訪的頁面所特有 (`Default.aspx`); 其他內容是所有的內容頁面的主版頁面中已定義且因此一致。


[![主版頁面的上方、 左側和底部部分定義標記](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**圖 06**： 主版頁面的上方、 左側和底部部分定義的標記 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


若要達到的網站配置 [圖 6] 所示，先更新`Site.master`主版頁面，使其包含下列宣告式標記：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

主版頁面的版面配置定義使用一系列的`<div>`HTML 項目。 `topContent` `<div>`包含出現在每個頁面頂端的標記時`mainContent`， `leftContent`，並`footerContent` `<div>` s 用來顯示頁面的內容、 左欄中和 「 電源已由 MicrosoftASP.NET"圖示，分別。 除了新增這些`<div>`項目，我也重新命名`ID`屬性中，將主要 ContentPlaceHolder 控制項`ContentPlaceHolder1`至`MainContent`。

格式與版面配置的規則，這些各種`<div>`中的項目拼寫[階層式樣式表 (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)檔案`Styles.css`，指定透過`<link>`項目中的主版頁面的`<head>`項目。 這些各種規則會定義每個外觀`<div>`先前所述的項目。 例如， `topContent` `<div>`項目，顯示 「 主版頁面教學課程 」 的文字和連結，其具有格式規則中指定`Styles.css`，如下所示：

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

如果您要遵照，在您的電腦，您必須下載本教學課程隨附的程式碼並加入`Styles.css`檔案加入專案。 同樣地，您也必須建立名為`Images`並將已下載的示範網站 」 提供技術的 Microsoft ASP.NET 」 圖示複製到您的專案。

> [!NOTE]
> CSS 和網頁格式的討論已超出本文的範圍。 如需 CSS 的詳細資訊，請參閱[CSS 教學課程](http://www.w3schools.com/css/default.asp)在[W3Schools.com](http://www.w3schools.com/)。 我也建議您下載本教學課程隨附的程式碼、 使用中的 CSS 設定`Styles.css`以查看不同的格式化規則的影響。


### <a name="creating-a-master-page-using-an-existing-design-template"></a>建立主版頁面使用現有的設計範本

多年來，我已建置小型-以中小型企業適用的 ASP.NET web 應用程式的數目。 我的用戶端的一些有他們想要使用現有的站台版面配置其他人雇用裁判的圖形設計工具。 少數信賴我設計網站配置。 如您所見 圖 6 所，工作的程式設計師來設計網站的版面配置是通常為具有執行 open-heart surgery 而醫生 daně 您會計為智。

幸運的是，有提供免費的 HTML 設計範本的 innumerous 網站-Google 傳回超過 6 百萬個結果的搜尋字詞 「 免費網站範本 」。 其中一個我最愛的項目是[OpenDesigns.org](http://opendesigns.org/)。一旦您找到您要的網站範本，將 CSS 檔案和映像新增至您的網站專案，並整合您的主版頁面範本的 HTML。

> [!NOTE]
> Microsoft 還另外提供一些[免費 ASP.NET 設計入門套件範本](https://msdn.microsoft.com/asp.net/aa336613.aspx)，整合到 Visual Studio 中的 [新的網站] 對話方塊。


## <a name="step-2-creating-associated-content-pages"></a>步驟 2： 建立相關聯的內容頁面

建立主版頁面時，我們已準備好開始建立繫結至主版頁面的 ASP.NET 網頁項目。 這類頁面指*內容頁*。

讓我們將新的 ASP.NET 網頁新增至專案，並將它繫結`Site.master`主版頁面。 在 [方案總管] 中的專案名稱上按一下滑鼠右鍵，然後選擇 [加入新項目] 選項。 選取的 Web 表單範本中，輸入名稱`About.aspx`，然後核取 選取主版頁面 核取方塊，如 圖 7 所示。 這樣會顯示 選取主版頁面對話方塊方塊中您可以在此選擇要使用的主版頁面的 （請參閱 圖 8）。

> [!NOTE]
> 如果您使用 Web 應用程式專案模型，而不網站專案模型的 ASP.NET 網站建立您不會看到 [圖 7] 所示的 [加入新項目] 對話方塊中的 [選取主版頁面] 核取方塊。 若要建立為內容頁面，使用 Web 應用程式專案模型時必須選擇 Web 內容表單範本，而不是 Web 表單範本。 選取 Web 內容表單範本，然後按一下 [新增] 之後, 相同選取主版頁面 [圖 8] 所示的對話方塊隨即出現。


[![加入新的內容頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**圖 07**： 加入新的內容頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![選取 Site.master 主版頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**圖 08**： 選取 `Site.master`主版頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


如下列宣告式標記所示，新增的 [內容] 頁面包含`@Page`，會指回其主版頁面和內容的控制項的主版頁面 ContentPlaceHolder 控制項的每個指示詞。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> 在步驟 1 的 「 建立簡單的網站配置 」 一節我重新命名`ContentPlaceHolder1`至`MainContent`。 如果您未重新命名此 ContentPlaceHolder 控制項的`ID`同樣地，在內容頁面的宣告式標記會稍有不同的標記，如上所示。 也就是第二個內容控制項之`ContentPlaceHolderID`會反映`ID`的相對應的 ContentPlaceHolder 控制您的主版頁面中。


當轉譯內容頁面、 ASP.NET 引擎必須 fuse 網頁的內容具有其主版頁面 ContentPlaceHolder 控制項的控制項。 ASP.NET 引擎會決定從的 [內容] 頁面的主版頁面`@Page`指示詞的`MasterPageFile`屬性。 如上述的標記所示，此內容的頁面繫結至`~/Site.master`。

因為主版頁面有兩個 ContentPlaceHolder 控制項-`head`和`MainContent`-Visual Web Developer 會產生兩個內容控制項。 每個內容控制項參考特定的 ContentPlaceHolder 透過其`ContentPlaceHolderID`屬性。

透過先前的全網站範本技術佔有主版頁面的位置是設計階段支援。 圖 9 顯示`About.aspx`檢視透過 Visual Web Developer 的 設計 檢視時的內容頁面。 請注意，雖然主版頁面內容會顯示，它會呈現灰色且無法修改。 內容控制項對應至主版頁面的 ContentPlaceHolders 是，不過，可以讓您編輯。 就像使用其他任何 ASP.NET 頁面上，您可以建立內容頁面的介面新增到 [來源] 或 [設計] 檢視的 Web 控制項。


[![[內容] 頁面的 [設計] 檢視會顯示這兩個指定頁面和主版頁面內容](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**圖 09**: 內容頁面的設計檢視會顯示兩個頁面特定和主版頁面內容 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>[內容] 頁面中加入標記和 Web 控制項

請花一點時間建立的某些內容`About.aspx`頁面。 如您所見 圖 10 中，我可以輸入"有關 Author"標題和幾個段落的文字，但也新增 Web 控制項的組合。 建立此介面之後, 請瀏覽`About.aspx`透過瀏覽器的頁面。


[![請瀏覽透過瀏覽器的 [about.aspx 的網頁] 頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**圖 10**： 瀏覽`About.aspx`頁面上透過瀏覽器 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


請務必了解要求的內容頁面和其相關聯的主版頁面是融合完全在 web 伺服器上的整體轉譯。 使用者的瀏覽器便會產生、 積 HTML。 若要確認這點，檢視瀏覽器移至 [檢視] 功能表並選擇來源收到的 HTML。 請注意，有沒有框架或其他任何特製化的技術，用於在單一視窗中顯示兩個不同的網頁。

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>繫結至現有的 ASP.NET 網頁的主版頁面

如我們在此步驟中所見的將新的內容頁面加入至 ASP.NET web 應用程式是簡單，只要選取 [選取主版頁面] 核取方塊，然後挑選主版頁面。 不幸的是，將現有的 ASP.NET 頁面轉換至主版頁面也不簡單。

若要繫結至現有的 ASP.NET 網頁的主版頁面中，您需要執行下列步驟：

1. 新增`MasterPageFile`屬性設定為 ASP.NET 網頁的`@Page`指示詞，它指向適當的主版頁面。
2. 針對每個 ContentPlaceHolders 主版頁面中，將內容控制項。
3. 選擇性地剪下並將現有的 ASP.NET 網頁的內容貼到適當的內容控制項。 我說 「 選擇性 」 這裡因為 ASP.NET 頁面可能包含已由主版頁面中，例如表示標記`DOCTYPE`，則`<html>`項目，並在 Web Form。

如需此程序，以及螢幕擷取畫面的逐步指示，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的[使用主版頁面與網站導覽](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx)教學課程。 「 更新`Default.aspx`和`DataSample.aspx`使用主版頁面 」 一節詳細說明這些步驟。

因為它是建立新的內容頁面，將現有的 ASP.NET 頁面轉換成內容的頁面比更為容易，我建議當您建立新的 ASP.NET 網站會將主版頁面新增至站台。 將所有新的 ASP.NET 網頁的繫結至這個主版頁面。 別擔心，如果初始的主版頁面很簡單或純文字;您可以稍後再更新主版頁面。

> [!NOTE]
> 在建立新的 ASP.NET 應用程式時，Visual Web Developer 將`Default.aspx`沒有繫結至主版頁面的頁面。 如果您想要轉換現有的 ASP.NET 頁面變成內容頁面的作法，就這樣做與`Default.aspx`。 或者，您可以刪除`Default.aspx`並重新加入它，但是這次的 [選取主版頁面] 核取方塊。


## <a name="step-3-updating-the-master-pages-markup"></a>步驟 3： 更新主版頁面的標記

主版頁面的主要好處之一是單一的主版頁面，可用來在網站上定義多個頁面的整體配置。 因此，更新站台的外觀及操作，需要更新單一檔案-主版頁面。

為了說明這種行為，讓我們更新我們的主版頁面包含目前的日期左邊的資料行的頂端。 新增名為標籤`DateDisplay`要`leftContent` `<div>`。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

接下來，建立`Page_Load`事件處理常式的主要頁面，然後新增下列程式碼：

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

上述程式碼中設定的標籤`Text`屬性目前的日期和時間格式化為一週的星期幾、 月和兩位數天數的名稱 （請參閱 圖 11）。 透過這項變更，重新審視您的內容頁面。 如 [圖 11] 所示，產生的標記會立即更新以包含主版頁面的變更。


[![主版頁面中所做的變更會反映當檢視內容頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**圖 11**： 主版頁面的變更會反映當檢視內容頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> 此範例所示，伺服器端 Web 控制項、 程式碼，以及事件處理常式，就可能會包含主版頁面。


## <a name="summary"></a>總結

主版頁面讓 ASP.NET 開發人員設計一致的全網站的版面配置，可輕鬆地更新。 Visual Web Developer 提供豐富的設計階段支援，則建立主版頁面和其相關聯的內容頁面很簡單，與建立標準的 ASP.NET 頁面。

我們在本教學課程中建立主版頁面範例有兩個 ContentPlaceHolder 控制項、 head 和 MainContent。 我們只 MainContent ContentPlaceHolder 控制項標記中指定我們的內容頁面上，不過。 在下一個教學課程中我們探討使用多個內容控制項，在 [內容] 頁面中的。 我們也會看到如何定義預設內容的標記控制內的主版頁面，以及如何切換使用預設標記中定義主要頁面，並提供自訂的標記，從 [內容] 頁面。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 設計工具： 免費建置 ASP.NET 網站使用 Web 標準的設計範本和指引](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET 主版頁面概觀](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [階層式樣式表 (CSS) 教學課程](http://www.w3schools.com/css/default.asp)
- [動態設定頁面的標題](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/articles/419.aspx)
- [主版頁面的快速入門教學課程](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](nested-master-pages-cs.md)
> [下一頁](multiple-contentplaceholders-and-default-content-vb.md)
