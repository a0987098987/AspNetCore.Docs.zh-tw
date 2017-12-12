---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "在 TFS 中建立 Team 專案 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何建立新的 team 專案的 Team Foundation Server (TFS) 2010年中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 4cb0d72330086ecb8cd9e6fb70ce0a57698dda5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-team-project-in-tfs"></a>在 TFS 中建立 Team 專案
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何建立新的 team 專案的 Team Foundation Server (TFS) 2010年中。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

## <a name="task-overview"></a>工作概觀

若要佈建，並在 TFS 中使用新的 team 專案，您將需要完成下列高階步驟：

- 將建立新的 team 專案的使用者，授與權限。
- 建立 team 專案。
- 權限授與將在專案運作的小組成員。
- 檢查某些內容中。

本主題將示範如何執行這些程序，以及使用者和工作角色可能會負責將每個程序，它會識別。 請注意，根據您的組織結構，這些工作可能是另一個人的責任。

工作與本主題中的逐步解說假設您已安裝並設定 TFS，及您是否已建立 team 專案集合設定程序的一部分。 如需有關這些假設，以及更一般的案例上的背景資訊，請參閱[設定 TFS 組建伺服器進行 Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)。

## <a name="grant-permissions-to-the-team-project-creator"></a>權限授與 Team 專案建立者

若要建立新的 team 專案，您會需要這些權限：

- 您必須擁有**建立新的專案**TFS 應用程式層上的權限。 您通常將授與此權限將使用者新增至**Project Collection Administrators** TFS 群組。 **Team Foundation Administrators**全域群組也包含此權限。
- 您必須建立新的小組網站 SharePoint 網站集合對應至 TFS 的 team 專案集合中的權限。 您通常將授與此權限新增至與 SharePoint 群組的使用者**完全控制**權限在 SharePoint 網站集合。
- 如果您使用 SQL Server Reporting Services 功能，您必須是屬於**Team Foundation 內容管理員**Reporting Services 中的角色。

### <a name="who-performs-these-procedures"></a>誰會執行這些程序？

一般而言，個人或群組管理 TFS 部署也會執行這些程序。

因為這是高特殊權限的權限集時，新的 team 專案是通常由一小部分的使用者建立負責管理 TFS 部署。 開發人員不通常會授與建立新的 team 專案所需的權限。

### <a name="grant-permissions-in-tfs"></a>在 TFS 中授與權限

如果您想要讓使用者建立新的 team 專案，第一個的高層級工作是將使用者加入**Project Collection Administrators**群組針對 team 專案集合。

**若要將使用者加入 Project Collection Administrators 群組**

