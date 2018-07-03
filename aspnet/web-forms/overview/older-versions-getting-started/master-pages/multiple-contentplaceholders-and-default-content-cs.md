---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: 多個 ContentPlaceHolders 和預設內容 (C#) |Microsoft Docs
author: rick-anderson
description: 將探討如何將多個內容中的預留位置新增至主版頁面，以及如何指定內容預留位置的預設內容。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: 19c9d03d9aacdf842fb12bd16c83859ec4299d68
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390080"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>多個 ContentPlaceHolders 和預設內容 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> 將探討如何將多個內容中的預留位置新增至主版頁面，以及如何指定內容預留位置的預設內容。


## <a name="introduction"></a>簡介

在先前的教學課程中，我們檢查主版頁面啟用的方式來建立一致的全網站的版面配置的 ASP.NET 開發人員。 主版頁面定義通用於所有其內容頁面的標記和可自訂的頁面為基礎的區域。 在上一個教學課程中，我們建立簡單的主版頁面 (`Site.master`) 和兩個內容頁面 (`Default.aspx`和`About.aspx`)。 我們的主版頁面包含名為兩個 ContentPlaceHolders`head`並`MainContent`，其位於`<head>`項目和 Web Form，分別。 內容的頁面都會有兩個內容控制項，而僅指定一個對應至標記`MainContent`。

中的兩個 ContentPlaceHolder 控制項，證明`Site.master`，主版頁面可能包含多個 ContentPlaceHolders。 不僅如此，主版頁面可以指定 ContentPlaceHolder 控制項的預設標記。 內容頁面，然後，可以選擇性地指定自己的標記或使用預設標記。 在本教學課程中我們探討使用主版頁面中的多個內容控制項，並了解如何定義 ContentPlaceHolder 控制項中的預設標記。

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>步驟 1： 將其他 ContentPlaceHolder 控制項加入至主版頁面

許多網站設計包含在螢幕上自訂頁面的頁面為基礎的數個區域。 `Site.master`我們在先前的教學課程中，建立主版頁面包含名為 Web 表單內的單一 ContentPlaceHolder `MainContent`。 具體來說，是位於此 ContentPlaceHolder `mainContent` `<div>`項目。

[圖 1] 顯示`Default.aspx`透過瀏覽器檢視時。 以紅色圈起的區域是對應至特定頁面的標記`MainContent`。


[![圈選的區域顯示的區域目前可自訂的頁面為基礎](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**圖 01**： 圓框區域顯示區域目前可自訂的頁面為基礎 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


想像一下，除了 [圖 1] 所示的區域，我們也需要將頁面特定的項目新增至下方的課程和新聞的左資料行區段。 若要這麼做，我們將另一個 ContentPlaceHolder 控制項加入主版頁面。 若要跟著做，請開啟`Site.master`主版頁面，在 Visual Web Developer 中，然後將 ContentPlaceHolder 控制項從工具箱拖曳至設計工具 [新聞] 區段之後。 設定 ContentPlaceHolder`ID`至`LeftColumnContent`。


[![ContentPlaceHolder 控制項加入主版頁面的左側資料行](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**圖 02**: ContentPlaceHolder 控制項加入主版頁面的左側資料行 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


加上`LeftColumnContent`ContentPlaceHolder 主版頁面，我們可以定義此區域的內容頁的頁面為基礎所包含的內容控制項中頁面`ContentPlaceHolderID`設定為`LeftColumnContent`。 我們會探討此程序的步驟 2。

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>步驟 2： 內容頁中定義新的 ContentPlaceHolder 的內容

將新的內容頁面加入至網站中，Visual Web Developer 會自動建立的內容控制項中選取的主版頁面中的每個 ContentPlaceHolder 的頁面。 新增`LeftColumnContent`ContentPlaceHolder 我們在步驟 1 中，新 ASP.NET 網頁會立即的主版頁面有三個內容控制項。

為了說明這點，將新的內容頁面新增至名為根目錄`MultipleContentPlaceHolders.aspx`繫結至`Site.master`主版頁面。 Visual Web Developer 會使用下列的宣告式標記建立此頁面：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

內容控制項參考時，輸入某些內容`MainContent`ContentPlaceHolders (`Content2`)。 接下來，新增下列標記，即可`Content3`內容控制項 (參考`LeftColumnContent`ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

之後新增此標記，請瀏覽透過瀏覽器頁面。 如 [圖 3] 所示，標記放在`Content3`（以紅色圈起） 的 [新聞] 區段下方的左側資料行中顯示內容的控制項。 標記置於`Content2`隨即出現 （藍色圓框） 的頁面部分。


[![左側的資料行現在會包含 [新聞] 區段下方的頁面特定內容](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**[圖 03**: 左邊資料行現在包含頁面特定內容下方新聞] 區段 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>現有的內容頁中定義的內容

建立新的內容頁面會自動納入我們在步驟 1 中新增 ContentPlaceHolder 控制項。 但我們兩個的現有內容頁面-`About.aspx`並`Default.aspx`-不需要的內容控制項`LeftColumnContent`ContentPlaceHolder。 若要指定這些兩個現有的頁面中此 ContentPlaceHolder 內容，我們需要自行加入內容控制項。

不像大多數的 ASP.NET Web 控制項，Visual Web Developer 工具箱不包含內容控制項的項目。 我們可以手動輸入內容的控制項宣告式標記中原始碼 檢視中，但更容易且更快速的方法是使用 設計 檢視。 開啟`About.aspx`頁面上，並切換至 [設計] 檢視。 圖 4 所示， `LeftColumnContent` ContentPlaceHolder 出現在 設計 檢視中，如果您將滑鼠移它，顯示的標題會讀取:"LeftColumnContent （主要） 」。 標題中包含的 「 主要 」 表示頁面中定義的這個 ContentPlaceHolder 沒有內容控制項。 ContentPlaceHolder，如所示的案例的內容控制項有`MainContent`，會讀取標題: 「*ContentPlaceHolderID* （自訂）。 」

若要加入內容控制項用於`LeftColumnContent`ContentPlaceHolder 至`About.aspx`、 展開 ContentPlaceHolder 的智慧標籤，然後按一下 建立自訂內容的連結。


[![About.aspx 的網頁的 [設計] 檢視顯示 LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**圖 04**： 針對 [設計] 檢視`About.aspx`示範`LeftColumnContent`ContentPlaceHolder ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


按一下 建立自訂內容的連結會產生所需之內容控制項中的頁面並將設定其`ContentPlaceHolderID`ContentPlaceHolder 屬性`ID`。 例如，按一下 建立自訂內容連結以獲得`LeftColumnContent`區域中的`About.aspx`將下列宣告式標記加入網頁：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>略過內容控制項

ASP.NET 不需要所有的內容頁面，包含的主版頁面中定義的每個 ContentPlaceHolder 內容控制項。 如果省略內容控制項，則 ASP.NET 引擎會使用主版頁面中 ContentPlaceHolder 內定義的標記。 此標記指 ContentPlaceHolder*預設內容*及其中內容的某些區域是常見的作法大部分的網頁，但其必須是可自訂的一小部分的頁面的案例中很有用。 步驟 3 將探索主版頁面中指定的預設內容。

目前，`Default.aspx`包含兩個內容控制項`head`並`MainContent`ContentPlaceHolders; 它不需要的內容控制項`LeftColumnContent`。 因此，當`Default.aspx`呈現`LeftColumnContent`用 ContentPlaceHolder 的預設內容。 因為我們尚未為這個 ContentPlaceHolder 定義任何預設內容中，最後的結果是，不需要標記，就會發出此區域。 若要確認這種行為，請瀏覽`Default.aspx`透過瀏覽器。 如 [圖 5] 所示，就會不發出任何標記 [新聞] 區段下方的左側資料行中。


[![LeftColumnContent ContentPlaceHolder 呈現沒有內容](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**圖 05**： 沒有內容呈現`LeftColumnContent`ContentPlaceHolder ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>步驟 3： 在主版頁面中指定預設的內容

有些網站設計包括其內容是相同的站台，除了一個或兩個例外狀況中的所有頁面的區域。 請考慮支援使用者帳戶的網站。 這類站台都需要其中訪客可以輸入其認證來登入網站的登入頁面。 若要加速登入程序，網站設計人員可能包含使用者名稱和密碼的文字方塊中的每一頁可讓使用者登入，而不需明確地造訪登入頁面的左上角。 雖然這些使用者名稱和密碼的文字方塊很有幫助，大部分的頁面中，它們是在登入頁面中，已經包含使用者的認證的文字方塊備援。

若要實作這項設計，您可以建立 ContentPlaceHolder 控制項在主版頁面的左上角。 顯示其左上角的使用者名稱和密碼的文字方塊時所需的每個頁面會建立此 ContentPlaceHolder 內容控制項，並新增必要的介面。 登入頁面上，相反地，可能會省略此 ContentPlaceHolder 加入內容控制項，或會建立內容控制項，以定義任何標記。 這種方法的缺點是，我們必須記住使用者名稱和密碼的文字方塊加入每一頁，我們將新增至站台 （除了 [登入] 頁面中）。 這會招惹麻煩。 我們可能忘了將這些文字方塊加入至頁面或兩個或更糟的是，我們可能未實作介面正確 （可能新增一個文字方塊，而非兩個）。

更好的解決方案是做為 ContentPlaceHolder 的預設內容中定義的使用者名稱和密碼的文字方塊。 如此一來，我們只需要覆寫此預設內容，不會顯示使用者名稱和密碼文字方塊的幾個頁面中 （登入頁面上，執行個體）。 為了說明 ContentPlaceHolder 控制項指定的預設內容，讓我們實作剛剛所討論的案例。

> [!NOTE]
> 本教學課程的其餘部分會更新我們的網站所有頁面，但登入頁面的左側資料行中包含登入介面。 不過，本教學課程不會檢查如何設定網站，以支援使用者帳戶。 如需有關本主題的詳細資訊，請參閱我[表單驗證、 授權、 使用者帳戶和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)教學課程。


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>新增 ContentPlaceHolder 並指定其預設內容

開啟`Site.master`主版頁面，並將下列標記新增至左側的資料行之間`DateDisplay`標籤和課程區段：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

新增此標記之後主版頁面的 設計 檢視看起來應該類似於 圖 6。


[![主版頁面包含登入控制項](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**圖 06**： 主版頁面包含登入控制項 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


此 ContentPlaceHolder， `QuickLoginUI`，已登入 Web 控制項做為其預設內容。 Login 控制項顯示使用者介面會提示使用者輸入其使用者名稱和密碼，以及登入 按鈕。 按一下 [登入] 按鈕，登入控制項在內部驗證使用者的認證，針對成員資格 API。 若要使用此登入控制項，在實務上，然後，您要設定您的站台使用成員資格。 本主題已超出本教學課程; 的範圍請參閱我[表單驗證、 授權、 使用者帳戶和角色](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)如需有關建置支援使用者帳戶的 web 應用程式的教學課程。

您可以自訂登入控制項的外觀或行為。 我已設定其兩個屬性：`TitleText`和`FailureAction`。 `TitleText`屬性值，預設為 「 登入 」，會顯示在控制項的使用者介面的頂端。 我已設定這個屬性，使其顯示 [登入] 的文字做為`<h3>`項目。 `FailureAction`屬性會指出如果使用者的認證無效，該怎麼辦。 預設值為`Refresh`，，讓使用者保持在相同頁面上，且會顯示登入控制項內的失敗訊息。 我已將它變更為`RedirectToLoginPage`，其會將使用者傳送至發生無效的認證時登入頁面。 我想要將使用者傳送登入頁面中，當使用者嘗試登入從其他頁面，但失敗，因為登入頁面可以包含額外的指示和選項，輕鬆地不符合左側資料行。 例如，登入頁面可能包含選項來擷取遺忘的密碼，或建立新的帳戶。

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>建立登入頁面，並覆寫預設內容

完成的主版頁面中，我們的下一個步驟是建立登入頁面。 將 ASP.NET 網頁新增至名為的站台的根目錄`Login.aspx`，繫結至`Site.master`主版頁面。 這樣會使用四個內容控制項建立頁面，其中每個 ContentPlaceHolders 定義於`Site.master`。

將登入控制項，加入`MainContent`內容控制項。 同樣地，請隨意新增任何內容`LeftColumnContent`區域。 不過，請確定保留的內容控制項`QuickLoginUI`ContentPlaceHolder 空白。 這可確保控制項不會出現登入頁面的左側資料行中的登入。

在定義的內容之後`MainContent`和`LeftColumnContent`區域，您的登入頁面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

圖 7 顯示此頁面上，當透過瀏覽器檢視。 因為此頁面指定的內容控制項`QuickLoginUI`ContentPlaceHolder，它會覆寫主版頁面中指定的預設內容。 結果是在主版頁面的設計檢視 （請參閱 圖 6） 則不會呈現此頁面中顯示登入控制項。


[![登入頁面 Represses QuickLoginUI ContentPlaceHolder 的預設內容](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**圖 07**: 登入頁面 Represses `QuickLoginUI` ContentPlaceHolder 的預設內容 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>在 新頁面中使用的預設內容

我們想要顯示的登入頁面以外的所有頁面左側的資料行中的 登入控制項。 若要這麼做，請登入頁面除外的所有內容頁面應該省略的內容控制項`QuickLoginUI`ContentPlaceHolder。 藉由略過的內容控制項，將會改為使用 ContentPlaceHolder 的預設內容。

我們現有的內容頁面- `Default.aspx`， `About.aspx`，並`MultipleContentPlaceHolders.aspx`-並包含的內容控制項`QuickLoginUI`因為之前我們該 ContentPlaceHolder 控制項加入至主版頁面建立它們。 因此，這些現有的頁面不需要更新。 不過，新的頁面新增至網站包含的內容控制項`QuickLoginUI`ContentPlaceHolder，根據預設。 因此，我們必須記住要移除這些內容控制項每次我們新增新的內容頁面 （除非我們想要覆寫 ContentPlaceHolder 的預設內容，如果是登入頁面）。

若要移除的內容控制項，您可以手動從來源檢視中刪除其宣告式標記或，從 設計 檢視中，選擇 預設為主版頁面的內容連結來從它的智慧標籤。 兩種方法移除內容控制項的頁面，並會產生相同 net 效果。

[圖 8] 顯示`Default.aspx`透過瀏覽器檢視時。 請記得，`Default.aspx`只有在其宣告式標記-一個用於指定的兩個內容控制項`head`，另一個用於`MainContent`。 如此一來，預設的內容`LeftColumnContent`和`QuickLoginUI`ContentPlaceHolders 會顯示。


[![會顯示預設內容 LeftColumnContent 和 QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**圖 08**: 預設內容`LeftColumnContent`並`QuickLoginUI`ContentPlaceHolders 顯示 ([按一下以檢視完整大小的影像](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>總結

ASP.NET 主版頁面模型可讓任意數目的 ContentPlaceHolders 主版頁面中。 更甚者 ContentPlaceHolders 包含預設內容，就會發出在案例中，沒有對應的內容在 [內容] 頁面中的控制項。 在本教學課程中，我們看到如何納入主版頁面中的其他 ContentPlaceHolder 控制項，以及如何定義這些新的和現有的 ASP.NET 頁面中的新 ContentPlaceHolders 內容控制項。 我們也討論過指定預設內容 ContentPlaceHolder，這是在只有少數的頁面必須自訂被標準化的內容放在特定區域內的案例中有用。

在下一個教學課程中，我們將檢驗`head`ContentPlaceHolder 的詳細資料，查看如何以宣告方式或以程式設計方式定義的標題、 中繼標籤及其他 HTML 標頭頁的頁面為基礎。

快樂地寫程式 ！

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Suchi Banerjee。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一頁](creating-a-site-wide-layout-using-master-pages-cs.md)
> [下一頁](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
