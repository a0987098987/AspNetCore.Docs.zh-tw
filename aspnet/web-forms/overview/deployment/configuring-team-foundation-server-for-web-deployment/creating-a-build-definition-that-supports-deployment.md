---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: 建立組建定義支援部署 |Microsoft Docs
author: jrjlee
description: 如果您想要執行任何一種建置 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案內的組建定義。 此主題 des...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: d0220ee5c7c066d107311c1235949cec5049cc27
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370279"
---
<a name="creating-a-build-definition-that-supports-deployment"></a><span data-ttu-id="05551-104">建立組建定義支援的部署</span><span class="sxs-lookup"><span data-stu-id="05551-104">Creating a Build Definition That Supports Deployment</span></span>
====================
<span data-ttu-id="05551-105">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="05551-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="05551-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="05551-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="05551-107">如果您想要執行任何一種建置 Team Foundation Server (TFS) 2010年中，您需要建立 team 專案內的組建定義。</span><span class="sxs-lookup"><span data-stu-id="05551-107">If you want to perform any kind of build in Team Foundation Server (TFS) 2010, you need to create a build definition within your team project.</span></span> <span data-ttu-id="05551-108">本主題描述如何在 TFS 中建立新的組建定義，以及如何控制建置程序，在 Team Build 中的 web 部署。</span><span class="sxs-lookup"><span data-stu-id="05551-108">This topic describes how to create a new build definition in TFS and how to control web deployment as part of the build process in Team Build.</span></span>


<span data-ttu-id="05551-109">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="05551-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="05551-110">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置和部署流程控制的兩個專案檔&#x2014;其中一個包含套用至每個目的地環境，以及包含環境特定建置和部署設定的組建指示。</span><span class="sxs-lookup"><span data-stu-id="05551-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="05551-111">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="05551-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="05551-112">工作概觀</span><span class="sxs-lookup"><span data-stu-id="05551-112">Task Overview</span></span>

<span data-ttu-id="05551-113">組建定義是可控制如何及何時組建就會發生在 TFS 中的 team 專案的機制。</span><span class="sxs-lookup"><span data-stu-id="05551-113">A build definition is the mechanism that controls how and when builds occur for team projects in TFS.</span></span> <span data-ttu-id="05551-114">指定每個組建定義：</span><span class="sxs-lookup"><span data-stu-id="05551-114">Each build definition specifies:</span></span>

- <span data-ttu-id="05551-115">您想要建置，Visual Studio 方案檔或自訂的 Microsoft Build Engine (MSBuild) 專案檔等項目。</span><span class="sxs-lookup"><span data-stu-id="05551-115">The things you want to build, like Visual Studio solution files or custom Microsoft Build Engine (MSBuild) project files.</span></span>
- <span data-ttu-id="05551-116">用來決定當組建應採取的準則將放置位置，例如手動觸發程序，持續整合 (CI)，或閘道簽入。</span><span class="sxs-lookup"><span data-stu-id="05551-116">The criteria that determine when a build should take place, like manual triggers, continuous integration (CI), or gated check-ins.</span></span>
- <span data-ttu-id="05551-117">要在 Team Build 應該傳送組建輸出，包括部署的成品，如 web 封裝和資料庫指令碼的位置。</span><span class="sxs-lookup"><span data-stu-id="05551-117">The location to which Team Build should send build outputs, including deployment artifacts like web packages and database scripts.</span></span>
- <span data-ttu-id="05551-118">應該保留每個組建的時間量。</span><span class="sxs-lookup"><span data-stu-id="05551-118">The amount of time that each build should be retained.</span></span>
- <span data-ttu-id="05551-119">建置程序的其他各種參數。</span><span class="sxs-lookup"><span data-stu-id="05551-119">Various other parameters of the build process.</span></span>

