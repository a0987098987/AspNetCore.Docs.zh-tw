---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: 部署您的網站使用 FTP 用戶端 (C#) |Microsoft Docs
author: rick-anderson
description: 部署 ASP.NET 應用程式的最簡單方式是以手動方式從開發環境的必要檔案複製到實際執行環境。 這個...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 50e1522a39cb7b8ea2ee4c664f166b45f22ff070
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842279"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>部署您的網站使用 FTP 用戶端 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> 部署 ASP.NET 應用程式的最簡單方式是以手動方式從開發環境的必要檔案複製到實際執行環境。 本教學課程會示範如何使用 FTP 用戶端將檔案從桌面到 web 主機提供者。


## <a name="introduction"></a>簡介

上一個教學課程導入簡單書籍檢閱 ASP.NET web 應用程式，它包含的少數幾個 ASP.NET 網頁、 主版頁面、 自訂的基底`Page`類別，就會有一些映像，以及三個的 CSS 樣式表。 我們現在已準備好部署到 web 主機提供者，此時應用程式會使用連線到網際網路的任何人存取此應用程式 ！


從我們的 「 討論區[*判斷哪些檔案需要部署*](determining-what-files-need-to-be-deployed-cs.md)教學課程中，我們知道檔案需要複製到 web 主機提供者。 （重新叫用會複製哪些檔案取決於是否您編譯應用程式明確或自動）。但是，我們該如何將檔案從開發環境 （桌面） 到生產環境 （web 主機提供者所管理的 web 伺服器）？ [ **F** ile **T** ransfer **P**協定 (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)是常用的通訊協定，將檔案從一部電腦複製到另一個，在網路上。 另一個選項是 FrontPage Server Extensions (FPSE)。 本教學課程著重於使用獨立 FTP 用戶端軟體部署到生產環境必要檔案從開發環境。

> [!NOTE]
> Visual Studio 包含適用於透過 FTP; 的發行網站工具這些工具，以及了解使用 FPSE，工具會在下一個教學課程中說明。


使用的 FTP，我們需要將檔案複製*FTP 用戶端*在開發環境。 FTP 用戶端是應用程式是設計用來將檔案從安裝到電腦正在執行的電腦複製*FTP 伺服器*。 （如果您的 web 主機提供者支援透過 FTP，檔案傳輸，大部分一樣，則其 web 伺服器上執行的 FTP 伺服器。）沒有可用的 FTP 用戶端應用程式的數目。 您的 web 瀏覽器可以做為 FTP 用戶端甚至兩倍。 我最喜愛的 FTP 用戶端，也就是，我將使用此教學課程中，是[FileZilla](http://filezilla-project.org/)，適用於 Windows、 Linux 和 Mac 的免費、 開放原始碼 FTP 用戶端。 任何 FTP 用戶端運作，不過，因此自由使用任何用戶端您最熟悉。

如果您已遵循沿著您將需要與 web 主機提供者之前建立帳戶才能完成本教學課程或後續的。 如先前的教學課程中所述，有廣泛的價格、 功能和服務品質與 1gb 的 web 主機提供者的公司。 本教學課程系列我將使用[折扣 ASP.NET](http://discountasp.net)為我的 web 主機提供者，但您可以依照任何 web 主機提供者，只要它們支援您的網站開發中的 ASP.NET 版本。 （這些教學課程被建立使用 ASP.NET 3.5。）此外，因為我們將會將檔案複製到 web 主機提供者使用 FTP，在本教學課程，在未來的務必您的 web 主機提供者支援其 web 伺服器的 FTP 存取。 幾乎所有的 web 主機提供者會提供這項功能，但您應該註冊前先仔細查看。

## <a name="deploying-the-book-review-web-application-project"></a>部署活頁簿檢閱 Web 應用程式專案

您應該記得，有兩個版本的書籍評論 web 應用程式： 一個使用 Web 應用程式專案模型 (BookReviewsWAP) 和其他使用網站專案模型 (BookReviewsWSP) 實作。 專案類型會影響是否自動或明確地編譯的站台和該編譯模型可讓您指定要部署需要的檔案。 因此，我們將檢視個別部署的 BookReviewsWAP 和 BookReviewsWSP 專案開始 BookReviewsWAP。 請花一點時間下載這些兩個 ASP.NET 應用程式，如果您有不這麼做。

瀏覽至啟動 BookReviewsWAP 專案`BookReviewsWAP`資料夾，然後按兩下`BookReviewsWAP.sln`檔案。 部署專案之前請務必建置它以確保原始程式碼的任何變更都包含在已編譯的組件。 若要建置的專案移至 [建置] 功能表，然後選擇 [建置 BookReviewsWAP] 功能表選項。 這會將專案中的原始程式碼編譯成單一組件中， `BookReviewsWAP.dll`，其會置於`Bin`資料夾。

現在，我們已準備好部署必要的檔案 ！ 啟動您的 FTP 用戶端並連接到您的 web 主機提供者的 web 伺服器。 （當您使用哪家虛擬主機公司註冊它們會透過電子郵件傳送如何連接到 FTP 伺服器的詳細資訊，包括 FTP 伺服器，以及使用者名稱和密碼的位址）。

從您的桌面的下列檔案複製到在您的 web 主機服務提供者的根網站資料夾中。 當您進入 web 伺服器的 FTP 在 web 主控提供者可能會在網站根目錄。 不過，有些 web 主機提供者都有名稱為的子`www`或`wwwroot`做為您的網站檔案的根資料夾。 最後，當 FTPing 檔案時您可能需要在實際執行環境-建立對應的資料夾結構`Bin`資料夾中，`Fiction`資料夾，`Images`資料夾，並依此類推。

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- 完整內容`Styles`資料夾
- 完整內容`Images`資料夾 (及其子資料夾， `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

[圖 1] 顯示 FileZilla 之後已複製必要的檔案。 FileZilla 會顯示本機電腦上的檔案，在左邊和右邊的遠端電腦上的檔案。 如 圖 1 所示，ASP.NET 原始程式碼檔案，例如`About.aspx.cs`，位於本機電腦 （開發環境），但未複製到 web 主機提供者 （生產環境） 因為程式碼檔案不需要使用時，部署明確的編譯。

> [!NOTE]
> 擁有來源的程式碼檔案，在實際執行伺服器上，沒有壞處，因為它們會被忽略。 ASP.NET 預設禁止原始程式碼檔的 HTTP 要求，且即使實際執行伺服器上的原始程式碼檔案有存取網站的訪客。 (亦即，如果使用者嘗試瀏覽`http://www.yoursite.com/Default.aspx.cs`它們會出現，表示錯誤頁面，這些類型的檔案-`.cs`檔案-禁止。)


[![若要從您的桌面的必要檔案複製到 Web 伺服器，在 Web 主機服務提供者使用 FTP 用戶端](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**圖 1**： 若要從您的桌面的必要的檔案複製到 Web 伺服器，在 Web 主機服務提供者使用 FTP 用戶端 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


部署您的網站後請花一點時間測試的網站。 如果您已購買的網域名稱，並設定 DNS 設定正確，您可以造訪您的網站，輸入您的網域名稱。 或者，您的 web 主機提供者應該提供您的 url，您的網站，這看起來就像*accountname*。*webhostprovider*.com 或*webhostprovider*.com /*accountname*。 比方說，我折扣 asp.net 帳戶的 URL 是： `http://httpruntime.web703.discountasp.net`。

[圖 2] 顯示已部署的書籍評論站台。 請注意我正在折扣 ASP 上檢視它。NET 的伺服器在`http://httpruntime.web703.discountasp.net`。 在此時間點的網際網路連線的任何人都可以檢視我的網站 ！ 如我們所預期，站台的外觀與行為就像在開發環境中測試它時一樣。

> [!NOTE]
> 如果您收到錯誤時檢視您的應用程式花點時間，請確認您已部署一組正確的檔案。 接下來，請檢查錯誤訊息，以查看它會顯示任何有關此問題的線索。 接下來，您可以向 web 主機公司的技術服務人員，或在適當的論壇上張貼您的問題[ASP.NET 論壇](https://forums.asp.net/)。


[![書籍評論網站是現在可以存取任何有網際網路連線](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**圖 2**： 透過網際網路連線的任何人是現在可存取的書籍評論網站 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>部署活頁簿檢閱網站專案

部署使用 BookReviewsWSP 網站專案中，例如自動編譯的 ASP.NET 應用程式時沒有任何編譯的組件中`Bin`資料夾。 如此一來，web 應用程式的原始程式碼檔必須部署到生產環境。 讓我們逐步解說這個程序。

與 Web 應用程式專案是個明智的選擇，以第一次建置應用程式，再將它部署。 而建置的網站專案時，不會建立組件，它會檢查有任何網頁中的編譯時期錯誤。 更好的立即找出這些錯誤，而不是讓您的網站造訪者為您探索 ！

一旦您已成功建置專案，請將下列檔案複製到在您的 web 主機服務提供者的根網站資料夾使用您的 FTP 用戶端。 您可能需要在實際執行環境中建立對應的資料夾結構。

> [!NOTE]
> 如果您已部署專案，但是仍然想要嘗試部署 BookReviewsWSP 專案的 BookReviewsWAP，先刪除所有網頁伺服器上部署 BookReviewsWAP 時已上傳的檔案，並接著部署 BookReviewsWSP 的檔案。


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- 完整內容`Styles`資料夾
- 完整內容`Images`資料夾 (及其子資料夾， `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

圖 3 顯示 FileZilla 之後複製所需的檔案。 如您所見，ASP.NET 來源的程式碼檔案，例如`About.aspx.cs`，會出現在本機電腦 （開發環境） 和 web 主機提供者 （生產環境），因為程式碼檔案需要時使用自動部署編譯。


[![若要從您的桌面的必要檔案複製到 Web 伺服器，在 Web 主機服務提供者使用 FTP 用戶端](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**圖 3**： 若要從您的桌面的必要的檔案複製到 Web 伺服器，在 Web 主機服務提供者使用 FTP 用戶端 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


使用者體驗不會受到應用程式的編譯模型。 相同的 ASP.NET 頁面都可以存取和它們的外觀和是否已建立網站，使用 Web 應用程式專案模型或網站專案模型的行為相同。

### <a name="updating-a-web-application-on-production"></a>更新生產環境 Web 應用程式

Web 應用程式開發和部署不是一次性的程序。 例如，建立書籍評論網站時我建置在各種頁面並在我的個人電腦 （開發環境） 撰寫隨附的程式碼。 在達到特定穩定的狀態之後, 我部署我的應用程式，以便其他人無法瀏覽網站並閱讀我的評論。 但部署沒有標記的結尾我在此站台上進行開發。 我可能會新增更多的書籍評論或實作新功能，例如允許速率叢書我訪客或保留他們自己的註解。 這類的增強功能會在開發環境上開發，而且完成時，則需要部署。 開發及部署，因此，會循環。 您開發應用程式，然後再部署它。 即時站台時和在生產環境中，加入新功能，並已修正 bug，經過一段時間，因此需要重新部署應用程式。 等等，依此類推。

如您所料，重新部署您只需要將新的和變更的檔案複製的 web 應用程式時。 不需要重新部署未變更的頁面或伺服器或用戶端支援的檔案 （雖然沒有壞處，在此情況下）。

> [!NOTE]
> 請記住，當使用明確的編譯時，每當您將新的 ASP.NET 網頁新增至專案，或進行任何程式碼相關的變更，需要重新建置您的專案，更新的組件中的一件事`Bin`資料夾。 因此，您必須更新 （以及其他新的和更新內容） 的生產 web 應用程式時，此更新的組件複製到生產環境。


也了解任何變為`Web.config`中的檔案或`Bin`目錄會停止並重新啟動網站的應用程式集區。 如果您的工作階段狀態儲存使用`InProc`模式 （預設值），則您的網站訪客會失去其工作階段狀態時修改這些金鑰的檔案。 若要避免這個錯誤，請考慮將儲存工作階段使用`StateServer`或`SQLServer`模式。 如需本主題的詳細資訊，請參閱[工作階段狀態模式](https://msdn.microsoft.com/library/ms178586.aspx)。

最後，請記住，重新部署應用程式可能會花費幾秒鐘的時間從數分鐘的時間，視需要複製到實際執行環境之檔案的大小與數量而定。 在這段期間瀏覽您的網站的使用者可能會遇到錯誤或奇怪的行為。 您可以 「 關閉 」 整個應用程式藉由新增名為的頁面`App_Offline.htm`給您的使用者說明您的應用程式根目錄的站台維護 （或任何） 已關閉，並將會備份短時間內。 當`App_Offline.htm`檔案存在，則 ASP.NET 執行階段將所有連入要求重新導向至該頁面。

## <a name="summary"></a>總結

部署 web 應用程式需要必要的檔案複製到生產環境的開發環境。 最常見的方法，檔案透過網路傳輸檔案傳輸通訊協定 (FTP)，而且大部分的 web 主機提供者支援其 web 伺服器的 FTP 存取。 在本教學課程中，我們看到如何使用 FTP 用戶端部署至 web 伺服器的所需的檔案。 一旦部署之後，網站可瀏覽的任何人都具有連線到網際網路 ！

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [應用程式\_Offline.htm 和解決 「 IE 易記錯誤 」 功能](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [工作階段狀態模式](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [上一頁](determining-what-files-need-to-be-deployed-cs.md)
> [下一頁](deploying-your-site-using-visual-studio-cs.md)
