---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: 了解建置程序 |Microsoft Docs
author: jrjlee
description: 本主題提供企業規模建置和部署程序的逐步解說。 本主題中所述的方法會使用自訂的 Microsoft 建置 Engin...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 581b7e996bf5aa4c76a6bf3d55a758677c0bf897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804312"
---
<a name="understanding-the-build-process"></a>了解建置程序
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題提供企業規模建置和部署程序的逐步解說。 本主題中所述的方法會使用自訂的 Microsoft Build Engine (MSBuild) 專案檔，來提供更細微的控制程序的各個層面。 在專案檔中，自訂的 MSBuild 目標來執行部署公用程式，如 Internet Information Services (IIS) Web 部署工具 (MSDeploy.exe) 和資料庫部署公用程式 VSDBCMD.exe。
> 
> > [!NOTE]
> > 先前的主題[了解專案檔](understanding-the-project-file.md)描述 MSBuild 專案檔的重要元件，引進了分割專案檔案，以支援部署至多個目標環境的概念。 如果您不熟悉這些概念，您應該檢閱[了解專案檔](understanding-the-project-file.md)您逐步完成本主題之前。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="build-and-deployment-overview"></a>建置和部署概觀

在 [連絡管理員解決方案](the-contact-manager-solution.md)，三個檔案來控制組建和部署程序：

- A*通用專案檔*(*Publish.proj*)。 這包含不會變更目的地環境之間的組建和部署指示。
- *環境特定專案檔*(*Env Dev.proj*)。 這包含專屬於特定的目的地環境的建置和部署設定。 例如，您可以使用*Env Dev.proj*檔案，以提供開發人員或測試環境的設定，並建立名為的替代檔案*Env Stage.proj*提供預備環境的設定環境。
- A*命令檔*(*發佈 Dev.cmd*)。 這包含指定的專案檔案的命令要執行的 MSBuild.exe。 您可以建立命令檔的每個目的地環境中，其中每個檔案包含 MSBuild.exe 命令指定不同的環境特定專案檔案。 這可讓開發人員部署至不同的環境，只要執行適當的命令檔。

在範例解決方案中，您可以找到這些發行方案資料夾中的三個檔案。

![](understanding-the-build-process/_static/image1.png)

您可以查看更多詳細資料中的這些檔案之前，讓我們看看當您使用這種方法時，整體的建置程序的運作方式。 概括而言，建置和部署程序看起來像這樣：

![](understanding-the-build-process/_static/image2.png)

發生第一件事是兩個專案檔&#x2014;一個包含通用的組建和部署指示，其中包含環境特定設定的另一個&#x2014;會合併成單一專案檔案。 MSBuild 接著適用於透過專案檔中的指示。 它會建置每個方案，每個專案中使用專案檔中的專案。 然後它會呼叫其他工具，如 Web Deploy (MSDeploy.exe) 和 VSDBCMD 公用程式，以將您的 web 內容和資料庫部署到目標環境。

從開始到完成，建置和部署程序會執行下列工作：

1. 它會刪除輸出目錄中，以準備進行全新的組建的內容。
2. 它會建置方案中的每個專案：

    1. Web 專案的&#x2014;在此情況下，ASP.NET MVC web 應用程式和 WCF web 服務&#x2014;建置程序會建立每個專案的 web 部署封裝。
    2. 資料庫專案的建置程序會建立每個專案的部署資訊清單 （.deploymanifest 檔案）。
3. 它使用 VSDBCMD.exe 公用程式來部署每個資料庫專案，在解決方案中，從專案檔中使用各種屬性&#x2014;目標連接字串和資料庫名稱&#x2014;以及.deploymanifest 檔案。
4. 它會使用 MSDeploy.exe 公用程式將在解決方案中，從專案檔中使用各種屬性，來控制部署程序的每個 web 專案部署。

若要追蹤此程序的更多詳細資料，您可以使用範例方案。