> [!NOTE]
> <span data-ttu-id="05551-120">如需有關組建定義的詳細資訊，請參閱[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。</span><span class="sxs-lookup"><span data-stu-id="05551-120">For more information on build definitions, see [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span>


<span data-ttu-id="05551-121">本主題將說明如何建立使用 CI 組建定義，以便開發人員簽入新的內容時，會觸發組建。</span><span class="sxs-lookup"><span data-stu-id="05551-121">This topic will show you how to create a build definition that uses CI, so that a build is triggered when a developer checks in new content.</span></span> <span data-ttu-id="05551-122">如果建置成功時，組建服務就會執行自訂的專案檔將方案部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="05551-122">If the build succeeds, the build service runs a custom project file to deploy the solution to a test environment.</span></span>

<span data-ttu-id="05551-123">當您觸發組建時，就需要進行這些動作：</span><span class="sxs-lookup"><span data-stu-id="05551-123">When you trigger a build, these actions need to happen:</span></span>

- <span data-ttu-id="05551-124">首先，Team Build 應該建置方案。</span><span class="sxs-lookup"><span data-stu-id="05551-124">First, Team Build should build the solution.</span></span> <span data-ttu-id="05551-125">此程序的一部分，Team Build 會叫用 Web 發行管線 (WPP) 來產生每個 web 應用程式專案，方案中的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="05551-125">As part of this process, Team Build will invoke the Web Publishing Pipeline (WPP) to generate web deployment packages for each of the web application projects in the solution.</span></span> <span data-ttu-id="05551-126">Team Build 也會執行任何與方案相關聯的單元測試。</span><span class="sxs-lookup"><span data-stu-id="05551-126">Team Build will also run any unit tests associated with the solution.</span></span>
- <span data-ttu-id="05551-127">如果方案建置失敗，則 Team Build 應該採取任何進一步的動作。</span><span class="sxs-lookup"><span data-stu-id="05551-127">If the solution build fails, Team Build should take no further action.</span></span> <span data-ttu-id="05551-128">單元測試失敗應該視為組建失敗。</span><span class="sxs-lookup"><span data-stu-id="05551-128">Unit test failures should be treated as a build failure.</span></span>
- <span data-ttu-id="05551-129">如果方案建置成功時，Team Build 應該執行自訂的專案檔，以控制部署的解決方案。</span><span class="sxs-lookup"><span data-stu-id="05551-129">If the solution build succeeds, Team Build should run the custom project file that controls the deployment of the solution.</span></span> <span data-ttu-id="05551-130">此程序的一部分，Team Build 會叫用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy)，目的地 web 伺服器上安裝的已封裝的 web 應用程式，並叫用的 VSDBCMD.exe 公用程式來執行建立資料庫在目的地資料庫伺服器上的指令碼。</span><span class="sxs-lookup"><span data-stu-id="05551-130">As part of this process, Team Build will invoke the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to install the packaged web applications on the destination web servers, and it will invoke the VSDBCMD.exe utility to run database creation scripts on the destination database servers.</span></span>

<span data-ttu-id="05551-131">這會說明此程序：</span><span class="sxs-lookup"><span data-stu-id="05551-131">This illustrates the process:</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

<span data-ttu-id="05551-132">[Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案包含自訂的 MSBuild 專案檔， *Publish.proj*，您可以從 MSBuild 或 Team Build 中執行。</span><span class="sxs-lookup"><span data-stu-id="05551-132">The [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution includes a custom MSBuild project file, *Publish.proj*, that you can run from MSBuild or Team Build.</span></span> <span data-ttu-id="05551-133">中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，此專案檔中定義的邏輯，將您的網頁套件和資料庫部署到目標環境。</span><span class="sxs-lookup"><span data-stu-id="05551-133">As described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), this project file defines the logic that deploys your web packages and databases to a target environment.</span></span> <span data-ttu-id="05551-134">此檔案包含省略建置和封裝程序，在 Team Build，讓部署工作來執行中執行的邏輯。</span><span class="sxs-lookup"><span data-stu-id="05551-134">The file includes logic that omits the building and packaging process if it's running in Team Build, leaving just the deployment tasks to run.</span></span> <span data-ttu-id="05551-135">這是因為當您自動執行部署，如此一來，您通常會想要確保成功建置方案，並會開始部署程序之前，傳遞任何單元測試。</span><span class="sxs-lookup"><span data-stu-id="05551-135">This is because when you automate deployment in this way, you'll typically want to ensure that the solution builds successfully and passes any unit tests before the deployment process commences.</span></span>

<span data-ttu-id="05551-136">下節將說明如何實作此程序建立新的組建定義。</span><span class="sxs-lookup"><span data-stu-id="05551-136">The next section explains how to implement this process by creating a new build definition.</span></span>

> [!NOTE]
> <span data-ttu-id="05551-137">此程序&#x2014;中的單一的自動程序會建置、 測試，以及部署方案&#x2014;可能是最適合部署至測試環境。</span><span class="sxs-lookup"><span data-stu-id="05551-137">This procedure&#x2014;in which a single automated process builds, tests, and deploys a solution&#x2014;is likely to be most suited to deployment to test environments.</span></span> <span data-ttu-id="05551-138">針對預備和生產環境很更多可能會想要從先前的組建時，您已驗證，並驗證測試環境中部署內容。</span><span class="sxs-lookup"><span data-stu-id="05551-138">For staging and production environments you're a lot more likely to want to deploy content from a previous build that you've already verified and validated in a test environment.</span></span> <span data-ttu-id="05551-139">這種方法在下一個主題中，所述[部署特定建置](deploying-a-specific-build.md)。</span><span class="sxs-lookup"><span data-stu-id="05551-139">This approach is described in the next topic, [Deploying a Specific Build](deploying-a-specific-build.md).</span></span>


### <a name="who-performs-this-procedure"></a><span data-ttu-id="05551-140">對於執行此程序？</span><span class="sxs-lookup"><span data-stu-id="05551-140">Who Performs This Procedure?</span></span>

<span data-ttu-id="05551-141">一般而言，TFS 系統管理員會執行此程序。</span><span class="sxs-lookup"><span data-stu-id="05551-141">Typically, a TFS administrator performs this procedure.</span></span> <span data-ttu-id="05551-142">在某些情況下，開發人員小組領導者可能需要在 TFS 中的 team 專案集合的責任。</span><span class="sxs-lookup"><span data-stu-id="05551-142">In some cases, a developer team leader may take responsibility for the team project collection in TFS.</span></span> <span data-ttu-id="05551-143">若要建立新的組建定義，您必須是隸屬**Project Collection Build Administrators**群組的 team 專案集合，其中包含您的解決方案。</span><span class="sxs-lookup"><span data-stu-id="05551-143">In order to create a new build definition, you need to be a member of the **Project Collection Build Administrators** group for the team project collection that contains your solution.</span></span>

## <a name="create-a-build-definition-for-ci-and-deployment"></a><span data-ttu-id="05551-144">建立組建定義 CI 組建和部署</span><span class="sxs-lookup"><span data-stu-id="05551-144">Create a Build Definition for CI and Deployment</span></span>

<span data-ttu-id="05551-145">下一個程序描述如何建立 CI 觸發的組建定義。</span><span class="sxs-lookup"><span data-stu-id="05551-145">The next procedure describes how to create a build definition that CI triggers.</span></span> <span data-ttu-id="05551-146">如果建置成功，解決方案會部署自訂的 MSBuild 專案檔中使用的邏輯。</span><span class="sxs-lookup"><span data-stu-id="05551-146">If the build succeeds, the solution is deployed using the logic in a custom MSBuild project file.</span></span>

<span data-ttu-id="05551-147">**若要建立的組建定義 CI 組建和部署**</span><span class="sxs-lookup"><span data-stu-id="05551-147">**To create a build definition for CI and deployment**</span></span>

1. <span data-ttu-id="05551-148">在 Visual Studio 2010 中，在**Team Explorer**視窗中，展開您的 team 專案節點，以滑鼠右鍵按一下**建置**，然後按一下**新增組建定義**。</span><span class="sxs-lookup"><span data-stu-id="05551-148">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. <span data-ttu-id="05551-149">在 **一般**索引標籤上，指定組建定義的名稱 (例如**DeployToTest**) 和選擇性描述。</span><span class="sxs-lookup"><span data-stu-id="05551-149">On the **General** tab, give the build definition a name (for example, **DeployToTest**) and an optional description.</span></span>
3. <span data-ttu-id="05551-150">在 **觸發程序**索引標籤上，選取您要觸發新組建的準則。</span><span class="sxs-lookup"><span data-stu-id="05551-150">On the **Trigger** tab, select the criteria on which you want to trigger a new build.</span></span> <span data-ttu-id="05551-151">例如，如果您想要建置方案，並部署到測試環境中，每次簽入新的程式碼的開發人員，請選取**持續整合**。</span><span class="sxs-lookup"><span data-stu-id="05551-151">For example, if you want to build the solution and deploy to the test environment every time a developer checks in new code, select **Continuous Integration**.</span></span>
4. <span data-ttu-id="05551-152">在 **組建預設值**索引標籤**組建輸出複製到下列置放資料夾**方塊中，輸入 drop 資料夾的通用命名慣例 (UNC) 路徑 (例如 **\\TFSBUILD\Drops**)。</span><span class="sxs-lookup"><span data-stu-id="05551-152">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="05551-153">這個置放位置會儲存數個組建，根據您設定的保留原則。</span><span class="sxs-lookup"><span data-stu-id="05551-153">This drop location stores several builds, depending on the retention policy you configure.</span></span> <span data-ttu-id="05551-154">當您想要發行至預備或生產環境，從特定組建部署成品時，這是您可以找到它們。</span><span class="sxs-lookup"><span data-stu-id="05551-154">When you want to publish deployment artifacts from a specific build to a staging or production environment, this is where you'll find them.</span></span>
5. <span data-ttu-id="05551-155">在上**程序**索引標籤中，於**建置流程檔**下拉式清單中，保持**DefaultTemplate.xaml**選取。</span><span class="sxs-lookup"><span data-stu-id="05551-155">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="05551-156">這是其中一個預設建置流程範本加入至所有新的 team 專案。</span><span class="sxs-lookup"><span data-stu-id="05551-156">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="05551-157">在 [**建置流程參數**資料表中，按一下**要建置的項目**資料列，然後再按一下**省略符號**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="05551-157">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. <span data-ttu-id="05551-158">在 [**項目，以建置**] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="05551-158">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="05551-159">瀏覽至您的方案檔案的位置，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="05551-159">Browse to the location of your solution file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. <span data-ttu-id="05551-160">在 [**項目，以建置**] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="05551-160">In the **Items to Build** dialog box, click **Add**.</span></span>
10. <span data-ttu-id="05551-161">在 **類型的項目**下拉式清單中選取**MSBuild 專案檔**。</span><span class="sxs-lookup"><span data-stu-id="05551-161">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
11. <span data-ttu-id="05551-162">瀏覽至自訂的專案檔案與您控制部署程序、 選取檔案，然後按一下位置**確定**。</span><span class="sxs-lookup"><span data-stu-id="05551-162">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. <span data-ttu-id="05551-163">**項目，以建置**對話方塊現在應該會顯示兩個項目。</span><span class="sxs-lookup"><span data-stu-id="05551-163">The **Items to Build** dialog box should now show two items.</span></span> <span data-ttu-id="05551-164">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="05551-164">Click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. <span data-ttu-id="05551-165">在上**程序**索引標籤**建置流程參數**資料表中，展開**進階**一節。</span><span class="sxs-lookup"><span data-stu-id="05551-165">On the **Process** tab, in the **Build process parameters** table, expand the **Advanced** section.</span></span>
14. <span data-ttu-id="05551-166">在  **MSBuild 引數**資料列中，新增任何 MSBuild 命令列引數，*任一*的建置項目需要。</span><span class="sxs-lookup"><span data-stu-id="05551-166">In the **MSBuild Arguments** row, add any MSBuild command-line arguments that *either* of your items to build requires.</span></span> <span data-ttu-id="05551-167">在連絡管理員解決方案案例中，這些引數則是必要項目：</span><span class="sxs-lookup"><span data-stu-id="05551-167">In the Contact Manager solution scenario, these arguments are required:</span></span>

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. <span data-ttu-id="05551-168">在這個範例中：</span><span class="sxs-lookup"><span data-stu-id="05551-168">In this example:</span></span>

    1. <span data-ttu-id="05551-169">**DeployOnBuild = true**並**DeployTarget = 封裝**引數是必要的當您建置連絡管理員解決方案。</span><span class="sxs-lookup"><span data-stu-id="05551-169">The **DeployOnBuild=true** and **DeployTarget=package** arguments are required when you build the Contact Manager solution.</span></span> <span data-ttu-id="05551-170">這會指示 MSBuild 來建立 web 部署套件之後建置每個 web 應用程式專案中所述[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="05551-170">This instructs MSBuild to create web deployment packages after building each web application project, as described in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>
    2. <span data-ttu-id="05551-171">**TargetEnvPropsFile**引數是必要的當您建置*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="05551-171">The **TargetEnvPropsFile** argument is required when you build the *Publish.proj* file.</span></span> <span data-ttu-id="05551-172">這個屬性會指出環境特定組態檔的位置中所述[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="05551-172">This property indicates the location of the environment-specific configuration file, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>
16. <span data-ttu-id="05551-173">在 **保留原則**索引標籤上，設定多少組建的每個您想要保留所需的類型。</span><span class="sxs-lookup"><span data-stu-id="05551-173">On the **Retention Policy** tab, configure how many builds of each type you want to retain as required.</span></span>
17. <span data-ttu-id="05551-174">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="05551-174">Click **Save**.</span></span>

## <a name="queue-a-build"></a><span data-ttu-id="05551-175">將組建排入佇列</span><span class="sxs-lookup"><span data-stu-id="05551-175">Queue a Build</span></span>

<span data-ttu-id="05551-176">此時，您已建立至少一個新的組建定義。</span><span class="sxs-lookup"><span data-stu-id="05551-176">At this point, you have created at least one new build definition.</span></span> <span data-ttu-id="05551-177">您所定義的建置程序現在會根據您在組建定義中指定的觸發程序執行。</span><span class="sxs-lookup"><span data-stu-id="05551-177">The build process you defined will now run according to the triggers you specified in the build definition.</span></span>

<span data-ttu-id="05551-178">如果您已設定使用 CI 組建定義，您可以測試您的組建定義兩種方式：</span><span class="sxs-lookup"><span data-stu-id="05551-178">If you've configured your build definition to use CI, you can test your build definition in two ways:</span></span>

- <span data-ttu-id="05551-179">簽入至 team 專案，以自動組建觸發程序的一些內容。</span><span class="sxs-lookup"><span data-stu-id="05551-179">Check in some content to the team project to trigger an automatic build.</span></span>
- <span data-ttu-id="05551-180">組建排入佇列以手動方式。</span><span class="sxs-lookup"><span data-stu-id="05551-180">Queue a build manually.</span></span>

<span data-ttu-id="05551-181">**若要手動將組建排入佇列**</span><span class="sxs-lookup"><span data-stu-id="05551-181">**To queue a build manually**</span></span>

1. <span data-ttu-id="05551-182">在 [ **Team Explorer** ] 視窗中，組建定義，以滑鼠右鍵按一下，然後按一下**佇列新組建**。</span><span class="sxs-lookup"><span data-stu-id="05551-182">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. <span data-ttu-id="05551-183">在 [**組建排入佇列**] 對話方塊中，檢閱組建屬性，然後按一下**佇列**。</span><span class="sxs-lookup"><span data-stu-id="05551-183">In the **Queue Build** dialog box, review the build properties, and then click **Queue**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

<span data-ttu-id="05551-184">若要檢閱進度與結果的組建&#x2014;不論它已觸發手動或自動&#x2014;連按兩下中的組建定義**Team Explorer**視窗。</span><span class="sxs-lookup"><span data-stu-id="05551-184">To review the progress and the outcome of a build&#x2014;regardless of whether it was triggered manually or automatically&#x2014;double-click the build definition in the **Team Explorer** window.</span></span> <span data-ttu-id="05551-185">這會開啟**Build 總管** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="05551-185">This will open a **Build Explorer** tab.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

<span data-ttu-id="05551-186">從這裡開始，您可以疑難排解失敗的組建。</span><span class="sxs-lookup"><span data-stu-id="05551-186">From here, you can troubleshoot failed builds.</span></span> <span data-ttu-id="05551-187">如果您按兩下個別組建時，您可以檢視摘要資訊，並逐一點選至詳細的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="05551-187">If you double-click an individual build, you can view summary information and click through to detailed log files.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

<span data-ttu-id="05551-188">您可以使用這項資訊來疑難排解失敗的組建，並解決任何問題，才能嘗試進行另一個組建。</span><span class="sxs-lookup"><span data-stu-id="05551-188">You can use this information to troubleshoot failed builds and address any problems before you attempt another build.</span></span>

> [!NOTE]
> <span data-ttu-id="05551-189">可能失敗，直到您已在目的地環境中所需的任何權限授與組建伺服器時，會執行部署邏輯的組建。</span><span class="sxs-lookup"><span data-stu-id="05551-189">Builds that execute deployment logic are likely to fail until you have granted the build server any permissions required in the destination environment.</span></span> <span data-ttu-id="05551-190">如需詳細資訊，請參閱 <<c0> [ 設定小組組建部署的權限](configuring-permissions-for-team-build-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="05551-190">For more information, see [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md).</span></span>


## <a name="monitor-the-build-process"></a><span data-ttu-id="05551-191">監視組建程序</span><span class="sxs-lookup"><span data-stu-id="05551-191">Monitor the Build Process</span></span>

<span data-ttu-id="05551-192">TFS 提供廣泛的功能，可協助您監視組建程序。</span><span class="sxs-lookup"><span data-stu-id="05551-192">TFS provides a broad range of functionality to help you monitor the build process.</span></span> <span data-ttu-id="05551-193">例如，TFS 可以傳送電子郵件或組建完成時，在工作列通知區域中顯示的警示。</span><span class="sxs-lookup"><span data-stu-id="05551-193">For example, TFS can send you an email or display alerts in your taskbar notification area when a build has completed.</span></span> <span data-ttu-id="05551-194">如需詳細資訊，請參閱 <<c0> [ 執行和監視組建](https://msdn.microsoft.com/library/ms181721.aspx)。</span><span class="sxs-lookup"><span data-stu-id="05551-194">For more information, see [Run and Monitor Builds](https://msdn.microsoft.com/library/ms181721.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="05551-195">結論</span><span class="sxs-lookup"><span data-stu-id="05551-195">Conclusion</span></span>

<span data-ttu-id="05551-196">本主題說明如何在 TFS 中建立組建定義。</span><span class="sxs-lookup"><span data-stu-id="05551-196">This topic described how to create a build definition in TFS.</span></span> <span data-ttu-id="05551-197">組建定義被設定 CI，以便在建置程序執行每當開發人員簽入至 team 專案的內容。</span><span class="sxs-lookup"><span data-stu-id="05551-197">The build definition is configured for CI, so the build process runs whenever a developer checks in content to the team project.</span></span> <span data-ttu-id="05551-198">組建定義執行自訂的 MSBuild 專案檔將網頁套件和資料庫指令碼部署到目標伺服器環境。</span><span class="sxs-lookup"><span data-stu-id="05551-198">The build definition executes a custom MSBuild project file to deploy web packages and database scripts to a target server environment.</span></span>

<span data-ttu-id="05551-199">為了讓自動化的部署，才能成功建置程序的一部分，您必須授與組建服務帳戶在目標 web 伺服器和目標資料庫伺服器上的適當權限。</span><span class="sxs-lookup"><span data-stu-id="05551-199">In order for an automated deployment to succeed as part of a build process, you'll need to grant appropriate permissions to the build service account on the target web servers and the target database server.</span></span> <span data-ttu-id="05551-200">本教學課程的最後一個主題[設定小組組建部署的權限](configuring-permissions-for-team-build-deployment.md)，說明如何識別及設定自動化的部署，從 Team Build 的伺服器所需的權限。</span><span class="sxs-lookup"><span data-stu-id="05551-200">The final topic in this tutorial, [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md), describes how to identify and configure the permissions required for automated deployment from a Team Build server.</span></span>

## <a name="further-reading"></a><span data-ttu-id="05551-201">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="05551-201">Further Reading</span></span>

<span data-ttu-id="05551-202">如需有關如何建立組建定義的詳細資訊，請參閱 <<c0> [ 建立基本組建定義](https://msdn.microsoft.com/library/ms181716.aspx)並[定義建置流程](https://msdn.microsoft.com/library/ms181715.aspx)。</span><span class="sxs-lookup"><span data-stu-id="05551-202">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="05551-203">如需詳細指引佇列組建的詳細資訊，請參閱[組建排入佇列](https://msdn.microsoft.com/library/ms181722.aspx)。</span><span class="sxs-lookup"><span data-stu-id="05551-203">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05551-204">[上一頁](configuring-a-tfs-build-server-for-web-deployment.md)
> [下一頁](deploying-a-specific-build.md)</span><span class="sxs-lookup"><span data-stu-id="05551-204">[Previous](configuring-a-tfs-build-server-for-web-deployment.md)
[Next](deploying-a-specific-build.md)</span></span>
