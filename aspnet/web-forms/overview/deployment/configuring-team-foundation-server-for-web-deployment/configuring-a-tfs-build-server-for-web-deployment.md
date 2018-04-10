---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: 設定 TFS 組建伺服器，用於 Web 部署 |Microsoft 文件
author: jrjlee
description: 本主題描述如何準備建置和部署您的方案使用 Team Build 及網際網路 Informat Team Foundation Server (TFS) 組建伺服器...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b3130ca7d36ffec457e1871fa62c1077b5e3174
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>設定用於 Web 部署的 TFS 組建伺服器
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何準備建置和部署您的方案使用 Team Build 和網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 的 Team Foundation Server (TFS) 組建伺服器。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

若要準備要建置和部署解決方案的組建伺服器，您將需要：

- 安裝和設定 TFS 組建服務。
- 安裝 Visual Studio 2010。
- 安裝任何產品或建置您的方案，例如版本的.NET Framework 或 ASP.NET MVC 所需的元件。
- 安裝 Web Deploy 2.0 或更新版本。

本主題將說明如何執行這些程序或指向其所在的其他資源。 工作與本主題中的逐步解說假設：

- 您開始以全新的伺服器組建執行 Windows Server 2008 R2 Service Pack 1。
- 伺服器已加入網域，使用靜態 IP 位址。
- 中所述，您已在不同的伺服器上安裝 TFS 應用程式層[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。

### <a name="who-performs-these-procedures"></a>誰會執行這些程序？

在大部分情況下，TFS 系統管理員會負責設定組建伺服器。 在某些情況下，開發人員小組可能需要特定的組建伺服器的擁有權。

## <a name="install-and-configure-the-tfs-build-service"></a>安裝和設定 TFS 組建服務

當您設定組建伺服器時，您的第一個工作是安裝和設定 TFS 組建服務。 此程序的一部分，您將需要：

- 安裝的 TFS 組建服務和設定服務帳戶。 任何建置工作，包括部署，將會使用組建服務帳戶的身分識別來執行。
- 建立*組建控制器*和一或多個*組建代理程式*。 每個組建控制器會管理一組組建代理程式。 當您的組建排入佇列時，組建控制器的組建將工作指派給可用的組建代理程式。 在 TFS 中的每一個 team 專案集合對應至單一的組建控制器。
- 設定您的組建輸出，將置放資料夾。 這是網路共用。 任何組建輸出，例如 web 部署套件，會傳送到卸除資料夾。

[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) MSDN 上的章節包含您需要才能執行這些工作的所有資源：

- Team Foundation Build 的概念性概觀，包括組建服務、 組建控制器和組建代理程式，請參閱[了解 Team Foundation 建置系統](https://msdn.microsoft.com/library/dd793166.aspx)。
- 如需安裝及設定組建服務的詳細資訊，請參閱[設定組建電腦](https://msdn.microsoft.com/library/ms181712.aspx)。
- 如需建立組建控制器的資訊，請參閱[建立和使用組建的控制器工作](https://msdn.microsoft.com/library/ee330987.aspx)。
- 如需建立組建代理程式的資訊，請參閱[建立和使用組建代理程式工作](https://msdn.microsoft.com/library/bb399135.aspx)。
- 如需建立及設定置放資料夾的資訊，請參閱[設定置放資料夾](https://msdn.microsoft.com/library/bb778394.aspx)。

## <a name="install-required-products-and-components"></a>安裝必要的產品和元件

若要啟用組建伺服器，以建立您的方案，您必須安裝任何產品、 元件或您的方案需要的組件。 安裝任何 web 平台元件之前，您應該在組建伺服器上安裝 Visual Studio 2010 （任何版本）。 這可確保的核心 Microsoft Build Engine (MSBuild) 目標檔案和 Web 發行管線 (WPP) 的目標檔案是可用於建置服務。 Visual Studio 安裝程式也應該安裝 Web Deploy，如果您打算部署 web 封裝做為建置流程的一部分，您將需要它。

若要安裝通用 web 平台元件，最好是使用[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。 這可確保您正在安裝每項產品的最新版本，它也會自動偵測並安裝任何必要條件，針對每個產品。 如果是[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)解決方案，您應該使用 Web Platform Installer，安裝這些產品和元件：

- **.NET Framework 4.0**. 如此才能執行此版本的.NET Framework 所建置的應用程式。
- **Web Deployment Tool 2.1 或更新版本**。 這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。 此程序的一部分，它將會安裝並啟動 Web 部署代理程式服務。 此服務可讓您部署 web 封裝，從遠端電腦。
- **ASP.NET MVC 3**. 這會安裝您要執行 ASP.NET MVC 3 應用程式的組件。

**若要安裝必要的產品和元件**

1. 安裝 Visual Studio 2010。 當系統提示您選取要安裝的功能，您應該包括：

    1. 您需要編譯任何程式設計語言。
    2. Visual Web Developer。 這可確保 WPP 目標，會新增至您的組建伺服器。

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Visual Studio 2010 安裝完成時，下載並安裝[Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) （如果它尚未包含在您的安裝媒體）。

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 可解決的錯誤，可以防止 MSBuild 尋找可執行檔 MSDeploy 錯誤。
3. 下載並啟動[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。
4. 在頂端**Web Platform Installer 3.0**視窗中，按一下 [**產品**。
5. 在左邊視窗中，瀏覽窗格中按一下 [**架構**。
6. 在**Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下 [**新增**。

    > [!NOTE]
    > 您可能已安裝.NET Framework 4.0，透過 Windows Update。 如果已安裝的產品或元件，Web Platform Installer 將會指示這取代**新增**按鈕的文字**已安裝**。

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. 在**ASP.NET MVC 3 (Visual Studio 2010)**資料列中，按一下 [**新增**。
8. 在導覽窗格中，按一下**伺服器**。
9. 在**Web 部署工具 2.1**資料列中，按一下 [**新增**。
10. 按一下 [安裝] 。 Web Platform Installer 將會顯示您的產品清單&#x2014;以及任何相關聯相依性&#x2014;安裝就會提示您接受授權條款。
11. 檢閱授權條款，然後如果您同意條款，按一下**我接受**。
12. 當安裝完成時，按一下 [**完成**，然後關閉**Web Platform Installer 3.0**視窗。

> [!NOTE]
> 如果您的部署程序包含使用 VSDBCMD.exe 或 SQLCMD.exe 等工具，您必須確定這些已安裝在您的組建伺服器上。 VSDBCMD.exe 是 Visual Studio 工具，而且通常會新增至伺服器安裝 Team Foundation Build 時。 SQLCMD.exe 是 SQL Server 的工具。 您可以下載從 SQLCMD.exe 單機版[Microsoft SQL Server 2008 R2 功能套件](https://go.microsoft.com/?linkid=9805134)頁面。


## <a name="conclusion"></a>結論

此時，您的組建伺服器已準備好開始建置和部署您的 web 應用程式專案。 下一個主題[建立組建定義，支援部署](creating-a-build-definition-that-supports-deployment.md)，說明如何建立及設定組建定義，以控制何時以及如何建立與部署您的專案。

## <a name="further-reading"></a>進一步閱讀

一般工作與 Team Build 的詳細指引，請參閱[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx)。

> [!div class="step-by-step"]
> [上一頁](adding-content-to-source-control.md)
> [下一頁](creating-a-build-definition-that-supports-deployment.md)