> [!NOTE]
> 如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="invoking-the-build-and-deployment-process"></a>叫用的組建和部署程序

若要將連絡管理員解決方案部署到開發人員測試環境中，執行開發人員*發佈 Dev.cmd*命令檔。 這樣會叫用 MSBuild.exe，指定*Publish.proj*作為要執行的專案檔案並*Env Dev.proj*做為參數值。


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl**切換 (簡稱 **/fileLogger**) 的組建輸出記錄到名為的檔案*msbuild.log*目前目錄中。 如需詳細資訊，請參閱 < [MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。


此時，MSBuild 會開始執行，載入*Publish.proj*檔案，並開始處理中的指示。 第一個指令會告知 MSBuild，以匯入專案檔**TargetEnvPropsFile**參數指定。


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile**參數指定*Env Dev.proj*檔案，因此 MSBuild 會將合併的內容*Env Dev.proj*檔案載入*Publish.proj*檔案。

MSBuild 遇到合併的專案檔中的下一個項目是屬性群組。 屬性會以其出現在檔案中的順序處理。 MSBuild 會建立的索引鍵 / 值對每個屬性，提供，符合任何指定的條件。 檔案中稍後定義的屬性會以稍早定義檔案中的相同名稱覆寫任何屬性。 例如，請考慮**OutputRoot**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


當 MSBuild 處理第一個**OutputRoot**項目，提供類似名稱的參數未提供，它會設定的值**OutputRoot**屬性設 **...\Publish\Out**。當它遇到第二個**OutputRoot**項目，如果條件評估為 **，則為 true**，它會覆寫的值**OutputRoot**屬性的值**OutDir**參數。

MSBuild 遇到下一個項目是單一項目群組，其中包含具名項目**ProjectsToBuild**。


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild 建置名為的項目清單，處理這個指示**ProjectsToBuild**。 在此情況下，項目清單包含單一值&#x2014;的路徑和方案檔的檔名。

此時，其餘的項目是目標。 目標會以不同方式處理屬性和項目從&#x2014;基本上，目標不會處理除非它們是明確地指定使用者或叫用的專案檔內的另一個建構。 請記得，開頭**專案**標記會包括**DefaultTargets**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


這會指示 MSBuild 叫用**FullPublish**叫用 MSBuild.exe 時指定的目標，如果目標不是。 **FullPublish**目標不包含任何工作，而是它只是指定相依性清單。


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


此相依性會告知 MSBuild，以執行為了**FullPublish**目標，它需要叫用此清單所提供的順序中的目標：

1. 它必須叫用**清除**目標。
2. 它必須叫用**BuildProjects**目標。
3. 它必須叫用**GatherPackagesForPublishing**目標。
4. 它必須叫用**PublishDbPackages**目標。
5. 它必須叫用**PublishWebPackages**目標。

### <a name="the-clean-target"></a>「 清除 」 目標

**清除**目標基本上會刪除輸出目錄和其所有內容，以準備將全新的組建。


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


請注意，目標包含**ItemGroup**項目。 當您定義屬性或項目內**目標**項目，您要建立*動態*屬性和項目。 換句話說，屬性或項目不被處理在執行目標之前。 輸出目錄可能不存在或包含的任何檔案，直到建置程序開始，因此您無法建置 **\_FilesToDelete**清單做為靜態項目; 您不必等到執行正在進行中。 此情況下，您可以建置清單做為目標內的動態項目。

> [!NOTE]
> 在此情況下，因為**清除**目標是要執行的第一，實際的無須使用動態項目群組。 不過，最好使用動態屬性和項目，在這種案例中，您可能想要不同的順序，在某個時間點執行的目標。  
> 您應該也設法避免宣告絕不會使用的項目。 如果您有只用於特定目標的項目，請考慮將它們放置在要移除任何不必要的額外負荷，在建置程序的目標。


擱置在一旁，動態項目**Clean**目標就相當簡單，並利用內建**訊息**，**刪除**，和**RemoveDir**工作：

1. 將訊息傳送至記錄器。
2. 建置要刪除檔案的清單。
3. 刪除檔案。
4. 移除輸出目錄。

### <a name="the-buildprojects-target"></a>BuildProjects 目標

**BuildProjects**目標基本上會建置範例方案中的所有專案。


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


此目標，在上一個主題中，詳細說明[了解專案檔](understanding-the-project-file.md)，以說明如何工作和目標參考屬性和項目。 此時，您有興趣主要**MSBuild**工作。 您可以使用這項工作來建置多個專案。 工作不會建立 MSBuild.exe; 的新執行個體它會使用目前的執行個體建置每個專案。 此範例中值得注意的重點是部署屬性：

- **DeployOnBuild**屬性會指示 MSBuild，以每個專案的組建完成時，將專案設定中執行的任何部署指示。
- **DeployTarget**屬性會識別您想要建置專案之後叫用的目標。 在此情況下，**封裝**目標建置成可部署 web 封裝的專案輸出。

> [!NOTE]
> **封裝**目標會叫用 Web 發行管線 (WPP)，可提供 MSBuild 和 Web Deploy 之間的整合。 如果您想要的內建目標 WPP 提供，檢閱一下*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing 目標

如果您先研究**GatherPackagesForPublishing**目標，您會注意到，它實際上不包含任何工作。 相反地，它包含三個動態項目會定義單一項目群組。


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


這些項目來參考時所建立的部署套件**BuildProjects**執行目標時。 您無法定義這些項目以靜態方式在專案檔中，因為項目所參考的檔案不存在直到**BuildProjects**執行目標。 相反地，項目必須是以動態方式定義內不會叫用之前的目標後**BuildProjects**執行目標。

項目不會使用這個目標內&#x2014;的項目和每個項目值相關聯的中繼資料，只會建置這個目標。 處理這些項目，一旦**PublishPackages**項目將包含兩個值的路徑*ContactManager.Mvc.deploy.cmd*檔案和路徑*ContactManager.Service.deploy.cmd*檔案。 Web Deploy 建立這些檔案，以針對每個專案中，web 封裝的一部分，這些是您必須叫用的檔案才能部署套件在目的地伺服器上。 如果您開啟其中一個檔案，您基本上會看到各種建置特定的參數值 MSDeploy.exe 命令。

**DbPublishPackages**項目將包含單一值的路徑*ContactManager.Database.deploymanifest*檔案。

> [!NOTE]
> 當您建置資料庫專案，並為 MSBuild 專案檔使用相同的結構描述時，會產生.deploymanifest 檔案。 它包含部署資料庫，包括資料庫結構描述 (.dbschema) 的位置，以及任何部署前和部署後指令碼的詳細資料所需的所有資訊。 如需詳細資訊，請參閱 <<c0> [ 概觀的資料庫建置和部署](https://msdn.microsoft.com/library/aa833165.aspx)。


您將進一步了解如何部署封裝和資料庫部署資訊清單建立和使用中[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)並[部署資料庫專案](deploying-database-projects.md)。

### <a name="the-publishdbpackages-target"></a>PublishDbPackages 目標

簡要來說， **PublishDbPackages**目標會叫用 VSDBCMD 公用程式來部署**ContactManager**至目標環境的資料庫。 設定資料庫部署牽涉到許多決策和細微差異，以及您將進一步了解這[部署資料庫專案](deploying-database-projects.md)和[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). 在本主題中，我們將焦點放在此目標，實際的運作。

首先，請注意，要包含項目的開頭標記**輸出**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


這是範例*目標批次處理*。 在 MSBuild 專案檔中，批次處理是一種技術，用於逐一查看集合。 值**輸出**屬性， **"%(DbPublishPackages.Identity)"**，是指**識別**中繼資料屬性**DbPublishPackages**項目清單。 此表示法，**Outputs=%***(ItemList.ItemMetadataName)*，會轉譯成：

- 分割中的項目**DbPublishPackages**到包含相同的項目批次**識別**中繼資料值。
- 執行一次每個批次的目標。

> [!NOTE]
> **身分識別**是其中一個[內建的中繼資料值](https://msdn.microsoft.com/library/ms164313.aspx)指派給每個項目上建立。 它的值是指**Include**屬性中**項目**項目的&#x2014;換句話說，路徑和檔案名稱的項目。


在此情況下，因為應該永遠不會有一個以上的項目具有相同的路徑和檔名，我們基本上正在使用的其中一個批次大小。 對於每個資料庫的套件，目標被執行一次。

您可以看到類似的標記法 **\_Cmd**屬性，建置 VSDBCMD 命令與適當的參數。


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


在此情況下， **%(DbPublishPackages.DatabaseConnectionString)**， **%(DbPublishPackages.TargetDatabase)**，並 **%(DbPublishPackages.FullPath)** 所有參考中繼資料值**DbPublishPackages**項目集合。 **\_Cmd**屬性由**Exec**工作中，叫用命令。


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


因為此表示法，而**Exec**工作會建立唯一的組合為基礎的批次**DatabaseConnectionString**， **TargetDatabase**，以及**FullPath**中繼資料值，以及工作會執行一次的每個批次。 這是範例*工作批次處理*。 不過，因為目標層級的批次已經分割成單一項目的批次中，我們項目集合**Exec**工作將會執行一次，一次針對目標的每個反覆項目。 換句話說，這項工作會叫用 VSDBCMD 公用程式，針對方案中每個資料庫封裝一次。

> [!NOTE]
> 如需有關目標和工作批次處理的詳細資訊，請參閱 MSBuild[批次處理](https://msdn.microsoft.com/library/ms171473.aspx)，[目標批次處理中的項目中繼資料](https://msdn.microsoft.com/library/ms228229.aspx)，並[工作批次處理中的項目中繼資料](https://msdn.microsoft.com/library/ms171474.aspx)。


### <a name="the-publishwebpackages-target"></a>PublishWebPackages 目標

此時，您已叫用**BuildProjects**目標，這會產生範例方案中的每個專案的 web 部署封裝。 隨附的每個套件是*deploy.cmd*檔案，其中包含將封裝部署至目標環境所需的 MSDeploy.exe 命令，以及*之 SetParameters.xml*指定的檔案必要的目標環境的詳細資料。 您也叫用**GatherPackagesForPublishing**目標，這會產生包含您建立的項目集合*deploy.cmd*您感興趣的檔案。 基本上， **PublishWebPackages**目標會執行這些函式：

- 其中操作*之 SetParameters.xml*檔案以包含正確的詳細資訊，目標環境中，每個套件使用**XmlPoke**工作。
- 它會叫用*deploy.cmd*每一個封裝，使用適當的參數檔案。

就如同**PublishDbPackages**目標**PublishWebPackages**目標會使用目標批次處理以確保每個 web 封裝的執行目標時一次。


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


目標，內**Exec**工作來執行*deploy.cmd*針對每個 web 封裝的檔案。


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


如需有關如何設定網頁套件部署的詳細資訊，請參閱[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。

## <a name="conclusion"></a>結論

本主題提供如何分割專案檔用來控制建置和部署程序從開始到結束 「 連絡人管理員 」 範例方案的逐步解說。 使用這種方法讓您執行複雜的企業級部署在單一、 高再現性的步驟中，只要執行環境特有的命令檔。

## <a name="further-reading"></a>進一步閱讀

專案檔和 WPP 更深入的簡介，請參閱[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William bartholomew< /，ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [上一頁](understanding-the-project-file.md)
> [下一頁](building-and-packaging-web-application-projects.md)
