---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: 建立組建定義支援部署 |Microsoft Docs
author: jrjlee
description: 如果您想要執行任何一種建置 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案內的組建定義。 此主題 des...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 18f88cff032bd0694ef98f0b19849f0edf1681ee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827384"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>建立組建定義支援的部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 如果您想要執行任何一種建置 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案內的組建定義。 本主題描述如何在 TFS 中建立新的組建定義，以及如何控制建置程序，在 Team Build 中的 web 部署。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置和部署流程控制的兩個專案檔&#x2014;其中一個包含套用至每個目的地環境，以及包含環境特定建置和部署設定的組建指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

組建定義是可控制如何及何時組建就會發生在 TFS 中的 team 專案的機制。 指定每個組建定義：

- 您想要建置，Visual Studio 方案檔或自訂的 Microsoft Build Engine (MSBuild) 專案檔等項目。
- 用來決定當組建應採取的準則將放置位置，例如手動觸發程序，持續整合 (CI)，或閘道簽入。
- 要在 Team Build 應該傳送組建輸出，包括部署的成品，如 web 封裝和資料庫指令碼的位置。
- 應該保留每個組建的時間量。
- 建置程序的其他各種參數。

> [!NOTE]
> 如需有關組建定義的詳細資訊，請參閱[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。


本主題將說明如何建立使用 CI 組建定義，以便開發人員簽入新的內容時，會觸發組建。 如果建置成功時，組建服務就會執行自訂的專案檔將方案部署到測試環境。

當您觸發組建時，就需要進行這些動作：

- 首先，Team Build 應該建置方案。 此程序的一部分，Team Build 會叫用 Web 發行管線 (WPP) 來產生每個 web 應用程式專案，方案中的 web 部署套件。 Team Build 也會執行任何與方案相關聯的單元測試。
- 如果方案建置失敗，則 Team Build 應該採取任何進一步的動作。 單元測試失敗應該視為組建失敗。
- 如果方案建置成功時，Team Build 應該執行自訂的專案檔，以控制部署的解決方案。 此程序的一部分，Team Build 會叫用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy)，目的地 web 伺服器上安裝的已封裝的 web 應用程式，並叫用的 VSDBCMD.exe 公用程式來執行建立資料庫在目的地資料庫伺服器上的指令碼。

這會說明此程序：

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案包含自訂的 MSBuild 專案檔， *Publish.proj*，您可以從 MSBuild 或 Team Build 中執行。 中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，此專案檔中定義的邏輯，將您的網頁套件和資料庫部署到目標環境。 此檔案包含省略建置和封裝程序，在 Team Build，讓部署工作來執行中執行的邏輯。 這是因為當您自動執行部署，如此一來，您通常會想要確保成功建置方案，並會開始部署程序之前，傳遞任何單元測試。

下節將說明如何實作此程序建立新的組建定義。

> [!NOTE]
> 此程序&#x2014;中的單一的自動程序會建置、 測試，以及部署方案&#x2014;可能是最適合部署至測試環境。 針對預備和生產環境很更多可能會想要從先前的組建時，您已驗證，並驗證測試環境中部署內容。 這種方法在下一個主題中，所述[部署特定建置](deploying-a-specific-build.md)。


### <a name="who-performs-this-procedure"></a>對於執行此程序？

一般而言，TFS 系統管理員會執行此程序。 在某些情況下，開發人員小組領導者可能需要在 TFS 中的 team 專案集合的責任。 若要建立新的組建定義，您必須是隸屬**Project Collection Build Administrators**群組的 team 專案集合，其中包含您的解決方案。

## <a name="create-a-build-definition-for-ci-and-deployment"></a>建立組建定義 CI 組建和部署

下一個程序描述如何建立 CI 觸發的組建定義。 如果建置成功，解決方案會部署自訂的 MSBuild 專案檔中使用的邏輯。

**若要建立的組建定義 CI 組建和部署**

1. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下**新增組建定義**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. 在 **一般**索引標籤上，指定組建定義的名稱 (例如**DeployToTest**) 和選擇性描述。
3. 在 **觸發程序**索引標籤上，選取您要觸發新組建的準則。 例如，如果您想要建置方案，並部署到測試環境中，每次簽入新的程式碼的開發人員，請選取**持續整合**。
4. 在 **組建預設值**索引標籤**組建輸出複製到下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如 **\\TFSBUILD\Drops**)。

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > 這個置放位置會儲存數個組建，根據您設定的保留原則。 當您想要發行至預備或生產環境，從特定組建部署成品時，這是您可以找到它們。
5. 在上**程序**索引標籤中，於**建置流程檔**下拉式清單中，保持**DefaultTemplate.xaml**選取。 這是其中一個預設建置流程範本加入至所有新的 team 專案。
6. 在 [**建置流程參數**資料表中，按一下**要建置的項目**資料列，然後再按一下**省略符號**] 按鈕。

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. 在 [**項目，以建置**] 對話方塊中，按一下**新增**。
8. 瀏覽至您的方案檔案的位置，然後按一下**確定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. 在 [**項目，以建置**] 對話方塊中，按一下**新增**。
10. 在 **類型的項目**下拉式清單中選取**MSBuild 專案檔**。
11. 瀏覽至自訂的專案檔案與您控制部署程序、 選取檔案，然後按一下位置**確定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **項目，以建置**對話方塊現在應該會顯示兩個項目。 按一下 [確定 **Deploying Office Solutions**]。

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. 在上**程序**索引標籤**建置流程參數**資料表中，展開**進階**一節。
14. 在  **MSBuild 引數**資料列中，新增任何 MSBuild 命令列引數，*任一*的建置項目需要。 在連絡管理員解決方案案例中，這些引數則是必要項目：

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. 在這個範例中：

    1. **DeployOnBuild = true**並**DeployTarget = 封裝**引數是必要的當您建置連絡管理員解決方案。 這會指示 MSBuild 來建立 web 部署套件之後建置每個 web 應用程式專案中所述[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。
    2. **TargetEnvPropsFile**引數是必要的當您建置*Publish.proj*檔案。 這個屬性會指出環境特定組態檔的位置中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。
16. 在 **保留原則**索引標籤上，設定多少組建的每個您想要保留所需的類型。
17. 按一下 [儲存] 。

## <a name="queue-a-build"></a>將組建排入佇列

此時，您已建立至少一個新的組建定義。 您所定義的建置程序現在會根據您在組建定義中指定的觸發程序執行。

如果您已設定使用 CI 組建定義，您可以測試您的組建定義兩種方式：

- 簽入至 team 專案，以自動組建觸發程序的一些內容。
- 組建排入佇列以手動方式。

**若要手動將組建排入佇列**

1. 在 [ **Team Explorer** ] 視窗中，組建定義，以滑鼠右鍵按一下，然後按一下**佇列新組建**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. 在 [**組建排入佇列**] 對話方塊中，檢閱組建屬性，然後按一下**佇列**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

若要檢閱進度與結果的組建&#x2014;不論它已觸發手動或自動&#x2014;連按兩下中的組建定義**Team Explorer**視窗。 這會開啟**Build 總管** 索引標籤。

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

從這裡開始，您可以疑難排解失敗的組建。 如果您按兩下個別組建時，您可以檢視摘要資訊，並逐一點選至詳細的記錄檔。

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

您可以使用這項資訊來疑難排解失敗的組建，並解決任何問題，才能嘗試進行另一個組建。

> [!NOTE]
> 可能失敗，直到您已在目的地環境中所需的任何權限授與組建伺服器時，會執行部署邏輯的組建。 如需詳細資訊，請參閱 <<c0> [ 設定小組組建部署的權限](configuring-permissions-for-team-build-deployment.md)。


## <a name="monitor-the-build-process"></a>監視組建程序

TFS 提供廣泛的功能，可協助您監視組建程序。 例如，TFS 可以傳送電子郵件或組建完成時，在工作列通知區域中顯示的警示。 如需詳細資訊，請參閱 <<c0> [ 執行和監視組建](https://msdn.microsoft.com/library/ms181721.aspx)。

## <a name="conclusion"></a>結論

本主題說明如何在 TFS 中建立組建定義。 組建定義被設定 CI，以便在建置程序執行每當開發人員簽入至 team 專案的內容。 組建定義執行自訂的 MSBuild 專案檔將網頁套件和資料庫指令碼部署到目標伺服器環境。

為了讓自動化的部署，才能成功建置程序的一部分，您必須授與組建服務帳戶在目標 web 伺服器和目標資料庫伺服器上的適當權限。 本教學課程的最後一個主題[設定小組組建部署的權限](configuring-permissions-for-team-build-deployment.md)，說明如何識別及設定自動化的部署，從 Team Build 的伺服器所需的權限。

## <a name="further-reading"></a>進一步閱讀

如需有關如何建立組建定義的詳細資訊，請參閱 <<c0> [ 建立基本組建定義](https://msdn.microsoft.com/library/ms181716.aspx)並[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。 如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/library/ms181722.aspx)。

> [!div class="step-by-step"]
> [上一頁](configuring-a-tfs-build-server-for-web-deployment.md)
> [下一頁](deploying-a-specific-build.md)
