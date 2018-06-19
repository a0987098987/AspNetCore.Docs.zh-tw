---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: 將內容加入至原始檔控制 |Microsoft 文件
author: jrjlee
description: 本主題說明如何將內容加入至 Team Foundation Server (TFS) 2010年中的原始檔控制。 說明如何將方案和專案加入至小組 proje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890430"
---
<a name="adding-content-to-source-control"></a>將內容加入至原始檔控制
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明如何將內容加入至 Team Foundation Server (TFS) 2010年中的原始檔控制。 說明如何將方案和專案加入至 TFS 中 team 專案，並說明如何將組件架構等外部相依性加入至原始檔控制。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

## <a name="task-overview"></a>工作概觀

在大部分情況下，開發人員小組的每個成員都應該能夠將內容加入至原始檔控制。 若要將方案加入至原始檔控制在 TFS 中，您將需要完成下列高階步驟：

- 連接到 team 專案。
- 將 team 專案的資料夾結構，在伺服器上對應至本機電腦上的資料夾結構。
- 加入原始檔控制方案和其內容。
- 將任何外部的相依性加入至原始檔控制。

本主題將說明如何執行這些程序。

工作與本主題中的逐步解說假設，您已經建立新的 TFS team 專案來管理您的內容。 如需有關如何建立新的 team 專案的詳細資訊，請參閱[在 TFS 中建立 Team 專案](creating-a-team-project-in-tfs.md)。

### <a name="who-performs-these-procedures"></a>誰會執行這些程序？

在大部分情況下，開發人員小組的每個成員應該能夠加入和修改特定的 team 專案內的內容。

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>連接到 Team 專案，並建立資料夾對應

您加入至原始檔控制的任何內容之前，您需要連接到 team 專案，並在本機電腦上建立伺服器上的資料夾結構與檔案系統之間的對應。

**連接到 team 專案，並將對應的本機路徑**

1. 在您開發人員工作站上，開啟 Visual Studio 2010。
2. 在 Visual Studio 中，在**小組**功能表上，按一下 **連接到 Team Foundation Server**。

    > [!NOTE]
    > 如果您已經設定 TFS 伺服器的連接，您可以略過步驟 3-6。
3. 在**連接到 Team 專案**對話方塊中，按一下 **伺服器**。
4. 在**新增/移除 Team Foundation Server**對話方塊中，按一下 **新增**。
5. 在**加入 Team Foundation Server**對話方塊中，提供您的 TFS 執行個體的詳細資料，然後按**確定**。

    ![](adding-content-to-source-control/_static/image1.png)
6. 在**新增/移除 Team Foundation Server**對話方塊中，按一下 **關閉**。
7. 在**連接到 Team 專案**對話方塊中，選取您想要連接，請選取小組的 TFS 執行個體的專案集合中，選取您想要加入的 team 專案並再按**連接**。

    ![](adding-content-to-source-control/_static/image2.png)
8. 在**Team Explorer**視窗中，展開您的 team 專案，然後按兩下**原始檔控制**。

    ![](adding-content-to-source-control/_static/image3.png)
9. 在**原始檔控制總管**索引標籤上，按一下 **未對應**。

    ![](adding-content-to-source-control/_static/image4.png)
10. 在**對應**對話方塊中，於**本機資料夾**方塊中，瀏覽至 （或建立） 本機資料夾，以做為 team 專案的根資料夾，然後按一下 **對應**。

    ![](adding-content-to-source-control/_static/image5.png)
11. 當系統提示您下載來源檔案時，按一下 **是**。

    ![](adding-content-to-source-control/_static/image6.png)

此時，已針對 team 專案的伺服器端資料夾對應至開發人員工作站上的本機資料夾。 您也已經從 team 專案下載任何現有的內容，以您的本機資料夾結構。 您現在可以開始將您自己的內容加入至原始檔控制。

## <a name="add-projects-and-solutions-to-source-control"></a>加入原始檔控制專案和方案

若要加入至原始檔控制的專案和方案，您需要將在本機電腦上將它們移至 team 專案的對應資料夾。 您可以再簽入與伺服器同步處理您的新增項目內容。

**若要將專案加入原始檔控制**

