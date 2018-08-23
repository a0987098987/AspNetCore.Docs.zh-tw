---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: 設定 Web 部署的伺服器環境 |Microsoft Docs
author: jrjlee
description: 本教學課程將示範如何設定伺服器環境，只要按一下，或自動化的支援網站部署，各種不同的畫面中的發行...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4f6433f0b8a9ad3b3634c9bcd8d95015eebaa865
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833268"
---
<a name="configuring-server-environments-for-web-deployment"></a>設定 Web 部署的伺服器環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程會示範如何設定伺服器環境，以支援一種單鍵或自動化、 網站部署和發佈各種不同的案例中。 本教學課程包含的主題，以引導您完成各種工作，例如設定 web 伺服器，以支援特定的方法，來部署及設定 Web 伺服陣列架構 (WFF) 伺服器陣列，以及提供以案例為基礎的概觀較高層級的端對端指引。
> 
> 本教學課程會使用 Fabrikam，Inc.部署案例中所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)作為參考點，如需範例和網路基礎結構。
> 
> 這些教學課程義大利文翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


本教學課程包含下列主題：

- [選擇 Web 部署的正確方法](choosing-the-right-approach-to-web-deployment.md)
- [案例：設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)
- [案例：設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [案例：設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)
- [設定 Web Deploy 發行的網頁伺服器 (遠端代理程式)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [設定 Web Deploy 發行的網頁伺服器 (Web Deploy 處理常式)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [設定 Web Deploy 發行的網頁伺服器 (離線部署)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [設定 Web Deploy 發行的資料庫伺服器](configuring-a-database-server-for-web-deploy-publishing.md)
- [使用 Web 伺服陣列架構建立伺服器陣列](creating-a-server-farm-with-the-web-farm-framework.md)
- [設定目標環境的部署屬性](configuring-deployment-properties-for-a-target-environment.md)

第一個主題中，[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)，描述主要的方法可用來發佈 web 應用程式，使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0。 它也會識別對應至每一種方法的案例。 從這裡開始，每個案例的主題會提供您需要完成的工作的高階概觀，並識別您要透過運作，可協助您完成這些工作的主題。

如果您使用分割的專案檔案方法中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)來建置及部署您的方案，最後一個主題中，[設定部署屬性的目標環境](configuring-deployment-properties-for-a-target-environment.md)，說明如何設定以部署至不同目的地環境的特定環境的專案檔。

## <a name="key-technologies"></a>主要技術

本教學課程著重於如何使用這些產品和技術來支援 web 部署：

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- IIS Web 管理服務 (WMSvc)

本教學課程也會牽涉到 Windows Server 2008 R2 及 SQL Server 2008 R2、 ASP.NET 4.0 ASP.NET MVC 3 的使用。

## <a name="other-tutorials-in-this-series"></a>在這一系列的其他教學課程

這會形成企業級 web 部署的一系列五個教學課程的一部分。 這些是系列中的其他教學課程：

- [部署 Web 應用程式，在企業案例](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此介紹的內容會提供教學課程系列中的內容相關的背景。 其中說明教學課程的案例中，並說明了如何工作和逐步解說所述，在整個系列納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 Microsoft Build Engine (MSBuild) 專案檔的概念性簡介、 Web 發行管線、 Web Deploy 和其他相關的技術。 它說明如何使用這些工具搭配來管理複雜的部署程序。
- [設定 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 Team Foundation Server (TFS) 支援各種部署案例，包括持續整合 (CI) 程序的一部分自動的部署，並手動觸發的特定組建的部署。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種進階的部署工作，例如自訂多個環境的資料庫部署、 從部署中排除檔案和資料夾以及部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](choosing-the-right-approach-to-web-deployment.md)
