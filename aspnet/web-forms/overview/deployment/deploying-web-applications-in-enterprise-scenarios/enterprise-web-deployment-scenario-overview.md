---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 企業 Web 部署： 案例概觀 |Microsoft 文件
author: jrjlee
description: 這組教學課程使用實際的層級的複雜性，以及虛構的企業部署案例中，範例方案，提供 ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890017"
---
<a name="enterprise-web-deployment-scenario-overview"></a>企業 Web 部署： 案例概觀
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 這組教學課程會使用範例方案實際的層級的複雜性，以及虛構的企業部署案例中，以提供參考實作，以及提供的工作和逐步解說的一般內容。 本主題描述教學課程案例，並介紹範例方案。


## <a name="scenario-description"></a>Scenario Description

Fabrikam，Inc.的虛構的公司，正在建立解決方案，可讓遠端銷售團隊，儲存和擷取連絡資訊從 web 介面。

在 Fabrikam，Inc.的應用程式生命週期管理 (ALM) 處理程序需要方案部署至三個伺服器環境的軟體開發流程的各個階段：

- 開發人員測試或 「 沙箱 」 環境。
- 內部網路為基礎預備環境中。
- 網際網路對向實際執行環境。

每個環境有不同的組態與安全性需求，而且每個會唯一部署面臨挑戰。

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam，inc.伺服器基礎結構

這是高層級的開發和部署基礎結構，在 Fabrikam，inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

開發人員工作站、 原始檔控制基礎結構、 開發人員測試環境中和完全位於內部網路內的網路 Fabrikam.net 網域的預備環境。 在實際執行環境位於周邊網路 （也稱為 DMZ 和遮蔽式子網路） 時，這可能會與網路隔離的內部網路防火牆。 這是常見的部署案例： 您通常從您的內部伺服器基礎結構，透過防火牆或閘道伺服器使用隔離您的網際網路對向 web 伺服器。

在這個範例中：

- Team Foundation Server (TFS) 2010 server 與另一個組建伺服器提供原始檔控制和持續整合 (CI) 功能。
- 開發人員測試環境，包括網際網路資訊服務 (IIS) 7.5 網頁伺服器和 SQL Server 2008 R2 資料庫伺服器。
- 實際執行環境，包括 Web Farm Framework (WFF) 控制器伺服器，以及 SQL Server 2008 R2 資料庫伺服器同步處理多個 IIS 7.5 網頁伺服器。 在實務上，資料庫伺服器可能會使用叢集或鏡像來改善延展性和可用性。
- 預備環境被為了儘可能密集地複寫實際執行環境的設定。
- 防火牆及網路隔離原則不允許直接自動部署從內部網路周邊網路。

