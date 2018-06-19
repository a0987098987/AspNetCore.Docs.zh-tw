---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 執行是什麼，若部署 |Microsoft 文件
author: jrjlee
description: 本主題說明如何執行 '該怎麼辦' （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 和 V 部署...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879981"
---
<a name="performing-a-what-if-deployment"></a>執行"What If"部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明如何執行 「 假設 」 （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 與 VSDBCMD 部署。 這可讓您判斷特定的目標環境上部署邏輯的影響之前您實際部署應用程式。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，兩個專案檔案中的組建和部署程序控制由&#x2014;其中一個包含組建指示適用於每個目的地環境中和包含特定環境的建置和部署設定。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="performing-a-what-if-deployment-for-web-packages"></a>執行"What If"部署 Web 封裝

Web Deploy 包含可讓您執行 「 假設 」 的部署中的功能 （或試用版） 模式。 當您部署 「 如果 」 模式中的成品時，Web Deploy 產生記錄檔，您必須執行部署，但它實際上不會變更目的地伺服器上的任何項目。 檢閱記錄檔可協助您了解您的部署必須在目的地伺服器上，尤其是何種影響：

- 將加入功能。
- 會取得更新項目。
- 會刪除項目。

因為 「 如果 」 部署不會實際變更目的地伺服器上，什麼它不一定會執行作業的任何項目是預測是否會成功部署。

中所述[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)，您可以部署 web 封裝使用兩種方式的 Web Deploy&#x2014;使用 MSDeploy.exe 命令列公用程式，直接或透過執行 *。 deploy.cmd*檔案會產生建置程序。

如果您直接使用 MSDeploy.exe，您可以藉由新增執行 「 假設 」 部署 **– whatif**旗標設為您的命令。 例如，若要評估您部署到預備環境的 ContactManager.Mvc.zip 封裝會發生什麼事，MSDeploy 命令應該與以下相似：


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


當您滿意您 「 如果 」 部署的結果時，您可以移除 **– whatif**旗標，以執行實際的部署。

