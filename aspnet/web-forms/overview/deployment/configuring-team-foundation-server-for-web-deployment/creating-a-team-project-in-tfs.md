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
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="7c07e-103">在 TFS 中建立 Team 專案</span><span class="sxs-lookup"><span data-stu-id="7c07e-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="7c07e-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7c07e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7c07e-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="7c07e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7c07e-106">本主題描述如何建立新的 team 專案的 Team Foundation Server (TFS) 2010年中。</span><span class="sxs-lookup"><span data-stu-id="7c07e-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="7c07e-107">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="7c07e-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="7c07e-108">工作概觀</span><span class="sxs-lookup"><span data-stu-id="7c07e-108">Task Overview</span></span>

<span data-ttu-id="7c07e-109">若要佈建，並在 TFS 中使用新的 team 專案，您將需要完成下列高階步驟：</span><span class="sxs-lookup"><span data-stu-id="7c07e-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="7c07e-110">將建立新的 team 專案的使用者，授與權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="7c07e-111">建立 team 專案。</span><span class="sxs-lookup"><span data-stu-id="7c07e-111">Create the team project.</span></span>
- <span data-ttu-id="7c07e-112">權限授與將在專案運作的小組成員。</span><span class="sxs-lookup"><span data-stu-id="7c07e-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="7c07e-113">檢查某些內容中。</span><span class="sxs-lookup"><span data-stu-id="7c07e-113">Check in some content.</span></span>

