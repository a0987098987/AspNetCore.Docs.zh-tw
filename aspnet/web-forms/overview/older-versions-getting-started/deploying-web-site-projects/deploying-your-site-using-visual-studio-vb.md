---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: 部署您使用 Visual Studio (VB) 的網站 |Microsoft Docs
author: rick-anderson
description: Visual Studio 包含部署網站的工具。 深入了解這些工具在本教學課程。
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: e0dd38bc158dea37d614a48a6783525ed99cd97a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819185"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>部署您的網站使用 Visual Studio (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio 包含部署網站的工具。 深入了解這些工具在本教學課程。


## <a name="introduction"></a>簡介

先前的教學課程會探討如何部署簡單的 ASP.NET web 應用程式，到 web 主機提供者。 具體來說，本教學課程示範了如何從開發環境的必要的檔案傳輸到生產環境中使用的 FTP 用戶端，例如 FileZilla。 Visual Studio 也提供內建的工具，以便部署到 web 主機提供者。 本教學課程會檢查兩個工具： 複製網站工具，從遠端網頁伺服器使用 FTP 或 FrontPage Server Extensions; 來回，可以移動檔案與發行工具，可將整個網站複製到指定的位置。


> [!NOTE]
> 其他 Visual Studio 所提供的部署相關工具，包括[Web 安裝專案](https://msdn.microsoft.com/library/wx3b589t.aspx)並[Web 部署專案](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en)增益集。 Web 安裝專案封裝網站的內容，並在同一個 MSI 檔案的組態資訊。 此選項是在內部部署網站或公司銷售客戶在他們自己的網頁伺服器安裝的預先封裝的 web 應用程式的最有用。 Web 部署專案增益集是 Visual Studio 增益集，可協助差異指定組態建置的開發環境和生產環境。 在本教學課程系列，將不會討論 web 安裝專案Web 部署專案摘要說明[*常見組態差異之間開發和生產環境*](common-configuration-differences-between-development-and-production-vb.md)教學課程。


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>部署您的網站使用複製網站工具

Visual Studio 複製網站工具是獨立的 FTP 用戶端類似的功能。 簡單的說，「 複製網站工具可讓您連線到遠端的網站，透過 FTP 或 FrontPage Server Extensions。 類似於 FileZilla 的使用者介面，複製網站使用者介面包含兩個窗格︰ 左的窗格會列出本機檔案，而右窗格會列出這些檔案在目的地伺服器上的。

> [!NOTE]
> 複製網站工具才適用於網站專案。 當您使用 Web 應用程式專案時，visual Studio 並未提供這項工具。


讓我們看看使用複製網站工具發行到生產環境的書籍評論應用程式。 因為複製網站工具僅適用於使用網站專案模型，我們可以只檢查使用此工具搭配 BookReviewsWSP 專案的專案。 開啟該專案。

啟動複製網站工具專案，依序按一下 [方案總管] （[圖 1] 中，圓框此圖示）; 中的複製網站圖示或者，您也可以從 [網站] 功能表中選取複製網站選項。 兩種方法會啟動 圖 1 所示複製網站使用者介面只有 圖 1 中的左的窗格會填入，因為我們尚未連線到遠端伺服器。


[![複製網站工具使用者介面是分割成兩個窗格](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**圖 1**: 複製網站工具的使用者介面是分割成兩個窗格 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image3.png))


若要部署我們的網站，我們要先連接到 web 主機提供者。 按一下頂端的 複製網站使用者介面的 連接 按鈕。 這會顯示 [開啟網站] 對話方塊，[圖 2] 所示。

您可以連接至目的地網站，從左邊選取四個選項的其中一項：

- **檔案系統**-選取此選項可將網站部署到可存取的資料夾或網路共用，從您的電腦。
- **本機 IIS** -使用此選項可將網站部署至您的電腦上安裝 IIS web 伺服器。
- **FTP 站台**-連線到遠端網站上，使用 FTP。
- **遠端站台**-連接至遠端網站使用 FrontPage Server Extensions。

大部分的 web 主機提供者支援 FTP，但更少提供 FrontPage Server 擴充功能的支援。 基於這個理由，我選擇 FTP 站台 選項，然後輸入連接資訊，如 圖 2 所示。


[![指定目的地網站](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**圖 2**： 指定目的地網站 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image6.png))


連線之後，複製網站工具載入在遠端站台，在右窗格中的檔案，並指出每個檔案的狀態： 新增、 刪除、 已變更或未變更。 您可以在遠端站台，或反向的本機站台複製檔案。

讓我們新增 新頁面，即可 BookReviewsWSP 專案然後再加以部署，讓我們能夠查看動作中的複製網站工具。 Visual Studio 中建立新的 ASP.NET 網頁，名為的根目錄中`Privacy.aspx`。 已使用主版頁面的頁面`Site.master`並將您網站的隱私權原則新增至這個頁面。 圖 3 顯示 Visual Studio，建立此頁面之後。


[![新增新的頁面上名為&lt;程式碼&gt;Privacy.aspx&lt;/c o d&gt;到網站的根資料夾](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**圖 3**： 新增新的頁面上名為`Privacy.aspx`網站的根資料夾 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image9.png))


接下來，回到複製網站使用者介面。 如 [圖 4] 所示，左的窗格現在會包含新的檔案-`Policy.aspx`和`Policy.aspx.vb`。 不僅如此，這些檔案會以箭號圖示和狀態的新密碼，指出它們存在本機站台上，但不是能在遠端站台的方式標示。


[![複製網站工具包含 新增&lt;程式碼&gt;Privacy.aspx&lt;/c o d&gt;於它的左窗格中的頁面](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**圖 4**： 複製網站工具包含 [新增]`Privacy.aspx`於它的左窗格中的頁面 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image12.png))


若要部署新的檔案選取它們，然後按一下 將它們傳送到遠端站台的箭號圖示。 傳輸完成後`Policy.aspx`和`Policy.aspx.vb`檔案放在 Unchanged 狀態與本機和遠端網站。

列出新的檔案，以及複製網站工具會反白顯示不同的本機和遠端站台之間的任何檔案。 若要查看此動作，請返回`Privacy.aspx`頁面上，並新增更多的幾個字隱私權原則。 儲存頁面，然後返回複製網站工具。 如 [圖 5] 所示，`Privacy.aspx`的左窗格中的頁面具有的狀態為已變更，指出它與遠端站台不同步。


[![複製網站工具指出&lt;程式碼&gt;Privacy.aspx&lt;/c o d&gt;頁面已變更](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**圖 5**： 複製網站工具指出`Privacy.aspx`已變更頁面 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image15.png))


複製網站工具也會指出，是否自上次複製作業後刪除檔案的位置。 刪除`Privacy.aspx`從本機專案，然後重新整理複製網站工具。 `Privacy.aspx`和`Privacy.aspx.vb`檔案保留列在左窗格中，但已刪除狀態，指出他們已在上次複製作業之後已經移除。

## <a name="publishing-a-web-application"></a>發佈 Web 應用程式

若要部署 web 應用程式從 Visual Studio 內的另一種方式是使用 [發佈] 選項，可透過 [建置] 功能表存取。 [發佈] 選項明確地編譯應用程式，，然後將複製的所有必要檔案至指定的遠端站台。 如我們所見，[發佈] 選項會是更冒失比複製網站工具。 複製網站工具可讓您檢查本機和遠端網站上的檔案，並可讓您上傳或下載所需的個別檔案，而 [發佈] 選項會將整個 web 應用程式部署。


將所有必要的檔案複製到指定的遠端站台，除了 [發佈] 選項，明確地編譯應用程式。 假設 Web 應用程式專案必須明確地編譯應包括一點也不為 [發佈] 選項，僅適用於 Web 應用程式專案。 可能有點令人意外，為 [發佈] 選項也很適用於網站專案。 如中所述[*判斷哪些檔案需要部署*](determining-what-files-need-to-be-deployed-vb.md)教學課程中，網站專案可以透過稱為程序明確編譯*預先編譯*。 本教學課程著重於使用 Web Application Projects; 中的 [發佈] 選項未來的教學課程將探索預先編譯，此時我們會返回查看使用網站專案中的 [發佈] 選項。

> [!NOTE]
> I： 如何部署網站，使用複製網站工具（影片）


如何： 發行 Web 應用程式專案 如何： 發行網站 安裝和部署專案在 Visual Studio 中 這會顯示對話方塊提示輸入目標位置等等 （請參閱 圖 6） 的其他組態選項。 更像是複製網站工具與您可以輸入指向本機資料夾、 在 IIS 上的本機網站、 支援 FrontPage Server Extensions 或 FTP 伺服器位址的遠端網站的位置。 您可以選擇是否要部署的檔案中取代遠端 web 伺服器上的檔案或將刪除所有發行前的遠端站台上的內容。 您也可以指定是否要複製：

- 只有在專案中執行應用程式，會省略不必要的原始程式碼和專案相關的檔案所需檔案。
- 所有的專案檔，其中包含的原始程式碼檔和 Visual Studio 專案檔，例如方案檔。
- 在來源專案資料夾中，這會將所有檔案都複製來源專案資料夾，不論它們是否包含在專案中的所有檔案。

另外還有一個選項來上傳的內容`App_Data`資料夾。


[![指定目的地網站](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**圖 6**： 指定目的地網站 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image18.png))


書籍評論應用程式的遠端站台會包含部署時複製 BookReviewsWSP 專案透過複製網站工具的檔案。 因此，讓我們開始刪除所有現有的內容的 [發佈] 選項。 此外，我們只是複製必要的檔案，而不會佔用不必要的原始程式碼和專案檔使用的生產環境。 指定這些選項後，按一下 [發行] 按鈕。 在下一步 的數秒內 Visual Studio 會將部署必要的檔案到目的地網站中，在 輸出 視窗中顯示其進度。

發行作業完成後，圖 7 顯示 FTP 站台上的檔案。 請注意，只有標記頁面和必要伺服器端和用戶端的支援檔案已上傳。


[![只有所需的檔案發行到生產環境](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**圖 7**： 只需要檔案已發行至實際執行環境 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image21.png))


[發佈] 選項會是較不細微的工具，比複製網站工具。 複製網站工具，可讓您檢查本機和遠端網站上的檔案，並查看它們之間的差異，而 [發佈] 選項會提供任何這類介面。 此外，複製網站工具可讓您進行一次性的變更上, 傳或刪除個別的檔案。 [發佈] 選項不允許這類精細的控制;相反地，它會將發佈*整個*應用程式。 此行為有其優缺點。 加上一端，您會知道何時使用 [發佈] 選項不會忘了重要的檔案上傳。 但是，是否您已進行小幅變更至非常大型的網站-使用 [發佈] 選項您不能更新該頁面，或兩個已修改，但改為您必須等待 Visual Studio 會部署在整個站台，會發生什麼情況。

您不常見，才會實際執行環境和開發環境不同內容的特定檔案。 重要的例子是應用程式的組態檔， `Web.config`。 因為 [發佈] 選項會盲目地複製 web 應用程式檔案它覆寫生產環境的自訂的組態檔中的開發環境的版本。 後續的教學課程將探討這個主題會進一步，並提供部署 web 應用程式，這類差異存在時的秘訣。

## <a name="summary"></a>總結

部署網站時，需要將必要的檔案複製到生產環境的開發環境。 上一個教學課程示範了如何使用 FTP 用戶端，例如 FileZilla 傳送檔案。 本教學課程中檢查兩個 Visual Studio 中的部署工具： 複製網站工具，並 [發佈] 選項。 複製網站工具大致與 FTP 用戶端，它也具有雙窗格介面，列出本機電腦和指定的遠端電腦，可讓您更輕鬆地將上傳或下載檔案的兩部電腦上的檔案。 [發佈] 選項是一種更種笨拙的工具，明確地編譯專案，然後整個應用程式部署到指定的目的地。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [複製網站複製網站工具](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [I： 如何部署網站，使用複製網站工具](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md)（影片）
- [如何： 發行 Web 應用程式專案](https://msdn.microsoft.com/library/aa983453.aspx)
- [如何： 發行網站](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [安裝和部署專案在 Visual Studio 中](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-your-site-using-an-ftp-client-vb.md)
> [下一頁](common-configuration-differences-between-development-and-production-vb.md)
