---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET 裝載選項 (C#) |Microsoft 文件
author: rick-anderson
description: ASP.NET web 應用程式通常設計、 建立，在本機開發環境中測試和要部署到實際執行環境 o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f8bb0e5a34d84d448af56285e8761c447229f7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888506"
---
<a name="aspnet-hosting-options-c"></a>ASP.NET 裝載選項 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> ASP.NET web 應用程式通常會設計、 建立，而且測試在本機開發環境，然後準備發行之後要部署到生產環境的需要。 本教學課程提供的部署程序的高階概觀，並做為這個教學課程系列的簡介。


## <a name="introduction"></a>簡介

Web 應用程式通常會設計、 建立，而且只有在網站上使用的程式設計人員能夠存取的開發環境中測試過。 準備好要發行應用程式之後，便會移至生產環境位置站台可以存取網際網路上的任何人。 此部署程序導入了一些挑戰：

- 實際執行環境中必須存在且可正確地安裝程式才可以部署 ASP.NET 應用程式。此外，您必須在實際執行環境保持為最新狀態的最新安全性修補程式。
- 一組正確的標記檔案、 程式碼檔案和支援檔案必須從開發環境複製到實際執行環境。 若是資料驅動的應用程式，這可能需要複製資料庫結構描述和 （或） 資料，以及。
- 可能有兩個環境的設定差異。 使用的資料庫連接字串或電子郵件伺服器在開發環境中可能會不同於實際執行環境。 不僅如此，應用程式的行為可能會取決於環境。 例如，開發工作中發生錯誤時的錯誤詳細資料可以顯示在畫面上，但在生產環境中發生錯誤時，相反地，應該顯示使用者易記的錯誤頁面和錯誤詳細資料以電子郵件傳送給開發人員。

提供設定和維護實際執行環境中為第一個驗證題目許多個人和企業外包其實際執行環境*web 主控提供者*。 Web 主控提供者會管理實際執行環境，代替您的公司。 有無數的 web 主機提供者，各有不同的價格和服務層級;請參閱 「 尋找 Web 主機提供者 」 上尋找這類的服務提供者的提示。

這是一系列查看部署至生產環境 web 主機提供者所管理的 ASP.NET web 應用程式所需的步驟的教學課程中的第一個。 透過這些教學課程的課程，我們將檢查：

- 檔案必須部署到 web 主機提供者。
- 簡化部署程序的工具。
- 如何部署資料庫。
- 部署使用的資料庫的秘訣[SQL 為基礎的成員資格和角色提供者](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)，以及模仿實際執行環境中的 網站管理工具。
- 順暢地在開發期間所做的變更更新生產資料庫的策略。
- 記錄錯誤發生在實際執行環境，以及發生錯誤時通知開發人員的方法上的技術。

這些教學課程被專很簡潔，並提供具有足夠的螢幕擷取畫面來引導您完成程序以視覺化方式的逐步指示。 本教學課程中我尋找 web 主控提供者提供 ASP.NET 部署程序和建議的概觀。 讓我們開始吧 ！

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET 部署程序概觀

簡而言之，ASP.NET 應用程式部署包含下列三個步驟：

1. 在生產環境中設定 web 應用程式、 web 伺服器和資料庫。
2. ASP.NET 網頁、 程式碼檔案中的組件同步處理`Bin`資料夾，以及 HTML 相關支援檔案，例如 CSS 和 JavaScript 檔案。
3. 同步處理資料庫結構描述和 （或） 資料。

Web 應用程式的組態資訊通常位於`Web.config`檔案，並包含資料庫的連接字串，錯誤處理準則 URL 重寫規則和電子郵件伺服器資訊。 有時候這項資訊是不同的開發和生產環境中相同的應用程式中的應用程式。 比方說，當開發的應用程式最好使用開發資料庫，讓您不在測試對實際執行資料庫。 如此一來，資料庫連接字串通常與開發和生產應用程式之間不同。 由於這些差異，所以部署的一部分牽涉到變更 web 應用程式的組態資訊。

Web 應用程式組態變更，除了步驟 1 也可能需要將 web 伺服器和資料庫的組態。 比方說，如果建立 ASP.NET 網頁，或從 web 伺服器上的目錄中刪除檔案然後網頁伺服器必須設定為允許修改這些檔案系統。 同樣地，可能需要進行資料庫的權限或驗證設定。


步驟 2 包含一組基本的 ASP.NET 網頁和支援檔案，在開發和生產環境之間進行同步處理。 ASP 特定集。網路相關的檔案，需要兩個環境之間同步處理取決於您在 Visual Studio 中，建立並在下一個教學課程中，討論的專案類型[*決定需要的檔案部署*](determining-what-files-need-to-be-deployed-cs.md). 第三個和第四個教學課程- [*部署您的網站使用 FTP* ](deploying-your-site-using-an-ftp-client-cs.md)和[*部署您的網站使用 Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -檢查不同的工具和技術用於同步處理這些檔案。

當建置資料導向應用程式正在使用的資料庫通常有兩個： 一個用於開發和另一個在生產環境。 在開發期間，開發資料庫結構描述也可以修改成包含新的資料表、 資料行、 預存程序和觸發程序，或可能修改移除或重新命名現有的資料庫物件。 進行這些變更的時間，應用程式部署到生產環境的時間之間的開發和生產資料庫未同步。此非同步需要修正在部署程序。 在未來的教學課程會檢查這些挑戰。

## <a name="finding-a-web-host-provider"></a>尋找 Web 主機提供者

ASP.NET 應用程式可以部署到任何 web 伺服器具有.NET Framework 和網際網路資訊服務 (IIS) 安裝。 您可能會裝載站台從個人電腦，假設您已連線到網際網路，知道如何設定您的路由器，以允許傳入的 web 要求。 許多公司一樣，您也無法裝載在內部網路中的電腦的站台。 不過，這些教學課程中，焦點會裝載您的網站與 web 主機提供者。

> [!NOTE]
> [IIS](https://www.iis.net/)是 Microsoft 的企業級 web 伺服器。 它隨附於非首頁的 Windows 版本，例如 Windows Server 2008 和 Windows Vista 特定版本。 您不需要安裝 IIS ASP.NET 應用程式服務在開發環境中，因為 Visual Studio 包含 ASP.NET 程式開發 Web 伺服器。 不過，ASP.NET 開發 Web 伺服器僅接受本機連接，並因此不能用於實際執行環境。


您可以將您的網站部署到 web 主機提供者必須先決定哪些公司業務。 有無數的 web 代管公司在市場中;控管公司的"web"的搜尋傳回超過 5 百萬個結果。 您尋找程式最適合您？ 慣用的搜尋引擎是好的起點，這與類似的網站一樣[TopHosts](http://www.tophosts.com/)和[HostCritique](http://www.hostcritique.net/)，其比較與對照各種裝載服務。 我也建議您的同事和同事要求任何建議。您也可以要求建議[裝載開啟論壇](https://forums.asp.net/158.aspx)在這裡[ASP.NET 論壇](https://forums.asp.net/)。

Web 代管公司通常會提供共用主控的計劃，而專用主控方案。 當使用共用裝載單一 web 伺服器主機數十如果不是好幾百個不同的網站。 與專用主控您的租用提供自己的網站和網站單獨公司的電腦。 共用主控方案可能包含支援 ASP.NET 網頁使用的每個月 $9.95 Microsoft Access 資料庫、 5 GB 的磁碟空間和 100 GB 的每月的頻寬流量的能力。 另一個共用的主控方案可能包含支援 ASP.NET 頁面存取的每個月 $19.95 Microsoft SQL Server 2008 資料庫伺服器、 10 GB 的磁碟空間和每月的頻寬流量的 250 GB。 專用主控方案通常更為昂貴，成本數個數百個金額每月，但是提供更好的效能和更多的控制，比共用裝載選項。 您選擇何種方案相依於您的預算，流量會收到您的網站，以及您預期您將需要的功能。

當您選擇的 web 主機提供者時的兩個重要考量是客戶服務及服務的品質。 如果您有問題或設定問題時，花多少時間未提交到 web 主機的技術服務問題，直到您收到的回應？ 如何可靠是公司的服務？ 經常是否有資料庫中斷？ 頻率沒有其電子郵件伺服器離線？ 您可以詢問公司將提供有關其執行時間詳細資料，並詢問有關其客戶服務的原則，但更笑話的方式，是來請求的目前和過去客戶，您可以透過線上論壇、 新聞群組或透過電子郵件 listservs 意見反應.

> [!NOTE]
> 某些 web 代管公司將其企業專注特定技術堆疊，例如.NET 或[燈](http://en.wikipedia.org/wiki/LAMP_stack)(**L** inux， **A** pache， **M**ySQL，和**P** HP)，因此請確定您選取的公司裝載 ASP.NET 應用程式。 也請檢查並確定它們支援的 ASP.NET 用來建置應用程式的版本。 而且，如果您要建置資料導向應用程式，請確定 web 主機提供相同的資料庫伺服器和您使用的版本。


## <a name="summary"></a>總結

ASP.NET web 應用程式通常會設計、 建立，並在本機開發環境中測試過。 一旦準備發行版本時，便會移至生產環境。 雖然也可以在您的個人電腦或您公司內的伺服器上代管 ASP.NET 網站，但是許多企業和個人選擇來委外其裝載到 web 主機提供者。

此教學課程系列會檢查部署 ASP.NET 應用程式到 web 主機提供者，來瀏覽常見的挑戰的步驟。 本教學課程提供 ASP.NET 部署程序的高階概觀，並提供提示來尋找適當的 web 主機提供者。 ASP.NET 相關檔案複製到生產環境部署您的網站時需要查看下一個教學課程。

祝您程式設計 ！

### <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [下一步](determining-what-files-need-to-be-deployed-cs.md)
