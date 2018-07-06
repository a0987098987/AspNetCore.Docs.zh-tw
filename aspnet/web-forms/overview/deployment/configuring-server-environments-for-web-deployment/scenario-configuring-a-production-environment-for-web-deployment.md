---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 案例： 設定 Web 部署的生產環境 |Microsoft Docs
author: jrjlee
description: 本主題描述在生產環境的典型的 web 部署案例，並說明您需要完成，才能設定類似的工作...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: dc16b2fadec8051f78d13ffeb97abf2d9e6930f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806709"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>案例： 設定 Web 部署的生產環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述在生產環境的典型的 web 部署案例，並說明您需要完成，才能設定類似的環境的工作。


生產環境是 web 應用程式或網站的最終目的地。 此時，您的應用程式是透過測試、 已部署至預備環境，和已準備好 「 線上授權。 」 生產環境的特性可以根據本質和 web 內容的目的，您的組織，目標對象，以及許多其他因素的大小範圍相當廣泛。 在企業規模案例中，生產環境可能具有下列特性：

- 環境是由多個負載平衡的 web 伺服器和一或多個資料庫伺服器，通常與容錯移轉叢集和資料庫鏡像所組成。
- 如果環境是網際網路對向，它可能是從內部部署網路隔離。 它可能是不同的子網路周邊網路中，可能在不同網域中，而且可能是完全不同的網路基礎結構上。
- 開發人員和組建伺服器處理序帳戶是不太可能在實際執行伺服器上具有系統管理員權限。
- 較不頻繁地比測試或預備環境部署上部署應用程式的變更。

> [!NOTE]
> 擴充資料庫部署到多部伺服器，已超出本教學課程的範圍。 如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/library/ms130214.aspx)。


例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Build server 包含可讓使用者建立連絡管理員解決方案，並將它部署至預備環境中單一步驟的組建定義。 在應用程式隨時可供部署到生產環境，因為安全性需求和網路基礎結構中，所加諸的條件約束時，生產環境系統管理員必須手動複製到實際執行 web 伺服器上的 web 套件並匯入它透過 Internet Information Services (IIS) 管理員。

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>解決方案概觀

在此案例中，您可以推算的部署需求分析這些事實：

- 由於安全性限制和網路設定，您無法設定生產環境，以支援一種單鍵或自動部署。 離線部署是唯一可行的方法，在此案例中。
- 生產環境中將包含多個 web 伺服器，因此您可以使用 Web 伺服陣列架構 (WFF) 來建立伺服器陣列。 使用這個方法，系統管理員只需要匯入至一個網頁伺服器 （主要伺服器），應用程式和 WFF 會複寫所有其他 web 伺服器上的生產環境中部署。

這些主題會提供您需要才能完成這些工作的所有資訊：

- [使用 Web 伺服陣列架構建立伺服器陣列](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何建立及設定使用 WFF，伺服器陣列，讓 web 平台產品和元件、 組態設定，以及網站和應用程式會複寫到多個負載平衡的 web 伺服器。
- [設定網頁伺服器，Web deploy 發行 （離線部署）](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。 本主題描述如何建置 web 伺服器，可讓系統管理員匯入和部署網頁套件以手動的方式，從全新的 Windows Server 2008 R2 組建。
- [設定資料庫伺服器，Web deploy 發行](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取和部署，從預設安裝的 SQL Server 2008 R2。

## <a name="further-reading"></a>進一步閱讀

如需設定的一般開發人員測試環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)。 如需設定一般的預備環境的指引，請參閱 <<c0> [ 案例： 設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一頁](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [下一頁](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
