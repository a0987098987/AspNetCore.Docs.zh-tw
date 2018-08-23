---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web 在企業中的部署 |Microsoft Docs
author: jrjlee
description: 本教學課程說明如何以符合您管理的部署，提供企業級 web 應用程式時會遇到的挑戰的許多...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 735cc5ac37e369e6149c526174c3f74a08db9758
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832624"
---
<a name="web-deployment-in-the-enterprise"></a>在企業中的 web 部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程說明如何符合許多您管理的企業級 web 應用程式開發、 測試、 預備及生產環境部署時會遇到的挑戰。 本教學課程包含參考解決方案以及概念和工作導向的內容，引導您完成各種常見的工作和程序的混合。
> 
> 這些教學課程義大利文翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


## <a name="enterprise-deployment-challenges"></a>企業部署方面的挑戰

他們決定以管理複雜的企業級解決方案的部署時，組織通常會遇到這些挑戰：

- 您可以將專案部署到多個環境，例如開發人員或測試環境，需要預備平台，以及實際執行伺服器。 解決方案需要針對每個環境的不同的組態設定一起部署。
- 您要部署多個相依的專案同時做為單一步驟或自動化建置和部署程序的一部分。
- 您需要能夠的磁碟機部署從自動化的程序。 例如，您要部署至測試環境的 web 應用程式，當新的程式碼簽入使用持續整合 (CI) 程序。
- 您必須能夠控制部署程序，並將部署變數設定從 Visual Studio 外部，為開發人員不太可能會有正確的組態設定或針對每種目標環境所需的認證。
- 您要部署結構描述為基礎的資料庫專案，並保留現有的資料，在後續的部署。
- 您需要部署臨機操作為基礎的成員資格資料庫，而不需要部署使用者帳戶資料。 此外，您可能也需要更新的已部署的成員資格資料庫結構描述，而不會遺失現有的使用者帳戶資料。
- 您要排除特定檔案或資料夾，當您將內容部署至各種目標環境。

## <a name="overview-of-approach"></a>方法的概觀

本教學課程，以及在此系列中，其他教學課程會使用此高層級的方式來克服上述挑戰。

- **您可以使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制整體的建置和部署程序。**
- 這可讓您建置及部署方案中的每個專案，透過單一、 可編寫指令碼作業的一部分。
- 使用簡單的環境特定專案檔，會設定環境特定設定。 相較於使用方案組態的 Visual Studio 的相關方法並發行設定檔來設定不同的環境的部署，此方法可讓您設定和管理從 Visual Studio 外部的部署程序。 這表示開發人員不需要提前知道連接字串、 服務端點、 伺服器認證和目的地環境的其他部署變數。
- Team Build 的 Team Foundation Server (TFS) 工作流程可以叫用自訂的專案檔。 這可讓您設定 CI 案例的自動化的部署。

**使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 來封裝及部署 web 應用程式專案。**

- Web Deploy 提供的架構可讓您封裝及部署您的 web 應用程式內容至目的地 IIS web 伺服器，以及相依性、 組態設定、 安全性設定，以及任何其他需求。
- 您可以控制整個的封裝和部署程序，從在您自訂的 MSBuild 專案檔中。 您也可以使用隨附於您的 web 部署套件，例如連接字串、 服務端點和 IIS 目的地詳細資料的組態設定。
- Web Deploy，Web 發行管線，以及提供許多擴充點可讓您自訂您的部署。 比方說，很容易就能從您的 web 部署套件中排除不必要的檔案和資料夾。

**您可以使用 VSDBCMD.exe 公用程式來部署及更新資料庫結構描述。**

- VSDBCMD 可讓您將部署從資料庫結構描述檔案 (.dbschema)，當您建置 Visual Studio 資料庫專案所產生的資料庫。 相反地，Web Deploy 所含的資料庫部署功能是更適合從本機的 SQL Server 執行個體部署現有的資料庫。
- VSDBCMD 內建與部署資料庫專案的 Visual Studio 的功能，可讓您將差異的更新部署至現有的目標資料庫。 這可讓您保留任何現有的資料，當您升級資料庫結構描述。
- 在您自訂的 MSBuild 專案檔中，您可以執行從 VSDBCMD 命令。

## <a name="content-map"></a>內容對應

本教學課程中涵蓋的主題可分為四個主要區域。

這些主題將介紹參考方案&#x2014;連絡管理員解決方案&#x2014;，並說明如何下載它，並將它設定在本機電腦上：

- [連絡管理員解決方案](the-contact-manager-solution.md)
- [設定連絡管理員解決方案](setting-up-the-contact-manager-solution.md)

這些主題會介紹 MSBuild 專案檔，說明如何建立和使用自訂的專案檔，並逐步解說連絡管理員解決方案的部署程序：

- [了解專案檔](understanding-the-project-file.md)
- [了解建置流程](understanding-the-build-process.md)

這些主題會說明 web 應用程式部署，包括如何建置和封裝程序運作，建置程序說明如何與 Web 發行管線整合、 如何修改部署參數，以及如何將 web 套件部署到目的地環境：

- [建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)
- [設定網頁套件部署的參數](configuring-parameters-for-web-package-deployment.md)
- [部署網頁套件](deploying-web-packages.md)

- [部署資料庫專案](deploying-database-projects.md)描述不同的技術，可用來部署 Visual Studio 資料庫專案，以及每一種方法的優缺點。 [建立和執行部署命令檔](creating-and-running-a-deployment-command-file.md)說明如何建立簡單的命令檔，以封裝您的部署邏輯，而且可讓您將複雜的解決方案部署為單一步驟的程序。
- 最後，[手動安裝網頁套件](manually-installing-web-packages.md)最後本教學課程將示範您可以將 IIS web 封裝匯入。

## <a name="key-technologies"></a>主要技術

在本教學課程的主題主要會使用這些技術來管理組建和部署：

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- VSDBCMD.exe 資料庫部署公用程式

## <a name="other-tutorials-in-this-series"></a>在這一系列的其他教學課程

這會形成企業級 web 部署的一系列五個教學課程的一部分。 這些是系列中的其他教學課程：

- [部署 Web 應用程式，在企業案例](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此介紹的內容會提供教學課程系列中的內容相關的背景。 其中說明教學課程的案例中，並說明了如何工作和逐步解說所述，在整個系列納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows 伺服器以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 套件部署。 它提供指導方針選擇適當的部署方法，為您自己的環境，並說明如何使用 Web 伺服陣列架構 (WFF) 伺服器陣列中的所有 web 伺服器之間複寫已部署的 web 應用程式。
- [設定 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的 CI 程序的一部分的部署，以及手動觸發的特定組建的部署。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種進階的部署工作，例如自訂多個環境的資料庫部署、 從部署中排除檔案和資料夾以及部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](the-contact-manager-solution.md)
