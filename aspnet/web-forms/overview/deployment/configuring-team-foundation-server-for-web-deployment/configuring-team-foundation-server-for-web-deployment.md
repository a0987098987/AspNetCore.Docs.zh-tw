---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: 設定 Web 部署的 Team Foundation Server |Microsoft Docs
author: jrjlee
description: 本教學課程會示範如何設定 Team Foundation Server (TFS) 2010年建置解決方案，並將 web 內容部署至各種目標環境。 這...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6430a96a8e430a8a30d062ec22868de829680806
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365337"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>設定 Web 部署的 Team Foundation Server
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程會示範如何設定 Team Foundation Server (TFS) 2010年建置解決方案，並將 web 內容部署至各種目標環境。 這包括持續整合 (CI) 案例中，您在其中部署內容會自動每次為開發人員進行變更。 它也可以包含手動觸發程序的情況下，系統管理員可能要觸發部署至預備環境之特定組建，一旦確認並驗證測試環境中建置。 在本教學課程的主題將引導您完成整個組態程序，包括：
> 
> - 如何在 TFS 中建立新的 team 專案。
> - 如何將內容新增至原始檔控制。
> - 如何設定為支援 CI 及部署組建伺服器。
> - 如何建立組建定義，其中包含部署邏輯。
> - 如何設定自動化部署的權限。
> 
> 這些教學課程義大利文翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


本教學課程假設您安裝 TFS 2010，並建立 team 專案集合初始設定程序的一部分。 [適用於 Visual Studio 2010 Team Foundation 安裝指南](https://go.microsoft.com/?linkid=9805132)提供這些工作的完整指導。

## <a name="context"></a>內容

這會形成名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程系列的一部分本系列教學課程使用範例解決方案&#x2014; [Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="scenario-overview"></a>情節概觀

高階的案例，這些教學課程中所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我們建議您檢閱本主題，再開始本教學課程。

## <a name="how-to-use-this-tutorial"></a>如何使用本教學課程

如果這是第一次您執行本教學課程中所述的工作，或如果您想要遵循的使用範例方案的範例，您應該逐步教學課程主題中的順序。 或者，您可以使用做為指導的個別主題，適用於特定工作。 本教學課程包含下列主題：

- [在 TFS 中建立 Team 專案](creating-a-team-project-in-tfs.md)。 Team 專案是原始檔控制、 程序管理，以及在 TFS 中的組建的核心單位。 您需要建立 team 專案，您可以將內容加入至原始檔控制，或建立組建定義之前。
- [將內容新增至原始檔控制](adding-content-to-source-control.md)。 一旦您建立 team 專案，您可以開始將內容新增至原始檔控制。 您必須加入您的專案和方案，以及任何外部的相依性，才能開始設定組建。
- [設定 TFS 組建伺服器，Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)。 如果您想要建置您的 team 專案內容，您必須設定組建伺服器。 在大部分情況下，這應該是從您的 TFS 安裝在不同電腦上。 若要設定組建伺服器，您需要安裝及設定 TFS 組建服務，安裝 Visual Studio 2010 中，建立組建控制器和組建代理程式，安裝任何產品或您的程式碼能夠成功建置，並安裝的元件Internet Information Services (IIS) Web Deployment Tool (Web Deploy)。
- [建立組建定義支援部署](creating-a-build-definition-that-supports-deployment.md)。 您可以在佇列或觸發在 TFS 組建啟動之前，您需要建立 team 專案的至少一個組建定義。 組建定義會定義組建，包括哪些項目應該包含在組建中，應該會觸發組建，以及 Team Build 應該其中傳送組建輸出的各個層面。 您可以設定組建定義，執行自訂的 Microsoft Build Engine (MSBuild) 專案檔，可讓您將部署邏輯包含在您的自動化組建。
- [部署特定的組建](deploying-a-specific-build.md)。 在許多案例中，您會想要將特定的組建，而不是最新的組建部署至目標環境。 在此情況下，您可以設定將從特定的置放資料夾的內容部署的組建定義。
- [設定權限的小組組建部署](configuring-permissions-for-team-build-deployment.md)。 如果組建服務將內容部署自動化的建置程序的一部分，您需要授與至任何目的地 web 伺服器和資料庫伺服器上組建服務帳戶的各種權限。

## <a name="key-technologies"></a>主要技術

本教學課程著重於如何使用這些產品和技術來支援自動化的建置和 web 部署：

- Visual Studio Team Foundation Server 2010
- Team Build 和 MSBuild
- Web Deploy

本教學課程也會牽涉到使用 Windows Server 2008 R2、 IIS 7.5、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。

## <a name="other-tutorials-in-this-series"></a>在這一系列的其他教學課程

這會形成企業級 web 部署的一系列五個教學課程的一部分。 這些是系列中的其他教學課程：

- [部署 Web 應用程式，在企業案例](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此介紹的內容會提供教學課程系列中的內容相關的背景。 其中說明教學課程的案例中，並說明了如何工作和逐步解說所述，在整個系列納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 Web 發行管線 (WPP)、 Web Deploy 和其他相關的技術。 它說明如何使用這些工具搭配來管理複雜的部署程序。
- [設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows 伺服器以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 套件部署。 它提供指導方針選擇適當的部署方法，為您自己的環境，並說明如何使用 Web 伺服陣列架構 (WFF) 伺服器陣列中的所有 web 伺服器之間複寫已部署的 web 應用程式。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種進階的部署工作，例如自訂多個環境的資料庫部署、 從部署中排除檔案和資料夾以及部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](creating-a-team-project-in-tfs.md)
