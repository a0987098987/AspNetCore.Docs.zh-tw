---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 簡介 ASP.NET Web Pages-使用 WebMatrix 發佈網站 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會介紹 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程系列中的最後一篇。 它討論如何發佈您的網站 t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: fe196e5db8fd1cecbe84b2eb970939303f9313d1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021452"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>ASP.NET Web Pages 簡介-使用 WebMatrix 發佈網站
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會介紹 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程系列中的最後一篇。 它討論如何在您的站台發佈至網際網路，以便其他人可以使用它。 它假設您已完成透過數列[建立 ASP.NET Web Pages 站台中的一致查詢](https://go.microsoft.com/fwlink/?LinkId=251585)。
> 
> 您將了解如何發佈站台使用：
> 
> - Microsoft Azure
> - Web 裝載方公司


## <a name="about-publishing-your-site"></a>關於發佈您的網站

到目前為止，您已完成您的本機電腦上，包括測試您的網頁的所有工作。 若要執行您<em>.cshtml</em>頁面，您已使用 WebMatrix，IIS Express 也就是內建的 web 伺服器。 但當然沒有人可以看到您已建立您的網站。 若要讓其他人使用您的網站，您必須將它發佈至網際網路。

除非您已經有公用 web 伺服器的存取權，否則發行會表示您所擁有的帳戶*雲端平台*或是*主機服務提供者*。 雲端平台，例如 Microsoft Azure，您的應用程式提供隨選基礎結構。 主機服務提供者是的公司擁有可公開存取的 web 伺服器，並可將租用您空間為您的網站。 主控方案，執行從幾個每月美金的 （或甚至是免費的） 到數以百計的大量商業網站的每月美金的小型站台。

> [!NOTE]
> 您可能必須透過網際網路服務提供者 (ISP)，讓您用來在家裡網際網路服務的公用 web 伺服器的存取權。 不過，您的主機服務提供者必須支援 ASP.NET Web Pages。 許多 Isp 不這麼做，但它一律是值得一查。


在本教學課程中，我們將提供您如何發佈的概觀。 它是不實際提供的所有項目，確切的詳細資料，因為程序稍微不同的每個主機服務提供者。 但是，您會了解的程序的運作方式。

本教學課程包含四個區段：

1. [預設頁面設定](#defaultpage)
2. 發行 （請選擇下列其中之一的項目）  
 a. [您的站台發佈至 Microsoft Azure](#azure)  
 b. [將發佈至哪家虛擬主機公司](#host)
3. [更新即時網站： 重新發行](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>預設頁面設定

當使用者巡覽至您的網站的基底位址時，您的站台的預設頁面會顯示給使用者。 例如，當 Default.htm 設為站台的預設頁面上，在 www.contoso.com，然後瀏覽至<strong>www.contoso.com</strong>瀏覽至相同<strong>www.contoso.com/Default.htm</strong>。

目前，您的網站使用**Default.cshtml**為預設的網頁。 此頁面為您的預設頁面，但在本教學課程您尚未加入任何內容至該頁面所以它會顯示空白頁面。 開啟 Default.cshtml，並取代為下列程式碼中的內容。

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

現在您的網站可供發行集。 首先，您會看到如何將網站部署至 Azure，以及如何將它部署到 web 主機服務公司。 其中一個的選項適用於您的網站，而且您只需要遵循下列其中一個部署選項。

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>您的站台發佈至 Microsoft Azure

本教學課程會先顯示如何將您的網站部署至 Microsoft Azure。 Microsoft 帳戶登入，您可以在 Azure 上建立最多 10 個免費網站。 這些免費的網站提供便利的方式來測試您的網站。 您隨時都可以刪除此範例中站台，稍後若要避免使用所有可用的網站。 您可以建立免費的試用帳戶，只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

在 WebMatrix 功能區中，按一下**發佈** 按鈕。

![WebMatrix 功能區中的 [發佈] 按鈕](publishing/_static/image1.png)

**發佈您的站台**對話方塊隨即出現。 如果您不具有 Microsoft 帳戶登入，對話方塊會包含**開始使用 Azure**連結。 按一下此連結。

![發佈您的網站](publishing/_static/image2.png)

如果您不具有 Microsoft 帳戶登入，您有一次機會來登入。 您必須登入 Microsoft 帳戶來發佈您的網站在 Azure 上。

![登入](publishing/_static/image3.png)

登入您的 Microsoft 帳戶之後, 的對話方塊會包含在 Azure 上建立新的站台，或連接至您現有的網站，在 Azure 上的其中一個連結。

![建立新的站台](publishing/_static/image4.png)

選取 **建立新的站台**。

如果您已命名為您的專案**WebPagesMovies**，為您的網站的預設名稱會是**webpagesmovies.azurewebsites.net**。 這個預設名稱是最有可能無法使用，紅色的驚嘆號所指示。

![預設的網站名稱](publishing/_static/image5.png)

將站台名稱變更為可供使用，並選取靠近您位置的位置。

![變更站台名稱](publishing/_static/image6.png)

按一下 [確定 **Deploying Office Solutions**]。

WebMatrix performss 測試來判斷伺服器是否與您的網站相容。

![相容性測試](publishing/_static/image7.png)

選取 **繼續**。

相容性測試的結果會顯示。

![相容性結果](publishing/_static/image8.png)

選取 **繼續**。

WebMatrix 會顯示檔案和將發佈到網站的資料庫。 由於這是您要發佈站台的第一次時，會列出所有的檔案。 您可以取消選取尚未準備好要發行的檔案。 在後續的發行集，將會顯示已變更的檔案。 請參閱[更新即時網站： 重新發行](#update)。

![發行預覽](publishing/_static/image9.png)

選取 **繼續**。

已將網站部署至 Azure 之後，會顯示訊息，指出已完成部署。

![發佈完成](publishing/_static/image10.png)

您的站台和資料庫已經發行至 Azure，並已公開上市。 按一下指出發行完成後，且您現在會看到您已部署的站台的訊息中的連結。 您或網際網路存取的任何人都可以新增或修改資料庫中的記錄。

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>將發佈至哪家虛擬主機公司

如果您決定不將發佈至 Azure，您可以改為將網站發佈到 web 主機服務公司。

按一下 **尋找虛擬主機**連結。

![在發行設定 對話方塊中的 '尋找虛擬主機' 按鈕](publishing/_static/image12.png)

移至的頁面會列出支援 ASP.NET 的主機服務提供者之 Microsoft 網站上。

![列出主機服務提供者的 Microsoft 網站上的頁面](publishing/_static/image13.png)

很明顯地，很難知道現在完全透過長期來看，您可能需要何種裝載功能。 以下是一些需要考量的事項：

- 基於 WebPagesMovies 站台的詳細資訊，您不需要個別的附加元件的 SQL Server，通常的額外成本。 在您的網站，您會使用 SQL Server Compact Edition，這各自獨立。 不過，您可能需要存取 SQL Server 的某些未來的網站所進行。 如果您認為您可能，請確定您可以稍後新增 SQL Server 功能。
- 檢查主控提供者是否支援 Web Deploy 發行通訊協定。 您可以使用 FTP 通訊協定，發行，但更方便地使用 Web Deploy。

某些網站提供免費的試用期間。 免費試用版是嘗試發行的好方法，並裝載時仍在做實驗使用 WebMatrix 及 ASP.NET Web Pages。

挑選您喜歡的其中一個。 本教學課程中，我們會選取獲得，因為雖然我們已建立本教學課程，該公司，可讓人們裝載站台的幾個月的免費促銷。

> [!NOTE]
> 本教學課程中的主機服務提供者的我們選擇不應解譯為該公司的簽署，超過任何其他。 但我們必須挑選一個圖中，而獲得許多公司的支援發佈 ASP.NET Web Pages 和 Web Deploy 通訊協定。


一般而言，您已使用主機服務提供者註冊之後，公司會傳送一封電子郵件，其中包含使用者名稱和密碼，web 伺服器，以及等等的 URL。 如果裝載的公司支援 Web Deploy 通訊協定，它們可能會傳送您的檔案，其中包含發行設定，或可讓您下載其中一個。 發行設定檔為您簡化程序。

當您註冊，且準備好要發行時，按一下**發佈**WebMatrix 功能區中的按鈕。 **發佈設定**對話方塊隨即出現。

如果裝載提供者傳送給您的發行設定檔，請按一下**匯入發佈設定**連結，並將檔案匯入。 如果您還沒有發佈設定檔，請使用代管公司傳送給您電子郵件中的值填寫欄位。 以下是什麼**發佈設定**對話方塊看起來像當您完成：

![在 [發行設定] 對話方塊中填入的發行設定](publishing/_static/image14.png)

按一下 **驗證連線**。 如果一切正常，則對話方塊中會回報**成功連線到**，這表示它可以與主控提供者的伺服器進行通訊。

![成功訊息，如果發行的設定是否正確](publishing/_static/image15.png)

如果發生問題時，WebMatrix 會會盡可能地告訴您問題的是：

![如果沒有發生問題的錯誤訊息發佈設定](publishing/_static/image16.png)

按一下 **儲存**以儲存設定。 若要執行測試，以確定它可以正確地通訊與裝載站台，WebMatrix 提供：

![若要執行發佈程序測試供應項目訊息](publishing/_static/image17.png)

按一下 [ **是**]。 WebMatrix 會將範例檔案上傳至主控提供者。 相容性測試完成時，WebMatrix 會報告結果：

![發行測試結果](publishing/_static/image18.png)

如果您是準備就緒，請繼續並按一下**繼續**實際啟動發佈程序。 WebMatrix 找出什麼檔案位於您的網站和已在主機伺服器 （目前，無） 以及可讓您在發行程序預覽：

![哪些發佈程序會將上傳的檔案的預覽](publishing/_static/image19.png)

要發行的檔案清單包含您已建立類似的網頁*Movies.cshtml*。 清單也包含協助程式，您已安裝，來為您的資料庫執行 SQL Server Compact Edition 檔案的檔案。 如此一來，在初始發佈程序可能很長。

按一下 [ **繼續**]。 WebMatrix 會將您的檔案複製到裝載提供者的伺服器。 完成時，狀態列會報告結果：

![狀態列訊息時在發行程序已成功完成](publishing/_static/image20.png)

若要查看您的即時網站，按一下 [狀態] 列中的連結。 新增*電影*至 URL，您會看到*Movies.cshtml*您所建立的檔案：

![即時網站顯示影片頁面](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>更新即時網站： 重新發行

一旦您已發行您的網站 （至 Azure 或哪家虛擬主機公司中），就會有兩份&mdash;電腦及服務提供者上的版本上的版本。 您可能會想要繼續開發網站 (如果沒有其他資料，為下一個教學課程集的一部分)。 當您這樣做時，您必須重新發佈您的網站，才能將變更從您的電腦複製到服務提供者。 在 WebMatrix 中的發行處理程序可以判斷檔案已變更您的站台上，並發行這些檔案。

若要查看重新發行的運作方式，開啟*Movies.cshtml*站台，進行一些小變更，然後儲存檔案。 例如，將標題變更為`Movies - Updated`。

按一下 **發佈**功能區按鈕。 WebMatrix 判斷變更的內容，並會顯示您的預覽會將發佈的檔案。

![[發佈] 對話方塊中，顯示已變更的檔案可供重新發行](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> 根據預設，WebMatrix 發行您的資料庫 (*.sdf*檔案) 只在第一次發行站台。 一旦您的站台發佈，並與網站互動的人員，即時網站上的資料庫通常會有站台的實際資料。 您一定要非常小心，不要覆寫即時資料庫 *.sdf*位於您的電腦，通常只包含測試資料的檔案。 這就是為什麼您看到警告**發佈將會覆寫任何遠端資料庫**，以及為什麼的核取方塊*WebPagesMovies.sdf*預設為清除。


按一下 [ **繼續**]。 WebMatrix 發佈變更的檔案，並顯示成功訊息，就跟您發行的第一次。

移至即時網站 （您可以按一下連結成功訊息中的如果它仍會顯示），並確認您的變更已發行。

> [!TIP] 
> 
> **從遠端編輯檔案**
> 
> 變更您的網站，然後再重新發佈的替代方案，您可以編輯直接在 WebMatrix 中的遠端檔案。 在此案例中，開啟 服務提供者上的檔案，並 WebMatrix 會下載一份它以進行編輯。 每次您儲存檔案時，WebMatrix 會將變更傳送到站台。
> 
> 遠端編輯是簡單的方式，對您的即時網站進行變更。 不過，您進行這種方式的變更不同步處理您的本機網站中的檔案。 若要同步的本機檔案與遠端站台，您可以下載遠端檔案。 此程序很像發行，除了以相反的方式。
> 
> 我們將不會更多說明關於遠端編輯與遠端下載設施的 WebMatrix 這裡。 它們非常有用，如果多人有不同的電腦上相同的站台上運作。 如需詳細資訊，請參閱 <<c0> [ 發行和編輯與 WebMatrix 2 Beta 的遠端站台](https://go.microsoft.com/fwlink/?LinkId=251591)。


## <a name="additional-resources"></a>其他資源

- [ASP.NET WebMatrix 的 ASP.NET Web Pages 論壇](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)，不妨張貼問題，並取得解答。

> [!div class="step-by-step"]
> [上一步](layouts.md)
