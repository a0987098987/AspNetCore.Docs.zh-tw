---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: 部署網頁套件 |Microsoft Docs
author: jrjlee
description: 本主題描述如何使用 Internet Information Services (IIS) Web Deployment Tool (Web...到遠端伺服器發佈 web 部署套件
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833792"
---
<a name="deploying-web-packages"></a>部署網頁套件
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 至遠端伺服器發佈 web 部署套件 2.0。
> 
> 有兩種主要方式，您可以在此部署到遠端伺服器的 web 套件：
> 
> - 您可以直接使用 MSDeploy.exe 命令列公用程式。
> - 您可以執行 *[專案名稱].deploy.cmd*建置程序會產生的檔案。
> 
> 最終結果是相同不論哪種方法使用。 基本上，所有 *。 deploy.cmd*檔案是一些預先定義的值，以執行 MSDeploy.exe，讓您不必提供完整的資訊，以便將封裝部署。 這可簡化部署程序。 相反地，使用 MSDeploy.exe 直接提供給您更大的彈性完全您套件的部署方式。
> 
> 您使用哪種方法將取決於各種因素，包括控制程度您需要將部署程序，而且不論您的目標網路部署遠端代理程式服務或 Web 部署處理常式。 本主題說明如何使用每一種方法，並識別每一種方法在適當時。
> 
> 工作與本主題中的逐步解說假設：
> 
> - 您已建置並封裝您的 web 應用程式中所述[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。
> - 您已修改*之 SetParameters.xml*檔案，以目標環境，提供正確的參數值，如中所述[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)。


執行 [*專案名稱*]*。 deploy.cmd*檔案是最簡單的方式，將 web 套件部署。 特別是，使用 *。 deploy.cmd*檔案提供透過直接使用 MSDeploy.exe 下列優點：

- 您不需要指定的 web 部署封裝的位置&#x2014; *。 deploy.cmd*檔案已經知道其所在。
- 您不需要指定的位置*之 SetParameters.xml*檔案&#x2014; *。 deploy.cmd*檔案已經知道其所在。
- 您不需要指定來源和目的地的 MSDeploy 提供者&#x2014; *。 deploy.cmd*檔案已經知道要使用哪一個值。
- 您不需要指定 MSDeploy 作業設定&#x2014; *。 deploy.cmd*檔案通常所需的值到 MSDeploy.exe 命令會自動新增。

在您使用之前 *。 deploy.cmd*檔案來部署 web 封裝時，您應該確保：

- *。 Deploy.cmd*檔案中，[*專案名稱*]。*之 SetParameters.xml*檔案和網頁套件 ([*專案名稱*]。*zip*) 位於相同的資料夾。
- 執行的電腦上安裝 web Deploy (MSDeploy.exe) *。 deploy.cmd*檔案。

*。 Deploy.cmd*檔案支援各種不同的命令列選項。 當您從命令提示字元執行該檔案時，這是基本語法：


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


您必須指定 **/T**旗標或 **/Y**旗標，指出您是否想要分別執行嘗試執行或即時部署 （請勿在同一個命令使用這兩個旗標）。 下表說明每個這些旗標的用途。

| 旗標 | 描述 |
| --- | --- |
| **/T** | 呼叫以 MSDeploy.exe **– whatif**旗標，指出嘗試執行。 而不是部署封裝，它會建立報表，如果您未部署此封裝會發生什麼事。 |
| **/Y** | 呼叫不含 MSDeploy.exe **– whatif**旗標。 這會將封裝部署在本機電腦或指定的目的地伺服器。 |
| **/M** | 指定目的地伺服器名稱，或服務 URL。 如需有關您可以在此處提供的值的詳細資訊，請參閱**端點考量**本主題中的區段。 如果您省略 **/M**旗標，封裝將會部署到本機電腦。 |
| **/A** | 指定 MSDeploy.exe 應該用來執行部署的驗證類型。 可能的值為**NTLM**並**基本**。 如果您省略 **/A**旗標，驗證類型預設值為**NTLM**部署為 Web 部署遠端代理程式服務，而**基本**以部署至 Web Deploy處理常式。 |
| **/U** | 指定使用者名稱。 這只有當您使用基本驗證時，才適用。 |
| **/P** | 指定密碼。 這只有當您使用基本驗證時，才適用。 |
| **/L** | 表示封裝應該要部署到本機 IIS Express 執行個體。 |
| **/G** | 指定使用部署封裝[tempAgent 提供者設定](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。 如果您省略 **/G**旗標，預設值為**false**。 |

> [!NOTE]
> 每次建置程序會建立 web 封裝，它也會建立名為的檔案 *[專案名稱].deploy readme.txt*說明這些部署選項。


除了這些旗標，您可以指定 Web Deploy 作業設定為其他 *。 deploy.cmd*參數。 您指定任何其他設定會直接傳到基礎 MSDeploy.exe 命令。 如需有關這些設定的詳細資訊，請參閱 < [Web Deploy 作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。

假設您想要將 ContactManager.Mvc web 應用程式專案部署到測試環境中，執行 *。 deploy.cmd*檔案。 您的測試環境設定為使用 Web 部署遠端代理程式服務，如中所述[設定 Web 伺服器進行 Web 部署發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 若要部署 web 應用程式，您需要完成後續步驟。

**若要部署 web 應用程式使用。 deploy.cmd 檔案**

1. 建置和封裝 web 應用程式專案，如中所述[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*檔案，以包含您的測試環境的正確的參數值中所述[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)。
3. 開啟 [命令提示字元] 視窗並瀏覽至的位置*ContactManager.Mvc.deploy.cmd*檔案。
4. 輸入下列命令，並再按 Enter 鍵：

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

在這個範例中：

- **/Y**旗標會指出您想要實際部署封裝，而不是執行試用版執行。
- **/M**旗標會指出您想要將封裝部署到名為 TESTWEB1 的伺服器。 此值，請從 MSDeploy.exe 會嘗試將封裝部署在與 Web 部署遠端代理程式服務 http://TESTWEB1/MSDeployAgentService 。
- **/A**旗標會指出您想要使用 NTLM 驗證。 因此，您不需要指定使用者名稱和密碼。

為了說明如何使用 *。 deploy.cmd*檔案可簡化部署程序，看看取得產生及執行您執行 MSDeploy.exe 命令*ContactManager.Mvc.deploy.cmd*使用如上所示的選項。


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


如需有關使用 *。 deploy.cmd*檔案來部署 web 封裝，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。

## <a name="using-msdeployexe"></a>使用 MSDeploy.exe

雖然使用 *。 deploy.cmd*檔案通常可簡化部署程序，並在某些情況下，建議您最好直接使用 MSDeploy.exe 時。 例如: 

- 如果您想要部署至 Web 部署處理常式以非系統管理員使用者身分，就無法使用 *。 deploy.cmd*檔案。 下所述，這是因為 Web Deploy 2.0 中的 bug**端點考量**。
- 如果您想要以手動方式切換不同*之 SetParameters.xml*中不同位置的檔案，您可能會想要直接使用 MSDeploy.exe。
- 如果您想要覆寫數個 MSDeploy.exe 命令列引數，您可能想要直接使用 MSDeploy.exe。

當您使用 MSDeploy.exe 時，您需要提供三個關鍵資訊：

- A **– 來源**參數，指出您的資料來自何處。
- A **– dest**參數，指出其中您要的資料。
- A **– 動詞**參數，指出[作業](https://technet.microsoft.com/library/dd568989(WS.10).aspx)您想要執行。

MSDeploy.exe 依賴[Web Deploy 提供者](https://technet.microsoft.com/library/dd569040(WS.10).aspx)程序的來源和目的地的資料。 Web 部署包含大量的這些提供者表示它可以使用的應用程式和資料來源的範圍&#x2014;例如，有提供者的 SQL Server 資料庫、 IIS web 伺服器、 憑證、 全域組件快取 (GAC) 組件中，各種不同的組態檔，以及許多其他類型的資料。 這兩個 **– 來源**參數並 **– dest**參數必須指定提供者，在表單中 **– 來源**: [*providerName*] = [*位置*]。 當您要部署網頁套件至 IIS 網站時，您應該使用下列值：

- **– 來源**一律會提供者[封裝](https://technet.microsoft.com/library/dd569019(WS.10).aspx)。 例如: 

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Dest**一律會提供者[自動](https://technet.microsoft.com/library/dd569016(WS.10).aspx)。例如: 

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **-Verb**總是**同步處理**。

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

此外，您必須指定各種其他[提供者特有的設定](https://technet.microsoft.com/library/dd569001(WS.10).aspx)和一般[作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 例如，假設您想要部署至預備環境 ContactManager.Mvc web 應用程式。 部署將目標 Web 部署處理常式，而且必須使用基本驗證。 若要部署 web 應用程式，您需要完成後續步驟。

**使用 MSDeploy.exe 的 web 應用程式部署**

1. 建置和封裝 web 應用程式專案，如中所述[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*檔案，以包含您的預備環境的正確的參數值中所述[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)。
3. 開啟 [命令提示字元] 視窗並瀏覽至 MSDeploy.exe 的位置。 這通常是在 Web 部署 V2\msdeploy.exe %PROGRAMFILES%\IIS\Microsoft。
4. 輸入下列命令，並再按 Enter 鍵 （忽略分行符號）：

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

在這個範例中：

- **– 來源**參數指定**封裝**提供者，並指出 web 封裝的位置。
- **– Dest**參數指定**自動**提供者。 **ComputerName**設定可在目的地伺服器上的 Web 部署處理常式的服務 URL。 **Authtype**設定表示您想要使用基本驗證，因此您需要提供**username**並**密碼**。 最後， **includeAcls ="False"** 設定可讓您表示您不想要將存取控制清單 (Acl) 中的檔案複製到目的地伺服器的來源 web 應用程式中。
- **– 動詞： 同步處理**引數會指出您要複寫的來源內容，在目的地伺服器上。
- **-DisableLink**引數可讓您表示您不想要複寫的應用程式集區、 虛擬目錄設定或目的地伺服器上的安全通訊端層 (SSL) 憑證。 如需詳細資訊，請參閱 < [Web 部署連結延伸](https://technet.microsoft.com/library/dd569028(WS.10).aspx)。
- **– SetParamFile**參數提供的位置*之 SetParameters.xml*檔案。
- **– AllowUntrusted**參數指出 Web Deploy 應該接受不受信任的憑證授權單位所發出的 SSL 憑證。 如果您要部署至 Web 部署處理常式，而且您已使用自我簽署的憑證來保護服務的 URL，您需要包含此參數。

## <a name="automating-web-package-deployment"></a>將 Web 套件部署自動化

在許多企業案例中，您會想將您的 web 套件部署較大的單一步驟或自動化的部署的一部分。 不論您選擇將您的 web 套件部署執行 *。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您可以將命令參數化，並呼叫它們從 Microsoft Build Engine (MSBuild) 中的目標專案檔。

在連絡人管理員範例方案中，看看**PublishWebPackages**中的目標*Publish.proj*檔案。 此目標，針對每個執行一次 *。 deploy.cmd*名為的項目清單所識別的檔案**PublishPackages**。 目標會使用屬性和項目中繼資料來建置針對每個一組完整的引數值 *。 deploy.cmd*檔案，然後使用**Exec**工作來執行命令。


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> 專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介的廣泛概觀，請參閱 <<c0> [ 了解專案檔](understanding-the-project-file.md)並[了解建置程序](understanding-the-build-process.md)。


## <a name="endpoint-considerations"></a>端點的考量

不論您是否執行部署網頁套件 *。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您必須指定電腦名稱或您的部署的服務端點。

如果目的地 web 伺服器設定為使用 Web 部署遠端代理程式服務的部署，您可以指定目標服務的 URL 做為目的地。


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


或者，您可以指定單獨的伺服器名稱作為目的地，以及 Web Deploy，將會推斷遠端代理程式服務的 URL。


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


如果目的地 web 伺服器設定為使用 Web 部署處理常式的部署，您需要指定端點位址的 IIS Web 管理服務 (WMSvc) 作為目的地。 根據預設，其格式為：


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


您可以為任何使用這些端點的目標 *。 deploy.cmd*檔案或直接 MSDeploy.exe。 不過，如果您想要以非系統管理員使用者，部署至 Web 部署處理常式中所述[設定 Web 伺服器進行 Web 部署發行 （Web 部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)，您需要將查詢字串新增到服務端點位址。


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


這是因為非系統管理員使用者不需要伺服器層級存取 IIS;他或她只有特定的 IIS 網站的存取權。 在撰寫時，因為在 Web 發行管線 (WPP)，一個錯誤時，您無法執行 *。 deploy.cmd*檔案中，使用包含查詢字串的端點位址。 在此案例中，您需要將 web 套件部署直接使用 MSDeploy.exe。

> [!NOTE]
> 如需有關 Web 部署遠端代理程式服務和 Web 部署處理常式的詳細資訊，請參閱 <<c0> [ 選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。 如需有關如何設定您的環境特定的專案檔案，以部署至這些端點的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="authentication-considerations"></a>驗證考量

不論您是否執行部署網頁套件 *。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您需要指定驗證類型。 Web Deploy 接受兩個可能值： **NTLM**或是**基本**。 如果您指定基本驗證時，您也需要提供使用者名稱和密碼。 有各種因素，您必須要知道當您選取驗證類型：

- 如果您要部署至 Web 部署遠端代理程式服務，您必須使用 NTLM 驗證。 遠端代理程式服務不接受基本驗證認證。
- 如果您要部署至 Web 部署處理常式，您可以使用 NTLM 或 基本驗證。 預設值是 「 基本驗證。 雖然基本驗證需要使用者名稱和密碼以純文字傳送，您的認證會受到保護，因為 Web 部署處理常式一律使用 SSL 加密。
- 如果您的網頁套件包含的資料庫中，在 web 伺服器和資料庫伺服器是不同的電腦，您將無法使用 NTLM 驗證，因為部署資料庫[NTLM 「 雙躍點 」 限制](https://go.microsoft.com/?linkid=9805120)。 您要部署連接字串中使用 SQL Server 認證，或提供 Web deploy 的基本驗證認證。 這個問題有描述更詳細地[部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

## <a name="conclusion"></a>結論

本主題描述如何部署網頁套件藉由執行 *。 deploy.cmd*檔案或直接使用 MSDeploy.exe。 它說明當每一種方法可能適合，並描述如何將參數化和更大的單一步驟或自動化的建置程序的過程中執行部署命令。

## <a name="further-reading"></a>進一步閱讀

如需有關如何建立並參數化 web 部署套件的指引，請參閱 <<c0> [ 建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)並[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)。 如需如何建置和部署 web 封裝，從 Team Foundation Server (TFS) 的執行個體的指引，請參閱[自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 如需如何自訂和部署程序的疑難排解資訊，請參閱[排除的檔案和資料夾從部署](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md)。

> [!div class="step-by-step"]
> [上一頁](configuring-parameters-for-web-package-deployment.md)
> [下一頁](deploying-database-projects.md)
