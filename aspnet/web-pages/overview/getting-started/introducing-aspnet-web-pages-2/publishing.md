---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "導入 ASP.NET Web Pages-使用 WebMatrix 中發行站台 |Microsoft 文件"
author: tfitzmac
description: "本教學課程是導入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程組中最後一篇。 其中也會討論如何發佈網站 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>簡介使用 WebMatrix 中發行站台的 ASP.NET Web Pages-
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程是導入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程組中最後一篇。 它討論如何將您的站台發佈至網際網路，以便其他人可以使用它。 它會假設您已完成透過數列[建立 ASP.NET Web Pages 站台中的一致查詢](https://go.microsoft.com/fwlink/?LinkId=251585)。
> 
> 您將學習如何發佈站台使用：
> 
> - Microsoft Azure
> - Web 代管公司


## <a name="about-publishing-your-site"></a>關於發佈您的網站

到目前為止，您所做過您的本機電腦上，包括測試您的網頁的所有工作。 若要執行您*.cshtml*頁面，您已使用的 WebMatrix，IIS Express 也就是內建的 web 伺服器。 但當然沒有人可以看到您已建立您的站台。 若要讓其他人使用您的網站，您必須發佈至網際網路。

除非您已經有公用的 web 伺服器的存取權，表示您所擁有的帳戶具有發佈*雲端平台*或*主機服務提供者*。 雲端平台，例如 Microsoft Azure 提供您的應用程式視基礎結構。 主機服務提供者是的公司擁有可公開存取的網頁伺服器，並可將租用您網站的空間。 主控方案，執行從一個月數美元 （或甚至是免費） 小型網站數以百計的一個月的高容量商業網站 （美元）。

> [!NOTE]
> 您可能必須透過您取得在家網際網路服務使用的網際網路服務提供者 (ISP) 的公用 web 伺服器的存取權。 不過，您的主機服務提供者必須支援 ASP.NET Web Pages。 許多 Isp 不這麼做，但值得一律檢查。


在本教學課程中，我們會提供您如何發行的概觀。 因為在程序的位元與不同的每個主機服務提供者不適合所有項目，提供精確的詳細資料。 但是，您會取得建議的程序的運作方式。

本教學課程包含四個區段：

1. [設定預設頁面](#defaultpage)
2. 發行 （請選擇下列其中之一）  
 a. [您的站台發佈至 Microsoft Azure](#azure)  
 b. [您的網站發行至 Web 主控公司](#host)
3. [更新即時網站： 重新發行](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>設定預設頁面

當使用者巡覽至您的網站的基底位址時，您的站台的預設頁面會顯示給使用者。 例如，當先前的範例在 www.contoso.com 設定為預設的網頁站台，然後瀏覽至**www.contoso.com**瀏覽至相同**www.contoso.com/Default.htm**。

目前，您的網站會使用**Default.cshtml**為預設的網頁。 此頁面是適合您的預設頁面上，但在本教學課程您尚未加入任何內容至該頁面，它會顯示空白頁面。 開啟 Default.cshtml，並取代為下列程式碼中的內容。

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

現在您的網站可供發行集。 首先，您會看到如何將網站部署至 Azure，以及如何將它部署至 web 主控公司。 其中一個的選項適用於您的網站，而且您只需要遵循其中一種部署選項。

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>您的站台發佈至 Microsoft Azure

本教學課程會先顯示如何將您的網站部署至 Microsoft Azure。 Microsoft 帳戶登入，您可以在 Azure 上建立最多 10 個可用的站台。 這些可用的網站提供便利的方式來測試您的網站。 您隨時都可以刪除此範例站台，稍後若要避免使用所有可用的網站。 您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

在 WebMatrix 功能區中，按一下**發行** 按鈕。

![WebMatrix 功能區中的 [發佈] 按鈕](publishing/_static/image1.png)

**發行您的站台**對話方塊隨即出現。 如果您有未登入您的 Microsoft 帳戶，對話方塊會包含**開始使用 Azure**連結。 按一下此連結。

![發行您的網站](publishing/_static/image2.png)

如果您不具有 Microsoft 帳戶登入，您可以有一次機會來登入。 您必須發佈 azure 網站來登入 Microsoft 帳戶。

![登入](publishing/_static/image3.png)

之後您的 Microsoft 帳戶登入，對話方塊會包含在 Azure 上建立新的網站，或連接到您現有的網站在 Azure 上的連結。

![建立新站台](publishing/_static/image4.png)

選取**建立新的站台**。

如果您命名為您的專案**WebPagesMovies**，為您的網站的預設名稱會是**webpagesmovies.azurewebsites.net**。 這個預設名稱是最有可能無法使用，紅色的驚嘆號所指示。

![預設的網站名稱](publishing/_static/image5.png)

將站台名稱變更為可用，並選取接近您位置的位置。

![變更站台名稱](publishing/_static/image6.png)

按一下 [確定]。

WebMatrix performss 測試來判斷伺服器是否與您的網站相容。

![相容性測試](publishing/_static/image7.png)

選取**繼續**。

會顯示相容性測試的結果。

![相容性結果](publishing/_static/image8.png)

選取**繼續**。

WebMatrix 會顯示檔案和資料庫，將會發行至網站。 由於這是您要發佈的站台的第一次時，會列出所有的檔案。 您可以取消核取尚未準備好要發行的檔案。 在後續的發行集，會顯示已變更的檔案。 請參閱[更新即時網站： 重新發行](#update)。

![發行預覽](publishing/_static/image9.png)

選取**繼續**。

已將網站部署至 Azure 之後，會顯示訊息，指出已完成部署。

![發行完成](publishing/_static/image10.png)

您的網站和資料庫已經發行至 Azure，而現在提供給大眾使用。 按一下訊息，指出發行完成，而且您現在可以看到您已部署的網站中的連結。 您或任何具有網際網路存取權的人可以新增或修改資料庫中的記錄。

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>您的網站發行至 Web 主控公司

如果您決定將發佈到 Azure，您可以改為虛擬主機公司來發行您的網站。

按一下**尋找虛擬主機**連結。

![發行設定 對話方塊中的 '到 web 裝載' 按鈕](publishing/_static/image12.png)

列出主機服務提供者支援 ASP.NET 的 Microsoft 網站上移至頁面。

![列出主機服務提供者的 Microsoft 網站上的頁面](publishing/_static/image13.png)

很明顯地，可能很難知道現在完全透過長期來看，您可能需要哪些主控功能。 以下是幾個要考慮的事項：

- 基於 WebPagesMovies 站台的詳細資訊，您不需要個別的附加元件的 SQL Server，通常的額外成本。 在您的網站，您使用 SQL Server Compact Edition，這各自獨立。 不過，您可能需要存取 SQL Server 的某些未來網站工作。 如果您認為您可能，請確定您可以稍後新增 SQL Server 功能。
- 請檢查主機服務提供者是否支援 Web Deploy 發行通訊協定。 您可以使用 FTP 通訊協定，發行，但更方便地使用 Web Deploy。

某些網站提供免費的試用期間。 免費試用版是一個好的方法，再試一次發行並同時裝載正在仍實驗 WebMatrix 及 ASP.NET 網頁。

選取您要的其中一個。 此教學課程中，我們會選取 DiscountASP.NET，因為我們已建立本教學課程，而該公司促銷，讓使用者在裝載站台的幾個月免費。

> [!NOTE]
> 我們選擇的主機服務提供者在此教學課程不應解譯為該公司支持，透過任何其他。 我們必須挑選一個圖中，但 DiscountASP.NET 是其中一個支援的 ASP.NET Web Pages 和 Web Deploy 通訊協定發行許多公司。


一般而言，您已經註冊過主機服務提供者之後，公司會傳送電子郵件，其中包含使用者名稱和密碼，網頁伺服器等的 URL。 如果代管公司會支援 Web Deploy 通訊協定，他們可能會傳送您所在的檔案發行設定，或可讓您下載一個。 發行設定檔讓您簡化程序。

當您已經註冊，並準備好要發行時，按一下**發行**WebMatrix 功能區中的按鈕。 **發行設定**對話方塊隨即出現。

如果主機服務提供者傳送給您的發行設定檔，按一下**匯入發行設定**連結，並將檔案匯入。 如果您還沒有的發行設定檔，請使用代管公司傳送給您電子郵件中的值填寫的欄位。 以下是**發行設定**對話方塊看起來會像當您完成：

![填入發行設定 對話方塊中的發行設定](publishing/_static/image14.png)

按一下**驗證連線**。 如果所有項目是 確定，對話方塊報告**已成功連接**，這表示它可以與裝載提供者的伺服器通訊。

![成功訊息，如果將發行設定正確無誤](publishing/_static/image15.png)

如果沒有問題，WebMatrix 會會盡可能地告訴您此問題：

![如果沒有發生問題的錯誤訊息發行設定](publishing/_static/image16.png)

按一下**儲存**以儲存設定。 若要執行測試，以確定它可以正確通訊與裝載站台，WebMatrix 提供：

![執行測試，在發行過程中提供的訊息](publishing/_static/image17.png)

按一下 [ **是**]。 WebMatrix 會上載至主機服務提供者的某些範例檔案。 當相容性測試完成時，WebMatrix 會報告結果：

![發行測試結果](publishing/_static/image18.png)

如果您準備好移，請繼續進行，然後按一下**繼續**實際啟動發行程序。 WebMatrix 找出哪些檔案位於您的網站和已存在的主機伺服器 （現在無） 以及發行程序的預覽可讓您：

![哪些發行程序都會將上傳的檔案的預覽](publishing/_static/image19.png)

要發行的檔案清單包含您已建立像網頁*Movies.cshtml*。 清單也包含協助程式，您已安裝，為您的資料庫執行 SQL Server Compact Edition 等檔案的檔案。 如此一來，初始發行處理程序很嚴重。

按一下 [ **繼續**]。 WebMatrix 會將您的檔案複製到裝載提供者的伺服器。 完成時，狀態列會報告結果：

![狀態列訊息發行程序成功完成時](publishing/_static/image20.png)

若要查看您即時網站，請按一下 [狀態] 列中的連結。 新增*電影*至的 URL，您將會看到*Movies.cshtml*您所建立的檔案：

![顯示 [影片] 頁面上的即時網站](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>更新即時網站： 重新發行

一旦您已發行您的網站 （到 Azure 或虛擬主機公司），有兩份&mdash;電腦及服務提供者上的版本上的版本。 您可能會想要繼續開發網站 (如果沒有其他資料，做為下一個教學課程組的一部分)。 當您這樣做時，您必須重新發佈您的網站，以將變更從您的電腦複製到服務提供者。 在 WebMatrix 中發行流程可以判斷檔案已變更您的站台上，並發佈僅擁有的檔案。

若要重新發佈的運作方式，請開啟*Movies.cshtml*站台，進行一些小型的變更，並儲存檔案。 例如，將標題變更為`Movies - Updated`。

按一下**發行**功能區按鈕。 WebMatrix 可決定變更，並顯示您的檔案，它會將發佈的預覽。

![[發行] 對話方塊，顯示已變更檔案隨時可供重新發行](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> 根據預設，WebMatrix 發行您的資料庫 (*.sdf*檔案) 只在第一次發行站台。 一旦發行您的網站，並與網站互動的人員，即時網站上的資料庫通常有站台的實際資料。 您一定要非常小心，不要覆寫即時資料庫與*.sdf*在您的電腦，通常只包含測試資料上的檔案。 這就是為什麼您看到警告**發行將會覆寫任何遠端資料庫**，為什麼的核取方塊和*WebPagesMovies.sdf*預設為清除。


按一下 [ **繼續**]。 WebMatrix 發行變更的檔案，並顯示成功訊息，如同您發行的第一次。

移至即時網站 （您可以按一下連結成功訊息中的如果仍然顯示），並確認您的變更已發行。

> [!TIP] 
> 
> **從遠端編輯檔案**
> 
> 變更您的網站，然後再重新發佈的替代方案，您可以編輯直接在 WebMatrix 中的遠端檔案。 在此案例中，開啟位於服務提供者的檔案和 WebMatrix 下載複製它供您編輯。 每次您儲存檔案時，WebMatrix 會將變更傳送至站台。
> 
> 遠端編輯是簡單的方式，可讓您即時網站的變更。 不過，您進行這種方式的變更未同步您的本機網站中的檔案。 若要同步的本機檔案與遠端站台，您可以下載的遠端檔案。 此程序的作用很像發行，除了以反向方向。
> 
> 我們將不會更說明有關遠端編輯和遠端下載設備的 WebMatrix 這裡。 它們很多人有不同的電腦上相同的網站上運作的很有用。 如需詳細資訊，請參閱[發行並編輯使用 WebMatrix 2 Beta 的遠端站台](https://go.microsoft.com/fwlink/?LinkId=251591)。


## <a name="additional-resources"></a>其他資源

- [ASP.NET WebMatrix ASP.NET Web Pages 論壇](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)，不妨張貼問題和取得解答。

>[!div class="step-by-step"]
[上一步](layouts.md)
