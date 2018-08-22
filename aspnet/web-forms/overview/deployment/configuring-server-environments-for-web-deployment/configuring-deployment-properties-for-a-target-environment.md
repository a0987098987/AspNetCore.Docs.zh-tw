---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: 設定部署屬性的目標環境 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定環境特有的屬性，以將範例連絡管理員解決方案部署至特定的目標環境...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: 1486cc7d8f25e823481dfab109d18c407c2d8063
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827255"
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>設定部署屬性的目標環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定環境特有的屬性，以將範例連絡管理員解決方案部署至特定的目標環境。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本系列教學課程使用範例解決方案&#x2014; [Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="process-overview"></a>處理程序概觀

您將使用來建置和部署連絡管理員解決方案的專案檔會分成兩個實體檔案：

- 包含標準的其中一項建置設定和指示 ( *Publish.proj*檔案)。
- 包含環境特定組建設定 (*Env Dev.proj*， *Env Stage.proj*等等)。

在建置時，適當的環境特定專案檔會合併到 universal *Publish.proj*形成一組完整的組建指示的檔案。 您可以藉由建立或自訂設定，以描述您自己的部署案例的特定環境的專案檔來設定部署至特定的目的地環境。

許多這些值取決於您的目的地環境的設定方式&#x2014;特別的是，您目標 web 伺服器是否設定為使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署處理常式。 如需有關這些方法，以及選擇您自己的環境的策略指引，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。

[Contact Manager 案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)需要兩個環境特有的專案檔：

