---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: 自訂多個環境的資料庫部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何修改特定的目標環境資料庫的屬性做為部署程序的一部分。 注意： 本主題假設個...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 3a368e5055f4921b68f5c5eb2739728a2f1fd4d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378069"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>自訂多個環境的資料庫部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何修改特定的目標環境資料庫的屬性做為部署程序的一部分。
> 
> > [!NOTE]
> > 本主題假設您要部署使用 MSBuild.exe 和 VSDBCMD.exe Visual Studio 2010 資料庫專案。 如需有關為什麼您可能會選擇這種方法的詳細資訊，請參閱[企業中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)並[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。
> 
> 
> 當您將資料庫專案部署到多個目的地時，您通常需要自訂每個目標環境的資料庫部署屬性。 例如，在測試環境中您會通常重新建立資料庫在每個部署，而在預備或生產環境中會更容易進行累加式更新，才能保留您的資料。
> 
> 在 Visual Studio 2010 資料庫專案中，部署設定都包含在部署組態 (.sqldeployment) 檔案。 本主題將說明如何建立環境特定部署組態檔案，並指定您的想来用來當作 VSDBCMD 參數。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

本主題假設：

- 您使用解決方案部署分割專案檔案的方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 中所述，從專案檔，以部署資料庫專案中，呼叫 VSDBCMD[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

若要建立支援不同的資料庫部署屬性之間的目標環境的部署系統，您將需要：

- 建立每個目標環境的部署組態 (.sqldeployment) 檔。
- 建立指定部署設定檔作為命令列切換 VSDBCMD 命令。
- 參數化 VSDBCMD 命令，在 Microsoft Build Engine (MSBuild) 專案檔案中，以 VSDBCMD 選項適合目標環境。

本主題將說明如何執行上述各程序。

## <a name="creating-environment-specific-deployment-configuration-files"></a>建立環境特定部署組態檔

根據預設，資料庫專案包含名為單一部署組態檔*Database.sqldeployment*。 如果您在 Visual Studio 2010 中開啟此檔案，您可以看到您可以使用不同的部署選項：

- **部署比較定序**。 這可讓您選擇是否要使用您的專案的資料庫定序 (*來源*定序) 或目的地伺服器的資料庫定序 (*目標*定序)。 在大部分情況下，您會想要使用來源定序，當您將部署到開發或測試環境。 當您部署至預備或生產環境時，您通常會想要保留不變，以避免任何互通性問題的目標定序。
- **部署資料庫屬性**。 這可讓您選擇是否要套用資料庫的屬性，如同*Database.sqlsettings*檔案。 當您第一次部署資料庫時，您應該部署資料庫屬性。 如果您正在更新現有的資料庫，屬性應該已經就緒，而且您應該不需要重新部署它們。
- **永遠重新建立資料庫**。 這可讓您選擇是否要重新建立目標資料庫，每次部署或進行將目標資料庫的增量變更的最新狀態，與您的結構描述。 如果您重新建立資料庫時，將會遺失現有的資料庫中的任何資料。 因此，您應該通常設定為**false**部署至預備環境或生產環境。
- **如果發生資料遺失可能會封鎖累加部署**。 這可讓您選擇是否應該停止部署，是否資料庫結構描述變更會造成資料遺失。 您通常設定為 **，則為 true**為了部署至生產環境中，提供您一個機會介入並保護任何重要資料。 如果您已將**永遠重新建立資料庫**要**false**，這項設定會有任何作用。
- **在單一使用者模式中執行部署**。 這通常不開發或測試環境中的問題。 不過，您應該通常設定為 **，則為 true**部署至預備環境或生產環境。 這可以防止使用者對資料庫進行變更，而部署正在進行中。
- **備份資料庫，再部署**。 您通常設定為 **，則為 true**當您將部署至生產環境中，以防範資料遺失。 您也可以將它設定為 **，則為 true**當您將部署至預備環境中，如果暫存資料庫包含大量資料。
- **產生之目標資料庫內但不是資料庫專案中物件的 DROP 陳述式**。 在大部分情況下，這是不可或缺的一部分對資料庫進行累加式變更。 如果您已將**永遠重新建立資料庫**要**false**，這項設定會有任何作用。
- **請勿使用 ALTER ASSEMBLY 陳述式來更新 CLR 型別**。 此設定會決定 SQL Server 至較新的組件版本更新 common language runtime (CLR) 類型的方式。 這應該設定為**false**在大部分情況下。

下表顯示不同的目的地環境的典型的部署設定。 不過，您的設定可能是您的實際需求而有所不同。

|  | 開發人員/測試 | 預備/整合 | 生產環境 |
| --- | --- | --- | --- |
| **部署比較定序** | 原始程式檔 | 目標 | 目標 |
| **部署資料庫屬性** | True | 只有第一次 | 只有第一次 |
| **永遠重新建立資料庫** | True | False | False |
| **如果發生資料遺失可能會，封鎖累加部署** | False | 或許 | True |
| **在單一使用者模式中執行部署指令碼** | False | True | True |
| **部署前備份資料庫** | False | 或許 | True |
| **產生 DROP 陳述式的目標資料庫中的物件，但不在資料庫專案** | False | True | True |
| **請勿使用 ALTER ASSEMBLY 陳述式來更新 CLR 型別** | False | False | False |
  

> [!NOTE]
> 如需有關資料庫部署屬性和環境的考量因素的詳細資訊，請參閱 <<c0> [ 概觀的資料庫專案設定](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)，[如何： 設定的部署詳細資料屬性](https://msdn.microsoft.com/library/dd172125.aspx)， [建置及部署資料庫到隔離式的開發環境](https://msdn.microsoft.com/library/dd193409.aspx)，並[建置，並將資料庫部署到預備環境或生產環境](https://msdn.microsoft.com/library/dd193413.aspx)。


若要支援多個目的地的資料庫專案的部署，您應該建立每個目標環境的部署組態檔。

**若要建立的環境特定設定檔**

1. 在 Visual Studio 2010 中，在**方案總管** 視窗中，以滑鼠右鍵按一下您的資料庫專案，然後**屬性**。
2. 在資料庫專案的 屬性 頁面中，在**部署**索引標籤中，於**部署設定檔**資料列中，按一下 **新增**。

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. 中**新的部署組態檔**對話方塊方塊中，為檔案提供有意義的名稱 (例如**TestEnvironment.sqldeployment**)，然後按一下 **儲存**。
4. 在  *[Filename] * * *.sqldeployment** 頁面設定部署屬性，以符合您的目的地環境的需求，然後儲存檔案。

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. 請注意新的檔案加入資料庫專案中的 [屬性] 資料夾。

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>在 VSDBCMD 中指定部署設定檔

當您使用 Visual Studio 2010 中的方案組態 （例如偵錯和發行） 時，您可以將部署設定檔，與每個組態。 當您建置的特定組態時，建置程序會產生組態特定部署組態檔案會指向特定組態的部署資訊清單檔。 不過，其中一個主要的目的是部署在這些教學課程中所述的方法是讓人能夠控制部署程序，而不需使用 Visual Studio 2010 和方案組態。 這種方法，將方案組態設定是無論目標部署環境相同。 量身打造您的資料庫部署到特定目的地環境，您可以使用 VSDBCMD 命令列選項來指定您的部署組態檔案。

若要在您 VSDBCMD 指定部署設定檔，請使用**p/DeploymentConfigurationFile**參數並提供您的檔案的完整路徑。 這會覆寫的部署資訊清單識別的部署組態檔。 例如，您可以使用此 VSDBCMD 命令來部署**ContactManager**至測試環境的資料庫：


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> 請注意，在建置程序可能會將檔案重新命名.sqldeployment 時它會將檔案複製到輸出目錄。


如果您使用預先部署或部署後 SQL 指令碼中的 SQL 命令變數，您可以使用類似的方法，您的部署相關聯的環境特定.sqlcmdvars 檔案。 在此案例中，您會使用**p/SqlCommandVariablesFile**參數來識別您.sqlcmdvars 檔案。

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>從 MSBuild 專案檔案執行 VSDBCMD 命令

您可以使用連線，叫用 MSBuild 專案檔從 VSDBCMD 命令**Exec** MSBuild 目標內的工作。 簡單來說，看起來應該像這樣：


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- 在實務上，來讀取和重複使用，方便您的專案檔您會想要建立來儲存各種命令列參數的屬性。 這可讓使用者提供特定環境的專案檔中的屬性值，或是覆寫從 MSBuild 命令列的預設值更容易。 如果您使用分割的專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，您應該據以分割您的組建指示和兩個檔案之間的屬性：
- 環境特定設定，例如部署組態檔案名稱、 資料庫連接字串和目標資料庫名稱，應該要以環境特有的專案檔。
- 通用專案檔中應使用的 MSBuild 目標，執行 VSDBCMD 命令，以及任何通用的屬性，例如 VSDBCMD 可執行檔的位置。

您也應該確定您建置資料庫專案，才能使.deploymanifest 檔案建立並準備好使用叫用 VSDBCMD。 您可以看到這種方法 > 主題中的完整範例[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，會引導您完成中的專案檔[Contact Manager 範例方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)。

## <a name="conclusion"></a>結論

本主題描述如何調整到不同的目的地環境的資料庫屬性，當您部署資料庫專案使用 MSBuild 和 VSDBCMD。 當您需要將資料庫專案部署為較大，企業級解決方案的一部分時，這個方法很有用。 這些解決方案通常會部署到多個目的地，例如沙箱化的開發或測試環境、 預備環境或整合的平台，和實際執行或即時環境中。 每個目標環境通常需要一組唯一的資料庫部署屬性。

## <a name="further-reading"></a>進一步閱讀

如需有關如何部署使用 VSDBCMD.exe 的資料庫專案的詳細資訊，請參閱[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 如需有關如何使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

MSDN 上的下列文章會提供資料庫部署更一般指導方針：

- [資料庫專案設定的概觀](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [如何： 設定的部署詳細資料屬性](https://msdn.microsoft.com/library/dd172125.aspx)
- [建置和部署資料庫到隔離的開發環境](https://msdn.microsoft.com/library/dd193409.aspx)
- [建置和部署至預備或生產環境的資料庫](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [上一頁](performing-a-what-if-deployment.md)
> [下一頁](deploying-database-role-memberships-to-test-environments.md)
