---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET 裝載選項 (C#) |Microsoft Docs
author: rick-anderson
description: ASP.NET web 應用程式通常設計、 建立，並在本機開發環境中測試和要部署到生產環境 o...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 3adad579c5893fb4f40e7043b9ece78740080f65
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826221"
---
<a name="aspnet-hosting-options-c"></a>ASP.NET 裝載選項 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> ASP.NET web 應用程式通常會設計、 建立，並在本機開發環境和需求部署至生產環境中，當其就緒版本進行測試。 本教學課程中提供的部署程序的高階概觀，並做為本教學課程系列的簡介。


## <a name="introduction"></a>簡介

Web 應用程式通常會設計、 建立，而且只有在網站上的程式設計師可以存取的開發環境中測試。 準備好要發行應用程式之後，便會移至生產環境在網際網路上的任何人都可以存取的網站。 此部署程序導入了幾項挑戰：

- 生產環境中必須存在且正確設定才能部署 ASP.NET 應用程式;此外，您必須在生產環境保持為最新的最新的安全性修補程式。
- 一組正確的標記檔案、 程式碼檔案和支援檔案必須從開發環境複製到實際執行環境。 對於資料驅動的應用程式，這可能需要複製的資料庫結構描述和/或資料，以及。
- 可能會有兩個環境之間的組態差異。 資料庫連接字串或電子郵件伺服器使用開發環境中可能會不同於實際執行環境。 不僅如此，應用程式的行為可能會取決於環境。 例如，在開發過程中發生錯誤時的錯誤詳細資料可以顯示在畫面上，但在生產環境中發生錯誤時，相反地，應該會顯示使用者易記的錯誤頁面和錯誤詳細資料以電子郵件傳送給開發人員。

若要排除第一項挑戰-設定和維護生產環境-許多個人和企業外包到它們的生產環境*web 主機服務提供者*。 Web 主控提供者會管理實際執行環境，代表您的公司。 有無數的 web 主機提供者，各有不同的價格和服務層級;請參閱 「 尋找 Web 主機提供者 」 一節，如需尋找這類的服務提供者的秘訣。

這是一系列看看 ASP.NET web 應用程式部署到生產環境中的 web 主機提供者所管理的相關步驟的教學課程中的第一個。 這些教學課程期間，我們會檢驗：

- 檔案必須部署到 web 主機提供者。
- 簡化部署程序的工具。
- 如何將資料庫部署。
- 部署使用的資料庫的祕訣[SQL 為基礎的成員資格和角色提供者](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)，以及以模擬網站的管理工具，在生產環境中的方式。
- 順暢地在開發期間所做的變更更新生產環境中的資料庫的策略。
- 記錄錯誤發生在實際執行環境，以及發生錯誤時，通知開發人員的方法上的技術。

這些教學課程專為很簡潔，並提供足夠的螢幕擷取畫面以視覺化方式引導您完成程序的逐步指示。 本教學課程中首上尋找 web 主控提供者提供的 ASP.NET 部署程序和建議的概觀。 讓我們開始吧 ！

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET 部署程序概觀

簡單的說，部署 ASP.NET 應用程式時，需要下列三個步驟：

1. 在生產環境中設定 web 應用程式、 web 伺服器和資料庫。
2. 同步處理 ASP.NET 頁面、 程式碼檔案中的組件`Bin`資料夾，然後 HTML 相關支援檔案，如 CSS 和 JavaScript 檔案。
3. 同步處理資料庫結構描述和/或資料。

Web 應用程式的組態資訊通常位於`Web.config`檔案，並包含資料庫的連接字串，錯誤處理的準則，URL 重寫規則，以及電子郵件伺服器資訊。 通常這項資訊是不同的開發與生產環境中相同的應用程式中的應用程式。 比方說，開發應用程式時最好使用開發資料庫，讓您不測試對實際執行資料庫。 如此一來，資料庫連接字串通常會開發和生產環境應用程式之間不同。 由於這些差異，所以部署的一部分牽涉到對 web 應用程式的組態資訊的變更。

Web 應用程式組態變更，除了步驟 1 也可能需要 web 伺服器和資料庫組態。 比方說，如果建立 ASP.NET 網頁，或從 web 伺服器上的目錄中刪除檔案然後網頁伺服器必須設定為允許這些檔案系統修改。 同樣地，可能需要對資料庫的權限或驗證設定。


步驟 2 中，牽涉到的一組基本的 ASP.NET 網頁和支援檔案，在開發和生產環境之間進行同步處理。 ASP 特定的集合。需要同步處理兩個環境之間的網路相關檔案取決於您在 Visual Studio 中建立，並在下一個教學課程中，討論的專案類型[*判斷哪些檔案需要部署*](determining-what-files-need-to-be-deployed-cs.md). 第三個和第四個教學課程- [*部署您的網站使用 FTP* ](deploying-your-site-using-an-ftp-client-cs.md)並[*部署您的網站使用的 Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -檢查不同的工具和技術，用於這些檔案的同步處理。

建置資料導向的應用程式有正在使用通常兩個資料庫時： 一個用於開發和生產環境。 在開發期間，開發資料庫的結構描述可以修改成包含新的資料表、 資料行、 預存程序和觸發程序，或可以修改成移除或重新命名現有的資料庫物件。 進行這些變更的時間與時間之間的應用程式部署到生產環境，開發和生產環境資料庫未同步。此非同步功能需要在部署過程中修正。 在未來的教學課程中，會檢查這些挑戰。

## <a name="finding-a-web-host-provider"></a>尋找 Web 主機提供者

ASP.NET 應用程式可以部署到具有.NET Framework 和 Internet Information Services (IIS) 安裝任何 web 伺服器。 您可以裝載站台，從您的個人電腦，假設您有寬頻連線到網際網路，了解如何設定您的路由器，以允許傳入的 web 要求。 許多公司都一樣，您也可以裝載在內部網路中的電腦的站台。 不過，這些教學課程中，焦點會裝載您的網站與 web 主機提供者。

> [!NOTE]
> [IIS](https://www.iis.net/)是 Microsoft 的企業級 web 伺服器。 附隨了非-家用版的 Windows，例如 Windows Server 2008 和 Windows Vista 特定版本。 您不需要安裝 IIS，以在開發環境中，提供 ASP.NET 應用程式，因為 Visual Studio 包含 ASP.NET 程式開發 Web 伺服器。 不過，ASP.NET Development Web Server 僅接受本機連接，並因此無法在生產環境中使用。


您可以將您的網站部署到 web 主機提供者之前必須先決定哪些公司来進行交易。 有無數的網站裝載在 marketplace 中; 中的公司「 web 主機服務公司 」 的搜尋傳回超過 5 萬筆結果。 您找到最適合您？ 慣用的搜尋引擎是好的起點，是網站，例如[TopHosts](http://www.tophosts.com/)並[HostCritique](http://www.hostcritique.net/)，這比較和對照不同的裝載服務。 我也建議您的同事或同事尋求任何建議;您也可尋求建議[裝載開放的論壇](https://forums.asp.net/158.aspx)在這裡[ASP.NET 論壇](https://forums.asp.net/)。

家虛擬主機公司通常會提供共用的主控方案，然後專用主控方案。 使用共用裝載單一 web 伺服器主機數十甚至數百個不同的網站。 有專用的裝載您的租用提供您的網站和您的站台本身的公司電腦。 共用的主控方案可能包含支援 ASP.NET 頁面中，能夠使用 Microsoft Access 資料庫、 5 GB 的磁碟空間和 100 GB 的每月的頻寬流量適用於每月 $9.95 美元。 另一個共用的主控方案可能包含支援 ASP.NET 頁面存取的每月美金 19.95 Microsoft SQL Server 2008 資料庫伺服器、 10 GB 的磁碟空間和 250 GB 的每月的頻寬流量。 專用託管方案通常更昂貴、 成本數個數百個金額每個月，但是提供更好的效能和更多的控制，比共用裝載選項。 您選擇何種計劃取決於您的預算，多少流量便會收到您的網站，以及您預期您會需要功能。

選擇 web 主機提供者時的兩個重要考量是客戶服務和服務品質。 如果您有疑問或發生設定問題，花多少時間會提交到 web 主機的技術支援問題，直到您收到的回應？ 公司的服務是如何可靠？ 他們經常有資料庫中斷嗎？ 頻率沒有其電子郵件伺服器離線？ 您一律可以詢問公司提供有關其執行時間詳細資料，並詢問有關其客戶服務的原則，但更笑話向其徵求意見的目前和過去客戶，您可以透過線上論壇、 新聞群組，以及電子郵件 listservs 執行.

> [!NOTE]
> 有些 web 裝載公司業務著重於特定的技術堆疊，例如.NET 或是[LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux， **A** pache， **M**ySQL，並**P** HP)，因此請確定您選取的公司裝載 ASP.NET 應用程式。 也請檢查以確保它們支援的 ASP.NET 用來建置您的應用程式的版本。 而且，如果您要建置資料導向的應用程式，請確定 web 主機提供相同的資料庫伺服器和您使用的版本。


## <a name="summary"></a>總結

ASP.NET web 應用程式通常會設計、 建立，並在本機開發環境中測試。 準備發行的版本之後，便會移至生產環境。 雖然可以在您的個人電腦上，或您公司內的伺服器上的主應用程式的 ASP.NET 網站，許多企業和個人選擇外包其裝載到 web 主機提供者。

本教學課程系列會檢查部署 ASP.NET 應用程式到 web 主機提供者，探索常見的挑戰的步驟。 本教學課程提供 ASP.NET 部署程序的高階概觀，並提供尋找適當的 web 主機提供者的秘訣。 下一個教學課程會探討 ASP.NET 相關的檔案需要部署您的網站時，要複製到實際執行環境。

快樂地寫程式 ！

### <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [下一步](determining-what-files-need-to-be-deployed-cs.md)
