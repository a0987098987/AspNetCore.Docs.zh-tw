---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 使用 Visual Studio 的 ASP.NET Web 部署： 簡介 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式，Azure App Service Web 應用程式或協力廠商裝載提供者，藉由使用 V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890680"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>使用 Visual Studio 的 ASP.NET Web 部署： 簡介
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式，Azure App Service Web 應用程式或協力廠商裝載提供者，藉由使用 Azure SDK for.NET Visual Studio 2012。 大部分的程序會很類似 Visual Studio 2013。
> 
> 您開發以使其可讓使用者透過網際網路的 web 應用程式。 但 web 程式設計教學課程通常會停止已示範如何取得在開發電腦上使用的項目之後，權限。 這一系列的教學課程的開始位置的其他人保持關閉狀態： 您已建立 web 應用程式，來測試它，而且準備就緒。 後續步驟 這些教學課程會示範如何先部署到測試，在本機開發電腦上的 IIS，然後再到 Azure 或協力廠商主機提供者針對預備和生產環境。 您要部署的範例應用程式是 web 應用程式專案使用 Entity Framework、 SQL Server 和 ASP.NET 成員資格系統。 範例應用程式會使用 ASP.NET Web Form，但顯示的程序也適用於 ASP.NET MVC 和 Web API。
> 
> 這些教學課程假設您知道如何使用 Visual Studio 中的 ASP.NET。 如果不這麼做，是很好的起點是[基本的 ASP.NET Web Form 教學課程](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md)或[基本 ASP.NET MVC 的教學課程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。
> 
> 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET 部署論壇](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)或[StackOverflow](http://stackoverflow.com)。
> 
> 此內容也會提供免費電子書中為[TechNet 電子書圖庫](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)。


## <a name="overview"></a>總覽

這些教學課程將引導您完成部署 ASP.NET web 應用程式，其中包含 SQL Server 資料庫。 測試，在本機開發電腦上的 iis，然後在 Azure 應用程式服務和 Azure SQL Database。 預備和生產環境中的 Web 應用程式，您會將部署第一次。 您會看到如何使用單鍵發行時，Visual Studio 部署，您會看到如何使用命令列部署。

教學課程的數目可能會使部署程序似乎鉅的任務。 事實上，基本程序很簡單。 不過，在真實世界的情況下，您通常需要執行額外的部署工作，例如，設定目標伺服器上的資料夾權限。 我們已經示範了一些這些額外的工作，希望讓教學課程不可能阻礙您成功部署實際的應用程式的資訊。

教學課程專為在執行順序，和每個組件為基礎的前述組件。 您可以略過組件不是適用於您的情況，但然後，您可能必須調整之後的教學課程中的程序。

## <a name="intended-audience"></a>適用對象

教學課程的目標是 ASP.NET 開發人員在環境中工作在其中：

- 在實際執行環境是 Azure App Service Web 應用程式或協力廠商主機服務提供者。
- 部署並不限於使用持續整合程序，但可能會直接從 Visual Studio 中完成。

從部署[原始檔控制](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)使用[持續傳遞](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)程序未涵蓋在這些教學課程除了示範如何從命令列部署的一個教學課程。 如需持續傳遞資訊，請參閱下列資源：

- [持續整合與持續傳遞 （使用 Windows Azure 建置實際的雲端應用程式）](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [部署在 Azure App Service web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)(教學課程，撰寫 Visual Studio 2010，還有適用於企業環境的實用資訊的較舊 set。)

## <a name="using-a-third-party-hosting-provider"></a>使用協力廠商主機服務提供者

教學課程將帶領您完成設定 Azure 帳戶和部署的應用程式在 Azure App Service Web 應用程式預備和生產環境的程序。 不過，您可以使用相同的基本程序部署至您所選擇的第三方主控提供者。 在教學課程透過唯一的處理程序移至 Azure，它們會說明這點，並建議何種您可以預期在協力廠商主機服務提供者的差異。

## <a name="deploying-web-app-projects"></a>部署 web 應用程式專案

在下載及部署為這些教學課程的範例應用程式是 Visual Studio web 應用程式專案。 不過，如果您安裝最新[Web 發行 Visual Studio 更新](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx)，您可以使用相同的部署方法和工具的 web 應用程式專案。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 專案

範例應用程式是 ASP.NET Web Form 專案，但了解如何執行的所有項目也會是適用於 ASP.NET MVC。 Visual Studio MVC 專案是只是另一種 web 應用程式專案。 唯一的差別在於，如果您要部署至主機服務提供者不支援 ASP.NET MVC 或它的目標版本，您必須確定您已安裝適當的 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)， [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)或[MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) 在您的專案中的 NuGet 套件。

## <a name="programming-language"></a>程式設計語言

範例應用程式使用 C# 教學課程不需要的 C# 中，知識但的教學課程所示的部署技術不因語言而異。

## <a name="database-deployment-methods"></a>資料庫的部署方法

有三種方式，您可以部署 SQL Server 資料庫，以及 Visual Studio 中的 web 部署：

- Entity Framework Code First 移轉
- DbDacFx Web Deploy 提供者
- DbFullSql Web Deploy 提供者

在本教學課程中，您將使用這些方法的前兩個。 DbFullSql Web Deploy 提供者是不再建議使用某些特定的情況下，例如從 SQL Server Compact 移轉至 SQL Server 除了傳統的方法。

本教學課程中所示的方法也沒有 SQL Server Compact 的 SQL Server 資料庫。 如需如何部署 SQL Server Compact 資料庫的資訊，請參閱[Visual Studio Web 部署與 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。

本教學課程中的方法需要您使用 Web Deploy 發行方法。 如果您偏好的其他發佈方法，例如 FTP、 檔案系統或 FPSE，請參閱[部署 web 應用程式部署的個別資料庫](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate)Visual Studio 和 ASP.NET 的 Web 部署內容對應中。

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First 移轉

在 Entity Framework 4.3 版中，Microsoft 引進 Code First 移轉。 Code First 移轉將會自動累加式變更資料模型和這些將變更傳播至資料庫的程序。 在舊版中的第一個程式碼，您通常會讓 Entity Framework 卸除並重新建立每次變更資料模型資料庫。 因為測試資料是容易重新建立，但在生產環境中通常想要更新資料庫結構描述而不卸除資料庫，這是不中開發的問題。 移轉功能可讓程式碼第一次更新資料庫卸除並重新建立它。 您可以讓程式碼第一次自動決定如何進行必要的結構描述的變更，或您可以撰寫程式碼，自訂所做的變更。 如需 Code First 移轉的簡介，請參閱[Code First 移轉](https://msdn.microsoft.com/library/hh770484.aspx)。

當您要部署 web 專案時，Visual Studio 可以自動化部署資料庫管理的 Code First 移轉程的序。 當您建立的發行設定檔時，您可以選取核取方塊標示為執行 Code First 移轉 （在應用程式啟動時執行）。 此設定會自動設定目的地伺服器上的應用程式 Web.config 檔案，使程式碼第一次使用的部署程序`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不會與資料庫的任何項目在部署程序。 當部署的應用程式來存取資料庫第一次部署之後時，Code First 自動建立資料庫或更新資料庫結構描述的最新版本。 如果應用程式實作移轉種子方法，方法在建立資料庫或執行更新結構描述。

在本教學課程中，您將使用 Code First 移轉來部署應用程式資料庫。

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web Deploy 提供者

不是由 Entity Framework Code First 管理 SQL Server 資料庫，您可以選取核取方塊，當您設定的發行設定檔會標示為更新資料庫。 在初始部署中，dbDacFx 提供者會在目的地資料庫，以符合來源資料庫中建立資料表和其他資料庫物件。 在後續部署工作，提供者會判斷來源和目的地資料庫之間的差異，它會更新目的地資料庫，以符合來源資料庫的結構描述。 根據預設，此提供者將不會進行任何變更，會導致資料遺失，例如當資料表或資料行卸除。

這個方法不會自動部署的資料庫資料表中的資料，但是您可以建立指令碼來這樣做，和設定 Visual Studio 以在部署期間執行它們。 在部署期間執行指令碼的另一個原因是因為它們會導致資料遺失，所以無法自動完成的結構描述變更。

在本教學課程中，您將部署 ASP.NET 成員資格資料庫使用 dbDacFx 提供者。

## <a name="troubleshooting-during-this-tutorial"></a>在本教學課程疑難排解

在部署期間，就會發生錯誤時，或已部署的站台無法正確執行，錯誤訊息不一定會提供明顯的解決方案。 幫助您進行一些常見的問題案例，[疑難排解參考頁面](troubleshooting.md)可用。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查 [疑難排解] 頁面。

## <a name="comments-welcome"></a>歡迎使用註解

教學課程的註解是 褖畫惎，且致力更新本教學課程時進行納入帳戶修正或改善教學課程的註解中所提供的建議。

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

本教學課程是撰寫為下列產品：

- Windows 8 或 Windows 7。
- Visual Studio 2012 或 Visual Studio 2012 Express for Web 與[最新更新](https://go.microsoft.com/fwlink/?LinkId=272486)。
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

您可以遵循本教學課程使用 Visual Studio 2010 SP1 或 Visual Studio 2013 中，但是某些螢幕擷取畫面會不同，而且有些功能可能會不同。

如果您使用 Visual Studio 2013，安裝[Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322)。

如果您使用 Visual Studio 2010 SP1，請安裝下列軟體：

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx)。

根據 SDK 相依性的多少您已經在電腦上，安裝 Azure SDK 可能需要很長的時間，從幾分鐘到半小時或更多。 即使您打算將發佈到協力廠商主機服務提供者而非 Azure，因為 SDK 包含最新的更新 Visual Studio web 發行功能，您需要 Azure SDK。

> [!NOTE]
> 本教學課程是以 Azure sdk 版本 1.8.1 寫入。 從那時起已發行其他功能具有較新版本。 教學課程已經更新至敘述這些功能和資源的詳細資訊的連結。


指示和螢幕擷取畫面，依據 Windows 8 中，但教學課程適用於 Windows 7 中說明的差異。

一些其他軟體，才能完成此教學課程中，但您不需要有尚未安裝。 本教學課程將引導您在需要時安裝它的步驟。

## <a name="download-the-sample-application"></a>下載範例應用程式

您要部署的應用程式名為 Contoso 大學，並已為您建立。 它是 university 網站上，根據 Contoso 大學應用程式中所述的鬆散的簡化的版本[ASP.NET 網站上的 Entity Framework 教學課程](https://asp.net/entity-framework/tutorials)。

當您已安裝的必要條件時，下載[Contoso 大學 web 應用程式](https://go.microsoft.com/fwlink/p/?LinkId=282627)。 *.Zip*檔案包含多個版本的專案。 若要解決的教學課程的步驟，請啟動與 C# 資料夾中的專案。 若要查看專案的教學課程結尾處的起來，開啟專案 ContosoUniversity 端資料夾中。

若要將專案準備好的工作的教學課程的步驟，執行下列步驟：

1. C# 資料夾中名為 ContosoUniversity 您使用 Visual Studio 專案所使用的任何資料夾中的資料夾中儲存 ContosoUniversity 方案檔。

    根據預設，這是 Visual Studio 2012 的下列資料夾：

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (在本教學課程螢幕擷取畫面中，針對專案資料夾位於根目錄中`C`： 磁碟機。)
2. 啟動 Visual Studio，並開啟專案。
3. 在**方案總管 中**，以滑鼠右鍵按一下方案，然後按一下**EnableNuGet 封裝還原**。
4. 建置方案。
5. 如果您收到編譯錯誤，請手動還原 NuGet 封裝：

    1. 在**方案總管 中**，以滑鼠右鍵按一下方案，然後按一下**管理方案的 NuGet 套件**。
    2. 在頂端**管理 NuGet 封裝**對話方塊就會看到**從這個方案沒有某些 NuGet 封裝。按一下即可還原。** 按一下**還原** 按鈕。
    3. 重建方案。
6. 按 CTRL + F5 執行應用程式。

    應用程式會開啟至 Contoso 大學首頁。

    ![首頁上開發人員](introduction/_static/image1.png)

    （可能會有等待時間時 Visual Studio 啟動 SQL Server Express LocalDB 執行個體，而您如果可能會發生逾時錯誤的程序耗時過長。 在此情況下只是重新啟動專案。）

從功能表列存取網站頁面，並可讓您執行下列功能：

- 顯示學生統計資料 （關於頁面）。
- 顯示、 編輯、 刪除和新增學生。
- 顯示和編輯的課程。
- 顯示和編輯講師。
- 顯示和編輯的部門。

以下是少數代表性的頁面的螢幕擷取畫面。

![學生網頁開發人員](introduction/_static/image2.png)

![新增學生網頁開發人員](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>檢閱會影響部署的應用程式功能

應用程式的下列功能會影響您部署的方式，或您必須將其部署執行。 每一種是數列中的下列教學課程中所述更多詳細資料。

- Contoso 大學使用 SQL Server 資料庫儲存應用程式資料，例如學生和講師名稱。 資料庫包含混合了測試資料和實際執行資料和您部署至實際執行時要排除的測試資料。
- 應用程式會使用 ASP.NET 成員資格系統，將使用者帳戶資訊儲存在 SQL Server 資料庫。 應用程式定義的系統管理員使用者，有一些限制的資訊的存取權。 您要部署的成員資格資料庫，不具測試帳戶，但以系統管理員帳戶。
- 應用程式會使用第三方錯誤記錄和報告公用程式。 必須與應用程式部署的組件中提供這個公用程式。
- 錯誤記錄公用程式將錯誤資訊寫入 XML 檔案中的檔案資料夾。 您必須確定 ASP.NET 中已部署的站台執行的帳戶具有寫入權限到此資料夾，而且您必須從部署中排除此資料夾。 （否則從測試環境的錯誤記錄檔資料可能會部署到生產環境和/或生產環境錯誤記錄檔可能已刪除。）
- 應用程式包含必須變更某些設定中已部署*Web.config*根據目的地環境 （測試、 預備或生產），以及根據組建必須變更其他設定檔組態 （偵錯或發行）。
- Visual Studio 方案可包含的類別庫專案。 應該部署此專案會產生的組件，而非專案本身。

## <a name="summary"></a>總結

在此數列中的第一個教學課程，您已下載範例 Visual Studio 專案，並檢閱影響您如何部署應用程式的網站功能。 下列教學課程中，在您準備進行部署的下列步驟來自動處理某些設定。 其他您以手動方式處理。

> [!div class="step-by-step"]
> [下一步](preparing-databases.md)
