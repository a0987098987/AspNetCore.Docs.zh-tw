---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: "部署您使用 FTP 用戶端 (C#) 的網站 |Microsoft 文件"
author: rick-anderson
description: "部署 ASP.NET 應用程式的最簡單方式是手動從開發環境的必要檔案複製到生產環境。 此..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c53dcf40cde244a9df9afc27b20c9e7ef288198
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>部署您的網站使用 FTP 用戶端 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> 部署 ASP.NET 應用程式的最簡單方式是手動從開發環境的必要檔案複製到生產環境。 本教學課程會示範如何將檔案從您的桌面，到 web 主機提供者使用 FTP 用戶端。


## <a name="introduction"></a>簡介

上一個教學課程導入簡單書籍檢閱 ASP.NET web 應用程式，這組成少數幾個 ASP.NET 網頁、 主版頁面、 自訂的基底`Page`類別，就會有一些映像，以及三個 CSS 樣式表。 我們現在已準備好部署到 web 主機提供者，此時應用程式將人皆可存取網際網路的連線與此應用程式 ！


從我們討論區[*決定需要的檔案部署*](determining-what-files-need-to-be-deployed-cs.md)教學課程，我們知道檔案必須複製到 web 主機提供者。 （重新叫用會複製哪些檔案取決於是否您編譯應用程式明確或自動）。但我們該如何將檔案從開發環境 （我們的桌面） 至實際執行環境 （web 主機提供者所管理的 web 伺服器） 嗎？ [ **F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)是常用的通訊協定，將檔案從一部電腦複製到另一個，在網路上。 另一個選項是 FrontPage Server 擴充功能 (FPSE)。 本教學課程著重在使用獨立的 FTP 用戶端軟體部署到生產環境必要檔案從開發環境。

> [!NOTE]
> Visual Studio 包含適用於透過 FTP; 發行網站工具這些工具，以及查看使用 FPSE，工具的下一個教學課程所述。


