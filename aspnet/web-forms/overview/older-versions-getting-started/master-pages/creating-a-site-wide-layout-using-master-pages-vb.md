---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: "建立全站台的版面配置使用主版頁面 (VB) |Microsoft 文件"
author: rick-anderson
description: "本教學課程會顯示主版頁面的基本概念。 也就是為何主版頁面的運作方式之一建立主版頁面、 什麼是內容預留位置的運作方式一個 cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 29671970dc6f53d0e14170cf6376c02634b7b08e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>建立全站台的版面配置使用主版頁面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> 本教學課程會顯示主版頁面的基本概念。 也就是為何主版頁面，其中一個要如何建立主版頁面內容的預留位置是什麼，如何一個建立的 ASP.NET 網頁，會使用主版頁面上，如何修改主版頁面自動反映在其相關聯的內容頁面，等等。


## <a name="introduction"></a>簡介

設計良好的一個屬性是網站的一致的全站台的頁面配置。 Www.asp.net 網站後，為例。 在撰寫本文時，每一頁會具有相同的內容，在頂端和頁面底部。 如圖 1 所示，每一頁的開頭處會顯示一份 Microsoft 社群的灰色列。 下方也就是網站標誌、 所在站台已轉譯，語言和核心區段的清單： 首頁、 開始、 深入了解、 下載及其他等等。 同樣地，頁面底部包含 www.asp.net、 著作權的陳述式，以及隱私權聲明連結廣告的相關資訊。


