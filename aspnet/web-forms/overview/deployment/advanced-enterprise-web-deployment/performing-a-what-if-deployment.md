---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 如果執行模擬部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何執行 '如果' （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 和 V 部署...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f54a4d91ea0f735d3cd5b99c0dda5d83cbb5e5d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830645"
---
<a name="performing-a-what-if-deployment"></a>執行"What If"部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何執行 「 假設 」 （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 與 VSDBCMD 部署。 這可讓您實際部署您的應用程式之前，判斷特定的目標環境上部署邏輯的影響。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置和部署流程控制的兩個專案檔&#x2014;其中一個包含套用至每個目的地環境，以及包含環境特定建置和部署設定的組建指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="performing-a-what-if-deployment-for-web-packages"></a>執行"What If"部署 Web 封裝

Web Deploy 包含可讓您執行中的部署 「 假設 」 的功能 （或試用版） 模式。 當您部署 「 假設 」 模式中的成品時，Web Deploy 產生記錄檔，如同執行部署，但它不會實際變更目的地伺服器上的任何項目。 檢閱記錄檔，可協助您了解您的部署必須在目的地伺服器上，特別是在何種影響：

- 項目就會被新增。
- 將更新項目。
- 項目將會刪除。

因為"what if"部署不會實際變更任何項目在目的地伺服器上，它不能永遠做什麼是預測是否會成功部署。

中所述[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)，您可以將使用兩種方式中的 Web Deploy 的 web 套件部署&#x2014;使用 MSDeploy.exe 命令列公用程式，直接或透過執行 *。 deploy.cmd*檔案會產生建置程序。

如果您直接使用 MSDeploy.exe，您可以執行"what if"部署加上 **– whatif**旗標，以您的命令。 比方說，若要評估您將 ContactManager.Mvc.zip 套件部署至預備環境會發生什麼事，MSDeploy 命令看起來應該像這樣：


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


當您滿意您 「 假設 」 部署的結果時，您可以移除 **– whatif**旗標，以執行即時的部署。