使用的 FTP，我們需要將檔案複製*FTP 用戶端*在開發環境中。 FTP 用戶端是應用程式是設計用來將檔案從安裝到電腦正在執行的電腦複製*FTP 伺服器*。 （如果您的 web 主機提供者支援透過 FTP，檔案傳輸，大部分一樣，就讓 web 伺服器上執行的 FTP 伺服器。）沒有可用的 FTP 用戶端應用程式的數字。 網頁瀏覽器可以做為 FTP 用戶端即使 double。 我最喜愛的 FTP 用戶端，也就是，我將使用此教學課程中，是[FileZilla](http://filezilla-project.org/)，適用於 Windows、 Linux 及 Mac 免費的開放原始碼 FTP 用戶端。 任何 FTP 用戶端運作，不過，因此可自由使用任何用戶端您熟悉最。

如果您是按照您將需要與 web 主機提供者才能建立帳戶才能完成本教學課程或後續的。 如同上一個教學課程中所述，有 1gb 的 web 主機提供者公司具有廣泛的價格、 功能和服務品質。 此教學課程系列我將使用[折扣 ASP.NET](http://discountasp.net)為我的 web 主機提供者，但您可以依照任何 web 主機提供者，只要它們支援開發您的網站中的 ASP.NET 版本。 （這些教學課程所建立使用 ASP.NET 3.5）。此外，因為我們將會將檔案複製到 web 主機提供者使用 FTP 在本教學課程和未來的務必確定您的 web 主機提供者支援其 web 伺服器的 FTP 存取。 幾乎所有的 web 主機提供者會提供這項功能，但您應該再次檢查之前註冊。

## <a name="deploying-the-book-review-web-application-project"></a>部署活頁簿檢閱 Web 應用程式專案

前文提過，有兩個版本的活頁簿檢閱 web 應用程式： 其中一個 Web 應用程式專案模型 (BookReviewsWAP) 和其他使用的網站專案模型 (BookReviewsWSP) 進行實作。 專案類型會影響是否自動或明確地編譯站台以及該編譯模型決定需要部署的檔案。 因此，我們將檢驗分別部署 BookReviewsWAP 和 BookReviewsWSP 專案開頭 BookReviewsWAP。 請花一點時間下載這些兩個 ASP.NET 應用程式，如果您有尚未完成操作。

瀏覽至啟動 BookReviewsWAP 專案`BookReviewsWAP`資料夾，然後按兩下`BookReviewsWAP.sln`檔案。 部署專案之前是建置，請確定已編譯的組件中包含原始程式碼的任何變更。 若要建置專案，請移至 建置 功能表並選擇 建置 BookReviewsWAP 功能表選項。 這會將來源中的程式碼專案編譯成單一組件， `BookReviewsWAP.dll`，這會置於`Bin`資料夾。

我們現在已準備好部署必要的檔案 ！ 啟動您的 FTP 用戶端並連接到您的 web 主機提供者的 web 伺服器。 （當您使用虛擬主機公司註冊它們會以電子郵件傳送您如何連接到 FTP 伺服器的詳細資訊，這包含 FTP 伺服器，以及使用者名稱和密碼的位址）。

從您的桌面的下列檔案複製到您的 web 主機提供者上的根網站資料夾中。 當您進入 web 伺服器的 FTP 在 web 裝載提供者時，可能會在網站的根目錄。 不過，有些 web 主機提供者都有名稱為的子`www`或`wwwroot`做為您的網站檔案的根資料夾。 最後，當 FTPing 檔案時您可能需要在實際執行環境-建立對應的資料夾結構`Bin`資料夾，`Fiction`資料夾，`Images`資料夾中，依此類推。

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- 完整內容`Styles`資料夾
- 完整內容`Images`資料夾 (和其子資料夾， `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

圖 1 顯示 FileZilla 之後已複製的必要檔案。 FileZilla 顯示左邊和右邊的遠端電腦上的檔案上的本機電腦上的檔案。 如圖 1 所示，ASP.NET 原始程式碼檔案，例如`About.aspx.cs`，位於本機電腦 （開發環境），但未複製到 web 主機提供者 （生產環境） 因為不需要使用時即將部署的程式碼檔案明確編譯。

> [!NOTE]
> 具有原始程式碼檔案的實際執行伺服器上，沒有壞處，因為它們會被忽略。 ASP.NET 會根據預設禁止 HTTP 要求的原始程式碼檔，讓即使在實際執行伺服器上的原始程式碼檔案有它們無法存取您的網站訪客。 (亦即，如果使用者嘗試瀏覽`http://www.yoursite.com/Default.aspx.cs`它們會得到錯誤頁面的說明，這些類型的檔案-`.cs`檔案-禁止。)


[![若要從您的桌面的必要檔案複製到 Web 主機提供者的 Web 伺服器使用 FTP 用戶端](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**圖 1**： 若要從您的桌面的必要檔案複製到 Web 主機提供者的 Web 伺服器使用 FTP 用戶端 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


部署您的站台後請花一點時間測試的站台。 如果您已購買了網域名稱，並設定 DNS 設定正常運作，您可以造訪您的網站，輸入您的網域名稱。 或者，您的 web 主機提供者應該提供您網站的 url 這看起來像*accountname*。*webhostprovider*.com 或*webhostprovider*.com /*accountname*。 比方說，我折扣的 asp.net 帳戶的 URL 是： `http://httpruntime.web703.discountasp.net`。

圖 2 顯示已部署的書籍檢閱站台。 請注意，檢視上折扣 ASP。網路的伺服器在`http://httpruntime.web703.discountasp.net`。 在此時間點的網際網路連線的任何人都可以檢視我的網站 ！ 如我們所預期，站台會看起來，並且是完全一樣的測試在開發環境中時的行為。

> [!NOTE]
> 如果您收到錯誤時檢視您的應用程式花一點時間，確定您部署了一組正確的檔案。 接下來，檢查錯誤訊息，以查看它會顯示任何有關此問題的線索。 接下來，您可以開啟 web 主機公司的技術服務人員，或將問題張貼到適當的論壇， [ASP.NET 論壇](https://forums.asp.net/)。


[![活頁簿檢閱站台是現在可以存取的人有網際網路連線](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**圖 2**： 書籍檢閱站台是現在可以存取網際網路連線的任何人 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>部署活頁簿檢閱網站專案

使用自動編譯，例如 BookReviewsWSP 網站專案，ASP.NET 應用程式部署時沒有任何編譯的組件中`Bin`資料夾。 如此一來，web 應用程式的原始程式碼檔必須部署到生產環境。 讓我們一步步完成此程序。

為 Web 應用程式專案與最好將第一次建置後再進行部署的應用程式。 而建置的網站專案不會建立組件，它並檢查有任何網頁中的編譯時間錯誤。 更好的立即找出這些錯誤，而不需要您的網站訪客為您探索 ！

一旦您已成功建置專案時，可用於您的 FTP 用戶端將下列檔案複製到您的 web 主機提供者上的根網站資料夾。 您可能需要在實際執行環境中建立對應的資料夾結構。

> [!NOTE]
> 如果您已部署專案，但是仍然想要再試一次部署 BookReviewsWSP 專案的 BookReviewsWAP，先刪除所有的網頁伺服器上部署 BookReviewsWAP 時已上傳的檔案，然後將檔案部署為 BookReviewsWSP。


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- 完整內容`Styles`資料夾
- 完整內容`Images`資料夾 (和其子資料夾， `BookCovers`)
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

圖 3 顯示 FileZilla 之後複製的必要檔案。 如您所見，ASP.NET 來源的程式碼檔案，例如`About.aspx.cs`，因為需要時使用自動部署的程式碼檔案都存在於本機電腦 （開發環境） 和 web 主機提供者 （生產環境）編譯。


[![若要從您的桌面的必要檔案複製到 Web 主機提供者的 Web 伺服器使用 FTP 用戶端](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**圖 3**： 若要從您的桌面的必要檔案複製到 Web 主機提供者的 Web 伺服器使用 FTP 用戶端 ([按一下以檢視完整大小的影像](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


使用者體驗不受應用程式的編譯模型。 相同的 ASP.NET 頁面都可以存取，並尋找及是否已建立網站，使用 Web 應用程式專案模型或為網站專案模型的行為相同。

### <a name="updating-a-web-application-on-production"></a>正在更新生產環境 Web 應用程式

Web 應用程式開發和部署不是一次性程序。 例如，建立活頁簿檢閱網站時我建置各種頁面並所隨附的程式碼撰寫我的個人電腦 （開發環境）。 在達到特定穩定的狀態之後, 我部署我的應用程式，以便其他人無法瀏覽的網站及閱讀我評論。 但是，部署不會將標示我此站台上開發的結尾。 可能新增多個活頁簿檢閱或實作新功能，例如允許我訪客速率書籍或讓他們自己的註解。 這類的增強功能會在開發環境中開發，並完成時，會想要部署。 開發和部署，因此，會循環。 您開發的應用程式，然後再部署它。 即時網站時，在生產環境中，加入新功能和 bug 固定的一段時間，因此需要重新部署應用程式。 等等，依此類推。

如您所預期，重新部署 web 應用程式，您只需要複製新和已變更的檔案時。 不需要重新部署未變更的頁面或伺服器端或用戶端的支援檔案 （雖然不會在此情況下）。

> [!NOTE]
> 使用明確編譯為，每當您將新的 ASP.NET 頁面加入至專案，或讓任何程式碼相關的變更，需要重新建置您的專案，就會更新中的組件時，請記住一件事`Bin`資料夾。 因此，您必須更新 （以及其他新的和更新內容） 的生產環境 web 應用程式時，此更新的組件複製到生產環境。


也必須了解任何變更為`Web.config`中的檔案或`Bin`目錄停止並重新啟動網站的應用程式集區。 如果您的工作階段狀態會儲存使用`InProc`模式 （預設值），則您的網站訪客將會遺失其工作階段狀態時修改這些金鑰檔案。 若要避免這個錯誤，請考慮將儲存工作階段使用`StateServer`或`SQLServer`模式。 如需有關本主題讀取[工作階段狀態模式](https://msdn.microsoft.com/library/ms178586.aspx)。

最後，請記住，重新部署應用程式可能需要幾秒鐘到幾分鐘的時間，檔案必須複製到生產環境的大小和數量。 在這段期間瀏覽您的站台的使用者可能會遇到錯誤或異常行為。 您可以 「 關閉 」 整個應用程式藉由新增名為的頁面`App_Offline.htm`至您的應用程式到您的使用者說明根目錄到站台維護 （或任何） 已關閉，且將會備份很快。 當`App_Offline.htm`檔案是否存在、 ASP.NET 執行階段將所有連入要求重新導向至該頁面。

## <a name="summary"></a>總結

Web 應用程式部署需要從開發環境的必要檔案複製到實際執行環境。 最常見的方法檔案透過網路傳輸檔案傳輸通訊協定 (FTP)，且大部分的 web 主機提供者支援其 web 伺服器的 FTP 存取。 在本教學課程中，我們看到如何使用 FTP 用戶端部署至 web 伺服器的所需的檔案。 一旦部署之後，網站可造訪的任何人都具有連線到網際網路 ！

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [應用程式\_Offline.htm 和解決 「 IE 易記錯誤 」 功能](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [工作階段狀態模式](https://msdn.microsoft.com/library/ms178586.aspx)

>[!div class="step-by-step"]
[上一頁](determining-what-files-need-to-be-deployed-cs.md)
[下一頁](deploying-your-site-using-visual-studio-cs.md)
