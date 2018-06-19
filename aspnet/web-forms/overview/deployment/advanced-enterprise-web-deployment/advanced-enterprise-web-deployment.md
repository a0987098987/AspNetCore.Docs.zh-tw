---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: 進階企業 Web 部署 |Microsoft 文件
author: jrjlee
description: 本教學課程會示範如何執行必要或適合在許多企業部署案例的各種工作。 義大利文的 translati for...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879929"
---
<a name="advanced-enterprise-web-deployment"></a>進階的企業 Web 部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程會示範如何執行必要或適合在許多企業部署案例的各種工作。
> 
> 這些教學課程的義大利文的翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


這會形成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求基礎的教學課程的一部分此教學課程使用範例方案&#x2014;[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="scenario-overview"></a>情節概觀

這些教學課程的高層級案例所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我們建議您檢閱本主題，在開始此教學課程之前。

## <a name="how-to-use-this-tutorial"></a>如何使用本教學課程

- 每個在本教學課程中的主題是獨立的並且將特定的挑戰或在企業部署案例中，就會發生的問題。 您不需要進行任何特定順序中的下列主題。 不過，本教學課程涵蓋一些進階的工作。 因此，您應該熟悉的概念和技術，[企業中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)教學課程涵蓋為了要能夠從這個內容的最大好處。
- 本教學課程包含下列主題：
- [執行部署"What If"](performing-a-what-if-deployment.md)。 在許多案例中，您需要在實際進行任何變更之前，判斷建議的部署目標環境或任何現有的內容上的影響。 本主題說明如何執行 「 假設 」 部署以產生記錄檔和資料庫更新指令碼，就像您部署的內容至目標環境，而不實際進行任何變更。 分析這些資源可協助您找出任何可能的問題即時部署之前。
- [自訂資料庫部署多個環境](customizing-database-deployments-for-multiple-environments.md)。 當您將資料庫專案部署到多個目的地時，您通常需要自訂每個目標環境的部署屬性。 例如，在測試環境中您會通常重新建立資料庫上每個部署，而在預備或生產環境中您可能遠超過進行累加式更新，才能保留您的資料。 本主題描述如何建立每個目標環境的環境特定部署組態 (.sqldeployment) 檔案到您的部署邏輯加入這些屬性變更。
- 將資料庫角色成員資格部署到測試環境。 當您重新建立的資料庫，每次部署&#x2014;比方說，做為持續整合 (CI) 的一部分建置並部署至測試環境&#x2014;您通常必須在每次設定資料庫角色成員資格。 例如，您通常必須與您的 web 應用程式相關聯的應用程式集區識別的權限授與。 本主題描述如何將部署後 SQL 指令碼加入至您的部署邏輯可以自動化此程序。
- [將成員資格資料庫部署至企業環境](deploying-membership-databases-to-enterprise-environments.md)。 ASP.NET 成員資格資料庫有可以使得在部署程序的各種特性。 例如，僅限結構描述的部署會讓資料庫處於非運作狀態。 在大部分情況下，最好是直接在每個目的地環境中建立成員資格資料庫。 不過，如果您沒有將成員資格資料庫部署，本主題描述的一些方法，您可以用來滿足固有的挑戰。
- [從部署中排除檔案和資料夾](excluding-files-and-folders-from-deployment.md)。 在某些情況下，您需要修改 web 套件到特定目的地環境的內容。 比方說，您可能想要包含 JavaScript 程式庫的完整版本，當您將部署到測試環境中，以支援用戶端偵錯，但使用縮短的版本的程式庫，當您將部署至預備或生產環境。 本主題說明如何排除特定檔案和資料夾從封裝建立處理序。
- [函式使用 Web 應用程式離線 Web 部署](taking-web-applications-offline-with-web-deploy.md)。 當您將方案部署至預備或生產環境時，您通常需要部署程序期間需要 web 應用程式離線。 本主題描述如何新增*應用程式\_offline.htm* web 應用程式在開始部署程序的檔案，並在結束時將其移除。 雖然*應用程式\_offline.htm*檔案位置中，瀏覽至 web 應用程式的使用者會自動重新導向至*應用程式\_offline.htm*檔案。
- [執行 Windows PowerShell 指令碼從 MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md)。 許多部署案例會需要更複雜的部署後動作，例如新增至登錄的自訂事件來源，或設定 SQL Server 執行個體之間的複寫。 這些動作通常是透過 Windows PowerShell 指令碼完成。 本主題描述如何從 Microsoft Build Engine (MSBuild) 專案檔中執行 Windows PowerShell 指令碼，做為建置和部署處理程序的一部分。
- [疑難排解封裝程序](troubleshooting-the-packaging-process.md)。 Web 發行管線 (WPP) 會定義名為 MSBuild 屬性**EnablePackageProcessLoggingAndAssert**可用來產生 web 應用程式專案的封裝程序的深入資訊。 本主題描述之屬性的用途以及如何使用它。

## <a name="key-technologies"></a>關鍵技術

本教學課程著重於如何使用這些產品和技術支援自動化的建置和 web 部署：

- Visual Studio 2010 和 Team Foundation Server (TFS) 2010
- MSBuild 和 TFS Team Build
- Internet Information Services (IIS) 7.5
- IIS Web Deployment Tool (Web Deploy) 2.1
- VSDBCMD.exe 資料庫部署公用程式

## <a name="other-tutorials-in-this-series"></a>在這一系列其他教學課程

這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。 這些是數列中的其他教學課程：

- [部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此入門內容提供教學課程系列內容的背景。 它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 WPP、 Web Deploy，和其他相關的技術。 它說明如何使用這些工具一起管理複雜的部署處理程序。
- [用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows servers 以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 封裝部署。 它提供有關選擇您自己的環境中，適當的部署方法的指引，並說明如何使用伺服器陣列中的所有 web 伺服器之間複寫部署的 web 應用程式的 Web 伺服陣列架構 (WFF)。
- [設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的部署 CI 程序的一部分，以及手動觸發部署的特定組建。

> [!div class="step-by-step"]
> [下一步](performing-a-what-if-deployment.md)
