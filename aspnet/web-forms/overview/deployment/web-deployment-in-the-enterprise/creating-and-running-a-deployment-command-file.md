---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: 建立和執行部署命令檔案 |Microsoft Docs
author: jrjlee
description: 本主題說明如何建置可讓您執行單一步驟中使用 Microsoft Build Engine (MSBuild) 專案檔，或重新部署的命令檔...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395369"
---
<a name="creating-and-running-a-deployment-command-file"></a>建立和執行部署命令檔
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何建立命令檔，可讓您執行以單一步驟、 高再現性的處理程序中使用 Microsoft Build Engine (MSBuild) 專案檔的部署。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本系列教學課程使用範例解決方案&#x2014; [Contactmanager](the-contact-manager-solution.md)方案&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解建置程序](understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="process-overview"></a>處理程序概觀

本主題中，您將了解如何建立和執行會使用這些專案檔案來執行可重複部署到您的目標環境的命令檔。 基本上，命令檔只需要包含 MSBuild 命令的：

- 告知 MSBuild，以執行環境無關*Publish.proj*檔案。
- 會告訴*Publish.proj*哪些檔案包含環境特定專案設定和其位置的檔案。

## <a name="create-an-msbuild-command"></a>建立一個 MSBuild 命令

中所述[了解建置程序](understanding-the-build-process.md)，環境特定專案檔&#x2014;，例如*Env Dev.proj*&#x2014;是要匯入環境無關*Publish.proj*在建置階段的檔案。 在一起，這兩個檔案會提供一組完整的指示，告知 MSBuild 如何建置和部署您的解決方案。

*Publish.proj*檔案使用**匯入**環境特定的專案檔案匯入的項目。


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


因此，當您使用 MSBuild.exe 來建置和部署連絡管理員解決方案時，您需要：

- 在上執行 MSBuild.exe *Publish.proj*檔案。
- 藉由提供一個名為的命令列參數來指定環境特有的專案檔的位置**TargetEnvPropsFile**。

若要這樣做，您的 MSBuild 命令看起來應該像這樣：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


從這裡開始，它是簡單的步驟，以移至可重複、 單一步驟的部署。 您只需要為.cmd 檔案中加入您的 MSBuild 命令。 在 連絡管理員解決方案，以發行資料夾包含名為的檔案*發佈 Dev.cmd* ，就是這麼。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl**參數會指示 MSBuild 來建立名為記錄檔*msbuild.log* MSBuild.exe 已叫用的工作目錄中。


若要部署或重新部署連絡管理員解決方案，您只需要執行*發佈 Dev.cmd*檔案。 當您執行該檔案時，MSBuild 將會：

- 建置方案中的所有專案。
- 產生可部署 web 封裝 web 應用程式專案。
- 產生.dbschema 和.deploymanifest 之資料庫專案檔案。
- 將 web 套件部署到 web 伺服器。
- 將資料庫部署到資料庫伺服器。

## <a name="run-the-deployment"></a>執行部署

當您已為您的目標環境中建立命令檔時，您應該能夠完成整個部署，只要執行檔案。

**若要將連絡管理員解決方案部署到您的測試環境**

1. 在您開發人員工作站上，開啟 Windows 檔案總管，並再瀏覽至的位置*發佈 Dev.cmd*檔案。
2. 按兩下檔案加以執行。
3. 如果**開啟檔案-安全性警告** 對話方塊出現時，按一下**執行**。
4. 如果您的組態設定和測試伺服器都設定正確，[命令提示字元] 視窗會顯示**建置成功**MSBuild 已完成處理的專案檔時，訊息。

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. 如果這是您已對此環境中部署解決方案的第一次，您必須將測試 web 伺服器機器帳戶，加入**db\_資料寫入元**並**db\_datareader**上的角色**ContactManager**資料庫。 此程序所述[設定資料庫伺服器進行 Web 部署發佈](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

    > [!NOTE]
    > 您只需要建立資料庫時，指派這些權限。 根據預設，建置程序將不會重新建立每個部署上的資料庫&#x2014;相反地，它會在 比較最新的結構描述現有的資料庫，並進行所需的變更。 如此一來，您應該只需要對應這些資料庫角色的第一次部署解決方案。
6. 開啟 Internet Explorer 並瀏覽至 Contact Manager 應用程式的 URL (例如`http://testweb1:85/ContactManager/`)。
7. 確認應用程式可以正常運作，您就可以將加入連絡人。

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>結論

建立命令檔，其中包含 MSBuild 指示提供您建置和部署到特定目的地環境的多專案方案的快速簡便方式。 如果您要重複多個目的地環境中部署您的解決方案，您可以建立多個的命令檔。 在每個命令檔，MSBuild 命令會建立相同的通用專案檔中，但它會指定不同的環境特定專案的檔案。 例如，發佈給開發人員或測試環境的命令檔可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


若要發佈至預備環境的命令檔可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> 如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


您也可以自訂每個環境的建置程序，藉由覆寫屬性，或設定在 MSBuild 命令的各種其他參數。 如需詳細資訊，請參閱 < [MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。

> [!div class="step-by-step"]
> [上一頁](deploying-database-projects.md)
> [下一頁](manually-installing-web-packages.md)
