---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: 部署 Web 應用程式，在企業案例中使用 Visual Studio 2010 |Microsoft Docs
author: jrjlee
description: 這套教學課程說明的工具和技術可用來部署 web 應用程式在各種不同的企業案例。 它說明如何充分利用...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: b55aeb863694ca3ac71f2a1a46d11981e178dcb4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816583"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>使用 Visual Studio 2010 的企業案例中的 部署 Web 應用程式
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 這套教學課程說明的工具和技術可用來部署 web 應用程式在各種不同的企業案例。 它說明如何讓最好使用 Visual Studio 2010、 Microsoft Build Engine (MSBuild)、 Internet Information Services (IIS) 7.5、 IIS Web Deployment Tool (Web Deploy)、 Web 伺服陣列架構 (WFF) 和公用程式，如要 VSDBCMD.exe 之類的技術簡化及管理在部署程序。 它包含概念性概觀和工作導向的指導方針，可協助您：
> 
> - 檢閱並建立企業級 web 應用程式的部署需求。
> - 設定以支援 web 部署的測試、 預備及生產環境 web 伺服器環境。
> - 設定 Team Foundation Server (TFS) 的持續整合 (CI) 程序，來支援自動化的 web 部署。
> - 企業級 web 應用程式部署到不同的伺服器環境使用不同的需求和限制。
> - 變更部署到不同的伺服器環境中執行的 web 應用程式。
> 
> > [!NOTE]
> > 雖然這些教學課程說明如何使用 TFS 作為 CI 伺服器時，本指南是輕鬆地調整任何 CI 伺服器。 您不需要深入了解 TFS，以了解並運用教學課程。
> 
> 
> 這些教學課程義大利文翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


## <a name="about-the-authors"></a>關於作者

Jason Lee 是使用的首席技術專家[內容主要](http://www.contentmaster.com/)，他致力與 Microsoft 產品和技術，尤其是 SharePoint 和 ASP.NET，好幾年。 Jason 擁有 mis 博士運算中的，目前 MCPD 和 MCTS 認證。 您可以閱讀 Jason 的技術部落格，網址[www.jrjlee.com](http://www.jrjlee.com/)。

Benjamin Curry 是使用的首席技術專家[內容主要](http://www.contentmaster.com/)人員已在他的職涯期間寫入白皮書、 SDK 文件、 PowerPoint 簡報和講師和線上訓練課程。 原始成員的 ASP.NET 文件集小組，他曾經與 Microsoft 的 web 技術，針對超過十年。

## <a name="target-audience"></a>目標對象

這套教學課程適用於 ASP.NET web 應用程式開發人員和解決方案架構設計人員使用 Visual Studio 2010 建立企業級 web 應用程式的人員。 若要從內容取得最大的價值，您應該熟悉使用 Visual Studio 2010，並有基本的熟悉 TFS，以及 ASP.NET MVC 3、 Windows Communication Foundation (WCF)、 IIS、 SQL 等 Microsoft web 平台技術的情況下伺服器和 Visual Studio 資料庫專案。 不過，您不需要先熟悉部署工具和技術，或需要了解如何設定 CI 系統。

## <a name="requirements"></a>需求

若要遵循逐步解說，並執行這些教學課程中說明的工作，您必須在您的開發電腦上安裝此軟體：

- Visual Studio 2010 Premium 或 Ultimate Edition Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 Service pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

若要執行這些逐步解說中所描述的部署步驟，您必須能夠存取範例 Web 應用程式的部署環境。 為了獲得最佳結果，這些環境應該反映您組織的企業部署模式。 然後，您可以修改此文件，以反映您組織的需求與部署環境中提供的逐步解說。

## <a name="series-contents"></a>系列內容

本節將介紹性是由兩個其他主題所組成。 這些專為提供的教學課程，請遵循的一些更廣泛的內容：

- [企業 Web 部署： 案例概觀](enterprise-web-deployment-scenario-overview.md)。 本主題說明能在這一系列的教學課程的每個支援的案例。 案例著重於名為 Fabrikam，Inc.，因為它在開發的企業級 web 應用程式的虛構公司的應用程式生命週期管理 (ALM) 需求。
- [應用程式生命週期管理： 從開發到生產環境](application-lifecycle-management-from-development-to-production.md)。 本主題提供高階、 端對端部署程序的概觀。 這個範例說明 Fabrikam，Inc.如何移動的企業級 ASP.NET web 應用程式透過測試、 預備及生產環境持續開發程序的一部分。

一系列包含四個教學課程的集合。 每個著重於 web 部署的不同層面：

- [Web 部署在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供 MSBuild 專案檔的概念性簡介、 Web 發行管線、 Web Deploy 和其他相關的技術。 它說明如何使用這些工具搭配來管理複雜的部署程序。
- [設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程說明如何設定 Windows 伺服器以支援各種部署案例，包括使用 Web Deployment Agent Service （「 遠端代理程式 」） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 套件部署。 它提供指導方針選擇適當的部署方法，為您自己的環境，並說明如何使用 WFF 跨伺服器陣列中的所有網頁伺服器覆寫已部署的 web 應用程式。
- [設定 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的 CI 程序的一部分的部署，以及手動觸發的特定組建的部署。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程說明如何完成各種進階的部署工作，例如自訂多個環境的資料庫部署、 從部署中排除檔案和資料夾以及部署程序期間取得 web 應用程式離線.

## <a name="where-to-start"></a>要從何處開始

提供的參考實作，並為提供的工作和逐步解說的一般內容，這套教學課程會使用實際的層級的複雜性，以及虛構的企業部署案例中，範例方案。 下一個主題中，[企業 Web 部署： 案例概觀](enterprise-web-deployment-scenario-overview.md)，介紹的案例和範例方案。 從該處，您可以使用透過教學課程和主題最緊密符合您的需求。

> [!div class="step-by-step"]
> [下一步](enterprise-web-deployment-scenario-overview.md)
