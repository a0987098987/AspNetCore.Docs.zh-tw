---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: 進階企業 Web 部署 |Microsoft Docs
author: jrjlee
description: 本教學課程會示範如何執行必要或適合在許多企業部署案例的各種工作。 義大利文的 translati for...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b4546079d15649ff2371ced6746a29a90bdfc85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401869"
---
<a name="advanced-enterprise-web-deployment"></a>進階的企業 Web 部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程會示範如何執行必要或適合在許多企業部署案例的各種工作。
> 
> 這些教學課程義大利文翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


這會形成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本系列教學課程使用範例解決方案&#x2014; [Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="scenario-overview"></a>情節概觀

高階的案例，這些教學課程中所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我們建議您檢閱本主題，再開始本教學課程。

## <a name="how-to-use-this-tutorial"></a>如何使用本教學課程

- 每個主題，在本教學課程都各自獨立，並解決特殊的挑戰或企業部署案例中，就會發生的問題。 您不需要進行任何特定順序中的下列主題。 不過，本教學課程涵蓋一些進階的工作。 因此，您應該已經熟悉它的概念和技術所[企業中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)教學課程將說明以取得從這個內容的最大好處。
- 本教學課程包含下列主題：
- [執行"What If"部署](performing-a-what-if-deployment.md)。 在許多案例中，您會想要判斷建議的部署目標環境上的任何現有內容的影響，實際進行任何變更之前。 本主題說明如何執行 「 假設 」 部署來產生記錄檔和資料庫更新指令碼，如同您已部署內容至目標環境中，而不實際進行任何變更。 分析這些資源可協助您找出任何潛在的問題即時部署之前。
- [自訂多個環境的資料庫部署](customizing-database-deployments-for-multiple-environments.md)。 當您將資料庫專案部署到多個目的地時，您通常需要自訂每個目標環境的部署屬性。 例如，在測試環境中您會通常重新建立資料庫在每個部署，而在預備或生產環境中會更容易進行累加式更新，才能保留您的資料。 本主題描述如何將這些屬性變更您的部署邏輯納入藉由建立每個目標環境的環境特定部署組態 (.sqldeployment) 檔案。
- 將資料庫角色成員資格部署到測試環境。 當您重新建立每個部署上的資料庫&#x2014;例如，做為一部分的連續整合 (CI) 建置並部署至測試環境&#x2014;您通常需要在每次設定資料庫角色成員資格。 例如，您通常必須與您的 web 應用程式相關聯的應用程式集區識別的權限授與。 本主題描述如何將部署後 SQL 指令碼新增至您的部署邏輯可以自動化此程序。
- [將成員資格資料庫部署至企業環境](deploying-membership-databases-to-enterprise-environments.md)。 ASP.NET 成員資格資料庫有可以使部署程序的各種特性。 比方說，僅限結構描述的部署會讓資料庫處於非運作狀態。 在大部分情況下，最好是直接在每個目的地環境中建立的成員資格資料庫。 不過，如果您沒有部署的成員資格資料庫，本主題描述一些您可以使用符合所帶來的挑戰的方法。
- [從部署中排除檔案和資料夾](excluding-files-and-folders-from-deployment.md)。 在某些情況下，您會想要調整您的 web 封裝到特定目的地環境的內容。 比方說，您可能想要包含的 JavaScript 程式庫的完整版本，部署至測試環境中，以支援用戶端偵錯，但使用縮短的程式庫版本，當您將部署至預備或生產環境時。 本主題說明如何排除特定檔案和資料夾從封裝的建立程序。
- [讓 Web 應用程式離線使用 Web 部署](taking-web-applications-offline-with-web-deploy.md)。 當您將解決方案部署至預備或生產環境時，您通常需要部署程序期間需要 web 應用程式離線。 本主題描述如何新增*應用程式\_offline.htm*在開始部署程序在 web 應用程式檔案，並將它移除結尾。 雖然*應用程式\_offline.htm*檔案的位置中，瀏覽至 web 應用程式的任何使用者會自動重新導向至*應用程式\_offline.htm*檔案。
- [執行 Windows PowerShell 指令碼從 MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md)。 許多部署案例需要更複雜的部署後動作，例如將自訂的事件來源新增至登錄，或設定 SQL Server 執行個體之間的複寫。 這些動作，通常是透過 Windows PowerShell 指令碼來完成。 本主題描述如何從 Microsoft Build Engine (MSBuild) 專案檔的 Windows PowerShell 指令碼執行的組建和部署程序的一部分。
- [疑難排解封裝程序](troubleshooting-the-packaging-process.md)。 Web 發行管線 (WPP) 會定義名為 MSBuild 屬性**EnablePackageProcessLoggingAndAssert**可用來產生 web 應用程式專案的封裝程序的深入資訊。 本主題描述屬性的動作以及如何使用它。

## <a name="key-technologies"></a>主要技術

本教學課程著重於如何使用這些產品和技術來支援自動化的建置和 web 部署：

- Visual Studio 2010 與 Team Foundation Server (TFS) 2010
- MSBuild 和 TFS Team Build
- Internet Information Services (IIS) 7.5
- IIS Web Deployment Tool (Web Deploy) 2.1
- VSDBCMD.exe 資料庫部署公用程式

## <a name="other-tutorials-in-this-series"></a>在這一系列的其他教學課程

這會形成企業級 web 部署的一系列五個教學課程的一部分。 這些是系列中的其他教學課程：

- [部署 Web 應用程式，在企業案例](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此介紹的內容會提供教學課程系列中的內容相關的背景。 其中說明教學課程的案例中，並說明了如何工作和逐步解說所述，在整個系列納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 WPP、 Web Deploy 和其他相關的技術。 它說明如何使用這些工具搭配來管理複雜的部署程序。
- [設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows 伺服器以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 套件部署。 它提供指導方針選擇適當的部署方法，為您自己的環境，並說明如何使用 Web 伺服陣列架構 (WFF) 伺服器陣列中的所有 web 伺服器之間複寫已部署的 web 應用程式。
- [設定 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的 CI 程序的一部分的部署，以及手動觸發的特定組建的部署。

> [!div class="step-by-step"]
> [下一步](performing-a-what-if-deployment.md)