- 部署到開發人員測試環境 (*Env Dev.proj*)。 開發人員測試環境設定為接受遠端使用遠端代理程式的部署中所述[案例： 設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)。 此檔案必須提供遠端代理程式，端點位址以及特定位置的設定，例如連接字串和服務端點。
- 部署至預備環境 (*Env Stage.proj*)。 預備環境已設定為接受遠端的部署使用 Web 部署處理常式，如中所述[案例： 設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 此檔案必須提供 Web 部署的處理常式的端點位址以及特定位置的設定，例如連接字串和服務端點。

請務必注意您在特定環境的專案檔中設定的設定不會影響 web 封裝本身的內容&#x2014;相反地，控制如何部署封裝和封裝時提供哪些參數值擷取。 您正在匯入生產環境 web 套件以手動方式，如中所述[案例： 設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)並[手動安裝網頁套件](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)，因此並不重要了哪些設定您環境特有的專案檔中時使用產生的封裝。 Internet Information Services (IIS) 管理員會提示您輸入任何參數化的值，例如連接字串和服務端點，當您匯入封裝。

若要將連絡管理員解決方案部署至目標環境中，您可以自訂此檔案或使用它做為範本並建立您自己的檔案。

**若要設定環境特定部署設定連絡管理員解決方案**

1. Visual Studio 2010 中開啟 ContactManager WCF 方案。
2. 在**方案總管** 視窗中，展開**發佈**資料夾中，展開**EnvConfig**資料夾，然後再按兩下**Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. 中的屬性值取代*Env Dev.proj*以正確的值為您自己的測試環境中的檔案。

    > [!NOTE]
    > 遵循此程序的資料表會提供有關每個屬性的詳細資訊。
4. 儲存您的工作，然後關閉*Env Dev.proj*檔案。

## <a name="choosing-the-right-deployment-properties"></a>選擇正確的部署屬性

下表描述在範例環境特定的專案檔案中，每個屬性的用途*Env Dev.proj*，並提供一些指引，您應該提供的值。


|                                                        屬性名稱                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        詳細資料                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong>目的地 web 伺服器或服務端點的名稱。               |                                                                                                                                                                                                                                              如果您要部署至目的地 web 伺服器上的遠端代理程式服務，您可以指定目標電腦名稱 (例如<strong>TESTWEB1</strong>或是<strong>TESTWEB1.fabrikam.net</strong>)，或者您可以指定遠端代理程式的端點 (例如`http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。 部署每個案例中運作方式相同。 如果您要部署至 Web 部署處理常式在目的地 web 伺服器上，您應該指定服務端點，並包含 IIS 網站，做為查詢字串參數的名稱 (例如`https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> Web Deploy 應該用來驗證遠端電腦的方法。          |                                                                                                                                                                                                                          這應該設定為<strong>NTLM</strong>或是<strong>基本</strong>。 通常，您會使用<strong>NTLM</strong>如果您要部署至遠端代理程式服務並<strong>基本</strong>如果您要部署至 Web 部署處理常式。 如果您使用基本驗證時，您也需要指定使用者名稱和密碼，才能執行部署應該模擬的 IIS Web Deployment Tool (Web Deploy)。 在此範例中，這些值會提供透過<strong>MSDeployUsername</strong>並<strong>MSDeployPassword</strong>屬性。 如果您使用 NTLM 驗證時，您可以省略這些屬性，或保留空白。                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong>如果您使用基本驗證，Web Deploy 會使用此帳戶在遠端電腦上。  |                                                                                                                                                                                                                                                                                                                                                                                                                       這應該採用格式<em>網域</em>\*使用者名稱 * (例如<strong>FABRIKAM\matt</strong>)。 如果指定基本驗證，只會使用此值。 如果您使用 NTLM 驗證時，就可以省略此屬性。 如果提供的值，將會忽略它。                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong>如果您使用基本驗證，Web Deploy 將使用此密碼在遠端電腦上。 |                                                                                                                                                                                                                                                                                                                                                                                                                    這是您在指定的使用者帳戶的密碼<strong>MSDeployUsername</strong>屬性。 如果指定基本驗證，只會使用此值。 如果您使用 NTLM 驗證時，就可以省略此屬性。 如果提供的值，將會忽略它。                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong>您要部署的連絡人管理員 MVC 應用程式的 IIS 路徑。     |                                                                                                                                                                                                                                                                                                                                                                        這應該是路徑，因為它出現在 [IIS 管理員] 中，在表單中 [<em>IIS 網站名稱</em>] / [<em>web</em><em>應用程式名稱</em>]。 請記住，必須存在才能部署您的應用程式的 IIS 網站。 例如，如果您已建立名為 DemoSite IIS 網站，您可以指定 MVC 應用程式的 IIS 路徑為 DemoSite/ContactManager。                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong>您要部署的連絡人管理員 WCF 服務的 IIS 路徑。    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          例如，如果您已建立名為 DemoSite IIS 網站，您可以指定做為 WCF 服務的 IIS 路徑<strong>DemoSite/ContactManagerService</strong>。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong> WCF 服務可以觸達的 URL。                   |                                                                                                                                                     這需要表單 [<em>IIS 網站根目錄 URL</em>] / [<em>服務應用程式名稱</em>] / [<em>服務端點</em>]。 例如，如果您已在連接埠 85 上建立 IIS 網站，URL 會採用格式`http://localhost:85/ContactManagerService/ContactService.svc`。 請記住，MVC 應用程式和 WCF 服務會部署至相同的伺服器。 如此一來，從其安裝所在的電腦只會存取此 URL。 因此，最好是使用 localhost 或 IP 位址，而非電腦名稱或主機標頭，在 URL 中。 如果您使用的電腦名稱或主機標頭[回送檢查](https://go.microsoft.com/?linkid=9805131)在 IIS 中的安全性功能可能會封鎖的 URL，並傳回<strong>HTTP 401.1-未經授權</strong>時發生錯誤。                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong>資料庫伺服器的連接字串。                  | 連接字串會判斷這兩種認證 VSDBCMD 將用來連線到資料庫伺服器，並建立資料庫和 web 伺服器應用程式集區將用來連線到資料庫伺服器和資料庫互動的認證。 基本上有兩個選擇這裡。 您可以指定<strong>整合式安全性 = true</strong>，在此情況下使用整合式的 Windows 驗證：<strong>資料來源 = TESTDB1; Integrated Security = true</strong>在此情況下，將會建立此資料庫使用執行可執行檔，VSDBCMD 的使用者和應用程式的認證來存取資料庫使用的 web 伺服器電腦帳戶的身分識別。 或者，您可以指定使用者名稱和 SQL Server 帳戶的密碼。 由 VSDBCMD 來建立資料庫和應用程式集區與資料庫互動，在此情況下，使用 SQL Server 認證：<strong>資料來源 = TESTDB1;使用者識別碼 = ASqlUser;密碼 = Pa$$w0rd</strong>本主題的逐步解說假設您將使用整合式的 Windows 驗證。 |
|        <strong>CmTargetDatabase</strong>您想要讓您將建立資料庫伺服器的資料庫名稱。        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     您在此處提供的值加入 VSDBCMD 命令中，做為參數。 它也用來建置 web 伺服器上的應用程式集區可用來與資料庫互動的完整連接字串。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

下列範例會示範如何設定這些屬性的特定部署案例。

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>範例 1&#x2014;部署至遠端代理程式服務

在這個範例中：

- 您要部署到 TESTWEB1 的遠端代理程式服務。
- 您在指示 Web Deploy 使用 NTLM 驗證。 Web 部署會使用您用來叫用 Microsoft Build Engine (MSBuild) 的認證來執行。
- 您使用整合式的驗證來部署**ContactManager** TESTDB1 的資料庫。 資料庫會使用您用來叫用 MSBuild 的認證來進行部署。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>範例 2&#x2014;部署至 Web 部署的處理常式端點

在這個範例中：

- 您要部署至 Web 部署的處理常式的服務端點上 STAGEWEB1。
- 您在指示 Web Deploy 能夠使用基本驗證。
- 您指定 Web Deploy 應該模擬 FABRIKAM\stagingdeployer 帳戶，在遠端電腦上。
- 您用來部署 SQL Server 驗證**ContactManager** STAGEDB1 的資料庫。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>結論

此時，您的專案檔案已完整設定來建置並部署至一或多個目的地環境的 連絡管理員解決方案。

若要使用這些專案檔案的單一步驟、 高再現性的部署程序的一部分，您需要執行*Publish.proj*檔案中，使用 MSBuild，並在特定環境的專案檔的位置，做為參數傳遞。 您可以各種方式來這麼做：

- 如需 MSBuild 及自訂的專案檔案服務簡介的概觀，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 如需如何制訂 MSBuild 命令來執行自訂的專案檔的資訊，請參閱[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。
- 如需如何併入單一步驟、 高再現性的部署命令檔中的 MSBuild 命令的資訊，請參閱[建立和執行部署命令檔](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)。
- 如需如何從 Team Build 中執行您的自訂專案檔的資訊，請參閱[建立組建定義該支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。

> [!div class="step-by-step"]
> [上一步](creating-a-server-farm-with-the-web-farm-framework.md)
