---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 案例： 設定 Web 部署的預備環境 |Microsoft Docs
author: jrjlee
description: 本主題說明典型的 web 部署案例中的預備環境，並說明您需要完成，才能設定類似的環境的工作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: fda4f056eebb77df3e93c63bdd4fabb071c0a0a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365223"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>案例： 設定 Web 部署的預備環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述預備環境的典型的 web 部署案例，並說明您需要完成，才能設定類似的環境的工作。


許多組織會使用預備環境來預覽 web 應用程式或網站的更新。 這可讓組織內的人員有機會探索和站台 」 會上線，"，或也就部署至生產環境之前，檢閱新功能或內容。 預備環境被設計來儘可能接近複寫生產環境，以便提供實際的預覽。 這種預備環境通常具有下列特性：

- 環境是由多個負載平衡的 web 伺服器和一或多個資料庫伺服器，通常與容錯移轉叢集和資料庫鏡像所組成。
- 開發小組或自動 Team Build 的伺服器，可能會以手動方式部署應用程式。
- 部署應用程式的處理序帳戶的使用者不太可能在臨時伺服器上具有系統管理員權限。
- 對應用程式部署頻繁地，因此必須支援單一步驟的環境，或自動部署。

> [!NOTE]
> 擴充資料庫部署到多部伺服器，已超出本教學課程的範圍。 如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/library/ms130214.aspx)。


例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Foundation Server (TFS) 中管理連絡管理員解決方案。 TFS 系統管理員，Rob Walters 已建立組建定義，讓觸發部署至預備環境所需的開發人員。

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

請注意，在大部分情況下，不一定要將最新的組建部署到預備環境。 相反地，您更容易想要部署特定的組建尚未經過驗證和測試環境中的驗證。

## <a name="solution-overview"></a>解決方案概觀

在此案例中，您可以推算的部署需求分析這些事實：

- 執行部署的使用者或處理序帳戶不會對系統管理員權限的預備伺服器，因此預備的 web 伺服器必須支援非系統管理員部署。 因此，您必須設定臨時 web 伺服器來使用 Web 部署處理常式，而不是遠端代理程式。
- 預備環境包含多個 web 伺服器，但它必須支援一種單鍵或自動化的部署，因此您必須使用建立伺服器陣列的 Web 伺服陣列架構 (WFF)。 使用這個方法，您可以部署一個網頁伺服器 （主要伺服器），應用程式，而且 WFF 會複寫所有其他 web 伺服器上的預備環境中部署。
- 執行部署處理序帳戶的使用者必須具有建立資料庫的權限。 因此，您必須將帳戶加入**dbcreator**在資料庫伺服器上，除了設定為支援遠端存取及部署資料庫伺服器的伺服器角色。

這些主題會提供您需要才能完成這些工作的所有資訊：

- [使用 Web 伺服陣列架構建立伺服器陣列](creating-a-server-farm-with-the-web-farm-framework.md)。 本主題描述如何建立及設定使用 WFF，伺服器陣列，讓 web 平台產品和元件、 組態設定，以及網站和應用程式會複寫到多個負載平衡的 web 伺服器。
- [Web deploy 發行設定網頁伺服器 (Web Deploy 處理常式)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 本主題描述如何建置支援 Web Deploy 發行、 使用遠端代理程式的方法，從全新的 Windows Server 2008 R2 建置的 web 伺服器。
- [設定資料庫伺服器，Web deploy 發行](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取和部署，從預設安裝的 SQL Server 2008 R2。

## <a name="further-reading"></a>進一步閱讀

如需設定的一般開發人員測試環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)。 如需設定一般的生產環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一頁](scenario-configuring-a-test-environment-for-web-deployment.md)
> [下一頁](scenario-configuring-a-production-environment-for-web-deployment.md)
