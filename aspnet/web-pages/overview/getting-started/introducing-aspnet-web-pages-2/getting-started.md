---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: 導入 ASP.NET Web Pages-開始使用 |Microsoft 文件
author: tfitzmac
description: WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。 使用 Visual Studio 或 Visual Studio 程式碼。 本指南...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5fd67a230f76774e102094f42426b8bb126c0cc6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a>介紹的 ASP.NET Web Pages-快速入門
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。 使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)或[Visual Studio 程式碼](https://code.visualstudio.com/)。
> 
> 
> 本指南及應用程式可讓您的 ASP.NET Web Pages （第 2 版或更新版本） 的概觀，以及 Razor 語法，輕量型的架構，用於建立動態網站。 也引進了 WebMatrix，用於建立網頁和網站的工具。
> 
> **層級**： 新 ASP.NET 網頁。  
> **技術假設**: HTML、 基本的階層式樣式表 (CSS)。
> 
> 您將了解的內容集的第一個教學課程中：
> 
> - ASP.NET Web Pages 技術是，它為。
> - WebMatrix 是什麼。
> - 如何安裝的所有項目。
> - 如何建立使用 WebMatrix 的網站。
>   
> 
> 功能/技術討論：
> 
> - Microsoft Web Platform Installer。
> - WebMatrix。
> - *.cshtml*頁面
>   
> 
> Mike 教宗最初撰寫本教學課程。 Tom FitzMacken 更新它適用於 Microsoft WebMatrix 3。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>您應該知道什麼？

我們假設您已熟悉：

- **HTML**。 沒有深入的專業知識是必要項。 我們將不會說明 HTML，但我們也不會使用複雜的任何項目。 我們會提供 HTML 教學課程，我們認為非常有用的連結。
- **階層式樣式表 (CSS)**。 與使用相同 HTML。
- **基本資料庫概念**。 如果您已試算表用於資料和排序和篩選是專業知識的層級的資料，我們通常假設這個教學課程的集合。

我們也假設您感興趣學習基本程式設計。 ASP.NET Web 網頁會使用稱為 C# 的程式設計語言。 您不必使用任何在程式設計中，只在它感興趣的背景。 如果您曾經編寫任何 JavaScript 之前的網頁中，您有足夠的背景。

請注意，是否您很熟悉程式設計，您可能會發現本教學課程一開始設定緩時變移動時發生我們將新的程式設計人員，了解。 隨著過去的第一個的幾個教學課程，不過，會有較少的基本程式設計，以說明，且項目時，會將上，在更快的剪輯。

## <a name="what-do-you-need"></a>您需要什麼？

以下是您需要的項目：

- 執行 Windows 8、 Windows 7、 Windows Server 2008 或 Windows Server 2012 的電腦。
- 即時的網際網路連線。
- 系統管理員權限 （所需的安裝程序）。

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web Pages 是什麼？

ASP.NET Web Pages 是一種架構，可用來建立動態網頁。 簡單的 HTML 網頁是靜態的。取決於其內容頁的固定 HTML 標記。 如同您建立以 ASP.NET Web Pages 動態頁面可讓您使用程式碼即時建立網頁內容。

動態網頁可讓您執行各種作業。 您可以使用的表單，要求使用者輸入，然後變更 此頁面會顯示，或它的外觀。 您可以從使用者取得資訊、 將它儲存在資料庫中，以及然後稍後列出。 您可以從您的網站，傳送電子郵件。 您可以與其他網站 （例如，對應服務） 上的服務互動，並產生整合來自這些來源的資訊的頁面。

## <a name="what-is-webmatrix"></a>WebMatrix 是什麼？

WebMatrix 是一種工具，整合網頁編輯器、 資料庫公用程式、 web 伺服器以進行測試頁面，以及您的網站發佈到網際網路的功能。 WebMatrix 是免費的而且容易安裝且容易使用。 （它也可以只 plain HTML 網頁，以及其他 PHP 之類的技術。）

您不會實際*有*運作以 ASP.NET Web Pages 使用 WebMatrix。 您可以使用文字編輯器 中，建立頁面，例如並測試頁面所使用的 web 伺服器，您具有存取權。 不過，WebMatrix 都非常容易，因此這些教學課程會使用整個 WebMatrix。

## <a name="about-these-tutorials"></a>關於這些教學課程

此教學課程集是如何使用 ASP.NET Web Pages 的簡介。 9 教學課程中總共有此簡介的教學課程集。 它的一系列的教學課程集，會讓您從 ASP.NET Web Pages 新手建立 real、 專業網站的一部分。

此第一個教學課程設定著重於說明如何使用以 ASP.NET Web Pages 的基本概念。 當您完成時，您可以使用其他挑選其中一段時間，而，更深入地瀏覽網頁的教學課程組。

我們故意繼續輕鬆深入說明。 然後每當我們顯示的項目，此教學課程集我們一律選擇我們認為的方式是最容易了解。 更新的教學課程集移到更深入，並且顯示更有效率或更有彈性方法 （也更有趣）。 但這些教學課程會要求您先了解基本概念。

您已啟動的教學課程集涵蓋這些功能的 ASP.NET Web Pages:

- 導入，並取得安裝的所有項目。 （這是您正在閱讀的教學課程中）。
- ASP.NET Web Pages 程式設計基本概念。
- 建立資料庫。
- 建立及處理使用者輸入的表單。
- 加入、 更新和刪除資料庫中的資料。

## <a name="what-will-you-create"></a>您將建立什麼？

本教學課程中設定，而且後續的心力都圍繞網站，您可以在其中列出您喜歡的電影。 您可以輸入電影、 編輯它們，並將它們列出。 以下是幾個您將建立在此教學課程的集合中的頁面。 第一個示範影片，列出您要建立的頁面：

![顯示的電影清單已經電影應用程式](getting-started/_static/image1.png)

此外，這裡有可讓您將新的電影資訊新增至您的網站頁面：

![顯示將加入電影頁面完成的電影應用程式](getting-started/_static/image2.png)

後續的教學課程集組建上設定，並加入更多的功能，例如將圖片上載、 讓登入的人、 傳送電子郵件，以及整合社交媒體和。

## <a name="see-this-app-running-on-azure"></a>請參閱 < 在 Azure 上執行此應用程式

若要查看即時 web 應用程式執行完成站台嗎？ 您可以部署到 Azure 帳戶的完整版本的應用程式，只要按一下下列按鈕。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

您需要 Azure 帳戶，以將此方案部署到 Azure。 如果您沒有帳戶，您會有下列選項：

- [開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。
- [啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。

## <a name="installing-everything"></a>安裝的所有項目

您可以使用 microsoft Web Platform Installer 安裝的所有項目。 作用中，您會安裝安裝程式，並再使用它來安裝其他項目。

若要使用的網頁，您必須至少有 Windows XP SP3 安裝或 Windows Server 2008 或更新版本。

在[網頁 」 頁面](../../../index.md)ASP.NET 網站中，按一下 **安裝**。

![ASP.NET 網頁站台顯示&quot;安裝 WebMatrix&quot;按鈕](getting-started/_static/image3.png)

系統會要求您接受授權條款及隱私權聲明，才能安裝 WebMatrix。

![接受條款，以開始安裝](getting-started/_static/image4.png)

按一下**執行**開始安裝。 (如果您想要儲存安裝程式，按一下**儲存**，然後從儲存所在的資料夾執行安裝程式。)

![](getting-started/_static/image5.png)

Web Platform Installer 出現時，您可以準備安裝 WebMatrix。 按一下 [安裝] 。

![](getting-started/_static/image6.png)

安裝程序找出它必須在電腦上安裝並啟動程序。 根據什麼完全已安裝，處理程序可以需要幾分鐘的時間到幾分鐘的時間。 選取**我接受**接受授權條款。

## <a name="hello-webmatrix"></a>Hello WebMatrix

完成時，安裝程序就可以自動啟動 WebMatrix。 如果不是，在 Windows 中，從**啟動**功能表，啟動**Microsoft WebMatrix**。

當您第一次啟動 WebMatrix 時，您可以使用您的 Microsoft 帳戶登入 Microsoft Azure 的機率。 只要登入，您會收到 10 的免費 web 應用程式透過 Azure。 這些免費的 web 應用程式提供便利的方式來測試您的應用程式。 如果您還沒有 Azure 帳戶，但您需要具有 MSDN 訂用帳戶，您可以[啟動您的 MSDN 訂用帳戶權益](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)。 否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

您不必立即登入以繼續進行這個教學課程。 如果您沒有登入現在，您仍然可以選擇稍後登入。 最後一個[主題](publishing.md)在本教學課程系列涵蓋如何將您的網站部署到 Azure; 因此，您必須登入才能完成該主題。

此時，請使用您的 Microsoft 帳戶或選取的登入**現在不要**在右下角。

![登入](getting-started/_static/image7.png)

若要開始，您會建立空白網站，並加入頁面。 在稍後的教學課程中，在此集合中，您將播放的其中一個內建的網站範本。

在 [開始] 視窗中，按一下**新增**。

![WebMatrix 啟動螢幕](getting-started/_static/image8.png)

範本是網站的預先建置的檔案和頁面的不同類型。 若要查看所有可用的預設範本，請選取範本庫選項。

![選取範本組件庫](getting-started/_static/image9.png)

在**快速入門**視窗中，選取**空白網站**從**ASP.NET**群組，並命名新的站台 」 WebPagesMovies"。

![WebMatrix 快速入門與選取的空白網站範本的視窗](getting-started/_static/image10.png)

按 [ **下一步**]。

如果您有登入您的 Microsoft 帳戶，您將有機會在 Azure 上建立站台。 根據您的網站的預設名稱名稱**WebPagesMovies.azurewebsites.net**建議; 不過，驚嘆號，表示這個名稱並不適用於 Windows Azure。 為了簡單起見，請選取**略過**略過目前在 Azure 上建立網站。 在這一系列，稍後我們將發佈站台至 Azure。

![建立 azure 站台](getting-started/_static/image11.png)

WebMatrix 會建立並開啟的網站：

![在 WebMatrix 中開啟新的 WebPagesMovies 網站](getting-started/_static/image12.png)

在頂端，也無需快速存取工具列功能區。 在左下方，您會看到工作區選取器切換的工作 (**網站**，**檔案**，**資料庫**，**報表**)。 右邊是內容窗格，針對編輯器 中與報表。 和底端有時候會看到訊息的通知列。

您將進一步了解 WebMatrix 和它的功能當您瀏覽這些教學課程。

## <a name="creating-a-web-page"></a>建立的 Web 網頁

熟悉 WebMatrix 及 ASP.NET 網頁，您將建立簡單的網頁。

在工作區中選擇器 中選取**檔案**工作區。 此工作區可讓您使用檔案和資料夾。 左的窗格會顯示您的站台的檔案結構。 功能區的變更，以顯示與檔案相關的工作。

![檔案在 WebMatrix 工作區](getting-started/_static/image13.png)

在 功能區中，按一下底下的箭頭**新增**，然後按一下 **新檔案**。

![使用&quot;新增&quot;命令在功能區中建立新的檔案](getting-started/_static/image14.png)

WebMatrix 會顯示一份檔案類型。 選取**CSHTML**，然後在**名稱**方塊中，輸入"HelloWorld"。 CSHTML 頁面為 ASP.NET Web Pages 頁面。

![建立新的 CSHTML 頁面命名 HelloWorld.cshtml](getting-started/_static/image15.png)

按一下 [確定 **Deploying Office Solutions**]。

WebMatrix 所建立的網頁，並在編輯器中開啟。

![WebMatrix 編輯器中新的 [HelloWorld] 頁面](getting-started/_static/image16.png)

如您所見，此頁面包含大部分一般 HTML 標記中的，除了區塊上方看起來像這樣：

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

新增程式碼中，您將會很快看到。

請注意，頁面的不同部分&mdash;項目名稱、 屬性和文字，再加上位於最上方的區塊，都是完全不同的色彩。 這稱為*語法反白顯示*，並讓您更輕鬆地清除所有項目。 它是其中一個功能，以簡化使用 WebMatrix 中的 web 網頁。

新增內容`<head>`和`<body>`項目如下列範例所示。 （如果您想，您可以只複製下列區塊並使用此程式碼取代整個的現有頁面。）

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

在快速存取工具列或**檔案**功能表上，按一下 **儲存**。

![WebMatrix 快速存取工具列中的儲存按鈕](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>測試網頁

在**檔案**工作區中，以滑鼠右鍵按一下*HelloWorld.cshtml*頁面，然後按一下**瀏覽器中啟動**。

![執行網頁使用 WebMatrix 功能區中的 [執行] 按鈕](getting-started/_static/image18.png)

WebMatrix 啟動內建的 web 伺服器 (IIS Express) 可讓您測試您的電腦上的頁面。 （如果沒有 IIS Express 在 WebMatrix 中，您就必須將網頁發行到 web 伺服器位置無法測試之前。）頁面會顯示在預設瀏覽器。

![&quot;Hello World&quot;網頁瀏覽器中執行](getting-started/_static/image19.png)

請注意，當您在 WebMatrix 中測試網頁，在瀏覽器的 URL 會像下面`http://localhost:33651/HelloWorld.cshtml.`名稱*localhost*指的是本機伺服器，這表示頁面由您自己電腦上的 web 伺服器。 如前所述，WebMatrix 會包含名為 IIS Express 時啟動頁面執行的 web 伺服器程式。

之後的數字*localhost* (例如， *localhost:33651*) 是指*連接埠號碼*您電腦上。 這是 「 通道 」 IIS Express 用於此特定的網站數目。 當您建立您的網站，並對每個您所建立的網站不同的連接埠號碼就會隨機選取從 1024 到 65536 的範圍。 （當您測試自己的站台時，連接埠號碼幾乎肯定會比 33561 數目不同。）每個網站使用不同的通訊埠，IIS Express 可以直接保留其中一個站台進行通訊。

稍後當您將網站發佈到公開的 web 伺服器，不會再看見*localhost*在 URL 中。 此時，您會看到較為典型的 URL，如`http://myhostingsite/mywebsite/HelloWorld.cshtml`或任何頁面。 您將進一步了解發行稍後在本教學課程系列的網站。

## <a name="adding-some-server-side-code"></a>伺服器端程式碼加入

關閉瀏覽器，並返回到 WebMatrix 中的頁面。

程式碼區塊中加入一行，使它看起來像下列程式碼：

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

這是有點 Razor 程式碼。 取得目前的日期和時間，以及將到該值可能清楚*變數*名為`currentDateTime`。 您要閱讀更多有關 Razor 語法，在下一個教學課程。

本文的頁面上之後,`<p>Hello World!</p>`項目，加入下列：

[!code-html[Main](getting-started/samples/sample4.html)]

這個程式碼取得您放入值`currentDateTime`頂端變數並將它插入頁面的標記。 `@`字元標示 ASP.NET Web Pages 中的程式碼頁面。

執行一次 （WebMatrix 會將變更儲存為您執行的頁面之前） 的頁面。 此時您會看到的日期和時間在頁面中。

![&quot;Hello World&quot;與動態產生的時間顯示瀏覽器中執行的頁面](getting-started/_static/image20.png)

請稍候片刻，然後重新整理瀏覽器中的頁面。 日期和時間顯示已更新。

在瀏覽器中，查看網頁原始檔。 它看起來像下列標記：

[!code-html[Main](getting-started/samples/sample5.html)]

請注意，`@{ }`上層區塊不存在。 也請注意，日期和時間顯示顯示的實際字元字串 (`1/18/2012 2:49:50 PM`或任何)，而非`@currentDateTime`像是中*.cshtml*頁面。 發生了什麼事如下，當您執行 [] 頁面上，ASP.NET 會處理所有的程式碼 （幾乎不在此情況下） 已標記為`@`。 程式碼會產生輸出，以及該輸出插入至頁面。

## <a name="this-is-what-aspnet-web-pages-are-about"></a>這是 ASP.NET 網頁所需

當您讀取 ASP.NET Web Pages 會產生動態網頁內容時，您已在此看到最好。 您剛建立的網頁包含您所見過的相同 HTML 標記。 它也可以包含可執行各種工作的程式碼。 在此範例中，它沒有簡單的工作，取得目前的日期和時間。 如前所述，您可以與 HTML，以產生輸出的頁面中穿插程式碼。 當其他人要求*.cshtml*仍在指針的網頁伺服器時，瀏覽器，ASP.NET 網頁處理 page。 ASP.NET 插入的程式碼的輸出 （如果有的話） 頁面為 HTML。 當程式碼處理完成時，ASP.NET 會傳送至瀏覽器的結果頁面。 所有瀏覽器範疇是 HTML。 以下是圖表：

![如何 ASP.NET 產生 HTML 動態的概念流程](getting-started/_static/image21.png)

概念很簡單，但許多有趣的工作可執行程式碼，而且有許多有趣的方式可以在其中您可以動態地將 HTML 內容加入頁面。 和 ASP.NET *.cshtml* ，像是任何 HTML 頁面上，也可以包含在瀏覽器本身 （JavaScript 和 jQuery 程式碼） 中執行的程式碼。 您將探索所有的這些作業在此教學課程集和後續的。

## <a name="coming-up-next"></a>接下來

在此系列的下一個教學課程，您瀏覽 ASP.NET Web Pages 稍微程式設計。

## <a name="additional-resources"></a>其他資源

[從頭開始建立 ASP.NET 網站](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)。 這是教學課程，特別是有關使用 WebMatrix (不 ASP.NET Web Pages)。 它會進入有點有關的一些其他功能不包含在此教學課程的集合中的 WebMatrix 的其他詳細資料。

> [!div class="step-by-step"]
> [下一步](intro-to-web-pages-programming.md)
