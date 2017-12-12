---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: "建立組建定義支援部署 |Microsoft 文件"
author: jrjlee
description: "如果您想要執行任何種類的組建 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案中的組建定義。 此主題 des..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c2e7a768c2cf9900731b822ec187093a4b250ead
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-build-definition-that-supports-deployment"></a>建立組建定義支援的部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 如果您想要執行任何種類的組建 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案中的組建定義。 本主題描述如何在 TFS 中建立新的組建定義，以及如何控制 web 部署在 Team Build 中建置程序的一部分。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，請在它的組建和部署程序由兩個專案檔 & #x 2014年; one 包含組建指示適用於每個目的地環境中和包含特定環境的建置和部署設定。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

組建定義會是控制如何及何時在 TFS 中的 team 專案發生組建的機制。 每個組建定義會指定：

- 您想要建置時，Visual Studio 方案檔或自訂的 Microsoft Build Engine (MSBuild) 專案檔等動作。
- 判斷應該執行組建時的準則將，像是手動觸發程序，持續整合 (CI)，或執行閘道簽入。
- Team Build 應該傳送到這個組建輸出，包括部署的成品，如 web 封裝和資料庫的指令碼位置。
- 每個組建應保留的時間量。
- 各種其他建置流程參數。

> [!NOTE]
> 如需組建定義的詳細資訊，請參閱[定義建置流程](https://msdn.microsoft.com/en-us/library/ms181715.aspx)。


本主題將說明如何建立組建定義使用 CI，如此當開發人員簽入新的內容時，會觸發建置。 如果建置成功，則組建服務執行自訂專案檔，將方案部署到測試環境。

當您設定觸發程序建置時，這些動作需要發生：

- 首先，Team Build 應該建置方案。 此程序的一部分，Team Build 會叫用 Web 發行管線 (WPP) 來產生每個 web 應用程式專案，在方案中的 web 部署套件。 Team Build 也會執行與解決方案相關聯的任何單元測試。
- 如果方案建置失敗，則 Team Build 應該採取任何進一步的動作。 單元測試失敗視為組建失敗。
- 如果方案建置成功，則 Team Build 應該執行控制方案的部署自訂的專案檔。 此程序中 Team Build 將會叫用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 在目的地 web 伺服器上安裝封裝的 web 應用程式，它將會叫用 VSDBCMD.exe 執行公用程式建立資料庫目的地資料庫伺服器上的指令碼。

下列說明處理程序：

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案包含自訂的 MSBuild 專案檔， *Publish.proj*，您可以執行的 MSBuild，或 Team Build。 中所述[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，此專案檔會定義將您的 web 封裝和資料庫部署到目標環境的邏輯。 檔案包含邏輯，省略建置和封裝程序，如果它正在 Team Build 離開部署工作來執行。 這是因為當您自動化部署，如此一來，您通常會想要確保成功建置方案，而會開始部署程序之前，傳遞任何單元測試。

下一節說明如何實作這個程序，藉由建立新的組建定義。

> [!NOTE]
> 此程序 & #x 2014; 單一的自動化程序會建立在其中，測試，並部署方案 & #x 2014年; 可能是最適合部署到測試環境。 針對預備與生產環境您很可能想將內容從某一個組建，您已驗證並驗證測試環境中部署。 這種方法在下一個主題中，說明[部署特定建置](deploying-a-specific-build.md)。


### <a name="who-performs-this-procedure"></a>誰會執行此程序？

通常，TFS 系統管理員會執行此程序。 在某些情況下，開發人員小組領導者會可能需要在 TFS 中的 team 專案集合的責任。 若要建立新的組建定義，您必須是屬於**Project Collection Build Administrators**群組 team 專案集合，其中包含您的方案。

## <a name="create-a-build-definition-for-ci-and-deployment"></a>建立組建定義 CI 和部署

下一個程序描述如何建立 CI 觸發的組建定義。 如果建置成功，會將解決方案部署自訂的 MSBuild 專案檔中使用的邏輯。

**若要建立組建定義 CI 和部署**

1. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下 **新增組建定義**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. 在**一般**索引標籤上，為指定的組建定義名稱 (例如， **DeployToTest**) 和選擇性描述。
3. 在**觸發程序**索引標籤上，選取您要觸發新組建的準則。 例如，如果您想要建置方案，每次簽入新的程式碼的開發人員部署到測試環境，選取**連續整合**。
4. 在**組建預設值**索引標籤的**複製組建輸出複製至下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如，  **\\TFSBUILD\Drops**)。

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > 這個置放位置會儲存數個組建，視您設定保留原則而定。 當您想要發行至預備或生產環境的部署成品從特定的組建時，這是您會發現它們。
5. 在**程序**索引標籤的**建置流程檔**下拉式清單中，保留**DefaultTemplate.xaml**選取。 這是加入到所有新的 team 專案的預設建置流程範本。
6. 在**建置流程參數**資料表中，按一下 [在**要建置的項目**資料列，然後再按一下**省略**] 按鈕。

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. 在**要建置的項目**對話方塊中，按一下 **新增**。
8. 瀏覽至您的方案檔的位置，然後按一下**確定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. 在**要建置的項目**對話方塊中，按一下 **新增**。
10. 在**類型的項目**下拉式清單中選取**MSBuild 專案檔**。
11. 檔案位置的自訂專案與您控制的部署程序，選取檔案，然後按一下瀏覽**確定**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **要建置的項目**對話方塊現在應該會顯示兩個項目。 按一下 [確定]。

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. 在**程序**索引標籤的**建置流程參數**資料表中，展開 **進階**> 一節。
14. 在**MSBuild 引數**資料列中，將任何 MSBuild 命令列引數，*任一*的項目來建立要求。 在連絡人管理員解決方案案例中，這些引數則是必要項目：

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. 在這個範例中：

    1. **DeployOnBuild = true**和**DeployTarget = 封裝**引數是必要的當您建置連絡人管理員解決方案。 這會指示 MSBuild 來建立 web 部署封裝之後建立的每個 web 應用程式專案中所述[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。
    2. **TargetEnvPropsFile**引數是必要的當您建置*Publish.proj*檔案。 中所述，此屬性會指出環境特定組態檔的位置，[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。
16. 在**保留原則**索引標籤上，設定每個型別的您想要保留視需要多少組建。
17. 按一下 [儲存] 。

## <a name="queue-a-build"></a>將組建排入佇列

此時，您已建立至少一個新的組建定義。 您所定義的建置程序現在將會根據您指定的組建定義中的觸發程序執行。

如果您已設定使用 CI 組建定義，您可以測試您的組建定義兩種方式：

- 簽入至 team 專案，以觸發自動的建置某些內容。
- 組建排入佇列以手動方式。

**若要手動將組建排入佇列**

1. 在**Team Explorer**視窗中，以滑鼠右鍵按一下組建定義，然後**佇列新組建**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. 在**佇列組建**對話方塊中，檢閱建置屬性，然後按一下**佇列**。

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

若要檢閱的進度和結果的組建 & #x 2014年; 不論它是否已觸發手動或自動 & #x 2014; 按兩下組建定義中的**Team Explorer**視窗。 這會開啟**Build 總管** 索引標籤。

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

從這裡，您可以疑難排解組建失敗。 如果您按兩下個別的組建，您可以檢視摘要資訊，然後按一下詳細的記錄檔。

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

您可以使用這項資訊來疑難排解失敗的組建，並解決任何問題，然後再嘗試另一個組建。

> [!NOTE]
> 可能失敗，直到您已在目的地環境中需要任何權限授與組建伺服器時，會執行部署邏輯的組建。 如需詳細資訊，請參閱[小組建置部署設定權限](configuring-permissions-for-team-build-deployment.md)。


## <a name="monitor-the-build-process"></a>監視建置程序

TFS 提供廣泛的功能，可協助您監視在建置程序。 例如，TFS 可以傳送電子郵件或完成組建後，工作列通知區域中顯示的警示。 如需詳細資訊，請參閱[執行和監視組建](https://msdn.microsoft.com/en-us/library/ms181721.aspx)。

## <a name="conclusion"></a>結論

本主題描述如何在 TFS 中建立的組建定義。 組建定義設定 CI，所以每當開發人員簽入至 team 專案的內容，在建置程序執行。 組建定義執行自訂的 MSBuild 專案檔至目標伺服器環境中部署 web 封裝和資料庫的指令碼。

為了讓自動化部署才能成功建置程序的一部分，您必須授與組建服務帳戶在目標 web 伺服器和目標資料庫伺服器上的適當權限。 本教學課程的最後一個主題[小組建置部署設定權限](configuring-permissions-for-team-build-deployment.md)，說明如何識別及設定從 Team Build 伺服器的自動部署所需的權限。

## <a name="further-reading"></a>進一步閱讀

如需有關如何建立組建定義的詳細資訊，請參閱[建立基本組建定義](https://msdn.microsoft.com/en-us/library/ms181716.aspx)和[定義建置流程](https://msdn.microsoft.com/en-us/library/ms181715.aspx)。 如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/en-us/library/ms181722.aspx)。

>[!div class="step-by-step"]
[上一頁](configuring-a-tfs-build-server-for-web-deployment.md)
[下一頁](deploying-a-specific-build.md)