[![Www.asp.net 網站所有頁面採用一致的外觀及操作](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

**圖 01**: www.asp.net 網站採用一致的外觀和感覺跨所有頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


設計良好的站台的另一個屬性是的簡易可以變更站台的外觀。 圖 1 顯示在 www.asp.net 首頁上，從 2008 年 3 月開始，但現在與本教學課程中的發行集，之間可能變更外觀與風格。 可能會展開上方的功能表項目，包括 MVC 架構的新區段。 否則可能會市嶄新的設計，含有不同的色彩、 字型和配置。 將這類變更套用至整個站台，應該是網頁的既快速又簡單的程序不需要修改數千個組成該網站。

在 ASP.NET 中建立全網站頁面範本可透過使用的*主版頁面*。 簡而言之，主版頁面是一種特殊類型的定義中所有常見的標記的 ASP.NET 網頁*內容頁面*以及自訂內容頁面的內容頁面為基礎的區域。 （內容的頁面是繫結至主版頁面的 ASP.NET 網頁）。每當變更主版頁面的版面配置或格式化時，所有其內容頁的輸出會同樣的立即更新，這讓使用者套用全站台的外觀會變更，只要更新和部署到單一檔案 （也就是主版頁面）。

這是一系列瀏覽使用主版頁面的教學課程中的第一個教學課程。 在此教學課程系列的過程中我們：

- 檢查建立主版頁面和其相關聯的內容頁面
- 討論各種不同的提示、 秘訣和設陷，
- 識別常見主版頁面的問題和因應措施，瀏覽
- 請參閱 < 如何從內容頁面和 a-相反，存取主版頁面
- 了解如何指定在執行階段的內容頁面的主版頁面和
- 其他進階主版頁面的主題。

這些教學課程被專很簡潔，並提供具有足夠的螢幕擷取畫面來引導您完成程序以視覺化方式的逐步指示。 每一個教學課程使用在 C# 和 Visual Basic 版本，包括使用的完整程式碼的下載。

本教學課程中我開頭看主版頁面的基本概念。 我們討論主版頁面的運作方式、 查看建立主版頁面和使用 Visual Web Developer 中，相關聯的內容頁面，請參閱如何主版頁面的變更會立即反映在其內容頁。 讓我們開始吧 ！

## <a name="understanding-how-master-pages-work"></a>了解主版頁面的運作方式

建立具有一致的全站台的頁面配置的網站需要每個網頁發出除了其自訂內容的一般格式標記。 例如，雖然 www.asp.net 上的每個教學課程或論壇文章都有自己唯一的內容，每個網頁也呈現一系列的一般`<div>`顯示最上層區段連結的項目： 首頁、 開始、 了解，等等。

有各種不同的技術，用於建立具有一致的外觀及操作的 web 網頁。 貝氏方法是只要複製並貼到所有的網頁，一般的版面配置標記，但這種方法有一些缺點。 簡單來說，每次建立新的頁面時，您必須記得複製並貼到頁面的 共用的內容。 這類複製和貼上作業與敞開錯誤可能不小心將共用標記的子集複製到新頁面。 和上方它，這個方法可讓新的實際痛苦因為必須編輯每個站台中的單一頁面，才能使用新的外觀及操作以取代現有的全站台的外觀。

在 ASP.NET 2.0 版中前, 頁面上通常用來放置常見標記中的開發人員[使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)然後在每個頁面中加入這些使用者控制項。 這種方法需要網頁開發人員手動將使用者控制項加入至每個新的頁面，請記住，但允許的簡單全站台的修改，因為更新的一般標記時使用者控制項所需修改。 不幸的是，Visual Studio.NET 2002年和 2003-版本的 Visual Studio 用來建立 ASP.NET 1.x 應用程式-以灰色方塊呈現在 [設計] 檢視中的使用者控制項。 因此，使用這種方法的網頁開發人員未不喜歡 WYSIWYG 的設計階段環境。

使用使用者控制項的缺點已解決在 ASP.NET 2.0 版和 Visual Studio 2005 中導入*主版頁面*。 主版頁面是一種特殊的 ASP.NET 頁面，定義全站台的標記和*區域*其中相關聯*內容頁面*定義其自訂的標記。 我們將會看到在步驟 1 中，這些區域是由 ContentPlaceHolder 控制項中定義。 ContentPlaceHolder 控制項只是表示主版頁面控制項階層架構中，由內容頁面可以插入自訂內容的位置。

> [!NOTE]
> ASP.NET 2.0 版後尚未變更的核心概念和主版頁面的功能。 不過，Visual Studio 2008 提供設計階段支援巢狀主版頁面，在 Visual Studio 2005 所缺少的功能。 我們將在未來的教學課程中使用巢狀主版頁面。


圖 2 顯示 www.asp.net 主版頁面的樣子。 請注意主版頁面定義常用全站台的版面配置的頂端、 底端，和每個頁面的右邊標記-，以及 ContentPlaceHolder 左方，每個個別的網頁的唯一內容所在的位置中。


![主版頁面定義全站台的配置和可編輯區域為基礎內容頁面的內容頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**圖 02**： 主版頁面定義全站台的配置和可編輯區域為基礎內容頁面的內容頁面


一旦定義主版頁面可以繫結至新的 ASP.NET 網頁，透過的刻度核取方塊。 呼叫內容頁面-這些 ASP.NET 網頁包括每個主版頁面的 ContentPlaceHolder 控制項內容的控制項。 當透過瀏覽器瀏覽內容的頁面時 ASP.NET 引擎建立的主版頁面控制項階層架構，並插入到適當的位置內容的頁面控制項階層架構。 轉譯此結合的控制項階層架構，而且產生的 HTML 會傳回給終端使用者的瀏覽器。 因此，[內容] 頁面會發出 ContentPlaceHolder 控制其主版頁面中定義的一般標記和定義於它自己的內容控制項內特定頁面的標記。 圖 3 說明這個概念。


[![要求的網頁標記為 Fused 至主版頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**圖 03**: 要求網頁的標記 Fused 至主版頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


既然我們已經討論過主版頁面工作的方式，讓我們看看建立主版頁面和使用 Visual Web Developer 中相關聯的內容頁面。

> [!NOTE]
> 若要前往的最大的可能對象，我們在此教學課程系列整個建置 ASP.NET 網站來建立 ASP.NET 3.5 與 Microsoft 的免費版本的 Visual Studio 2008， [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 如果您還沒有升級至 ASP.NET 3.5 中，也請別擔心-在這些教學課程工作中所討論的概念也使用 ASP.NET 2.0 和 Visual Studio 2005。 但是，某些示範應用程式可以使用以.NET Framework 3.5 版; 新增功能當使用 3.5 特有功能時，包含附註，討論如何在 2.0 版中實作類似的功能。 不要記住，示範應用程式可供下載從每個教學課程的目標.NET Framework 3.5 版中，會導致`Web.config`檔案，其中包含 3.5 特有的組態項目。 未先移除 3.5 特定標記的來源，將無法運作長劇本短時，如果您有尚未然後可下載的 web 應用程式的電腦上安裝.NET 3.5 `Web.config`。 請參閱[將分解 ASP.NET 3.5 版的`Web.config`檔案](http://www.4guysfromrolla.com/articles/121207-1.aspx)如需有關本主題。


## <a name="step-1-creating-a-master-page"></a>步驟 1： 建立主版頁面

我們可以建立和使用主要和內容頁面瀏覽之前，我們先需要 ASP.NET 網站。 開始建立新檔案系統以 ASP.NET 網站。 若要完成此動作，啟動 Visual Web Developer 然後移至 檔案 功能表並選擇新的網站，顯示新的網站 對話方塊 （請參閱圖 4）。 選擇 ASP.NET 網站範本、 設定到檔案系統位置下拉式清單中，選擇要將網站的資料夾和設定的語言為 Visual Basic。 這會建立新的網站與`Default.aspx`ASP.NET 網頁`App_Data`資料夾，和`Web.config`檔案。

> [!NOTE]
> Visual Studio 支援的專案管理的兩種模式： 網站專案和 Web 應用程式專案。 網站專案缺少專案檔，而 Web 應用程式專案模仿專案架構在 Visual Studio.NET 2002年/2003年-它們包括專案檔和編譯成單一組件，放在專案的原始程式碼`/bin`資料夾。 Visual Studio 2005 一開始只支援的網站專案，雖然 Web 應用程式專案模型已經重新引入含 Service Pack 1。Visual Studio 2008 提供這兩個專案的模型。 Visual Web Developer 2005 和 2008年版本，不過，僅支援的網站專案。 我使用我的示範，本教學課程系列的網站專案模型。 如果您正在使用非 Express edition，而且想要使用[Web 應用程式專案模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)相反地，您可以任意去以執行這項操作，但請注意螢幕和相對於必須採取的步驟上看到的內容之間可能會有些不一致螢幕擷取畫面所示，在這些教學課程所提供的指示。


[![建立新檔案系統為基礎的網站](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**圖 04**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


接下來，加入主版頁面的根目錄中的站台的專案名稱上按一下滑鼠右鍵，選擇 加入新項目並選取主版頁面的範本。 請注意主版頁面結尾副檔名`.master`。 這個新的主版頁面命名為`Site.master`並按一下 [新增]。


[![加入主版頁面名稱為網站 Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**圖 05**： 加入主版頁面具名`Site.master`網站 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


以下列宣告式標記會加入新的主版頁面檔案透過 Visual Web Developer 中建立主版頁面：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

宣告式標記中的第一行是[`@Master`指示詞](https://msdn.microsoft.com/library/ms228176.aspx)。 `@Master`指示詞是類似於[`@Page`指示詞](https://msdn.microsoft.com/library/ydy4x04a.aspx)出現在 ASP.NET 網頁中。 它會定義的伺服器端語言 (VB) 和資訊的位置和主版頁面的程式碼後置類別的繼承。

`DOCTYPE`和網頁的宣告式標記下方`@Master`指示詞。 此頁面包含四個伺服器端控制項以及靜態 HTML:

- **Web Form ( `<form runat="server">`)** -因為所有 ASP.NET 網頁通常 Web Form-因為主版頁面可能包含必須出現在 Web 表單中的 Web 控制項務必要將 Web Form 加入至您的主版頁面 （而非 Web Form 加入 e除此之外，每個內容頁面）。
- **ContentPlaceHolder 控制項，名為`ContentPlaceHolder1`**  -這個 ContentPlaceHolder 控制項中的 Web 表單隨即出現，並做為內容頁面的使用者介面區域。
- **伺服器端`<head>`元素**-`<head>`項目具有`runat="server"`屬性，因此可透過伺服器端程式碼存取。 `<head>`項目實作這種方式，讓網頁的標題和其他`<head>`-相關的標記可能加入，或以程式設計方式調整。 例如，設定 ASP.NET 網頁的`Title`屬性變更`<title>`所呈現項目`<head>`伺服器控制項。
- **ContentPlaceHolder 控制項，名為`head`**  -這個 ContentPlaceHolder 控制項出現在內`<head>`伺服器控制項，並可用來以宣告方式將內容加入到`<head>`項目。

此預設主版頁面的宣告式標記做為起點來設計您自己的主版頁面。 隨意編輯 HTML 或加入其他 Web 控制項或 ContentPlaceHolders 主版頁面。

> [!NOTE]
> 當設計主版頁面，確定主版頁面包含 Web Form，而且至少一個 ContentPlaceHolder 控制會出現在此 Web Form。


### <a name="creating-a-simple-site-layout"></a>建立簡單的站台版面配置

讓我們來展開`Site.master`的預設宣告式標記建立其中所有頁面的都共用的網站配置： 常見的標頭; 瀏覽、 新聞和其他全站台的內容，會顯示 「 電源的 Microsoft ASP.NET 」 圖示頁尾與左側資料行。 圖 6 顯示主版頁面的最終結果，其內容頁的其中一個透過瀏覽器檢視時。 圖 6 中的紅色圈選的區域旨在說明所造訪的頁面 (`Default.aspx`); 其他內容是在主版頁面中定義且因此一致跨所有內容頁面。


[![主版頁面的頂端、 左邊和下方部分定義標記](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**圖 06**： 主版頁面的頂端、 左邊和下方部分定義標記 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


若要達到圖 6 中所顯示的網站配置，一開始更新`Site.master`主版頁面，使其包含下列的宣告式標記：

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

主版頁面的配置會定義使用一系列的`<div>`HTML 項目。 `topContent` `<div>`包含出現在每個頁面頂端的標記時`mainContent`， `leftContent`，和`footerContent` `<div>` s 用來顯示頁面的內容、 左側資料行和 「 電源已由 MicrosoftASP.NET"圖示，分別。 除了新增這些`<div>`項目，我也重新命名`ID`主要 ContentPlaceHolder 控制項從屬性`ContentPlaceHolder1`至`MainContent`。

格式和配置規則，這些各種`<div>`項目拼寫中[階層式樣式表 (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)檔案`Styles.css`，透過指定`<link>`項目中的主版頁面的`<head>`項目。 這些不同的規則定義的每個外觀與風格`<div>`先前所述的項目。 例如， `topContent` `<div>`元素，會顯示"主版頁面教學課程 」 文字及連結，其具有格式規則中指定`Styles.css`，如下所示：

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

如果您要遵照在您的電腦，您必須下載本教學課程隨附的程式碼並將加入`Styles.css`檔案加入專案。 同樣地，您也需要建立一個名為資料夾`Images`並下載的示範網站的 「 電源的 Microsoft ASP.NET 」 圖示將複製到您的專案。

> [!NOTE]
> CSS 和網頁格式的討論超出本文章的範圍。 如需 CSS 的詳細資訊，請參閱[CSS 教學課程](http://www.w3schools.com/css/default.asp)在[W3Schools.com](http://www.w3schools.com/)。也建議您下載本教學課程隨附的程式碼、 使用中的 CSS 設定`Styles.css`來查看不同的格式規則的效果。


### <a name="creating-a-master-page-using-an-existing-design-template"></a>建立使用現有設計範本主版頁面

過去幾年我建立 ASP.NET web 應用程式的小型-至中型公司的數的字。 某些用戶端有現有的站台版面配置，選擇他們想要使用;其他人雇用一流的圖形設計工具。 幾個託給我設計網站配置。 如您所見的圖 6，工作的程式設計人員設計的網站配置最好是通常為為具有您會計執行 open-heart surgery 而醫生則您的稅金。

幸運的是，有提供免費的 HTML 設計範本的 innumerous 網站-Google 傳回超過 6 百萬個結果的搜尋字詞 」 可用的網站範本 」。 我最喜歡的項目是[OpenDesigns.org](http://opendesigns.org/)。一旦您找到您要的網站範本，將 CSS 檔案和影像加入至您的網站專案，並將範本的 HTML 整合到您的主版頁面。

> [!NOTE]
> Microsoft 也提供了許多[釋放 ASP.NET 設計開始套件範本](https://msdn.microsoft.com/asp.net/aa336613.aspx)，整合到 Visual Studio 中的 [新的網站] 對話方塊。


## <a name="step-2-creating-associated-content-pages"></a>步驟 2： 建立相關聯的內容頁面

建立主版頁面時，我們已經準備好開始建立 ASP.NET 網頁繫結至主版頁面項目。 這類頁面指*內容頁面*。

讓我們加入專案中加入新的 ASP.NET 頁面，並將它繫結`Site.master`主版頁面。 在 方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇 加入新項目選項。 選取的 Web 表單範本中，輸入名稱`About.aspx`，如圖 7 所示，然後核取 選取主版頁面 」 核取方塊。 如此一來就會顯示 選取主版頁面對話方塊方塊中您可以在其中選擇要使用的主版頁面的 （請參閱圖 8）。

> [!NOTE]
> 如果您已建立您的 ASP.NET 網站使用的 Web 應用程式專案的模型，而不網站專案模型不會看到圖 7 所示的 [加入新項目] 對話方塊中，「 選取主版頁面 」 核取方塊。 若要建立內容頁面時使用 Web 應用程式專案模型您必須選擇 Web 內容表單範本，而不是 Web 表單範本。 選取 Web 內容的表單範本，然後按一下 [新增] 之後, 相同選取主版頁面圖 8 所示的對話方塊隨即出現。


[![加入新的內容頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**圖 07**： 加入新的內容頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![選取 Site.master 主版頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**圖 08**： 選取`Site.master`主版頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


如下列宣告式標記顯示，新的內容頁面包含`@Page`，往回指向其主版頁面和內容控制項主版頁面的 ContentPlaceHolder 控制項的每個指示詞。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> 在步驟 1 中的 「 建立簡單的網站 」 版面配置區段我重新命名為`ContentPlaceHolder1`至`MainContent`。 如果您未重新命名此 ContentPlaceHolder 控制項`ID`同樣地，在內容頁面的宣告式標記會稍有不同的標記，如上所示。 也就是第二個內容控制項的`ContentPlaceHolderID`會反映`ID`對應 ContentPlaceHolder 的控制您的主版頁面中。


ASP.NET 引擎時呈現內容頁面，必須將網頁的內容使用其主版頁面的 ContentPlaceHolder 控制項的控制項。 ASP.NET 引擎會決定內容頁面的主版頁面從`@Page`指示詞的`MasterPageFile`屬性。 此內容的頁面如下所示的上述的標記，繫結至`~/Site.master`。

因為主版頁面有兩個 ContentPlaceHolder 控制項-`head`和`MainContent`-Visual Web Developer 中產生兩個內容控制項。 每個內容控制項參考特定 ContentPlaceHolder 透過其`ContentPlaceHolderID`屬性。

透過先前的全站台的範本技術地位主版頁面的位置是設計階段支援。 圖 9 顯示`About.aspx`透過 Visual Web Developer 的 [設計] 檢視中檢視時的內容頁面。 請注意，在主版頁面內容時顯示，它會呈現灰色且無法修改。 對應至主版頁面的 ContentPlaceHolders 將內容控制項，不過，是可編輯。 就像其他任何 ASP.NET 網頁，您可以建立內容頁面的介面加入透過來源或設計檢視的 Web 控制項。


[![內容頁面的 [設計] 檢視會顯示這兩個指定頁面和主版頁面內容](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**圖 09**: 內容頁面的設計檢視會顯示頁面專屬和都主版頁面內容 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>內容頁面中加入標記和 Web 控制項

請花一點時間建立的某些內容`About.aspx`頁面。 您可以看到在圖 10 中，輸入 「 關於作者 」 標題和幾個段落的文字，不過可自由太新增 Web 控制項。 在建立之後這個介面，請瀏覽`About.aspx`透過瀏覽器的頁面。


[![瀏覽 about.aspx 的網頁 頁面，透過瀏覽器](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**圖 10**： 瀏覽`About.aspx`頁面透過瀏覽器 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


請務必了解，要求的內容頁面和其相關聯的主版頁面 fused 而呈現整個完全在 web 伺服器上。 然後，終端使用者的瀏覽器會傳送產生、 合成 HTML。 若要確認這種情況，請檢視瀏覽器移至 [檢視] 功能表並選擇來源收到的 HTML。 請注意，有任何框架或其他任何特殊的技術，在單一視窗中顯示兩個不同的網頁。

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>繫結至現有的 ASP.NET 網頁的主版頁面

如我們在此步驟中所見，ASP.NET web 應用程式中加入新的內容頁面是容易，只要選取 「 選取主版頁面 」 的核取方塊，然後挑選主版頁面。 不幸的是，將現有的 ASP.NET 網頁轉換成主版頁面也不簡單。

要繫結至現有的 ASP.NET 網頁的主版頁面中，您必須執行下列步驟：

1. 新增`MasterPageFile`屬性加入 ASP.NET 網頁的`@Page`指示詞，指向適當的主版頁面。
2. ContentPlaceHolders 主版頁面中的每個將內容控制項。
3. 選擇性地剪下並將現有的 ASP.NET 網頁的內容貼到適當的內容控制項。 我說 「 選擇性 」 此處，因為 ASP.NET 頁面可能包含標記為已透過表示主版頁面中，例如`DOCTYPE`、`<html>`項目和 Web Form。

如需這個程序，以及螢幕擷取畫面的逐步指示，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的[使用主版頁面和站台瀏覽](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx)教學課程。 「 更新`Default.aspx`和`DataSample.aspx`使用主版頁面 」 一節詳細說明這些步驟。

因為它更容易建立新的內容頁面，比轉換的內容頁面中的現有 ASP.NET 網頁，建議您每當您建立新的 ASP.NET 網站會將主版頁面新增至站台。 將所有新的 ASP.NET 網頁的繫結至這個主版頁面。 別擔心，如果初始的主版頁面很簡單或純文字。您可以在稍後更新主版頁面。

> [!NOTE]
> 在建立新的 ASP.NET 應用程式時，Visual Web Developer 中加入`Default.aspx`並未繫結至主版頁面的頁面。 如果您想要將現有的 ASP.NET 網頁轉換成內容頁的練習，請繼續進行和執行這項操作與`Default.aspx`。 或者，您可以刪除`Default.aspx`然後重新加入，但是這次檢查 「 選取的主版頁面 」 核取方塊。


## <a name="step-3-updating-the-master-pages-markup"></a>步驟 3： 更新主版頁面的標記

主版頁面的主要優點之一是單一的主版頁面可能用於在網站上定義許多網頁的整體配置。 因此，更新站台的外觀及操作，需要更新單一檔案的主版頁面。

若要將說明此行為，讓我們來更新我們包含頂端的左側資料行中的目前日期的主版頁面。 新增名為標籤`DateDisplay`至`leftContent` `<div>`。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

接下來，建立`Page_Load`事件處理常式的主要頁面上，並加入下列程式碼：

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

上述程式碼會設定標籤的`Text`屬性目前的日期和時間格式化為一週的星期幾、 月份和兩位數天數的名稱 （請參閱圖 11）。 透過這項變更，重新檢查您的內容頁面。 圖 11 顯示，因為產生的標記會立即更新要包含的主版頁面的變更。


[![主版頁面所做的變更會反映時檢視內容頁面](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**圖 11**： 主版頁面的變更會反映時檢視內容頁面 ([按一下以檢視完整大小的影像](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> 如本範例所示，主版頁面可能包含伺服器端 Web 控制項，程式碼和事件處理常式。


## <a name="summary"></a>總結

主版頁面可讓 ASP.NET 開發人員設計可輕鬆地更新一致全站台的配置。 建立主版頁面和其相關聯的內容頁很簡單，只建立標準的 ASP.NET 網頁，Visual Web Developer 提供豐富的設計階段支援。

我們在本教學課程中建立的主版頁面範例有兩個 ContentPlaceHolder 控制項，前端和 MainContent。 我們僅指定 MainContent ContentPlaceHolder 控制項標記在內容頁面中，不過。 我們在下一個教學課程中使用多個內容控制項的內容頁面。 我們也會了解如何定義預設標記的內容控制項內主版頁面中，以及如何若要切換使用預設標記中定義主要頁面，並提供自訂的標記，從 [內容] 頁面。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 設計工具： 上建置 ASP.NET 網站使用 Web 標準釋放設計範本和指引](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET 主版頁面概觀](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [階層式樣式表 (CSS) 教學課程](http://www.w3schools.com/css/default.asp)
- [動態設定頁面的標題](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [在 ASP.NET 中的主版頁面](http://www.odetocode.com/articles/419.aspx)
- [主版頁面的快速入門教學課程](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一頁](nested-master-pages-cs.md)
[下一頁](multiple-contentplaceholders-and-default-content-vb.md)