1. 在 TFS 伺服器上，在**啟動**功能表上，指向**所有程式**，按一下  **Microsoft Team Foundation Server 2010**，然後按一下  **Team Foundation管理主控台**。
2. 在導覽樹狀檢視中，依序展開**應用程式層**，然後按一下  **Team 專案集合**。

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. 在**Team 專案集合**窗格中，選取 team 專案的集合，您想要管理。

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. 在**一般**索引標籤上，按一下 **群組成員資格**。

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. 在**全域群組**對話方塊中，選取**Project Collection Administrators**群組，然後再按一下**屬性**。
6. 在**Team Foundation Server 群組屬性**對話方塊中，選取**Windows 使用者或群組**，然後按一下 **新增**。

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. 在**選取使用者、 電腦或群組**] 對話方塊中，輸入您想要能夠建立新的 team 專案的使用者的使用者名稱按一下**檢查名稱**，然後按一下 [**確定**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. 在**Team Foundation Server 群組屬性**對話方塊中，按一下 **確定**。
9. 在**全域群組**對話方塊中，按一下 **關閉**。

### <a name="grant-permissions-in-sharepoint-services"></a>授與 SharePoint Services 中的權限

接下來，您必須提供對應至 TFS team 專案集合的 SharePoint 網站集合中建立新的小組網站的使用者權限。

**若要授與 SharePoint 網站集合的完整控制權限**

1. 在 Team Foundation Server 管理主控台上**Team 專案集合**頁面上，選取您想要管理 team 專案集合。
2. 在**SharePoint 網站**索引標籤上，記下的值**目前預設網站位置**URL。

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. 開啟 Internet Explorer，然後移至您在步驟 2 中記下 URL。

    > [!NOTE]
    > 如果您不登入 windows 的使用者身分建立 team 專案集合，您必須以登入 SharePoint 這位使用者才能繼續。
4. 在**網站動作**功能表上，按一下 **站台設定**。

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. 在**站台設定**頁面的 **使用者和權限**，按一下 **人員與群組**。
6. 在左側的導覽面板中，按一下 **群組**。

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. 在**人員與群組： 所有群組**頁面上，按一下**設定組成群組為此站台**。

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > 您可能會收到**HTTP 404 找不到**錯誤，因為有雙重 HTTP 編碼錯誤。 如果發生這種情況，請與這個取代 URL:   
    > [*網站集合 URL*] /\_layouts/permsetup.aspx  
    > 例如:   
    > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. 上**設定組成群組為此站台**頁面上，新增的使用者，將建立 team 專案，**擁有者**群組，然後再按一下**確定**。

    ![](creating-a-team-project-in-tfs/_static/image10.png)

如需有關如何讓使用者可以建立 team 專案集合中的新 team 專案的詳細資訊，請參閱[設定 Team 專案集合的系統管理員權限](https://msdn.microsoft.com/en-us/library/dd547204.aspx)。

## <a name="create-a-new-team-project-and-add-users"></a>建立新的 Team 專案並新增使用者

一旦您擁有必要的權限，您可以使用**Team Explorer**建立新的 team 專案的 Visual Studio 2010 中的視窗。 此方法會提供精靈，收集所有必要的資訊並執行 TFS、 SharePoint 和 SQL Server Reporting Services 中的所需的工作。 您還需要在新的 team 專案上的權限授與小組的成員開發人員，以便讓使用者加入和修改內容。

### <a name="who-performs-these-procedures"></a>誰會執行這些程序？

通常是 TFS 系統管理員或開發人員小組領導者會執行這些程序。

### <a name="create-a-new-team-project"></a>建立新的 Team 專案

下一個程序描述如何在 TFS 2010 中建立新的 team 專案。

**若要建立新的 team 專案**

1. 在**啟動**功能表上，指向**所有程式**，按一下  **Microsoft Visual Studio 2010**，以滑鼠右鍵按一下**Microsoft Visual Studio 2010**，然後按一下 **系統管理員身分執行**。

    > [!NOTE]
    > 如果您不要以系統管理員身分執行 Visual Studio 2010，新的 Team 專案精靈就會失敗的最後一個步驟。
2. 如果**使用者帳戶控制**對話方塊出現時，按一下**是**。
3. 在 Visual Studio 中，在**小組**功能表上，按一下 **連接到 Team Foundation Server**。

    > [!NOTE]
    > 如果您已經設定 TFS 伺服器的連接，您可以省略步驟 4-7。
4. 在**連接到 Team 專案**對話方塊中，按一下 **伺服器**。
5. 在**新增/移除 Team Foundation Server**對話方塊中，按一下 **新增**。
6. 在**加入 Team Foundation Server**對話方塊中，提供您的 TFS 執行個體的詳細資料，然後按**確定**。

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. 在**新增/移除 Team Foundation Server**對話方塊中，按一下 **關閉**。
8. 在**連接到 Team 專案**對話方塊中選取的 TFS 執行個體，您想要連接，請選取 team 專案的集合要加入，然後按一下 **連接**。

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. 在**Team Explorer**視窗中，以滑鼠右鍵按一下 team 專案集合，然後按一下 **新的 Team 專案**。

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. 在**新的 Team 專案**對話方塊中，提供的名稱和描述，以 team 專案，然後再按一下**下一步**。

    > [!NOTE]
    > 如果您的 team 專案包含空格，您可能會進行到使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 的輸出路徑從部署封裝時所面對一些問題。 在路徑中的空間可讓更加難以執行 Web Deploy 命令。

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. 在**選取流程範本**頁面上，選取您要用來管理開發程序，然後按一下的流程範本**下一步**。

    > [!NOTE]
    > Tfs 流程範本的資訊，請參閱[流程範本和工具](https://msdn.microsoft.com/en-us/vstudio/aa718795)。
12. 在**小組網站設定**頁面上，保留不變的預設設定，然後按一下**下一步**。
13. 此設定建立或識別，SharePoint 小組網站與 TFS team 專案相關聯。 您的開發小組可以使用此站台來管理文件、 參與討論區，建立 wiki 頁面以及執行各項程式碼無關的其他工作。 如需詳細資訊，請參閱[SharePoint 產品之間互動和 Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx)。
14. 在**指定原始檔控制設定**頁面上，保留不變的預設設定，然後按一下**下一步**。
15. 這項設定識別或 TFS 資料夾階層，做為您內容的根資料夾中建立的位置。
16. 在**確認 Team 專案設定**頁面上，按一下**完成**。
17. 新的 team 專案已成功建立時，在**建立 Team 專案**頁面上，按一下**關閉**。

### <a name="add-users-to-a-team-project"></a>將使用者加入至 Team 專案

既然您已建立新的 team 專案，您可以授與權限給使用者，讓它們開始新增及需要共同作業的內容。

**將使用者加入至 team 專案**

1. 在 Visual Studio 2010 中，在**Team Explorer**視窗中，以滑鼠右鍵按一下 team 專案，指向**Team 專案設定**，然後按一下 **群組成員資格**。

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. 若要讓使用者以新增、 修改及移除原始檔控制下的程式碼，您需要將他或她**參與者**群組。
3. 在**專案群組**對話方塊中，選取**參與者**群組，然後再按一下**屬性**。

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. 在**Team Foundation Server 群組屬性**對話方塊中，選取**Windows 使用者或群組**，然後按一下 **新增**。

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. 在**選取使用者、 電腦或群組**] 對話方塊中，輸入您想要加入至 team 專案中，使用者的使用者名稱按一下**檢查名稱**，然後按一下 [**確定**。

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. 在**Team Foundation Server 群組屬性**對話方塊中，按一下 **確定**。
7. 在**專案群組**對話方塊中，按一下 **關閉**。

## <a name="conclusion"></a>結論

此時，新的 team 專案是可供使用，並開發團隊可以開始加入的內容和開發程序的共同作業。

下一個主題[將內容加入至原始檔控制](adding-content-to-source-control.md)，說明如何將內容加入至原始檔控制。

## <a name="further-reading"></a>進一步閱讀

如需更廣泛指引在 TFS 中建立 team 專案的詳細資訊，請參閱[建立 Team 專案](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx)。 如需有關如何讓使用者可以建立 team 專案集合中的新 team 專案的詳細資訊，請參閱[設定 Team 專案集合的系統管理員權限](https://msdn.microsoft.com/en-us/library/dd547204.aspx)。 如需有關如何將使用者加入至 team 專案的詳細資訊，請參閱[將使用者加入 Team 專案](https://msdn.microsoft.com/en-us/library/bb558971.aspx)。

>[!div class="step-by-step"]
[上一頁](configuring-team-foundation-server-for-web-deployment.md)
[下一頁](adding-content-to-source-control.md)
