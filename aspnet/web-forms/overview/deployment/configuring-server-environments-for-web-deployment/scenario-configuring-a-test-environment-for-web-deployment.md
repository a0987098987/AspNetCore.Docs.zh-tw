---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 案例： 設定 Web 部署測試環境 |Microsoft Docs
author: jrjlee
description: 本主題說明典型的 web 部署案例中，開發人員或測試環境，並說明您需要完成，才能設定 si 工作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382490"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>案例： 設定 Web 部署測試環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明開發人員在典型的 web 部署案例或測試環境，並說明您需要完成，才能設定類似的環境的工作。


當開發人員開發 web 應用程式時，它們是通常給存取伺服器環境，可用來在實際的設定中測試其應用程式的變更。 這種開發或測試環境通常具有下列特性：

- 環境是由單一 web 伺服器和單一資料庫伺服器所組成。
- 開發人員通常會在伺服器上，讓它們設定環境，其應用程式的需求以有系統管理員權限。
- 對應用程式部署頻繁地，因此必須支援單一步驟的環境，或自動部署。

例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Matt 世昕是 Fabrikam，Inc.的開發人員Matt 使用連絡管理員解決方案，並且需要定期變更部署到測試環境。 Matt 是測試 web 伺服器和測試資料庫伺服器上的系統管理員。 一開始，Matt 必須要能夠直接將方案部署到測試環境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

為工作進展和更多開發人員加入小組成員，這項方案設定持續整合 (CI) 在 Team Foundation Server (TFS) 中的連絡人管理員。 每當開發人員簽入內容，Team Build 應該建置方案、 執行任何單元測試和自動將方案部署到測試環境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>解決方案概觀

測試環境必須支援單一步驟或自動化部署，從遠端電腦，因此您可以選擇兩種主要方式。 您可以：

- 設定測試的 web 伺服器，以支援使用 Web Deployment Agent Service （「 遠端代理程式 」） 的部署。
- 設定測試的 web 伺服器，以支援使用 Web Deploy 處理常式的部署。

> [!NOTE]
> 您也可以使用[On Demand Web 部署](https://technet.microsoft.com/library/ee517345(WS.10).aspx)（「 暫存代理程式 」）。 這是類似於遠端代理程式方法，根據需求和限制。


在此情況下，開發人員，目的地伺服器上具有系統管理員權限，並在測試環境不受限制嚴格的安全性限制，因此合理的選擇是要設定測試的 web 伺服器，以支援使用遠端代理程式的部署。 這是較不複雜，而且需要比 Web 部署的處理常式方法的較低的初始設定。 您也需要設定您的資料庫伺服器，以支援遠端存取和部署。

這些主題會提供您需要才能完成這些工作的所有資訊：

- [設定網頁伺服器，Web deploy 發行 （遠端代理程式）](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 本主題描述如何建置支援 Web Deploy 發行、 使用遠端代理程式的方法，從全新的 Windows Server 2008 R2 建置的 web 伺服器。
- [設定資料庫伺服器，Web deploy 發行](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取和部署，從預設安裝的 SQL Server 2008 R2。

## <a name="further-reading"></a>進一步閱讀

如需設定一般的預備環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 如需設定一般的生產環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一頁](choosing-the-right-approach-to-web-deployment.md)
> [下一頁](scenario-configuring-a-staging-environment-for-web-deployment.md)
