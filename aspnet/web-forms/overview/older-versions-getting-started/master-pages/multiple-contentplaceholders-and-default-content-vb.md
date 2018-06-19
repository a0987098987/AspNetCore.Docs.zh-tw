---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: 多個 ContentPlaceHolders 和預設的內容 (VB) |Microsoft 文件
author: rick-anderson
description: 會檢查如何將多個內容的預留位置加入至主版頁面，以及如何指定預設的內容中內容的預留位置。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd1d8f34dba52a04c0d9f6a1961df7b97405b42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889445"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>多個 ContentPlaceHolders 和預設的內容 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> 會檢查如何將多個內容的預留位置加入至主版頁面，以及如何指定預設的內容中內容的預留位置。


## <a name="introduction"></a>簡介

在前述教學課程中我們檢查主版頁面的啟用方式 ASP.NET 開發人員建立一致的全站台的版面配置。 主版頁面定義通用於所有其內容頁的標記和頁面的頁面為基礎的可自訂的區域。 在上一個教學課程中，我們建立簡單的主版頁面 (`Site.master`) 和兩個內容頁面 (`Default.aspx`和`About.aspx`)。 我們的主版頁面包含名為兩個 ContentPlaceHolders`head`和`MainContent`，其位於`<head>`項目和 Web Form 中，分別。 時內容的頁面每個具有兩個內容控制項，我們只可以指定針對對應至一個標記`MainContent`。

中的兩個 ContentPlaceHolder 控制項證明`Site.master`，主版頁面可能包含多個 ContentPlaceHolders。 不僅如此，主版頁面可以指定預設 ContentPlaceHolder 控制項的標記。 內容頁面，然後，可以選擇性地指定自己的標記或使用預設標記。 在此教學課程中我們會查看主版頁面中使用多個內容控制項，並了解如何定義 ContentPlaceHolder 控制項中的預設標記。

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>步驟 1： 將其他 ContentPlaceHolder 控制項加入至主版頁面

許多網站設計包含數個區域在螢幕上所自訂的頁面為基礎。 `Site.master`我們在先前的教學課程中，建立的主版頁面包含名為 Web 表單內的單一 ContentPlaceHolder `MainContent`。 具體而言，是位於此 ContentPlaceHolder `mainContent` `<div>`項目。

圖 1 顯示`Default.aspx`透過瀏覽器檢視時。 系統會以紅色圈選起來的區域對應至特定頁面的標記`MainContent`。


