---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 企業 Web 部署： 案例概觀 |Microsoft Docs
author: jrjlee
description: 這套教學課程會使用實際的層級的複雜性，以及虛構的企業部署案例中，範例方案，來提供 ref...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ec5b62f3991fa256bc8efe7abe9b953d61d1a515
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825342"
---
<a name="enterprise-web-deployment-scenario-overview"></a>企業 Web 部署： 案例概觀
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 提供的參考實作，並為提供的工作和逐步解說的一般內容，這套教學課程會使用實際的層級的複雜性，以及虛構的企業部署案例中，範例方案。 本主題說明教學課程的案例，並導入了範例解決方案。


## <a name="scenario-description"></a>案例描述

Fabrikam，Inc.，這家虛構公司的就建立一個解決方案，可讓遠端銷售團隊，儲存和擷取連絡資訊的 web 介面。

在 Fabrikam，Inc.的應用程式生命週期管理 (ALM) 處理程序需要的解決方案部署到三個伺服器環境，在軟體開發程序的各個階段：

- 開發人員測試或 「 沙箱 」 環境。
- 內部網路式預備環境中。
- 網際網路對向的生產環境。

每個這些環境有不同的組態和安全性需求，以及每個對造成唯一部署方面的挑戰。

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam，inc.伺服器基礎結構

這是在 Fabrikam，Inc.的高階開發和部署基礎結構

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

開發人員工作站、 來源控制基礎結構、 開發人員測試環境中和所有位於 Fabrikam.net 網域內的內部網路的預備環境。 在實際執行環境位於周邊網路 （也稱為 DMZ 和遮蔽式子網路） 時，這可能會與網路隔離的內部網路防火牆。 這是常見的部署案例： 您通常會從您的內部伺服器基礎結構，透過使用防火牆或閘道伺服器隔離您的網際網路對向 web 伺服器。

在這個範例中：

- Team Foundation Server (TFS) 2010 server 與另一個組建伺服器提供原始檔控制和連續整合 (CI) 功能。
- 開發人員測試環境，包括 Internet Information Services (IIS) 7.5 網頁伺服器和 SQL Server 2008 R2 資料庫伺服器。
- 實際執行環境，包括同步處理 Web 伺服陣列架構 (WFF) 控制站伺服器，以及 SQL Server 2008 R2 資料庫伺服器的多個 IIS 7.5 網頁伺服器。 在實務上，資料庫伺服器可能會使用叢集或鏡像來改善延展性和可用性。
- 預備環境被設計來儘可能密集地複寫生產環境的設定。
- 防火牆和網路隔離原則不允許直接、 自動化部署來自內部網路周邊網路。

