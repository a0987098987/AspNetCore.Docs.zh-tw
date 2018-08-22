---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: 簡介 ASP.NET Web Pages-開始使用 |Microsoft Docs
author: tfitzmac
description: WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。 使用 Visual Studio 或 Visual Studio 程式碼。 本指南...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 835e359edc87335366c82e35c1ff04902b70334b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823648"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>ASP.NET Web Pages-快速入門簡介
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。 使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)或是[Visual Studio Code](https://code.visualstudio.com/)。
> 
> 
> 本指南及應用程式可讓您的 ASP.NET Web Pages （版本 2 或更新版本） 的概觀和 Razor 語法，來建立動態網站的輕量型架構。 它也引入了 WebMatrix，用於建立網頁和網站的工具。
> 
> **層級**： 新增至 ASP.NET Web 網頁。  
> **假設的技能**: HTML、 基本的階層式樣式表 (CSS)。
> 
> 您將學到什麼集的第一個教學課程中：
> 
> - 說明 ASP.NET Web Pages 技術和用途。
> - WebMatrix 是什麼。
> - 如何安裝所有項目。
> - 如何使用 WebMatrix 建立網站。
>   
> 
> 功能/技術討論：
> 
> - Microsoft Web Platform Installer。
> - WebMatrix。
> - *.cshtml*頁面
>   
> 
> Mike 主教最初撰寫本教學課程。 Tom FitzMacken 會更新它適用於 Microsoft WebMatrix 3。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>您應該知道什麼？

我們假設您已熟悉：

- **HTML**。 需要沒有深入的專業知識。 我們不會說明的 HTML，但我們也不會使用複雜的任何項目。 我們將提供我們認為它們很好用的 HTML 教學課程的連結。
- **階層式樣式表 (CSS)**。 相同的 HTML。
- **基本資料庫概念**。 如果您已試算表用於資料和排序和篩選的資料，是層級的專業知識，我們通常假設本教學課程組。

我們也假設您想要學習基本的程式設計。 ASP.NET Web Pages 使用稱為 C# 的程式設計語言。 您不需要有在程式設計中，只在它的感興趣的任何背景。 如果您曾經撰寫任何 JavaScript 在網頁上之前，您有足夠的背景。

雖然我們將新的程式設計人員，了解，請注意，如果您熟悉的程式設計，您可能會發現本教學課程一開始就設定移動緩慢。 隨著我們越來越過去的第一個的幾個教學課程，不過，會有較基本的程式設計，以說明，且項目時，會將上，在更快速的剪輯。

## <a name="what-do-you-need"></a>您需要什麼？

以下是您需要的項目：

- 執行 Windows 8、 Windows 7、 Windows Server 2008 或 Windows Server 2012 的電腦。
- 即時的網際網路連線。
- （所需的安裝程序） 的系統管理員權限。

## <a name="what-is-aspnet-web-pages"></a>ASP.NET Web Pages 是什麼？

ASP.NET Web Pages 是一種架構，可用來建立動態網頁。 簡單的 HTML 網頁是靜態的;其內容取決於固定在頁面的 HTML 標記。 建立以 ASP.NET Web Pages 等動態頁面可讓您使用程式碼，在作業中，建立網頁內容。

動態頁面可讓您執行各種作業。 您可以使用的表單，要求使用者輸入，然後變更  頁面上所顯示的項目，或它的樣子。 您可以從使用者取得資訊，將它儲存在資料庫中，則稍後列出。 您可以從您的網站傳送電子郵件。 您可以在網站 （例如，對應服務） 上其他服務進行互動，並產生頁面，將那些來源的資訊整合。

## <a name="what-is-webmatrix"></a>WebMatrix 是什麼？

WebMatrix 是一種工具，整合網頁編輯器、 資料庫公用程式、 web 伺服器以進行測試頁面，以及可將您的網站發佈到網際網路的功能。 WebMatrix 是免費的而且容易安裝卻簡單易用。 （它也適用於只是單純的 HTML 網頁，以及適用於 PHP 之類的其他技術。）

您不會實際*有*要以 ASP.NET Web Pages 使用 WebMatrix。 您可以使用文字編輯器 中，建立頁面，例如及測試頁面使用 web 伺服器，您可以存取。 不過，WebMatrix 全都非常容易，因此這些教學課程會使用整個 WebMatrix。

## <a name="about-these-tutorials"></a>關於這些教學課程

本教學課程組是如何使用 ASP.NET Web Pages 簡介。 9 的教學課程中總共有本入門教學課程組。 它的一系列的教學課程的集合可讓您從 ASP.NET Web Pages 新手進入建立真實的專業網站的一部分。

本教學課程中第一個設定將示範如何使用 ASP.NET Web Pages 的基本概念。 當您完成時，您可以使用其中一段時間，而可深入探索網頁收取的其他教學課程集合。

我們刻意會輕鬆深入的說明。 然後每當我們顯示一些內容，本教學課程組我們一律選擇我們認為的方式是最容易了解。 稍後的教學課程集移至更深入的討論，並告訴您更有效率或更有彈性的方法 （也更有趣）。 但這些教學課程要求您先了解基本概念。

您剛啟動的教學課程集涵蓋這些功能的 ASP.NET Web Pages:

- 簡介和開始安裝的所有項目。 （這是您正在閱讀教學課程中）。
- ASP.NET Web Pages 程式設計的基本概念。
- 建立資料庫。
- 建立及處理使用者輸入的表單。
- 加入、 更新和刪除資料庫中的資料。

## <a name="what-will-you-create"></a>您會建立什麼？

本教學課程中設定，而且後續的不外乎的網站，您可以在其中列出您要的電影。 您可以輸入電影、 加以編輯，以及列出它們。 以下是您將建立在此教學課程的集合中的頁面數。 第一個顯示電影清單，您將建立的頁面：

![已電影應用程式顯示電影清單](getting-started/_static/image1.png)

而以下是可讓您將新的電影資訊新增至您的網站頁面：

![完成的電影應用程式顯示新增影片頁面](getting-started/_static/image2.png)

後續的教學課程將建置在此設定，並新增更多的功能，例如上傳圖片，讓使用者登入、 傳送電子郵件，以及與社交媒體整合。

## <a name="see-this-app-running-on-azure"></a>請參閱在 Azure 上執行此應用程式

若要查看為即時 web 應用程式正在執行的完成的網站嗎？ 您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

您需要有 Azure 帳戶才能將此解決方案部署至 Azure。 如果您還沒有帳戶，您會有下列選項：

- [免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
- [啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。

## <a name="installing-everything"></a>安裝的所有項目

您可以使用 microsoft Web Platform Installer 來安裝的所有項目。 作用中，您會安裝安裝程式，並再用它來安裝所有其他項目。

若要使用的網頁，您必須至少有 Windows XP SP3 安裝或 Windows Server 2008 或更新版本。

在 [網頁 」 頁面](../../../index.md)ASP.NET 網站中，按一下**安裝**。

![ASP.NET Web 站台顯示&quot;安裝 WebMatrix&quot;按鈕](getting-started/_static/image3.png)

系統會要求您接受這些授權條款和隱私權聲明，然後再安裝 WebMatrix。

![接受條款，以開始安裝](getting-started/_static/image4.png)

按一下 **執行**開始安裝。 (如果您想要儲存該安裝程式，請按一下**儲存**，然後從儲存位置的資料夾執行安裝程式。)

![](getting-started/_static/image5.png)

Web Platform Installer 出現時，您可以準備安裝 WebMatrix。 按一下 [安裝] 。

![](getting-started/_static/image6.png)

安裝程序找出它必須在電腦上安裝，並啟動程序。 取決於什麼完全有安裝，程序可能需要幾分鐘的時間從數分鐘的時間。 選取 **我接受**接受授權條款。

## <a name="hello-webmatrix"></a>Hello WebMatrix

完成時，安裝程序就可以自動啟動 WebMatrix。 如果沒有，在 Windows 中，從**開始**功能表，啟動**Microsoft WebMatrix**。

當您第一次啟動 WebMatrix 時，您可以使用您的 Microsoft 帳戶登入 Microsoft Azure 的機率。 登入，您會收到透過 Azure 的 10 個免費的 web 應用程式。 這些免費的 web 應用程式提供便利的方式來測試您的應用程式。 如果您還沒有 Azure 帳戶，但您需要 MSDN 訂用帳戶，您可以[啟用您的 MSDN 訂用帳戶權益](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)。 否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

您不必立即登入以繼續進行本教學課程。 如果您未登入現在，您仍然必須稍後登入選項。 上次[主題](publishing.md)在本教學課程系列會說明如何將您的網站部署至 Azure; 因此，您必須登入以完成該主題。

此時，請使用您的 Microsoft 帳戶或選取的登入**不是現在**右上角。

![登入](getting-started/_static/image7.png)

若要開始，您將建立空白的網站，並新增頁面。 在稍後的教學課程中，在此集合中，您將播放使用其中一個內建的網站範本。

在 [開始] 視窗中，按一下**新增**。

![WebMatrix 啟動螢幕](getting-started/_static/image8.png)

範本是網站的預先建置的檔案和頁面的不同類型。 若要查看所有可用的預設範本，請選取 [範本庫] 選項。

![選取範本資源庫](getting-started/_static/image9.png)

在 **快速入門**視窗中，選取**空白網站**從**ASP.NET**群組，並命名新的站台 」 WebPagesMovies"。

![WebMatrix 快速開始視窗中選取的空白網站範本](getting-started/_static/image10.png)

按 [ **下一步**]。

如果您已登入您的 Microsoft 帳戶，您將有機會在 Azure 上建立站台。 根據您的網站的預設名稱的名稱**WebPagesMovies.azurewebsites.net**建議; 不過，驚嘆號表示此名稱不是 Windows Azure 上提供。 為了簡單起見，請選取**略過**略過目前在 Azure 上建立網站。 稍後在本系列中，我們會將站台發佈至 Azure。

![建立 azure 網站](getting-started/_static/image11.png)

WebMatrix 會建立並開啟 站台：

![在 WebMatrix 中開啟新的 WebPagesMovies 站台](getting-started/_static/image12.png)

在頂端，還有快速存取工具列和功能區。 在左下方，您會看到工作區選取器切換工作 (**站台**，**檔案**，**資料庫**，**報表**)。 在右邊是 [內容] 窗格中，編輯器和報告。 和下方偶而會看到訊息的通知列。

您將進一步了解 WebMatrix 和它的功能當您瀏覽這些教學課程。

## <a name="creating-a-web-page"></a>建立的 Web 網頁

熟悉 WebMatrix 及 ASP.NET 網頁，您將建立簡單的頁面。

在 工作區選取器中，選取**檔案**工作區。 此工作區可讓您使用 檔案和資料夾。 左的窗格會顯示您的站台的檔案結構。 功能區的變更，以顯示與檔案相關的工作。

![在 WebMatrix 中的檔案工作區](getting-started/_static/image13.png)

在 功能區中，按一下 底下的箭號**的新**，然後按一下**新檔案**。

![使用&quot;新增&quot;命令在功能區中建立新的檔案](getting-started/_static/image14.png)

WebMatrix 會顯示一份檔案類型。 選取  **CSHTML**，然後在**名稱**方塊中，輸入"HelloWorld"。 CSHTML 頁面是 ASP.NET Web Pages 頁面。

![建立新的 CSHTML 頁面命名 HelloWorld.cshtml](getting-started/_static/image15.png)

按一下 [確定 **Deploying Office Solutions**]。

WebMatrix 所建立的網頁，並在編輯器中開啟。

![WebMatrix 編輯器中新的 [HelloWorld] 頁面](getting-started/_static/image16.png)

如您所見，頁面會包含大部分是一般的 HTML 標記，除了區塊上方看起來像這樣：

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

新增程式碼，因為您很快會看到。

請注意，頁面的不同部分&mdash;項目名稱、 屬性和文字，以及位於頂端的區塊 — 所有在不同的色彩。 這就叫做*語法反白顯示*，並讓您更輕鬆地清除所有項目。 它是其中一個功能，可讓您輕鬆地在 WebMatrix 中的網頁使用。

新增的內容`<head>`和`<body>`如下列範例所示的項目。 （如果您想，您可以只將下列區塊複製和使用此程式碼取代整個現有的頁面。）

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

在 快速存取工具列或在**檔案**功能表上，按一下**儲存**。

![在 WebMatrix 快速存取工具列中 [儲存] 按鈕](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>測試頁面

在 **檔案**工作區中，以滑鼠右鍵按一下*HelloWorld.cshtml*頁面，然後按一下**瀏覽器中啟動**。

![執行使用 WebMatrix 功能區中的 [執行] 按鈕的頁面](getting-started/_static/image18.png)

WebMatrix 啟動內建網頁伺服器 (IIS Express) 可供您測試您的電腦上的頁面。 （不含 IIS Express 在 WebMatrix 中，您必須將網頁發行到 web 伺服器位置之前，您可以測試它。）頁面會顯示在預設瀏覽器。

![&quot;Hello World&quot;瀏覽器中執行的頁面](getting-started/_static/image19.png)

請注意，當您在 WebMatrix 中測試頁面，在瀏覽器中的 URL 會像下面`http://localhost:33651/HelloWorld.cshtml.`名稱*localhost*指的是本機伺服器，這表示頁面由您自己的電腦上的 web 伺服器。 如所述，WebMatrix 便會包含名為 IIS Express 時啟動的頁面上所執行的 web 伺服器程式。

之後的數字*localhost* (例如*localhost:33651*) 是指*連接埠號碼*您電腦上。 這是 「 通道 」，IIS Express 會使用此特定的網站數目。 從 1024 到 65536 的範圍，當您建立您的網站，並對每個您所建立的網站不同的連接埠號碼會隨機選取。 （當您測試您自己的網站時，連接埠號碼幾乎肯定會比 33561 不同的數字。）每個網站使用不同的連接埠，IIS Express 可以直接保留其中一個站台通訊。

稍後當您將網站發佈到公開的 web 伺服器，您不會再看見*localhost*在 URL 中。 此時，您會看到更一般的 URL，例如`http://myhostingsite/mywebsite/HelloWorld.cshtml`或任何頁面。 您將進一步了解稍後在本教學課程系列中發行站台。

## <a name="adding-some-server-side-code"></a>加入一些伺服器端程式碼

關閉瀏覽器，並返回 [WebMatrix] 頁面。

程式碼區塊中加入一行，使它看起來像下列程式碼：

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

這是有點 Razor 程式碼。 它會取得目前的日期和時間，並放到該值可能清楚*變數*名為`currentDateTime`。 您將深入了解有關在下一個教學課程中的 Razor 語法。

本文的頁面上之後,`<p>Hello World!</p>`項目，新增下列：

[!code-html[Main](getting-started/samples/sample4.html)]

此程式碼取得值，您將放入`currentDateTime`頂端變數並將它插入網頁的標記。 `@`字元標示 ASP.NET Web Pages 中的程式碼頁面。

執行一次 （WebMatrix 將變更儲存為您執行頁面之前） 頁面。 此時您會看到網頁的時間與日期。

![&quot;Hello World&quot;與動態產生的時間顯示瀏覽器中執行的頁面](getting-started/_static/image20.png)

請稍候片刻，然後重新整理瀏覽器頁面。 日期和時間顯示已更新。

在瀏覽器中，了解網頁原始檔。 它看起來像下列標記：

[!code-html[Main](getting-started/samples/sample5.html)]

請注意，`@{ }`上層區塊不存在。 也請注意，日期和時間顯示的實際字元字串 (`1/18/2012 2:49:50 PM`或任何)，而非`@currentDateTime`還用像是 *.cshtml*頁面。 發生了什麼事如下，當您執行頁面時，ASP.NET 會處理所有的程式碼 （很少在此情況下） 已標`@`。 程式碼會產生輸出，以及該輸出插入至頁面。

## <a name="this-is-what-aspnet-web-pages-are-about"></a>這是 ASP.NET Web Pages 是關於

當您閱讀 ASP.NET Web Pages 會產生動態網頁內容時，您已在此看到是概念。 您剛建立的網頁包含您曾看過的同一個 HTML 標記。 它也可以包含可執行各種工作的程式碼。 在此範例中，它會執行簡單的工作，取得目前的日期和時間。 如您所見，您可以顛倒的程式碼與 HTML，以在網頁中產生輸出。 當有人要求 *.cshtml*的瀏覽器，ASP.NET 頁面處理頁面上，它仍在 web 伺服器的實際操作時。 ASP.NET 插入的程式碼的輸出 （如果有的話） 的頁面為 HTML。 程式碼處理完成時，ASP.NET 會將產生的頁面傳送至瀏覽器。 像瀏覽器的只是 HTML。 以下是圖表：

![ASP.NET 如何產生 HTML 動態的概念流程](getting-started/_static/image21.png)

這個概念十分簡單，但有許多有趣的工作可執行程式碼，而且有許多有趣的方式，在其中您可以動態將 HTML 內容新增至頁面。 和 ASP.NET *.cshtml*頁面，例如任何 HTML 頁面上，也可以包含在瀏覽器本身 （JavaScript 和 jQuery 程式碼） 中執行的程式碼。 您將探索所有在本教學課程組和後續的這些項目。

## <a name="coming-up-next"></a>接下來的下一步

在本系列中下一個教學課程中，您可以探索 ASP.NET Web Pages 程式設計稍微。

## <a name="additional-resources"></a>其他資源

[從頭開始建立 ASP.NET 網站](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)。 這是教學課程中，特別是關於使用 WebMatrix (不 ASP.NET Web Pages)。 它會進入有點一些我們不會討論在此教學課程的集合中的 WebMatrix 的其他功能的更詳細。

> [!div class="step-by-step"]
> [下一步](intro-to-web-pages-programming.md)
