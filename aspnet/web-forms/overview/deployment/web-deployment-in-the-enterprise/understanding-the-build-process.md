---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: "了解建置程序 |Microsoft 文件"
author: jrjlee
description: "本主題提供企業規模建置和部署處理程序的逐步解說。 本主題中描述的方法會使用自訂的 Microsoft 建置 Engin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 551e31a7a2d0a4e6259f74977c2f8e21cb694e42
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-build-process"></a>了解建置程序
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題提供企業規模建置和部署處理程序的逐步解說。 本主題中描述的方法會使用自訂的 Microsoft Build Engine (MSBuild) 專案檔，來提供更細微的控制每個層面的程序。 在專案檔中，自訂的 MSBuild 目標可用來執行部署公用程式，例如網際網路資訊服務 (IIS) Web 部署工具 (MSDeploy.exe) 和資料庫部署公用程式 VSDBCMD.exe。
> 
> > [!NOTE]
> > 先前的主題中，[了解專案檔](understanding-the-project-file.md)、 所述的 MSBuild 專案檔案的重要元件和導入分割專案檔，以支援部署到多個目標環境的概念。 如果您不熟悉這些概念，您應該檢閱[了解專案檔](understanding-the-project-file.md)逐步進行本主題之前。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](understanding-the-project-file.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="build-and-deployment-overview"></a>建置和部署概觀

在[連絡人管理員解決方案](the-contact-manager-solution.md)，三個檔案來控制組建和部署程序：

- A*通用專案檔*(*Publish.proj*)。 這包含組建和部署指示目的地環境之間不會變更。
- *環境特定專案檔*(*Env Dev.proj*)。 這包含專屬於特定的目的地環境的建置和部署設定。 例如，您可以使用*Env Dev.proj*檔案，以提供開發人員或測試環境的設定，並建立名為的替代檔案*Env Stage.proj*以提供暫存設定環境。
- A*命令檔*(*發行 Dev.cmd*)。 這包含命令指定的專案檔案，您想要執行的 MSBuild.exe。 您可以建立命令檔的每個目的地環境，其中每個檔案包含指定不同的環境特定專案檔案的 MSBuild.exe 命令。 這可讓開發人員部署至不同的環境，只要執行適當的命令檔。

在範例方案中，您可以找到這些發行方案資料夾中的三個檔案。

![](understanding-the-build-process/_static/image1.png)

您可以查看更多詳細資料中的這些檔案之前，讓我們看看當您使用這種方法時，整體的建置程序的運作方式。 在高層級，建置和部署處理程序看起來像這樣：

![](understanding-the-build-process/_static/image2.png)

發生第一件事是兩個專案檔案 & #x 2014; 一個包含通用組建和部署指示，以及一個包含環境特定的設定 & #x 2014; 會合併成單一專案檔案。 MSBuild 然後適用於透過專案檔案中的指示。 會建立每個方案，每個專案中使用專案檔中的專案。 依序呼叫其他工具，像是 Web Deploy (MSDeploy.exe) 和 VSDBCMD 公用程式，將您的 web 內容和資料庫部署到目標環境。

從開始到完成，建置和部署處理程序會執行這些工作：

1. 它會刪除輸出目錄中，以準備進行全新建置的內容。
2. 它會建置方案中的每個專案：

    1. Web 專案 & #x 2014; 在此情況下，將 ASP.NET MVC web 應用程式和 WCF web 服務 & #x 2014年; 建置程序會建立每個專案的 web 部署封裝。
    2. 資料庫專案，建置程序會建立每個專案的部署資訊清單 （.deploymanifest 檔案）。
3. 它使用 VSDBCMD.exe 公用程式來部署方案，使用各種屬性，從專案檔 & #x 2014年; 目標連接字串和資料庫名稱 & #x 2014年; 以及.deploymanifest 檔案中的每一個資料庫專案。
4. 它會使用 MSDeploy.exe 公用程式來部署方案、 專案檔中使用各種屬性，來控制部署程序中的每個 web 專案。

若要追蹤的詳細程序，您可以使用範例方案。

> [!NOTE]
> 如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="invoking-the-build-and-deployment-process"></a>叫用的組建和部署程序

若要將連絡人管理員方案部署到開發人員測試環境，開發人員執行*發行 Dev.cmd*命令檔。 這樣會叫用 MSBuild.exe，指定*Publish.proj*執行之專案檔和*Env Dev.proj*做為參數值。


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl**切換 (簡稱**/fileLogger**) 組建輸出記錄到名為*msbuild.log*目前目錄中。 如需詳細資訊，請參閱[MSBuild 命令列參考](https://msdn.microsoft.com/en-us/library/ms164311.aspx)。


此時，MSBuild 開始執行、 載入*Publish.proj*檔案，並開始處理中的指示。 第一個指令會告知 MSBuild 匯入專案檔**TargetEnvPropsFile**參數指定。


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile**參數會指定*Env Dev.proj*檔案，因此 MSBuild 會合併的內容*Env Dev.proj*檔案*Publish.proj*檔案。

MSBuild 遇到合併的專案檔中的下一個項目包括屬性群組。 屬性會以其出現在檔案中的順序進行處理。 MSBuild 將會建立提供任何指定的條件符合每個屬性，索引鍵 / 值組。 稍後檔案中定義的屬性會以先前定義檔案中的相同名稱覆寫任何屬性。 例如，請考慮**OutputRoot**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


當 MSBuild 處理第一個**OutputRoot**項目，提供類似的具名參數未提供，它會設定的值**OutputRoot**屬性**...\Publish\Out**。當它遇到第二個**OutputRoot**項目，如果條件評估為**true**，它會覆寫的值**OutputRoot**屬性的值**OutDir**參數。

MSBuild 遇到下一個項目是單一項目群組，其中包含項目具名**ProjectsToBuild**。


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild 建置名為的項目清單來處理這個指示**ProjectsToBuild**。 在此情況下，項目清單所包含的單一值 & #x 2014年; 路徑和檔案名稱的方案檔。

此時，其餘的項目是目標。 目標會以不同方式處理從屬性和項目 & #x 2014年; 基本上，目標不會處理除非它們會明確指定使用者或叫用另一個專案檔中建構。 請記得，開啟**專案**標記包含**DefaultTargets**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


這會指示要叫用 MSBuild **FullPublish**目標，如果目標不是指定何時會叫用 MSBuild.exe。 **FullPublish**目標不包含任何工作，而它只會指定相依性清單。


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


此相依性會告知 MSBuild，為了執行**FullPublish**目標，它需要叫用此清單所提供的順序中的目標：

1. 它必須叫用**清除**目標。
2. 它必須叫用**BuildProjects**目標。
3. 它必須叫用**GatherPackagesForPublishing**目標。
4. 它必須叫用**PublishDbPackages**目標。
5. 它必須叫用**PublishWebPackages**目標。

### <a name="the-clean-target"></a>「 清除 」 目標

**清除**目標基本上會刪除的輸出目錄及其所有內容，以準備新的組建。


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


請注意，目標包括**ItemGroup**項目。 當您定義屬性或項目內**目標**項目，您要建立*動態*屬性和項目。 換句話說，屬性或項目不被處理之前執行目標。 輸出目錄可能不存在或包含的任何檔案，直到開始建置程序，因此您無法建置 **\_FilesToDelete**做為靜態項目清單，您必須等候直到執行正在進行中。 因此，您會建置清單做為動態項目在目標內。

> [!NOTE]
> 在此情況下，因為**清除**目標是要執行的第一個，所以在不實際的需要，使用動態項目群組。 不過，最好使用動態屬性和項目，在這種案例中，您可能想要執行不同的順序，在某個時間點目標。  
> 您也應該是用來避免宣告不會使用的項目。 如果您只會由特定目標的項目，請考慮將它們全放置在要移除任何不必要的額外負荷，在建置程序上的目標。


擱置在一旁，動態項目**清除**目標就相當簡單，並使用內建**訊息**，**刪除**，和**RemoveDir**工作：

1. 傳送訊息至記錄器。
2. 建立要刪除檔案的清單。
3. 刪除檔案。
4. 移除輸出目錄。

### <a name="the-buildprojects-target"></a>BuildProjects 目標

**BuildProjects**目標基本上建置範例方案中的所有專案。


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


在上一個主題中，某些詳細資料中所說明的這個目標[了解專案檔](understanding-the-project-file.md)，以說明如何工作和目標參考屬性和項目。 此時，您有興趣主要**MSBuild**工作。 您可以使用這項工作來建立多個專案。 工作不會建立新的執行個體的 MSBuild.exe。它會使用目前的執行個體建立的每個專案。 此範例中值得注意的重點是部署屬性：

- **DeployOnBuild**屬性會指示 MSBuild 專案設定中執行的任何部署指示，每個專案的組建完成時。
- **DeployTarget**屬性會識別您想要建置專案之後叫用的目標。 在此情況下，**封裝**目標建置成可部署 web 封裝的專案輸出。

> [!NOTE]
> **封裝**目標會叫用 Web 發行管線 (WPP)，可提供 MSBuild 和 Web Deploy 之間的整合。 如果您想要看看的內建目標 WPP 提供，檢閱*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing 目標

如果您先研究**GatherPackagesForPublishing**目標，您會發現它實際上並未包含任何工作。 相反地，它包含單一項目群組，定義三個動態項目。


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


這些項目參考時所建立的部署套件**BuildProjects**執行目標。 您無法定義這些項目以靜態方式在專案檔中，因為項目所參考的檔案不存在直到**BuildProjects**執行目標。 相反地，項目必須以動態方式中定義的目標使用，不會叫用直到之後**BuildProjects**執行目標。

項目未使用於此目標 & #x 2014年; 此目標，只要建置的項目和每個項目值相關聯的中繼資料。 一旦處理這些項目， **PublishPackages**項目會包含兩個值的路徑*ContactManager.Mvc.deploy.cmd*檔案和路徑*ContactManager.Service.deploy.cmd*檔案。 Web Deploy web 套件，針對每個專案中，建立這些檔案，這些是您必須叫用的檔案才能將封裝部署在目的地伺服器上。 如果您開啟這些檔案，您基本上會看到以不同的建置特定的參數值 MSDeploy.exe 命令。

**DbPublishPackages**項目會包含單一值的路徑*ContactManager.Database.deploymanifest*檔案。

> [!NOTE]
> 當您建置資料庫專案，並為 MSBuild 專案檔使用相同的結構描述時，會產生.deploymanifest 檔案。 它包含部署資料庫，包括資料庫結構描述 (.dbschema) 的位置以及任何的預先部署和部署後指令碼的詳細資料所需的所有資訊。 如需詳細資訊，請參閱[概觀的資料庫建置和部署](https://msdn.microsoft.com/en-us/library/aa833165.aspx)。


您將進一步了解如何部署封裝和資料庫部署資訊清單建立及用於[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)和[部署資料庫專案](deploying-database-projects.md)。

### <a name="the-publishdbpackages-target"></a>PublishDbPackages 目標

簡單來說， **PublishDbPackages**目標會叫用 VSDBCMD 公用程式來部署**ContactManager**至目標環境的資料庫。 設定資料庫部署包含許多決策和細節，以及您將學習更多相關內容位於[部署資料庫專案](deploying-database-projects.md)和[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). 本主題中，我們將焦點放在此目標，實際的函數。

首先，請注意，開頭標記包含**輸出**屬性。


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


這是範例*目標批次處理*。 MSBuild 專案檔案中的批次處理是用於逐一查看集合的技術。 值**輸出**屬性**"%(DbPublishPackages.Identity)"**，是指**識別**中繼資料屬性**DbPublishPackages**項目清單。 此表示法，**輸出 = %***(ItemList.ItemMetadataName)*，會轉譯成：

- 分割中的項目**DbPublishPackages**成批次含有相同的項目**識別**中繼資料值。
- 執行一次每個批次的目標。

> [!NOTE]
> **識別**是其中一個[內建中繼資料值](https://msdn.microsoft.com/en-us/library/ms164313.aspx)，指派給每個項目上建立。 它的值是指**Include**屬性**項目**項目 & #x 2014年; 換句話說，路徑和檔案名稱的項目。


在此情況下，不應具有相同的路徑和檔案名稱的多個項目，因為我們基本上正在使用的其中一個批次大小。 對於每個資料庫套件一次執行目標。

您可以看到類似的標記法 **\_Cmd**屬性，建置 VSDBCMD 命令與適當的參數。


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


在此情況下， **%(DbPublishPackages.DatabaseConnectionString)**， **%(DbPublishPackages.TargetDatabase)**，和**%(DbPublishPackages.FullPath)**參照中繼資料值的**DbPublishPackages**項目集合。  **\_Cmd**屬性供**Exec**工作中，叫用命令。


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


因為此表示法，而**Exec**工作將會建立唯一的組合為基礎的批次**DatabaseConnectionString**， **TargetDatabase**，和**FullPath**中繼資料值，而且工作將會執行一次針對每個批次。 這是範例*工作批次處理*。 不過，因為目標層級的批次已經有分割成單一項目的批次中，我們項目集合**Exec**工作將會執行一次，每個反覆項目的目標只能出現一次。 換句話說，這項工作會叫用一次針對方案中每個資料庫封裝 VSDBCMD 公用程式。

> [!NOTE]
> 如需有關目標和工作批次處理的詳細資訊，請參閱 < MSBuild[批次處理](https://msdn.microsoft.com/en-us/library/ms171473.aspx)，[目標批次處理中的項目中繼資料](https://msdn.microsoft.com/en-US/library/ms228229.aspx)，和[工作批次處理中的項目中繼資料](https://msdn.microsoft.com/en-us/library/ms171474.aspx)。


### <a name="the-publishwebpackages-target"></a>PublishWebPackages 目標

現在您已叫用**BuildProjects**目標，這會產生範例方案中的每個專案的 web 部署封裝。 隨附的每個套件為*deploy.cmd*檔案，其中包含要將封裝部署至目標環境所需的 MSDeploy.exe 命令，以及*SetParameters.xml*指定的檔案必要的目標環境的詳細資料。 您也叫用**GatherPackagesForPublishing**目標，這會產生項目集合，包含*deploy.cmd*您感興趣的檔案。 基本上， **PublishWebPackages**目標可執行這些功能：

- 其中操作*SetParameters.xml*檔案包含正確的詳細資料，與目標環境中，每個套件使用**XmlPoke**工作。
- 它會叫用*deploy.cmd*每一個封裝，使用適當的參數檔案。

就像**PublishDbPackages**目標， **PublishWebPackages**目標會使用目標批次處理來確保每個 web 封裝的執行目標是一次。


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


目標內**Exec** 」 工作可用來執行*deploy.cmd*針對每個 web 封裝的檔案。


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


如需有關如何設定部署 web 封裝的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。

## <a name="conclusion"></a>結論

本主題提供如何使用分割專案檔來控制組建和部署程序從開始到完成連絡人管理員 」 範例方案的逐步解說。 使用這種方法讓您執行複雜，企業規模的部署，在單一、 可重複執行步驟中，只要執行環境特有的命令檔。

## <a name="further-reading"></a>進一步閱讀

專案檔和 WPP 的更深入簡介，請參閱[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

>[!div class="step-by-step"]
[上一頁](understanding-the-project-file.md)
[下一頁](building-and-packaging-web-application-projects.md)
