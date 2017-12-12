---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "設定部署屬性的目標環境 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何設定環境特有的屬性，以將範例連絡人管理員方案部署至特定目標環境..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>設定的目標環境的部署屬性
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定環境特有的屬性，以將範例連絡人管理員方案部署至特定目標環境。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)方案 & #x 2014; 代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎分割專案檔案描述的方法中[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="process-overview"></a>處理程序概觀

您要用以建置和部署連絡人管理員方案的專案檔會分成兩個實體檔案：

- 另一個則包含通用建置設定和指示 ( *Publish.proj*檔案)。
- 另一個則包含特定環境的建置設定 (*Env Dev.proj*， *Env Stage.proj*等等)。

在建置時，適當的環境特定專案檔會合併到通用*Publish.proj*形成一組完整的組建指令檔。 您可以藉由建立或自訂特定環境的專案檔，以描述您自己的部署案例的設定來設定部署到特定目的地環境。

許多這些值由目的地環境的設定方式 & #x 2014年; 特別是，是否目標 web 伺服器設定為使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署處理常式。 如需有關這些方法，以及選擇您自己的環境的正確方法的指引，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。

[連絡人管理員案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)需要兩個環境特定專案檔案：

- 開發人員的測試環境的部署 (*Env Dev.proj*)。 開發人員測試環境設定為接受遠端使用遠端代理程式的部署中所述[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。 這個檔案必須提供遠端代理程式，端點位址，以及特定位置的設定，例如連接字串和服務端點。
- 部署到預備環境 (*Env Stage.proj*)。 預備環境設定為接受使用 Web 部署處理常式的遠端部署中所述[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。 提供 Web 部署的處理常式的端點位址以及特定位置的設定，例如連接字串和服務端點需要此檔案。

請務必注意您在特定環境的專案檔中設定的設定不會影響網站的內容會封裝本身 & #x 2014年; 相反地，控制如何部署封裝和提供哪些參數值時擷取的封裝。 您正在 web 封裝，述手動匯入實際執行環境[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)和[手動安裝 Web 封裝](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)，因此它並不重要了哪些設定您在特定環境的專案檔時使用產生封裝。 網際網路資訊服務 (IIS) 管理員會提示您輸入任何參數化的值，例如連接字串和服務端點，當您匯入封裝。

若要將連絡人管理員方案部署到目標環境，您可以自訂此檔案或使用做為範本並建立您自己的檔案。

**設定連絡人管理員解決方案的環境特定部署設定**

1. Visual Studio 2010 中開啟 ContactManager WCF 方案。
2. 在**方案總管] 中**視窗中，展開 [**發行**資料夾中，展開**EnvConfig**資料夾，然後再按兩下**Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. 中的屬性值取代*Env Dev.proj*以正確的值用於測試環境中的檔案。

    > [!NOTE]
    > 遵循此程序的資料表會提供有關每一個屬性的詳細資訊。
4. 儲存您的工作，然後關閉*Env Dev.proj*檔案。

## <a name="choosing-the-right-deployment-properties"></a>選擇正確的部署屬性

下表描述的範例環境特定專案檔中的每個屬性的目的*Env Dev.proj*，並提供一些指引您應該提供的值。

| 屬性名稱 | 詳細資料 |
| --- | --- |
| **MSDeployComputerName**目的地 web 伺服器或服務端點的名稱。 | 如果您要部署至目的地 web 伺服器上的遠端代理程式服務，您可以指定目標電腦名稱 (例如， **TESTWEB1**或**TESTWEB1.fabrikam.net**)，或者您可以指定遠端代理程式的端點 (例如， `http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。 部署每個案例中的運作方式相同。 如果您要部署至 Web 部署處理常式目的地 web 伺服器上，您應該指定服務端點，並包含的 IIS 網站做為查詢字串參數名稱 (例如， `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。 |
| **MSDeployAuth** Web Deploy 應該用來驗證遠端電腦的方法。 | 這應該設定為**NTLM**或**基本**。 通常，您會使用**NTLM**如果您要部署至遠端代理程式服務和**基本**如果您要部署至 Web 部署處理常式。 如果您使用基本驗證，您也需要指定使用者名稱和密碼，才能執行這種部署應該模擬的 IIS Web Deployment Tool (Web Deploy)。 在此範例中，這些值透過提供**MSDeployUsername**和**MSDeployPassword**屬性。 如果您使用 NTLM 驗證，您可以省略這些屬性，或將其保留空白。 |
| **MSDeployUsername**如果您使用基本驗證時，Web Deploy 會使用此帳戶在遠端電腦上。 | 此格式應為*網域*\*username * (例如， **FABRIKAM\matt**)。 如果您指定基本驗證，才使用這個值。 如果您使用 NTLM 驗證，則可省略的屬性。 如果提供值，將會忽略它。 |
| **MSDeployPassword**如果您使用基本驗證時，Web Deploy 將使用此密碼在遠端電腦上。 | 這是您在中指定的使用者帳戶的密碼**MSDeployUsername**屬性。 如果您指定基本驗證，才使用這個值。 如果您使用 NTLM 驗證，則可省略的屬性。 如果提供值，將會忽略它。 |
| **ContactManagerIisPath**您要部署連絡人管理員 MVC 應用程式的 IIS 路徑。 | 這應該是路徑，因為它出現在 [IIS 管理員] 中，在表單中 [*IIS 網站名稱*] / [*web**應用程式名稱*]。 請記住，IIS 網站必須存在才能部署您的應用程式。 比方說，如果您已建立名為 DemoSite 的 IIS 網站，您可以為 DemoSite/ContactManager 指定 MVC 應用程式的 IIS 路徑。 |
| **ContactManagerServiceIisPath**您要部署連絡人管理員 WCF 服務的 IIS 路徑。 | 例如，如果您已建立名為 DemoSite 的 IIS 網站，您可以指定做為 WCF 服務的 IIS 路徑**DemoSite/ContactManagerService**。 |
| **ContactManagerTargetUrl**可以到達 WCF 服務的 URL。 | 這需要表單 [*IIS 網站的根 URL*] / [*服務應用程式名稱*] / [*服務端點*]。 例如，如果您已建立 IIS 網站在連接埠 85，URL 會的形式`http://localhost:85/ContactManagerService/ContactService.svc`。 請記住，在 MVC 應用程式和 WCF 服務會部署至相同的伺服器。 如此一來，從電腦安裝僅存取此 URL。 因此，最好是在 URL 中使用 localhost 或 IP 位址，而非電腦名稱或主機標頭。 如果您使用的電腦名稱或主機標頭，[回送核取](https://go.microsoft.com/?linkid=9805131)在 IIS 中的安全性功能可能會封鎖的 URL，並傳回**HTTP 401.1-未經授權**錯誤。 |
| **CmDatabaseConnectionString**資料庫伺服器的連接字串。 | 連接字串會判斷這兩種認證 VSDBCMD 將用來連線到資料庫伺服器和建立資料庫和 web 伺服器應用程式集區會使用連線到資料庫伺服器，與資料庫互動的認證。 基本上有兩個選擇這裡。 您可以指定**整合式安全性 = true**，在此情況下使用整合式的 Windows 驗證：**資料來源 = TESTDB1; Integrated Security = true**在此情況下，將會建立此資料庫使用執行可執行檔 VSDBCMD 的使用者和應用程式的認證會存取 web 伺服器電腦帳戶的身分識別的資料庫。 或者，您可以指定使用者名稱和 SQL Server 帳戶的密碼。 在此情況下，SQL Server 認證可用來建立資料庫 VSDBCMD 和應用程式集區與資料庫互動：**資料來源 = TESTDB1;使用者 Id = ASqlUser;密碼 = Pa$ $w0rd**本主題中的逐步解說假設您將使用整合式的 Windows 驗證。 |
| **CmTargetDatabase**您想要提供您將資料庫伺服器建立該資料庫的名稱。 | 您在此處提供的值加入 VSDBCMD 命令中，做為參數。 它也用來建置 web 伺服器上的應用程式集區可用來與資料庫互動的完整連接字串。 |
  

這些範例說明如何設定這些屬性的特定部署案例。

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>範例 1 & #x 2014年; 部署至遠端代理程式服務

在這個範例中：

- 您要部署至遠端代理程式上的服務 TESTWEB1。
- 您正在指示為使用 NTLM 驗證的 Web Deploy。 Web 部署將會使用您用來叫用 Microsoft Build Engine (MSBuild) 的認證來執行。
- 您正在使用整合式的驗證來部署**ContactManager** TESTDB1 的資料庫。 資料庫會使用您用來叫用 MSBuild 的認證來進行部署。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>範例 2 & #x 2014年; 部署至 Web 部署處理常式端點

在這個範例中：

- 您要部署至 Web 部署的處理常式的服務端點上 STAGEWEB1。
- 您正在指示使用基本驗證的 Web Deploy。
- 您會指定 Web Deploy 應該模擬 FABRIKAM\stagingdeployer 帳戶，在遠端電腦上。
- 您正在使用 SQL Server 驗證來部署**ContactManager** STAGEDB1 的資料庫。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>結論

此時，您的專案檔案已完整設定建置，並將連絡人管理員方案部署到一或多個目的地環境。

若要使用這些專案檔做為單一步驟、 可重複執行部署處理程序的一部分，您需要執行*Publish.proj*檔案使用 MSBuild，並在特定環境的專案檔的位置做為參數傳遞。 您可以透過各種方式：

- 如需 MSBuild 及簡介自訂專案檔的概觀，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 如需如何編寫執行您的自訂專案檔案的 MSBuild 命令資訊，請參閱[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。
- 如需如何併入單一步驟、 可重複執行部署的命令檔中的 MSBuild 命令資訊，請參閱[建立和執行部署指令檔](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)。
- 如需如何從 Team Build 執行您的自訂專案檔資訊，請參閱[建立組建定義該支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。

>[!div class="step-by-step"]
[上一步](creating-a-server-farm-with-the-web-farm-framework.md)
