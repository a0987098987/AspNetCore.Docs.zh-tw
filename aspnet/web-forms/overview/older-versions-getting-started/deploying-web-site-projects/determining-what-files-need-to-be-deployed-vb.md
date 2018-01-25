---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: "判斷檔案必須為部署 (VB) |Microsoft 文件"
author: rick-anderson
description: "需要從開發環境部署到生產環境的檔案部分取決於 ASP.NET 應用程式建置時是否我們..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: aad0d4d4f7db5942c51255c34f36be73ed0e1f2d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>判斷檔案必須為部署 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> 需要從開發環境部署到生產環境的檔案部分取決於 ASP.NET 應用程式建置時是否使用的網站模式或 Web 應用程式模型。 深入了解這兩個專案模型以及如何為專案模型影響部署。


## <a name="introduction"></a>簡介

ASP.NET web 應用程式部署需要 ASP.NET 相關的檔案複製到生產環境的開發環境。 ASP.NET 相關的檔案包含 ASP.NET web 網頁標記和程式碼和用戶端和伺服器端支援檔案。 用戶端的支援檔案是與您的網頁所參考的直接傳送至瀏覽器-影像、 CSS 檔案和 JavaScript 檔案，例如那些檔案。 伺服器端支援檔案包括用來在伺服器端處理要求。 這包括組態檔、 web 服務、 類別檔案、 輸入資料集和 LINQ SQL 檔案，和其他項目。

一般而言，所有用戶端的支援檔案應從開發環境都複製到生產環境，但是都複製哪些伺服器端支援檔案取決於明確編譯伺服器端程式碼的組件 ( `.dll`檔案)，或如果您有自動產生這些組件。 本教學課程中，反白顯示需要明確編譯為組件與具有此編譯步驟，就會發生自動程式碼時即將部署的檔案。

## <a name="explicit-compilation-versus-automatic-compilation"></a>自動編譯與明確編譯

ASP.NET web pages 劃分為宣告式標記和原始程式碼的程式碼。 宣告式標記部分包含 HTML、 Web 控制項和資料繫結語法。程式碼部分會包含以 Visual Basic 或 C# 程式碼撰寫事件處理常式。 標記和程式碼的某些部分通常分成不同的檔案：`WebPage.aspx`包含的宣告式標記時`WebPage.aspx.vb`裝載程式碼。

名為 ASP.NET 網頁，請考慮`Clock.aspx`包含 Label 控制項，其文字屬性設定為目前的日期和時間載入頁面。 宣告式標記部分 (在`Clock.aspx`) 會包含標記標籤 Web 控制項-以`<asp:Label runat="server" id="TimeLabel" />`-時的程式碼部分 (在`Clock.aspx.vb`) 會有`Page_Load`事件處理常式，以下列程式碼：

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

為了讓 ASP.NET 引擎服務的這個頁面上，在頁面的程式碼部分的要求 (  *`WebPage`*  `.aspx.vb`檔案) 必須先編譯。 明確或自動，則可能會發生這個編譯。