詳細說明在第二個教學課程中，每個環境的設定是[設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

### <a name="team-roles-for-alm"></a>Alm 的小組角色

這些使用者包括建立、 管理、 建置和發行連絡管理員解決方案：

- Matt 世昕是 Fabrikam，Inc.的 web 應用程式開發人員他是小組的使用 Visual Studio 2010 開發連絡管理員解決方案的一部分。 Matt 在開發人員測試環境，可讓他設定環境，以符合其需求的伺服器上具有完整的系統管理員權限。 他也擁有使用者存取權，他會儲存連絡管理員解決方案的原始程式碼的 Visual Studio 2010 的 TFS 執行個體。
- Rob Walters 是 Fabrikam，Inc.開發小組的伺服器系統管理員。 Rob TFS 伺服器上的系統管理存取權，以便他可以設定 TFS 與 Team Build 的各個層面。 Rob 也具有系統管理存取權的測試和預備 web 伺服器，並可做為資料庫管理員 (DBA) 測試和預備環境中的資料庫伺服器。 Rob 有 Team Build 在伺服器上設定 TFS 以執行這些工作：

    - 建置並執行單元測試應用程式，每當使用者簽入至 TFS 的檔案。 這稱為 CI。
    - 一旦應用程式通過單元測試，自動部署到測試環境的連絡人管理員應用程式。 這包括在初始部署後，發行至測試伺服器，在初始部署和資料庫的任何更新的資料庫。
    - 部署到預備環境中單一步驟的程序的連絡人管理員應用程式。
    - 建立 Web 伺服器管理員和 DBA 可以使用發行到生產環境應用程式的 Web 封裝。
- Lisa Andrews 是伺服器系統管理員負責部署到 Fabrikam，Inc.實際執行伺服器應用程式。 她必須 TFS Team Build 儲存位置的 web 部署封裝之後建立的連絡人管理員應用程式共用的讀取權限。 她也有生產網頁伺服器的系統管理存取權，讓她可以部署到生產環境應用程式。 此外，她會做為部署到生產環境中的資料庫伺服器的資料庫和資料庫更新的 DBA。

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>連絡管理員解決方案

請連絡管理員解決方案的設計可以讓已註冊、 登入的使用者，新增和編輯透過 web 介面的連絡資訊。 連絡管理員解決方案包含四個個別專案：

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**。 這是代表方案的進入點的 ASP.NET MVC3 web 應用程式專案。 它提供一些基本的 web 應用程式功能，例如讓使用者能夠建立及檢視連絡人詳細資料。 應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人和 ASP.NET 應用程式服務資料庫來管理驗證和授權。
- **ContactManager.Database**。 這是 Visual Studio 2010 資料庫專案。 專案定義商店連絡人詳細資料的資料庫結構描述。
- **ContactManager.Service**。 這是 WCF web 服務專案。 WCF 會公開端點，可讓呼叫端執行建立、 擷取、 更新和刪除 (CRUD) 作業，請連絡管理員資料庫上。 服務會依賴連絡管理員資料庫和 ContactManager.Common.dll 組件。
- **ContactManager.Common**。 這是類別庫專案。 WCF 服務依賴這個組件中定義的型別。

在此系列中，第一個教學課程中提供完整檢閱方案和其部署需求[企業中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>部署工作

部署至不同的環境，在大型組織中的應用程式是數個不同的工作。 以下是這些教學課程涵蓋的主要工作：

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

以下是本文件稍早所述的使用者的觀點來看的部署程序中每個步驟的清單：

1. 所有小組成員都檢閱連絡管理員解決方案，來判斷索引鍵的部署需求和問題的 Visual Studio 2010 中。
2. Matt 世昕可以部署到開發人員測試環境，以進行部署的邏輯，初步測試直接從開發人員工作站連絡管理員解決方案。
3. Matt 世昕在 TFS 中加入至原始檔控制應用程式。
4. Rob Walters 建立在 Team Build 中的各種連絡管理員解決方案的組建定義。 一個組建定義會將解決方案部署到開發人員測試環境中，每當使用者簽入新的程式碼使用 CI。 另一個組建定義可讓使用者觸發程序部署至預備環境所需。
5. 每當使用者簽入新的程式碼，Team Build 會自動建置解決方案元件、 執行單元測試，並將方案部署到如果建置成功時，開發人員測試環境和單元測試成功。
6. 當使用者觸發部署至預備環境時，此解決方案將封裝中，以及部署在單一步驟的程序。 此程序也會產生手動部署到生產環境的套件。
7. Lisa Andrews 會藉由手動匯入在步驟 6 中建立的 web 套件部署到生產環境應用程式。

### <a name="key-deployment-issues"></a>重要的部署問題

連絡管理員解決方案和 Fabrikam，Inc.案例反白顯示各種常見的問題和當您部署複雜的企業級規模解決方案時，可能會遇到的挑戰。 例如: 

- 您可以將專案部署到多個環境，例如開發人員或測試環境，需要預備平台，以及實際執行伺服器。 解決方案需要針對每個環境的不同的組態設定一起部署。
- 您要部署多個相依的專案同時做為單一步驟或自動化建置和部署程序的一部分。
- 您需要能夠的磁碟機部署從自動化的程序。 例如，您要部署至預備環境的 web 應用程式，當新的程式碼簽入使用 CI 程序。
- 您必須能夠控制部署程序，並將部署變數設定從 Visual Studio 外部，為開發人員不太可能會有正確的組態設定或針對每種目標環境所需的認證。
- 您要部署結構描述為基礎的資料庫專案，並保留現有的資料，在後續的部署。
- 您需要部署臨機操作為基礎的成員資格資料庫，而不需要部署使用者帳戶資料。 此外，您可能也需要更新的已部署的成員資格資料庫結構描述，而不會遺失現有的使用者帳戶資料。
- 您要排除特定檔案或資料夾，當您將內容部署至各種目標環境。

此外，管理部署，頻繁且累加式更新時便會放棄一些額外的挑戰。 例如: 

- 每次簽入新的程式碼的開發人員，您就會執行單元測試。 您只想要部署解決方案，如果程式碼通過單元測試。
- 當您部署至預備或生產環境 web 應用程式時，您想要將使用者重新導向*應用程式\_offline.htm*部署程序期間的檔案。
- 您想要記錄部署活動。 部署程序應該傳送成功或失敗的部署的電子郵件的通知給指定的收件者。
- 在自動化的部署失敗時，部署程序應重試目前的部署，或改為部署前一個網頁套件。

> [!div class="step-by-step"]
> [上一頁](deploying-web-applications-in-enterprise-scenarios.md)
> [下一頁](application-lifecycle-management-from-development-to-production.md)
