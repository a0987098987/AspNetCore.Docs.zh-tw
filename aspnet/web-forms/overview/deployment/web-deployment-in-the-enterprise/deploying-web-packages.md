---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: "部署 Web 封裝 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何發行 web 部署套件到遠端伺服器使用網際網路資訊服務 (IIS) Web Deployment Tool (Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: cd2bfa07262155b68ac4605fc7e9748d276d3193
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="deploying-web-packages"></a>部署 Web 封裝
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 至遠端伺服器發行 web 部署套件 2.0。
> 
> 有兩種主要的方式，您可以在此部署到遠端伺服器的 web 套件：
> 
> - 您可以直接使用 MSDeploy.exe 命令列公用程式。
> - 您可以執行*[專案名稱].deploy.cmd*建立期間產生的檔案。
> 
> 最終結果是哪種方法不論您使用相同的。 基本上，所有*。 deploy.cmd*檔案是要預先決定的值，以執行 MSDeploy.exe，因此您不需要提供以部署封裝的詳細資訊。 這樣可簡化部署程序。 相反地，使用 MSDeploy.exe 直接提供您更多的彈性透過完全套件的部署方式。
> 
> 您使用哪種方法將取決於各種因素，包括控制程度，您需要透過部署程序，以及是否您的目標 Web 部署遠端代理程式服務或 Web 部署處理常式。 本主題說明如何使用每一種方法，並識別每一種方法是適當時。
> 
> 工作與本主題中的逐步解說假設：
> 
> - 您建置並封裝您的 web 應用程式中所述[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。
> - 修改了*SetParameters.xml*檔案，以提供目標環境的右側參數值中所述[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。


執行 [*專案名稱*]*。 deploy.cmd*檔案是最簡單的方式部署 web 封裝。 特別是，使用*。 deploy.cmd*檔案可提供透過直接使用 MSDeploy.exe 的下列優點：

- 您不需要指定的 web 部署套件 & #x 2014; 位置*。 deploy.cmd*檔案已經知道其所在。
- 您不需要指定的位置*SetParameters.xml*檔案 & #x 2014; *。 deploy.cmd*檔案已經知道其所在。
- 您不需要指定來源和目的地 MSDeploy 提供者 & #x 2014; *。 deploy.cmd*檔案已經知道要使用的值。
- 您不需要指定 MSDeploy 作業設定 & #x 2014; *。 deploy.cmd*檔案常需要的值給 MSDeploy.exe 命令會自動新增。

在使用之前*。 deploy.cmd*檔案來部署 web 封裝時，您應該確保：

- *。 Deploy.cmd*檔案中，[*專案名稱*]。*SetParameters.xml*檔案，以及 web 套件 ([*專案名稱*]。*zip*) 位在相同資料夾中。
- 執行的電腦上安裝 web Deploy (MSDeploy.exe) *。 deploy.cmd*檔案。

*。 Deploy.cmd*檔案支援多種命令列選項。 當您從命令提示字元執行該檔案時，這是基本語法：


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


您必須指定**/T**旗標或**/Y**旗標，指出您是否要分別執行試用或即時部署 （請勿在相同命令中使用這兩個旗標）。 下表說明每個這些旗標的用途。

| 旗標 | 描述 |
| --- | --- |
| **/T** | 呼叫 MSDeploy.exe 與**– whatif**旗標，指出嘗試執行。 而不是部署封裝，它會建立一份您未部署封裝會發生什麼事。 |
| **/Y** | 呼叫不含 MSDeploy.exe **– whatif**旗標。 這會將封裝部署到本機電腦或指定的目的地伺服器。 |
| **/M** | 指定目的地伺服器名稱，或服務 URL。 如需有關您可以在這裡提供的值的詳細資訊，請參閱**端點考量**本主題中的區段。 如果您省略**/M**旗標，封裝將會部署到本機電腦。 |
| **/A** | 指定 MSDeploy.exe 應該用來執行部署的驗證類型。 可能的值為**NTLM**和**基本**。 如果您省略**/A**旗標的驗證類型預設為**NTLM**部署至 Web 部署遠端代理程式服務和**基本**以部署至 Web Deploy處理常式。 |
| **/U** | 指定使用者名稱。 這僅適用於您使用基本驗證。 |
| **/P** | 指定密碼。 這僅適用於您使用基本驗證。 |
| **/L** | 表示封裝應該要部署到本機 IIS Express 執行個體。 |
| **/G** | 指定封裝使用部署[tempAgent 提供者設定](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。 如果您省略**/G**旗標，則值預設為**false**。 |

> [!NOTE]
> 每次建置程序會建立 web 封裝，它也會建立名為*[專案名稱].deploy readme.txt*來說明這些部署選項。


除了這些旗標，您可以指定 Web Deploy 作業設定為其他*。 deploy.cmd*參數。 您指定任何其他設定會直接傳遞至基礎 MSDeploy.exe 命令。 如需有關這些設定的詳細資訊，請參閱[Web 部署作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。

假設您想要執行的測試環境中部署 ContactManager.Mvc web 應用程式專案*。 deploy.cmd*檔案。 您的測試環境設定為使用 Web 部署遠端代理程式服務中所述[設定 Web 伺服器進行 Web 部署發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。 若要部署 web 應用程式，您需要完成下一個步驟。

**若要部署 web 應用程式使用。 deploy.cmd 檔案**

1. 建置並封裝中所述的 web 應用程式專案中，[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*檔案以包含正確的參數值，為您的測試環境中所述[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。
3. 開啟 [命令提示字元] 視窗並瀏覽至的位置*ContactManager.Mvc.deploy.cmd*檔案。
4. 輸入下列命令，並按 Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

在這個範例中：

- **/Y**旗標指出您想要實際部署封裝，而不是執行使用試用版執行。
- **/M**旗標，指出您想要將封裝部署到名為 TESTWEB1 的伺服器。 此值，從 MSDeploy.exe 會嘗試將封裝部署到 http://TESTWEB1/MSDeployAgentService 的 Web 部署遠端代理程式服務。
- **/A**旗標，指出您想要使用 NTLM 驗證。 因此，您不需要指定使用者名稱和密碼。

說明如何使用*。 deploy.cmd*檔案可簡化部署程序，看看 MSDeploy.exe 命令取得產生並執行當您執行*ContactManager.Mvc.deploy.cmd*使用以上所顯示的選項。


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


如需有關使用*。 deploy.cmd*檔案來部署 web 封裝，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。

## <a name="using-msdeployexe"></a>使用 MSDeploy.exe

雖然使用*。 deploy.cmd*檔案通常可簡化部署程序中，有一些情況時，它通常會直接使用 MSDeploy.exe。 例如: 

- 如果您想要部署至 Web 部署處理常式以非管理員使用者，則無法使用*。 deploy.cmd*檔案。 這是因為 Web Deploy 2.0 中的 bug，如底下所述**端點考量**。
- 如果您想要手動切換不同*SetParameters.xml*中不同位置的檔案，您可能會想要直接使用 MSDeploy.exe。
- 如果您想要覆寫數個 MSDeploy.exe 命令列引數，您可能會想要直接使用 MSDeploy.exe。

當您使用 MSDeploy.exe 時，您需要提供三個關鍵的資訊：

- A **– 來源**參數，指出您的資料來自何處。
- A **– 目的地**參數，指出您的資料移至的位置。
- A **– 動詞**參數，指出[作業](https://technet.microsoft.com/library/dd568989(WS.10).aspx)您想要執行。

MSDeploy.exe 依賴[Web Deploy 提供者](https://technet.microsoft.com/library/dd569040(WS.10).aspx)來處理來源和目的地的資料。 Web Deploy 包含代表的範圍就能夠使用的應用程式和資料來源 & #x 2014年的提供者大量; 例如，有 SQL Server 資料庫、 IIS web 伺服器、 憑證、 全域組件快取 (GAC) 組件，提供者不同不同的組態檔，以及許多其他類型的資料。 這兩個**– 來源**參數和**– 目的地**參數必須指定提供者，在表單中**– 來源**: [*providerName*] = [*位置*]。 當您要將 web 套件部署至 IIS 網站時，您應該使用這些值：

- **– 來源**提供者一律是[封裝](https://technet.microsoft.com/library/dd569019(WS.10).aspx)。 例如: 

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– 目的地**提供者一律是[自動](https://technet.microsoft.com/library/dd569016(WS.10).aspx)。例如: 

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– 動詞**一律**同步**。

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

此外，您必須指定各種其他[提供者專屬的設定](https://technet.microsoft.com/library/dd569001(WS.10).aspx)和一般[作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 例如，假設您想要部署到預備環境 ContactManager.Mvc web 應用程式。 部署目標 Web 部署處理常式，而且必須使用基本驗證。 若要部署 web 應用程式，您需要完成下一個步驟。

**若要部署使用 MSDeploy.exe 的 web 應用程式**

1. 建置並封裝中所述的 web 應用程式專案中，[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。
2. 修改*ContactManager.Mvc.SetParameters.xml*檔案以包含您的預備環境的正確參數值中所述[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。
3. 開啟命令提示字元視窗，並瀏覽至 MSDeploy.exe 的位置。 這通常是 %PROGRAMFILES%\IIS\Microsoft Web 部署 V2\msdeploy.exe。
4. 輸入下列命令，並再按 Enter 鍵 （忽略分行符號）：

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

在這個範例中：

- **– 來源**參數會指定**封裝**提供者，並指出 web 封裝的位置。
- **– 目的地**參數會指定**自動**提供者。 **ComputerName**設定目的地伺服器上提供的 Web 部署的處理常式服務 URL。 **a**設定表示您想要使用基本驗證，因此您需要提供**username**和**密碼**。 最後， **includeAcls ="False"**設定表示您不想要複製到目的地伺服器的來源 web 應用程式中的檔案的存取控制清單 (Acl)。
- **– 動詞命令： 同步處理**引數指出您想要將目的地伺服器上的來源內容複寫。
- **– DisableLink**引數表示您不想複寫應用程式集區、 虛擬目錄設定或目的地伺服器上的安全通訊端層 (SSL) 憑證。 如需詳細資訊，請參閱[Web 部署連結延伸](https://technet.microsoft.com/library/dd569028(WS.10).aspx)。
- **– SetParamFile**參數提供的位置*SetParameters.xml*檔案。
- **-AllowUntrusted**參數指出 Web Deploy 應該接受不受信任的憑證授權單位所發出的 SSL 憑證。 如果您要部署至 Web 部署處理常式，而且您使用自我簽署的憑證來保護服務 URL，您需要包含此參數。

## <a name="automating-web-package-deployment"></a>自動化 Web 套件部署

在許多企業案例中，您要部署 web 封裝為較大的單一步驟或自動的部署的一部分。 不論您是否選擇執行部署 web 封裝*。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您可以決定要將命令參數化及呼叫它們從 Microsoft Build Engine (MSBuild) 中的目標專案檔。

在連絡人管理員範例方案中，看看**PublishWebPackages**中目標*Publish.proj*檔案。 此目標，執行一次針對每個*。 deploy.cmd*名為的項目清單所識別檔案**PublishPackages**。 目標會使用屬性和項目中繼資料，每個建置一組完整的引數值*。 deploy.cmd*檔案，然後再使用**Exec**工作來執行命令。


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> 如需更廣泛的專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介概觀，請參閱[了解專案檔](understanding-the-project-file.md)和[瞭解建置程序](understanding-the-build-process.md)。


## <a name="endpoint-considerations"></a>端點的考量

不論您是否藉由執行部署 web 封裝*。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您必須指定電腦名稱或您的部署的服務端點。

如果目的地 web 伺服器設定為使用 Web 部署遠端代理程式服務的部署，您可以指定目標服務的 URL 做為目的地。


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


或者，您可以指定單獨的伺服器名稱做為目的地，以及 Web Deploy 會推斷遠端代理程式服務的 URL。


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


如果目的地 web 伺服器設定為使用 Web 部署處理常式的部署，您需要做為目的地指定 IIS Web 管理服務 (WMSvc) 的端點位址。 根據預設，這採用下列格式：


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


您可以使用這些端點的任何目標*。 deploy.cmd*檔案或直接 MSDeploy.exe。 不過，如果您想要以非管理員使用者，部署至 Web 部署處理常式中所述[設定 Web 伺服器進行 Web 部署發行 （網頁部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)，您需要將查詢字串新增至服務端點位址。


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


這是因為非管理員的使用者沒有伺服器層級存取 IIS。他只需要特定的 IIS 網站的存取權。 您無法在 bug 中 Web 發行管線 (WPP)，因為寫入時執行*。 deploy.cmd*檔案使用的端點位址，包括查詢字串。 在此案例中，您需要直接使用 MSDeploy.exe 部署 web 封裝。

> [!NOTE]
> 如需有關 Web 部署遠端代理程式服務和 Web 部署處理常式的詳細資訊，請參閱[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。 如需如何設定您的環境特定專案檔案，將部署至這些端點的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="authentication-considerations"></a>驗證考量

不論您是否藉由執行部署 web 封裝*。 deploy.cmd*檔案，或藉由直接使用 MSDeploy.exe，您需要指定驗證類型。 Web Deploy 接受兩個可能值： **NTLM**或**基本**。 如果您指定基本驗證，您也需要提供使用者名稱和密碼。 有各種因素，您必須了解當您選取驗證類型：

- 如果您要部署至 Web 部署遠端代理程式服務，您必須使用 NTLM 驗證。 遠端代理程式服務不接受基本驗證認證。
- 如果您要部署至 Web 部署處理常式，您可以使用 NTLM 或基本驗證。 預設值是基本驗證。 基本驗證會仰賴使用者名稱和密碼以純文字傳送，雖然您的認證會受到保護，因為 Web 部署處理常式會一律使用 SSL 加密。
- 如果您 web 的封裝包含的資料庫中，而在 web 伺服器和資料庫伺服器不同的電腦，您將無法使用 NTLM 驗證，因為部署資料庫[NTLM 「 雙躍點 」 限制](https://go.microsoft.com/?linkid=9805120)。 您需要部署連接字串中使用 SQL Server 認證，或提供 Web 部署的基本驗證認證。 這個問題有更詳細地描述[部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

## <a name="conclusion"></a>結論

本主題描述如何部署 web 封裝藉由執行*。 deploy.cmd*檔案或直接使用 MSDeploy.exe。 它說明何時可能適合使用每一種方法，和它描述如何將參數化和較大的單一步驟或自動化建置程序的一部分執行的部署命令。

## <a name="further-reading"></a>進一步閱讀

如需如何建立及參數化 web 部署套件的指引，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)和[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。 如需如何建置和部署 web 封裝，從 Team Foundation Server (TFS) 執行個體的指引，請參閱[用於自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 如需如何自訂及部署程序的疑難排解資訊，請參閱[排除檔案和資料夾從部署](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md)。

>[!div class="step-by-step"]
[上一頁](configuring-parameters-for-web-package-deployment.md)
[下一頁](deploying-database-projects.md)
