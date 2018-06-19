---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: 部署您使用 Visual Studio (VB) 的網站 |Microsoft 文件
author: rick-anderson
description: Visual Studio 包含工具部署網站。 深入了解這些工具在本教學課程。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c0e094761a92b10555d2cfcef586ad8c2fb8d27
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888873"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>部署您的網站使用 Visual Studio (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio 包含工具部署網站。 深入了解這些工具在本教學課程。


## <a name="introduction"></a>簡介

前述教學課程探討了解如何部署簡單的 ASP.NET web 應用程式，到 web 主機提供者。 具體來說，本教學課程示範了如何使用像 FileZilla FTP 用戶端從開發環境的必要的檔案傳輸到生產環境。 Visual Studio 也提供內建的工具，以便部署到 web 主機提供者。 本教學課程會檢查這些工具的兩個： 複製網站工具，與使用 FTP 或 FrontPage Server Extensions; 遠端網頁伺服器，可以移動檔案和 [發行] 工具，將整個網站複製到指定的位置。


> [!NOTE]
> Visual Studio 所提供其他部署相關的工具包括[Web 安裝專案](https://msdn.microsoft.com/library/wx3b589t.aspx)和[Web 部署專案](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en)增益集。 Web 安裝專案封裝的網站內容與組態資訊成單一的 MSI 檔案。 此選項便會在內部部署的網站或公司販售客戶安裝在他們自己的網頁伺服器的預先封裝的 web 應用程式的最有用的。 Web 部署專案增益集是 Visual Studio 增益集，可促進差異指定組態建置開發環境和生產環境。 本教學課程系列; 將不會討論 web 安裝專案Web 部署專案會摘要在[*通用組態差異之間開發和生產*](common-configuration-differences-between-development-and-production-vb.md)教學課程。


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>部署您的網站使用複製網站工具

Visual Studio 複製網站工具是獨立的 FTP 用戶端類似的功能。 簡而言之，複製網站工具可讓您連線到遠端的網站，透過 FTP 或 FrontPage Server Extensions。 類似於 FileZilla 的使用者介面，複製網站使用者介面包含兩個窗格： 左的窗格會列出本機檔案，而右窗格會列出目的地伺服器上的這些檔案。

> [!NOTE]
> 複製網站工具僅適用於網站專案。 當您使用 Web 應用程式專案與 visual Studio 的確有提供這項工具。


讓我們看看書籍檢閱應用程式發行至生產環境使用複製網站工具。 複製網站工具只能搭配使用的網站專案模型專案，因為我們只可以檢查與 BookReviewsWSP 專案中使用此工具。 開啟該專案。

在 方案總管 （圖 1 中所會圈出此圖示）; 複製網站圖示，即可啟動複製網站工具專案或者，您也可以從 網站 功能表中選取複製網站選項。 兩種方法啟動圖 1; 複製網站使用者介面因為我們尚未連線到遠端伺服器，就會擴展的左的窗格中圖 1。


[![複製網站工具使用者介面是分割成兩個窗格](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**圖 1**： 複製網站工具的使用者介面是分割成兩個窗格 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image3.png))


若要部署 「 我們需要先連線到 web 主機提供者的網站。 按一下 [連接] 按鈕複製網站使用者介面的頂端。 這會顯示開啟網站對話方塊中，在圖 2 所示。

您可以連接至目的地網站，從左側選取四個選項的其中一個：

- **檔案系統**-選取此選項可從您的電腦，將您的網站部署到可存取的資料夾或網路共用。
- **本機 IIS** -使用此選項可將網站部署至電腦上安裝 IIS web 伺服器。
- **FTP 站台**-連線到遠端網站上，使用 FTP。
- **遠端站台**-連線到遠端網站，使用 FrontPage Server Extensions。

大部分 web 主機提供者支援 FTP，但更少提供的 FrontPage Server Extension 支援。 因此，我選擇 FTP 站台選項並在圖 2 所示，然後輸入連接資訊。


[![指定目的地網站](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**圖 2**： 指定目的地網站 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image6.png))


您連接之後，複製網站工具載入在右窗格中的遠端站台的檔案，並指出每個檔案的狀態： 新增、 刪除、 已變更或未變更。 您可以從本機站台檔案複製到遠端站台或相反的。

讓我們加入一個新頁面，即可 BookReviewsWSP 專案，然後加以部署，我們可以看到在動作中的複製網站工具。 Visual Studio 中建立新的 ASP.NET 網頁，名為的根目錄中`Privacy.aspx`。 已使用主版頁面的頁面`Site.master`並將站台的隱私權原則加入至這個頁面。 圖 3 顯示 Visual Studio，建立此頁面之後。


[![加入新的頁面名稱為&lt;程式碼&gt;Privacy.aspx&lt;/c o&gt;到網站的根資料夾](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**圖 3**： 加入新的頁面名稱為`Privacy.aspx`到網站的根資料夾 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image9.png))


接下來，返回複製網站使用者介面。 如圖 4 所示，左的窗格現在會包含新的檔案-`Policy.aspx`和`Policy.aspx.vb`。 除此之外，這些檔案會以箭號圖示及標示狀態的新密碼，指出存在本機站台上，但不是能在遠端站台。


[![複製網站工具包含 新增&lt;程式碼&gt;Privacy.aspx&lt;/c o&gt;其左窗格中的頁面](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**圖 4**： 複製網站工具包含 新增`Privacy.aspx`其左窗格中的頁面 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image12.png))


若要部署新的檔案選取它們，然後按一下 將它們傳送到遠端站台的箭號圖示。 傳輸完成後`Policy.aspx`和`Policy.aspx.vb`狀態未變更本機和遠端網站上的檔案存在。

列出新的檔案，以及複製網站工具會反白顯示不同的本機和遠端站台之間的任何檔案。 若要查看此作業如何，傳回到`Privacy.aspx`頁面上，並將幾個的多個字加入至隱私權原則。 儲存網頁，然後返回複製網站工具。 如圖 5 所示，`Privacy.aspx`頁面的左窗格中具有的狀態已變更，指出它與遠端站台同步處理。


[![複製網站工具表示&lt;程式碼&gt;Privacy.aspx&lt;/c o&gt;頁面已變更](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**圖 5**： 複製網站工具表示`Privacy.aspx`頁面已變更 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image15.png))


複製網站工具也會指出，是否檔案已刪除最後一個複製作業之後的位置。 刪除`Privacy.aspx`從本機專案和重新整理複製網站工具。 `Privacy.aspx`和`Privacy.aspx.vb`檔案仍列出在左窗格中，但已刪除狀態，表示它們已在上次複製作業之後已經移除。

## <a name="publishing-a-web-application"></a>發行 Web 應用程式

部署 web 應用程式從 Visual Studio 中的另一種方式是使用 [發佈] 選項，可透過 [建置] 功能表存取。 [發佈] 選項明確地編譯的應用程式，，然後將複製的所有必要的檔案到指定的遠端站台。 我們會看到，[發佈] 選項會比複製網站工具更遲鈍。 複製網站工具可讓您檢查本機和遠端網站上的檔案，並允許您將上傳或下載個別的檔案，視需要而 [發佈] 選項會將整個 web 應用程式部署。


將所有必要的檔案複製到指定的遠端站台，除了 [發佈] 選項也會明確編譯應用程式。 假設 Web 應用程式專案需要明確編譯應該就不會對 [發佈] 選項僅適用於 Web 應用程式專案。 可能有點令人意外是 [發佈] 選項也會提供適用於網站專案。 如中所述[*決定需要的檔案部署*](determining-what-files-need-to-be-deployed-vb.md)教學課程中，網站專案可以透過稱為程序明確編譯*先行編譯*。 本教學課程著重於使用 [發佈] 選項與 Web 應用程式專案。未來的教學課程將探索先行編譯，此時，我們將傳回查看網站專案中使用 [發佈] 選項。

> [!NOTE]
> 網站專案和 Web 應用程式專案的 Visual Studio 中提供 [發佈] 選項時，Visual Web Developer 只提供 Web 應用程式專案的 [發佈] 選項。


讓我們看看部署活頁簿檢閱應用程式使用 [發佈] 選項。 啟動 Visual Studio 中開啟 BookReviewsWAP （Web 應用程式專案）。 從 發行 功能表選擇 建置 BookReviewsWAP 專案。 這會顯示對話方塊提示輸入目標位置，（請參閱圖 6） 其他組態選項。 更像是複製網站工具與您可以輸入本機資料夾、 本機 IIS 網站、 支援 FrontPage Server Extensions 或 FTP 伺服器位址的遠端網站會指向的位置。 您可以選擇是否已部署的檔案中取代遠端 web 伺服器上的檔案，或刪除所有的發行前位於遠端站台內容。 您也可以指定是否要複製：

- 只有執行應用程式，會省略不必要的程式碼與專案相關的檔案所需的專案中檔案。
- 所有專案檔，其中包括原始程式碼檔和 Visual Studio 專案檔，都例如方案檔。
- 來源專案資料夾，不論它們是否包含在專案中的來源專案資料夾中複製所有檔案中的所有檔案。

另外還有一個選項來上傳的內容`App_Data`資料夾。


[![指定目的地網站](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**圖 6**： 指定目的地網站 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image18.png))


活頁簿檢閱應用程式的遠端站台包含複製 BookReviewsWSP 專案透過複製網站工具時部署檔案。 因此，我們必須先刪除所有現有內容的 [發佈] 選項。 此外，我們只複製必要的檔案，而不是使用不需要的原始程式碼和專案檔想堆在實際執行環境。 指定這些選項後，按一下 [發行] 按鈕。 接下來的幾秒透過 Visual Studio 會將部署必要的檔案到目的地網站中，在 [輸出] 視窗中顯示其進度。

發行作業完成之後，圖 7 顯示 FTP 站台上的檔案。 請注意，只有標記頁面和需要伺服器端和用戶端的支援檔案已上傳。


[![只有所需的檔案發行到生產環境](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**圖 7**： 只需要檔案發行到實際執行環境 ([按一下以檢視完整大小的影像](deploying-your-site-using-visual-studio-vb/_static/image21.png))


[發佈] 選項是比複製網站工具較不 nuanced 的工具。 複製網站工具可讓您檢查本機和遠端網站上的檔案，並查看有何不同，而 [發佈] 選項會提供任何這類介面。 此外，複製網站工具可讓您進行一次性變更上傳或刪除個別檔案。 [發佈] 選項不允許這類更細微的控制。相反地，其所發行*整個*應用程式。 這種行為有其優缺點。 在加上端中，您知道使用 [發佈] 選項不會忘了重要的檔案上傳時。 但是，是否小幅變更對非常大型的網站的 [發佈] 選項無法更新該頁面或兩個已修改，但改為必須等候時，Visual Studio 部署在整個站台，會發生什麼情況。

是很常見，才會實際執行環境和開發環境不同內容的某些檔案的位置。 索引鍵的範例是應用程式的組態檔， `Web.config`。 [發佈] 選項盲目 web 應用程式會將檔案複製，因為它以覆寫實際執行環境的自訂的設定檔開發環境中的版本。 後續的教學課程-本主題將進一步探索，並提供部署 web 應用程式，這類差異存在時的秘訣。

## <a name="summary"></a>總結

部署網站時，需要從開發環境的必要檔案複製到實際執行環境。 上一個教學課程示範了如何使用像 FileZilla FTP 用戶端傳送檔案。 本教學課程中檢查兩個 Visual Studio 中的部署工具： 複製網站工具和 [發佈] 選項。 複製網站工具是類似於 FTP 用戶端有兩個 paned 介面，列出本機電腦和指定的遠端電腦，以簡化上傳或下載檔案，將兩台電腦上的檔案。 [發佈] 選項是更遲鈍工具明確編譯專案，然後再部署到指定的目的地整個應用程式。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [複製網站複製網站工具](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [I： 如何部署網站使用複製網站工具](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md)（影片）
- [如何： 發行 Web 應用程式專案](https://msdn.microsoft.com/library/aa983453.aspx)
- [如何： 發行網站](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [安裝和部署專案，Visual Studio 中](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-your-site-using-an-ftp-client-vb.md)
> [下一頁](common-configuration-differences-between-development-and-production-vb.md)
