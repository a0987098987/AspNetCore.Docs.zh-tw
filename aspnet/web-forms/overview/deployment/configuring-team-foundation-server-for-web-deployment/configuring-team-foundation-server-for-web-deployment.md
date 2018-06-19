---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: 設定 Team Foundation Server Web 部署 |Microsoft 文件
author: jrjlee
description: 本教學課程將說明如何設定 Team Foundation Server (TFS) 2010年建置解決方案，並將 web 內容部署至各種目標環境。 這...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c4cfac333c9400d9ee613ba88520b0b0439873f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892643"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>設定用於 Web 部署的 Team Foundation Server
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程將說明如何設定 Team Foundation Server (TFS) 2010年建置解決方案，並將 web 內容部署至各種目標環境。 這包括持續整合 (CI) 案例中，您在其中部署內容會自動每次在開發人員進行變更。 它也可以包含手動觸發程序案例中，系統管理員可能要部署到預備環境特定組建觸發程序，一旦已驗證和驗證測試環境中建置。 本教學課程中的主題將引導您完成整個組態程序，包括：
> 
> - 如何在 TFS 中建立新的 team 專案。
> - 如何將內容加入至原始檔控制。
> - 如何設定支援 CI 和部署組建伺服器。
> - 如何建立組建定義，其中包含部署邏輯。
> - 如何設定用於自動化部署的權限。
> 
> 這些教學課程的義大利文的翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


本教學課程假設您安裝 TFS 2010，並建立 team 專案集合初始設定程序的一部分。 [For Visual Studio 2010 Team Foundation 安裝指南](https://go.microsoft.com/?linkid=9805132)提供這些工作的廣泛的指引。

## <a name="context"></a>內容

這會形成名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="scenario-overview"></a>情節概觀

這些教學課程的高層級案例所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我們建議您檢閱本主題，在開始此教學課程之前。

## <a name="how-to-use-this-tutorial"></a>如何使用本教學課程

如果這是第一次您已經執行本教學課程中所述的工作，或如果您想要遵循範例使用範例方案，您應該在處理過程中順序的教學課程主題。 或者，您可以使用做為指導的個別主題，適用於特定工作。 本教學課程包含下列主題：

- [在 TFS 中建立 Team 專案](creating-a-team-project-in-tfs.md)。 Team 專案是原始檔控制、 程序管理，以及建置在 TFS 中的核心單位。 您需要建立 team 專案之前，您可以將內容加入至原始檔控制，或建立組建定義。
- [將內容加入至原始檔控制](adding-content-to-source-control.md)。 一旦您已建立 team 專案，您可以開始將內容加入至原始檔控制。 您必須加入您的專案和方案，以及任何外部的相依性，就可以開始設定組建之前。
- [設定 TFS 組建伺服器，用於 Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)。 如果您想要建置您的 team 專案內容，您必須設定組建伺服器。 在大部分情況下，這應該是從您的 TFS 安裝在不同電腦上。 若要設定組建伺服器，您必須安裝及設定 TFS 組建服務，安裝 Visual Studio 2010、 建立組建控制器和組建代理程式，安裝任何產品或程式碼需要為了成功建置，並安裝的元件Internet Information Services (IIS) Web Deployment Tool (Web Deploy)。
- [建立組建定義支援部署](creating-a-build-definition-that-supports-deployment.md)。 您可以啟動佇列或觸發在 TFS 中的組建之前，您需要為您的 team 專案建立至少一個組建定義。 組建定義會定義每個方面的建置，包括哪些項目應該包含在組建中，應該會觸發組建，以及 Team Build 應該傳送位置組建輸出。 您可以設定組建定義，以執行自訂的 Microsoft Build Engine (MSBuild) 專案檔，它可讓您部署邏輯併入您的自動化組建。
- [部署特定的組建](deploying-a-specific-build.md)。 在許多案例中，您會想要將特定的組建，而不是最新的組建部署至目標環境。 在此情況下，您可以設定將從特定的卸除資料夾的內容部署的組建定義。
- [設定的權限給小組建置部署](configuring-permissions-for-team-build-deployment.md)。 如果組建服務是要將內容部署自動化的建置程序的一部分，您需要授與任何目的地 web 伺服器和資料庫伺服器上的組建服務帳戶的各種權限。

## <a name="key-technologies"></a>關鍵技術

本教學課程著重於如何使用這些產品和技術支援自動化的建置和 web 部署：

- Visual Studio Team Foundation Server 2010
- Team Build 和 MSBuild
- Web Deploy

本教學課程也會牽涉到使用 Windows Server 2008 R2、 IIS 7.5、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。

## <a name="other-tutorials-in-this-series"></a>在這一系列其他教學課程

這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。 這些是數列中的其他教學課程：

- [部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此入門內容提供教學課程系列內容的背景。 它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 Web 發行管線 (WPP)、 Web Deploy，和其他相關的技術。 它說明如何使用這些工具一起管理複雜的部署處理程序。
- [用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows servers 以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 封裝部署。 它提供有關選擇您自己的環境中，適當的部署方法的指引，並說明如何使用伺服器陣列中的所有 web 伺服器之間複寫部署的 web 應用程式的 Web 伺服陣列架構 (WFF)。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](creating-a-team-project-in-tfs.md)
