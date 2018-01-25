---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "建立和執行部署指令檔案 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何建立命令檔，以供您執行使用 Microsoft Build Engine (MSBuild) 專案檔做為單一步驟，重新部署..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>建立和執行部署指令檔
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何建立命令檔，以供您執行使用 Microsoft Build Engine (MSBuild) 專案檔，以逐步執行、 可重複執行的處理程序的部署。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員](the-contact-manager-solution.md)方案 & #x 2014; 代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎分割專案檔案描述的方法中[瞭解建置程序](understanding-the-build-process.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="process-overview"></a>處理程序概觀

在本主題中，您將學習如何建立及執行使用這些專案檔來執行可重複部署到您的目標環境的命令檔。 基本上，命令檔只需要包含 MSBuild 命令的：

- 會告知執行環境無從驗證的 MSBuild *Publish.proj*檔案。
- 會告知*Publish.proj*檔案中的檔案包含特定環境的專案設定，以及到哪裡尋找它。

## <a name="create-an-msbuild-command"></a>建立 MSBuild 命令

中所述[瞭解建置程序](understanding-the-build-process.md)，環境特定專案檔 & #x 2014; 例如， *Env Dev.proj*（& s) #x 2014; 設計來匯入環境無從驗證*Publish.proj*在建置階段檔案。 結合，這兩個檔案，提供一組完整的指示，告知 MSBuild 如何建置和部署您的方案。

*Publish.proj*檔案使用**匯入**環境特定專案檔案匯入的項目。


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


因此，當您使用 MSBuild.exe 來建置及部署方案，請連絡管理員，您需要：

- 在執行 MSBuild.exe *Publish.proj*檔案。
- 提供名為命令列參數來指定環境特定專案檔的位置**TargetEnvPropsFile**。

若要這樣做，MSBuild 命令應該會與以下相似：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


從這裡開始，它是簡單的步驟，以移至可重複、 單一步驟的部署。 您只需要為 MSBuild 命令加入至.cmd 檔案。 在連絡人管理員解決方案中，發行資料夾會包含名為*發行 Dev.cmd*會完全這。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl**參數會指示 MSBuild 來建立名為記錄檔*msbuild.log* MSBuild.exe 已叫用的工作目錄中。


若要部署或重新部署方案，請連絡管理員，您只需要執行*發行 Dev.cmd*檔案。 當您執行檔案時，則 MSBuild 將會：

- 建置方案中的所有專案。
- 產生可部署 web 封裝，web 應用程式專案。
- 產生.dbschema 和.deploymanifest 檔案的資料庫專案。
- 將 web 套件部署至 web 伺服器。
- 將資料庫部署到資料庫伺服器。

## <a name="run-the-deployment"></a>執行部署

當您已建立命令檔的目標環境時，您應該能夠只需執行該檔案以完成整個部署。

**若要將連絡人管理員方案部署到您的測試環境**

1. 在您開發人員工作站上，開啟 Windows 檔案總管，然後瀏覽至的位置*發行 Dev.cmd*檔案。
2. 按兩下該檔案來執行它。
3. 如果**開啟檔案-安全性警告**對話方塊出現時，按一下**執行**。
4. 如果將您的組態設定和測試伺服器設定正確，在命令提示字元視窗會顯示**建置成功**MSBuild 已完成處理的專案檔時，訊息。

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. 如果這是方案部署到此環境的第一次，您必須將測試 web 伺服器機器帳戶，加入**db\_datawriter**和**db\_datareader**角色**ContactManager**資料庫。 此程序所述[設定資料庫伺服器來進行 Web 部署發行](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

    > [!NOTE]
    > 您只需要建立資料庫時，指派這些權限。 根據預設，在建置程序將無法重新建立每個部署 & #x 2014年上的資料庫; 相反地，它會比較現有的資料庫最新的結構描述並進行所需的變更。 如此一來，您應該只需要將這些資料庫角色對應您部署方案的第一次。
6. 開啟 Internet Explorer 並瀏覽至 連絡人管理員應用程式的 URL (例如， `http://testweb1:85/ContactManager/`)。
7. 確認應用程式可以正常運作，您可以新增連絡人。

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>結論

建立包含 MSBuild 指示的命令檔可讓您建置與部署到特定目的地環境的多專案方案的快速簡便方式。 如果您需要重複將方案部署至多個目的地環境，您可以建立多個指令檔。 在每個命令檔，MSBuild 命令會建立相同的通用專案檔中，但它會指定不同的環境特定專案檔。 例如，要發行到開發人員或測試環境的命令檔可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


若要發行到預備環境的命令檔可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> 如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


您也可以自訂每個環境的建置程序來覆寫屬性，或在 MSBuild 命令中設定各種其他參數。 如需詳細資訊，請參閱[MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。

>[!div class="step-by-step"]
[上一頁](deploying-database-projects.md)
[下一頁](manually-installing-web-packages.md)
