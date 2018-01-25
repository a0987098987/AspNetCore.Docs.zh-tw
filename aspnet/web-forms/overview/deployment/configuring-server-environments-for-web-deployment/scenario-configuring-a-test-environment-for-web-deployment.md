---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "案例： 設定測試環境，用於 Web 部署 |Microsoft 文件"
author: jrjlee
description: "本主題說明開發人員在典型的 web 部署案例或測試環境，並說明您需要完成，才能設定 si 工作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 23e317c6e0b6daf2d7937b73738e5cb6fa32cde2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>案例： 用於 Web 部署設定測試環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明開發人員在典型的 web 部署案例或測試環境，並說明您必須完成才能相似的環境所設定的工作。


當開發人員處理 web 應用程式時，它們通常會獲得存取它們可用來在實際的設定中測試變更他們的應用程式伺服器環境。 這種開發或測試環境通常具有下列特性：

- 環境是由單一網頁伺服器和單一資料庫伺服器所組成。
- 開發人員通常會有管理員權限，讓他們設定環境，以其應用程式的需求，伺服器上。
- 對應用程式部署頻繁地，所以環境必須支援單一步驟或自動部署。

例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Matt 世昕是 Fabrikam，Inc.的開發人員Matt 連絡人管理員方案，並定期將變更部署到測試環境需求。 Matt 是測試 web 伺服器和測試資料庫伺服器上的系統管理員。 一開始，Matt 需要能夠直接將方案部署到測試環境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

為工作進展，開發人員加入小組，解決方案會設定持續整合 (CI) Team Foundation Server (TFS) 中的連絡管理員。 每當開發人員會檢查內容中，Team Build 應該建置此方案、 執行任何單元測試和自動將方案部署到測試環境。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>解決方案概觀

測試環境必須支援單一步驟或自動化部署從遠端電腦，因此您可以選擇兩種主要方法。 您可以：

- 設定測試的 web 伺服器，以支援使用 Web Deployment Agent Service （「 遠端代理程式 」） 的部署。
- 設定測試的 web 伺服器，以支援使用 Web Deploy 的處理常式的部署。

> [!NOTE]
> 您也可以使用[Web 部署隨選](https://technet.microsoft.com/library/ee517345(WS.10).aspx)（「 暫存代理程式 」）。 這是類似的遠端代理程式方法，根據需求和限制條件。


在此情況下，開發人員，目的地伺服器上具有系統管理員權限和測試環境不是具有嚴格的安全性限制，因此合理的選擇是要設定測試的 web 伺服器，以支援使用遠端代理程式部署。 這是較不複雜，因此需要比 Web 部署的處理常式方法的較低的初始設定。 您還需要設定您的資料庫伺服器，以支援遠端存取和部署。

這些主題提供您需要才能完成這些工作的所有資訊：

- [設定 Web 伺服器的 Web Deploy 發行 （遠端代理程式）](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 本主題描述如何建立支援 Web Deploy 發行，使用遠端代理程式的方法，從乾淨的 Windows Server 2008 R2 組建開始的 web 伺服器。
- [設定資料庫伺服器的 Web Deploy 發行變更](configuring-a-database-server-for-web-deploy-publishing.md)。 本主題描述如何設定資料庫伺服器以支援遠端存取，以及部署中，從預設安裝的 SQL Server 2008 R2 開始。

## <a name="further-reading"></a>進一步閱讀

如需設定一般的預備環境的指引，請參閱[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 如需設定的標準生產環境的指引，請參閱[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)。

>[!div class="step-by-step"]
[上一頁](choosing-the-right-approach-to-web-deployment.md)
[下一頁](scenario-configuring-a-staging-environment-for-web-deployment.md)