詳細說明第二個教學課程中，每個環境的設定是[用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

### <a name="team-roles-for-alm"></a>Alm 小組角色

建立、 管理、 建置和發佈連絡人管理員方案會與這些使用者：

- Matt 世昕是 Fabrikam，Inc.的 web 應用程式開發人員他是小組使用 Visual Studio 2010 開發連絡人管理員方案的一部分。 Matt 在開發人員測試環境，讓他設定以符合其需求的環境中的伺服器擁有完整的系統管理員權限。 他也具有他將儲存連絡人管理員解決方案的原始程式碼的 Visual Studio 2010 的 TFS 執行個體的使用者存取權。
- Rob 郭為 Fabrikam，Inc.開發小組的伺服器管理員。 Rob TFS 伺服器上具有系統管理存取權，以便他可以設定 TFS 與 Team Build 的各個層面。 Rob 也有測試伺服器與臨時 web 伺服器的系統管理權限，並當做測試和預備環境中的資料庫伺服器的資料庫管理員 (DBA)。 Rob 具有 Team Build 在伺服器上設定 TFS 執行這些工作：

    - 建置並執行應用程式的單元測試，每當使用者簽入至 TFS 的檔案。 這稱為 CI。
    - 一旦應用程式傳遞單元測試，自動部署到測試環境的連絡人管理員應用程式。 這包括初始部署後將資料庫發行到測試伺服器上初始部署並對資料庫的任何更新。
    - 連絡人管理員應用程式部署到預備環境中單一步驟的程序。
    - 建立 Web 伺服器管理員和 DBA 可以使用應用程式發行至生產環境 Web 封裝。
- Lisa Andrews 是伺服器系統管理員負責部署至 Fabrikam，Inc.實際執行伺服器應用程式。 她必須 TFS Team Build 儲存的 web 部署套件一旦它建置連絡人管理員應用程式共用的讀取權限。 她也有生產網頁伺服器的系統管理存取權，因此她可以部署到生產環境應用程式。 此外，她會做為 DBA 人員將資料庫和資料庫更新部署到生產環境中的資料庫伺服器。

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>請連絡管理員解決方案

請連絡管理員解決方案被為了讓已註冊、 登入的使用者，新增和編輯連絡人資訊透過 web 介面。 連絡管理員方案包含四個個別專案：

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. 這是 ASP.NET MVC3 web 應用程式專案，表示方案的進入點。 它提供一些基本 web 應用程式功能，例如使用者提供的功能，以建立及檢視連絡人詳細資料。 應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人及管理驗證和授權的 ASP.NET 應用程式服務資料庫。
- **ContactManager.Database**. 這是 Visual Studio 2010 資料庫專案。 專案定義的商店連絡人詳細資料的資料庫結構描述。
- **ContactManager.Service**. 這是 WCF web 服務專案。 建立允許執行的呼叫端的端點，WCF 會公開擷取、 更新和刪除 (CRUD) 作業，請連絡管理員資料庫上。 服務會仰賴連絡人管理員資料庫和 ContactManager.Common.dll 組件。
- **ContactManager.Common**. 這是類別庫專案。 WCF 服務依賴這個組件中定義的型別。

本系列，第一個教學課程中所提供的方案和其部署需求的完整檢閱[企業中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>部署工作

部署至不同的環境，在大型組織中的應用程式是數個不同的工作。 以下是這些教學課程涵蓋的主要工作：

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

以下是部署程序中從本文件稍早所述的使用者的觀點來看每個步驟的清單：

1. 所有小組成員都檢閱連絡人管理員中的方案來判斷索引鍵的部署需求和問題的 Visual Studio 2010。
2. Matt 世昕可能部署連絡人管理員方案開發人員工作站直接從開發人員的測試環境，以進行初始的部署邏輯測試。
3. Matt 世昕 TFS 中加入至原始檔控制應用程式。
4. Rob 郭建立在 Team Build 中的各種連絡管理員解決方案的組建定義。 一個組建定義會將方案部署到開發人員測試環境，每當使用者簽入新的程式碼使用 CI。 另一個組建定義可讓使用者觸發程序部署到預備環境所需。
5. 每次使用者簽入新的程式碼中，Team Build 自動建置解決方案元件、 執行單元測試，並將方案部署到如果建置成功時，開發人員測試環境和單元測試成功。
6. 當使用者觸發的部署到預備環境時，方案會封裝並部署在單一步驟的程序。 此程序也會產生手動部署到生產環境的套件。
7. Lisa Andrews 來手動匯入在步驟 6 中建立的 web 套件部署到生產環境應用程式。

### <a name="key-deployment-issues"></a>關鍵的部署問題

請連絡管理員解決方案，以及 Fabrikam，Inc.案例反白顯示各種常見的問題及部署複雜，企業規模的解決方案時可能遇到的挑戰。 例如: 

- 您必須要能將專案部署到多個環境，例如開發人員或測試環境中，暫存平台和實際執行伺服器。 方案必須先部署每個環境的不同組態設定。
- 您必須同時在單一步驟或自動化建置和部署程序的一部分部署多個相依專案。
- 您需要能夠的磁碟機部署從自動化的程序。 例如，您要部署到預備環境的 web 應用程式，當新的程式碼簽入使用 CI 程序。
- 您需要能夠控制部署程序以及設定部署變數，從 Visual Studio 外部，為開發人員不太可能會有正確的組態設定或針對每個目標環境所需的認證。
- 您需要部署結構描述為基礎的資料庫專案，並保留現有的資料，後續部署。
- 您需要部署臨機操作為基礎的成員資格資料庫，而不需部署使用者帳戶資料。 此外，您可能也需要更新的已部署的成員資格資料庫結構描述，而不會遺失現有的使用者帳戶資料。
- 您要排除特定檔案或資料夾，當您將內容部署至各種目標環境。

此外，頻繁和累加式更新時，管理部署會帶出一些其他的挑戰。 例如: 

- 每次簽入新的程式碼的開發人員，您會執行單元測試。 您只想要部署方案，如果在程式碼通過單元測試。
- 當您部署至預備或生產環境 web 應用程式時，您想要將使用者重新導向*應用程式\_offline.htm*部署程序的持續時間的檔案。
- 您想要記錄部署活動。 部署程序應該傳送給指定的收件者的電子郵件通知的成功或失敗的部署。
- 如果自動化的部署失敗，部署程序應該重試目前的部署，或改為部署先前 web 套件。

> [!div class="step-by-step"]
> [上一頁](deploying-web-applications-in-enterprise-scenarios.md)
> [下一頁](application-lifecycle-management-from-development-to-production.md)
