---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "案例： 設定預備環境，用於 Web 部署 |Microsoft 文件"
author: jrjlee
description: "本主題描述預備環境的一般 web 部署案例，並說明您需要完成，才能設定類似 env 工作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b5f223f59a8b222f4f01322d228cf7434e3dfc14
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>案例： 設定用於 Web 部署的預備環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述預備環境的一般 web 部署案例，並說明您必須完成才能相似的環境所設定的工作。


許多組織會使用預備環境來預覽更新 web 應用程式或網站。 這可讓組織內的人員有機會瀏覽和站台 」 變成作用中 」 或換句話說部署到生產環境之前，檢閱新功能或內容。 預備環境被為了盡可能，以便提供實際的預覽複寫實際執行環境。 這種執行環境通常具有下列特性：

- 環境是由多個負載平衡 web 伺服器和一或多個資料庫伺服器，通常搭配容錯移轉叢集和鏡像資料庫所組成。
- 由開發小組或自動由 Team Build 的伺服器應用程式可能以手動方式部署。
- 部署應用程式的處理序帳戶的使用者不太可能在臨時伺服器上具有系統管理員權限。
- 對應用程式部署頻繁地，所以環境必須支援單一步驟或自動部署。

> [!NOTE]
> 擴充資料庫部署到多部伺服器已超出本教學課程的範圍。 如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/en-us/library/ms130214.aspx)。


例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Foundation Server (TFS) 管理連絡人管理員解決方案。 TFS 系統管理員，Rob 郭已經建立可讓開發人員觸發部署到預備環境所需的組建定義。

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

請注意，在大部分情況下，您不一定要將最新組建部署到預備環境。 相反地，您有更多可能想要部署特定的組建已經經過驗證和測試環境中的驗證。

## <a name="solution-overview"></a>解決方案概觀

在此案例中，您可以減少從部署需求的分析這些事實：

- 執行部署的使用者或處理序帳戶將不會有管理員權限臨時伺服器，因此臨時 web 伺服器必須支援非系統管理員部署。 因此，您必須設定臨時 web 伺服器來使用 Web 部署處理常式，而不是遠端代理程式。
- 預備環境包含多個 web 伺服器，但必須支援一種單鍵或自動化部署，因此您必須要用來建立伺服器陣列的 Web 伺服陣列架構 (WFF)。 使用這個方法，您可以部署一個網頁伺服器 （主要伺服器），應用程式，而且 WFF 會複寫所有其他 web 伺服器上的預備環境中部署。
- 使用者或執行部署的處理序帳戶必須具有建立資料庫的權限。 因此，您必須將帳戶加入**dbcreator**資料庫伺服器，除了設定支援遠端存取和部署資料庫伺服器上的伺服器角色。

這些主題提供您需要才能完成這些工作的所有資訊：

- [建立伺服器陣列與 Web 伺服陣列架構](creating-a-server-farm-with-the-web-farm-framework.md)。 本主題描述如何建立及設定伺服器陣列使用 WFF，以便跨多個負載平衡 web 伺服器複寫的 web platform 產品和元件、 組態設定和網站和應用程式。
- [設定 Web Deploy 發行變更網頁伺服器 (Web Deploy 處理常式)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 本主題描述如何建立支援 Web Deploy 發行，使用遠端代理程式的方法，從乾淨的 Windows Server 2008 R2 組建開始的 web 伺服器。
- [設定資料庫伺服器的 Web Deploy 發行變更](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取，以及部署中，從預設安裝的 SQL Server 2008 R2 開始。

## <a name="further-reading"></a>進一步閱讀

如需設定的一般開發人員測試環境的指引，請參閱[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。 如需設定的標準生產環境的指引，請參閱[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一頁](scenario-configuring-a-test-environment-for-web-deployment.md)
[下一頁](scenario-configuring-a-production-environment-for-web-deployment.md)