> [!NOTE]
> 如需有關 MSDeploy.exe 命令列選項的詳細資訊，請參閱[Web 部署作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。


如果您使用 *。 deploy.cmd*檔案中，您可以藉由執行 「 假設 」 部署 **/t**旗標 （試用模式） 旗標，而不是 **/y**中的旗標 （"yes"或更新模式）您的命令。 例如，若要評估執行部署 ContactManager.Mvc.zip 封裝會發生什麼事 *。 deploy.cmd*檔案中，您的命令應該會與以下相似：


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


當您滿意 「 試用模式 」 部署的結果時，您可以取代 **/t**加上旗標與 **/y**旗標，以執行實際的部署：


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> 如需命令列選項的詳細資訊 *。 deploy.cmd*檔，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。 如果您執行 *。 deploy.cmd*檔案但未指定任何旗標，命令提示字元會顯示可用旗標的清單。


## <a name="performing-a-what-if-deployment-for-databases"></a>執行"What If"部署的資料庫

本節假設您使用 VSDBCMD 公用程式來執行累加式、 結構描述為基礎的資料庫部署。 這種方法更詳細地說明[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 我們建議，您自己熟悉本主題之前套用此處描述的概念。

當您使用在 VSDBCMD**部署**模式中，您可以使用 **/dd** (或 **/DeployToDatabase**) 是否 VSDBCMD 實際部署資料庫，或只產生控制旗標部署指令碼。 如果您要部署.dbschema 檔案，這是行為：

- 如果您指定 **/dd+** 或 **/dd**，VSDBCMD 會產生部署指令碼及部署資料庫。
- 如果您指定 **/dd-** 或省略參數，VSDBCMD 皆會產生部署指令碼只。

> [!NOTE]
> 如果您要部署.deploymanifest 檔案，而不是.dbschema 檔案的行為 **/dd**參數是很複雜。 基本上，VSDBCMD 會略過的值 **/dd**切換如果.deploymanifest 檔案包含**DeployToDatabase**值的項目**True**。 [部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)說明此行為，以完整模式。


例如，若要產生的部署指令碼**ContactManager**資料庫而不需實際部署的資料庫，VSDBCMD 命令應該會與以下相似：


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD 差異式資料庫部署工具，而且部署指令碼以動態方式在這種情況產生以包含所有 SQL 命令需要更新目前資料庫中，如果有指定的結構描述。 檢閱部署指令碼，是實用的方式，來判斷什麼影響您的部署會對目前的資料庫和它所包含的資料。 例如，您可能想要決定：

- 是否將會移除任何現有的資料表，並，因而造成資料遺失。
- 是否作業的順序會帶來風險造成資料遺失，例如，如果您要分割或合併的資料表。

如果您滿意部署指令碼，您可以重複使用 VSDBCMD **/dd+** 進行變更的旗標。 或者，您可以編輯的部署指令碼，以符合您的需求，然後在資料庫伺服器上手動執行它。

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>將"What If"功能整合到自訂的專案檔

在更複雜的部署案例中，您會想中所述，使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來封裝您的組建和部署邏輯，[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 例如，在[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案， *Publish.proj*檔案：

- 建置方案。
- 使用 Web Deploy 封裝和部署 ContactManager.Mvc 應用程式。
- 使用 Web Deploy 封裝和部署 ContactManager.Service 應用程式。
- 部署**ContactManager**資料庫。

當您將多個 web 封裝及/或資料庫的部署整合至單一步驟的程序以這種方式時，您也可以在 「 如果 」 模式中執行整個部署的選項。

*Publish.proj*檔案示範如何您可以這樣做。 首先，您必須建立要儲存 「 如果 」 值的屬性：


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


在此情況下，您已建立一個名為屬性**WhatIf**預設值是**false**。 使用者可以覆寫此值將屬性設定為**true**在命令列參數中，當您稍後就會看到。

下一個階段是參數化 Web Deploy 和 VSDBCMD 命令，以便反映旗標**WhatIf**屬性值。 例如下, 一個目標 (取自*Publish.proj*檔案，並簡化) 執行 *。 deploy.cmd*來部署 web 封裝的檔案。 根據預設，此命令包含 **/Y**參數 （"yes"或更新模式）。 如果**WhatIf**設**true**，將會取代運算式 **/T** （試用版或 「 假設 」 模式） 的參數。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


同樣地下, 一個目標會使用 VSDBCMD 公用程式來部署資料庫。 根據預設， **/dd**未包含參數。 這表示 VSDBCMD 會產生部署指令碼，但不是會將部署資料庫&#x2014;換句話說，「 假設 」 狀況。 如果**WhatIf**屬性未設定為**true**、 **/dd**加入參數及 VSDBCMD 會部署資料庫。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


您可以使用相同的方法，所有相關的命令參數化專案檔中。 當您想要執行 「 假設 」 部署時，然後您可以只提供**WhatIf**屬性值，從命令列：


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


如此一來，您可以執行 「 假設 」 部署在單一步驟中所有專案元件。

## <a name="conclusion"></a>結論

本主題說明如何執行 「 假設 」 使用 Web Deploy、 VSDBCMD 和 MSBuild 的部署。 「 假設 」 部署可讓您評估建議的部署的影響，實際對目的地環境的任何變更之前。

## <a name="further-reading"></a>進一步閱讀

如需有關 Web Deploy 命令列語法的詳細資訊，請參閱[Web 部署作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。 如需命令列選項，當您使用的指引 *。 deploy.cmd*檔案，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。 如需 VSDBCMD 命令列語法的指引，請參閱[VSDBCMD 的命令列參考。EXE （部署和結構描述匯入）](https://msdn.microsoft.com/library/dd193283.aspx)。

> [!div class="step-by-step"]
> [上一頁](advanced-enterprise-web-deployment.md)
> [下一頁](customizing-database-deployments-for-multiple-environments.md)