1. 在您開發人員工作站上，將移至 team 專案對應的資料夾結構中的適當位置的專案和方案。

    > [!NOTE]
    > 許多組織必須如何專案和方案安排原始檔控制中的慣用的方法。 如需有關資料夾結構的指引，請參閱[How To： 結構您原始檔控制資料夾 Team Foundation Server 中](https://msdn.microsoft.com/library/bb668992.aspx)。
2. Visual Studio 2010 中開啟的方案。
3. 在**方案總管 中**視窗中，以滑鼠右鍵按一下方案，然後**將方案加入至原始檔控制**。

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > 在某些情況下，根據您的組織喜歡結構的內容在 TFS 中，您可能需要將專案加入原始檔控制，分別可提供更細微的控制，透過您的程式碼的組織方式。
4. 確認**原始檔控制總管**索引標籤會顯示您已新增 team 專案的伺服器資料夾結構內的內容。

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **原始檔控制總管**索引標籤會顯示您的內容不再提示您之後，因為您將您的解決方案新增至本機檔案系統上的對應資料夾。 如果您的方案位於未對應的位置，系統會提示您指定 TFS 和您的本機檔案系統中的資料夾位置。
5. 上**原始檔控制總管**索引標籤的**資料夾** 窗格中，以滑鼠右鍵按一下 team 專案 (比方說， **ContactManager**)，然後按一下 **簽入暫止的變更**。
6. 在**檢查 – 原始程式檔中**對話方塊中，輸入註解，，然後按一下**簽入**。

    ![](adding-content-to-source-control/_static/image9.png)

此時您已將您的方案加入原始檔控制在 TFS 中。

## <a name="add-external-dependencies-to-source-control"></a>加入原始檔控制中的外部相依性

當您將專案或方案加入原始檔控制時，也會新增任何檔案和資料夾，您的專案或方案中。 不過，在許多情況下，專案和方案也仰賴外部的相依性，例如本機的組件，才能正常。 您需要加入至原始檔控制，讓 Team Build 和開發人員小組的其他成員的任何此類資源已成功建置程式碼。

例如，連絡人管理員範例方案的資料夾結構包含名為封裝的資料夾。 這包含組件和支援的各種資源 ADO.NET Entity Framework 4.1。 [Packages] 資料夾不是連絡人管理員方案的一部分，但沒有它的方案會成功建立。 若要啟用 Team Build 以建置方案，您要加入原始檔控制中的 [packages] 資料夾。

> [!NOTE]
> 包含的封裝資料夾是典型的 Entity Framework 或類似的資源，加入您的方案使用 Visual Studio 2010 的 NuGet 延伸模組時，會發生什麼事。


**將非專案內容加入至原始檔控制**

1. 確定您要新增 （例如，[packages] 資料夾） 的項目位於您本機檔案系統上的對應資料夾內的適當位置。
2. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案，然後按兩下**原始檔控制**。

    ![](adding-content-to-source-control/_static/image10.png)
3. 在**原始檔控制總管**索引標籤的**資料夾**窗格中，選取想要新增的資料夾包含的項目或項目。
4. 按一下**將項目加入資料夾** 按鈕。

    ![](adding-content-to-source-control/_static/image11.png)
5. 在**加入原始檔控制**對話方塊方塊中，選取資料夾或項目您想要新增，然後再按**下一步**。

    ![](adding-content-to-source-control/_static/image12.png)
6. 在**排除項目**索引標籤上，選取任何必要的項目，已自動排除 （例如，組件），然後按一下**這些項目**。

    ![](adding-content-to-source-control/_static/image13.png)
7. 在**要加入項目**索引標籤上，確認您想要包含的所有檔案會列出，然後按一下**完成**。

    ![](adding-content-to-source-control/_static/image14.png)
8. 在**原始檔控制總管**視窗中，按一下 [**簽入**] 按鈕。

    ![](adding-content-to-source-control/_static/image15.png)
9. 在**檢查 – 原始程式檔中**對話方塊中，輸入註解，，然後按一下**簽入**。

此時，您已加入的外部相依性為您的方案加入原始檔控制。

## <a name="conclusion"></a>結論

本主題描述如何連接到 team 專案，將對應的資料夾結構，並將內容加入至原始檔控制。 如需有關如何使用原始檔控制下的項目詳細資訊，請參閱[使用版本控制](https://msdn.microsoft.com/library/ms181368.aspx)。

下一個主題[設定 TFS 組建伺服器的 Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)，說明如何準備的 TFS Team Build server 建置和部署您的方案。

## <a name="further-reading"></a>進一步閱讀

更廣泛使用在 TFS 中的原始檔控制的詳細資訊，請參閱[使用版本控制](https://msdn.microsoft.com/library/ms181368.aspx)。

> [!div class="step-by-step"]
> [上一頁](creating-a-team-project-in-tfs.md)
> [下一頁](configuring-a-tfs-build-server-for-web-deployment.md)
