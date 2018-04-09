---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: 設定伺服器環境，用於 Web 部署 |Microsoft 文件
author: jrjlee
description: 本教學課程顯示如何將伺服器環境以支援一種單鍵或自動化、 部署網站和各種不同的畫面中的發行設定...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>用於 Web 部署設定伺服器環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教學課程將說明如何設定伺服器環境以支援一種單鍵或自動化、 部署網站和各種不同案例中的發行。 本教學課程包含的主題，以引導您完成各種工作，例如設定 web 伺服器，以支援特定的方法，來部署和設定 Web 伺服陣列架構 (WFF) 伺服器的伺服器陣列，以及提供以案例為基礎的概觀較高層級的端對端指引。
> 
> 本教學課程會使用 Fabrikam，Inc.的部署案例中所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)作為範例和網路基礎結構的參考點。
> 
> 這些教學課程的義大利文的翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


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

第一個主題，[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)，描述主要的方法可用於發行 web 應用程式使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 2.0。 它也可以識別對應至每一種方法的案例。 從這裡開始，每個案例主題提供您需要完成的工作的高階概觀，並識別您將需要透過運作來協助您完成這些工作的主題。

如果您使用分割專案檔案描述的方法中[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)建置和部署方案時，最後一個主題，[設定部署屬性的目標環境](configuring-deployment-properties-for-a-target-environment.md)，說明如何設定以部署至不同目的地環境的特定環境的專案檔。

## <a name="key-technologies"></a>關鍵技術

本教學課程著重於如何使用這些產品和技術，支援 web 部署：

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- IIS Web 管理服務 (WMSvc)

本教學課程也會牽涉到使用 Windows Server 2008 R2、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。

## <a name="other-tutorials-in-this-series"></a>在這一系列其他教學課程

這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。 這些是數列中的其他教學課程：

- [部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 此入門內容提供教學課程系列內容的背景。 它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。
- [Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 Microsoft Build Engine (MSBuild) 專案檔的概念性簡介、 Web 發行管線、 Web Deploy，和其他相關的技術。 它說明如何使用這些工具一起管理複雜的部署處理程序。
- [設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 Team Foundation Server (TFS) 支援各種部署案例，包括持續整合 (CI) 程序的一部分的自動的部署，以及手動觸發部署的特定的組建。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.

> [!div class="step-by-step"]
> [下一步](choosing-the-right-approach-to-web-deployment.md)
