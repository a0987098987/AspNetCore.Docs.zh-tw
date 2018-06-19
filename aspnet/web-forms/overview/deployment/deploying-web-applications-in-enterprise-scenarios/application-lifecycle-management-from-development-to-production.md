---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 應用程式生命週期管理： 從開發到生產環境 |Microsoft 文件
author: jrjlee
description: 本主題將說明如何虛構的公司管理的 ASP.NET web 應用程式透過測試、 預備及生產環境長條上方，以部署...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 8beeffb374df09c6695a1845199d30006ddcc1b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887276"
---
<a name="application-lifecycle-management-from-development-to-production"></a>應用程式生命週期管理： 從開發到生產環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題將說明如何虛構的公司管理 ASP.NET web 應用程式的測試、 預備及生產環境透過連續的開發程序的一部分部署。 在主題中，會提供進一步資訊及如何執行特定工作的逐步解說的連結。
> 
> 本主題旨在提供高層級概觀[系列的教學課程](deploying-web-applications-in-enterprise-scenarios.md)在企業中的 web 部署。 如果您不熟悉的一些概念，此處所述，別擔心&#x2014;遵循教學課程提供的詳細的資訊，所有這些工作和技巧。
> 
> > [!NOTE]
> > 簡單明瞭的註冊起見，本主題不討論更新資料庫做為部署程序的一部分。 不過，對資料庫功能進行累加式更新是許多的企業部署案例的需求，而且您可以找到如何完成這項作業，稍後在本教學課程系列的相關指引。 如需詳細資訊，請參閱[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。


## <a name="overview"></a>總覽

如下圖所示的部署程序為基礎的 Fabrikam，Inc.部署案例中所述[企業 Web 部署： 案例概觀](enterprise-web-deployment-scenario-overview.md)。 您先研究本主題之前，您應該閱讀案例概觀。 基本上，此案例會檢查組織管理相當複雜的 web 應用程式的部署方式[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)，透過在一般企業環境中的各種階段。

概括而言，連絡人管理員解決方案會透過這些階段的開發和部署程序的一部分：

1. 開發人員會檢查 Team Foundation Server (TFS) 2010年成一些程式碼。
2. TFS 建置程式碼，並執行與 team 專案相關聯的任何單元測試。
3. TFS 會將方案部署到測試環境。
4. 開發團隊會驗證，並驗證測試環境中的方案。
5. 預備環境的系統管理員執行 「 假設 」 部署到預備環境，以建立部署是否會造成任何問題。
6. 預備環境的系統管理員會執行即時部署到預備環境。
7. 方案會經歷使用者接受度測試預備環境中。
8. Web 部署套件是以手動方式匯入到生產環境。

這些階段會形成連續開發週期的一部分。

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

在實務上，處理程序為稍微複雜一點比此，您會看到當我們查看更多詳細資料的每個階段。 Fabrikam，Inc.會針對每個目標環境中使用不同的方法來部署。

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

本主題的其餘部分會檢查此部署生命週期的這些重要階段：

- **必要條件**： 您需要設定伺服器基礎結構之前您備妥您的部署邏輯, 的方式。
- **初始開發及部署**： 您需要在您第一次部署方案之前執行的動作。
- **若要測試部署**： 如何封裝及內容部署到測試環境中自動當開發人員簽入新的程式碼時。
- **部署到預備環境**： 如何部署特定是建置到預備環境，以及如何執行 「 假設 」 部署，以確保部署不會造成任何問題。
- **部署至生產環境**： 如何將實際執行環境中匯入 web 封裝，當網路基礎結構會防止遠端部署。

## <a name="prerequisites"></a>必要條件

任何部署案例中的第一個工作是確保您的伺服器基礎結構符合您的部署工具和技術需求。 在此情況下，Fabrikam，Inc.設定它的伺服器基礎結構，像這樣：

- TFS 會設定為包含 team 專案集合、 組建控制器和組建代理程式。 請參閱[用於自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)如需詳細資訊。
- 測試環境設定為接受使用 Web Deployment Agent Service （「 遠端代理程式 」），遠端部署中所述[案例： 在測試環境設定用於 Web 部署](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md)和[設定 Web 伺服器的 Web Deploy 發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。
- 預備環境設定為接受使用 Web 部署的處理常式的端點，遠端部署中所述[案例： 設定用於 Web 部署的預備環境](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md)和[網頁伺服器設定web Deploy 發行變更 (Web Deploy 處理常式)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。
- 實際執行環境設定為允許系統管理員可以手動匯入網際網路資訊服務 (IIS) 中，web 部署套件中所述[案例： 設定用於 Web 部署實際執行環境](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md)和[設定 Web 伺服器的 Web Deploy 發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。

## <a name="initial-development-and-deployment"></a>初始的開發和部署

Fabrikam，Inc.開發小組可以在第一次部署連絡人管理員方案之前，它必須執行這些工作：

- 在 TFS 中建立新的 team 專案。
- 建立包含部署邏輯的 Microsoft Build Engine (MSBuild) 專案檔。
- 建立觸發程序部署程序的 TFS 組建定義。

### <a name="create-a-new-team-project"></a>建立新的 Team 專案

- TFS 系統管理員，Rob 郭會建立新的 team 專案，則應用程式中所述[在 TFS 中建立 Team 專案](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md)。 接下來，領導開發人員 Matt 世昕，建立基本架構的解決方案。 他會檢查其檔案到在 TFS 中，新的 team 專案中所述[新增內容至原始檔控制](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md)。

### <a name="create-the-deployment-logic"></a>建立部署邏輯

Matt 世昕建立各種自訂 MSBuild 專案檔，使用分割專案檔案描述的方法中[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 Matt 建立：

- 專案檔名為*Publish.proj*執行部署程序。 此檔案包含 MSBuild 目標的建置中方案的專案、 建立 web 封裝和封裝部署到目的地伺服器的環境。
- 環境特定專案檔名為*Env Dev.proj*和*Env Stage.proj*。 這包含會分別針對測試環境和預備環境，例如連接字串、 服務端點，以及將會收到 web 封裝的遠端服務的詳細資料的設定。 如需有關選擇特定的目的地環境中正確設定的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。

若要執行部署，使用者執行*Publish.proj*檔案使用 MSBuild，或小組所建置，並指定相關的特定環境的專案檔的位置 (*Env Dev.proj*或*Env Stage.proj*) 做為命令列引數。 *Publish.proj*檔然後匯入特定環境的專案檔來建立一組完整的發行每一個目標環境的指示。

> [!NOTE]
> 這些自訂專案檔案的運作的方式與您用於叫用 MSBuild 的機制無關。 例如，您可以使用 MSBuild 命令列直接中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 中所述，您可以從 命令檔，執行專案檔[建立和執行部署指令檔](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)。 或者，您可以執行專案檔從 TFS 中的組建定義中所述[建立組建定義該支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。  
> 每個案例中的最終結果是相同&#x2014;MSBuild 執行合併的專案檔，並將方案部署至目標環境。 這可以讓您有極大的彈性觸發您發佈程序的方式。


他已建立自訂專案檔案，一旦 Matt 將它們加入至方案資料夾，並簽入至原始檔控制。

### <a name="create-build-definitions"></a>建立組建定義

做為最終的準備工作，Matt 和 Rob 共同搭配，建立三個新的 team 專案的組建定義：

- **DeployToTest**。 這會建置連絡人管理員解決方案，並將其部署到測試環境中，每次簽入，就會發生。
- **DeployToStaging**。 當開發人員的組建排入佇列時，這會部署到預備環境的指定前一個組建資源。
- **DeployToStaging-WhatIf**。 當開發人員的組建排入佇列，這會執行 「 假設 」 部署到預備環境。

以下各節提供更多詳細資料上每個組建定義。

## <a name="deployment-to-test"></a>若要測試部署

在 Fabrikam，Inc.開發小組會維護測試環境，以進行各種不同的測試活動，例如驗證和驗證、 使用性測試、 相容性測試，及臨機操作或探勘測試的軟體。

開發團隊已在名為的 TFS 中建立的組建定義**DeployToTest**。 這個組建定義會使用持續整合觸發程序，這表示每當 Fabrikam，Inc.開發小組的成員執行簽入建置流程的執行。 觸發建置時，會將組建定義：

- 建置 ContactManager.sln 方案。 接著，這會建置方案中的每個專案。
- （如果已成功建置方案），請在方案資料夾結構中執行的任何單元測試。
- 執行自訂的專案檔，以控制部署程序 （如果方案建置成功，並傳遞任何單元測試）。

最終結果是，如果方案建置成功，並傳遞單元測試，web 封裝和部署任何其他資源會部署至測試環境。

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>部署程序的運作方式為何？

**DeployToTest**組建定義提供 msbuild 的這些引數：


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true**和**DeployTarget = 封裝**Team Build 建立的方案中的專案時，會使用屬性。 當專案的 web 應用程式專案，這些屬性會指示 MSBuild 來建立專案的 web 部署封裝。 **TargetEnvPropsFile**屬性會告知*Publish.proj*檔案尋找要匯入的特定環境的專案檔的位置。

> [!NOTE]
> 如需如何建立組建定義，就像這樣的詳細逐步解說，請參閱[建立組建定義該支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。


*Publish.proj*檔案包含建置方案中的每個專案的目標。 不過，它也包含條件式邏輯，便可略過這些組建目標如果您在 Team Build 中執行檔案。 這可讓您善用 Team Build 提供，例如執行單元測試的能力的其他建置功能。 如果方案建置或單元測試失敗， *Publish.proj*將不會執行檔案和應用程式將不會部署。

條件式邏輯便可達成評估**BuildingInTeamBuild**屬性。 這是會自動設為 MSBuild 屬性**true**當您使用 Team Build 來建置您的專案。

## <a name="deployment-to-staging"></a>部署到預備環境

當建置符合所有需求的開發人員小組在測試環境中時，小組可能要將相同的組建部署到預備環境。 預備環境通常會設定為符合實際執行或 「 即時 」 環境緊密越好，例如，根據伺服器規格、 作業系統和軟體和網路設定的特性。 預備環境通常用於負載測試、 使用者接受度測試和更廣泛的內部評論。 直接從組建伺服器的組建部署到預備環境。

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

用來將方案部署到預備環境中的組建定義**DeployToStaging-WhatIf**和**DeployToStaging**，都具有這些特性：

- 它們實際上不建立任何項目。 當 Rob 將解決方案部署到預備環境時，他想要部署特定的現有組建的已驗證和測試環境中進行驗證。 若要執行的自訂專案檔，來控制部署程序只需要的組建定義。
- 當 Rob 觸發組建時，他會使用組建參數來指定哪些組建包含他想要從組建伺服器部署的資源。
- 不會自動觸發的組建定義。 Rob 手動組建排入佇列時，他想要將方案部署到預備環境。

這是部署到預備環境的高階程序：

1. 預備環境管理員 Rob 郭排入佇列的組建使用**DeployToStaging-WhatIf**組建定義。 Rob 會使用組建定義參數來指定哪些他想要部署的組建。
2. **DeployToStaging-WhatIf**組建定義執行 「 假設 」 模式中的自訂專案檔。 這會產生記錄檔，好像 Rob 執行即時部署中，但它實際上不會進行任何變更，到目的地環境。
3. Rob 檢閱記錄檔，以確定在預備環境部署的效果。 特別是，Rob 想要檢查會加入、 更新項目，以及將刪除。
4. Rob 是否滿足，部署將不會進行任何不想要變更現有的資源或資料，他排入佇列的組建使用**DeployToStaging**組建定義。
5. **DeployToStaging**組建定義執行自訂的專案檔。 這些部署資源發行至預備環境中在主要 web 伺服器時。
6. Web 伺服陣列架構 (WFF) 控制器會同步處理在預備環境中的 web 伺服器。 這樣可讓應用程式提供伺服器陣列中的所有 web 伺服器上。

### <a name="how-does-the-deployment-process-work"></a>部署程序的運作方式為何？

**DeployToStaging**組建定義提供 msbuild 的這些引數：


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile**屬性會告知*Publish.proj*檔案尋找要匯入的特定環境的專案檔的位置。 **OutputRoot**屬性會覆寫內建值，表示建置資料夾包含您想要部署之資源的位置。 當 Rob 組建排入佇列時，他會使用**參數**索引標籤，以提供更新的值，如**OutputRoot**屬性。

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> 如需有關如何建立組建定義，就像這樣的詳細資訊，請參閱[部署特定建置](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md)。


**DeployToStaging-WhatIf**組建定義包含相同的部署邏輯**DeployToStaging**組建定義。 不過，它包含的其他引數**WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


內*Publish.proj*檔案， **WhatIf**屬性會指出部署的所有資源應經過都發佈在 「 如果 」 模式。 換句話說，如同部署方面，但不實際變更目的地環境中，會產生記錄檔。 這可讓您評估建議的部署影響&#x2014;特定、 項目會加入，功能將會更新，和會刪除項目中&#x2014;實際進行任何變更之前。

> [!NOTE]
> 如需有關如何設定 「 如果 」 部署的詳細資訊，請參閱[執行 「 假設 」 部署](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md)。


一旦您已部署到預備環境中的主要 web 伺服器應用程式，WFF 會自動同步處理應用程式在伺服器陣列中的所有伺服器。

> [!NOTE]
> 如需有關設定以同步處理 web 伺服器 WFF 的詳細資訊，請參閱[建立伺服器陣列與 Web 伺服陣列架構](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md)。


## <a name="deployment-to-production"></a>部署至生產環境

組建已核准在預備環境中，Fabrikam，Inc.小組可以發行至生產環境應用程式。 在實際執行環境是應用程式進入 「 即時 」 和到達它的目標對象的一般使用者的位置。

在實際執行環境是在面對網際網路的周邊網路中。 這是與包含組建伺服器的內部網路隔離。 實際執行環境系統管理員，Lisa Andrews 必須手動複製 web 部署套件從組建伺服器，並匯入 IIS 主要實際執行網頁伺服器上。

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

這是部署到實際執行環境的高階程序：

1. 開發人員小組建議 Lisa 組建可供部署至生產環境。 小組在組建伺服器上建議 Lisa 之位置的置放資料夾中的 web 部署套件。
2. Lisa 收集網頁套件從組建伺服器，並將其複製到實際執行環境中主要 web 伺服器。
3. Lisa 會匯入及發行 web 封裝，在主要 web 伺服器上的使用 IIS 管理員。
4. WFF 控制器會同步處理實際執行環境中的 web 伺服器。 這樣可讓應用程式提供伺服器陣列中的所有 web 伺服器上。

### <a name="how-does-the-deployment-process-work"></a>部署程序的運作方式為何？

IIS 管理員包含匯入應用程式套件精靈，以簡化將 web 套件發行至 IIS 網站。 如需如何執行此程序的逐步解說，請參閱[手動安裝 Web 封裝](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)。

## <a name="conclusion"></a>結論

本主題提供典型企業規模的 web 應用程式的部署生命週期的示範。

本主題一系列的 web 應用程式部署的各種層面提供指引的教學課程的一部分。 在實務上，每個階段的部署程序中，有許多其他工作和考量，不可能在單一的逐步解說涵蓋這些項目。 如需詳細資訊，請參閱這些教學課程：

- [Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教學課程提供使用 MSBuild 和 IIS Web Deployment Tool (Web Deploy) 的 web 部署技術的完整介紹。
- [用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教學課程提供如何設定 Windows server 環境，以支援各種部署案例的指引。
- [設定適用於 Team Foundation Server 自動 Web 部署](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教學課程提供如何將部署邏輯整合到 TFS 的建置程序指引。
- [進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教學課程提供指引如何符合某些更複雜的部署挑戰該組織字體。

> [!div class="step-by-step"]
> [上一步](enterprise-web-deployment-scenario-overview.md)
