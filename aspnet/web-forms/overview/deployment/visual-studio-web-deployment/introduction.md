---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 使用 Visual Studio 的 ASP.NET Web 部署： 簡介 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式到 Azure App Service Web Apps 或協力廠商裝載提供者，藉由使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: ad06639db9c5de78fb9926c7ab00e348bcbb20cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382552"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>使用 Visual Studio 的 ASP.NET Web 部署： 簡介
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式到 Azure App Service Web Apps 或協力廠商裝載提供者，藉由 Visual Studio 2012 使用適用於.NET 的 Azure SDK。 程序的大部分都類似 Visual Studio 2013。
> 
> 您開發以使其可讓使用者透過網際網路的 web 應用程式。 但 web 程式設計教學課程通常會停止已經示範如何取得您的開發電腦上使用的項目之後，權限。 這一系列的教學課程開始其他省略的位置： 您已建置的 web 應用程式，進行測試，且準備好了。 後續步驟 這些教學課程會示範如何將第一次部署到測試，在本機開發電腦上的 IIS，然後到 Azure 或協力廠商裝載提供者來預備和生產環境。 您要部署範例應用程式是 web 應用程式專案使用 Entity Framework、 SQL Server 和 ASP.NET 成員資格系統。 範例應用程式會使用 ASP.NET Web Form，但顯示的程序也適用於 ASP.NET MVC 和 Web API。
> 
> 這些教學課程假設您知道如何使用 Visual Studio 中的 ASP.NET。 如果不這麼做，是良好起點[基本的 ASP.NET Web Form 教學課程](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md)或是[基本的 ASP.NET MVC 教學課程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。
> 
> 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET 部署論壇](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)或是[StackOverflow](http://stackoverflow.com)。
> 
> 此內容也會當作免費電子書[TechNet 電子書的組件庫](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)。


## <a name="overview"></a>總覽

這些教學課程會引導您完成部署包含 SQL Server 資料庫的 ASP.NET web 應用程式。 進行測試，您本機開發電腦上的 IIS 然後再到 Azure App Service 和 Azure SQL Database 來預備和生產環境中的 Web 應用程式，您會將部署第一次。 您將了解如何使用單鍵發行時，Visual Studio 部署，您會看到如何使用命令列部署。

教學課程的數目可能會使部署程序看起來很令人怯步。 事實上，基本程序很簡單。 不過，在真實世界的情況下，您通常需要執行額外的部署工作 — 例如，在目標伺服器上設定資料夾權限。 我們已示範了一些教學課程不會漏掉可能阻礙您成功部署實際的應用程式的資訊，希望中的這些額外工作。

教學課程專為依序執行，以及每個部分是根據前一個部分。 您可以略過與您的情況下，不相關的組件，但接著您可能需要調整程序中稍後的教學課程。

## <a name="intended-audience"></a>適用對象

教學課程的目標是 ASP.NET 開發人員在環境中運作位置：

- 生產環境是 Azure App Service Web Apps 或協力廠商主機服務提供者。
- 部署並不限於使用持續整合程序，但可能會直接從 Visual Studio 中完成。

從部署[原始檔控制](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)使用[持續傳遞](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)除了一本教學課程示範如何從命令列來部署這些教學課程未涵蓋程序。 如需持續傳遞的資訊，請參閱下列資源：

- [持續整合與持續傳遞 （使用 Windows Azure 建置真實世界的雲端應用程式）](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [部署 Azure App Service 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [部署 Web 應用程式，在企業案例](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)（針對 Visual Studio 2010，仍有有用的資訊適用於企業環境所撰寫的教學課程的較舊集。）

## <a name="using-a-third-party-hosting-provider"></a>使用協力廠商主機服務提供者

教學課程會引導您設定 Azure 帳戶和應用程式部署至 Azure App Service 中的 Web 應用程式的預備和生產環境的程序。 不過，您可以使用相同的基本程序部署到您選擇的協力廠商裝載提供者。 在本教學課程會唯一處理程序移轉至 Azure，它們會說明這點，也建議在協力廠商主機服務提供者，您可以預期哪些差異。

## <a name="deploying-web-app-projects"></a>部署 web 應用程式專案

在下載及部署這些教學課程中的範例應用程式是 Visual Studio web 應用程式專案。 不過，如果您安裝最新[適用於 Visual Studio Web Publish Update](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx)，您可以使用相同的部署方法和工具的 web 應用程式專案。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 專案

範例應用程式是 ASP.NET Web Form 專案，但您將了解如何進行的所有項目也會適用於 ASP.NET MVC。 Visual Studio MVC 專案是另一種 web 應用程式專案。 唯一的差別在於，如果您要部署至不支援 ASP.NET MVC 或您的目標版本，它的主控提供者，您必須確定您已安裝適當的 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)， [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)或[MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) 在專案中的 NuGet 套件。

## <a name="programming-language"></a>程式設計語言

範例應用程式會使用 C# 教學課程不需要知道 C# 中，但這些教學課程所示的部署技術不因語言而異。

## <a name="database-deployment-methods"></a>資料庫部署方法

有三種方式，您可以部署 SQL Server 資料庫，以及在 Visual Studio 中的 web 部署：

- Entity Framework Code First 移轉
- DbDacFx Web Deploy 提供者
- DbFullSql Web Deploy 提供者

在本教學課程中，您將使用這些方法的前兩個。 DbFullSql Web Deploy 提供者是不再建議使用某些特定的情況下，例如從 SQL Server Compact 移轉至 SQL Server 除了傳統的方法。

本教學課程中所示的方法都沒有 SQL Server Compact 的 SQL Server 資料庫。 如需如何部署 SQL Server Compact 資料庫的詳細資訊，請參閱[Visual Studio Web 部署與 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。

本教學課程中所示的方法會要求您使用 Web Deploy 發行方法。 如果您偏好的其他發佈方法，例如 FTP、 檔案系統或 FPSE，請參閱[部署資料庫與 web 應用程式部署分開](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate)Visual Studio 及 ASP.NET 的 Web 部署內容對應中。

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First 移轉

在 Entity Framework 4.3 版中，Microsoft 引進了 Code First 移轉。 Code First 移轉會自動累加式變更資料模型和傳播這些變更資料庫的程序。 在舊版的 Code First 中，您通常會讓 Entity Framework 卸除並重新建立每次變更資料模型資料庫。 因為測試資料會輕鬆重新建立，但在生產環境中您通常想要更新資料庫結構描述而不需要卸除資料庫，這是不中開發的問題。 移轉功能可讓 Code First 來更新資料庫卸除並重新建立它。 您可以讓 Code First 自動決定如何進行必要的結構描述的變更，或您可以撰寫程式碼，自訂所做的變更。 如需 Code First 移轉，請參閱 < [Code First 移轉](https://msdn.microsoft.com/library/hh770484.aspx)。

當您部署 web 專案時，Visual Studio 可以自動化部署由 Code First 移轉管理資料庫的程序。 當您建立發行設定檔時，您可以選取核取方塊標示為 Execute Code First Migrations （在應用程式啟動時執行）。 此設定會使部署程序會自動設定應用程式 Web.config 檔案，在目的地伺服器上的，讓 Code First 會使用`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不會在部署程序執行與資料庫的任何項目。 當部署的應用程式來存取資料庫第一次部署之後時，Code First 自動建立資料庫或更新至最新版本的資料庫結構描述。 如果應用程式實作移轉種子方法，建立資料庫，或在更新結構描述之後，也會執行方法。

在本教學課程中，您將使用 Code First 移轉來部署應用程式資料庫。

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web Deploy 提供者

不由 Entity Framework Code First 管理 SQL Server 資料庫，您可以選取核取方塊，當您設定發佈設定檔會標示為更新資料庫。 初始部署期間，dbDacFx 提供者會在目的地資料庫，以符合來源資料庫中建立資料表和其他資料庫物件。 在後續的部署，提供者會判斷來源和目的地資料庫之間的差異，它會更新目的地資料庫，以符合來源資料庫的結構描述。 根據預設，此提供者不會進行任何會導致資料遺失，例如當資料表或資料行卸除的變更。

這個方法不會自動部署的資料庫資料表中的資料，但您可以建立指令碼，這樣做，並設定 Visual Studio 以在部署期間執行它們。 在部署期間執行指令碼的另一個原因是要進行結構描述變更，因為它們會造成資料遺失，所以無法自動完成。

在本教學課程中，您將使用 ASP.NET 成員資格資料庫部署的 dbDacFx 提供者。

## <a name="troubleshooting-during-this-tutorial"></a>針對本教學課程期間進行疑難排解

在部署期間，發生錯誤時，或已部署的站台無法正確執行，錯誤訊息不一定會提供一個明顯的解決方案。 若要利用一些常見的問題案例，協助您[疑難排解參考頁面](troubleshooting.md)可用。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查 [疑難排解] 頁面。

## <a name="comments-welcome"></a>歡迎使用的註解

教學課程的註解是 褖畫惎，且致力更新本教學課程時進行納入帳戶修正或改善教學課程的註解中所提供的建議。

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

本教學課程是撰寫適用於下列產品：

- Windows 8 或 Windows 7。
- Visual Studio 2012 或 Visual Studio 2012 Express for Web 含[最新的更新](https://go.microsoft.com/fwlink/?LinkId=272486)。
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

您可以遵循本教學課程使用 Visual Studio 2010 SP1 或 Visual Studio 2013，但某些螢幕擷取畫面將會不同，有些功能可能會不同。

如果您使用 Visual Studio 2013，請安裝[Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322)。

如果您使用 Visual Studio 2010 SP1，請安裝下列軟體：

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx)。

根據多少 SDK 相依性您已經有您的電腦上，安裝 Azure SDK 可能需要較長的時間，從數分鐘到半小時以上。 即使您打算發佈至協力廠商主機服務提供者而非 Azure 功能，因為 SDK 包含最新的更新，Visual Studio web 發佈功能，您需要 Azure SDK。

> [!NOTE]
> 本教學課程是以 Azure SDK 版本 1.8.1 寫入。 從那時起已發行並具備額外功能的較新版本。 已更新的教學課程提及這些功能和資源有其相關的詳細資訊連結。


指示和螢幕擷取畫面，根據 Windows 8 中，但這些教學課程說明適用於 Windows 7 的差異。

其他一些軟體，才能完成教學課程中，但您不需要有安裝。 本教學課程將引導您逐步完成在需要時，請安裝它。

## <a name="download-the-sample-application"></a>下載範例應用程式

您要部署的應用程式名為 Contoso 大學，並已為您建立。 它是一個簡化的版的大學網站上，根據 Contoso 大學應用程式中所述的鬆散[Entity Framework ASP.NET 網站上的教學課程](https://asp.net/entity-framework/tutorials)。

當您已安裝的必要條件時，下載[Contoso 大學 web 應用程式](https://go.microsoft.com/fwlink/p/?LinkId=282627)。 *.Zip*檔案包含多個版本的專案。 若要完成本教學課程的步驟，開始使用 C# 資料夾中的專案。 若要查看專案的教學課程結尾處的外觀，開啟專案 ContosoUniversity 端資料夾中。

若要準備專案，可使用教學課程的步驟，請執行下列步驟：

1. 儲存在名為 ContosoUniversity 您使用 Visual Studio 專案所使用的任何資料夾中的資料夾中的 C# 資料夾 ContosoUniversity 方案檔。

    根據預設，這是 Visual Studio 2012 的下列資料夾：

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (在本教學課程螢幕擷取畫面，如 [專案] 資料夾位於根目錄中`C`： 磁碟機。)
2. 啟動 Visual Studio，並開啟專案。
3. 在 **方案總管**，以滑鼠右鍵按一下方案，然後按一下**EnableNuGet 套件還原**。
4. 建置方案。
5. 如果您收到編譯錯誤，以手動方式還原 NuGet 套件：

    1. 在 **方案總管**，以滑鼠右鍵按一下方案，然後按一下 **管理方案的 NuGet 套件**。
    2. 在頂端**管理 NuGet 套件**對話方塊中，您會看到**此解決方案中遺漏了某些 NuGet 套件的。按一下即可還原。** 按一下 [**還原**] 按鈕。
    3. 重建方案。
6. 按 CTRL-F5 執行應用程式。

    應用程式會開啟至 Contoso 大學首頁上。

    ![首頁上開發](introduction/_static/image1.png)

    （可能會有的等候時間時 Visual Studio 啟動 SQL Server Express LocalDB 執行個體，而您可能會收到逾時錯誤，處理程序的時間太長。 在此情況下只是重新啟動專案。）

網站頁面，可從功能表列存取，並可讓您執行下列功能：

- 顯示學生統計資料 （[關於] 頁面）。
- 顯示、 編輯、 刪除和新增學生。
- 顯示和編輯的課程。
- 顯示和編輯講師。
- 顯示和編輯的部門。

以下是一些具代表性的頁面的螢幕擷取畫面。

![學生頁面開發人員](introduction/_static/image2.png)

![加入學生頁面開發人員](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>檢閱會影響部署的應用程式功能

應用程式的下列功能會影響您部署的方式，或您必須手動將它部署。 每一種是更詳細說明在下列教學課程系列中。

- Contoso 大學使用 SQL Server 資料庫來儲存應用程式資料，例如 student 和 instructor 的名稱。 資料庫包含混合的測試資料和實際執行資料，並部署至生產環境時需要排除測試資料。
- 應用程式會使用 ASP.NET 成員資格系統，將使用者帳戶資訊儲存在 SQL Server 資料庫。 應用程式會定義具有某些限制的資訊的存取權的系統管理員使用者。 您要部署的成員資格資料庫，而不需要測試帳戶，但以系統管理員帳戶。
- 應用程式會使用第三方錯誤記錄和報告公用程式。 必須與應用程式一起部署的組件中提供此公用程式。
- 錯誤記錄公用程式會在檔案資料夾，將錯誤資訊寫入 XML 檔案中。 您必須確定此資料夾中，已部署之網站中執行 ASP.NET 帳戶具有寫入權限，而且您不必從部署中排除此資料夾。 （否則錯誤記錄檔資料，從測試環境可能會部署到生產環境和/或生產錯誤記錄檔可能會被刪除。）
- 應用程式包含必須變更某些設定中部署*Web.config*取決於目的地環境 （測試、 預備或生產），以及必須根據組建變更其他設定檔案組態 （偵錯或發行）。
- Visual Studio 方案包含的類別庫專案。 此專案會產生的組件應該部署，而不是專案本身。

## <a name="summary"></a>總結

在此系列中的第一個教學課程，您已下載的範例 Visual Studio 專案，並檢閱會影響您如何部署應用程式的網站功能。 在下列教學課程中，您準備進行部署設定自動處理這些事項。 其他您手動處理。

> [!div class="step-by-step"]
> [下一步](preparing-databases.md)