<span data-ttu-id="7c07e-114">本主題將示範如何執行這些程序，以及使用者和工作角色可能會負責將每個程序，它會識別。</span><span class="sxs-lookup"><span data-stu-id="7c07e-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="7c07e-115">請注意，根據您的組織結構，這些工作可能是另一個人的責任。</span><span class="sxs-lookup"><span data-stu-id="7c07e-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="7c07e-116">工作與本主題中的逐步解說假設您已安裝並設定 TFS，及您是否已建立 team 專案集合設定程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="7c07e-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="7c07e-117">如需有關這些假設，以及更一般的案例上的背景資訊，請參閱[設定 TFS 組建伺服器進行 Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="7c07e-118">權限授與 Team 專案建立者</span><span class="sxs-lookup"><span data-stu-id="7c07e-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="7c07e-119">若要建立新的 team 專案，您會需要這些權限：</span><span class="sxs-lookup"><span data-stu-id="7c07e-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="7c07e-120">您必須擁有**建立新的專案**TFS 應用程式層上的權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="7c07e-121">您通常將授與此權限將使用者新增至**Project Collection Administrators** TFS 群組。</span><span class="sxs-lookup"><span data-stu-id="7c07e-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="7c07e-122">**Team Foundation Administrators**全域群組也包含此權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="7c07e-123">您必須建立新的小組網站 SharePoint 網站集合對應至 TFS 的 team 專案集合中的權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="7c07e-124">您通常將授與此權限新增至與 SharePoint 群組的使用者**完全控制**權限在 SharePoint 網站集合。</span><span class="sxs-lookup"><span data-stu-id="7c07e-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="7c07e-125">如果您使用 SQL Server Reporting Services 功能，您必須是屬於**Team Foundation 內容管理員**Reporting Services 中的角色。</span><span class="sxs-lookup"><span data-stu-id="7c07e-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="7c07e-126">誰會執行這些程序？</span><span class="sxs-lookup"><span data-stu-id="7c07e-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="7c07e-127">一般而言，個人或群組管理 TFS 部署也會執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="7c07e-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="7c07e-128">因為這是高特殊權限的權限集時，新的 team 專案是通常由一小部分的使用者建立負責管理 TFS 部署。</span><span class="sxs-lookup"><span data-stu-id="7c07e-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="7c07e-129">開發人員不通常會授與建立新的 team 專案所需的權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="7c07e-130">在 TFS 中授與權限</span><span class="sxs-lookup"><span data-stu-id="7c07e-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="7c07e-131">如果您想要讓使用者建立新的 team 專案，第一個的高層級工作是將使用者加入**Project Collection Administrators**群組針對 team 專案集合。</span><span class="sxs-lookup"><span data-stu-id="7c07e-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="7c07e-132">**若要將使用者加入 Project Collection Administrators 群組**</span><span class="sxs-lookup"><span data-stu-id="7c07e-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="7c07e-133">在 TFS 伺服器上，在**啟動**功能表上，指向**所有程式**，按一下  **Microsoft Team Foundation Server 2010**，然後按一下  **Team Foundation管理主控台**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="7c07e-134">在導覽樹狀檢視中，依序展開**應用程式層**，然後按一下  **Team 專案集合**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="7c07e-135">在**Team 專案集合**窗格中，選取 team 專案的集合，您想要管理。</span><span class="sxs-lookup"><span data-stu-id="7c07e-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="7c07e-136">在**一般**索引標籤上，按一下 **群組成員資格**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="7c07e-137">在**全域群組**對話方塊中，選取**Project Collection Administrators**群組，然後再按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="7c07e-138">在**Team Foundation Server 群組屬性**對話方塊中，選取**Windows 使用者或群組**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="7c07e-139">在**選取使用者、 電腦或群組**] 對話方塊中，輸入您想要能夠建立新的 team 專案的使用者的使用者名稱按一下**檢查名稱**，然後按一下 [**確定**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="7c07e-140">在**Team Foundation Server 群組屬性**對話方塊中，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="7c07e-141">在**全域群組**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="7c07e-142">授與 SharePoint Services 中的權限</span><span class="sxs-lookup"><span data-stu-id="7c07e-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="7c07e-143">接下來，您必須提供對應至 TFS team 專案集合的 SharePoint 網站集合中建立新的小組網站的使用者權限。</span><span class="sxs-lookup"><span data-stu-id="7c07e-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="7c07e-144">**若要授與 SharePoint 網站集合的完整控制權限**</span><span class="sxs-lookup"><span data-stu-id="7c07e-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="7c07e-145">在 Team Foundation Server 管理主控台上**Team 專案集合**頁面上，選取您想要管理 team 專案集合。</span><span class="sxs-lookup"><span data-stu-id="7c07e-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="7c07e-146">在**SharePoint 網站**索引標籤上，記下的值**目前預設網站位置**URL。</span><span class="sxs-lookup"><span data-stu-id="7c07e-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="7c07e-147">開啟 Internet Explorer，然後移至您在步驟 2 中記下 URL。</span><span class="sxs-lookup"><span data-stu-id="7c07e-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c07e-148">如果您不登入 windows 的使用者身分建立 team 專案集合，您必須以登入 SharePoint 這位使用者才能繼續。</span><span class="sxs-lookup"><span data-stu-id="7c07e-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="7c07e-149">在**網站動作**功能表上，按一下 **站台設定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="7c07e-150">在**站台設定**頁面的 **使用者和權限**，按一下 **人員與群組**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="7c07e-151">在左側的導覽面板中，按一下 **群組**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="7c07e-152">在**人員與群組： 所有群組**頁面上，按一下**設定組成群組為此站台**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > <span data-ttu-id="7c07e-153">您可能會收到**HTTP 404 找不到**錯誤，因為有雙重 HTTP 編碼錯誤。</span><span class="sxs-lookup"><span data-stu-id="7c07e-153">You may receive an **HTTP 404 Not Found** error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="7c07e-154">如果發生這種情況，請與這個取代 URL:</span><span class="sxs-lookup"><span data-stu-id="7c07e-154">If this occurs, replace the URL with this:</span></span>   
    > <span data-ttu-id="7c07e-155">[*網站集合 URL*] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="7c07e-155">[*site collection URL*]/\_layouts/permsetup.aspx</span></span>  
    > <span data-ttu-id="7c07e-156">例如: </span><span class="sxs-lookup"><span data-stu-id="7c07e-156">For example:</span></span>  
    > <span data-ttu-id="7c07e-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="7c07e-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="7c07e-158">上**設定組成群組為此站台**頁面上，新增的使用者，將建立 team 專案，**擁有者**群組，然後再按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="7c07e-159">如需有關如何讓使用者可以建立 team 專案集合中的新 team 專案的詳細資訊，請參閱[設定 Team 專案集合的系統管理員權限](https://msdn.microsoft.com/en-us/library/dd547204.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="7c07e-160">建立新的 Team 專案並新增使用者</span><span class="sxs-lookup"><span data-stu-id="7c07e-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="7c07e-161">一旦您擁有必要的權限，您可以使用**Team Explorer**建立新的 team 專案的 Visual Studio 2010 中的視窗。</span><span class="sxs-lookup"><span data-stu-id="7c07e-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="7c07e-162">此方法會提供精靈，收集所有必要的資訊並執行 TFS、 SharePoint 和 SQL Server Reporting Services 中的所需的工作。</span><span class="sxs-lookup"><span data-stu-id="7c07e-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="7c07e-163">您還需要在新的 team 專案上的權限授與小組的成員開發人員，以便讓使用者加入和修改內容。</span><span class="sxs-lookup"><span data-stu-id="7c07e-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="7c07e-164">誰會執行這些程序？</span><span class="sxs-lookup"><span data-stu-id="7c07e-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="7c07e-165">通常是 TFS 系統管理員或開發人員小組領導者會執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="7c07e-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="7c07e-166">建立新的 Team 專案</span><span class="sxs-lookup"><span data-stu-id="7c07e-166">Create a New Team Project</span></span>

<span data-ttu-id="7c07e-167">下一個程序描述如何在 TFS 2010 中建立新的 team 專案。</span><span class="sxs-lookup"><span data-stu-id="7c07e-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="7c07e-168">**若要建立新的 team 專案**</span><span class="sxs-lookup"><span data-stu-id="7c07e-168">**To create a new team project**</span></span>

1. <span data-ttu-id="7c07e-169">在**啟動**功能表上，指向**所有程式**，按一下  **Microsoft Visual Studio 2010**，以滑鼠右鍵按一下**Microsoft Visual Studio 2010**，然後按一下 **系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c07e-170">如果您不要以系統管理員身分執行 Visual Studio 2010，新的 Team 專案精靈就會失敗的最後一個步驟。</span><span class="sxs-lookup"><span data-stu-id="7c07e-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="7c07e-171">如果**使用者帳戶控制**對話方塊出現時，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="7c07e-172">在 Visual Studio 中，在**小組**功能表上，按一下 **連接到 Team Foundation Server**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c07e-173">如果您已經設定 TFS 伺服器的連接，您可以省略步驟 4-7。</span><span class="sxs-lookup"><span data-stu-id="7c07e-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="7c07e-174">在**連接到 Team 專案**對話方塊中，按一下 **伺服器**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="7c07e-175">在**新增/移除 Team Foundation Server**對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="7c07e-176">在**加入 Team Foundation Server**對話方塊中，提供您的 TFS 執行個體的詳細資料，然後按**確定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="7c07e-177">在**新增/移除 Team Foundation Server**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="7c07e-178">在**連接到 Team 專案**對話方塊中選取的 TFS 執行個體，您想要連接，請選取 team 專案的集合要加入，然後按一下 **連接**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="7c07e-179">在**Team Explorer**視窗中，以滑鼠右鍵按一下 team 專案集合，然後按一下 **新的 Team 專案**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="7c07e-180">在**新的 Team 專案**對話方塊中，提供的名稱和描述，以 team 專案，然後再按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c07e-181">如果您的 team 專案包含空格，您可能會進行到使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 的輸出路徑從部署封裝時所面對一些問題。</span><span class="sxs-lookup"><span data-stu-id="7c07e-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="7c07e-182">在路徑中的空間可讓更加難以執行 Web Deploy 命令。</span><span class="sxs-lookup"><span data-stu-id="7c07e-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="7c07e-183">在**選取流程範本**頁面上，選取您要用來管理開發程序，然後按一下的流程範本**下一步**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c07e-184">Tfs 流程範本的資訊，請參閱[流程範本和工具](https://msdn.microsoft.com/en-us/vstudio/aa718795)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/en-us/vstudio/aa718795).</span></span>
12. <span data-ttu-id="7c07e-185">在**小組網站設定**頁面上，保留不變的預設設定，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="7c07e-186">此設定建立或識別，SharePoint 小組網站與 TFS team 專案相關聯。</span><span class="sxs-lookup"><span data-stu-id="7c07e-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="7c07e-187">您的開發小組可以使用此站台來管理文件、 參與討論區，建立 wiki 頁面以及執行各項程式碼無關的其他工作。</span><span class="sxs-lookup"><span data-stu-id="7c07e-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="7c07e-188">如需詳細資訊，請參閱[SharePoint 產品之間互動和 Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="7c07e-189">在**指定原始檔控制設定**頁面上，保留不變的預設設定，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="7c07e-190">這項設定識別或 TFS 資料夾階層，做為您內容的根資料夾中建立的位置。</span><span class="sxs-lookup"><span data-stu-id="7c07e-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="7c07e-191">在**確認 Team 專案設定**頁面上，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="7c07e-192">新的 team 專案已成功建立時，在**建立 Team 專案**頁面上，按一下**關閉**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="7c07e-193">將使用者加入至 Team 專案</span><span class="sxs-lookup"><span data-stu-id="7c07e-193">Add Users to a Team Project</span></span>

<span data-ttu-id="7c07e-194">既然您已建立新的 team 專案，您可以授與權限給使用者，讓它們開始新增及需要共同作業的內容。</span><span class="sxs-lookup"><span data-stu-id="7c07e-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="7c07e-195">**將使用者加入至 team 專案**</span><span class="sxs-lookup"><span data-stu-id="7c07e-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="7c07e-196">在 Visual Studio 2010 中，在**Team Explorer**視窗中，以滑鼠右鍵按一下 team 專案，指向**Team 專案設定**，然後按一下 **群組成員資格**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="7c07e-197">若要讓使用者以新增、 修改及移除原始檔控制下的程式碼，您需要將他或她**參與者**群組。</span><span class="sxs-lookup"><span data-stu-id="7c07e-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="7c07e-198">在**專案群組**對話方塊中，選取**參與者**群組，然後再按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="7c07e-199">在**Team Foundation Server 群組屬性**對話方塊中，選取**Windows 使用者或群組**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="7c07e-200">在**選取使用者、 電腦或群組**] 對話方塊中，輸入您想要加入至 team 專案中，使用者的使用者名稱按一下**檢查名稱**，然後按一下 [**確定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="7c07e-201">在**Team Foundation Server 群組屬性**對話方塊中，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="7c07e-202">在**專案群組**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="7c07e-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7c07e-203">結論</span><span class="sxs-lookup"><span data-stu-id="7c07e-203">Conclusion</span></span>

<span data-ttu-id="7c07e-204">此時，新的 team 專案是可供使用，並開發團隊可以開始加入的內容和開發程序的共同作業。</span><span class="sxs-lookup"><span data-stu-id="7c07e-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="7c07e-205">下一個主題[將內容加入至原始檔控制](adding-content-to-source-control.md)，說明如何將內容加入至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="7c07e-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7c07e-206">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="7c07e-206">Further Reading</span></span>

<span data-ttu-id="7c07e-207">如需更廣泛指引在 TFS 中建立 team 專案的詳細資訊，請參閱[建立 Team 專案](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="7c07e-208">如需有關如何讓使用者可以建立 team 專案集合中的新 team 專案的詳細資訊，請參閱[設定 Team 專案集合的系統管理員權限](https://msdn.microsoft.com/en-us/library/dd547204.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span> <span data-ttu-id="7c07e-209">如需有關如何將使用者加入至 team 專案的詳細資訊，請參閱[將使用者加入 Team 專案](https://msdn.microsoft.com/en-us/library/bb558971.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c07e-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/en-us/library/bb558971.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7c07e-210">[上一頁](configuring-team-foundation-server-for-web-deployment.md)
[下一頁](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="7c07e-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
