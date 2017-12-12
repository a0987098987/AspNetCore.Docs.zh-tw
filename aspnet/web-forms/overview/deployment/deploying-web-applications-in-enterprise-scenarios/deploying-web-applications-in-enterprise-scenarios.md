---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: "部署 Web 應用程式中使用 Visual Studio 2010 的企業案例 |Microsoft 文件"
author: jrjlee
description: "這組教學課程描述工具和技術可用來部署 web 應用程式中各種企業案例。 它說明如何充分運用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>部署 Web 應用程式中使用 Visual Studio 2010 的企業案例
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 這組教學課程描述工具和技術可用來部署 web 應用程式中各種企業案例。 它說明如何進行有效運用的技術，像是 Visual Studio 2010、 Microsoft Build Engine (MSBuild)、 網際網路資訊服務 (IIS) 7.5、 IIS Web Deployment Tool (Web Deploy)、 Web Farm Framework (WFF) 和 VSDBCMD.exe 以類似的公用程式簡化，並管理部署程序。 它包括概念性概觀和以工作為導向的指引，協助您：
> 
> - 檢閱並建立企業級 web 應用程式的部署需求。
> - 設定測試、 預備及生產環境 web 伺服器環境以支援 web 部署。
> - 設定 Team Foundation Server (TFS) 的持續整合 (CI) 程序，來支援自動化的 web 部署。
> - 企業級 web 應用程式部署到不同的伺服器環境與不同的需求和限制。
> - 將變更部署到不同的伺服器環境中執行的 web 應用程式。
> 
> > [!NOTE]
> > 雖然這些教學課程描述 TFS 作為 CI 伺服器，本指南是輕鬆地調整 CI 的任何伺服器。 您不需要詳細的知識的了解並運用教學課程的 TFS。
> 
> 
> 這些教學課程的義大利文的翻譯，請瀏覽[http://www.lucamorelli.it](http://www.lucamorelli.it)。


## <a name="about-the-authors"></a>關於作者

Jason Lee 會與主體 concentrated technology[內容 Master](http://www.contentmaster.com/)他有已使用的儲存和 Microsoft 產品與技術，尤其是 SharePoint 和 ASP.NET，好幾年。 Jason 保存計算博士而且目前 MCPD 和 MCTS 認證。 您可以閱讀 Jason 的技術部落格在[www.jrjlee.com](http://www.jrjlee.com/)。

Benjamin Curry 會與主體 concentrated technology[內容 Master](http://www.contentmaster.com/)誰具有生涯期間寫入白皮書、 SDK 文件、 PowerPoint 簡報和講師帶領和線上訓練課程。 原始成員的 ASP.NET 文件集小組，他曾與 Microsoft 的 web 技術，用於透過十年以上。

## <a name="target-audience"></a>目標對象

這組教學課程適用於 ASP.NET web 應用程式開發人員和方案架構設計人員使用 Visual Studio 2010 建立企業級 web 應用程式。 若要從內容取得大部分的值，您應該能夠使用 Visual Studio 2010，請與 TFS，以及在得知 ASP.NET MVC 3、 Windows Communication Foundation (WCF)、 IIS、 SQL 等的 Microsoft web 平台技術的基本的認識伺服器和 Visual Studio 資料庫專案。 不過，您不需要熟悉部署工具與技術或需要知道如何設定 CI 系統。

## <a name="requirements"></a>需求

若要遵循逐步解說，並執行這些教學課程描述的工作，您將需要在開發電腦上安裝此軟體：

- Visual Studio 2010 Premium 或 Ultimate Edition 含 Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 Service pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

若要執行這些逐步解說中所描述的部署步驟，您必須能夠存取範例 Web 應用程式的部署環境。 為獲得最佳結果，這些環境中應該會反映您組織的企業部署模式。 然後，您可以修改此文件，以反映您組織的需求與部署環境中提供的逐步解說。

## <a name="series-contents"></a>序列內容

本節介紹包含兩個其他的主題。 這些設計來提供更廣泛的一些內容，為遵循教學課程：

- [企業 Web 部署： 案例概觀](enterprise-web-deployment-scenario-overview.md)。 本主題描述可以加強這個數列的教學課程的每個案例。 案例著眼於名為 Fabrikam，Inc.，為其開發企業規模的 web 應用程式的虛構公司的應用程式生命週期管理 (ALM) 需求。
- [應用程式生命週期管理： 從開發到生產環境](application-lifecycle-management-from-development-to-production.md)。 本主題提供部署程序的高層級、 端對端概觀。 它說明 Fabrikam，Inc.如何移動企業規模 ASP.NET web 應用程式透過測試、 預備及生產環境持續開發程序的一部分。

系列中包括四個教學課程的集合。 每個著重於 web 部署的不同層面：

- [Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 Web 發行管線、 Web Deploy，和其他相關的技術。 它說明如何使用這些工具一起管理複雜的部署處理程序。
- [用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows servers 以支援各種部署案例，包括使用 Web Deployment Agent Service （「 遠端代理程式 」） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 封裝部署。 它提供有關選擇您自己的環境中，適當的部署方法的指引，並說明如何使用 WFF 將已部署的 web 應用程式複寫到伺服器陣列中的所有網頁伺服器。
- [設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的部署 CI 程序的一部分，以及手動觸發部署的特定組建。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.

## <a name="where-to-start"></a>要從何處開始

這組教學課程會使用範例方案實際的層級的複雜性，以及虛構的企業部署案例中，以提供參考實作，以及提供的工作和逐步解說的一般內容。 下一個主題[企業 Web 部署： 案例概觀](enterprise-web-deployment-scenario-overview.md)，介紹案例和範例方案。 從該處，您可以透過教學課程和主題最符合您的需求。

>[!div class="step-by-step"]
[下一步](enterprise-web-deployment-scenario-overview.md)
