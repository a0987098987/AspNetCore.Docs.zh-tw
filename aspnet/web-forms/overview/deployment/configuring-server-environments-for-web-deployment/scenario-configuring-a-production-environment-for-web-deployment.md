---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 案例： 設定的生產環境 Web 部署 |Microsoft 文件
author: jrjlee
description: 本主題說明在實際執行環境的一般 web 部署案例，並說明您必須完成才能設定類似的工作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>案例： 設定用於 Web 部署的實際執行環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明在實際執行環境的一般 web 部署案例，並說明您必須完成才能相似的環境所設定的工作。


在實際執行環境是針對 web 應用程式或網站的最終目的地。 此時，您的應用程式已透過測試、 已部署至預備環境，並準備好"上線。 」 在實際執行環境的特性可能會依廣泛的本質和 web 內容的目的，您的組織，您的目標對象和許多其他因素的大小。 在企業規模案例中，實際執行環境可能具有下列特性：

- 環境是由多個負載平衡 web 伺服器和一或多個資料庫伺服器，通常搭配容錯移轉叢集和鏡像資料庫所組成。
- 如果環境是網際網路對向，很可能會從您的內部網路隔離。 也可能位於周邊網路中不同的子網路上，可能在不同網域中，並可能完全不同的網路基礎結構上。
- 開發人員和組建伺服器處理序帳戶是不太可能會在實際執行伺服器上具有系統管理員權限。
- 對應用程式會部署較不頻繁地比測試或預備環境部署。

> [!NOTE]
> 擴充資料庫部署到多部伺服器已超出本教學課程的範圍。 如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/library/ms130214.aspx)。


例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Build server 包含讓使用者建立連絡人管理員解決方案，並將它部署到預備環境中單一步驟的組建定義。 當應用程式隨時可供部署至生產環境，因為安全性需求和網路基礎結構中，所加諸的條件約束的實際執行環境的系統管理員必須手動複製到實際執行 web 伺服器上的 web 套件並匯入它透過網際網路資訊服務 (IIS) 管理員。

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>解決方案概觀

在此案例中，您可以減少從部署需求的分析這些事實：

- 由於安全性限制和網路設定，您無法設定以支援單鍵或自動部署實際執行環境。 離線的部署是唯一可行的方法，在此案例中。
- 實際執行環境含有多部網頁伺服器，因此您可以使用 Web 伺服陣列架構 (WFF) 來建立伺服器陣列。 使用這個方法，系統管理員只需要一個 web 伺服器 （主要伺服器），應用程式匯入和 WFF 會複寫所有其他 web 伺服器上實際執行環境中部署。

這些主題提供您需要才能完成這些工作的所有資訊：

- [建立伺服器陣列與 Web 伺服陣列架構](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何建立及設定伺服器陣列使用 WFF，以便跨多個負載平衡 web 伺服器複寫的 web platform 產品和元件、 組態設定和網站和應用程式。
- [設定 Web 伺服器的 Web Deploy 發行 （離線部署）](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。 本主題描述如何建置網頁伺服器，可讓系統管理員匯入和部署 web 封裝，手動從乾淨的 Windows Server 2008 R2 組建開始。
- [設定資料庫伺服器的 Web Deploy 發行變更](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取，以及部署中，從預設安裝的 SQL Server 2008 R2 開始。

## <a name="further-reading"></a>進一步閱讀

如需設定的一般開發人員測試環境的指引，請參閱[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。 如需設定一般的預備環境的指引，請參閱[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一頁](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [下一頁](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
