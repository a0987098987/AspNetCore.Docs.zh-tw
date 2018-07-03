---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: 判斷檔案必須為部署 (C#) |Microsoft Docs
author: rick-anderson
description: 從開發環境部署至生產環境需要的檔案部分取決於 ASP.NET 應用程式建置時是否我們...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 750e2e19fdaaf2b11304b2227e7c582668d1a567
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363069"
---
<a name="determining-what-files-need-to-be-deployed-c"></a>判斷檔案必須為部署 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip)或[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> 從開發環境部署至生產環境需要的檔案部分取決於是否使用網站模型還是 Web 應用程式模型建置 ASP.NET 應用程式。 深入了解這兩個專案模型和專案模型如何影響部署。


## <a name="introduction"></a>簡介

部署 ASP.NET web 應用程式需要 ASP.NET 相關的檔案複製到生產環境的開發環境。 ASP.NET 相關的檔案包含 ASP.NET web 網頁標記和程式碼和用戶端和伺服器端支援檔案。 用戶端支援檔案是與您的 web 網頁所參考的直接傳送到瀏覽器-影像、 CSS 檔案和 JavaScript 檔案，例如那些檔案。 伺服器端支援檔案包括用來在伺服器端處理要求。 這包括組態檔、 web 服務、 類別檔案、 Typed DataSets，以及 LINQ 至 SQL 檔案，而其他項目。

一般情況下，用戶端支援的所有檔案應從開發環境都複製到生產環境中，但都複製哪些伺服器端支援檔案取決於是否您明確地編譯的伺服器端程式碼的組件 ( `.dll`檔案)，或如果您遇到自動產生這些組件。 本教學課程中，反白顯示必須明確地編譯成組件與具有會自動執行此編譯步驟的 程式碼時部署的檔案。

## <a name="explicit-compilation-versus-automatic-compilation"></a>自動編譯與明確的編譯

ASP.NET 網頁會分配到宣告式標記和來源的程式碼。 宣告式標記部分包括 HTML、 Web 控制項及資料繫結語法;程式碼部分會包含以 Visual Basic 或 C# 程式碼撰寫的事件處理常式。 標記和程式碼部分通常分成不同的檔案：`WebPage.aspx`包含的宣告式標記時`WebPage.aspx.cs`裝載程式碼。

請考慮名為包含 Label 控制項，其文字屬性設定為目前的日期和時間，當頁面載入的 Clock.aspx ASP.NET 網頁。 宣告式標記部分 (在`Clock.aspx`) 會包含 Label Web 控制項-標記`<asp:Label runat="server" id="TimeLabel" />`同時在程式碼部分 (在`Clock.aspx.cs`) 會有`Page_Load`為下列程式碼的事件處理常式：

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

為了讓 ASP.NET 引擎要求此頁面，頁面的程式碼部分 (`WebPage.aspx.cs`檔案) 必須先編譯。 明確或自動，則可能會發生這個編譯。

如果編譯會明確地發生，則整個應用程式的原始程式碼會編譯成一或多個組件 (`.dll`檔案) 位於應用程式的`Bin`目錄。 如果編譯會自動發生，則所產生的自動產生組件時，根據預設，會變成`Temporary ASP.NET`Files 資料夾，位於`%WINDOWS%\Microsoft.NET\Framework\` *&lt;版本&gt;*，雖然這個位置是可透過設定[`<compilation>`項目](https://msdn.microsoft.com/library/s10awwz0.aspx)在`Web.config`。 使用明確的編譯中，您必須採取某些動作到 ASP.NET 應用程式的程式碼編譯成組件，並在部署之前，會發生這個步驟。 自動編譯與編譯程序，就會發生在 web 伺服器上時第一次存取資源。

不論哪一種編譯模型使用，所有的 ASP.NET 網頁的標記部分 (`WebPage.aspx`檔案) 必須複製到生產環境。 使用明確的編譯，您需要註冊中的組件複製`Bin`資料夾，但您不需要複製設定 ASP.NET 網頁的程式碼部分 (`WebPage.aspx.cs`檔案)。 自動編譯與您要複製的程式碼的部分檔案，以便將程式碼存在，且當瀏覽網頁時可自動編譯。 每個 ASP.NET 網頁的標記部分包含`@Page`與屬性，指出是否已明確編譯網頁的相關聯的程式碼，或者是否必須自動編譯指示詞。 如此一來，生產環境可以緊密地與其中一種編譯模型，您不需要套用任何特殊組態設定，以表示用於明確或自動編譯。

表 1 摘要說明不同的檔案時使用明確的編譯，自動編譯與部署。 請注意，不論編譯模型您應該一律會部署組件`Bin`資料夾中，如果該資料夾存在。 `Bin`資料夾包含的組件特定的 web 應用程式中，使用明確的編譯模型時，包含已編譯的原始程式碼。 `Bin`目錄也包含其他專案中的組件和任何開放原始碼或協力廠商組件則可以使用，而且這些需要為實際執行伺服器上。 因此，一般的經驗法則，複製`Bin`一直到生產階段部署時的資料夾。 (如果您使用自動編譯模型，而且未使用任何外部組件則不會有`Bin`目錄-沒關係 ！)

| **編譯模型** | **部署標記部分檔案嗎？** | **部署原始程式碼檔嗎？** | **部署中的組件`Bin`目錄嗎？** |
| --- | --- | --- | --- |
| 明確的編譯 | [是] | 否 | [是] |
| 自動編譯 | [是] | [是] | 是 （如果存在的話） |

**表 1:** 部署哪些檔案取決於所使用的編譯模型。

## <a name="taking-a-trip-down-memory-lane"></a>採取下記憶體通道一趟車程

會使用何種編譯方法，部分取決於，在 Visual Studio 中管理 ASP.NET 應用程式的方式。 自。NET 的起始於 2000 年，有四個不同版本的 Visual Studio-Visual Studio.NET 2002年、 Visual Studio.NET 2003年中，Visual Studio 2005 和 Visual Studio 2008。 Visual Studio.NET 2002年和 2003年管理 ASP.NET 應用程式使用*Web 應用程式專案模型*。 Web 應用程式專案模型的主要功能如下：

- 專案結構會定義單一專案檔中的檔案。 未在專案檔中定義的任何檔案不會考慮 Visual Studio web 應用程式的一部分。
- 使用明確的編譯。 建置專案將專案中的程式碼檔案編譯成單一組件放入`Bin`資料夾。

Microsoft 發行的 Visual Studio 2005 時它們會卸除對 Web 應用程式專案模型的支援，並取代為網站專案模型。 網站專案模型區別本身的 Web 應用程式專案模型如下：

- 而不需要詳細說明的專案檔案的單一專案檔案，檔案系統就會改用。 簡單地說，web 應用程式資料夾 （或子資料夾） 中的任何檔案會被視為專案的一部分。
- 建置 Visual Studio 中的專案不會建立中的組件`Bin`目錄。 相反地，建置網站專案報告任何編譯時期錯誤。
- 自動編譯支援。 網站專案通常會部署，將標記和來源的程式碼複製到生產環境中，雖然程式碼可以是先行編譯 （明確編譯）。

發行 Visual Studio 2005 Service Pack 1 時，Microsoft 就會恢復為 Web 應用程式專案模型。 不過，Visual Web Developer 持續為僅支援網站專案模型。 好消息是，這項限制已卸除 Visual Web Developer 2008 Service Pack 1。 現在，您可以在 Visual Studio （和 Visual Web Developer） 使用 Web 應用程式專案模型或網站專案模型中建立 ASP.NET 應用程式。 這兩個模型各有其優缺點。 請參閱[Web 應用程式專案簡介： 比較網站專案和 Web Application Projects](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5)比較兩個模型，並可協助您判斷哪一種專案模型最適合您的情況。

## <a name="exploring-the-sample-web-application"></a>探索範例 Web 應用程式

本教學課程中下載包含一個稱為書籍評論的 ASP.NET 應用程式。 網站會模擬使用者可能會建立的嗜好網站分享他們的著作中，檢閱與線上社群。 此 ASP.NET web 應用程式是非常簡單，而且包含下列資源：

- `Web.config`應用程式的組態檔。
- 主版頁面 (`Site.master`)。
- 七個不同的 ASP.NET 頁面： 

    - ~`/Default.aspx`-網站首頁。
    - ~`/About.aspx` -「 關於網站 」 頁面。
    - ~`/Fiction/Default.aspx` -列出已檢閱的故事書籍的頁面。 

        - ~`/Fiction/Blaze.aspx` -檢閱 Richard Bachman novel *Blaze*。
    - ~/`Tech/Default.aspx` -列出已檢閱的技術書籍的頁面。 

        - ~/`Tech/CYOW.aspx`-檢閱*建立您自己的網站*。
        - ~/`Tech/TYASP35.aspx` -檢閱*教導您自己 ASP.NET 3.5 24 小時內*。
- 三個不同 CSS 檔案 [Styles] 資料夾中。
- 四個影像檔-提供的 ASP.NET 標誌和映像的三個檢閱書本的封面-所有位於`Images`資料夾。
- A`Web.sitemap`檔案，其中定義網站地圖和用來顯示在功能表`Default.aspx`根目錄中的頁面並`Fiction`和`Tech`資料夾。
- 名為的類別檔案`BasePage.cs`定義基底`Page`類別。 此類別會擴充功能`Page`類別會自動設定`Title`屬性，根據在網站導覽中的頁面的位置。 簡單的說，任何的 ASP.NET 程式碼後置類別延伸`BasePage`(而不是`System.Web.UI.Page`) 會有其值設為根據其位置在網站導覽中的標題。 比方說，當檢視 ~ /`Tech/CYOW.aspx`  頁面上，標題設為"首頁:: 技術:: 建立您自己的網站 」。

[圖 1] 顯示透過瀏覽器檢視時的書籍評論網站的螢幕擷取畫面。 這裡您會看到頁面 ~ /`Tech/TYASP35.aspx`，這會檢閱本書*教導您自己 ASP.NET 3.5 24 小時內*。 跨越頁面和左側的資料行中的功能表的頂端階層連結為基礎網站導覽結構中定義`Web.sitemap`。 在右上角的影像是其中一個映像位於書封面`Images`資料夾。 網站的外觀及操作定義透過階層式樣式表的規則而為名的頁面配置定義在主版頁面中，[Styles] 資料夾中的 CSS 檔案所拼出`Site.master`。


[![活頁簿會檢閱網站提供各式各樣的產品評論](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**圖 1:** 活頁簿會檢閱網站提供各式各樣的產品評論 ([按一下以檢視完整大小的影像](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


此應用程式不會使用資料庫;每個檢閱會實作為個別的網頁應用程式中。 本教學課程 （及下一步 的數個教學課程） 逐步部署不需要資料庫的 web 應用程式。 不過，在未來的教學課程中我們將增強儲存評論、 讀者提供任何意見，以及其他資訊在資料庫內，此應用程式，並將探索需要執行，將正確的資料驅動 web 應用程式的步驟。

> [!NOTE]
> 這些教學課程著重於裝載 ASP.NET 應用程式與 web 主機提供者，而不會探索附屬的主題，例如 ASP。NET 的站台對應系統或使用基底`Page`類別。 如需有關這些技術，以及在整個教學課程所涵蓋的其他主題的詳細背景，請在每個教學課程結尾處指進一步閱讀 > 一節。


本教學課程下載具有 web 應用程式的兩個複本，每一個實作為不同的 Visual Studio 專案類型： BookReviewsWAP、 Web 應用程式專案和 BookReviewsWSP，網站專案。 這兩個專案使用 Visual Web Developer 2008 SP1 所建立，並使用 ASP.NET 3.5 SP1。 若要使用這些專案開始解壓縮到您的桌面內容。 若要開啟 Web 應用程式專案 (BookReviewsWAP)，瀏覽至 BookReviewsWAP 資料夾，按兩下方案檔、 `BookReviewsWAP.sln`。 若要開啟的網站專案 (BookReviewsWSP)，啟動 Visual Studio 然後，從 [檔案] 功能表中，選擇 [開啟網站] 選項，瀏覽至`BookReviewsWSP`資料夾在桌面上，按一下 [確定]。


在本教學課程的看看哪些檔案中剩餘的兩個區段必須部署應用程式時，將複製到實際執行環境。 下面兩個教學課程- *[部署您的網站使用 FTP](deploying-your-site-using-an-ftp-client-cs.md)* 並*[部署您的網站使用的 Visual Studio](deploying-your-site-using-visual-studio-cs.md)*  -顯示不同的方式將這些檔案複製到 web 主機提供者。

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>決定要為 Web 應用程式專案部署檔案

Web 應用程式專案模型使用明確的編譯-專案的原始程式碼會編譯成單一組件每次建置應用程式。 此編譯包含 ASP.NET 頁面的程式碼後置檔案 (~ /`Default.aspx.cs`，~ /`About.aspx.cs`等等)，以及`BasePage.cs`類別。 產生的組件名為 BookReviewsWAP.dll 且位於應用程式的`Bin`目錄。

[圖 2] 顯示書籍評論 Web 應用程式專案所組成的檔案。


[![[方案總管] 列出構成 Web 應用程式專案的檔案](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**圖 2**: 方案總管 列出構成 Web 應用程式專案的檔案


若要部署使用的 Web 應用程式專案模型開始建置的應用程式以明確編譯為組件的最新的原始碼開發 ASP.NET 應用程式。 接下來，將下列檔案複製到生產環境：

- 檔案包含的宣告式標記每個 asp.net 網頁，例如 ~ /`Default.aspx`，~ /`About.aspx`，依此類推。 此外，任何主版頁面和使用者控制項的宣告式標記設定複本。
- 組件 (`.dll`檔案) 中`Bin`資料夾。 您不需要的程式資料庫檔案複製 (`.pdb`)，或任何 XML 檔案，您可能會發現在`Bin`目錄。

您不需要將 ASP.NET 頁面的原始程式碼檔複製到生產環境中，也不需要複製`BasePage.cs`類別檔案。

> [!NOTE]
> 如 圖 2 所示`BasePage`類別會實作為在專案中，放在名為的資料夾中的類別檔案`HelperClasses`。 當編譯專案中的程式碼`BasePage.cs`檔案以及 ASP.NET 頁面的程式碼後置類別編譯成單一組件中， `BookReviewsWAP.dll.` ASP.NET 具有名為的特殊資料夾`App_Code`，設計用來保存適用於 Web 的類別檔案站台的專案。 中的程式碼`App_Code`資料夾就會自動編譯，因此不應搭配 Web 應用程式專案。 相反地，您應該將您的應用程式類別檔案，名為的正常資料夾置於`HelperClasses`，或`Classes`，或其他類似。 或者，您可以將類別檔案放在個別的類別庫專案。


除了將 ASP.NET 相關的標記檔案和中的組件複製`Bin`資料夾中，您也需要將用戶端支援檔案複製-映像和 CSS 檔案-其他伺服器端支援檔案，以及`Web.config`和`Web.sitemap`。 這些用戶端和伺服器端必須支援檔案複製到實際執行環境，不論您是使用明確或自動編譯。

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>決定要部署之網站專案檔案的檔案

網站專案模型支援自動編譯、 使用 Web 應用程式專案模型時，不提供的功能。 使用明確的編譯必須編譯為組件專案的原始程式碼，並將該組件複製到實際執行環境。 相反地，自動編譯與您只是複製原始程式碼到生產環境，並視需要編譯視執行階段。

在 Visual Studio 中的 [建置] 功能表選項會出現在 Web 應用程式專案和網站專案項目。 建置 Web 應用程式專案將專案的原始程式碼編譯成單一組件位於`Bin`目錄; 建置網站專案會檢查是否有任何編譯時期錯誤，但不會建立任何組件。 若要部署 ASP.NET 應用程式使用的網站專案模型開發所有您要做為複製實際執行環境中，適當的檔案，但我會建議您第一次建置專案，以確保沒有任何編譯時期錯誤。

圖 3 顯示構成書籍評論網站專案的檔案。


 [![[方案總管] 會列出組成的網站專案的檔案](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**圖 3**: 方案總管 會列出組成的網站專案的檔案


部署網站專案時，需要將所有的 ASP.NET 相關的檔案複製到實際執行的環境，其中包含 ASP.NET 網頁、 主版頁面和使用者控制項的標記頁面，以及其程式碼檔案。 您也需要將任何類別檔案，例如 BasePage.cs 複製。 請注意，`BasePage.cs`檔案會位於`App_Code`是特殊的 ASP.NET 資料夾中的網站專案使用的類別檔案的資料夾。 特殊資料夾必須與中的類別檔案建立在生產環境，也`App_Code`開發環境上的資料夾必須複製到`App_Code`在實際執行上的資料夾。

除了 ASP.NET 標記和來源的程式碼檔案複製，您也需要將用戶端支援檔案-映像和 CSS 檔案-其他伺服器端支援檔案，以及`Web.config`和`Web.sitemap`。

> [!NOTE]
> 網站專案也可以使用明確的編譯。 未來的教學課程將探討如何明確地編譯的網站專案。


## <a name="summary"></a>總結

部署 ASP.NET 應用程式需要必要的檔案複製到生產環境的開發環境。 需要進行同步處理的檔案，一組精確取決於是否 ASP.NET 應用程式的編譯程式碼明確或自動。 設定 Visual Studio 是否會影響編譯策略來管理 ASP.NET 應用程式使用 Web 應用程式專案模型或網站專案模型。

Web 應用程式專案模型使用明確的編譯，並將專案的程式碼編譯成單一組件中`Bin`資料夾。 部署應用程式、 ASP.NET 網頁的標記部分的內容時`Bin`資料夾必須被推送至生產環境，不需要-程式碼檔案和程式碼後置類別，例如-應用程式中的原始程式碼要複製到實際執行環境。

網站專案模型預設會使用自動編譯，雖然您可以明確地編譯的網站專案中，我們在未來將會看到教學課程。 使用自動編譯 ASP.NET 應用程式部署需要的標記部分*和*原始程式碼必須複製到實際執行環境。 會在第一次要求時程式碼將會自動在實際執行環境中編譯。

現在，我們檢驗了開發和生產環境之間進行同步處理需要的檔案我們準備好要部署到 web 主機提供者的書籍評論應用程式。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 編譯概觀](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET 使用者控制項](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [正在檢查 ASP。NET 的網站巡覽](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web 應用程式專案簡介](https://msdn.microsoft.com/library/aa730880.aspx)
- [主版頁面教學課程](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [頁面之間共用程式碼](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [使用自訂的基底類別的 ASP.NET 頁面的程式碼後置類別](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005 的網站專案系統： 這是什麼，以及為何我們做了它嗎？](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [逐步解說： 將網站專案轉換成 Visual Studio 中的 Web 應用程式專案](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [上一頁](asp-net-hosting-options-cs.md)
> [下一頁](deploying-your-site-using-an-ftp-client-cs.md)
