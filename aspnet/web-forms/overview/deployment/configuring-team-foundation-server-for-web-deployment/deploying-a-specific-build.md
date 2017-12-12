---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "部署特定的組建 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何部署 web 封裝和資料庫指令碼從特定的前一個組建至新的目的地，像是預備或生產環境的流程圖..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a>部署特定的組建
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何部署 web 封裝和資料庫指令碼至新的目的地，像是預備或生產環境特定的前一個組建。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，請在它的組建和部署程序由兩個專案檔 & #x 2014年; one 包含組建指示適用於每個目的地環境中和包含特定環境的建置和部署設定。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

到目前為止，在此教學課程的集合中的主題有著重於如何建置、 封裝和部署 web 應用程式和資料庫做為單一步驟的一部分，或自動化程序。 不過，某些常見的情況下，您要從組建置放資料夾中的清單中選取您所部署的資源。 換句話說，最新的組建可能不是您想要部署的組建。

請考慮在先前的主題所述的持續整合 (CI) 案例[建立組建定義，支援部署](creating-a-build-definition-that-supports-deployment.md)。 您已建立組建定義 Team Foundation Server (TFS) 2010年中。 每當開發人員會將程式碼簽入 TFS，Team Build 會建置您的程式碼、 建立 web 封裝和資料庫的指令碼做為建置流程的一部分、 執行任何單元測試，和將您的資源部署到測試環境。 建立組建定義時所設定的保留原則，根據 TFS 將會保留特定數目的前一個組建。

![](deploying-a-specific-build/_static/image1.png)

現在，假設您已執行驗證和驗證對下列其中一種測試建置在測試環境中，您準備好應用程式部署至預備環境。 在此同時，開發人員可能已簽入新的程式碼。 您不想重建方案，並將部署到預備環境中，您不想要將最新組建部署到預備環境。 相反地，您會想要部署您已驗證，並在測試伺服器上驗證特定組建。

若要達成此目的，您需要告知 Microsoft Build Engine (MSBuild) 尋找的網頁套件和特定的組建產生資料庫指令碼的位置。

## <a name="overriding-the-outputroot-property"></a>覆寫 OutputRoot 屬性

在[範例方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*檔案宣告名為屬性**OutputRoot**。 顧名思義，這是根資料夾，其中包含建置流程所產生的所有項目。 在*Publish.proj*檔案，您所見， **OutputRoot**屬性參考到所有部署資源的根位置。

> [!NOTE]
> **OutputRoot**是常用的屬性名稱。 Visual C# 和 Visual Basic 專案檔也會宣告此屬性，將所有組建輸出的根位置。


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


如果您想專案檔來部署 web 封裝和資料庫指令碼從不同位置 & #x 2014年; 類似的輸出前一個 TFS 組建 & #x 2014; 您只需要覆寫**OutputRoot**屬性。 您應該設定的相關組建資料夾的屬性值 Team Build 的伺服器上。 如果您從命令列執行 MSBuild，您可以指定的值**OutputRoot**做為命令列引數：


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


在實務上，不過，您也想要略過**建置**目標 & #x 2014; 沒有點中建置您的方案，如果您不打算使用組建輸出。 您可以藉由指定您想要從命令列執行的目標來這麼做：


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


不過，在大部分情況下，您需要建置您的部署邏輯到 TFS 組建定義。 這可讓使用者與**組建排入佇列**觸發部署從任何 Visual Studio 安裝，以連線至 TFS 伺服器的權限。

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>建立組建定義，以便部署特定組建

下一個程序描述如何建立組建定義，可讓使用者使用單一命令的預備環境部署觸發程序。

在此情況下，您不想組建定義，以實際建置的任何項目 （& c) 2014年 #x; 您只想要自訂的專案檔中執行部署邏輯。 *Publish.proj*檔案包含條件式邏輯，會略過**建置**目標如果檔案在 Team Build 中執行。 它會透過評估內建**BuildingInTeamBuild**屬性，會自動設為**true**如果您在 Team Build 中執行您的專案檔。 如此一來，您可以略過建置程序，並執行現有的組建部署至專案檔即可。

**若要建立組建定義來手動觸發部署**

1. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下 **新增組建定義**。

    ![](deploying-a-specific-build/_static/image2.png)
2. 在**一般**索引標籤上，為指定的組建定義名稱 (例如， **DeployToStaging**) 和選擇性描述。
3. 在**觸發程序**索引標籤上，選取**手動-簽入不會觸發新組建**。
4. 在**組建預設值**索引標籤的**複製組建輸出複製至下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如，  **\\TFSBUILD\Drops**)。

    ![](deploying-a-specific-build/_static/image3.png)
5. 在**程序**索引標籤的**建置流程檔**下拉式清單中，保留**DefaultTemplate.xaml**選取。 這是加入到所有新的 team 專案的預設建置流程範本。
6. 在**建置流程參數**資料表中，按一下 [在**要建置的項目**資料列，然後再按一下**省略**] 按鈕。

    ![](deploying-a-specific-build/_static/image4.png)
7. 在**要建置的項目**對話方塊中，按一下 **新增**。
8. 在**類型的項目**下拉式清單中選取**MSBuild 專案檔**。
9. 檔案位置的自訂專案與您控制的部署程序，選取檔案，然後按一下瀏覽**確定**。

    ![](deploying-a-specific-build/_static/image5.png)
10. 在**要建置的項目**對話方塊中，按一下 **確定**。
11. 在**建置流程參數**資料表中，展開 **進階**> 一節。
12. 在**MSBuild 引數**資料列，指定您的環境特定專案檔的位置並將您的組建資料夾位置的預留位置：

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > 您必須覆寫**OutputRoot**值每次組建排入佇列。 這點會在下一個程序中說明。
13. 按一下 [儲存] 。

當您觸發組建時，您需要更新**OutputRoot**屬性以指向您要部署的組建。

**若要部署的組建定義從特定的組建**

1. 在**Team Explorer**視窗中，以滑鼠右鍵按一下組建定義，然後**佇列新組建**。

    ![](deploying-a-specific-build/_static/image7.png)
2. 在**佇列組建**對話方塊**參數**索引標籤上，依序展開**進階**> 一節。
3. 在**MSBuild 引數**資料列中，以取代值**OutputRoot**屬性與您的組建資料夾位置。 例如: 

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > 請務必包含結尾斜線結尾的組建資料夾的路徑。
4. 按一下**佇列**。

當組建排入佇列，專案檔會將資料庫指令碼部署 web 封裝從組建置放資料夾中指定**OutputRoot**屬性。

## <a name="conclusion"></a>結論

本主題描述如何發行部署資源，例如 web 封裝和資料庫的指令碼，從上一個特定建置使用分割專案檔的部署模型。 它說明如何覆寫**OutputRoot**屬性以及如何部署邏輯併入 TFS 組建定義。

## <a name="further-reading"></a>進一步閱讀

如需有關如何建立組建定義的詳細資訊，請參閱[建立基本組建定義](https://msdn.microsoft.com/en-us/library/ms181716.aspx)和[定義建置流程](https://msdn.microsoft.com/en-us/library/ms181715.aspx)。 如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/en-us/library/ms181722.aspx)。

>[!div class="step-by-step"]
[上一頁](creating-a-build-definition-that-supports-deployment.md)
[下一頁](configuring-permissions-for-team-build-deployment.md)