> [!NOTE]
> 如需有關 MSDeploy.exe 命令列選項的詳細資訊，請參閱[Web Deploy 作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。


如果您使用 *。 deploy.cmd*檔案中，您可以執行"what if"部署包括 **/t**旗標 （試用模式） 旗標，而非 **/y**旗標 （"yes"或更新模式）您的命令。 例如，若要評估您部署 ContactManager.Mvc.zip 封裝執行會發生什麼事 *。 deploy.cmd*檔案中，您的命令應該看起來像這樣：


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


當您滿意您的 「 試用模式 」 部署的結果時，您可以取代 **/t**旗標進行 **/y**旗標，以執行即時的部署：


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> 如需有關命令列選項 *。 deploy.cmd*檔，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。 如果您執行 *。 deploy.cmd*檔案而不指定任何旗標，命令提示字元會顯示可用的旗標的清單。


## <a name="performing-a-what-if-deployment-for-databases"></a>執行"What If"部署資料庫

本節假設您使用 VSDBCMD 公用程式來執行累加式、 結構描述為基礎的資料庫部署。 這種方法更詳細地說明[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 我們建議，您自己熟悉本主題之前套用以下所述的概念。

當您使用在 VSDBCMD**部署**模式中，您可以使用 **/dd** (或 **/DeployToDatabase**) 是否 VSDBCMD 實際部署資料庫，或只產生要控制旗標部署指令碼。 如果您要部署.dbschema 檔案，這是行為：

- 如果您指定 **/dd+** 或是 **/dd**，VSDBCMD 會產生部署指令碼，並將資料庫部署。
- 如果您指定 **/dd-** 或省略此參數，VSDBCMD 會產生只部署指令碼。

> [!NOTE]
> 如果您要部署.deploymanifest 檔案，而不是.dbschema 檔案的行為 **/dd**參數是複雜多了。 基本上，VSDBCMD 會略過的值 **/dd**如果.deploymanifest 檔案包含切換**DeployToDatabase**值的項目**True**。 [部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)描述完整的這個行為。


例如，若要產生的部署指令碼**ContactManager**資料庫而不實際部署的資料庫，VSDBCMD 命令應該看起來像這樣：


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD 是差異資料庫部署工具，並因此產生部署指令碼是以動態方式以包含所有的 SQL 命令需要更新目前資料庫中，如果存在的話，指定的結構描述。 檢閱部署指令碼是一個實用的方式，來判斷項目會影響您的部署會對目前的資料庫和它所包含的資料。 例如，您可以判斷：

- 是否將會移除任何現有的資料表，以及是否會導致資料遺失。
- 是否作業的順序會帶來風險的資料遺失，比方說，如果您要分割或合併的資料表。

如果您是部署指令碼感到滿意，您可以重複使用 VSDBCMD **/dd+** 旗標，以進行變更。 或者，您可以編輯的部署指令碼，以符合您的需求，然後在資料庫伺服器上手動執行它。

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>將"What If"功能整合到自訂的專案檔

在更複雜的部署案例中，您會想要來封裝您的組建和部署邏輯，使用自訂的 Microsoft Build Engine (MSBuild) 專案檔，如中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 例如，在[Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案， *Publish.proj*檔案：

- 建置方案。
- 使用 Web Deploy 封裝和部署 ContactManager.Mvc 應用程式。
- 使用 Web Deploy 封裝和部署 ContactManager.Service 應用程式。
- 部署**ContactManager**資料庫。

當您將多個 web 封裝及/或資料庫的部署整合的單一步驟程序，如此一來時，您也可以選擇在 「 假設 」 模式中執行整個部署。

*Publish.proj*檔會示範如何可以這麼做。 首先，您必須建立要儲存 「 假設 」 值的屬性：


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


在此情況下，您已建立名為的屬性**WhatIf**預設值是**false**。 使用者可以覆寫此值的屬性設定為 **，則為 true**在命令列參數中，您很快會看到。

下一個階段是參數化 Web Deploy 和 VSDBCMD 命令，讓旗標會反映**WhatIf**屬性值。 例如下, 一個目標 (取自*Publish.proj*檔案，並簡化) 執行 *。 deploy.cmd*將 web 套件部署的檔案。 根據預設，此命令包含 **/Y**參數 （"yes"或更新模式）。 如果**WhatIf**設為 **，則為 true**，將會取代運算式 **/T** （試用版或 「 假設 」 模式） 的參數。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


同樣地下, 一個目標會將資料庫部署中，使用 VSDBCMD 公用程式。 根據預設， **/dd**未包含參數。 這表示 VSDBCMD 會產生部署指令碼，但並不會部署資料庫&#x2014;也就是說，「 假設 」 案例。 如果**WhatIf**屬性未設定為 **，則為 true**，則 **/dd**會加入參數，而且 VSDBCMD 會將資料庫部署。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


您可以使用相同的方法，專案檔中的所有相關的命令參數化。 當您想要執行 「 假設 」 部署時，您可以接著只需要提供**WhatIf**屬性值，從命令列：


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


如此一來，您可以執行"what if"部署在單一步驟中所有專案元件。

## <a name="conclusion"></a>結論

本主題說明如何執行 「 假設 」 使用 Web Deploy、 VSDBCMD 和 MSBuild 的部署。 "What if"部署可讓您評估建議的部署的影響，實際對目的地環境的任何變更之前。

## <a name="further-reading"></a>進一步閱讀

如需有關 Web Deploy 命令列語法的詳細資訊，請參閱 < [Web Deploy 作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 如需命令列選項，當您使用的指引 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。 如需 VSDBCMD 命令列語法的指引，請參閱[VSDBCMD 的命令列參考。（「 部署 」 和 「 結構描述匯入 」） 的 EXE](https://msdn.microsoft.com/library/dd193283.aspx)。

> [!div class="step-by-step"]
> [上一頁](advanced-enterprise-web-deployment.md)
> [下一頁](customizing-database-deployments-for-multiple-environments.md)