[![圈選的區域顯示的區域目前可自訂的頁面為基礎](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**圖 01**： 圓框區域為基礎 頁面的顯示區域目前可自訂 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


假設除了區域圖 1 所示，我們也需要網頁特定項目加入下方課程及新聞左側資料行區段。 若要達成此目的，我們將另一個 ContentPlaceHolder 控制項加入主版頁面。 若要跟著做，請開啟`Site.master`主版頁面，在 Visual Web Developer 中，然後將 ContentPlaceHolder 控制項從工具箱拖曳至設計工具拖曳新聞區段之後。 設定 ContentPlaceHolder`ID`至`LeftColumnContent`。


[![ContentPlaceHolder 控制項加入主版頁面的左側資料行](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**圖 02**: ContentPlaceHolder 控制項加入主版頁面的左側資料行 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


加上的`LeftColumnContent`ContentPlaceHolder 主版頁面中的，我們可以定義此區域的內容頁的頁面為基礎所包含的內容控制項在頁面`ContentPlaceHolderID`設`LeftColumnContent`。 我們會檢查此程序的步驟 2。

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>步驟 2： 在內容頁面中定義新的 ContentPlaceHolder 的內容

將新的內容頁面加入至網站，Visual Web Developer 會自動建立的內容控制項中選取的主版頁面中的每個 ContentPlaceHolder 的頁面。 新增`LeftColumnContent`ContentPlaceHolder 至在步驟 1 中，新 ASP.NET 網頁會立即我們主版頁面有三個內容控制項。

為了說明這點，將新的內容頁面加入至名為的根目錄`MultipleContentPlaceHolders.aspx`繫結至`Site.master`主版頁面。 Visual Web Developer 會以下列宣告式標記建立此頁面：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

內容控制項的參考時，輸入某些內容`MainContent`ContentPlaceHolders (`Content2`)。 接下來，加入下列標記以`Content3`內容控制項 (它會參考`LeftColumnContent`ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

在新增之後此標記，請瀏覽透過瀏覽器頁面。 如圖 3 所示，標記放置在`Content3`（以紅色圈選起來） [新聞] 區段下方的左邊欄位中顯示內容控制項。 標記放置在`Content2`隨即出現 （藍色中所圈出） 頁面的右邊。


[![左側資料行現在會包含 [新聞] 區段下方的特定頁面的內容](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**圖 03**: 左邊資料行現在包含頁面的特定內容下方新聞區段 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>在現有的內容頁面中定義的內容

建立新的內容頁面會自動併入我們加入在步驟 1 中的 ContentPlaceHolder 控制項。 但我們兩個的現有內容頁面-`About.aspx`和`Default.aspx`-不需要的內容控制項`LeftColumnContent`ContentPlaceHolder。 若要指定此 ContentPlaceHolder 這兩個現有的分頁中的內容，我們需要自行加入內容控制項。

不像大多數的 ASP.NET Web 控制項，Visual Web Developer 工具箱不包含內容控制項項目。 我們可以手動輸入內容控制項的宣告式標記中的來源檢視，但更容易且更快速的方法是使用 [設計] 檢視。 開啟`About.aspx`頁面上，並切換至 [設計] 檢視。 如圖 4 所示， `LeftColumnContent` ContentPlaceHolder 會顯示在 [設計] 檢視中，如果滑鼠游標移，顯示的標題會讀取:"LeftColumnContent （主要） 」。 標題中包含的 「 主要 」 表示沒有此 ContentPlaceHolder 的頁面中定義的內容控制項。 如果那里存在 ContentPlaceHolder，如同在案例中的內容控制項`MainContent`，標題會讀取:"*ContentPlaceHolderID* （自訂）。 」

若要加入的內容控制項`LeftColumnContent`到 ContentPlaceHolder `About.aspx`、 展開 ContentPlaceHolder 的智慧標籤，然後按一下 建立自訂內容的連結。


[![About.aspx 的網頁的 [設計] 檢視會顯示 LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**圖 04**: 設計檢視`About.aspx`顯示`LeftColumnContent`ContentPlaceHolder ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


按一下 建立自訂內容的連結會產生所需之內容的頁面和集合中的控制其`ContentPlaceHolderID`ContentPlaceHolder 屬性`ID`。 例如，按一下 建立自訂內容的連結，`LeftColumnContent`地區`About.aspx`將下列宣告式標記加入至頁面：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>省略內容控制項

ASP.NET 不需要所有的內容頁面，包含在主版頁面中定義的每個 ContentPlaceHolder 的內容控制項。 如果省略內容控制項，則 ASP.NET 引擎會使用主版頁面 ContentPlaceHolder 內定義的標記。 此標記指 ContentPlaceHolder*預設內容*及其中內容的某些區域通用的大部份的頁面，但必須是可自訂的量少的頁面的狀況中很有用。 步驟 3 中，瀏覽主版頁面中指定的預設內容。

目前，`Default.aspx`包含兩個內容控制項，`head`和`MainContent`ContentPlaceHolders; 它不需要的內容控制項`LeftColumnContent`。 因此，當`Default.aspx`呈現`LeftColumnContent`用 ContentPlaceHolder 的預設內容。 因為我們尚未定義此 ContentPlaceHolder 任何預設的內容，最後的結果是，沒有標記，就會發出此區域。 若要確認這種行為，請瀏覽`Default.aspx`透過瀏覽器。 如圖 5 所示，就會不發出任何標記 [新聞] 區段下方的左邊欄位中。


[![LeftColumnContent ContentPlaceHolder 呈現沒有內容](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**圖 05**： 沒有內容呈現`LeftColumnContent`ContentPlaceHolder ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>步驟 3： 在主版頁面中指定預設的內容

一些網站設計，包括其內容是相同的站台，除了一個或兩個例外狀況中的所有頁面的區域。 請考慮一個支援使用者帳戶的網站。 此類網站需要訪客其中輸入他們的認證來登入網站的登入頁面。 若要加速登入程序，網站設計人員可能包含使用者名稱和密碼的文字方塊中的每個頁面，即可讓使用者登入，而不需要明確地瀏覽的登入頁面的左上角。 雖然這些使用者名稱和密碼的文字方塊中大部分的頁面很有幫助，它們是多餘的登入頁面上，已經包含文字方塊之使用者的認證。

若要實作這項設計，您可以建立 ContentPlaceHolder 控制項中的主版頁面的左上角。 顯示其左上角的使用者名稱和密碼的文字方塊所需的每個頁面會建立此 ContentPlaceHolder 的內容控制項，並加入必要的介面。 登入頁面上，相反地，可能會省略此 ContentPlaceHolder 加入內容控制項，或建立內容控制項，以定義沒有標記。 這種方法的缺點是，我們必須記得要將使用者名稱和密碼的文字方塊，我們將新增至站台 （除了登入頁面） 的每一頁。 這要求時發生問題。 我們可能忘了將這些文字方塊加入至網頁或兩個或更糟的是，我們可能會實作介面正確 （或許新增一個文字方塊中，而非兩個）。

更好的解決方案是做為 ContentPlaceHolder 的預設內容中定義的使用者名稱和密碼的文字方塊。 如此一來，我們只需要覆寫這個預設的內容中不會顯示使用者名稱和密碼文字方塊的幾個頁面 （登入頁面上，執行個體）。 為了說明 ContentPlaceHolder 控制項指定預設的內容，讓我們實作剛才所討論的案例。

> [!NOTE]
> 本教學課程的其餘部分更新我們的網站要在所有頁面，但登入頁面的左側資料行中包含登入介面。 不過，本教學課程不會檢查如何設定網站以支援使用者帳戶。 如需有關本主題的詳細資訊，請參閱我[表單驗證、 授權、 使用者帳戶和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)教學課程。


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>加入 ContentPlaceHolder 並指定其預設內容

開啟`Site.master`主版頁面，並將下列標記加入至左側的資料行之間`DateDisplay`標籤和課程 > 一節：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

加入這個標記之後主版頁面的 [設計] 檢視看起來應該類似於圖 6。


[![主版頁面包含登入控制項](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**圖 06**： 主版頁面包含登入控制項 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


此 ContentPlaceHolder， `QuickLoginUI`，已登入 Web 控制項做為其預設內容。 登入控制項顯示的使用者介面，會提示使用者輸入其使用者名稱和密碼，以及登入 按鈕。 時按一下 [登入] 按鈕，登入控制項在內部會驗證使用者的認證，成員資格應用程式開發介面。 若要使用此登入控制項，在實務上，然後，您要設定站台使用成員資格。 本主題已超出本教學課程; 的範圍請參閱我[表單驗證、 授權、 使用者帳戶和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)教學課程，如需有關建置 web 應用程式支援的使用者帳戶。

請隨意自訂登入控制項的行為或外觀。 我已設定其兩個屬性：`TitleText`和`FailureAction`。 `TitleText`屬性值，預設值為"登入 」，會顯示在控制項的使用者介面的頂端。 我已設定這個屬性，使其顯示的文字 [登入] 為`<h3>`項目。 `FailureAction`屬性會指出如果使用者的認證無效，該怎麼辦。 預設值為`Refresh`，，讓使用者保持在相同頁面上，且會顯示登入控制項內的失敗訊息。 我已變更其`RedirectToLoginPage`，傳送的目標使用者發生無效的認證登入頁面。 我想要將登入頁面傳送給使用者，當使用者嘗試登入從其他頁面，而失敗，因為登入頁面可以包含額外的指示和輕鬆不會放入左側資料行的選項。 例如，登入頁面可能包含選項來擷取忘記的密碼，或建立新的帳戶。

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>建立登入頁面和覆寫預設內容

完整的主版頁面中下, 一步是建立登入頁面。 加入 ASP.NET 網頁站台的根目錄，名為`Login.aspx`，繫結至`Site.master`主版頁面。 這樣將會使用四個內容控制項建立頁面，各供一個 ContentPlaceHolders 定義在`Site.master`。

將登入控制項加入`MainContent`內容控制項。 同樣地，您可以任意去任何將內容加入到`LeftColumnContent`區域。 不過，請確定保留的內容控制項`QuickLoginUI`ContentPlaceHolder 空白。 這可確保控制項沒有登入頁面的左側資料行中出現的登入。

在定義的內容之後`MainContent`和`LeftColumnContent`區域，您的登入頁面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

圖 7 顯示透過瀏覽器檢視時，此頁面。 因為此頁面指定的內容控制項`QuickLoginUI`ContentPlaceHolder，它會覆寫主版頁面中指定的預設內容。 最後的結果是在主版頁面的設計檢視 （請參閱圖 6） 則不會呈現此頁面中，顯示登入控制項。


[![登入頁面 Represses QuickLoginUI ContentPlaceHolder 的預設內容](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**圖 07**: 登入頁面 Represses `QuickLoginUI` ContentPlaceHolder 的預設內容 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>在新的頁面中使用的預設內容

我們想要在登入頁面除外的所有頁面的左側資料行中顯示登入控制項。 若要達成此目的，除了登入頁面的所有內容頁面應該省略的內容控制項`QuickLoginUI`ContentPlaceHolder。 藉由略過的內容控制項，將會改為使用 ContentPlaceHolder 的預設內容。

我們現有的內容頁面- `Default.aspx`， `About.aspx`，和`MultipleContentPlaceHolders.aspx`-不包含的內容控制項`QuickLoginUI`因為我們該 ContentPlaceHolder 控制項加入至主版頁面之前所建立。 因此，這些現有的頁面不需要更新。 不過，網站新增新的頁面包含的內容控制項`QuickLoginUI`ContentPlaceHolder，根據預設。 因此，我們必須記得要移除這些內容控制項每次 （除非我們想要覆寫 ContentPlaceHolder 的預設內容，如果是登入頁面） 我們新增了新的內容頁面。

若要移除的內容控制項，您可以手動從來源檢視中刪除其宣告式標記或，從 設計 檢視中，選擇 預設為主版頁面的內容連結的智慧標記。 兩種方法移除內容控制項和產生的頁面從相同網路效果。

圖 8 顯示`Default.aspx`透過瀏覽器檢視時。 請記得，`Default.aspx`只有兩個內容控制項中的宣告式標記-一個用於指定`head`，另一個用於`MainContent`。 如此一來，預設的內容如`LeftColumnContent`和`QuickLoginUI`ContentPlaceHolders 會顯示。


[![顯示的 預設內容 LeftColumnContent 和 QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**圖 08**: 預設內容`LeftColumnContent`和`QuickLoginUI`ContentPlaceHolders 會顯示 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>總結

ASP.NET 主版頁面模型允許任意數目的 ContentPlaceHolders 主版頁面中。 什麼是多個，ContentPlaceHolders 包含預設內容，就會發出在案例中，沒有對應的內容控制項的內容頁面。 在此教學課程中我們了解如何將其他 ContentPlaceHolder 控制項加入主版頁面中，以及如何為新的和現有的 ASP.NET 網頁中的這些新 ContentPlaceHolders 定義內容控制項。 我們也看指定預設內容中 ContentPlaceHolder，這是適用於僅頁面需求來自訂其他少數標準化的特定區域內的內容的情況。

在下一個教學課程中我們將檢驗`head`ContentPlaceHolder 的詳細資料，查看如何以宣告方式和程式設計的方式定義標題、 meta 標記，以及其他 HTML 標頭頁面的頁面為基礎。

祝您程式設計 ！

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Suchi Banerjee。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](creating-a-site-wide-layout-using-master-pages-vb.md)
> [下一頁](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
