---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: 部署特定的組建 |Microsoft Docs
author: jrjlee
description: 本主題說明如何部署 web 封裝和資料庫指令碼從特定的前一個組建至新的目的地，例如預備或生產環境的流程圖...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 312cd6f42463722b76996a3a848102946e0ca265
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832464"
---
<a name="deploying-a-specific-build"></a>部署特定的組建
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何部署 web 封裝和資料庫指令碼至新的目的地，像是預備或生產環境特定的前一個組建。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置和部署流程控制的兩個專案檔&#x2014;其中一個包含套用至每個目的地環境，以及包含環境特定建置和部署設定的組建指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

到目前為止，本教學課程組中的主題著重於如何建置、 封裝和部署 web 應用程式和資料庫，在單一步驟或自動化程序。 不過，在某些常見的情況下，您會想從組建置放資料夾中的清單中選取您所部署的資源。 換句話說，最新的組建可能不想要部署的組建。

持續整合 (CI) 所述的案例中先前的主題，請考慮[建立組建定義，支援部署](creating-a-build-definition-that-supports-deployment.md)。 您已建立組建定義 Team Foundation Server (TFS) 2010年中。 每當開發人員會將程式碼簽入 TFS，Team Build 會建置您的程式碼、 建置程序的一部分建立 web 封裝和資料庫指令碼、 執行任何單元測試，和將您的資源部署到測試環境。 根據您建立組建定義時所設定的保留原則，TFS 將會保留先前的組建數目。

![](deploying-a-specific-build/_static/image1.png)

現在，假設您已先執行驗證和驗證測試，針對其中一種建置在測試環境中，且您已經準備好應用程式部署至預備環境。 在此同時，可能會有開發人員簽入新的程式碼。 您不想要重新建置方案並部署至預備環境，以及您不想要將最新的組建部署至預備環境。 相反地，您會想要部署特定的組建，您已驗證，並在測試伺服器上驗證。

若要達成此目的，您需要告知 Microsoft Build Engine (MSBuild) 來尋找特定的組建產生資料庫指令碼與 web 封裝的位置。

## <a name="overriding-the-outputroot-property"></a>覆寫 OutputRoot 屬性

在 [範例方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)，則*Publish.proj*檔案會宣告一個名為屬性**OutputRoot**。 顧名思義，這是根資料夾，其中包含所有建置程序會產生的項目。 在  *Publish.proj*檔案，您所見， **OutputRoot**屬性會參考所有的部署資源的根位置。

> [!NOTE]
> **OutputRoot**是常用的屬性名稱。 Visual C# 和 Visual Basic 專案檔也會宣告此屬性，將所有組建輸出的根位置。


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


如果您想要部署網頁套件和資料庫從不同位置的指令碼專案檔&#x2014;像先前的 TFS 組建的輸出&#x2014;您只需要覆寫**OutputRoot**屬性。 您應該設定的相關組建資料夾的屬性值，在 Team Build 的伺服器。 如果您已從命令列執行 MSBuild，您可以指定的值**OutputRoot**做為命令列引數：


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


在實務上，不過，您也想要跳過**建置**目標&#x2014;沒有任何點中建置您的解決方案，如果您不打算使用組建輸出。 您可以指定您想要從命令列執行的目標來執行這項操作：


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


不過，在大部分情況下，您會想要在 TFS 組建定義中建立您的部署邏輯。 這可讓使用者使用**組建排入佇列**任何 Visual Studio 安裝，以連線至 TFS 伺服器的部署觸發程序的權限。

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>建立組建定義，以便部署特定組建

下一個程序描述如何建立組建定義，可讓使用者觸發程序部署至預備環境使用單一命令。

在此情況下，您不想要實際建置任何的組建定義&#x2014;您只想要執行自訂的專案檔中的部署邏輯。 *Publish.proj*檔案包含條件式邏輯，會略過**建置**目標，如果檔案在 Team Build 中執行。 其做法是內建**BuildingInTeamBuild**屬性，它會自動設為 **，則為 true**如果您在 Team Build 中執行您的專案檔。 如此一來，您可以略過建置程序，並只需執行專案檔來部署現有的組建。

**若要建立組建定義，若要手動觸發部署**

1. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下**新增組建定義**。

    ![](deploying-a-specific-build/_static/image2.png)
2. 在 **一般**索引標籤上，指定組建定義的名稱 (例如**DeployToStaging**) 和選擇性描述。
3. 在 **觸發程序**索引標籤上，選取**手動-簽入不會觸發新組建**。
4. 在 **組建預設值**索引標籤**組建輸出複製到下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如 **\\TFSBUILD\Drops**)。

    ![](deploying-a-specific-build/_static/image3.png)
5. 在上**程序**索引標籤中，於**建置流程檔**下拉式清單中，保持**DefaultTemplate.xaml**選取。 這是其中一個預設建置流程範本加入至所有新的 team 專案。
6. 在 [**建置流程參數**資料表中，按一下**要建置的項目**資料列，然後再按一下**省略符號**] 按鈕。

    ![](deploying-a-specific-build/_static/image4.png)
7. 在 [**項目，以建置**] 對話方塊中，按一下**新增**。
8. 在 **類型的項目**下拉式清單中選取**MSBuild 專案檔**。
9. 瀏覽至自訂的專案檔案與您控制部署程序、 選取檔案，然後按一下位置**確定**。

    ![](deploying-a-specific-build/_static/image5.png)
10. 在 [**要建置的項目**] 對話方塊中，按一下**確定**。
11. 在 **建置流程參數**資料表中，展開**進階**一節。
12. 在  **MSBuild 引數**資料列，指定您的環境特定專案檔的位置並將您的組建資料夾位置的預留位置：

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > 您必須覆寫**OutputRoot**值每次組建排入佇列。 這會涵蓋在下一個程序。
13. 按一下 [儲存] 。

當您觸發組建時，您需要更新**OutputRoot**屬性以指向您要部署的組建。

**若要部署的組建定義從特定的組建**

1. 在 [ **Team Explorer** ] 視窗中，組建定義，以滑鼠右鍵按一下，然後按一下**佇列新組建**。

    ![](deploying-a-specific-build/_static/image7.png)
2. 在 **組建排入佇列**對話方塊的 **參數**索引標籤上，展開**進階**一節。
3. 在  **MSBuild 引數**列中的值取代**OutputRoot**屬性與您的組建資料夾的位置。 例如: 

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > 請務必包含結尾斜線結尾的組建資料夾的路徑。
4. 按一下 **佇列**。

當組建排入佇列，專案檔會將資料庫指令碼部署 web 封裝從組建置放資料夾中所指定**OutputRoot**屬性。

## <a name="conclusion"></a>結論

本主題描述如何發行部署的資源，例如 web 封裝和資料庫指令碼，從上一個特定組建中使用分割的專案檔的部署模型。 它說明如何覆寫**OutputRoot**屬性，以及如何部署邏輯併入一份 TFS 組建定義。

## <a name="further-reading"></a>進一步閱讀

如需有關如何建立組建定義的詳細資訊，請參閱 <<c0> [ 建立基本組建定義](https://msdn.microsoft.com/library/ms181716.aspx)並[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。 如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/library/ms181722.aspx)。

> [!div class="step-by-step"]
> [上一頁](creating-a-build-definition-that-supports-deployment.md)
> [下一頁](configuring-permissions-for-team-build-deployment.md)
