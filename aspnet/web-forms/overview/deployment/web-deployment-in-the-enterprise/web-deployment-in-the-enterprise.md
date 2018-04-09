---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Web 企業中的部署 |Microsoft 文件
author: jrjlee
description: 本教學課程說明如何符合許多管理對內企業規模的 web 應用程式的部署時，就會發生的驗證題目...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: fc463cb689f4f63a12788b80958c9fc8fe20119d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="web-deployment-in-the-enterprise"></a>在企業中的 web 部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程說明如何符合許多管理企業規模 web 應用程式部署至開發、 測試、 預備及生產環境時，您會遇到的挑戰。 本教學課程包含參考方案以及概念和工作導向的內容，引導您完成各種常見的工作和程序的混合。
> 
> 這些教學課程的義大利文的翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


## <a name="enterprise-deployment-challenges"></a>企業部署挑戰

它們看起來來管理複雜，企業級解決方案的部署時，組織通常會遇到這些挑戰：

- 您必須要能將專案部署到多個環境，例如開發人員或測試環境中，暫存平台和實際執行伺服器。 方案必須先部署每個環境的不同組態設定。
- 您必須同時在單一步驟或自動化建置和部署程序的一部分部署多個相依專案。
- 您需要能夠的磁碟機部署從自動化的程序。 例如，您要使用持續整合 (CI) 處理程序部署至測試環境的 web 應用程式，當新的程式碼簽入。
- 您需要能夠控制部署程序以及設定部署變數，從 Visual Studio 外部，為開發人員不太可能會有正確的組態設定或針對每個目標環境所需的認證。
- 您需要部署結構描述為基礎的資料庫專案，並保留現有的資料，後續部署。
- 您需要部署臨機操作為基礎的成員資格資料庫，而不需部署使用者帳戶資料。 此外，您可能也需要更新的已部署的成員資格資料庫結構描述，而不會遺失現有的使用者帳戶資料。
- 您要排除特定檔案或資料夾，當您將內容部署至各種目標環境。

## <a name="overview-of-approach"></a>方法的概觀

此教學課程中，以及在這一系列，其他的教學課程會使用此高層級的方式，以符合上面所述的挑戰。

- **使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制整個建置和部署程序。**
- 這可讓您建置和部署方案中的每個專案做為單一、 可編寫指令碼作業的一部分。
- 環境特定設定是使用簡單的特定環境的專案檔。 相較於使用方案組態 Visual Studio – 以中心方法並將發行設定檔來設定不同的環境的部署，這種方法可讓您設定及管理從 Visual Studio 外部的部署程序。 這表示開發人員不需要進階的連接字串、 服務端點、 伺服器認證和其他的部署變數目的地環境的知識。
- Team Build Team Foundation Server (TFS) 工作流程可以叫用自訂的專案檔。 這可讓您設定 CI 案例的自動化的部署。

**使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 來封裝及部署 web 應用程式專案。**

- Web Deploy 提供一種架構，可讓您封裝及部署您的 web 應用程式內容到目的地 IIS 網頁伺服器，以及相依性、 組態設定、 安全性設定，以及任何其他需求。
- 您可以控制整個的封裝和部署程序，從您自訂的 MSBuild 專案檔內。 您也可以使用隨附於您的 web 部署套件，例如連接字串、 服務端點和 IIS 目的詳細資料的組態設定。
- Web Deploy，Web 發行管線，以及提供許多可讓您自訂您的部署的擴充點。 例如，很容易就能從 web 部署套件中排除不必要的檔案和資料夾。

**使用 VSDBCMD.exe 公用程式來部署和更新資料庫結構描述。**

- VSDBCMD 可讓您部署資料庫結構描述檔案 (.dbschema)，其中建置 Visual Studio 資料庫專案時所產生的資料庫。 相反地，Web Deploy 所含的資料庫部署功能是更適合部署現有的資料庫從本機 SQL Server 執行個體。
- 不同於部署資料庫專案的 Visual Studio 功能，VSDBCMD 可讓您將差異更新部署至現有的目標資料庫。 這可讓您保留任何現有的資料，當您升級資料庫結構描述。
- 您可以執行 VSDBCMD 命令，從您自訂的 MSBuild 專案檔內。

## <a name="content-map"></a>內容對應

本教學課程涵蓋的主題可分為四個主要區域。

這些主題會介紹參考方案&#x2014;連絡人管理員解決方案&#x2014;以及描述如何下載並設定在本機電腦上：

- [連絡管理員解決方案](the-contact-manager-solution.md)
- [設定連絡管理員解決方案](setting-up-the-contact-manager-solution.md)

這些主題會介紹 MSBuild 專案檔，說明如何建立和使用自訂的專案檔，並逐步解說，請連絡管理員方案的部署程序：

- [了解專案檔](understanding-the-project-file.md)
- [了解建置流程](understanding-the-build-process.md)

這些主題會描述 web 應用程式部署，包括如何建置和封裝程序運作、 如何與 Web 發行管線整合建置流程、 如何修改部署的參數，以及如何將 web 套件部署到目的地環境：

- [建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)
- [設定網頁套件部署的參數](configuring-parameters-for-web-package-deployment.md)
- [部署網頁套件](deploying-web-packages.md)

- [部署資料庫專案](deploying-database-projects.md)描述不同的技術，可用來部署 Visual Studio 資料庫專案，以及每一種方法的優缺點。 [建立和執行部署指令檔](creating-and-running-a-deployment-command-file.md)說明如何建立簡單的命令檔封裝部署邏輯，可讓您將複雜的方案部署為單一步驟的程序。
- 最後，[手動安裝 Web 封裝](manually-installing-web-packages.md)總結教學課程，以顯示您將 IIS web 封裝匯入。

## <a name="key-technologies"></a>關鍵技術

本教學課程中的主題主要會使用這些技術來管理組建與部署：

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- VSDBCMD.exe 資料庫部署公用程式

## <a name="other-tutorials-in-this-series"></a>在這一系列其他教學課程

這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。 這些是數列中的其他教學課程：

- [部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此入門內容提供教學課程系列內容的背景。 它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows servers 以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 封裝部署。 它提供有關選擇您自己的環境中，適當的部署方法的指引，並說明如何使用伺服器陣列中的所有 web 伺服器之間複寫部署的 web 應用程式的 Web 伺服陣列架構 (WFF)。
- [設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的部署 CI 程序的一部分，以及手動觸發部署的特定組建。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](the-contact-manager-solution.md)