如果編譯會明確地發生，則整個應用程式的原始程式碼會編譯成一或多個組件 (`.dll`檔案) 位於應用程式的`Bin`目錄。 如果編譯會自動發生，則所產生的自動產生組件時，根據預設，會變成 「`Temporary ASP.NET Files`資料夾，請參閱`%WINDOWS%\Microsoft.NET\Framework\<version>`，不過這個位置可透過設定[ &lt;編譯&gt;元素](https://msdn.microsoft.com/library/s10awwz0.aspx)中`Web.config`。 使用明確編譯，您必須採取某些動作到 ASP.NET 應用程式的程式碼編譯成組件，並在部署之前，會發生這個步驟。 使用自動編譯編譯處理程序，就會發生在 web 伺服器上第一次存取資源時。

不論哪一種編譯模型使用，所有的 ASP.NET 網頁的標記部份 (`WebPage.aspx`檔案) 必須複製到生產環境。 您需要將複製的組件明確編譯`Bin`資料夾中，但是您不需要將複製的 ASP.NET 網頁程式碼某些部分 (`WebPage.aspx.vb`檔案)。 使用自動編譯，您需要將複製程式碼部分檔案，因此程式碼存在，且當瀏覽網頁時可以自動編譯。 每個 ASP.NET 網頁的標記部分包含`@Page`與屬性，指出是否已明確編譯網頁的相關聯的程式碼，或是否需要自動編譯指示詞。 如此一來，在實際執行環境可以緊密地與兩種模式中編譯，您不需要套用任何特殊的組態設定，以指出使用明確或自動編譯。

表 1 摘要說明不同的檔案時使用明確編譯自動編譯和部署。 請注意，不論編譯模型使用您一律將部署中的組件`Bin`資料夾中，則該資料夾存在。 `Bin`資料夾包含組件特定的 web 應用程式，可使用明確編譯模型時，包含已編譯的原始程式碼。 `Bin`目錄也會包含從其他專案的組件和任何您可能使用的開放原始碼或協力廠商組件，這些必須實際執行伺服器上。 因此，一般的經驗法則，複製`Bin`到實際執行部署時的資料夾。 (如果您使用自動編譯模型，而且未使用任何外部組件則不需要`Bin`目錄的 [確定]，！)

| **編譯模型** | **將部署標記的部分檔案嗎？** | **將原始程式碼檔的部署嗎？** | **部署中的組件`Bin`目錄嗎？** |
| --- | --- | --- | --- |
| 明確編譯 | [是] | 否 | [是] |
| 自動編譯 | [是] | [是] | 是 （如果存在的話） |

**表 1： 部署哪些檔案取決於所使用的編譯模型。**

## <a name="taking-a-trip-down-memory-lane"></a>取得記憶體通道關閉路線

使用何種編譯方法，部分取決於，Visual Studio 中管理 ASP.NET 應用程式的方式。 因為。網路的起始於 2000 年，已有四種不同版本的 Visual Studio-Visual Studio.NET 2002年、 Visual Studio.NET 2003年中，Visual Studio 2005 中和 Visual Studio 2008。 Visual Studio.NET 2002年和 2003年管理 ASP.NET 應用程式使用*Web 應用程式專案*模型。 Web 應用程式專案模型的主要功能如下：

- 架構專案會定義在單一專案檔中的檔案。 專案檔中未定義任何檔案不會考慮 Visual Studio web 應用程式的一部分。
- 使用明確的編譯。 建置專案將在專案中的程式碼檔案編譯成單一組件放入`Bin`資料夾。

當 Microsoft 發行 Visual Studio 2005 它們會卸除對 Web 應用程式專案模型的支援，並取代為網站專案模型。 網站專案模型區別來自*Web 應用程式專案*模型如下：

- 而不是專案檔會列出在單一專案檔案，檔案系統會使用。 簡單地說，web 應用程式資料夾 （或子資料夾） 內的任何檔案視為專案的一部分。
- 建置 Visual Studio 中的專案不會建立組件中的`Bin`目錄。 相反地，建置網站專案報告任何編譯時間錯誤。
- 自動編譯支援。 通常會部署的網站專案會將標記和原始程式碼的程式碼複製到生產環境，雖然程式碼可以是先行編譯 （明確編譯）。

發行 Visual Studio 2005 Service Pack 1 時，Microsoft 會恢復 Web 應用程式專案模型。 不過，Visual Web Developer 中繼續執行至僅支援網站專案模型。 好消息是這項限制已遭丟棄，Visual Web Developer 2008 Service Pack 1。 現在您可以建立 ASP.NET 應用程式在 Visual Studio （和 Visual Web Developer） 使用 Web 應用程式專案模式的網站專案。 這兩個模型會有其優缺點。 請參閱[簡介 Web 應用程式專案： 比較網站專案和 Web 應用程式專案](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5)比較的兩個模型，可協助您判斷哪一種專案模型最適合您的情況。

## <a name="exploring-the-sample-web-application"></a>瀏覽範例 Web 應用程式

在此教學課程下載包含稱為書籍評論的 ASP.NET 應用程式。 網站會模擬人可能會建立一個嗜好網站共用其活頁簿中，檢閱與線上社群。 這個 ASP.NET web 應用程式是非常簡單，而且包含下列資源：

- `Web.config`應用程式的組態檔。
- 主版頁面 (`Site.master`)。
- 七個不同的 ASP.NET 頁面：

    - ~/`Default.aspx`-網站首頁。
    - ~/`About.aspx`-「 相關網站 」 頁面。
    - ~/`Fiction/Default.aspx`-列出已檢閱小說書籍的頁面。

        - ~/`Fiction/Blaze.aspx`-檢閱 Richard Bachman novel *Blaze*。
    - ~/`Tech/Default.aspx`-列出已檢閱技術書籍的頁面。

        - ~/`Tech/CYOW.aspx`-檢閱*建立您自己的網站*。
        - ~/`Tech/TYASP35.aspx`-檢閱*教導您自己 ASP.NET 3.5 24 小時內*。
- 三個不同 CSS 檔案中的`Styles`資料夾。
- 四個影像檔-提供的 ASP.NET 標誌和映像的過程中的三個檢閱書籍-所有位於`Images`資料夾。
- A`Web.sitemap`檔案，此站台對應會定義用來顯示功能表中的檔案`Default.aspx`的根目錄中的頁面和`Fiction`和`Tech`資料夾。
- 類別檔案命名為`BasePage.vb`定義基底`Page`類別。 這個類別會擴充功能的`Page`類別藉由自動設定`Title`屬性根據網站導覽中頁面的位置。 簡單地說，任何 ASP.NET 程式碼後置類別延伸`BasePage`(而不是`System.Web.UI.Page`) 會有標題設為值根據其站台地圖中的位置。 比方說，當檢視 ~ /`Tech/CYOW.aspx`  頁面上，設定標題為 「 首頁:: 技術:: 建立您自己的網站 」。

圖 1 顯示書籍檢閱網站透過瀏覽器檢視時的螢幕擷取畫面。 這裡您會看到頁面 ~ / Tech/TYASP35.aspx，這會檢閱本書*教導您自己 ASP.NET 3.5 24 小時內*。 跨越頁面左側的資料行中的功能表的頂端，階層連結列會根據網站導覽結構中定義`Web.sitemap`。 在右下角的影像是其中一個映像位於封面`Images`資料夾。 透過階層式樣式表規則拼寫的 CSS 檔案中所定義網站的外觀及操作`Styles`資料夾，而在主版頁面中，定義為名的頁面配置`Site.master`。


[![活頁簿會檢閱網站提供檢閱上的項目分類](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**圖 1**: 書籍會檢閱網站提供檢閱上的項目分類 ([按一下以檢視完整大小的影像](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


此應用程式不會使用資料庫;每個檢閱會實作為個別的網頁應用程式中。 本教學課程 （和 下一步的幾個教學課程） 逐步部署不需要資料庫的 web 應用程式。 不過，未來教學課程中我們加強儲存評論、 讀取器的註解，以及其他資訊在資料庫內，此應用程式，並將探索必須執行才能正確部署的資料驅動的 web 應用程式的步驟。

> [!NOTE]
> 這些教學課程著重於裝載 ASP.NET 應用程式與 web 主機提供者，而不會探索像 ASP 輔助主題。網路的站台對應或系統使用基底類別。 如需有關這些技術，以及更多背景上的其他主題涵蓋在本教學課程，請參閱進一步閱讀區段結尾的每一個教學課程。


本教學課程下載 web 應用程式的兩個複本，每個實作為不同的 Visual Studio 專案類型： BookReviewsWAP、 Web 應用程式專案，以及 BookReviewsWSP，網站專案。 這兩個專案使用 Visual Web Developer 2008 SP1 所建立，並使用 ASP.NET 3.5 SP1。 若要使用這些專案先解壓縮到您的桌面內容。 若要開啟 Web 應用程式專案 (BookReviewsWAP)，瀏覽至`BookReviewsWAP`資料夾，按兩下方案檔`BookReviewsWAP.sln`。 若要開啟的網站專案 (BookReviewsWSP)，啟動 Visual Studio，然後從 [檔案] 功能表中，選擇開啟網站，瀏覽至`BookReviewsWSP`資料夾在桌面上，按一下 [確定]。


在此教學課程查看哪些檔案，您必須部署應用程式時，複製到實際執行環境中其餘的兩個區段。 下面兩個教學課程- [*部署您的網站使用 FTP* ](deploying-your-site-using-an-ftp-client-vb.md)和[*部署您的網站使用 Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -顯示不同的方式將這些檔案複製到 web 主機提供者。

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>決定要部署 Web 應用程式專案的檔案

Web 應用程式專案模型會使用明確的編譯-專案的原始程式碼會編譯到單一組件每次建置應用程式。 此編譯包含 ASP.NET 網頁的程式碼後置檔案 (~ /`Default.aspx.vb`，~ /`About.aspx.vb`等等)，並將`BasePage.vb`類別。 產生的組件名為`BookReviewsWAP.dll`且其位於應用程式的`Bin`目錄。

圖 2 顯示組成書籍檢閱 Web 應用程式專案的檔案。


[![[方案總管] 列出構成 Web 應用程式專案的檔案。](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**圖 2**: [方案總管] 列出構成 Web 應用程式專案的檔案


> [!NOTE]
> 如圖 2 所示，ASP.NET 網頁的程式碼後置檔案不會顯示在 [方案總管] 中 Visual Basic Web 應用程式專案。 若要檢視網頁的程式碼後置類別，以滑鼠右鍵按一下方案總管] 中的頁面上，選擇 [檢視程式碼。


若要部署 ASP.NET 應用程式所建立的應用程式，以明確編譯為組件的最新的原始程式碼中使用 Web 應用程式專案模型開始開發。 接下來，將下列檔案複製到生產環境：

- 檔案的宣告式標記包含每個 asp.net 頁面上，例如 ~ /`Default.aspx`，~ /`About.aspx`，依此類推。 此外，任何主版頁面和使用者控制項的宣告式標記上複製。
- 組件 (`.dll`檔案) 中`Bin`資料夾。 您不需要的程式資料庫檔案複製 (`.pdb`) 或您可以在中找到的任何 XML 檔案`Bin`目錄。

您不需要將 ASP.NET 網頁的原始程式檔複製到生產環境，也不需要複製`BasePage.vb`類別檔案。

> [!NOTE]
> 如圖 2 所示，`BasePage`類別實作為類別檔案，在專案中，放置在名為資料夾`HelperClasses`。 當編譯專案中的程式碼`BasePage.vb`檔案的 ASP.NET 網頁程式碼後置類別一起編譯成單一組件， `BookReviewsWAP.dll`。 ASP.NET 具有名為的特殊資料夾`App_Code`，設計來保存網站專案的類別檔案。 中的程式碼`App_Code`資料夾就會自動編譯，因此不應與 Web 應用程式專案。 相反地，您應該將您的應用程式類別檔案，在名為一般資料夾`HelperClasses`，或`Classes`，或其他類似。 或者，您可以將類別檔案放在個別的類別庫專案。


除了 ASP.NET 相關的標記檔案和組件中的複製`Bin`資料夾中，您也需要將用戶端的支援檔案的映像和 CSS 檔案-複製的其他伺服器端支援的檔案，以及`Web.config`和`Web.sitemap`。 這些用戶端和伺服器端必須支援檔案複製到實際執行環境，不論您是使用明確或自動編譯。

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>決定要部署的網站專案檔案的檔案

網站專案模型支援自動編譯時，使用 Web 應用程式專案的模型時，不提供的功能。 使用明確編譯必須編譯為組件的專案的原始程式碼，並將該組件複製到實際執行環境。 相反地，使用自動編譯您直接複製原始碼到生產環境，並視需要編譯視執行階段。

在 Visual Studio 中的 [建置] 功能表選項會出現在 Web 應用程式專案和網站專案。 建立 Web 應用程式專案將專案的原始碼編譯成單一組件位於`Bin`目錄; 建置網站專案會檢查是否有任何編譯時間錯誤，但不建立任何組件。 若要部署 ASP.NET 應用程式使用的網站專案模型所有您只需要開發會複製到生產環境中，適當的檔案，但我會建議您第一次建置專案，以確保沒有任何編譯時間錯誤。

圖 3 顯示組成書籍檢閱網站專案的檔案。


[![[方案總管] 列出構成的網站專案的檔案。](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**圖 3**: [方案總管] 列出構成的網站專案的檔案


部署的網站專案時，需要將所有的 ASP.NET 相關的檔案複製到生產環境層，其中包含 ASP.NET 網頁、 主版頁面和使用者控制項的標記頁面，以及其程式碼檔案。 您也需要複製任何類別檔案，例如`BasePage.vb`。 請注意，`BasePage.vb`檔案位於`App_Code`是特殊的 ASP.NET 資料夾中的網站專案使用的類別檔案的資料夾。 特殊資料夾必須與類別檔案中建立，實際執行`App_Code`必須在開發環境中的資料夾複製到`App_Code`實際執行上的資料夾。

除了複製 ASP.NET 標記和原始程式碼的程式碼檔案，您也需要將用戶端的支援檔案-映像和 CSS 檔案-複製的其他伺服器端支援的檔案，以及`Web.config`和`Web.sitemap`。

> [!NOTE]
> 網站專案也可以使用明確編譯。 未來的教學課程將會檢驗如何明確編譯的網站專案。


## <a name="summary"></a>總結

部署 ASP.NET 應用程式牽涉到從開發環境的必要檔案複製到實際執行環境。 精確的集合進行同步處理所需要的檔案取決於是否 ASP.NET 應用程式的程式碼明確或自動編譯。 採用的編譯策略會受到是否設定 Visual Studio 來管理 ASP.NET 應用程式使用 Web 應用程式專案模型或為網站專案模型。

Web 應用程式專案模型會使用明確編譯，並將專案的程式碼編譯成單一組件中`Bin`資料夾。 部署應用程式、 ASP.NET 網頁的標記一部分的內容時`Bin`資料夾必須推送到生產環境，不需要的程式碼檔案和程式碼後置類別，如範例-應用程式中的原始程式碼要複製到實際執行環境。

網站專案模型會根據預設，使用自動編譯，雖然可以明確編譯網站專案中，我們在未來將會看到教學課程。 使用自動編譯 ASP.NET 應用程式部署需要的標記部分*和*原始程式碼必須複製到實際執行環境。 會在第一次要求時，程式碼會自動編譯在實際執行環境。

現在，我們檢驗了開發和生產環境之間進行同步處理需要的檔案我們準備要部署活頁簿檢閱應用程式到 web 主機提供者。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 編譯概觀](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET 使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [正在檢查 ASP。網路的站台瀏覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web 應用程式專案的簡介](https://msdn.microsoft.com/library/aa730880.aspx)
- [主版頁面教學課程](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [頁面之間共用程式碼](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [ASP.NET 網頁的程式碼後置類別中使用自訂的基底類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005 的網站專案系統： 它是什麼，以及未我們為什麼它嗎？](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [逐步解說： 將網站專案轉換成 Visual Studio 中的 Web 應用程式專案](https://msdn.microsoft.com/library/aa983476.aspx)

>[!div class="step-by-step"]
[上一頁](asp-net-hosting-options-vb.md)
[下一頁](deploying-your-site-using-an-ftp-client-vb.md)
